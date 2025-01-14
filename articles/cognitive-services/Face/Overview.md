---
title: ¿Qué es el servicio Azure Face?
titleSuffix: Azure Cognitive Services
description: El servicio Azure Face ofrece algoritmos de IA que permiten detectar, reconocer y analizar caras humanas en las imágenes.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 11/23/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: reconocimiento facial, software de reconocimiento facial, análisis facial, coincidencia facial, aplicación de reconocimiento facial, búsqueda de caras por imagen, búsqueda de reconocimiento facial
ms.openlocfilehash: e159ead12179f86406fd7df22475229298f95ee8
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2021
ms.locfileid: "106285474"
---
# <a name="what-is-the-azure-face-service"></a>¿Qué es el servicio Azure Face?

> [!WARNING]
> El 11 de junio de 2020 Microsoft anunció que no venderá tecnología de reconocimiento facial a los departamentos de policía de Estados Unidos hasta que se promulgue un reglamento estricto cimentado en los derechos humanos. Por lo tanto, es posible que los clientes no usen las características o la funcionalidad del reconocimiento facial incluidas en los servicios de Azure, como Face o Video Indexer, si un es un departamento de policía de Estados Unidos o permite el uso de dichos servicios por parte de cualquiera de ellos.

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

El servicio Azure Face ofrece algoritmos de IA que detectan, reconocen y analizan caras humanas en las imágenes. El software de reconocimiento facial es muy importante en muchos escenarios diferentes, como la seguridad, la interfaz de usuario natural, el análisis y la administración de contenido de las imágenes, las aplicaciones móviles y la robótica.

El servicio Face proporciona varias funciones de análisis facial distintas, que se describen en las secciones siguientes.

Esta documentación contiene los siguientes tipos de artículos:
* Los [inicios rápidos](./Quickstarts/client-libraries.md) son instrucciones paso a paso que permiten realizar llamadas al servicio y obtener los resultados en un breve período de tiempo. 
* Las [guías paso a paso](./Face-API-How-to-Topics/HowtoDetectFacesinImage.md) contienen instrucciones para usar el servicio de maneras más específicas o personalizadas.
* Los [artículos conceptuales](./concepts/face-detection.md) proporcionan explicaciones detalladas de la funcionalidad y las características del servicio.
* Los [tutoriales](./Tutorials/FaceAPIinCSharpTutorial.md) son guías más largas que muestran cómo usar este servicio como componente en soluciones empresariales más amplias.

## <a name="face-detection"></a>Detección de caras

El servicio Face detecta rostros humanos en una imagen y devuelve las coordenadas del rectángulo de sus ubicaciones. Si lo desea, la detección de caras puede extraer una serie de atributos faciales, como la posición de la cabeza, el género, la edad, las emociones, el vello facial y las gafas.

> [!NOTE]
> La característica de detección de caras también está disponible a través del [servicio Computer Vision](../computer-vision/overview.md). Sin embargo, si desea realizar más operaciones con datos de caras, debe usar este servicio en su lugar.

![Una imagen de una mujer y un hombre, con rectángulos dibujados en torno a sus caras y edad y sexo.](./Images/Face.detection.jpg)

