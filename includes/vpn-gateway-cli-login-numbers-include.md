---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 52f828b9ba7bd286184b44a3d843c2cf8c75c592
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92755332"
---
1. Inicie sesión en la suscripción de Azure con el comando [az login](/cli/azure/) y siga las instrucciones que aparecen en pantalla. Para más información sobre el inicio de sesión, consulte [Introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli).

   ```azurecli
   az login
   ```
2. Si tiene más de una suscripción de Azure, enumere las suscripciones de la cuenta.

   ```azurecli
   az account list --all
   ```
3. Especifique la suscripción que desea usar.

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
