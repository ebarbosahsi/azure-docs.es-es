---
title: 'Inicio rápido: Biblioteca cliente o API REST de Form Recognizer'
titleSuffix: Azure Cognitive Services
description: Use la biblioteca cliente o API REST de Form Recognizer con el fin de crear una aplicación de procesamiento de formularios que extraiga pares clave-valor y datos de tabla de los documentos personalizados.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/29/2021
ms.author: lajanuar
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: forms processing, automated data processing
ms.openlocfilehash: 43182892aa80f6226ffb5177d0be2e24d15ead5c
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467233"
---
# <a name="quickstart-use-the-form-recognizer-client-library-or-rest-api"></a>Inicio rápido: Uso de la biblioteca cliente o API REST de Form Recognizer

Comience a usar Form Recognizer mediante el lenguaje de desarrollo de su elección. Azure Form Recognizer es un servicio cognitivo que le permite crear software de procesamiento de datos automatizado mediante tecnología de aprendizaje automático. Identifique y extraiga texto, pares clave-valor, marcas de selección, datos de tabla, etc., de los documentos de formulario; el servicio genera datos estructurados que incluyen las relaciones en el archivo original. Puede usar Form Recognizer a través de la API REST o el SDK. Siga estos pasos para instalar el paquete SDK y probar el código de ejemplo para realizar tareas básicas. 

Use Form Recognizer para:

* [Análisis de diseño](#analyze-layout)
* [Análisis de facturas](#analyze-invoices)
* [Entrenar un modelo personalizado](#train-a-custom-model)
* [Analizar formularios con un modelo personalizado](#analyze-forms-with-a-custom-model)
* [Análisis de las confirmaciones de recepción](#analyze-receipts)
* [Análisis de tarjetas de presentación](#analyze-business-cards)
* [Administrar modelos personalizados](#manage-your-custom-models)

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end
