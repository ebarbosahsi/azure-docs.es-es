---
title: Base de referencia de seguridad de Azure Machine Learning
description: La base de referencia de seguridad de Azure Machine Learning proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: machine-learning
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: f8bde45cfdf9cf1c9d50faee76161461d8b0aa0a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104607836"
---
# <a name="azure-security-baseline-for-azure-machine-learning"></a>Base de referencia de seguridad de Azure Machine Learning

Esta base de referencia de seguridad aplica la guía de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md) a Microsoft Azure Machine Learning. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices aplicables a Azure Machine Learning. Se han excluido los **controles** no aplicables a Azure Machine Learning.

 
Para ver cómo Azure Machine Learning se adapta por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Azure Machine Learning completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Azure Machine Learning depende de otros servicios de Azure para los recursos de proceso. Los recursos de proceso (destinos de proceso) se usan para entrenar e implementar modelos. Puede crear estos destinos de proceso en una red virtual. Por ejemplo, puede usar la instancia de proceso de Azure Virtual Machine Learning para entrenar un modelo y, después, implementarlo en Azure Kubernetes Service (AKS). Puede proteger los ciclos de vida de aprendizaje automático mediante el aislamiento de los trabajos de entrenamiento e inferencia de Azure Machine Learning dentro de una red virtual de Azure.

Azure Firewall puede usarse para controlar el acceso al área de trabajo de Azure Machine Learning y a la red pública de Internet.

- [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md)

