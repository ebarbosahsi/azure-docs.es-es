---
title: 'Contenido para adultos y subido de tono: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de contenido para adultos en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5d5961ecae2fbc154ae6f1acd74df2bb74024fa1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "96532625"
---
# <a name="detect-adult-content"></a>Detección de contenido para adultos

Computer Vision puede detectar material para adultos en imágenes para que los programadores puedan restringir la presentación de dichas imágenes en su software. Las marcas de contenido se aplican con una puntuación entre cero y uno para que los desarrolladores puedan interpretar los resultados según sus preferencias.

> [!NOTE]
> Una gran parte de esta funcionalidad está disponible en el servicio [Azure Content Moderator](../content-moderator/overview.md). Consulte esta alternativa para las soluciones en escenarios de moderación de contenido más rigurosas, como la moderación de texto y los flujos de trabajo de revisión humana.

## <a name="content-flag-definitions"></a>Definiciones de las marcas de contenido

La clasificación "adulto" contiene varias categorías diferentes:

- Las imágenes **para adultos** son aquellas de naturaleza explícitamente sexual que suelen representar desnudos y actos sexuales.
- Las imágenes **subidas de tono** son las imágenes de naturaleza sexualmente sugerente y que a menudo contienen material sexual menos explícito que las imágenes etiquetadas como **para adultos**.
- Las imágenes **violentas** muestran sangre o violencia.

## <a name="use-the-api"></a>Uso de la API

Puede detectar contenido para adultos con la API [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b). Al agregar el valor de `Adult` al parámetro de consulta **visualFeatures**, la API devuelve tres propiedades booleanas &mdash;`isAdultContent`, `isRacyContent` y `isGoryContent`&mdash; en su respuesta JSON. El método también devuelve las propiedades correspondientes&mdash;`adultScore`, `racyScore` y `goreScore`&mdash; que representan las puntuaciones de confianza entre cero y uno para cada categoría correspondiente.

- [Inicio rápido: API REST Computer Vision o bibliotecas cliente](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
