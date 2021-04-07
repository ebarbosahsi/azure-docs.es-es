---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: a5f829fc0e153c3a427fd1c7cc7d5c613cd624f6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "83648250"
---
Azure Functions admite el uso compartido de recursos entre orígenes (CORS). CORS se configura [en el portal](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors) y mediante la [CLI de Azure](/cli/azure/functionapp/cors). La lista de orígenes permitidos de CORS se aplica en el nivel de la aplicación de función. Con CORS habilitado, las respuestas incluyen el encabezado `Access-Control-Allow-Origin`. Para obtener más información, consulte [Uso compartido de recursos entre orígenes](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors). 