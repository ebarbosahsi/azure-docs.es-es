---
title: Cambiar el tamaño de un grupo de capacidad o de un volumen de Azure NetApp Files  | Microsoft Docs
description: Aprenda a cambiar el tamaño de un grupo de capacidad o de un volumen. Al cambiar el tamaño del grupo de capacidad, cambia la capacidad adquirida de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/10/2021
ms.author: b-juche
ms.openlocfilehash: 869f46207b940521ee0b66b5afa9c6e2718ab04f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104594485"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Cambiar el tamaño de un grupo de capacidad o de un volumen
Puede cambiar el tamaño de un grupo de capacidad o de un volumen según sea necesario. 

## <a name="resize-the-capacity-pool"></a>Cambiar el tamaño del grupo de capacidad 

Puede cambiar el tamaño del grupo de capacidad en incrementos o decrementos de 1 TiB. Aun así, el tamaño del grupo de capacidad no puede ser inferior a 4 TiB. Al cambiar el tamaño del grupo de capacidad, cambia la capacidad adquirida de Azure NetApp Files.

1. En la hoja de administración de cuenta de NetApp, haga clic en el grupo de capacidad cuyo tamaño desee cambiar. 
2. Haga clic con el botón derecho en el nombre del grupo de capacidad o haga clic en el icono "…" al final de la fila del grupo de capacidad para mostrar el menú contextual. 
3. Use las opciones del menú contextual para cambiar el tamaño del grupo de capacidad o eliminarlo.

## <a name="resize-a-volume"></a>Cambiar el tamaño de un volumen

Puede cambiar el tamaño de un volumen según sea necesario. El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo.

1. En la hoja de administración de cuenta de NetApp, haga clic en **Volúmenes**. 
2. Haga clic con el botón derecho en el nombre del volumen cuyo tamaño desee cambiar o haga clic en el icono "…" al final de la fila del volumen para mostrar el menú contextual.
3. Use las opciones del menú contextual para cambiar el tamaño del volumen o eliminarlo.

## <a name="resize-a-cross-region-replication-destination-volume"></a>Cambio de tamaño de un volumen de destino de replicación entre regiones 

En una relación de [replicación entre regiones](cross-region-replication-introduction.md), el tamaño de un volumen de destino se cambia automáticamente en función del tamaño del volumen de origen. De este modo, no es necesario cambiar el tamaño del volumen de destino por separado. Este cambio de tamaño automático es aplicable cuando los volúmenes establecen una relación de replicación activa, o cuando el emparejamiento de replicación se interrumpe con la [operación de resincronización](cross-region-replication-manage-disaster-recovery.md#resync-replication). 

En la tabla siguiente se describe el comportamiento de cambio de tamaño del volumen de destino en función del [estado del reflejo](cross-region-replication-display-health-status.md):

|  Estado del reflejo  | Comportamiento de cambio de tamaño del volumen de destino |
|-|-|
| *Reflejado* | Cuando el volumen de destino se ha inicializado y está listo para recibir actualizaciones de creación de reflejo, el cambio de tamaño del volumen de origen cambia automáticamente el tamaño de los volúmenes de destino. |
| *Interrumpido* | Cuando se cambia el tamaño del volumen de origen y el estado del reflejo se *interrumpe*, el tamaño del volumen de destino se cambia automáticamente con la [operación de resincronización](cross-region-replication-manage-disaster-recovery.md#resync-replication).  |
| *Sin inicializar* | Al cambiar el tamaño del volumen de origen y el estado del reflejo es aún *inicializar*, el cambio de tamaño del volumen de destino debe realizarse manualmente. Como tal, se recomienda esperar a que finalice la inicialización (es decir, cuando el estado del reflejo sea *reflejado*) para cambiar el tamaño del volumen de origen. | 

> [!IMPORTANT]
> Asegúrese de que dispone de suficiente espacio en los grupos de capacidad para los volúmenes de origen y de destino de la replicación entre regiones. Al cambiar el tamaño del volumen de origen, se cambia automáticamente el tamaño del volumen de destino. Sin embargo, si el grupo de capacidad que hospeda el volumen de destino no tiene suficiente espacio, se producirá un error al cambiar de tamaño los volúmenes de origen y de destino.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
- [Administración de un grupo de capacidad de QoS manual](manage-manual-qos-capacity-pool.md)
- [Cambio dinámico del nivel de servicio de un volumen](dynamic-change-volume-service-level.md) 