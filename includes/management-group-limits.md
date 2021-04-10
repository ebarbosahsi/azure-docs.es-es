---
title: archivo de inclusión
description: archivo de inclusión
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: cdcf6215973755444da9e513761de7ac71e479d4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98738930"
---
| Recurso | Límite |
| --- | --- |
| Grupos de administración por inquilino de Azure AD | 10 000 |
| Suscripciones por grupo de administración | Sin límite. |
| Niveles de jerarquía de grupos de administración | Nivel raíz más 6 niveles<sup>1</sup> |
| Grupo de administración primario directo por grupo de administración | Uno |
| [Implementaciones de nivel de grupo de administración](../articles/azure-resource-manager/templates/deploy-to-management-group.md) por ubicación | 800<sup>2</sup> |

<sup>1</sup>Los 6 niveles no incluyen el nivel de suscripción.

<sup>2</sup>Si se alcanza el límite de 800 implementaciones, elimine las implementaciones que ya no necesite del historial. Para eliminar las implementaciones de nivel de grupo de administración, use [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) o [az deployment mg delete](/cli/azure/deployment/mg#az-deployment-mg-delete).
