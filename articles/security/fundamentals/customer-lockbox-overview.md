---
title: Caja de seguridad del cliente de Microsoft Azure
description: Introducción técnica de la Caja de seguridad del cliente de Microsoft Azure, que proporciona control sobre el acceso de proveedor en la nube cuando Microsoft necesite tener acceso a los datos del cliente.
author: TerryLanfear
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 02/19/2021
ms.openlocfilehash: 0146e4fcaf70d37975dc587a266c47bf4b3f4601
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103461681"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Caja de seguridad del cliente de Microsoft Azure

> [!NOTE]
> Para usar esta característica, su organización debe tener un [plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/) con un nivel mínimo de tipo **Desarrollador**.

La Caja de seguridad del cliente de Microsoft Azure proporciona una interfaz para los clientes y así permitirles revisar y aprobar o rechazar las solicitudes de acceso de datos de cliente. Se utiliza en casos donde un ingeniero de Microsoft necesita obtener acceso a los datos del cliente durante una solicitud de soporte técnico.

Este artículo se describe cómo habilitar Caja de seguridad del cliente y cómo se inician, siguen y almacenan las solicitudes de Caja de seguridad del cliente para revisiones y auditorías posteriores.

<a name='supported-services-and-scenarios-in-general-availability'></a><a name='supported-services-and-scenarios-in-preview'></a>
## <a name="supported-services-and-scenarios-general-availability"></a>Servicios y escenarios admitidos (Disponibilidad general)

Ahora los servicios siguientes están disponibles con carácter general para Caja de seguridad del cliente:

- Azure API Management
- Azure App Service
- Azure Cognitive Services
- Azure Container Registry
- Azure Database for MySQL
- Azure Databricks
- Azure Data Box
- Explorador de datos de Azure
- Azure Data Factory
- Azure Database for PostgreSQL
- Azure Functions
- HDInsight de Azure
- Azure Kubernetes Service
- Azure Monitor
- Azure Storage
- Azure SQL Database
- Transferencias de suscripciones de Azure
- Azure Synapse Analytics
- Máquinas virtuales de Azure (que abarcan el acceso de escritorio remoto, el acceso a los volcados de memoria y los discos administrados)

## <a name="enable-customer-lockbox"></a>Habilitación de Caja de seguridad del cliente

