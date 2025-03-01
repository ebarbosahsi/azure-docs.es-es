---
title: Solución de problemas de ejecución de paquetes en SSIS Integration Runtime
description: En este artículo se proporciona orientación para solucionar problemas de ejecución de paquetes de SSIS en SSIS Integration Runtime
ms.service: data-factory
ms.topic: conceptual
ms.author: wenjiefu
author: RodgeFu
ms.reviewer: sawinark
ms.custom: seo-lt-2019
ms.date: 04/15/2019
ms.openlocfilehash: 6eecedbc28bcb8bc0bd46534a2c2692636f6f2c1
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105934009"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>Solución de problemas de ejecución de paquetes en SSIS Integration Runtime

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se incluyen los errores más comunes que puede encontrar al ejecutar paquetes de SQL Server Integration Services (SSIS) en SSIS Integration Runtime. Se describen las posibles causas y acciones para resolver los errores.

## <a name="where-to-find-logs-for-troubleshooting"></a>Dónde encontrar los registros para solucionar problemas

Use el portal de Azure Data Factory para comprobar la salida de la actividad de ejecución del paquete de SSIS. La salida incluye el resultado de la ejecución, los mensajes de error y el identificador de la operación. Para obtener instrucciones detalladas, consulte [Supervisar la canalización](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline).

Use el catálogo de SSIS (SSISDB) para comprobar los registros de detalle de la ejecución. Para obtener más información, consulte [Monitor de ejecución de paquetes y otras operaciones](/sql/integration-services/performance/monitor-running-packages-and-other-operations).

## <a name="common-errors-causes-and-solutions"></a>Errores, causas y soluciones comunes

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Mensaje de error: "Se ha agotado el tiempo de espera de la conexión" o "Error en el servicio al procesar la solicitud. Inténtelo de nuevo."

