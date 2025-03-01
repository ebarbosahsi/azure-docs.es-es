---
title: 'Azure ExpressRoute: Tablas ARP: solución de problemas'
description: En esta página se proporcionan instrucciones sobre cómo obtener tablas del Protocolo de resolución de direcciones (ARP) para un circuito ExpressRoute.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: troubleshooting
ms.date: 12/15/2020
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 7d8ae2c58979c66ebbbab366d172179bdeee4253
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97561586"
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>Obtención de tablas ARP en el modelo de implementación de Resource Manager
> [!div class="op_single_selector"]
> * [PowerShell: administrador de recursos](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell: clásico](expressroute-troubleshooting-arp-classic.md)
> 
> 

Este artículo le guía por los pasos necesarios para comprender las tablas ARP del circuito ExpressRoute.

> [!IMPORTANT]
> Este documento está pensado para ayudarle a diagnosticar y corregir problemas sencillos. No pretende sustituir al soporte técnico de Microsoft. Si no puede resolver el problema siguiendo las instrucciones que se describen a continuación, abra una incidencia de soporte técnico dirigida al [soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
> 
> 

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Protocolo de resolución de direcciones (ARP) y tablas ARP
El Protocolo de resolución de direcciones (ARP) es un protocolo de nivel 2 definido en [RFC 826](https://tools.ietf.org/html/rfc826). ARP se utiliza para asignar la dirección Ethernet (dirección MAC) con una dirección IP.

La tabla de ARP proporciona la siguiente información sobre las interfaces principal y secundaria de cada tipo de emparejamiento:

1. Asignación de la dirección IP de la interfaz del enrutador local a la dirección MAC
2. Asignación de la dirección IP de la interfaz del enrutador de ExpressRoute a la dirección MAC
3. Antigüedad de la asignación

Las tablas ARP pueden ayudar a validar la configuración de nivel 2 y a solucionar los problemas básicos de conectividad de nivel 2. 

Ejemplo de tabla ARP: 

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           10.0.0.1   ffff.eeee.dddd
  0 Microsoft         10.0.0.2   aaaa.bbbb.cccc
```


En la siguiente sección se proporciona información sobre cómo puede ver las tablas ARP que ven los enrutadores de borde de ExpressRoute. 

## <a name="prerequisites-for-learning-arp-tables"></a>Requisitos previos para comprender las tablas ARP
Asegúrese de que la información siguiente es correcta antes de continuar:

* Un circuito de ExpressRoute válido configurado con al menos un emparejamiento. El circuito debe estar completamente configurado por el proveedor de conectividad. Usted o su proveedor de conectividad debe haber configurado al menos los emparejamientos privados de Azure, los públicos de Azure o los de Microsoft en este circuito.
* Intervalos de direcciones IP que se usan para configurar los emparejamientos. Revise los ejemplos de asignación de direcciones IP en la [página de requisitos de enrutamiento de ExpressRoute](expressroute-routing.md) para comprender cómo se asignan las direcciones IP a las interfaces. Puede obtener información sobre la configuración de emparejamiento en la página [Creación y modificación del enrutamiento de un circuito ExpressRoute](expressroute-howto-routing-arm.md).
* Información del equipo de red o del proveedor de conectividad sobre las direcciones MAC de las interfaces usadas con estas direcciones IP.
* Debe tener como mínimo el módulo más reciente de PowerShell para Azure (versión 1.50 o superior).

> [!NOTE]
> Si el proveedor de servicio proporciona la capa 3 y las tablas ARP están en blanco en el portal o en la salida siguiente, actualice la configuración del circuito mediante el botón de actualización del portal. Esta operación aplicará la configuración de enrutamiento correcta a su circuito. 
>
>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Obtención de las tablas ARP para el circuito ExpressRoute
En esta sección se proporcionan instrucciones sobre cómo ver las tablas ARP por emparejamiento mediante PowerShell. Usted o su proveedor de conectividad deben haber configurado el emparejamiento antes de seguir adelante. Cada circuito tiene dos rutas de acceso (principal y secundaria). Puede comprobar en la tabla ARP cada ruta de acceso de forma independiente.

### <a name="arp-tables-for-azure-private-peering"></a>Tablas ARP para el emparejamiento privado de Azure
El siguiente cmdlet proporciona el emparejamiento privado de Azure para las tablas ARP:

```azurepowershell
# Required Variables
$RG = "<Your Resource Group Name Here>"
$Name = "<Your ExpressRoute Circuit Name Here>"

# ARP table for Azure private peering - Primary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

# ARP table for Azure private peering - Secondary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 
```

A continuación se muestra el resultado de ejemplo de una de las rutas de acceso:

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           10.0.0.1   ffff.eeee.dddd
  0 Microsoft         10.0.0.2   aaaa.bbbb.cccc
```


### <a name="arp-tables-for-azure-public-peering"></a>Tablas ARP para el emparejamiento público de Azure
El siguiente cmdlet proporciona las tablas ARP para el emparejamiento público de Azure:

```azurepowershell
# Required Variables
$RG = "<Your Resource Group Name Here>"
$Name = "<Your ExpressRoute Circuit Name Here>"

# ARP table for Azure public peering - Primary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

# ARP table for Azure public peering - Secondary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 
```


A continuación se muestra el resultado de ejemplo de una de las rutas de acceso:

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           64.0.0.1   ffff.eeee.dddd
  0 Microsoft         64.0.0.2   aaaa.bbbb.cccc
```


### <a name="arp-tables-for-microsoft-peering"></a>Tablas ARP para el emparejamiento de Microsoft
El siguiente cmdlet proporciona las tablas ARP para el emparejamiento de Microsoft:

```azurepowershell
# Required Variables
$RG = "<Your Resource Group Name Here>"
$Name = "<Your ExpressRoute Circuit Name Here>"

# ARP table for Microsoft peering - Primary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

# ARP table for Microsoft peering - Secondary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 
```


A continuación se muestra el resultado de ejemplo de una de las rutas de acceso:

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           65.0.0.1   ffff.eeee.dddd
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```


## <a name="how-to-use-this-information"></a>Uso de esta información
La tabla ARP de un emparejamiento se puede usar para determinar o validar la configuración y la conectividad de nivel 2. En esta sección se proporciona información general sobre la apariencia de las tablas ARP en distintos escenarios.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Tabla ARP cuando un circuito está en estado operativo (estado esperado)
* La tabla de ARP tendrá una entrada para el lado local con una dirección IP válida y la dirección MAC. Lo mismo puede verse en el lado de Microsoft. 
* El último octeto de la dirección IP local siempre será un número impar.
* El último octeto de la dirección IP de Microsoft siempre será un número par.
* La misma dirección MAC aparecerá en el lado de Microsoft para los tres emparejamientos (principales o secundarios). 

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           65.0.0.1   ffff.eeee.dddd
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>Tabla ARP cuando el lado del proveedor de conectividad o local tiene problemas
Si se produce un problema con el proveedor local o de conectividad, la tabla de ARP mostrará una de las dos cosas. Verá que la dirección MAC local se muestra como incompleta o que solo puede ver la entrada de Microsoft en la tabla de ARP.
  
```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------   
  0 On-Prem           65.0.0.1   Incomplete
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```
or
   
```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```  

> [!NOTE]
> Abra una solicitud de soporte técnico con su proveedor de conectividad para depurar tales problemas. Si la tabla ARP no tiene direcciones IP de las interfaces que se asignan a direcciones MAC, revise la información siguiente:
> 
> 1. Si la primera dirección IP de la subred /30 asignada para el vínculo entre el MSEE-PR y MSEE se utiliza en la interfaz de MSEE-PR. Azure usa siempre la segunda dirección IP para los MSEE.
> 2. Compruebe si las etiquetas VLAN de servicio (S-Tag) y el cliente (C-Tag) coinciden en el par MSEE PR y MSEE.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>Tabla ARP cuando el lado de Microsoft tiene problemas
* Cuando haya problemas en el lado de Microsoft, no verá una tabla de ARP para un emparejamiento. 
* Abra una incidencia de soporte técnico dirigida al [soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Especifique que tiene un problema con la conectividad de nivel 2. 

## <a name="next-steps"></a>Pasos siguientes
* Validar las configuraciones de nivel 3 para el circuito ExpressRoute.
  * Obtener el resumen de ruta para determinar el estado de las sesiones BGP.
  * Obtener la tabla de rutas para determinar qué prefijos se anuncian a través de ExpressRoute.
* Validar la transferencia de datos revisando los bytes de entrada y de salida.
* Si sigue teniendo problemas, abra una incidencia de soporte técnico dirigida al [Soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

