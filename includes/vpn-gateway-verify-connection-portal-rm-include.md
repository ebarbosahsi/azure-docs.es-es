---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3c825f042bb3e7fee5c00a8b34c12ca2d05f8d2e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92217984"
---
En Azure Portal, puede ver el estado de la conexión de una instancia de VPN Gateway de Resource Manager navegando a la conexión. Los pasos siguientes muestran una manera de navegar a su conexión y realizar las comprobaciones necesarias.

1. En [Azure Portal](https://portal.azure.com), haga clic en **Todos los recursos** y navegue a la puerta de enlace de la red virtual.
1. En la hoja de la puerta de enlace de la red virtual, haga clic en **Conexiones**. Puede ver el estado de cada conexión.

   :::image type="content" source="./media/vpn-gateway-verify-connection-portal-rm-include/connections.png" alt-text="Ver conexiones" lightbox="./media/vpn-gateway-verify-connection-portal-rm-include/connections-expand.png":::

1. Haga clic en el nombre de la conexión que desee comprobar. En **Essentials**, puede ver más información acerca de la conexión. Los valores del campo **Estado** serán "Correcto" y "Conectado" cuando haya realizado una conexión correcta.

   :::image type="content" source="./media/vpn-gateway-verify-connection-portal-rm-include/essentials.png" alt-text="Comprobación de conexiones" lightbox="./media/vpn-gateway-verify-connection-portal-rm-include/essentials-expand.png":::
