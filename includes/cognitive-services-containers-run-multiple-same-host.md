---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 09aa5affefc576606984ea7116b3d9bda4ba83d5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96000559"
---
### <a name="run-multiple-containers-on-the-same-host"></a>Ejecución de varios contenedores en el mismo host

Si tiene pensado ejecutar varios contenedores con puertos expuestos, asegúrese de que ejecuta cada contenedor con un puerto expuesto diferente. Por ejemplo, ejecute el primer contenedor en el puerto 5000 y el segundo en el puerto 5001.

Puede tener este contenedor y un contenedor de Azure Cognitive Services diferente en ejecución simultáneamente en el HOST. También puede tener varios contenedores del mismo contenedor de Cognitive Services en ejecución.
