---
title: Preguntas frecuentes sobre Azure Route Server
description: Obtenga respuestas a algunas preguntas frecuentes sobre Azure Route Server.
services: route-server
author: duongau
ms.service: route-server
ms.topic: article
ms.date: 03/08/2021
ms.author: duau
ms.openlocfilehash: 6eed0ed3e936b0e9a534c82a3105c2ed37cab3d5
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102485426"
---
# <a name="azure-route-server-preview-faq"></a>Preguntas frecuentes sobre Azure Route Server (versión preliminar)

> [!IMPORTANT]
> Azure Route Server (versión preliminar) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-is-azure-route-server"></a>¿Qué es Azure Route Server?

Azure Route Server es un servicio totalmente administrado que permite administrar fácilmente el enrutamiento entre la aplicación virtual de red (NVA) y la red virtual.

### <a name="is-azure-route-server-just-a-vm"></a>¿Azure Route Server es solo una máquina virtual?

No. Azure Route Server es un servicio diseñado con alta disponibilidad. Si se implementa en una región de Azure que admite [Availability Zones](../availability-zones/az-overview.md), tendrá redundancia de nivel de zona.

### <a name="what-routing-protocols-does-azure-route-server-support"></a><a name = "protocol"></a>¿Qué protocolos de enrutamiento admite Azure Route Server?

Azure Route Server solo admite Protocolo de puerta de enlace de borde (BGP). La NVA debe admitir BGP externo de salto múltiple porque deberá implementar Azure Route Server en una subred dedicada de la red virtual. El [ASN](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) que elija debe ser diferente del que usa Azure Route Server al configurar BGP en la NVA.

### <a name="does-azure-route-server-route-data-traffic-between-my-nva-and-my-vms"></a>¿Azure Route Server enruta el tráfico de datos entre la NVA y las máquinas virtuales?

No. Azure Route Server solo intercambia rutas BGP con la NVA. El tráfico de datos va directamente desde la NVA a la máquina virtual elegida y directamente desde la máquina virtual a la NVA.

### <a name="if-azure-route-server-receives-the-same-route-from-more-than-one-nva-will-it-program-all-copies-of-the-route-but-each-with-a-different-next-hop-to-the-vms-in-the-virtual-network"></a>Si Azure Route Server recibe la misma ruta de más de una NVA, ¿programará todas las copias de la ruta (pero cada una con un próximo salto diferente) en las máquinas virtuales de la red virtual?

Sí, solo si la ruta tiene la misma longitud de ruta AS. Cuando las máquinas virtuales envían tráfico al destino de esta ruta, los hosts de la máquina virtual realizarán un enrutamiento multidireccional de igual costo (ECMP). Sin embargo, si una NVA envía la ruta con una longitud de ruta AS más corta que la de otras NVA, Azure Route Server solo programará la ruta que tenga el próximo salto establecido en esta NVA en las máquinas virtuales de la red virtual.

### <a name="does-azure-route-server-support-vnet-peering"></a>¿Azure Route Server admite el emparejamiento de VNET?

Sí. Si empareja una red virtual que hospeda la instancia de Azure Route Server con otra red virtual y habilita el uso de la puerta de enlace remota en esa red virtual, Azure Route Server aprenderá los espacios de direcciones de esa red virtual y los enviará a todas las NVA emparejadas.

### <a name="what-autonomous-system-numbers-asns-can-i-use"></a>¿Qué números de sistema autónomo (ASN) puedo usar?

Puede utilizar sus propios ASN públicos o ASN privados en la aplicación virtual de red. No se pueden usar los rangos reservados por Azure o IANA.
Los siguientes ASN están reservados por Azure o IANA:

* ASN reservados por Azure:
    * ASN públicos: 8074, 8075, 12076
    * ASN privados: 65515, 65517, 65518, 65519, 65520
* ASN [reservados por IANA](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml):
    * 23456, 64496-64511, 65535-65551

### <a name="can-i-use-32-bit-4-byte-asns"></a>¿Se pueden usar ASN de 32 bits (4 bytes)?

No, Azure Route Server solo admite ASN de 16 bits (2 bytes).

## <a name="route-server-limits"></a><a name = "limitations"></a>Límites de Route Server

Azure Route Server tiene los límites siguientes (por implementación).

| Resource | Límite |
|----------|-------|
| Número de emparejamientos de BGP admitidos | 8 |
| Número de rutas que puede anunciar cada emparejamiento de BGP a Azure Route Server | 200 |
| Número de rutas que puede anunciar Azure Route Server a ExpressRoute o a la puerta de enlace de VPN | 200 |

Si la NVA anuncia más rutas que el límite, se eliminará la sesión BGP. Si esto sucede en la puerta de enlace y en el servidor de enrutamiento de Azure, perderá la conectividad de la red local a Azure. Para más información, consulte [Diagnóstico de problemas de enrutamiento en una máquina virtual](../virtual-network/diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte [Configuración de una instancia de Azure Route Server](quickstart-configure-route-server-powershell.md).