Estas son las posibles causas y las acciones recomendadas:
* El origen o el destino de los datos está sobrecargado. Compruebe la carga en el origen o el destino de los datos y observe si tiene suficiente capacidad. Por ejemplo, si usa Azure SQL Database, considere la posibilidad de escalar verticalmente si es probable que el tiempo de espera de la base de datos se agote.
* La red entre SSIS Integration Runtime y el origen o el destino de los datos es inestable, especialmente si la conexión se realiza entre regiones o entre un entorno local y Azure. Para aplicar el patrón de reintento en el paquete de SSIS, siga estos pasos:
  * Compruebe que los paquetes de SSIS se pueden volver a ejecutar en caso de error sin efectos secundarios (por ejemplo, pérdida o duplicación de datos).
  * Configure las opciones **Reintentar** e **Intervalo de reintentos** de la actividad **Ejecutar paquete de SSIS**  en la pestaña **General**. ![Establecimiento de propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Para un componente de origen o destino ADO.NET y OLE DB, establezca **ConnectRetryCount** y **ConnectRetryInterval** en el administrador de conexiones en el paquete de SSIS o la actividad de SSIS.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Mensaje de error: "El origen ADO NET no pudo adquirir la conexión «...»" con "Se produjo un error relacionado con la red o específico de una instancia al establecer una conexión con SQL Server. No se detectó el servidor o no estaba accesible."

Normalmente, este problema significa que no se puede acceder al origen o el destino de los datos desde SSIS Integration Runtime. Las razones pueden variar. Pruebe estas acciones:
* Asegúrese de que está pasando el nombre o IP del origen o el destino de los datos correctamente.
* Asegúrese de que el firewall está configurado correctamente.
* Compruebe que la red virtual esté configurada correctamente si el origen o el destino de los datos es un entorno local:
  * Puede comprobar si el problema proviene de una configuración de red virtual mediante el aprovisionamiento de una VM de Azure en la misma red virtual. A continuación, compruebe si se puede acceder al origen o el destino de los datos desde la VM de Azure.
  * Puede encontrar más detalles sobre el uso de una red virtual con una instancia de SSIS Integration Runtime en [Unión de una instancia de Azure-SSIS Integration Runtime a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Mensaje de error: "El origen ADO NET no pudo adquirir la conexión «...»" con "Error al crear un administrador de conexiones administrado".

La causa posible es que el proveedor de ADO.NET que se usa en el paquete no está instalado en el entorno de ejecución de integración de SSIS. Para instalar el proveedor, puede usar una configuración personalizada. Puede encontrar más detalles sobre la configuración personalizada en [Customize setup for the Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Personalizar la configuración para Azure-SSIS Integration Runtime).

### <a name="error-message-the-connection--is-not-found"></a>Mensaje de error: "No se encontró la conexión «...»"

Un problema conocido en versiones anteriores de SQL Server Management Studio (SSMS) puede provocar este error. Si el paquete contiene un componente personalizado (por ejemplo, los componentes de asociados o el paquete de características de Azure de SSIS) que no está instalado en la máquina donde se usa SSMS para realizar la implementación, SSMS quitará el componente y provocará el error. Actualice [SSMS](/sql/ssms/download-sql-server-management-studio-ssms) a la versión más reciente con el problema corregido.

### <a name="error-messagessis-executor-exit-code--1073741819"></a>Mensaje de error: "SSIS Executor exit code: -1073741819." (Código de salida del ejecutor SSIS: 1073741819).

* Causa posible y acción recomendada:
  * Este error puede deberse a la limitación del origen y el destino de Excel cuando se ejecutan varios orígenes o destinos de Excel en paralelo en varios subprocesos. Para solucionar esta limitación, cambie los componentes de Excel para que se ejecuten en secuencia, o sepárelos en diferentes paquetes y desencadénelos mediante "Tarea Ejecutar paquete" con la propiedad ExecuteOutOfProcess establecida como true.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Mensaje de error: "No hay suficiente espacio en el disco"

Este error significa que el disco local está agotado en el nodo de SSIS Integration Runtime. Compruebe si el paquete o la configuración personalizada consume una gran cantidad de espacio en disco:
* Si el paquete consume el disco, se liberará cuando finalice la ejecución del paquete.
* Si la configuración personalizada consume el disco, deberá detener el entorno de ejecución de integración de SSIS, modificar el script y volver a iniciar el entorno de ejecución de integración. El contenedor de blobs de Azure que especificó para la configuración personalizada se copiará en el nodo de SSIS Integration Runtime, así que compruebe si existe cualquier contenido innecesario en ese contenedor.

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>Mensaje de error: "Error al recuperar el recurso del elemento principal. Microsoft.SqlServer.IntegrationServices.Scale.ScaleoutContract.Common.MasterResponseFailedException: Código: 300004. Descripción: No se pudo cargar el archivo "***"".

* Causa posible y acción recomendada:
  * Si la actividad de SSIS ejecuta el paquete desde el sistema de archivos (archivo de paquete o archivo de proyecto), este error se producirá si el proyecto, el paquete o el archivo de configuración no son accesibles con la credencial de acceso del paquete que proporcionó en la actividad de SSIS.
    * Si usa Azure File:
      * La ruta de acceso del archivo debe empezar por \\\\\<storage account name\>.file.core.windows.net\\\<file share path\>
      * El dominio debe ser "Azure".
      * El nombre de usuario debe ser \<storage account name\>
      * La contraseña debe ser \<storage access key\>
    * Si usa un archivo local, compruebe que la red virtual y la credencial y los permisos de acceso al paquete estén configurados correctamente para que Azure SSIS Integration Runtime pueda acceder al recurso compartido de archivos local.

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>Mensaje de error: "El nombre de archivo "..." especificado en la conexión no era válido"

* Causa posible y acción recomendada:
  * Se ha especificado un nombre de archivo no válido.
  * Asegúrese de que usa el FQDN (nombre de dominio completo) en lugar del nombre corto en el administrador de conexiones.

### <a name="error-message-cannot-open-file-"></a>Mensaje de error: "No se puede abrir el archivo «...»"

Este error se produce cuando la ejecución del paquete no encuentra un archivo en el disco local en SSIS Integration Runtime. Pruebe estas acciones:
* No use la ruta de acceso absoluta en el paquete que se está ejecutando en SSIS Integration Runtime. Use el directorio de trabajo de ejecución actual (.) o la carpeta temporal (%TEMP%).
* Si necesita conservar algunos archivos en nodos de SSIS Integration Runtime, prepare los archivos como se describe en [Personalizar la configuración](how-to-configure-azure-ssis-ir-custom-setup.md). Cuando finalice la ejecución, se limpiarán todos los archivos del directorio de trabajo.
* En lugar de almacenar el archivo en el nodo de SSIS Integration Runtime, use Azure Files. Para obtener más información, consulte [Usar recursos compartidos de archivos de Azure](/sql/integration-services/lift-shift/ssis-azure-files-file-shares#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Mensaje de error: "La base de datos “SSISDB” ha alcanzado su cuota de tamaño"

Una causa posible es que la base de datos de SSISDB creada en Azure SQL Database o en Instancia administrada de SQL haya alcanzado su cuota. Pruebe estas acciones:
* Considere la posibilidad de aumentar el número de DTU de la base de datos. Encontrará los detalles en [Límites de SQL Database para un servidor lógico](../azure-sql/database/resource-limits-logical-server.md).
* Compruebe si el paquete generaría muchos registros. Si es así, puede configurar un trabajo elástico para limpiar estos registros. Para obtener más detalles, consulte [Limpieza de los registros de SSISDB con trabajos de Azure Elastic Database](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Mensaje de error: "The request limit for the database is ... and has been reached" (El límite de solicitudes para la base de datos es ... y se alcanzó).

Si se ejecutan muchos paquetes en paralelo en SSIS Integration Runtime, este error podría producirse porque SSISDB alcanzó su límite de solicitudes. Considere la posibilidad de aumentar el DTC de SSISDB para resolver este problema. Encontrará los detalles en [Límites de SQL Database para un servidor lógico](../azure-sql/database/resource-limits-logical-server.md).

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Mensaje de error: "SSIS Operation failed with unexpected operation status: ..." (Error en la operación de SSIS con un estado de operación inesperado: ...).

El error se debe, principalmente, a un problema temporal. Intente volver a realizar la ejecución del paquete. Para aplicar el patrón de reintento en el paquete de SSIS, siga estos pasos:

* Compruebe que los paquetes de SSIS se pueden volver a ejecutar en caso de error sin efectos secundarios (por ejemplo, pérdida o duplicación de datos).
* Configure las opciones **Reintentar** e **Intervalo de reintentos** de la actividad **Ejecutar paquete de SSIS**  en la pestaña **General**. ![Establecimiento de propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Para un componente de origen o destino ADO.NET y OLE DB, establezca **ConnectRetryCount** y **ConnectRetryInterval** en el administrador de conexiones en el paquete de SSIS o la actividad de SSIS.

### <a name="error-message-there-is-no-active-worker"></a>Mensaje de error: "No hay ningún trabajo activo".

Normalmente, este error significa que SSIS Integration Runtime tiene un estado incorrecto. Compruebe el estado y los errores detallados en Azure Portal. Para obtener más información, vea [Azure-SSIS Integration Runtime](./monitor-integration-runtime.md#azure-ssis-integration-runtime).

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Mensaje de error: "Your integration runtime cannot be upgraded and will eventually stop working, since we cannot access the Azure Blob container you provided for custom setup" (El entorno de ejecución de integración no se puede actualizar y finalmente dejará de funcionar, ya que no se puede acceder al contenedor de blobs de Azure que proporcionó para la configuración personalizada)".

Este error se produce cuando SSIS Integration Runtime no puede acceder al almacenamiento configurado para la configuración personalizada. Compruebe si el URI de la firma de acceso compartido (SAS) que ha proporcionado es válido y no ha expirado.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Mensaje de error: "Proveedor OLE DB de Microsoft para Analysis Services. "Hresult: 0x80004005 Descripción:" Error de COM: Error de COM: mscorlib; Se produjo una excepción en el destino de la invocación"

Una posible causa es que el nombre de usuario o la contraseña con Multi-Factor Authentication de Azure AD habilitado esté configurado para la autenticación de Azure Analysis Services. Esta autenticación no se admite en SSIS Integration Runtime. Intente usar una entidad de servicio para la autenticación de Azure Analysis Services:

1. Prepare una entidad de servicio como se describe en [Automatización con entidades de servicio](../analysis-services/analysis-services-service-principal.md).
2. En Connection Manager, configure **Usar un nombre de usuario y contraseña específicos**, establezca **app: *&lt;AppID&gt;* @*&lt;TenantID&gt;** * como nombre de usuario y clientSecret como contraseña. A continuación, se muestra un ejemplo de un nombre de usuario con el formato correcto:
 
   `app:12345678-9012-3456-789a-bcdef012345678@9abcdef0-1234-5678-9abc-def0123456789abc`
1. En el Administrador de conexiones, configure **Usar un nombre de usuario y contraseña específicos**: defina **AppID** como nombre de usuario y **clientSecret** como contraseña.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Mensaje de error: "ADONET Source has failed to acquire the connection {GUID} with the following error message: Login failed for user 'NT AUTHORITY\ANONYMOUS LOGON'" (El origen de ADONET no ha podido adquirir la conexión {GUID} con el siguiente mensaje de error: error de inicio de sesión para el usuario "NT AUTHORITY\ANONYMOUS LOGON")

Asegúrese de no configurar el método de autenticación del administrador de conexiones como **Autenticación de contraseña de Active Directory** cuando el parámetro *ConnectUsingManagedIdentity* sea **True** . Puede configurarlo como **Autenticación de SQL**, que se ignora si *ConnectUsingManagedIdentity* está definido.

### <a name="error-message-0xc020801f-at--odata-source--cannot-acquire-a-managed-connection-from-the-run-time-connection-manager"></a>Mensaje de error: "0xC020801F at ..., Origen de OData [...]: No se puede adquirir una conexión administrada del administrador de conexiones en tiempo de ejecución"

Una posible causa es que la seguridad de la capa de transporte (TLS) no esté habilitada en el entorno de ejecución de integración de SSIS, que es necesario para el origen de OData. Puede habilitar TLS en el entorno de ejecución de integración de SSIS mediante la personalización del programa de instalación. Encontrará más detalles en [No puede conectarse a la OData de Project online desde SSIS](/office365/troubleshoot/cant-connect-project-online-odata-from-ssis) e [Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="error-message-request-staging-task-with-operation-guid--fail-since-error-failed-to-dispatch-staging-operation-with-error-message-microsoftsqlserverintegrationservicesaisagentcoreaisagentexception-failed-to-load-data-proxy"></a>Mensaje de error: "Request staging task with operation guid ... fail since error: Failed to dispatch staging operation with error message: Microsoft.SqlServer.IntegrationServices.AisAgentCore.AisAgentException: Failed to load data proxy" (La solicitud de la tarea de la tarea provisional con GUID de operación ha producido un error: No se pudo enviar una operación provisional con el mensaje de error: Microsoft.SqlServer.IntegrationServices.AisAgentCore.AisAgentException: No se pudo cargar el proxy de datos).

Asegúrese de que Azure-SSIS Integration Runtime está configurado con el entorno de ejecución de integración autohospedado. Puede encontrar más información en [Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR en Azure Data Factory](self-hosted-integration-runtime-proxy-ssis.md).

### <a name="error-message-staging-task-status-failed-staging-task-error-errorcode-2010-errormessage-the-self-hosted-integration-runtime--is-offline"></a>Mensaje de error: "Staging task status: Failed. Staging task error: ErrorCode: 2010, ErrorMessage: The Self-hosted Integration Runtime ... is offline" (Estado de la tarea provisional: Con error. Error provisional de la tarea: Código de error: 2010, Mensaje de error: El entorno de ejecución de integración autohospedado está sin conexión).

Asegúrese de que el entorno de ejecución de integración autohospedado está instalado y se ha iniciado. Puede obtener más información en [Creación y configuración de un entorno de ejecución de integración autohospedado](create-self-hosted-integration-runtime.md).

### <a name="error-message-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-error-the-requested-ole-db-provider--is-not-registered-if-the-64-bit-driver-is-not-installed-run-the-package-in-32-bit-mode"></a>Mensaje de error: "Staging task error: ErrorCode: 2906, ErrorMessage: Package execution failed., Output: {"OperationErrorMessages": "Error: The requested OLE DB provider ... is not registered. If the 64-bit driver is not installed, run the package in 32-bit mode..." (Error de la tarea provisional: Código de error: 2906, Mensaje de error: Error de ejecución de paquete. Salida: {"OperationErrorMessages": "Error: El proveedor de OLE DB solicitado no está registrado. Si el controlador de 64 bits no está instalado, ejecute el paquete en modo de 32 bits).

Asegúrese de que el proveedor correspondiente utilizado por los conectores de OLE DB del paquete está instalado correctamente en la máquina del entorno de ejecución de integración autohospedado. Puede encontrar más información en [Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR en Azure Data Factory](self-hosted-integration-runtime-proxy-ssis.md#prepare-the-self-hosted-ir).

### <a name="error-message-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-error-systemiofileloadexception-could-not-load-file-or-assembly-microsoftwindowsazurestorage-version-cultureneutral-publickeytoken31bf3856ad364e35-or-one-of-its-dependencies-the-located-assemblys-manifest-definition-does-not-match-the-assembly-reference"></a>Mensaje de error: "Staging task error: ErrorCode: 2906, ErrorMessage: Package execution failed., Output: {"OperationErrorMessages": "Error: System.IO.FileLoadException: Could not load file or assembly 'Microsoft.WindowsAzure.Storage, Version=..., Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.'..." (Error de tarea provisional: Código de error: 20906, mensaje de error: Error de ejecución de paquete, Salida: {"OperationErrorMessages": "Error: No se puede cargar el archivo o ensamblado "Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" ni una de sus dependencias. La definición de manifiesto de ensamblado encontrada no coincide con la referencia de ensamblado).

Una posible causa es que el entorno de ejecución de integración autohospedado no está instalado o actualizado correctamente. Se recomienda descargar y volver a instalar el entorno de ejecución de integración autohospedado más reciente. Puede obtener más información en [Creación y configuración de un entorno de ejecución de integración autohospedado](create-self-hosted-integration-runtime.md#installation-best-practices).

### <a name="error-message-a-connection-is-required-when-requesting-metadata-if-you-are-working-offline-uncheck-work-offline-on-the-ssis-menu-to-enable-the-connection"></a>Mensaje de error: "A connection is required when requesting metadata. If you are working offline, uncheck Work Offline on the SSIS menu to enable the connection" (Se requiere una conexión al solicitar metadatos. Si está trabajando sin conexión, desactive Trabajar sin conexión en el menú SSIS para habilitar la conexión).

* Causa posible y acción recomendada:
  * Si se muestra también un mensaje de advertencia que indica que el componente no admite el uso del administrador de conexiones con el valor de ConnectByProxy establecido en true en el registro de ejecución, significa que se usa un administrador de conexiones en un componente que todavía no ha admitido "ConnectByProxy". Los componentes admitidos se pueden encontrar en [Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR en Azure Data Factory](self-hosted-integration-runtime-proxy-ssis.md#enable-ssis-packages-to-connect-by-proxy).
  * El registro de ejecución se puede encontrar en el [informe de SSMS](/sql/integration-services/performance/monitor-running-packages-and-other-operations#reports) o en la carpeta de registro especificada en la actividad de ejecución de paquetes SSIS.
  * Como alternativa, la red virtual también se puede usar para acceder a los datos locales. Se puede encontrar más información en [Unión de Azure-SSIS Integration Runtime a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-staging-task-status-failed-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-ssis-executor-exit-code--1n-loglocation-ssistelemetryexecutionlog-effectiveintegrationruntime--executionduration--durationinqueue--integrationruntimequeue--"></a>Mensaje de error: "Staging task status: Failed. Staging task error: ErrorCode: 2906, ErrorMessage: Package execution failed., Output: {"OperationErrorMessages": "SSIS Executor exit code: -1.\n", "LogLocation": "...\\SSISTelemetry\\ExecutionLog\\...", "effectiveIntegrationRuntime": "...", "executionDuration": ..., "durationInQueue": { "integrationRuntimeQueue": ... }}" (Estado de la tarea provisional: Con error. Error de la tarea provisional: Código de error: 2906, Mensaje de error: Error de ejecución del paquete, Salida: {"OperationErrorMessages": "Código de salida del ejecutor SSIS: -1.\n", "LogLocation": "...SSISTelemetry ExecutionLog...", "effectiveIntegrationRuntime": "...", "executionDuration": ..., "durationInQueue": { "integrationRuntimeQueue": ... }})

Asegúrese de que el entorno de ejecución de Visual C++ está instalado en la máquina del entorno de ejecución de integración autohospedado. Puede encontrar más información en [Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR en Azure Data Factory](self-hosted-integration-runtime-proxy-ssis.md#prepare-the-self-hosted-ir).

### <a name="multiple-package-executions-are-triggered-unexpectedly"></a>Se desencadenan varias ejecuciones de paquetes de forma inesperada

* Causa posible y acción recomendada:
  * La actividad de procedimiento almacenado de ADF o la actividad de búsqueda se utilizan para desencadenar la ejecución de paquetes SSIS. El comando t-sql puede encontrarse con problemas transitorios y desencadenar una nueva ejecución, lo que provocaría varias ejecuciones de paquetes.
  * Use la actividad ExecuteSSISPackage en su lugar, que garantiza que la ejecución de paquetes no volverá a ejecutarse a menos que el usuario establezca un recuento de reintentos en la actividad. Se puede encontrar más información en [https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity](./how-to-invoke-ssis-package-ssis-activity.md).
  * Refine el comando de t-sql para poder volver a realizar la ejecución; para ello, compruebe si ya se ha desencadenado una ejecución.

### <a name="package-execution-takes-too-long"></a>La ejecución del paquete tarda demasiado

Estas son las posibles causas y las acciones recomendadas:

* Se han programado demasiadas ejecuciones de paquetes en SSIS Integration Runtime. Todas estas ejecuciones estarán esperando su turno en una cola.
  * Para determinar el valor máximo, use esta fórmula:

    Recuento de ejecución en paralelo máximo por IR = recuento de nodos * Máxima ejecución en paralelo por nodo
  * Para obtener información sobre cómo establecer el número de nodos y la ejecución en paralelo máxima por nodo, vea [Creación de una instancia de Azure-SSIS Integration Runtime en Azure Data Factory](create-azure-ssis-integration-runtime.md).
* SSIS Integration Runtime se ha detenido o su estado es incorrecto. Para obtener información sobre cómo comprobar el estado y los errores de SSIS Integration Runtime, vea [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime).

También es recomendable definir un tiempo de expiración en la pestaña **General**: ![Definir propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png).

### <a name="poor-performance-in-package-execution"></a>Rendimiento deficiente en la ejecución del paquete

Pruebe estas acciones:

* Asegúrese de que SSIS Integration Runtime esté en la misma región que el origen y el destino de los datos.

* Defina el nivel de registro de la ejecución del paquete como **Rendimiento** para recopilar información sobre la duración para cada componente de la ejecución. Para obtener más información, consulte [Registro de Integration Services (SSIS)](/sql/integration-services/performance/integration-services-ssis-logging).

* Compruebe el rendimiento del nodo de IR en Azure Portal:
  * Para obtener información sobre cómo supervisar SSIS Integration Runtime, consulte [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime).
  * Para encontrar el historial de CPU/memoria para SSIS Integration Runtime, consulte las métricas de la instancia de Data Factory en Azure Portal.
    ![Supervisar las métricas de SSIS Integration Runtime](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
