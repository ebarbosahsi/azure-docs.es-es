---
title: 'Mantenimiento programado en Azure Database for PostgreSQL: Hiperescala (Citus)'
description: 'En este artículo se describe la característica de mantenimiento programado de Azure Database for PostgreSQL: Hiperescala (Citus).'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/22/2021
ms.openlocfilehash: 0d11ec8815cc0f5082f95c5331321f043e86eece
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105027506"
---
# <a name="scheduled-maintenance-in-azure-database-for-postgresql--hyperscale-citus"></a>Mantenimiento programado en Azure Database for PostgreSQL: Hiperescala (Citus)

Azure Database for PostgreSQL: Hiperescala (Citus) realiza un mantenimiento periódico para conservar la base de datos administrada protegida, estable y actualizada.  Durante el mantenimiento, todos los nodos del grupo de servidores obtienen nuevas características, actualizaciones y revisiones.

Las principales características del mantenimiento programado de Hiperescala (Citus) son:

* Las actualizaciones se aplican al mismo tiempo en todos los nodos del grupo de servidores.
* Las notificaciones acerca del próximo mantenimiento se publican en Azure Service Health con cinco días de antelación.
* Normalmente, hay al menos 30 días entre los eventos de mantenimiento correctos de un grupo de servidores.

## <a name="notification-about-upcoming-maintenance"></a>Notificación acerca del próximo mantenimiento

Las notificaciones acerca del próximo mantenimiento se publican en Azure Service Health y también se pueden:

* Enviar por correo electrónico a una dirección concreta
* Enviar por correo electrónico a un rol de Azure Resource Manager
* Enviar en un mensaje de texto (SMS) a dispositivos móviles
* Insertar como una notificación en una aplicación de Azure
* Entregar como un mensaje de voz

> [!IMPORTANT]
> Normalmente, hay al menos 30 días entre los eventos de mantenimiento programado correctos de un grupo de servidores.
>
> Sin embargo, en el caso de una actualización de emergencia crítica, como una vulnerabilidad grave, la ventana de notificación podría ser inferior a cinco días. La actualización crítica se puede aplicar al servidor aunque se haya realizado un mantenimiento programado correcto durante los últimos 30 días.

Si se produce un error en el mantenimiento o se cancela, el sistema generará una notificación.
Intentará realizar el mantenimiento de nuevo según la configuración de programación actual y le notificará cinco días antes del siguiente evento de mantenimiento.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [obtener notificaciones sobre próximos eventos de mantenimiento](../service-health/service-notifications.md) con Azure Service Health.
* Obtenga información sobre cómo [configurar alertas sobre próximos eventos de mantenimiento programado](../service-health/resource-health-alert-monitor-guide.md).
