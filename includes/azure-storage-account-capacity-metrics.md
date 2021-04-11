---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 1aeaa8607618a6f0a2dbf756e6ddae420627bce6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "101750354"
---
En esta tabla se muestran [métricas de nivel de cuenta](../articles/azure-monitor/essentials/metrics-supported.md#microsoftstoragestorageaccounts).

| Métrica | Descripción |
| ------------------- | ----------------- |
| UsedCapacity | La cantidad de almacenamiento que utiliza la cuenta de almacenamiento. En las cuentas de almacenamiento estándar, es la suma de la capacidad usada por un blob, una tabla, un archivo y una cola. Tanto en las cuentas de almacenamiento Premium como en las cuentas de Blob Storage, coincide con BlobCapacity. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |