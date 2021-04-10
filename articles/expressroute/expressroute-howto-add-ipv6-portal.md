---
title: 'Azure ExpressRoute: Adición de compatibilidad con IPv6 mediante Azure Portal'
description: Aprenda a agregar compatibilidad con IPv6 para conectarse a implementaciones de Azure mediante Azure Portal.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/09/2021
ms.author: duau
ms.openlocfilehash: 9b85caa3611c484fc17423637dc36ce20b48b823
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104868484"
---
# <a name="add-ipv6-support-for-private-peering-using-the-azure-portal-preview"></a>Adición de compatibilidad con IPv6 para el emparejamiento privado mediante Azure Portal (versión preliminar)

En este artículo se describe cómo agregar compatibilidad con IPv6 para conectarse a través de ExpressRoute a los recursos de Azure mediante Azure Portal. 

> [!Note]
> Esta característica está disponible actualmente en versión preliminar en [regiones de Azure con Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-region#azure-regions-with-availability-zones). Por lo tanto, el circuito ExpressRoute se puede crear con cualquier ubicación de emparejamiento, pero las implementaciones basadas en IPv6 a las que se conecta deben encontrarse en una región con Availability Zones.

## <a name="register-for-public-preview"></a>Registro para la versión preliminar pública
Antes de agregar la compatibilidad con IPv6, primero debe inscribir su suscripción. Para realizar la inscripción, ejecute los siguientes comandos a través de Azure PowerShell:

1.  Inicie sesión en Azure y seleccione la suscripción. Ejecute estos comandos para la suscripción que contiene el circuito ExpressRoute y para la suscripción que contiene las implementaciones de Azure (si son diferentes).

    ```azurepowershell-interactive
    Connect-AzAccount 

    Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
    ```

1. Registre la suscripción para la versión preliminar pública con el siguiente comando:
    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowIpv6PrivatePeering -ProviderNamespace Microsoft.Network
    ```

El equipo de ExpressRoute aprobará su solicitud en un plazo de 2 a 3 días laborables.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

En un explorador, vaya a [Azure Portal](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

## <a name="add-ipv6-private-peering-to-your-expressroute-circuit"></a>Adición de emparejamiento privado IPv6 al circuito ExpressRoute

1. [Cree un circuito ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) o navegue hasta el circuito existente que quiere cambiar.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-circuit.png" alt-text="Captura de pantalla de la lista de circuitos ExpressRoute.":::

1. Seleccione la configuración de emparejamiento **Azure privado**.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-peering.png" alt-text="Captura de pantalla de la página de información general de ExpressRoute.":::

1. Agregue un emparejamiento privado IPv6 a la configuración de emparejamiento privado IPv4 existente; para ello, seleccione "Ambas" en **Subredes**, o seleccione "IPv6" para habilitar el emparejamiento privado IPv6 en el nuevo circuito. Proporcione un par de subredes IPv6 /126 que posea para el vínculo principal y los vínculos secundarios. Desde cada una de estas subredes, asignará la primera dirección IP utilizable para el enrutador, ya que Microsoft usa la segunda dirección IP utilizable para su enrutador. Seleccione **Guardar** la configuración de emparejamiento una vez que haya especificado todos los parámetros.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-peering.png" alt-text="Captura de pantalla de la adición de IPv6 en la página de emparejamiento privado.":::

1. Una vez que la configuración se haya aceptado correctamente, debería ver algo similar al siguiente ejemplo.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/view-ipv6-peering.png" alt-text="Captura de pantalla de IPv6 configurada para el emparejamiento privado.":::

## <a name="update-your-connection-to-an-existing-virtual-network"></a>Actualización de la conexión a una red virtual existente

Siga los pasos que se indican a continuación si tiene un entorno existente de recursos de Azure en una región con Availability Zones con el que quisiera usar el emparejamiento privado IPv6.

1. Navegue a la red virtual a la que está conectado el circuito ExpressRoute.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-vnet.png" alt-text="Captura de pantalla de la lista de redes virtuales.":::

1. Vaya a la pestaña **Espacio de direcciones** y agregue un espacio de direcciones IPv6 a la red virtual. Seleccione **Guardar** el espacio de direcciones.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-space.png" alt-text="Captura de pantalla de la adición del espacio de direcciones IPv6 a la red virtual.":::

1. Vaya a la pestaña **Subredes** y seleccione **GatewaySubnet**. Active **Agregar un espacio de direcciones IPv6** y proporcione un espacio de direcciones IPv6 para la subred. La subred IPv6 de la puerta de enlace debe ser /64 o superior. Seleccione **Guardar** la configuración una vez que haya especificado todos los parámetros.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-gateway-space.png" alt-text="Captura de pantalla de la adición del espacio de direcciones IPv6 a la subred.":::

1. Si tiene una puerta de enlace con redundancia de zona existente, vaya a la puerta de enlace de ExpressRoute y actualice el recurso para habilitar la conectividad IPv6. De lo contrario, [cree la puerta de enlace de red virtual](expressroute-howto-add-gateway-portal-resource-manager.md) mediante una SKU con redundancia de zona (ErGw1AZ, ErGw2AZ, ErGw3AZ). Si tiene previsto usar FastPath, use ErGw3AZ.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/refresh-gateway.png" alt-text="Captura de pantalla de la actualización de la puerta de enlace.":::

## <a name="create-a-connection-to-a-new-virtual-network"></a>Creación de una conexión a una nueva red virtual

Siga los pasos que se indican a continuación si tiene previsto conectarse a un nuevo conjunto de recursos de Azure en una región con Availability Zones mediante el emparejamiento privado IPv6.

1. Cree una red virtual de doble pila con espacios de direcciones IPv4 e IPv6. Para obtener más información, consulte [Creación de una red virtual](../virtual-network/quick-create-portal.md#create-a-virtual-network).

1. [Creación de la subred de puerta de enlace de doble pila](expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet).

1. [Cree la puerta de enlace de red virtual](expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway) mediante una SKU con redundancia de zona (ErGw1AZ, ErGw2AZ, ErGw3AZ). Si tiene previsto usar FastPath, use ErGw3AZ (tenga en cuenta que esta opción solo está disponible para los circuitos mediante ExpressRoute Direct).

1. [Vincule la red virtual a su circuito ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md).

## <a name="limitations"></a>Limitaciones
Aunque la compatibilidad con IPv6 está disponible para las conexiones a las implementaciones en regiones con Availability Zones, no se admiten los siguientes casos de uso:

* Conexiones a implementaciones en Azure a través de una SKU de puerta de enlace ExpressRoute sin AZ
* Conexiones a implementaciones en regiones que no sin AZ
* Conexiones Global Reach entre circuitos ExpressRoute
* Uso de ExpressRoute con WAN virtual
* FastPath con circuitos que no son de ExpressRoute Direct
* FastPath con circuitos en las siguientes ubicaciones de emparejamiento: Dubái
* Coexistencia con VPN Gateway

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas de ExpressRoute, consulte los siguientes artículos:

* [Comprobación de la conectividad de ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Solución de problemas de rendimiento de red](expressroute-troubleshooting-network-performance.md)
