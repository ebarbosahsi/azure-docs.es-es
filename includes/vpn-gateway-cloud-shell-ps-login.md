---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 24d146da7946176c92902698d0f52ae01baf79ee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93061658"
---
Si PowerShell se ejecuta localmente, abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta de Azure. El cmdlet *Connect-AzAccount* le pide las credenciales. Después de la autenticación, descarga la configuración de la cuenta para que esté disponible en Azure PowerShell.

Si usa Azure Cloud Shell, en lugar de ejecutar PowerShell localmente, observará que no es preciso ejecutar *Connect-AzAccount*. Azure Cloud Shell se conecta a su cuenta de Azure automáticamente después de seleccionar **Probar**.

1. Si ejecuta PowerShell localmente, inicie sesión.

   ```azurepowershell
   Connect-AzAccount
   ```

1. Si tiene más de una suscripción, obtenga una lista de las suscripciones de Azure.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```

1. Especifique la suscripción que desea usar.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```
