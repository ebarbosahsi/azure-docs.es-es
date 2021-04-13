---
title: archivo de inclusión
description: archivo de inclusión
services: container-service
author: mlearned
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: mlearned
ms.custom: include file
ms.openlocfilehash: 1c2dec106ae72ddead7bda54792fa74e38eb6660
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106081006"
---
| Recurso                                                                                                           | Límite                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Número máximo de clústeres por suscripción                                                                                  | 1000                                                                                                                                                                                                        |
| Número máximo de nodos por clúster con los conjuntos de disponibilidad de máquina virtual y el SKU básico de Load Balancer                       | 100                                                                                                                                                                                                         |
| Número máximo de nodos por clúster con Virtual Machine Scale Sets y el [SKU estándar de Load Balancer][standard-load-balancer] | 1000 (100 nodos por [grupo de nodos][node-pool])                                                                                                                                                                 |
| Número máximo de pods por nodo: [redes básicas][basic-networking] con Kubenet                                           | 110                                                                                                                                                                                                         |
| Número máximo de pods por nodo: [redes avanzadas][advanced-networking] con Azure Container Networking Interface        | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Azure Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30                                                                                        |
| Versión preliminar del complemento Open Service Mesh (OSM) AKS                                                                          | Versión del clúster de Kubernetes: 1.19+<sup>2</sup><br />Controladores OSM por clúster: 1<sup>2</sup><br />Pods por controlador OSM: 500<sup>2</sup><br />Cuentas de servicio de Kubernetes administradas por OSM: 50<sup>2</sup> |

<sup>1</sup>Cuando implementa un clúster de Azure Kubernetes Service (AKS) con la CLI de Azure o una plantilla de Resource Manager, este valor es configurable en hasta 250 pods por nodo. No puede configurar un número máximo de pods por nodo después de haber implementado un clúster AKS, o si implementa un clúster mediante Azure Portal.<br />

<sup>2</sup> El complemento OSM para AKS se encuentra en estado de versión preliminar y se someterá a mejoras adicionales antes de estar disponible con carácter general. Durante la fase de versión preliminar, se recomienda no superar los límites indicados.<br />

<!-- LINKS - Internal -->

[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->

[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