- [Uso de áreas de trabajo detrás de Azure Firewall para Azure Machine Learning](how-to-access-azureml-behind-firewall.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

**Instrucciones**: Azure Machine Learning depende de otros servicios de Azure para los recursos de proceso. Asigne grupos de seguridad de red a las redes que se crean como la implementación de Machine Learning. 

Habilite los registros de flujo de los grupos de seguridad de red y envíe los registros a una cuenta de Azure Storage para su auditoría. También puede enviar los registros de flujo a un área de trabajo de Log Analytics y luego usar Análisis de tráfico para proporcionar información detallada sobre los patrones de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red, identificar las zonas activas y las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía**: Puede habilitar HTTPS para proteger la comunicación con los servicios web que implementa Azure Machine Learning. Los servicios web se implementan en Azure Kubernetes Services (AKS) o en Azure Container Instances (ACI) y protegen los datos enviados por los clientes. También puede usar la dirección IP privada con AKS para restringir la puntuación, de modo que solo los clientes detrás de una red virtual puedan tener acceso al servicio web.

- [Uso de TLS para proteger un servicio web con Azure Machine Learning](how-to-secure-web-service.md)

- [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: Habilite el estándar de DDoS Protection en las redes virtuales asociadas a la instancia de Machine Learning para protegerse de los ataques por denegación de servicio distribuidos (DDoS). Use la detección de amenazas integrada de Azure Security Center para detectar las comunicaciones con direcciones IP de Internet malintencionadas o no utilizadas.

Implemente Azure Firewall en cada uno de los límites de red de la organización con el filtrado basado en inteligencia de amenazas habilitado y configurado para alertar y denegar el tráfico de red malintencionado.

- [Configuración de la protección contra DDoS](../ddos-protection/manage-ddos-protection.md)

- [Uso de áreas de trabajo detrás de Azure Firewall para Azure Machine Learning](how-to-access-azureml-behind-firewall.md)

- [Para más información sobre la detección de amenazas de Azure Security Center](../security-center/azure-defender.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: En el caso de las máquinas virtuales con la extensión adecuada instalada en los servicios de Azure Machine Learning, puede habilitar la captura de paquetes de Network Watcher para investigar actividades anómalas. 

- [Creación de una instancia de Network Watcher](../network-watcher/network-watcher-create.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: Implemente la solución de firewall que prefiera en cada uno de los límites de red de su organización para detectar o bloquear el tráfico malintencionado.

Seleccione en Azure Marketplace una oferta que admita la funcionalidad de IDS/IPS con funcionalidades de inspección de carga.  Cuando la inspección de carga no sea un requisito, se puede usar la inteligencia sobre amenazas de Azure Firewall. El filtrado basado en inteligencia sobre amenazas de Azure Firewall se usa para alertar o bloquear el tráfico desde y hacia dominios y direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas con Azure Firewall](../firewall/threat-intel.md)

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Instrucciones**: No aplicable; esta recomendación está pensada para las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: En el caso de los recursos que necesitan acceso a la cuenta de Azure Machine Learning, use las etiquetas de servicio de Virtual Network con el fin de definir controles de acceso a la red en grupos de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, AzureMachineLearning) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

Azure Machine Learning Service documenta una lista de etiquetas de servicio para sus destinos de proceso dentro de una red virtual que ayuda a minimizar la complejidad y que puede usar como directrices en la administración de la red.

- [Más información sobre el uso de etiquetas de servicio](../virtual-network/service-tags-overview.md)

- [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para los recursos de red asociados a los espacios de nombres de Azure Machine Learning con Azure Policy. Use alias de Azure Policy en los espacios de nombres "Microsoft.MachineLearning" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los espacios de nombres de Machine Learning. 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: Use etiquetas para los recursos de red asociados a la implementación de Azure Machine Learning a fin de organizarlos de manera lógica según una taxonomía.

En el caso de un recurso de la red virtual de Azure Machine Learning que admita el campo Descripción, úselo para documentar las reglas que permiten el tráfico hacia y desde una red.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: Use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con Azure Machine Learning. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Creación de alertas en Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Ingiera registros a través de Azure Monitor para agregar datos de seguridad generados por Azure Machine Learning. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Configuración de registros de diagnóstico para Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/monitor-azure-machine-learning#configuration)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Instrucciones**: Habilite Configuración de diagnóstico en los recursos de Azure para poder acceder a los registros de auditoría, seguridad y diagnóstico. Los registros de actividad, que están disponibles automáticamente, incluyen el origen del evento, la fecha, el usuario, la marca de tiempo, las direcciones de origen y de destino, y otros elementos útiles.

También puede poner en correlación los registros de operaciones de Machine Learning Service con fines de seguridad y cumplimiento.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings)

- [Descripción del registro y de los distintos tipos de registro de Azure](/azure/azure-monitor/platform/platform-logs-overview)

- [Habilitación del registro en Azure Machine Learning](how-to-track-experiments.md)

- [Supervisión de Azure Machine Learning](monitor-azure-machine-learning.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Instrucciones**: Si el recurso de proceso es propiedad de Microsoft, este será responsable de su recolección y supervisión. 

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. Para cualquier recurso de proceso que sea propiedad de su organización, use Azure Security Center para supervisar el sistema operativo. 

- [Recopilación de registros de host internos de máquina virtual de Azure con Azure Monitor](/azure/azure-monitor/learn/quick-collect-azurevm)

- [Descripción de la recopilación de datos de Azure Security Center](../security-center/security-center-enable-data-collection.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Instrucciones**: En Azure Monitor, establezca el período de retención de registro para las áreas de trabajo de Log Analytics asociadas a las instancias de Azure Machine Learning según las normativas de cumplimiento de su organización.

- [Establecimiento de parámetros de retención de registros](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía**: Analice y supervise los registros en busca de comportamientos anómalos y revise periódicamente los resultados de su instancia de Azure Machine Learning. Use Azure Monitor y un área de trabajo de Log Analytics para revisar los registros y realizar consultas en los datos de dichos registros.

También puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros. 

- [Realización de consultas para Azure Machine Learning en áreas de trabajo de Log Analytics](https://docs.microsoft.com/azure/machine-learning/monitor-azure-machine-learning#analyzing-log-data)

- [Habilitación del registro en Azure Machine Learning](how-to-track-experiments.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Introducción a las consultas de Log Analytics](../azure-monitor/logs/log-analytics-tutorial.md)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Instrucciones**: En Azure Monitor, configure los registros relacionados con Azure Machine Learning en el registro de actividad y la configuración de diagnóstico de Machine Learning para enviar los registros a un área de trabajo de Log Analytics que se va a consultar o a una cuenta de almacenamiento para el almacenamiento de archivo a largo plazo. Use el área de trabajo de Log Analytics para crear alertas para actividades anómalas que se encuentren en registros y eventos de seguridad.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel.

- [Para más información sobre las alertas de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/monitor-azure-machine-learning#alerts)

- [Alertas sobre datos de registro del área de trabajo de Log Analytics](/azure/azure-monitor/learn/tutorial-response)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Instrucciones**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de la implementación de Antimalware de Azure Machine Learning Service.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, habilite la recopilación de eventos antimalware de Microsoft Antimalware para Azure Cloud Services y Virtual Machines.

- [Configuración de la extensión de Microsoft Antimalware para Cloud Services](/powershell/module/servicemanagement/azure.service/set-azurevmmicrosoftantimalwareextension)

- [Descripción de Microsoft Antimalware](../security/fundamentals/antimalware.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía**: No aplicable: Azure Machine Learning no procesa ni genera registros relacionados con DNS.

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso propiedad de su organización, use Azure Security Center para habilitar la supervisión del registro de eventos de seguridad para las máquinas virtuales de Azure. Azure Security Center aprovisiona el agente de Log Analytics en todas las máquinas virtuales de Azure compatibles y en las nuevas que se crean si el aprovisionamiento automático está habilitado. O bien puede instalar el agente manualmente. El agente habilita el evento 4688 de creación de procesos y el campo commandline dentro del evento 4688. El registro de eventos registra los nuevos procesos creados en la máquina virtual y los servicios de detección de Security Center los supervisan.

- [Recopilación de datos en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="identity-and-access-control"></a>Identidad y Access Control

*Para más información, consulte [Azure Security Benchmark: identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Puede usar la pestaña Administración de identidades y acceso de un recurso en Azure Portal para configurar el control de acceso basado en roles de Azure (Azure RBAC) y mantener el inventario en los recursos de Azure Machine Learning. Los roles se aplican a usuarios, grupos, entidades de servicio e identidades administradas en Active Directory. Puede usar roles integrados o personalizados para usuarios y grupos.

Azure Machine Learning proporciona roles integrados para escenarios de administración comunes en Azure Machine Learning. Un usuario que tiene un perfil en Azure Active Directory (Azure AD) puede asignar estos roles a usuarios, grupos, entidades de servicio o identidades administradas para conceder o denegar el acceso a recursos y operaciones en los recursos de Azure Machine Learning.

Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

- [Información sobre el control de acceso basado en roles de Azure en Azure Machine Learning](how-to-assign-roles.md)

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: La administración del acceso a los recursos de Machine Learning se controla a través de Azure Active Directory (Azure AD). Azure AD no tiene el concepto de contraseñas predeterminadas.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Azure Machine Learning incluye tres roles predeterminados cuando se crea un área de trabajo, que crean procedimientos operativos estándar en torno al uso de las cuentas de propietario.

También puede habilitar el acceso Just-in-Time a cuentas administrativas mediante Azure Active Directory (Azure AD) Privileged Identity Management y Azure Resource Manager.

- [Para más información sobre los roles predeterminados de Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-assign-roles#default-roles)

- [Más información acerca de Privileged Identity Management](/azure/active-directory/privileged-identity-management/index)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Guía**: Machine Learning se integra con Azure Active Directory (Azure AD). Use el SSO de Azure AD en lugar de configurar credenciales independientes individuales por servicio. Use las recomendaciones de identidades y acceso de Azure Security Center.

- [Descripción del SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: Habilite la autenticación multifactor de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

- [Habilitación de la autenticación multifactor en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Instrucciones**: use una estación de trabajo segura y administrada por Azure (también conocida como una estación de trabajo de acceso con privilegios o PAW) para las tareas administrativas que requieren privilegios elevados.

- [Descripción de las estaciones de trabajo seguras administradas por Azure](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Habilitación de la autenticación multifactor de Azure Active Directory (Azure AD)](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: use los informes de seguridad y la supervisión de Azure Active Directory (Azure AD) para detectar cuándo se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8: Administración de los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: use ubicaciones con nombre de Azure Active Directory (Azure AD) para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre de Azure AD](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.
 
El acceso de los roles puede tener un ámbito de varios niveles en Azure. En el caso de Machine Learning, los roles se pueden administrar en el nivel de área de trabajo; por ejemplo, si tiene acceso de propietario a un área de trabajo, es posible que no tenga acceso de propietario al grupo de recursos que contiene el área de trabajo. Esto proporciona controles de acceso más pormenorizados para separar roles dentro del mismo grupo de recursos. 

- [Administración del acceso a un área de trabajo de Azure Machine Learning](how-to-assign-roles.md) 
 
- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Instrucciones**: Azure Active Directory (Azure AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso e identidades de Azure AD para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

use Azure AD Privileged Identity Management (PIM) para la generación de registros y alertas cuando se produzca actividad sospechosa o insegura en el entorno.

- [Descripción de los informes de Azure AD](/azure/active-directory/reports-monitoring)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

- [Implementación de Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: tiene acceso a los orígenes de registro de eventos de riesgo, auditoría y actividad de inicio de sesión de Azure Active Directory (Azure AD), que permiten la integración en cualquier herramienta SIEM o de supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Instrucciones**: use las características de protección de identidad de Azure Active Directory (Azure AD) para configurar respuestas automatizadas a las acciones sospechosas que se detecten relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía**: No aplicable: Azure Machine Learning Service no es compatible con la caja de seguridad del cliente.

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.
 
- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Guía**: Implemente el aislamiento mediante suscripciones independientes y grupos de administración para dominios de seguridad individuales, como el tipo de entorno y el nivel de confidencialidad de los datos. Puede restringir el nivel de acceso a los recursos de Azure que necesitan sus aplicaciones y entornos empresariales. Puede controlar el acceso a los recursos de Azure a través de RBAC de Azure.
 
- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

 
- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: Use una solución de terceros de Azure Marketplace en los perímetros de la red para supervisar la transferencia no autorizada de información confidencial y bloquear dichas transferencias al tiempo que alerta a los profesionales de seguridad de la información. 

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y protege a los clientes contra la pérdida y exposición de los datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos. 

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía**: Los servicios web implementados a través de Azure Machine Learning solo admiten la versión 1.2 de TLS que aplica el cifrado de datos en tránsito.

- [Uso de TLS para proteger un servicio web con Azure Machine Learning](how-to-secure-web-service.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: Las características de identificación, clasificación y prevención de pérdida de datos no están disponibles aún en Azure Machine Learning. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Uso de RBAC de Azure para administrar el acceso a los recursos

**Guía**: Azure Machine Learning admite el uso de Azure Active Directory (Azure AD) para autorizar las solicitudes a los recursos de Machine Learning. Con Azure AD, puede usar el control de acceso basado en roles (RBAC) de Azure para conceder permisos a una entidad de seguridad, que puede ser un usuario o una entidad de servicio de aplicación.

- [Administración del acceso a un área de trabajo de Azure Machine Learning](how-to-assign-roles.md)

- [Uso de Azure RBAC para la autorización de Kubernetes](../aks/manage-azure-rbac.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Azure Machine Learning almacena instantáneas, salidas y registros en la cuenta de Azure Blob Storage vinculada al área de trabajo de Azure Machine Learning y la suscripción. Todos los datos almacenados en Azure Blob Storage se cifran en reposo con claves administradas por Microsoft. También puede cifrar los datos almacenados en Azure Blob Storage con sus propias claves en Machine Learning Service. 

- [Cifrado de datos de Azure Machine Learning en reposo](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#encryption-at-rest)

- [Descripción del cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md)

- [Configuración de las claves de cifrado administradas por el cliente](../storage/common/customer-managed-keys-configure-key-vault.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Instrucciones**: Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en las instancias de producción de Azure Machine Learning y otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de la administración de vulnerabilidades de Azure Machine Learning Service. 

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, siga las recomendaciones de Azure Security Center para realizar evaluaciones de vulnerabilidades en las máquinas virtuales de Azure, las imágenes de contenedor y los servidores SQL°Server.

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Guía**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de la administración de revisiones de Azure Machine Learning Service. 

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use Update Management de Azure Automation para asegurarse de que las actualizaciones de seguridad más recientes estén instaladas en las máquinas virtuales Windows y Linux. En el caso de las máquinas virtuales con Windows, asegúrese de que Windows Update se ha habilitado y configurado para actualizarse automáticamente.

- [Configuración de Update Management para máquinas virtuales en Azure](../automation/update-management/overview.md)

- [Descripción de las directivas de seguridad de Azure supervisadas por Security Center](../security-center/policy-reference.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="53-deploy-an-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de recursos de proceso que son propiedad de su organización, use una solución de administración de revisiones de terceros. Los clientes que ya usan Configuration Manager en su entorno también pueden usar System Center Updates Publisher, lo que les permite publicar actualizaciones personalizadas en Windows Server Update Service. Esto permite que Update Management revise las máquinas que usan Configuration Manager como repositorio de actualizaciones con software de terceros.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, siga las recomendaciones de Azure Security Center para realizar evaluaciones de vulnerabilidades en las máquinas virtuales de Azure, las imágenes de contenedor y los servidores SQL°Server. Exporte los resultados de análisis en intervalos coherentes y compare los resultados con análisis anteriores para comprobar que se han corregido las vulnerabilidades. Al usar recomendaciones de administración de vulnerabilidades sugeridas por Azure Security Center, puede dinamizar en el portal de la solución seleccionada para ver los datos de análisis históricos.

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Guía**: No aplicable; esta guía está pensada para recursos de proceso.

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

## <a name="inventory-and-asset-management"></a>Administración de recursos y del inventario

*Para más información, consulte [Azure Security Benchmark: inventario y administración de recursos](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar y detectar los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) en las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos en las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Azure Resource Graph Explorer, se recomienda crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía**: Aplique etiquetas a los recursos de Azure, lo que proporciona metadatos para organizarlos de forma lógica según una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [ Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [ Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)
 
- [ Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

* Tipos de recursos no permitidos
* Tipos de recursos permitidos

Además, use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use Change Tracking e Inventario de Azure Automation para automatizar la recopilación de información de inventario de las máquinas virtuales Windows y Linux. El nombre del software, la versión, el publicador y la hora de actualización están disponibles en Azure Portal. Para obtener la fecha de instalación de software y otro tipo de información, habilite los diagnósticos de nivel de invitado y dirija los registros de eventos de Windows al área de trabajo de Log Analytics.

- [Habilitación de la recopilación de inventario de Azure Automation para una máquina virtual](../automation/automation-tutorial-installed-software.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Instrucciones**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use la supervisión de la integridad de los archivos (FIM) de Azure Security Center para identificar todo el software instalado en las máquinas virtuales. Otra opción que se puede usar en lugar de FIM o junto con ella es Change Tracking e Inventario de Azure Automation para recopilar el inventario de las máquinas virtuales Linux y Windows. 

Puede implementar su propio proceso para quitar software no autorizado. También puede usar una solución de terceros para identificar software no aprobado.

Quite los recursos de Azure cuando ya no los necesite.

- [Habilitación de la supervisión de la integridad de los archivos](../security-center/security-center-file-integrity-monitoring.md)

- [Información general de Change Tracking e Inventario en Azure Automation](../automation/change-tracking/overview.md)

- [Habilitación del inventario de máquinas virtuales de Azure](../automation/automation-tutorial-installed-software.md)

- [Eliminación de grupos de recursos y recursos de Azure](../azure-resource-manager/management/delete-resource-group.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos que son propiedad de su organización, use los controles de aplicaciones adaptables de Azure Security Center para garantizar que solo se ejecute software autorizado y que se bloquee la ejecución de todo el software no autorizado en Azure Virtual Machines.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

* Tipos de recursos no permitidos
* Tipos de recursos permitidos

Además, use Azure Resource Graph para consultar y detectar recursos en las suscripciones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Instrucciones**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use los controles de aplicaciones adaptables de Azure Security Center para especificar a qué tipos de archivo puede aplicarse o no una regla.

Implemente una solución de terceros si los controles de aplicaciones adaptables no cumplen con el requisito.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: use el acceso condicional de Azure Active Directory (Azure AD) para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="612-limit-users-ability-to-execute-scripts-in-compute-resources"></a>6.12: Limitación de la capacidad de los usuarios para ejecutar scripts en los recursos de proceso

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, según el tipo de scripts, puede usar configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en los recursos de proceso de Azure. También puede usar los controles de aplicaciones adaptables de Azure Security Center para asegurarse de que solo se ejecute software autorizado y de que todo el software no autorizado no se ejecute en Azure Virtual Machines.

- [Control de la ejecución de scripts de PowerShell en entornos Windows](/powershell/module/microsoft.powershell.security/set-executionpolicy)

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../security-center/security-center-adaptive-application.md)

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Instrucciones**: No aplicable; esta recomendación está pensada para las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

## <a name="secure-configuration"></a>Configuración segura

*Para más información, consulte [Azure Security Benchmark: configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Defina e implemente configuraciones de seguridad estándar para Azure Machine Learning Service con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.MachineLearning" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los servicios de Azure Machine Learning.

Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON), que se debe revisar para asegurarse de que las configuraciones cumplan los requisitos de seguridad de la organización.

También puede usar las recomendaciones de Azure Security Center como línea de base de configuración segura para los recursos de Azure.

Azure Machine Learning es totalmente compatible con los repositorios de Git para hacer el seguimiento del trabajo; puede clonar repositorios directamente al sistema de archivos compartido de su área de trabajo, usar Git en la estación de trabajo local y asegurarse de que las configuraciones seguras se apliquen a los recursos de código como parte de su entorno de Machine Learning.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de las configuraciones seguras para el sistema operativo de Azure Machine Learning Service.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use las recomendaciones de Azure Security Center para mantener las configuraciones de seguridad en todos los recursos de proceso.  Además, puede usar imágenes de sistema operativo personalizado o la configuración de estado de Azure Automation para establecer la configuración de seguridad del sistema operativo que requiere su organización.

- [Cómo supervisar las recomendaciones de Azure Security Center](../security-center/security-center-recommendations.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

- [Introducción a State Configuration de Azure Automation](../automation/automation-dsc-overview.md)

- [Carga de un VHD y su uso para crear máquinas virtuales Windows nuevas en Azure](../virtual-machines/windows/upload-generalized-managed.md)

- [Creación de una VM Linux desde un disco personalizado con la CLI de Azure](../virtual-machines/linux/upload-vhd.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use la directiva de Azure Policy [deny] y [deploy if not exist] para aplicar la configuración segura en los recursos de Azure. Además, puede usar plantillas de Azure Resource Manager para mantener la configuración de seguridad de los recursos de Azure que requiere su organización. 
 
- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)
 
- [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)
 
- [ Información general sobre plantillas de Azure Resource Manager](../azure-resource-manager/templates/overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de las configuraciones seguras para el sistema operativo de Azure Machine Learning Service.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, siga las recomendaciones de Azure Security Center sobre cómo realizar evaluaciones de vulnerabilidades en los recursos de proceso de Azure.  Además, puede usar plantillas de Azure Resource Manager, imágenes de sistema operativo personalizado o State Configuration de Azure Automation para mantener la configuración de seguridad del sistema operativo que requiere su organización.   Las plantillas de máquina virtual de Microsoft combinadas con State Configuration de Azure Automation pueden ayudar a cumplir y mantener los requisitos de seguridad. 

Tenga en cuenta que las imágenes de máquinas virtuales de Azure Marketplace que publica Microsoft, las administra y mantiene Microsoft mismo. 

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

- [Creación de una máquina virtual de Azure a partir de una plantilla de Resource Manager](../virtual-machines/windows/ps-template.md)

- [Introducción a State Configuration de Azure Automation](../automation/automation-dsc-overview.md)

- [Creación de una máquina virtual de Windows en Azure Portal](../virtual-machines/windows/quick-create-portal.md)

- [Información sobre cómo descargar la plantilla de máquina virtual](/previous-versions/azure/virtual-machines/windows/download-template)

- [Script de ejemplo para cargar un disco duro virtual en Azure y crear una máquina virtual nueva](/previous-versions/azure/virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Si usa definiciones personalizadas de Azure Policy para los recursos de Machine Learning u otros relacionados, use Azure Repos para almacenar y administrar el código de forma segura.

Azure Machine Learning es totalmente compatible con los repositorios de Git para hacer el seguimiento del trabajo; puede clonar repositorios directamente al sistema de archivos compartido de su área de trabajo, usar Git en la estación de trabajo local y asegurarse de que las configuraciones seguras se apliquen a los recursos de código como parte de su entorno de Machine Learning.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Documentación de Azure Repos](/azure/devops/repos/)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía**: Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use el control de acceso basado en roles de Azure (Azure RBAC) para asegurarse de que solo los usuarios autorizados puedan acceder a las imágenes personalizadas. Use una instancia de Azure Shared Image Gallery para compartir imágenes con diferentes usuarios, entidades de servicio o grupos de Azure Active Directory (Azure AD) en su organización. Almacene imágenes de contenedor en Azure Container Registry y use Azure RBAC para garantizar que solo los usuarios autorizados puedan tener acceso.

- [Descripción de Azure RBAC](../role-based-access-control/rbac-and-directory-admin-roles.md)

- [Descripción de Azure RBAC para Container Registry](../container-registry/container-registry-roles.md)

- [Configuración de Azure RBAC](../role-based-access-control/quickstart-assign-role-user-portal.md)

- [Introducción a la galería de imágenes compartidas](../virtual-machines/shared-image-galleries.md)

- [Uso de Azure RBAC para la autorización de Kubernetes](../aks/manage-azure-rbac.md)

**Responsabilidad**: No aplicable

**Supervisión de Azure Security Center**: ninguna

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: Use alias de Azure Policy en el espacio de nombres "Microsoft.MachineLearning" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Uso de alias](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Guía**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de la implementación de la configuración segura de Azure Machine Learning Service.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use State Configuration de Azure Automation para los nodos de Desired State Configuration (DSC) en cualquier centro de datos local o en la nube. Puede incorporar fácilmente máquinas, asignarles configuraciones declarativas y ver informes que muestran el cumplimiento de cada máquina con el estado deseado que especifique. 

- [Habilitación de State Configuration de Azure Automation](../automation/automation-dsc-onboarding.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use Azure Security Center para realizar análisis de base de referencia para los recursos de Azure. Además, use Azure Policy para alertar y auditar las configuraciones de recursos de Azure.
 
 
 
- [ Corrección de recomendaciones en Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Guía**: Si el recurso de proceso es propiedad de Microsoft, Microsoft es responsable de la supervisión de la configuración segura automatizada de Azure Machine Learning Service.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use Proceso y aplicaciones de Azure Security Center y siga las recomendaciones para máquinas virtuales, servidores y contenedores.

- [Descripción de las recomendaciones sobre contenedores de Azure Security Center](../security-center/container-security.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Instrucciones**: Use identidades administradas con Azure Key Vault para simplificar la administración de secretos de las aplicaciones en la nube.

Azure Machine Learning admite el cifrado del almacén de datos con claves administradas por el cliente, debe administrar los requisitos de seguridad y cumplimiento normativo de rotación y revocación de claves por organización. 

Use Azure Key Vault para pasar secretos a las ejecuciones remotas de forma segura en lugar de texto no cifrado en los scripts de entrenamiento.

- [Claves administradas por el cliente de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-blob-storage)

- [Uso de secretos de credenciales de autenticación en ejecuciones de entrenamiento de Azure Machine Learning](how-to-use-secrets-in-runs.md)

- [Uso de identidades administradas para recursos de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Creación de un almacén de claves](../key-vault/general/quick-create-portal.md)

- [Autenticación en Azure Key Vault](../key-vault/general/authentication.md)

- [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-portal.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: Azure Machine Learning admite los roles integrados y la capacidad de crear roles personalizados. Use identidades administradas para proporcionar servicios de Azure con una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, como Key Vault, sin necesidad de credenciales en el código.

- [Administración del acceso a un área de trabajo de Azure Machine Learning](how-to-assign-roles.md)

- [Configuración de identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="malware-defense"></a>Defensa contra malware

*Para más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-antimalware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Machine Learning), aunque no se ejecuta en el contenido del cliente.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, use Microsoft Antimalware para Azure para supervisar y defender continuamente los recursos. Para Linux, use una solución antimalware de terceros. Use también la detección de amenazas de Azure Security Center para los servicios de datos para detectar malware cargado en cuentas de almacenamiento.

- [Configuración de Microsoft Antimalware para Azure](../security/fundamentals/antimalware.md)

- [Protección contra amenazas en Azure Security Center](../security-center/azure-defender.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Machine Learning), aunque no se ejecuta en el contenido del cliente.

Es su responsabilidad realizar un análisis previo del contenido que se carga en recursos de Azure que no son de proceso. Microsoft no puede acceder a los datos del cliente y, por lo tanto, no puede realizar exámenes antimalware del contenido del cliente en su nombre.

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="83-ensure-antimalware-software-and-signatures-are-updated"></a>8.3: Garantía de la actualización del software y las firmas antimalware

**Guía**: Microsoft Antimalware está habilitado y se conserva para el host subyacente que admite los servicios de Azure (por ejemplo, Azure Machine Learning), aunque no se ejecuta en el contenido del cliente.

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso e incluso en sus propios recursos de proceso. En el caso de los recursos de proceso que son propiedad de su organización, siga las recomendaciones de Proceso y aplicaciones de Azure Security Center para asegurarse de que todos los puntos de conexión estén actualizados con las firmas más recientes. Para Linux, use una solución antimalware de terceros.

- [Implementación de Microsoft Antimalware para Azure](../security/fundamentals/antimalware.md)

**Responsabilidad**: Compartido

**Supervisión de Azure Security Center**: ninguna

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: La administración de la recuperación de datos en Machine Learning Service se realiza a través de las administraciones de datos en almacenes de datos conectados. Asegúrese de seguir las directrices de recuperación de datos para almacenes conectados a fin de hacer una copia de seguridad de los datos según las directivas de la organización del cliente.

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](../backup/backup-azure-restore-files-from-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Instrucciones**: La copia de seguridad de datos en Machine Learning Service se realiza a través de las administraciones de datos en almacenes de datos conectados. Habilite Azure Backup para las máquinas virtuales y configure la frecuencia y los períodos de retención deseados. Haga una copia de seguridad de las claves administradas por el cliente en Azure Key Vault.

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](../backup/backup-azure-restore-files-from-vm.md)

- [Restauración de las claves de Key Vault en Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: La validación de la copia de seguridad de datos en Machine Learning Service se realiza a través de las administraciones de datos en almacenes de datos conectados. Realice periódicamente la restauración de datos de contenido en Azure Backup. Asegúrese de que puede restaurar las claves administradas por el cliente de las que se hizo una copia de seguridad.

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](../backup/backup-azure-restore-files-from-vm.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Instrucciones**: Para la copia de seguridad local, el cifrado en reposo se proporciona con la frase de contraseña que proporcione durante la copia de seguridad en Azure. Use el control de acceso basado en roles de Azure (RBAC de Azure) para proteger las copias de seguridad y las claves administradas por el cliente. 

Habilite la eliminación temporal y la protección de purga en Key Vault para proteger las claves contra la eliminación accidental o malintencionada. Si Azure Storage se usa para almacenar las copias de seguridad, habilite la eliminación temporal para guardar y recuperar los datos cuando se eliminen blobs o instantáneas de blobs.

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

- [Procedimiento para habilitar la eliminación temporal y la protección de purga en Key Vault](../storage/blobs/soft-delete-blob-overview.md)

- [Eliminación temporal para Azure Blob Storage](../storage/blobs/soft-delete-blob-overview.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Guía**: desarrolle una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia. 

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Uso de la guía de control de incidentes de seguridad de equipos de NIST para la creación de su propio plan de respuesta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque las suscripciones con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales. Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las funcionalidades de respuesta a los incidentes de los sistemas periódicamente; así, ayudará a proteger los recursos de Azure. Identifique puntos débiles y brechas y después revise el plan de respuesta según sea necesario.

- [Publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: exporte las alertas y recomendaciones de Azure Security Center y mediante la característica de exportación continua para ayudar a identificar riesgos en los recursos de Azure. La exportación continua permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para enviar las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilidad**: Customer

**Supervisión de Azure Security Center**: ninguna

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: use la característica de automatización de flujos de trabajo de Azure Security Center para desencadenar automáticamente las alertas y recomendaciones de seguridad con el fin de proteger los recursos de Azure.

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

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

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).
