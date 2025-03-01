---
title: Línea de referencia de seguridad de Azure para Azure Data Factory
description: La línea de base de seguridad de Azure Data Factory proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: a21ae2ce79c500455c5735f4d82e7852e8474ad1
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105559152"
---
# <a name="azure-security-baseline-for-azure-data-factory"></a>Línea de referencia de seguridad de Azure para Azure Data Factory

Esta línea de base de seguridad aplica las instrucciones descritas en [Azure Security Benchmark versión 1.0](../security/benchmarks/overview-v1.md) a Azure Data Factory. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de **controles de seguridad** que se definen en Azure Security Benchmark y en las directrices relacionadas aplicables a Azure Data Factory. Se han excluido los **controles** no aplicables a Azure Data Factory.

Para ver cómo Azure Data Factory se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure Data Factory](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Al crear una instancia de Azure-SSIS Integration Runtime (IR), tiene la opción de unirse a ella con una red virtual. Esto permitirá a Azure Data Factory crear determinados recursos de red, como un grupo de seguridad de red (NSG) y un equilibrador de carga. También tiene la posibilidad de proporcionar su propia dirección IP pública estática o que Azure Data Factory cree una.  En el NSG que Azure Data Factory crea automáticamente, el puerto 3389 está abierto para todo el tráfico de forma predeterminada. Se debe bloquear para garantizar que solo los administradores tienen acceso.

IR autohospedado puede implementarse en un equipo local o en una máquina virtual de Azure dentro de una red virtual. Asegúrese de que la implementación de la subred de la red virtual tenga un grupo de seguridad de red configurado solo para permitir el acceso administrativo. Azure-SSIS IR no permite la salida del puerto 3389 de forma predeterminada en la regla de Firewall de Windows de cada nodo IR, por motivos de protección. Puede proteger los recursos configurados por la red virtual mediante la asociación de un NSG con la subred y el establecimiento de reglas estrictas.

Si Private Link está disponible, use puntos de conexión privados para proteger los recursos que se vinculan a la canalización de Azure Data Factory, como Azure SQL Server. Con Private Link, el tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft y elimina la exposición a la red pública de Internet.

- [Creación de una instancia de Azure-SSIS IR](create-azure-ssis-integration-runtime.md)

- [Creación y configuración de un entorno de ejecución de integración autohospedado](create-self-hosted-integration-runtime.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un NSG con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Unión de una instancia de Azure-SSIS IR a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md#virtual-network-configuration)

- [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: Use Azure Security Center y corrija las recomendaciones de protección de red para la red virtual y el grupo de seguridad de red asociado a la implementación de Integration Runtime.

Además, habilite los registros de flujo de grupo de seguridad de red (NSG) para el NSG que protege la implementación de Integration Runtime y envíe registros a una cuenta de Azure Storage para la auditoría del tráfico.

También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: Habilite DDoS Protection estándar en las redes virtuales asociadas a la implementación de Integration Runtime para protegerse de los ataques por denegación de servicio distribuidos. Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas o no utilizadas.

- [Configuración de la protección contra DDoS](../ddos-protection/manage-ddos-protection.md)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](../security-center/azure-defender.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: Habilite los registros de flujo de grupo de seguridad de red (NSG) para el NSG que protege la implementación de Integration Runtime y envíe registros a una cuenta de Azure Storage para la auditoría del tráfico.

También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Instrucciones**: Si desea inspeccionar el tráfico de salida de Azure-SSIS IR, puede enrutar el tráfico iniciado desde allí al dispositivo de firewall local mediante la tunelización forzada de Azure ExpressRoute o a un dispositivo virtual de red (NVA) de Azure Marketplace que admita capacidades IDS/IPS. Si la detección y/o la prevención de intrusiones basadas en la inspección de carga no es un requisito, se puede usar Azure Firewall con la inteligencia sobre amenazas.

- [Unión de una instancia de Azure-SSIS Integration Runtime a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md)

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas con Azure Firewall](../firewall/threat-intel.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: Use etiquetas de servicio de red virtual para definir los controles de acceso a la red en un grupo de seguridad de red (NSG) o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, DataFactoryManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico de entrada para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Descripción y uso de las etiquetas de servicio](../virtual-network/service-tags-overview.md)

- [Descripción de las etiquetas de servicio específicas de Azure Data Factory](join-azure-ssis-integration-runtime-virtual-network.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para los recursos y la configuración de red asociados a las instancias de Azure Data Factory con Azure Policy. Use alias de Azure Policy en los espacios de nombres "Microsoft.DataFactory" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de las instancias de Azure Data Factory. También puede usar las definiciones de directiva integradas relacionadas con las redes o las instancias de Azure Data Factory, como, "DDoS Protection Standard debe estar habilitado".

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ejemplos de Azure Policy para redes](../governance/policy/samples/index.md)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: Use etiquetas para los recursos relacionados con la seguridad de red y el flujo de tráfico de las instancias de Azure Data Factory para proporcionar metadatos y organización lógica.

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", para asegurarse de que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: Use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con las instancias de Azure Data Factory. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/essentials/activity-log.md#view-the-activity-log)

- [Creación de alertas en Azure Monitor](../azure-monitor/alerts/alerts-activity-log.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Instrucciones**: Ingiera registros a través de Azure Monitor para agregar datos de seguridad generados por Azure Data Factory. Dentro de Azure Monitor, podrá consultar el área de trabajo de Log Analytics que está configurada para recibir los registros de actividad de Azure Data Factory. Use cuentas de Azure Storage para almacenar registros a largo plazo o archivarlos, o use los centros de eventos para exportar los datos a otros sistemas.

Como alternativa, puede habilitar datos incorporados en Azure Sentinel o una Administración de eventos e información de seguridad (SIEM) de terceros. También puede integrar Azure Data Factory con Git para aprovechar varias ventajas del control de código fuente, como la capacidad de realizar un seguimiento de los cambios y auditarlos, y la capacidad de revertir los cambios que presenten errores.

- [Configuración del diagnóstico](../azure-monitor/essentials/diagnostic-settings.md#create-in-azure-portal)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Control de código fuente en Azure Data Factory](source-control.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Para el registro de la auditoria del plano de control, habilite la configuración de diagnóstico del registro de actividad de Azure y envíela a un área de trabajo de Log Analytics, Azure Event Hubs o una cuenta de Azure Storage para su archivo. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control para los recursos de Azure.

Use la configuración de diagnóstico para configurar los registros de diagnóstico para los recursos que no son de proceso en Azure Data Factory, como métricas y datos de ejecución de canalización. Azure Data Factory almacena los datos de ejecución de canalización durante 45 días. Para conservar estos datos durante un período de tiempo más largo, guarde los registros de diagnóstico en una cuenta de almacenamiento de cara a la auditoría o la inspección manual y especifique el tiempo de retención en días.  También puede transmitir los registros a Azure Event Hubs o enviar los registros a un área de trabajo de Log Analytics para su análisis.

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](../azure-monitor/essentials/activity-log.md)

- [Descripción de los registros de diagnóstico de Azure Data Factory](monitor-using-azure-monitor.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Instrucciones**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, puede usar Azure Monitor para recopilar datos de la máquina virtual. La instalación de la extensión de máquina virtual de Log Analytics permite a Azure Monitor recopilar datos de las máquinas virtuales de Azure. Así, Azure Security Center podrá proporcionar la supervisión del registro de eventos de seguridad para Virtual Machines. Dado el volumen de datos que genera el registro de eventos de seguridad, no se almacena de forma predeterminada. 

Si su organización quiere conservar los datos del registro de eventos de seguridad, se pueden almacenar en un nivel de recopilación de datos, donde se pueden consultar en Log Analytics.

- [Recopilación de datos de Azure Virtual Machines con Azure Monitor](../azure-monitor/vm/quick-collect-azurevm.md)

- [Habilitación de la recopilación de datos en Azure Security Center](../security-center/security-center-enable-data-collection.md#data-collection-tier)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: Habilite la configuración de diagnóstico para Azure Data Factory. Si decide almacenar registros en un área de trabajo de Log Analytics, establezca el período de retención de dicha área de trabajo de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

- [Habilitación de los registros de diagnóstico en Azure Data Factory](monitor-using-azure-monitor.md)

- [Configuración de parámetros de retención de registros de áreas de trabajo de Log Analytics](../azure-monitor/logs/manage-cost-storage.md#change-the-data-retention-period)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Instrucciones**: Habilite la configuración de diagnóstico para Azure Data Factory y envíe registros a un área de trabajo de Log Analytics. Use Log Analytics para analizar y supervisar los registros en busca de comportamientos anómalos y revise periódicamente los resultados. Asegúrese de habilitar también la configuración de diagnóstico para los almacenes de datos relacionados con las implementaciones de Azure Data Factory. Consulte la base de referencia de seguridad de cada servicio para obtener instrucciones sobre cómo habilitar la configuración de diagnóstico.

Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, habilite también la configuración de diagnóstico para la máquina virtual.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Esquema de Log Analytics](./monitor-using-azure-monitor.md#schema-of-logs-and-events)

- [Recopilación de datos de una máquina virtual de Azure con Azure Monitor](../azure-monitor/vm/quick-collect-azurevm.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Instrucciones**: Puede generar alertas sobre las métricas admitidas en Data Factory. Para hacerlo, vaya a la sección Alertas y métricas de Azure Monitor.

Configure los valores de diagnóstico para Azure Data Factory y envíe registros a un área de trabajo de Log Analytics. En el área de trabajo Log Analytics, configure las alertas que se activarán cuando se cumpla un conjunto predefinido de condiciones. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

Además, asegúrese de habilitar la configuración de diagnóstico para los servicios relacionados con los almacenes de datos. Puede consultar la base de referencia de seguridad de cada servicio para obtener instrucciones.

- [Alertas en Azure Data Factory](./monitor-visually.md#alerts)

- [Página de todas las métricas compatibles](../azure-monitor/essentials/metrics-supported.md)

- [Configuración de las alertas en el área de trabajo de Log Analytics](../azure-monitor/alerts/alerts-log.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía**: Si ejecuta Integration Runtime en una máquina virtual de Azure, puede usar Microsoft Antimalware para Azure Cloud Services y Virtual Machines y configurar las máquinas virtuales para registrar eventos en una cuenta de Azure Storage. Configure un área de trabajo de Log Analytics para ingerir los eventos de las cuentas de almacenamiento y crear alertas cuando corresponda. Siga las recomendaciones en Azure Security Center: "Proceso y aplicaciones". 

- [Configuración de Microsoft Antimalware para Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

- [Habilitación de la supervisión de nivel de invitado para Virtual Machines](../cost-management-billing/cloudyn/azure-vm-extended-metrics.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, puede habilitar el registro de auditoría de la línea de comandos. Azure Security Center proporciona supervisión del registro de eventos de seguridad para las máquinas virtuales de Azure.  Security Center aprovisiona Microsoft Monitoring Agent en todas las máquinas virtuales de Azure compatibles y en las que se crean si el aprovisionamiento automático está habilitado o puede instalar el agente manualmente.  El agente habilita el evento 4688 de creación de procesos y el campo CommandLine dentro del evento 4688. El registro de eventos registra los nuevos procesos creados en la VM y los servicios de detección de Security Center supervisa dichos procesos.

- [Recopilación de datos en Azure Security Center](../security-center/security-center-enable-data-collection.md#data-collection-tier)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: control de identidades y acceso](../security/benchmarks/security-control-identity-access-control.md).* .

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: En Azure Data Factory, asegúrese de realizar un seguimiento y conciliar el acceso de usuarios de forma periódica. Para crear instancias de Data Factory, la cuenta de usuario que use para iniciar sesión en Azure debe ser miembro de los roles colaborador o propietario, o ser administrador de la suscripción de Azure.

Además, en el nivel de inquilino, Azure Active Directory (Azure AD) tiene roles integrados que se deben asignar explícitamente y se pueden consultar. Use el módulo PowerShell de Azure AD para realizar consultas ad hoc y detectar cuentas que son miembros de grupos administrativos que tienen acceso administrativo al plano de control de las instancias de Azure Data Factory.

Si bien Azure AD es el método recomendado para administrar el acceso de los usuarios, tenga en cuenta que, si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, esta también puede tener cuentas locales. Las cuentas locales y de dominio deben revisarse y administrarse, normalmente con una superficie de memoria mínima. Además, se recomienda revisar Privileged Identity Manager en la característica Just-in-Time para reducir la disponibilidad de los permisos administrativos.

- [Roles y permisos para Azure Data Factory](concepts-roles-permissions.md)

- [Información sobre Privileged Identity Manager](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

- [Información de cuentas locales](../active-directory/devices/assign-local-admin.md#manage-the-device-administrator-role)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: Azure Data Factory usa Azure Active Directory (Azure AD) para proporcionar acceso a Azure Portal, así como a la consola de Azure Data Factory. Sin embargo, Azure AD no incluye el concepto de contraseñas predeterminadas; el usuario es responsable de cambiar o no permitir las contraseñas predeterminadas para las aplicaciones personalizadas o de terceros.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas para el acceso al plano de control de Azure (Azure Portal), así como a la consola de Azure Data Factory. Use la administración de identidades y acceso de Azure Security Center para supervisar el número de cuentas administrativas en Azure Active Directory (Azure AD).

Además, para ayudarle a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center o las directivas integradas de Azure Policy, por ejemplo:
- Debe haber más de un propietario asignado a su suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

Si ejecuta Integration Runtime en una máquina virtual de Azure, las cuentas de administrador en Azure Virtual Machines también se pueden configurar con Azure Privileged Identity Manager (PIM). Azure Privileged Identity Manager proporciona varias opciones, como elevación Just-in-Time, autenticación multifactor y opciones de delegación, de modo que los permisos solo están disponibles durante intervalos de tiempo específicos y requieren una segunda persona para su aprobación.

- [Descripción de identidad y acceso en Azure Security Center](../security-center/security-center-identity-access.md)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Información sobre Privileged Identity Manager](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Roles y permisos para Azure Data Factory](concepts-roles-permissions.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Use un registro de aplicaciones de Azure (entidad de servicio) para recuperar un token que la aplicación o función puedan usar para acceder e interactuar con los almacenes de Recovery Services.

- [Llamada a las API REST de Azure](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

- [Registro de la aplicación cliente (entidad de servicio) con Azure Active Directory (Azure AD)](/rest/api/azure/#register-your-client-application-with-azure-ad)

- [Información de API de Azure Recovery Services](/rest/api/recoveryservices/)

- [Información sobre la API de REST para Azure Data Factory](/rest/api/datafactory/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía**: utilice estaciones de trabajo de acceso con privilegios (PAW) que tengan configurada la autenticación multifactor para iniciar sesión en los recursos de Azure y configurarlos. 

- [Más información sobre las estaciones de trabajo con privilegios de acceso](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: Use los informes de seguridad de Azure Active Directory (Azure AD) para la generación de registros y alertas cuando se produzcan actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, también puede incorporar la máquina virtual a Azure Sentinel. Microsoft Azure Sentinel es una solución de administración de eventos de información de seguridad (SIEM) y respuesta automatizada de orquestación de seguridad (SOAR) que es escalable y nativa de la nube. Azure Sentinel ofrece análisis de seguridad inteligente e inteligencia frente a amenazas en toda la empresa, de forma que proporciona una única solución para la detección de alertas, la visibilidad de amenazas, la búsqueda proactiva y la respuesta a amenazas.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: Use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Una factoría de datos se puede asociar con una identidad administrada para recursos de Azure que represente esa factoría de datos concreta. Esta identidad administrada se puede usar para la autenticación de Azure SQL Database. Puede acceder a la factoría designada y copiar datos desde la base de datos o en la base de datos usando esta identidad.

Si ejecuta Integration Runtime (IR) en una máquina virtual de Azure, puede usar identidades administradas para autenticarse en cualquier servicio que admita la autenticación de Azure Active Directory (Azure AD), incluido Key Vault, sin necesidad de credenciales en el código. El código que se ejecuta en una máquina virtual puede usar la identidad administrada para solicitar tokens de acceso de los servicios que admiten la autenticación de Azure AD.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [¿Qué son las identidades administradas de recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md)

- [Copia y transformación de datos en Azure SQL Database mediante Azure Data Factory](connector-azure-sql-database.md)

- [Configuración y administración de la autenticación de Azure AD con Azure SQL Database](../azure-sql/database/authentication-aad-configure.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure Active Directory (Azure AD) proporciona registros para ayudarle a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

Si ejecuta Runtime Integration en una máquina virtual de Azure, tendrá que revisar los grupos de seguridad locales y los usuarios para asegurarse de que no hay cuentas inesperadas que puedan poner en peligro el sistema.

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

- [Descripción de los informes de Azure AD](../active-directory/reports-monitoring/index.yml)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: Tiene acceso a los orígenes de registro de eventos de riesgo, auditoría y actividad de inicio de sesión de Azure Active Directory (Azure AD), que permiten la integración con cualquier herramienta SIEM o de supervisión. Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas de registro deseadas en Log Analytics.

Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, incorpore la máquina virtual a Azure Sentinel. Microsoft Azure Sentinel es una solución de administración de eventos de información de seguridad (SIEM) y respuesta automatizada de orquestación de seguridad (SOAR) que es escalable y nativa de la nube. Azure Sentinel ofrece análisis de seguridad inteligente e inteligencia frente a amenazas en toda la empresa, de forma que proporciona una única solución para la detección de alertas, la visibilidad de amenazas, la búsqueda proactiva y la respuesta a amenazas.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Autorización del acceso a recursos de Event Hubs mediante Azure AD](../event-hubs/authorize-access-azure-active-directory.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización para recursos de Azure Data Factory, como Azure SQL Database o Azure Virtual Machines. Para la desviación de comportamiento de inicio de sesión de cuenta en el plan de control (Azure Portal), use las características de detección de riesgos y de Azure AD Identity Protection para configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Configuración y administración de la autenticación de Azure AD con SQL](../azure-sql/database/authentication-aad-configure.md?tabs=azure-powershell)

- [Habilitación de la autenticación de Azure AD para Azure-SSIS Integration Runtime](enable-aad-authentication-azure-ssis-ir.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Instrucciones**: en escenarios de soporte técnico en los que Microsoft necesita acceder a los datos del cliente, la Caja de seguridad del cliente de Azure proporciona una interfaz para que los clientes revisen y aprueben o rechacen las solicitudes de acceso a los datos del cliente. Tenga en cuenta que aunque la Caja de seguridad de Azure no esté disponible para Azure Data Factory, admite instancias de Azure SQL Database y Azure Virtual Machines.

 

- [Descripción de la Caja de seguridad del cliente](../security/fundamentals/customer-lockbox-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

use la característica de clasificación y detección de datos de Azure SQL Database. La clasificación y detección de datos proporciona funcionalidades avanzadas integradas en Azure SQL Database para detectar, clasificar, etiquetar y proteger los datos confidenciales de las bases de datos.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Uso de la detección y la clasificación de datos para Azure SQL Server](../azure-sql/database/data-discovery-and-classification-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Las instancias de Integration Runtime deben separarse por red virtual (VNet) o subred, y etiquetarse debidamente.

También se puede usar puntos de conexión privados para realizar el aislamiento de red. Un punto de conexión privado de Azure es una interfaz de red que le conecta de forma privada y segura a un servicio con la tecnología de Azure Private Link. El punto de conexión privado usa una dirección IP privada de la red virtual, colocando el servicio de manera eficaz en su red virtual.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Descripción de Private Link](../private-link/private-endpoint-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Instrucciones**: En el caso de orígenes de datos (como Azure SQL Database) que almacenan o procesan información confidencial para la implementación de Azure Data Factory, marque los recursos relacionados como confidenciales mediante etiquetas.

Si está disponible una instancia de Private Link, use puntos de conexión privados para proteger los recursos que se vinculan a la canalización de Azure Data Factory,. El tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft, eliminando la exposición a la red pública de Internet. También puede reducir el riesgo de la exfiltración de datos mediante la configuración de un conjunto estricto de reglas de salida en un grupo de seguridad de red (NSG) y la Asociación de ese NSG con la subred.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de un NSG con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md) 

- [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía**: Si el almacén de datos en la nube es compatible con HTTPS o TLS, todas las transferencias de datos entre los servicios de movimiento de datos de Data Factory y un almacén de datos en la nube se realizan a través del canal seguro HTTPS o TLS. La versión de TLS usada es la 1.2. 

Todas las conexiones a Azure SQL Database y Azure Synapse Analytics (antes SQL Data Warehouse) requieren cifrado (SSL/TLS) siempre que haya datos en tránsito hacia y desde la base de datos. Al crear una canalización con JSON, agregue la propiedad encryption y establézcala en true en la cadena de conexión. Para Azure Storage, puede usar HTTPS en la cadena de conexión.

- [Descripción del cifrado en tránsito en Azure Data Factory](data-movement-security-considerations.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: Si usa Azure Data Factory para copiar y transformar las instancias de Azure SQL Database, use la característica de detección y clasificación de datos de Azure SQL Database. La clasificación y detección de datos proporciona funcionalidades avanzadas integradas en Azure SQL Database para detectar, clasificar, etiquetar y proteger los datos confidenciales de las bases de datos.

Las características de clasificación y detección de datos todavía no están disponibles para otros servicios de Azure.

- [Uso de la detección y la clasificación de datos para Azure SQL Server](../azure-sql/database/data-discovery-and-classification-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Utilice el control de acceso basado en roles de Azure (Azure RBAC) para controlar el acceso al plano de control de Azure Data Factory (Azure Portal).

Para crear instancias de Data Factory, la cuenta de usuario que use para iniciar sesión en Azure debe ser un miembro de los roles colaborador o propietario, o ser administrador de la suscripción de Azure.

En el caso de los orígenes de datos de Data Factory, como Azure SQL Database, consulte la base de referencia de seguridad de ese servicio para obtener más información sobre RBAC de Azure.

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Roles y permisos para Azure Data Factory](concepts-roles-permissions.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Storage ni los recursos de proceso. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Se recomienda habilitar el mecanismo de cifrado de datos para los almacenes de datos relacionados con las implementaciones de Azure Data Factory. Puede consultar la base de referencia de seguridad de ese servicio para obtener más información sobre el cifrado de datos en reposo.

Si ejecuta Integration Runtime en una máquina virtual de Azure, los discos virtuales de Windows Virtual Machines (VM) se cifran en reposo mediante el cifrado del lado servidor o el cifrado de discos de Azure (ADE). Azure Disk Encryption aprovecha la característica BitLocker de Windows para cifrar los discos administrados con claves administradas por el cliente dentro de la máquina virtual invitada. El cifrado del lado servidor con claves administradas por el cliente mejora en ADE al permitir el uso de cualquier tipo de sistema operativo y de imágenes para las máquinas virtuales mediante el cifrado de datos en el servicio Storage.

Puede almacenar credenciales o valores de secreto en una instancia de Azure Key Vault y usarlos durante la ejecución de la canalización para pasarlos a las actividades. También puede almacenar las credenciales de los almacenes de datos y los procesos en una instancia de Azure Key Vault. Azure Data Factory las recupera al ejecutar una actividad que usa el almacén de datos o el proceso.

- [Descripción del cifrado en reposo en Azure Data Factory](data-movement-security-considerations.md)

- [Cifrado del lado servidor de Azure Managed Disks](../virtual-machines/disk-encryption.md)

- [Azure Disk Encryption para máquinas virtuales Windows](../virtual-machines/windows/disk-encryption-overview.md)

- [Uso de secretos de Azure Key Vault en actividades de canalización](how-to-use-azure-key-vault-secrets-pipeline-activities.md) 

- [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Instrucciones**: Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en Azure Data Factory y los recursos relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/alerts/alerts-activity-log.md)

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/alerts/alerts-activity-log.md)

- [Registro de Azure Storage Analytics](../storage/common/storage-analytics-logging.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Si usa Azure SQL Database como almacén de datos, habilite Advanced Data Security para Azure SQL Database y siga las recomendaciones de Azure Security Center sobre cómo realizar evaluaciones de vulnerabilidades en las instancias de Azure SQL Server.

Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, siga las recomendaciones de Azure Security Center sobre cómo realizar evaluaciones de vulnerabilidades en las máquinas virtuales.  Use las recomendaciones de Azure Security o una solución de terceros para realizar evaluaciones de vulnerabilidades para las máquinas virtuales. 

- [Cómo ejecutar valoraciones de vulnerabilidad en Azure SQL Database](../azure-sql/database/sql-vulnerability-assessment.md)

- [Habilitación de Advanced Data Security](../azure-sql/database/azure-defender-for-sql.md)

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Instrucciones**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, use la solución Azure Update Management para administrar las actualizaciones y revisiones de las máquinas virtuales.  Update Management se basa en el repositorio de actualización configurado localmente para aplicar revisiones a sistemas de Windows compatibles. Herramientas como System Center Updates Publisher (Updates Publisher) le permiten publicar actualizaciones personalizadas en Windows Server Update Services (WSUS). Este escenario permite que Update Management aplique revisiones a las máquinas que usan Configuration Manager como repositorio de actualizaciones con software de terceros.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Solución Update Management de Azure](../automation/update-management/overview.md)

- [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](../automation/update-management/manage-updates-for-vm.md)

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Guía**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, puede usar una solución de administración de revisiones de terceros.  Puede usar la solución Azure Update Management para administrar las actualizaciones y las revisiones de las máquinas virtuales.  Update Management se basa en el repositorio de actualización configurado localmente para aplicar revisiones a sistemas de Windows compatibles. Herramientas como System Center Updates Publisher (Updates Publisher) le permiten publicar actualizaciones personalizadas en Windows Server Update Services (WSUS). Este escenario permite que Update Management aplique revisiones a las máquinas que usan Configuration Manager como repositorio de actualizaciones con software de terceros. 

- [Solución Update Management de Azure](../automation/update-management/overview.md)

- [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](../automation/update-management/manage-updates-for-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: Si ejecuta Integration Runtime en una máquina virtual de Azure, exporte los resultados de análisis a intervalos coherentes y compare los resultados para comprobar que se han corregido las vulnerabilidades. Al usar una recomendación de administración de vulnerabilidades sugerida por Azure Security Center, puede ir al portal de la solución seleccionada para ver los datos de análisis históricos.

- [Descripción del escáner de vulnerabilidades integrado para máquinas virtuales](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Guía**: Si ejecuta Integration Runtime en una máquina virtual de Azure, puede usar el examen de vulnerabilidades nativo. El detector de vulnerabilidades incluido con Azure Security Center cuenta con la tecnología de Qualys. El detector de Qualys es la herramienta líder para la identificación en tiempo real de las vulnerabilidades en Virtual Machines de Azure.

Cuando Security Center identifica vulnerabilidades, presenta los resultados y la información relacionada como recomendaciones. La información relacionada incluye pasos para la corrección, CVE relacionados, puntuaciones de CVSS y mucho más. Puede ver las vulnerabilidades identificadas para una o varias suscripciones, o para una máquina virtual específica.

- [Escáner de vulnerabilidades integrado para máquinas virtuales](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) dentro de las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

Además, use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Defina los recursos de Azure y el software aprobados para los recursos de proceso.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones. 

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, aproveche el inventario de máquinas virtuales de Azure para automatizar la recopilación de información sobre todo el software en Virtual Machines. Azure Automation proporciona un control completo durante la implementación, las operaciones y la retirada de las cargas de trabajo y recursos.

Nota: El nombre del software, la versión, el publicador y la hora de actualización están disponibles en Azure Portal. Para obtener acceso a la fecha de instalación y otra información, se requiere que el cliente habilite los diagnósticos de nivel de invitado y transfiera los registros de eventos de Windows a un área de trabajo de Log Analytics.

- [Introducción a Azure Automation](../automation/automation-intro.md)

- [Habilitación del inventario de máquinas virtuales de Azure](../automation/automation-tutorial-installed-software.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Instrucciones**: Si ejecuta Integration Runtime en una máquina virtual de Azure, Azure Automation proporciona un control completo durante la implementación, las operaciones y la retirada de las cargas de trabajo y recursos.  Puede usar Change Tracking para identificar todo el software instalado en Virtual Machines. Puede implementar su propio proceso o usar State Configuration de Azure Automation para quitar software no autorizado.

- [Introducción a Azure Automation](../automation/automation-intro.md)

- [Seguimiento de cambios en el entorno con la solución Change Tracking](../automation/change-tracking/overview.md)

- [Introducción a State Configuration de Azure Automation](../automation/automation-dsc-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Instrucciones**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, use los controles de aplicaciones adaptables de Azure Security Center para asegurarse de que solo se ejecute software autorizado y de que se bloquee todo el software no autorizado en las máquinas virtuales.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/index.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Instrucciones**: El control de aplicaciones adaptables es una solución de un extremo a otro inteligente y automatizada de Azure Security Center que ayuda a controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales de Azure y que no son de Azure (Windows y Linux).  Implemente una solución de terceros si esto no cumple los requisitos de la organización.

Tenga en cuenta que esto solo se aplica si Integration Runtime se ejecuta en una máquina virtual de Azure.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Configure el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando "Bloquear acceso" en la aplicación Microsoft Azure Management.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Limitación de capacidad de los usuarios para ejecutar scripts en recursos de proceso

**Guía**: Si ejecuta Runtime Integration en una máquina virtual de Azure, según el tipo de scripts, puede usar configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en los recursos de proceso de Azure. También puede usar los controles de aplicación adaptables de Azure Security Center para asegurarse de que solo se ejecute software autorizado y de que todo el software no autorizado no se ejecute en Azure Virtual Machines.

- [Control de la ejecución de scripts de PowerShell en entornos Windows](/powershell/module/microsoft.powershell.security/set-executionpolicy)

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Instrucciones**: Las aplicaciones de alto riesgo implementadas en el entorno de Azure se pueden aislar mediante una red virtual, subred, suscripciones, grupos de administración, etc. y proteger suficientemente con una instancia de Azure Firewall, un firewall de aplicaciones web (WAF) o un grupo de seguridad de red (NSG). 

- [Redes virtuales y máquinas virtuales en Azure](../virtual-machines/network-overview.md)

- [¿Qué es Azure Firewall?](../firewall/overview.md)

- [¿Qué es el firewall de aplicaciones web de Azure?](../web-application-firewall/overview.md)

- [Grupos de seguridad de red](../virtual-network/network-security-groups-overview.md)

- [¿Qué es Azure Virtual Network?](../virtual-network/virtual-networks-overview.md)

- [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md)

- [Guía de decisiones de suscripción](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Defina e implemente configuraciones de seguridad estándar para Azure Data Factory con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.DataFactory" para crear directivas personalizadas a fin de auditar o aplicar la configuración de las instancias de Azure Data Factory.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía**: Si ejecuta Runtime Integration en una máquina virtual de Azure, use la recomendación de Azure Security Center [Corrección de vulnerabilidades en las configuraciones de seguridad de las máquinas virtuales] para mantener las configuraciones de seguridad en todos los recursos de proceso.

- [Cómo supervisar las recomendaciones de Azure Security Center](../security-center/security-center-recommendations.md)

- [Cómo corregir las recomendaciones de Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use la directiva de Azure Policy [deny] y [deploy if not exist] para aplicar la configuración segura en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Información sobre la creación de plantillas de Azure Resource Manager](../virtual-machines/windows/ps-template.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, tenga en cuenta que hay varias opciones para mantener una configuración segura de las máquinas virtuales para la implementación:
- Plantillas de Azure Resource Manager: Son archivos basados en JSON que se usan para implementar una máquina virtual desde Azure Portal. Las plantillas personalizadas deberán mantenerse. Microsoft realiza el mantenimiento en las plantillas base.
- Disco duro virtual (VHD) personalizado: En algunas circunstancias, puede ser necesario usar archivos VHD personalizados, como cuando se trabaja con entornos complejos que no se pueden administrar a través de otros medios. 
- State Configuration de Azure Automation: Una vez implementado el sistema operativo base, se puede usar para un control más pormenorizado de la configuración y para aplicarla a través del marco de automatización.

En la mayoría de los escenarios, las plantillas de máquina virtual base de Microsoft combinadas con Desired State Configuration de Azure Automation pueden ayudar a cumplir y mantener los requisitos de seguridad.

- [Información sobre cómo descargar la plantilla de máquina virtual](/previous-versions/azure/virtual-machines/windows/download-template)

- [Información sobre la creación de plantillas de Azure Resource Manager](../virtual-machines/windows/ps-template.md)

- [Carga de un VHD de máquina virtual personalizado en Azure](../virtual-machines/windows/upload-generalized-managed.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Si usa definiciones de personalizadas de Azure Policy, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Documentación de Azure Repos](/azure/devops/repos/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía**: Si usa imágenes personalizadas, use el control de acceso basado en rol de Azure (Azure RBAC) para asegurarse de que solo los usuarios autorizados pueden acceder a las imágenes. En el caso de las imágenes de contenedor, almacénelas en Azure Container Registry y aproveche Azure RBAC para asegurarse de que solo los usuarios autorizados puedan acceder a las imágenes.

Se puede usar el rol de colaborador de Data Factory para crear y administrar factorías de datos, así como los recursos secundarios que contienen.

- [Descripción de Azure RBAC](../role-based-access-control/rbac-and-directory-admin-roles.md) 

- [Descripción de Azure RBAC para Container Registry](../container-registry/container-registry-roles.md) 

- [Configuración de Azure RBAC](../role-based-access-control/quickstart-assign-role-user-portal.md)

- [Roles y permisos para Azure Data Factory](concepts-roles-permissions.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: Use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.DataFactory" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Guía**: Esta recomendación se puede aplicar si Integration Runtime se ejecuta en una máquina virtual de Azure. Azure Automation State Configuration es un servicio de administración de configuración para los nodos de Desired State Configuration (DSC) de cualquier centro de datos local o en la nube. Permite la escalabilidad en miles de máquinas de forma rápida y sencilla desde una ubicación central y segura. Puede incorporar fácilmente máquinas, asignarles configuraciones declarativas y ver informes que muestran el cumplimiento de cada máquina con el estado deseado que especifique. 

- [Incorporación de máquinas para su administración mediante Azure Automation State Configuration](../automation/automation-dsc-onboarding.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Instrucciones**: Use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.DataFactory" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Use la directiva de Azure Policy [audit], [deny] y [deploy if not exist] para aplicar automáticamente las configuraciones en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Guía**: Esta recomendación se puede aplicar si Integration Runtime se ejecuta en una máquina virtual de Azure. Azure Automation State Configuration es un servicio de administración de configuración para los nodos de Desired State Configuration (DSC) de cualquier centro de datos local o en la nube. Permite la escalabilidad en miles de máquinas de forma rápida y sencilla desde una ubicación central y segura. Puede incorporar fácilmente máquinas, asignarles configuraciones declarativas y ver informes que muestran el cumplimiento de cada máquina con el estado deseado que especifique. 

- [Incorporación de máquinas para su administración mediante Azure Automation State Configuration](../automation/automation-dsc-onboarding.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Instrucciones**: Use Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de secretos para las aplicaciones en la nube.

También puede almacenar credenciales o valores de secreto en una instancia de Azure Key Vault y usarlos durante la ejecución de la canalización para pasarlos a las actividades. Asegúrese de que la eliminación temporal esté habilitada.

- [Integración con identidades administradas de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Creación de un almacén de claves](../key-vault/secrets/quick-create-portal.md)

- [Autenticación en Azure Key Vault](../key-vault/general/authentication.md)

- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

- [Uso de secretos de Azure Key Vault en actividades de canalización](how-to-use-azure-key-vault-secrets-pipeline-activities.md)

- [Eliminación temporal de Azure Key Vault](../key-vault/general/soft-delete-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: Al crear una factoría de datos, se puede crear también una identidad administrada. La identidad administrada es una aplicación administrada registrada en Azure Active Directory (Azure AD) que representa a esta factoría de datos específica.

- [Identidad administrada de Azure Data Factory](data-factory-service-identity.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía**: Si ejecuta Integration Runtime en una máquina virtual de Azure, puede usar Microsoft Antimalware para máquinas virtuales de Windows Azure a fin de supervisar y defender continuamente sus recursos. 

- [Configuración de Microsoft Antimalware para Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure App Service), pero no se ejecuta en el contenido.

Examine previamente los archivos que se cargan en recursos de Azure que no son de proceso, como App Service, Data Lake Storage, Blob Storage, etc. 

Use la detección de amenazas de Azure Security Center para los servicios de datos para detectar malware cargado en cuentas de almacenamiento. 

- [Descripción de Microsoft Antimalware para Azure Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

- [Descripción de la detección de amenazas de Azure Security Center para servicios de datos](../security-center/azure-defender.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Asegúrese de que se han actualizado el software y las firmas antimalware

**Guía**: Cuando se implementa, Microsoft Antimalware para Azure instalará automáticamente y de forma predeterminada las actualizaciones más recientes de firma, plataforma y motor. Siga las recomendaciones en Azure Security Center: "Proceso y aplicaciones" para asegurarse de que todos los puntos de conexión estén actualizados con las firmas más recientes. El sistema operativo Windows se puede proteger aún más con seguridad adicional para limitar el riesgo de ataques basados en malware y virus con el servicio Microsoft Defender Advanced Threat Protection que se integra con Azure Security Center.

- [Implementación de Microsoft Antimalware para Azure Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md)

- [Protección contra amenazas avanzada de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/onboard-configure)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Instrucciones**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, habilite Azure Backup y configure la máquina virtual, así como la frecuencia deseada y el período de retención para las copias de seguridad automáticas.

Para cualquiera de los almacenes de datos, consulte la base de referencia de seguridad de ese servicio para obtener recomendaciones sobre cómo realizar copias de seguridad periódicas y automatizadas.

- [Información general sobre la copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-vms-introduction.md)

- [Copia de seguridad de una máquina virtual de Azure desde la configuración de esta](../backup/backup-azure-vms-first-look-arm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copias de seguridad de las claves administradas por el cliente

**Guía**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure, habilite Azure Backup y las máquinas virtuales de Azure de destino, así como la frecuencia deseada y los períodos de retención. Realice una copia de seguridad de las claves administradas por el cliente en Azure Key Vault.

Para cualquiera de los almacenes de datos, consulte la base de referencia de seguridad de ese servicio para obtener recomendaciones sobre cómo realizar copias de seguridad periódicas y automatizadas.

- [Información general sobre la copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-vms-introduction.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Instrucciones**: Si ejecuta Integration Runtime en una máquina virtual de Azure, asegúrese de que existe la capacidad de realizar la restauración periódica de los datos de contenido en Azure Backup. Si es necesario, pruebe la restauración del contenido en una VLAN aislada. Pruebe periódicamente la restauración de la copia de seguridad de las claves administradas por el cliente.

Para cualquiera de los almacenes de datos, consulte la base de referencia de seguridad de ese servicio para obtener instrucciones sobre la validación de copias de seguridad.

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](../backup/backup-azure-restore-files-from-vm.md)

- [Restauración de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Si ejecuta Integration Runtime en una máquina virtual (VM) de Azure y hace una copia de seguridad de la máquina virtual con Azure Backup, la máquina virtual se cifra en reposo con Storage Service Encryption (SSE). Azure Backup también puede hacer una copia de seguridad de las VM de Azure cifradas mediante Azure Disk Encryption (ADE). Azure Disk Encryption se integra con las claves de cifrado de BitLocker (BEK), que se protegen en un almacén de claves como secretos. Azure Disk Encryption también se integra con las claves de cifrado de las claves de Azure Key Vault (KEK). Habilite la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

- [Eliminación temporal para máquinas virtuales](../backup/backup-azure-security-feature-cloud.md)

- [Información general sobre la eliminación temporal de Azure Key Vault](../key-vault/general/soft-delete-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Guía**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente.  Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Instrucciones**: use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft. 

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Introducción a Azure Security Benchmark V2](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).