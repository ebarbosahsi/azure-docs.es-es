---
title: Precios y SKU de Cognitive Services
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: 8cc4bc6907f83ce062fed82dde17815fc4debd67
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104719920"
---
Consulte la lista de SKU e información sobre precios a continuación. 

#### <a name="multi-service"></a>Varios servicios

| Servicio                    | Clase                      |
|----------------------------|---------------------------|
| Varios servicios Consulte la [página de precios](https://azure.microsoft.com/pricing/details/cognitive-services/)para más información.            | `CognitiveServices`     |


#### <a name="vision"></a>Visión

| Servicio                    | Clase                      |
|----------------------------|---------------------------|
| Computer Vision            | `ComputerVision`          |
| Custom Vision: Prediction | `CustomVision.Prediction` |
| Custom Vision: Training   | `CustomVision.Training`   |
| Caras                       | `Face`                    |
| Form Recognizer            | `FormRecognizer`          |

#### <a name="search"></a>Search

| Servicio            | Clase                  |
|--------------------|-----------------------|
| Bing Autosuggest   | `Bing.Autosuggest.v7` |
| Bing Custom Search | `Bing.CustomSearch`   |
| Bing Entity Search | `Bing.EntitySearch`   |
| Bing Search        | `Bing.Search.v7`      |
| Bing Spell Check   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Voz

| Servicio            | Clase                 |
|--------------------|----------------------|
| Speech Services    | `SpeechServices`     |
| Reconocimiento de voz | `SpeakerRecognition` |

#### <a name="language"></a>Idioma

| Servicio            | Clase                |
|--------------------|---------------------|
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Text Analytics     | `TextAnalytics`     |
| Traducción del texto   | `TextTranslation`   |

#### <a name="decision"></a>Decisión

| Servicio           | Clase               |
|-------------------|--------------------|
| Anomaly Detector  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizer      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Planes de tarifas y facturación

Los planes de tarifas (y la cantidad que se factura) se basan en el número de transacciones que se envían con la información de autenticación. Cada plan de tarifa especifica lo siguiente:
* El número máximo de transacciones permitidas por segundo (TPS).
* Las características de servicio habilitadas en el plan de tarifa.
* El costo de un número predefinido de transacciones. Si se supera este número, se producirá un cargo adicional, tal como se especifica en los [detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) del servicio.

> [!NOTE]
> Muchas de las instancias de Cognitive Services tienen un nivel gratuito que se puede usar para probar el servicio. Para usar el nivel gratuito, utilice `F0` como SKU del recurso.