---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 9f079be76d4d75f05b4a8e132ac425255eed8558
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "88605964"
---
<!--Create a media services asset CLI-->

El siguiente comando de la CLI de Azure crea un nuevo recurso de Media Services. Reemplace los valores `assetName`, `amsAccountName` y `resourceGroup` por los valores con los que esté trabajando actualmente.

```azurecli
az ams asset create -n assetName -a amsAccountName -g resourceGroup
```