Para más información sobre la detección de caras, vea el artículo sobre los conceptos de la [Detección de caras](concepts/face-detection.md). Vea también la documentación de referencia de [Detect API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Verificación de caras

Verify API realiza una autenticación con dos caras detectadas o a partir de una cara detectada en un objeto personal. En la práctica, evalúa si dos caras pertenecen a la misma persona. Esta funcionalidad puede ser muy útil en escenarios de seguridad. Para más información, vea la guía de conceptos [Reconocimiento facial](concepts/face-recognition.md) o la documentación de referencia de [Verify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="find-similar-faces"></a>Búsqueda de caras similares

Find Similar API realiza una coincidencia facial entre una cara objetivo y un conjunto de caras candidatas, y busca un conjunto más reducido de caras parecidas a la cara objetivo. Esto resulta útil para realizar una búsqueda de caras por imagen. 

Se admiten dos modos de funcionamiento, **matchPerson** y **matchFace**. El modo **matchPerson** devuelve las caras parecidas tras filtrar por la misma persona mediante [Verify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). El modo **matchFace** ignora el filtro de la misma persona. Devuelve una lista de caras candidatas similares que pueden o no pertenecer a la misma persona.

En el siguiente ejemplo se muestra la cara objetivo:

![Una mujer sonriendo](./Images/FaceFindSimilar.QueryFace.jpg)

Y estas imágenes son las caras candidatas:

![Cinco imágenes de personas sonriendo. Las imágenes a y b muestran a la misma persona.](./Images/FaceFindSimilar.Candidates.jpg)

Al buscar cuatro caras parecidas, el modo **matchPerson** devuelve a y b, que muestran a la misma persona que la cara objetivo. El modo **matchFace** devuelve a, b, c y d: exactamente cuatro caras candidatas, aunque algunas no sean la misma persona que el objetivo o el nivel de similitud sea bajo. Para más información, vea la guía de conceptos [Reconocimiento facial](concepts/face-recognition.md) o la documentación de referencia de [Find Similar API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Agrupación de caras

Group API divide un conjunto de caras desconocidas en varios grupos en función de la similitud. Cada grupo es un subconjunto apropiado separado del conjunto original de caras. Todas las caras de un grupo probablemente pertenecen a la misma persona. Puede haber varios grupos diferentes para una sola persona. Los grupos se diferencian por otro factor, por ejemplo, la expresión. Para más información, vea la guía de conceptos [Reconocimiento facial](concepts/face-recognition.md) o la documentación de referencia de [Group API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="person-identification"></a>Identificación de personas

Identify API se usa para identificar una cara detectada en una base de datos de personas (búsqueda de reconocimiento facial). Esta característica puede resultar útil para el etiquetado automático de imágenes en software de administración de fotografías. Cree la base de datos con antelación y puede modificarla con el tiempo.

La siguiente imagen muestra un ejemplo de una base de datos llamada `"myfriends"`. Cada grupo puede contener hasta un millón de objetos de persona diferentes. Cada objeto de persona puede tener hasta 248 caras registradas.

![Una cuadrícula con tres columnas para diferentes personas, cada una con tres filas de imágenes de caras.](./Images/person.group.clare.jpg)

Una vez creada y entrenada la base de datos, puede realizar la identificación en el grupo con una cara nueva detectada. Si la cara se identifica como una persona en el grupo, se devuelve el objeto de persona.

Para más información sobre la identificación de personas, vea la guía de conceptos [Reconocimiento facial](concepts/face-recognition.md) o la documentación de referencia de [Identify API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="deploy-on-premises-using-docker-containers"></a>Implementación local mediante contenedores de Docker

[Use el contenedor de Face (versión preliminar)](face-how-to-install-containers.md) para implementar características de API locales. Este contenedor de Docker permite acercar el servicio a los datos para mejorar el cumplimiento, la seguridad o por otras razones operativas.

## <a name="sample-apps"></a>Aplicaciones de ejemplo

Las aplicaciones de ejemplo siguientes muestran algunas formas de usar el servicio Face:

- [API Face: biblioteca de cliente Windows y ejemplo](https://github.com/Microsoft/Cognitive-Face-Windows) es una aplicación de WPF que demuestra varios escenarios de detección, análisis e identificación de caras.
- [Aplicación FamilyNotes UWP](https://github.com/Microsoft/Windows-appsample-familynotes) es una aplicación de la Plataforma universal de Windows (UWP) que usa la identificación facial junto con la voz, Cortana, la escritura manuscrita y la cámara en un escenario de uso compartido de notas en familia.

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de datos

Al igual que sucede con todos los recursos de Cognitive Services, los desarrolladores que usan el servicio Face deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, vea la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Siga una guía de inicio rápido para codificar los componentes básicos de una aplicación de reconocimiento facial en el idioma de su elección.

- [Inicio rápido de la biblioteca cliente](quickstarts/client-libraries.md)