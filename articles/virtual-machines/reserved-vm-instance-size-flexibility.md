---
title: 'Flexibilidad en el tamaño de las máquinas virtuales: Azure Reserved VM Instances'
description: Obtenga información sobre a qué serie de tamaño se aplica un descuento de reserva cuando compra una instancia reservada de máquina virtual.
author: yashesvi
ms.service: virtual-machines
ms.subservice: reserved-instances
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 03/01/2021
ms.author: yashar
ms.openlocfilehash: 9270b7fb135eddd78a7aeb30cf88af4d704e176e
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102030715"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances

Al comprar una instancia reservada de VM, puede elegir entre optimizar la flexibilidad de tamaño de instancia o la prioridad de capacidad. Para obtener más información acerca de cómo establecer o cambiar la configuración de optimización de instancias reservadas de VM, consulte [Cambiar la configuración de optimización para instancias reservadas de máquina virtual](../cost-management-billing/reservations/manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).

Con una instancia reservada de máquina virtual optimizada para conseguir flexibilidad en el tamaño de la instancia, la reserva que adquiera se puede aplicar a los tamaños de las máquinas virtuales del mismo grupo de flexibilidad de tamaño de instancia. Por ejemplo, si compra una reserva para un tamaño de máquina virtual de la serie DSv2 como, por ejemplo, Standard_DS3_v2, el descuento por la reserva se puede aplicar a los otros tamaños que aparecen en el mismo grupo de flexibilidad de tamaño de instancia:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Pero ese descuento de reserva no se aplica a los tamaños de máquinas virtuales que aparecen en grupos de flexibilidad de tamaño de instancia diferentes, como las SKU en la serie DSv2 de memoria alta: Standard_DS11_v2, Standard_DS12_v2, etc.

Dentro del grupo de flexibilidad de tamaño de instancia, el número de máquinas virtuales al que se aplica el descuento por la reserva depende del tamaño de máquina virtual que elija al comprar una reserva. También depende de los tamaños de las máquinas virtuales que tenga en ejecución. La columna de relación compara la superficie relativa para cada tamaño de máquina virtual en ese grupo de flexibilidad de tamaño de instancia. Use el valor de relación para calcular cómo se aplica el descuento por la reserva a las máquinas virtuales que tiene en ejecución.

## <a name="examples"></a>Ejemplos

Los ejemplos siguientes usan los tamaños y relaciones en la tabla de la serie DSv2.

Va a comprar una instancia reservada de máquina virtual con el tamaño Standard_DS4_v2 en la que la relación o superficie relativa comparada con los otros tamaños de esa serie es 8.

- Escenario 1: se ejecutan ocho máquinas virtuales con un tamaño Standard_DS1_v2 con una relación de 1. El descuento por la reserva se aplica a las ocho máquinas virtuales.
- Escenario 2: se ejecutan dos máquinas virtuales con un tamaño Standard_DS2_v2 con una relación de 2 cada una. También se ejecuta una máquina virtual con tamaño Standard_DS3_v2 con una relación de 4. La superficie total es 2+2+4=8. Por tanto, el descuento por la reserva se aplica a las tres máquinas virtuales.
- Escenario 3: se ejecuta una máquina virtual con un tamaño Standard_DS5_v2 con una relación de 16. El descuento por la reserva se aplicaría a la mitad del costo de proceso de esa máquina virtual.

En las siguientes secciones se muestra qué tamaños están en el mismo grupo de serie de tamaño cuando compra una instancia reservada de máquina virtual optimizada con flexibilidad de tamaño.

## <a name="instance-size-flexibility-ratio-for-vms"></a>Relación de flexibilidad de tamaño de instancia para máquinas virtuales 

El siguiente CSV contiene los grupos, ArmSkuName y las relaciones de flexibilidad de tamaño de la instancia.  

[Relaciones de flexibilidad de tamaño de instancia](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

Azure mantiene actualizados el vínculo y el esquema para que pueda usar el archivo mediante programación.

## <a name="view-vm-size-recommendations"></a>Visualización de recomendaciones de tamaño de máquina virtual

Azure muestra recomendaciones de tamaño de máquina virtual en la experiencia de compra. Para ver las recomendaciones de tamaño más pequeño, seleccione **Group by smallest size** (Agrupar por tamaño más pequeño).

:::image type="content" source="./media/reserved-vm-instance-size-flexibility/select-product-recommended-quantity.png" alt-text="Captura de pantalla que muestra las cantidades recomendadas." lightbox="./media/reserved-vm-instance-size-flexibility/select-product-recommended-quantity.png" :::

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [¿Qué son las reservas de Azure?](../cost-management-billing/reservations/save-compute-costs-reservations.md)
