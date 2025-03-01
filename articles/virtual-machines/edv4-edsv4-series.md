---
title: Series Edv4 y Edsv4
description: Especificaciones para las VM de las series Ev4, Edv4, Esv4 and Edsv4.
author: brbell
ms.author: brbell
ms.reviewer: jushiman
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 02/04/2020
ms.openlocfilehash: 407d208ef0385c795d086bb495ed51a020395cd2
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2021
ms.locfileid: "106443993"
---
# <a name="edv4-and-edsv4-series"></a>Series Edv4 y Edsv4

Las series Edv4 y Edsv4 se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) en una configuración con subprocesamiento múltiple, son ideales para distintas aplicaciones empresariales con un uso intensivo de memoria y cuentan con hasta 504 GiB de RAM, tecnología [Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), tecnología [Intel&reg; Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) e [Intel&reg; Advanced Vector Extensions 512 (Intel&reg; AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). También admiten [Intel&reg; Deep Learning Boost](https://software.intel.com/content/www/us/en/develop/topics/ai/deep-learning-boost.html). Estos nuevos tamaños de VM tendrán un almacenamiento local un 50 % más grande, así como una mayor E/S por segundo del disco local para lectura y escritura, en comparación con los tamaños [Ev3/Esv3](./ev3-esv3-series.md) con [VM de segunda generación](./generation-2.md). Cuenta con una velocidad de reloj en todos los núcleos de 3,4 GHz. 

## <a name="edv4-series"></a>Serie Edv4

Los tamaños de la serie Edv4 se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Los tamaños de máquina virtual Edv4 cuentan con hasta 504 GiB de RAM, además de almacenamiento SSD local rápido y de gran tamaño (hasta 2400 GiB). Estas máquinas virtuales son ideales para aplicaciones empresariales de uso intensivo de memoria y aplicaciones que se benefician del almacenamiento local de baja latencia y alta velocidad. Puede conectar almacenamiento de discos SSD y HDD estándar a las VM Edv4. 

[ACU](acu.md): 195 - 210<br>
[Premium Storage](premium-storage-performance.md): No compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): No compatible<br>
[Migración en vivo](maintenance-and-updates.md): Compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): Compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible (*se requieren 4 vCPU como mínimo*)<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | <sup>**</sup> Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps | Nº máx. NIC|Ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_E2d_v4  | 2 | 16 | 75 | 4 | 19000/120 | 2|1000 |
| Standard_E4d_v4  | 4 | 32 | 150 | 8 | 38500/242 | 2|2000 |
| Standard_E8d_v4 | 8 | 64 | 300 | 16 | 77000/485 | 4|4000 |
| Standard_E16d_v4 | 16 | 128 | 600 | 32 | 154000/968 | 8|8000 |
| Standard_E20d_v4 | 20 | 160 | 750 | 32 | 193000/1211  | 8|10000 |
| Standard_E32d_v4 | 32 | 256 | 1200 | 32 | 308000/1936 | 8|16000 |
| Standard_E48d_v4 | 48 | 384 | 1800 | 32 | 462000/2904 | 8|24000 |
| Standard_E64d_v4 | 64 | 504 | 2400 | 32 | 615000/3872 | 8|30000 |


<sup>**</sup> Estos valores de IOPS se pueden garantizar mediante las [VM Gen2](generation-2.md)

## <a name="edsv4-series"></a>Serie Edsv4

Los tamaños de la serie Edsv4 se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). Los tamaños de máquina virtual Edsv4 cuentan con hasta 504 GiB de RAM, además de almacenamiento SSD local, rápido y de gran tamaño (hasta 2400 GiB). Estas máquinas virtuales son ideales para aplicaciones empresariales de uso intensivo de memoria y aplicaciones que se benefician del almacenamiento local de baja latencia y alta velocidad.

[ACU](acu.md): 195-210<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): Compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): Compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible (*se requieren 4 vCPU como mínimo*)<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): Compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | <sup>**</sup> Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. NIC|Ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|---|
| Standard_E2ds_v4  | 2 | 16 | 75 | 4 | 19000/120(50) | 3200/48 | 2|1000 |
| Standard_E4ds_v4  | 4 | 32 | 150 | 8 | 38500/242(100) | 6400/96 | 2|2000 |
| Standard_E8ds_v4 | 8 | 64 | 300 | 16 | 77000/485(200) | 12800/192 | 4|4000 |
| Standard_E16ds_v4 | 16 | 128 | 600 | 32 | 154000/968(400) | 25600/384 | 8|8000 |
| Standard_E20ds_v4 | 20 | 160 | 750 | 32 | 193000/1211(500)  | 32000/480  | 8|10000 |
| Standard_E32ds_v4 | 32 | 256 | 1200 | 32 | 308000/1936(800) | 51200/768  | 8|16000 |
| Standard_E48ds_v4 | 48 | 384 | 1800 | 32 | 462000/2904(1200) | 76800/1152 | 8|24000 |
| Standard_E64ds_v4 <sup>1</sup> | 64 | 504 | 2400 | 32 | 615000/3872(1600) | 80000/1200 | 8|30000 |
| Standard_E80ids_v4 <sup>2</sup> | 80 | 504 | 2400 | 32 | 615000/3872(1600) | 80000/1500 | 8|30000 |

<sup>1</sup> [Tamaños de núcleos restringidos disponibles](./constrained-vcpu.md).

<sup>2</sup> La instancia está aislada en el hardware dedicado a un solo cliente.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Otros tamaños e información

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

Calculadora de precios: [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)

Más información sobre los tipos de disco: [Tipos de disco](./disks-types.md#ultra-disk)


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
