---
title: 'Modificación de la configuración de la dirección IP de la puerta de enlace: PowerShell'
description: Este artículo explica paso a paso cómo cambiar los prefijos de direcciones IP de la puerta de enlace de red local con PowerShell.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/10/2021
ms.author: cherylmc
ms.openlocfilehash: 07d9124105424d42970800b1c5bca956d512722d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100388675"
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>Modificación de la configuración de la puerta de enlace de red local mediante PowerShell

A veces, cambia la configuración de la puerta de enlace de red local AddressPrefix o GatewayIPAddress. Este artículo muestra cómo modificar la configuración de puerta de enlace de red local. También puede modificar estas configuraciones mediante un método diferente, si selecciona una opción diferente de la lista siguiente:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [CLI de Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>Antes de empezar

Instale la versión más reciente de los cmdlets de PowerShell de Azure Resource Manager. Consulte [Cómo instalar y configurar Azure PowerShell](/powershell/azure/) para obtener más información sobre cómo instalar los cmdlets de PowerShell.

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Modificación de los prefijos de direcciones IP

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Modificación de la dirección IP de la puerta de enlace

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Puede comprobar la conexión de la puerta de enlace. Consulte [Comprobación de una conexión de puerta de enlace](vpn-gateway-verify-connection-resource-manager.md).