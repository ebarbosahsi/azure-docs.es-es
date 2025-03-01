---
title: Implementación de tipos de nodo sin estado en un clúster de Service Fabric
description: Aprenda a crear e implementar tipos de nodo sin estado en el clúster de Azure Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: pepogors
ms.openlocfilehash: eb19005019a6e4e878f6b0bd6a145048d4a2804c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103563783"
---
# <a name="deploy-an-azure-service-fabric-cluster-with-stateless-only-node-types-preview"></a>Implementación de un clúster de Azure Service Fabric con tipos de nodo sin estado (versión preliminar)
Los tipos de nodo de Service Fabric incluyen suposiciones inherentes según las cuales se supone que en algún momento se agregarán servicios con estado en los nodos. Los tipos de nodos sin estado reducen las restricciones de esta suposición en cierto tipo de nodo, lo que permite que ese tipo de nodo use otras características, como operaciones de escalado horizontal más rápidas, compatibilidad con actualizaciones automáticas del sistema operativo en la durabilidad Bronze y un escalado horizontal a más de 100 nodos en un solo conjunto de escalado de máquina virtuales.

* Los tipos de nodo principales no se pueden configurar para que no tengan estado.
* Los tipos de nodo sin estado solo se admiten con los niveles de durabilidad Bronze.
* Los tipos de nodo sin estado solo se admiten en la versión en tiempo de ejecución 7.1.409 o superior de Service Fabric.


Están disponibles plantillas de ejemplo: [Plantilla de tipos de nodos sin estado de Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="enabling-stateless-node-types-in-service-fabric-cluster"></a>Habilitación de tipos de nodo sin estado en el clúster de Service Fabric
Para establecer uno o varios tipos de nodos como nodos sin estado en un recurso de clúster, establezca la propiedad en **isStateless** "true". Cuando implemente un clúster de Service Fabric con tipos de nodo sin estado, recuerde que debe tener al menos un tipo de nodo principal en el recurso de clúster.

* El valor de apiVersion del recurso de clúster de Service Fabric debe ser "2020-12-01-preview" o superior.

```json
{
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "isStateless": false,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Bronze",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": false,
        "isStateless": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    }    
    ],
}
```

## <a name="configuring-virtual-machine-scale-set-for-stateless-node-types"></a>Configuración del conjunto de escalado de máquinas virtuales para tipos de nodos sin estado
Para habilitar los tipos de nodo sin estado, debe configurar el recurso del conjunto de escalado de máquinas virtuales subyacente de la siguiente manera:

* El valor de la propiedad **singlePlacementGroup** debe establecerse en **false** si debe escalar más de 100 máquinas virtuales.
* El valor **upgradePolicy** del conjunto de escalado cuyo valor de **mode** debe establecerse en **Rolling**.
* El modo de actualización gradual requiere la configuración de la extensión de mantenimiento de la aplicación o de los sondeos de estado. Configure el sondeo de estado con la configuración predeterminada para los tipos de nodo sin estado, tal como se sugiere a continuación. Una vez que las aplicaciones se implementan en el tipo de nodo, los puertos de la extensión de mantenimiento o del sondeo de estado se pueden cambiar para supervisar el estado de la aplicación.

>[!NOTE]
> Es necesario que el número de dominios de error de plataforma se actualice a 5 cuando un tipo de nodo sin estado esté respaldado por un conjunto de escalado de máquinas virtuales que esté abarcando varias zonas. Consulte esta [plantilla](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/15-VM-2-NodeTypes-Windows-Stateless-CrossAZ-Secure) para obtener más información.
> 
> **platformFaultDomainCount:5**
```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "properties": {
        "overprovision": "[variables('overProvision')]",
        "upgradePolicy": {
          "mode": "Rolling",
          "automaticOSUpgradePolicy": {
            "enableAutomaticOSUpgrade": true
          }
        }
    }
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Bronze",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
        },
        "typeHandlerVersion": "1.0"
    }
    },
    {
        "type": "extensions",
        "name": "HealthExtension",
        "properties": {
            "publisher": "Microsoft.ManagedServices",
            "type": "ApplicationHealthWindows",
            "autoUpgradeMinorVersion": true,
            "typeHandlerVersion": "1.0",
            "settings": {
            "protocol": "tcp",
            "port": "19000"
            }
            }
        },
    ]
}
```

## <a name="networking-requirements"></a>Requisitos de red
### <a name="public-ip-and-load-balancer-resource"></a>Recurso de IP pública y Load Balancer
Para habilitar el escalado a más de 100 VM en un recurso del conjunto de escalado de máquinas virtuales, los recursos de Load Balancer y de IP a los que hace referencia ese conjunto de escalado de máquinas virtuales deben usar una SKU *estándar*. La creación de un recurso de Load Balancer o de IP sin la propiedad SKU creará una SKU básica, que no admite el escalado a más de 100 VM. Una instancia de Load Balancer de SKU estándar bloqueará todo el tráfico procedente del exterior de forma predeterminada; para permitir el tráfico exterior, debe implementarse un NSG en la subred.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> No es posible hacer un cambio en contexto de SKU en los recursos de IP pública y de Load Balancer. Si va a migrar desde recursos existentes que tienen una SKU básica, consulte la sección sobre la migración de este artículo.

### <a name="virtual-machine-scale-set-nat-rules"></a>Reglas NAT del conjunto de escalado de máquinas virtuales
Las reglas NAT de entrada de Load Balancer deben coincidir con los grupos NAT del conjunto de escalado de máquinas virtuales. Cada conjunto de escalado de máquinas virtuales debe tener un único grupo NAT de entrada.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Reglas de salida de Load Balancer de SKU estándar
Standard Load Balancer y la IP pública estándar presentan capacidades nuevas y comportamientos diferentes en la conectividad de salida en comparación con el uso de SKU básicas. Si quiere conectividad saliente al trabajar con las SKU de nivel Estándar, debe definirlas con las direcciones IP públicas estándar o con la instancia pública de Load Balancer estándar. Para más información, consulte [Conexiones de salida](../load-balancer/load-balancer-outbound-connections.md) y [Azure Standard Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
> La plantilla estándar hace referencia a un NSG que permite todo el tráfico de salida de forma predeterminada. El tráfico de entrada se limita a los puertos necesarios para las operaciones de administración de Service Fabric. Las reglas de NSG pueden modificarse para satisfacer sus requisitos.

>[!NOTE]
> Cualquier clúster de Service Fabric que haga uso de la SKU estándar para SLB deberá asegurarse de que cada tipo de nodo tiene una regla que permite el tráfico saliente en el puerto 443. Esto es necesario para completar la configuración del clúster, produciéndose un error en cualquier implementación sin esta regla.



### <a name="migrate-to-using-stateless-node-types-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Migración para usar los tipos de nodo sin estado desde un clúster mediante la instancia de Load Balancer y una dirección IP de una SKU básica
En todos los escenarios de migración es necesario agregar un nuevo tipo de nodo sin estado. Tenga en cuenta que el tipo de nodo existente no se puede migrar para que sea sin estado.

Para migrar un clúster, que estaba usando una instancia de Load Balancer y una IP con una SKU básica, primero debe crear un recurso de Load Balancer e IP completamente nuevo mediante la SKU estándar. No es posible actualizar estos recursos en contexto.

Los nuevos recursos de LB e IP se deben mencionar en los nuevos tipos de nodo sin estado que quiera usar. En el ejemplo anterior, se agregan nuevos recursos del conjunto de escalado de máquinas virtuales que se usarán para los tipos de nodo sin estado. Estos conjuntos de escalado de máquinas virtuales hacen referencia a los recursos de LB e IP recién creados y están marcados como tipos de nodo sin estado en el recurso de clúster de Service Fabric.

Para empezar, deberá agregar los nuevos recursos a la plantilla de Resource Manager existente. Estos recursos incluyen:
* Un recurso de IP pública mediante la SKU estándar.
* Un recurso de Load Balancer mediante la SKU estándar.
* Un NSG al que hace referencia la subred en la que implementa los conjuntos de escalado de máquinas virtuales.

Una vez haya terminado la implementación de los recursos, puede empezar a deshabilitar los nodos en el tipo de nodo que quiera quitar del clúster original.

>[!NOTE]
> Al usar el escalado automático con tipos de nodos sin estado con durabilidad Bronze, después de la operación de reducción vertical, el estado del nodo no se limpia automáticamente. Para limpiar el estado del nodo de los nodos inactivos durante la escalabilidad automática, se recomienda usar la [aplicación auxiliar de escalabilidad automática de Service Fabric](https://github.com/Azure/service-fabric-autoscale-helper).

## <a name="next-steps"></a>Pasos siguientes 
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Tipos de nodo y conjuntos de escalado de máquinas virtuales](service-fabric-cluster-nodetypes.md)

