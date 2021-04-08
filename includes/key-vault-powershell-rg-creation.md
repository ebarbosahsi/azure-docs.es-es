---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: 872164dfee9f35881fca97b0339bd95c64a2f5ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99070221"
---
Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Use el cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) de Azure PowerShell para crear un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*. 

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```
