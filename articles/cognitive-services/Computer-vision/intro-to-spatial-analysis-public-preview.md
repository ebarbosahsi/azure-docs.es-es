---
title: ¿Qué es Spatial Analysis?
titleSuffix: Azure Cognitive Services
description: En este documento se explican los conceptos básicos y las características de un contenedor de Computer Vision Spatial Analysis.
services: cognitive-services
author: nitinme
manager: nitinme
ms.author: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 03/29/2021
ms.openlocfilehash: 5aa34ce15d96112450a7c15debcd92312c1d9e19
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2021
ms.locfileid: "106284549"
---
# <a name="what-is-spatial-analysis"></a>¿Qué es Spatial Analysis?

El servicio Spatial Analysis ayuda a las organizaciones a maximizar el valor de sus espacios físicos mediante el conocimiento de los movimientos y la presencia de personas en un área determinada. Le permite ingerir vídeos de cámaras de CCTV o de vigilancia, ejecutar operaciones de inteligencia artificial para extraer información de las secuencias de vídeo y generar eventos para que los usen otros sistemas. Con la entrada de una transmisión de cámara, una operación de inteligencia artificial puede hacer cosas como contar el número de personas que entran en un sitio o medir el cumplimiento de las directrices de uso de máscaras faciales y distanciamiento físico.

<!--This documentation contains the following types of articles:
* The [quickstarts](./quickstarts-sdk/analyze-image-client-library.md) are step-by-step instructions that let you make calls to the service and get results in a short period of time. 
* The [how-to guides](./Vision-API-How-to-Topics/HowToCallVisionAPI.md) contain instructions for using the service in more specific or customized ways.
* The [conceptual articles](tbd) provide in-depth explanations of the service's functionality and features.
* The [tutorials](./tutorials/storage-lab-tutorial.md) are longer guides that show you how to use this service as a component in broader business solutions.-->

## <a name="what-it-does"></a>Qué hace

Las operaciones principales de Spatial Analysis se basan en una canalización que ingiere vídeo, detecta personas en él, realiza un seguimiento de esas personas cuando aparecen en el vídeo y genera eventos a medida que la gente interactúa con regiones de interés.

## <a name="spatial-analysis-features"></a>Características de Spatial Analysis

| Característica | Definición |
|------|------------|
| **Detección de personas** | Este componente responde a la pregunta, "¿Dónde están las personas en esta imagen?" Busca personas en una imagen y pasa un rectángulo delimitador que indica la ubicación de cada persona al componente de seguimiento. |
| **Seguimiento de personas** | Este componente conecta las detecciones de personas cuando se mueven delante de una cámara. Usa una lógica temporal relativa a la forma en que suelen moverse las personas, e información básica sobre la apariencia general de la gente. No realiza un seguimiento de las personas a lo largo de varias cámaras. Si una persona sale del campo de visión de una cámara durante más de aproximadamente un minuto y luego vuelve a entrar en él, el sistema la percibirá como una nueva persona. El seguimiento de personas no identifica de forma exclusiva a las personas entre las cámaras. No usa el reconocimiento facial ni el seguimiento de la forma de caminar. |
| **Detección de máscaras faciales** | Este componente detecta la ubicación de la cara de una persona en el campo de visión de la cámara e identifica la presencia de una máscara facial. La operación de inteligencia artificial examina las imágenes del vídeo y cuando se detecta una cara, el servicio coloca un rectángulo delimitador alrededor de la cara. Con las funcionalidades de detección de objetos, identifica la presencia de máscaras faciales dentro del cuadro de límite. La detección de máscaras faciales no implica distinguir una cara de otra, predecir ni clasificar atributos faciales ni realizar el reconocimiento facial. |
| **Región de interés** | Es una zona o línea definidas por el usuario en el fotograma de vídeo de entrada. Cuando una persona interactúa con esta región del vídeo, el sistema genera un evento. Por ejemplo, para la operación PersonCrossingLine, se define una línea en el vídeo. Cuando una persona cruza esa línea, se genera un evento. |
| **Evento** | Un evento es la salida principal de Spatial Analysis. Cada operación emite un evento específico de forma periódica (aproximadamente una vez por minuto) o cada vez que se produce un desencadenador concreto. El evento incluye información sobre lo que pasó en el vídeo de entrada, pero no incluye imágenes ni vídeo. Por ejemplo, la operación PeopleCount puede emitir un evento que contenga un recuento actualizado cada vez que el número de personas cambia (es decir, el evento se desencadena) o una vez cada minuto (el evento se realiza periódicamente). |

## <a name="get-started"></a>Introducción

### <a name="public-preview-gating"></a>Acceso a la versión preliminar pública

Para garantizar que Spatial Analysis se usa en los escenarios para los que se diseñó, esta tecnología está a disposición de aquellos clientes que pasen por un proceso de solicitud. Para obtener acceso a Spatial Analysis, deberá empezar por rellenar el formulario de admisión en línea. [Realice aquí la solicitud](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u).

El acceso a la versión preliminar pública de Spatial Analysis está sujeto a la exclusiva discreción de Microsoft en función de los criterios de idoneidad, el proceso de investigación y la disponibilidad para admitir un número limitado de clientes durante esta versión preliminar controlada. Para la versión preliminar pública, se buscan clientes que tengan una relación significativa con Microsoft y que estén interesados en trabajar con nosotros en los casos de uso recomendados y en escenarios adicionales que estén en consonancia con nuestros compromisos de inteligencia artificial.

### <a name="follow-a-quickstart"></a>Consulta de inicios rápidos

Una vez que se le haya concedido acceso a Spatial Analysis, siga el [inicio rápido](spatial-analysis-container.md) para configurar el contenedor y empezar a analizar vídeo.

## <a name="responsible-use-of-spatial-analysis-technology"></a>Uso responsable de la tecnología de análisis espacial

Para aprender a usar la tecnología de análisis espacial de forma responsable, consulte la [nota de transparencia](/legal/cognitive-services/computer-vision/transparency-note-spatial-analysis?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext). Las notas de transparencia de Microsoft están pensadas para ayudarle a entender cómo funciona nuestra tecnología de inteligencia artificial, las elecciones que los propietarios del sistema pueden hacer que influyan en el rendimiento y el comportamiento del sistema y la importancia de pensar en todo el sistema, incluida la tecnología, las personas y el entorno.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Contenedor Spatial Analysis](spatial-analysis-container.md)''''''''''''