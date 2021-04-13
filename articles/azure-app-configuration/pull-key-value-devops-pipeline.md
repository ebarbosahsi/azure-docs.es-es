---
title: Envío de configuraciones desde App Configuration con Azure Pipelines
description: Aprenda a utilizar Azure Pipelines para extraer pares clave-valor a un almacén de App Configuration
services: azure-app-configuration
author: drewbatgit
ms.service: azure-app-configuration
ms.topic: how-to
ms.date: 11/17/2020
ms.author: drewbat
ms.openlocfilehash: 1c01984f6a359c0fd1f5d06d26d97d4a84973f57
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106056794"
---
# <a name="pull-settings-to-app-configuration-with-azure-pipelines"></a>Extracción de configuraciones a App Configuration con Azure Pipelines

La tarea [Azure App Configuration](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task) extrae los pares clave-valor del almacén de App Configuration y los establece como variables de Azure Pipelines, que pueden ser consumidos por las tareas posteriores. Esta tarea complementa la tarea [Azure App Configuration Push](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task-push) que envía pares clave-valor desde un archivo de configuración al almacén de App Configuration. Para más información, consulte [Envío de configuraciones a App Configuration con Azure Pipelines](push-kv-devops-pipeline.md).

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
- Almacén de App Configuration: cree uno de forma gratuita en [Azure Portal](https://portal.azure.com).
- Proyecto de Azure DevOps: [cree uno gratis](https://go.microsoft.com/fwlink/?LinkId=2014881).
- Tarea Azure App Configuration: descárguela de manera gratuita de [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task#:~:text=Navigate%20to%20the%20Tasks%20tab,the%20Azure%20App%20Configuration%20instance.).  

## <a name="create-a-service-connection"></a>Creación de una conexión de servicio

Una [conexión de servicio](/azure/devops/pipelines/library/service-endpoints) permite acceder a los recursos en la suscripción de Azure desde el proyecto de Azure DevOps.

1. En Azure DevOps, vaya al proyecto que contiene la canalización de destino y abra la **Configuración del proyecto** en la parte inferior izquierda.
1. En **Canalizaciones**, seleccione **Conexiones de servicio**.
1. Si no tiene ninguna conexión de servicio, haga clic en el botón **Crear una conexión de servicio** en el centro de la pantalla. Si no, haga clic en **Nueva conexión de servicio** en la parte superior derecha de la página.
1. Seleccione **Azure Resource Manager**.
![Captura de pantalla que muestra la selección de Azure Resource Manager en la lista desplegable Nueva conexión de servicio.](./media/new-service-connection.png)
1. En el cuadro de diálogo **Método de autenticación**, seleccione **Entidad de servicio (automática)** .
    > [!NOTE]
    > Actualmente no se admite la autenticación de **identidad administrada** para la tarea de App Configuration.
1. Rellene la suscripción y el recurso. Asigne un nombre a la conexión del servicio.

Ahora que ha creado la conexión de servicio, busque el nombre de la entidad de servicio asignada a ella. En el paso siguiente, agregará una nueva asignación de roles a esta entidad de servicio.

1. Vaya a **Configuración del proyecto** > **Conexiones de servicio**.
1. Seleccione la conexión de servicio que creó en la sección anterior.
1. Seleccione **Administrar entidad de servicio**.
1. Tome nota del **nombre para mostrar** que se muestra.

## <a name="add-role-assignment"></a>Agregar asignación de roles

Asigne el rol de App Configuration adecuado a la conexión de servicio que se va a utilizar en la tarea de forma que esta pueda acceder al almacén de App Configuration.

1. Navegue al almacén de App Configuration de destino. Para ver un tutorial sobre cómo configurar un almacén de App Configuration, consulte [Creación de un almacén de App Configuration](./quickstart-dotnet-core-app.md#create-an-app-configuration-store) en una de las guías de inicio rápido de Azure App Configuration.
1. En el lado izquierdo, seleccione **Control de acceso (IAM)** .
1. En el lado derecho, haga clic en el botón **Agregar asignaciones de roles**.
![Captura de pantalla que muestra el botón Agregar asignaciones de roles.](./media/add-role-assignment-button.png)
1. En **Rol**, seleccione **Lector de los datos de App Configuration**. Este rol permite que la tarea lea en el almacén de App Configuration. 
1. Seleccione la entidad de servicio asociada con la conexión de servicio que creó en la sección anterior.
![Captura de pantalla que muestra el cuadro de diálogo Agregar asignación de roles.](./media/add-role-assignment-reader.png)

> [!NOTE]
> Para resolver unas referencias de Azure Key Vault en App Configuration, también se debe conceder permiso a la conexión de servicio para leer secretos en las instancias de Azure Key Vault a las que se hace referencia.
  
## <a name="use-in-builds"></a>Uso en compilaciones

En esta sección se explicará cómo usar la tarea Azure App Configuration en una canalización de compilación de Azure DevOps.

1. Vaya a la página de canalización de compilación al hacer clic en **Canalizaciones** > **Canalizaciones**. Para obtener documentación sobre la canalización de compilación, consulte [Creación de su primera canalización](/azure/devops/pipelines/create-first-pipeline?tabs=net%2Ctfs-2018-2%2Cbrowser).
      - Si va a crear una nueva canalización de compilación, en el último paso del proceso, en la pestaña **Revisar**, seleccione **Mostrar asistente** en el lado derecho de la canalización.
      ![Captura de pantalla que muestra el botón Mostrar asistente de una nueva canalización.](./media/new-pipeline-show-assistant.png)
      - Si usa una canalización de compilación existente, haga clic en el botón **Editar** en la parte superior derecha.
      ![Captura de pantalla que muestra el botón Editar de una canalización existente.](./media/existing-pipeline-show-assistant.png)
1. Busque la tarea **Azure App Configuration**.
![Captura de pantalla que muestra el cuadro de diálogo Agregar tarea con Azure App Configuration en el cuadro de búsqueda.](./media/add-azure-app-configuration-task.png)
1. Configure los parámetros necesarios para que la tarea extraiga los pares clave-valor del almacén de App Configuration. Las descripciones de los parámetros están disponibles en la sección **Parámetros** siguiente y en la información sobre herramientas junto a cada parámetro.
      - Establezca el parámetro **Suscripción de Azure** en el nombre de la conexión de servicio creada en un paso anterior.
      - Establezca **App Configuration name** (Nombre de App Configuration) en el nombre del recurso del almacén de App Configuration.
      - Deje los valores predeterminados en los restantes parámetros.
![Captura de pantalla que muestra los parámetros de la tarea de App Configuration.](./media/azure-app-configuration-parameters.png)
1. Guarde y ponga en cola una compilación. El registro de compilación mostrará los errores que se produjeron durante la ejecución de la tarea.

## <a name="use-in-releases"></a>Uso en versiones

En esta sección se explicará cómo usar la tarea Azure App Configuration en las canalizaciones de versión de Azure DevOps.

1. Vaya a la página de canalización de versión; para ello, seleccione **Canalizaciones** > **Versiones**. Para obtener documentación sobre la canalización de versión, consulte [Canalizaciones de versión](/azure/devops/pipelines/release).
1. Elija una canalización de versión existente. Si no tiene una, seleccione **Nueva canalización** para crear una.
1. Seleccione el botón **Editar** en la esquina superior derecha para editar la canalización de versión.
1. En el menú desplegable **Tareas**, seleccione la **Fase** en la que quiere agregar la tarea. Puede encontrar más información sobre las fases [aquí](/azure/devops/pipelines/release/environments).
![Captura de pantalla que muestra la fase seleccionada en el menú desplegable Tareas.](./media/pipeline-stage-tasks.png)
1. Haga clic en **+** junto al trabajo al que quiere agregar una nueva tarea.
![Captura de pantalla que muestra el botón de signo más junto al trabajo.](./media/add-task-to-job.png)
1. Busque la tarea **Azure App Configuration**.
![Captura de pantalla que muestra el cuadro de diálogo Agregar tarea con Azure App Configuration en el cuadro de búsqueda.](./media/add-azure-app-configuration-task.png)
1. Configure los parámetros necesarios dentro de la tarea para extraer los pares clave-valor del almacén de App Configuration. Las descripciones de los parámetros están disponibles en la sección **Parámetros** siguiente y en la información sobre herramientas junto a cada parámetro.
      - Establezca el parámetro **Suscripción de Azure** en el nombre de la conexión de servicio creada en un paso anterior.
      - Establezca **App Configuration name** (Nombre de App Configuration) en el nombre del recurso del almacén de App Configuration.
      - Deje los valores predeterminados en los restantes parámetros.
1. Guarde y ponga en cola una versión. El registro de versión mostrará los errores que se hayan encontrado durante la ejecución de la tarea.

## <a name="parameters"></a>Parámetros

La tarea Azure App Configuration usa los siguientes parámetros:

- **Suscripción de Azure**: una lista desplegable que contiene las conexiones de servicio de Azure disponibles. Para actualizar la lista de conexiones de servicio de Azure disponibles, presione el botón **Refresh Azure subscription** (Actualizar suscripción a Azure) a la derecha del cuadro de texto.
- **Nombre de App Configuration**: una lista desplegable que carga los almacenes de configuración disponibles para la suscripción seleccionada. Para actualizar la lista de almacenes de configuración disponibles, presione el botón **Refresh App Configuration Name** (Actualizar nombre de App Configuration) a la derecha del cuadro de texto.
- **Key Filter** (Filtro de clave): el filtro se puede utilizar para seleccionar los pares clave-valor que se solicitan desde Azure App Configuration. Un valor de * seleccionará todos los pares clave-valor. Para más información, consulte [Consulta de claves y valores](concept-key-value.md#query-key-values).
- **Etiqueta**: especifica la etiqueta que se debe usar al seleccionar los pares clave-valor del almacén de App Configuration. Si no se proporciona ninguna etiqueta, se recuperarán los pares clave-valor que no tengan etiqueta. No se permiten los caracteres siguientes: , *.
- **Trim Key Prefix** (Recortar prefijo de clave): especifica uno o más prefijos que se deben recortar de las claves de App Configuration antes de establecerlos como variables. Se pueden separar varios prefijos mediante un carácter de nueva línea.

## <a name="use-key-values-in-subsequent-tasks"></a>Uso de pares clave-valor en tareas posteriores

Los pares clave-valor que se recuperan de App Configuration se establecen como variables de canalización, que son accesibles como variables de entorno. La clave de la variable de entorno es la clave del par clave-valor que se recupera de App Configuration después de recortar el prefijo, si se especifica.

Por ejemplo, si una tarea posterior ejecuta un script de PowerShell, podría consumir un par clave-valor con la clave 'myBuildSetting' de esta forma:
```powershell
echo "$env:myBuildSetting"
```
Y el valor se imprimirá en la consola.

> [!NOTE]
> Las referencias de Azure Key Vault en App Configuration se resolverán y se establecerán como [variables secretas](/azure/devops/pipelines/process/variables#secret-variables). En las canalizaciones de Azure, las variables secretas se enmascaran desde el registro. No se pasan a las tareas como variables de entorno y, en su lugar, se deben pasar como entradas. 

## <a name="troubleshooting"></a>Solución de problemas

Si se produce un error inesperado, los registros de depuración se pueden habilitar; para ello, establezca la variable de canalización `system.debug` en `true`.

## <a name="faq"></a>Preguntas más frecuentes

**¿Cómo puedo componer mi configuración a partir de varias claves y etiquetas?**

Hay ocasiones en las que es posible que la configuración deba componerse a partir de varias etiquetas, por ejemplo, default y dev. Se pueden utilizar varias tareas de App Configuration en una canalización para implementar este escenario. Los pares clave-valor recuperados por una tarea en un paso posterior reemplazarán cualquier valor de los pasos anteriores. En el ejemplo mencionado anteriormente, se puede utilizar una tarea para seleccionar los pares clave-valor con la etiqueta default mientras que una segunda tarea puede seleccionar los pares clave-valor con la etiqueta dev. Las claves con la etiqueta dev invalidarán las mismas claves con la etiqueta default.