Ahora puede habilitar Caja de seguridad del cliente desde el [módulo de administración](https://aka.ms/customerlockbox/administration) en la hoja Caja de seguridad del cliente.  

> [!NOTE]
> Para habilitar Caja de seguridad del cliente, la cuenta de usuario debe tener [asignado el rol de administrador global](../../active-directory/roles/manage-roles-portal.md).

## <a name="workflow"></a>Flujo de trabajo

Los siguientes pasos describen un flujo de trabajo típico para una solicitud de la Caja de seguridad del cliente.

1. Un usuario en una organización tiene un problema con su carga de trabajo de Azure.

2. Esta persona soluciona el problema pero, como no puede corregirlo, se abre una incidencia de soporte técnico desde [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). La incidencia de soporte técnico se asigna a un ingeniero del servicio de soporte técnico de Azure.

3. Un ingeniero del soporte técnico de Azure revisa la solicitud de servicio y determina los pasos siguientes para resolver el problema.

4. Si el ingeniero de soporte técnico no puede solucionar el problema mediante la telemetría y las herramientas estándar, el siguiente paso es solicitar permisos con privilegios elevados mediante un servicio de acceso Just-In-Time (JIT). Esta solicitud puede ser del ingeniero de soporte técnico original o de otro, porque el problema se ha escalado al equipo de DevOps de Azure.

5. Una vez que el Ingeniero de Azure envía la solicitud de acceso, el servicio Just-In-Time evalúa la solicitud teniendo en cuenta factores como:
    - El ámbito del recurso
    - Si el solicitante es una identidad aislada o si usa autenticación multifactor.
    - Niveles de permisos

    De acuerdo con la regla JIT, esta solicitud también puede incluir una aprobación de los aprobadores internos de Microsoft. Por ejemplo, el aprobador podría ser el líder del soporte técnico al cliente o el encargado de DevOps.

6. Cuando la solicitud requiere obtener acceso directo a los datos del cliente, se inicia una solicitud de Caja de seguridad del cliente. Por ejemplo, se obtiene acceso desde el escritorio remoto a la máquina virtual de un cliente.

    La solicitud se encuentra ahora en un estado de **Cliente notificado**, según el cual se espera la aprobación del cliente antes de conceder el acceso.

7. En la organización del cliente, el usuario que tiene el [rol de propietario](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) de la suscripción de Azure recibe un correo electrónico de Microsoft para notificarle sobre la solicitud de acceso pendiente. Para las solicitudes de la Caja de seguridad del cliente, esta persona es el aprobador designado.

    Correo electrónico de ejemplo:

    ![Caja de seguridad del cliente de Azure: notificaciones por correo electrónico](./media/customer-lockbox-overview/customer-lockbox-email-notification.png)

8. La notificación por correo electrónico proporciona un vínculo a la hoja **Caja de seguridad del cliente** del módulo Administración. Al usar este enlace, el aprobador designado inicia sesión en Azure Portal para ver las solicitudes pendientes que su organización tiene para la Caja de seguridad del cliente:

    ![Caja de seguridad del cliente Azure: página de aterrizaje](./media/customer-lockbox-overview/customer-lockbox-landing-page.png)

   La solicitud permanece en la cola del cliente durante cuatro días. Transcurrido ese tiempo, la solicitud de acceso expira automáticamente y no se concede acceso a los ingenieros de Microsoft.

9. Para obtener los detalles de la solicitud pendiente, el aprobador designado puede seleccionar la solicitud de la caja de seguridad desde **Solicitudes pendientes**:

    ![Caja de seguridad del cliente de Azure: ver la solicitud pendiente](./media/customer-lockbox-overview/customer-lockbox-pending-requests.png)

10. El aprobador designado también puede seleccionar el **ID DE SOLICITUD DE SERVICIO** para ver la solicitud de la incidencia de soporte técnico que creó el usuario original. Esta información proporciona un contexto para el motivo por el cual el Soporte técnico de Microsoft está involucrado y el historial del problema informado. Por ejemplo:

    ![Caja de seguridad del cliente de Azure: ver la incidencia de soporte técnico](./media/customer-lockbox-overview/customer-lockbox-support-ticket.png)

11. Después de revisar la solicitud, el aprobador designado selecciona **Aprobar** o **Denegar**:

    ![Caja de seguridad del cliente de Azure: seleccionar Aprobar o Denegar](./media/customer-lockbox-overview/customer-lockbox-approval.png)

    Como resultado de la selección:
    - **Aprobar**:  Se concede acceso al ingeniero de Microsoft. El acceso se concede durante un período predeterminado de ocho horas.
    - **Denegar**: La solicitud de acceso con privilegios elevados del ingeniero de Microsoft se rechaza y no se realiza ninguna otra acción.

Con fines de auditoría, las acciones realizadas en este flujo de trabajo se registran en los [registros de solicitud de la Caja de seguridad del cliente](#auditing-logs).

## <a name="auditing-logs"></a>Registros de auditoría

Los registros de la Caja de seguridad del cliente se almacenan en los registros de actividad. En Azure Portal, seleccione **Registros de actividad** para ver la información de auditoría relacionada con las solicitudes de la Caja de seguridad del cliente. Puede filtrar acciones específicas, tales como:
- **Denegar la solicitud de la caja de seguridad**
- **Crear la solicitud de la caja de seguridad**
- **Aprobar la solicitud de la caja de seguridad**
- **Expiración de la solicitud de la caja de seguridad**

Por ejemplo:

![Caja de seguridad del cliente de Azure: registros de actividad](./media/customer-lockbox-overview/customer-lockbox-activitylogs.png)

## <a name="customer-lockbox-integration-with-azure-security-benchmark"></a>Integración de Caja de seguridad del cliente con la prueba comparativa de seguridad de Azure

Hemos introducido un nuevo control de línea base ([3.13](../benchmarks/security-control-identity-access-control.md#313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios)) en las pruebas comparativas de seguridad de Azure que cubren la aplicabilidad de Caja de seguridad del cliente. Ahora los clientes pueden aprovechar el punto de referencia para revisar la aplicabilidad de Caja de seguridad del cliente para un servicio.

## <a name="exclusions"></a>Exclusiones

Las solicitudes de Caja de seguridad del cliente no se activan en los siguientes escenarios de soporte técnico de ingeniería:

- Un ingeniero de Microsoft debe realizar una actividad que esté fuera de los procedimientos operativos estándar. Por ejemplo, para recuperar o restaurar servicios en escenarios inesperados o impredecibles.
- Un ingeniero de Microsoft accede a la plataforma de Azure como parte de la solución de problemas y, sin darse cuenta, obtiene acceso a los datos del cliente. Por ejemplo, el equipo de Azure Network realiza la resolución de problemas que resulta en una captura de paquetes en un dispositivo de red. En este escenario, si el cliente cifra los datos mientras están en tránsito, el ingeniero no podrá leerlos.

## <a name="next-steps"></a>Pasos siguientes

Caja de seguridad del cliente está disponible para todos los clientes que tengan un [plan de Soporte técnico de Azure](https://azure.microsoft.com/support/plans/) con un nivel mínimo de **Desarrollador**. Puede habilitar Caja de seguridad del cliente desde el [módulo de administración](https://aka.ms/customerlockbox/administration) en la hoja Caja de seguridad del cliente.

Un ingeniero de Microsoft inicia las solicitudes de Caja de seguridad del cliente si esta acción es necesaria para avanzar un caso de soporte técnico.
