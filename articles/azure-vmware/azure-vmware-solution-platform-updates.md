---
title: Actualizaciones de la plataforma para Azure VMware Solution
description: Más información sobre las actualizaciones de la plataforma en Azure VMware Solution.
ms.topic: reference
ms.date: 03/24/2021
ms.openlocfilehash: da6317d49edd3f40e1a8f2518f91fe353bbae285
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105045219"
---
# <a name="platform-updates-for-azure-vmware-solution"></a>Actualizaciones de la plataforma para Azure VMware Solution

Azure VMware Solution va a aplicar actualizaciones importantes a partir de marzo de 2021. Va a recibir una notificación por medio de Azure Service Health con la escala de tiempo del mantenimiento. Para obtener más información, vea [Actualizaciones y mejoras de la nube privada de Azure VMware Solution](concepts-upgrades.md).

## <a name="march-24-2021"></a>24 de marzo de 2021
Todas las nubes privadas de Azure VMware Solution se implementan con VMware vCenter, versión 6.7U3l y NSX-T, versión 3.1.1. Cualquier nube privada existente se va a actualizar **hasta junio de 2021** a las versiones mencionadas anteriormente.

Va a recibir un correo electrónico con la fecha y hora de mantenimiento planeado. Puede volver a programar una actualización. El correo electrónico también proporciona detalles sobre el componente actualizado, su efecto en las cargas de trabajo, el acceso a la nube privada y otros servicios de Azure.  Una hora antes de la actualización va a recibir una notificación y luego otra cuando termine.

## <a name="march-15-2021"></a>15 de marzo de 2021 

- El servicio Azure VMware Solution va a efectuar tareas de mantenimiento **hasta el 19 de marzo de 2021** para actualizar la instancia de vCenter Server de la nube privada a la versión vCenter Server 6.7 Update 3l.

- VMware vCenter no va a estar disponible durante este tiempo.  Por lo tanto, no va a poder administrar las máquinas virtuales (detener, iniciar, crear, eliminar) ni el escalado de la nube privada (agregar o quitar servidores y clústeres). Pero la alta disponibilidad de VMware va a seguir funcionando para proteger las máquinas virtuales existentes. 
 
Para obtener más información sobre esta versión de vCenter, vea [Notas de la versión de VMware vCenter Server 6.7, actualización 3l](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/vsphere-vcenter-server-67u3l-release-notes.html).

## <a name="march-4-2021"></a>4 de marzo de 2021

- Azure VMware Solution va a aplicar [VMware ESXi 6.7, Patch Release ESXi670-202011002](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/esxi670-202011002.html) a las nubes privadas existentes **hasta el 15 de marzo de 2021**.

- Las soluciones alternativas documentadas para la pila de vSphere, según [VMSA-2021-0002](https://www.vmware.com/security/advisories/VMSA-2021-0002.html), también se van a aplicar **hasta el 15 de marzo de 2021**.

>[!NOTE]
>Esto no provoca interrupciones y no debe afectar a los servicios o las cargas de trabajo de Azure VMware. Durante el mantenimiento, varias alertas de VMware, como _Lost network connectivity on DVPorts_ (Conectividad de red perdida en DVPorts) y _Lost uplink redundancy on DVPorts_ (Redundancia de vínculo superior perdido en DVPorts), aparecen en vCenter y se van borrando automáticamente a medida que progresa el mantenimiento.

## <a name="post-update"></a>Después de la actualización
Una vez finalizada, aparecen las versiones más recientes de los componentes de VMware. Si nota algún problema o tiene alguna pregunta, póngase en contacto con nuestro equipo de soporte técnico al abrir una incidencia de soporte técnico.