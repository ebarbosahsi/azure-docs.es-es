---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: a2f5264db1f95bcea524a87a61735cf730af23ba
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105645414"
---
## <a name="enable-event-grid-resource-provider"></a>Habilitación del proveedor de recursos de Event Grid

Si aún no ha usado anteriormente Event Grid en su suscripción de Azure, puede que tenga que registrar el proveedor de recursos de Event Grid. Ejecute el comando siguiente:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Puede tardar unos instantes en finalizarse el registro. Para comprobar el estado, ejecute:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Cuando `RegistrationStatus` sea `Registered`, estará preparado para continuar.
