---
title: 'Azure VMware Solution by CloudSimple: asignación de direcciones IP públicas'
description: Se describe cómo asignar direcciones IP públicas para las máquinas virtuales del entorno de nube privada.
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bb46ad726cd3b99324e9bb96b998ed1b4da885de
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97899191"
---
# <a name="allocate-public-ip-addresses-for-private-cloud-environment"></a>Asignación de direcciones IP públicas para un entorno de nube privada

Abra la pestaña de direcciones IP públicas en la página de red para asignar direcciones IP públicas para las máquinas virtuales del entorno de nube privada.

1. [Acceda al portal de CloudSimple](access-cloudsimple-portal.md) y seleccione **Network** (Red) en el menú lateral.
2. Seleccione **Public IPs** (Direcciones IP públicas).
3. Haga clic en **New Public IP** (Nueva IP pública).

    ![Página de direcciones IP públicas](media/public-ips-page.png)

4. Escriba un nombre para identificar la entrada de la dirección IP.
5. Mantenga la ubicación predeterminada.
6. Use el control deslizante para cambiar el tiempo de espera de inactividad si lo desea.
7. Escriba la dirección IP local a la que quiere asignar una dirección IP pública.
8. Escriba un nombre DNS asociado.
9. Haga clic en **Enviar**.

![Asignación de IP públicas](media/network-public-ip-allocate.png)

Comienza la tarea de asignar la dirección IP pública. Puede comprobar el estado de la tarea en la página **Activity > Tasks** (Actividad > Tareas). Cuando se complete la asignación, se muestra la nueva entrada en la página de direcciones IP públicas.
