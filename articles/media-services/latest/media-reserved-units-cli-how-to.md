---
title: Escalado de unidades reservadas de multimedia (MRU) con CLI
description: En este tema se muestra cómo usar la CLI para escalar el procesamiento multimedia con Azure Media Services.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/22/2021
ms.author: inhenkel
ms.openlocfilehash: c5fa3aa8397ea6e13500717f035c414af8de8e3d
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2021
ms.locfileid: "106121683"
---
# <a name="how-to-scale-media-reserved-units"></a>Escalado de unidades reservadas de multimedia

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

En este artículo se muestra cómo escalar las unidades reservadas de multimedia (MRU) para una codificación más rápida.

> [!WARNING]
> Este comando dejará de funcionar en las cuentas de Media Services que se crearon con la versión 2020-05-01 de la API o con una versión posterior. Para estas cuentas, las unidades reservadas de multimedia ya no son necesarias, ya que el sistema se escalará y reducirá automáticamente en función de la carga. Si no ve la opción para administrar MRU en Azure Portal, es que está usando una cuenta creada con la API 2020-05-01 o posterior.

## <a name="prerequisites"></a>Prerequisites

[Cree una cuenta de Media Services](./account-create-how-to.md).

Conocimientos sobre las [Unidades reservadas de multimedia](concept-media-reserved-units.md).

## <a name="scale-media-reserved-units-with-cli"></a>Escalado de unidades reservadas de multimedia con CLI

Ejecute el comando `mru`.

El comando [mru de la cuenta de ams az](/cli/azure/ams/account/mru) siguiente establece las unidades reservadas de multimedia en cuenta "amsaccount" mediante los parámetros **count** y **type**.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Facturación

Se le cobra en función del número de minutos que se aprovisionan las unidades reservadas de multimedia en su cuenta. Esto ocurre independientemente de si se ejecutan trabajos en la cuenta. Para obtener una explicación detallada, vea la sección de preguntas más frecuentes de la página de [precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Paso siguiente

[Análisis de vídeos](analyze-videos-tutorial.md)

## <a name="see-also"></a>Consulte también

* [Cuotas y límites](limits-quotas-constraints-reference.md)
