---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 9213bdd8e63fc1e8ab7bdba8ac7a569be0dfe6be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93424850"
---
Ejecute el siguiente comando para ver [registros de streaming](../articles/azure-functions/functions-run-local.md#enable-streaming-logs) casi en tiempo real:

```console
func azure functionapp logstream <APP_NAME> 
```

En una ventana de terminal independiente o en el explorador, vuelva a llamar a la función remota. En la ventana de terminal, se mostrará un registro detallado de la ejecución de la función en Azure. 