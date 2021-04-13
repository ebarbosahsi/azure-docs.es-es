---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: include
ms.date: 04/05/2021
ms.author: duau
ms.custom: include file
ms.openlocfilehash: 27f5755ce8b7d204cad6cdc2281d7992bf86615a
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2021
ms.locfileid: "106504717"
---
Al crear una puerta de enlace de red virtual, debe especificar la SKU de la puerta de enlace que desea usar. Cuando se selecciona una SKU de puerta de enlace superior, se asignan más CPU y mayor ancho de banda de red a la puerta de enlace y. como resultado, esta admite un mayor rendimiento de red a la red virtual. 

Las puertas de enlace de red virtual de ExpressRoute pueden utilizar las SKU siguientes:

|     | VPN Gateway y ExpressRoute coexisten | FastPath | Número máximo de conexiones de circuito |
| --- | --- | --- | --- |
| **Estándar** | Sí | No | 4 |
| **HighPerformance** | Sí | No | 4 |
| **UltraPerformance** | Sí | Sí | 16 |