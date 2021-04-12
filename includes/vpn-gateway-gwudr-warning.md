---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: cf9d4c3fd96df83361e7d9aa89ba702d37265ec6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95563708"
---
Las rutas definidas por el usuario con un destino 0.0.0.0/0 y NSG en GatewaySubnet **no se admiten**. Se bloqueará la creación de puertas de enlace creadas con esta configuración. Las puertas de enlace requieren acceso a los controladores de administración para que funcionen correctamente. [Propagación de rutas BGP](../articles/virtual-network/virtual-networks-udr-overview.md#border-gateway-protocol) debe establecerse en "Habilitado" en GatewaySubnet para garantizar la disponibilidad de la puerta de enlace. Si se establece en deshabilitado, la puerta de enlace no funcionará.