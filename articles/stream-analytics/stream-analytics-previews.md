---
title: Características en vista previa (GB) de Azure Stream Analytics
description: En este artículo se enumeran las características de Azure Stream Analytics que están actualmente en versión preliminar.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/16/2021
ms.openlocfilehash: 55745c022038fa85f5b114f2bc347ed7292665eb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104589657"
---
# <a name="azure-stream-analytics-preview-features"></a>Características en vista previa (GB) de Azure Stream Analytics

En este artículo se resumen todas las características actualmente en versión preliminar de Azure Stream Analytics. No se recomienda el uso de características en vista previa en un entorno de producción.

## <a name="authenticate-to-sql-database-output-with-managed-identities-preview"></a>Autenticación en la salida de SQL Database con identidades administradas (versión preliminar)

Azure Stream Analytics admite la [autenticación de identidades administradas](../active-directory/managed-identities-azure-resources/overview.md) para los receptores de salida de Azure SQL Database. Las identidades administradas eliminan las limitaciones de los métodos de autenticación basada en el usuario, como la necesidad de volver a realizar la autenticación debido a los cambios de contraseña. 

## <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Puntuación de alto rendimiento en tiempo real con modelos de ML personalizados y administrados por Azure Machine Learning

Azure Stream Analytics admite puntuaciones en tiempo real y de alto rendimiento aprovechando modelos de Machine Learning entrenados previamente personalizados administrados por Azure Machine Learning y hospedados en Azure Kubernetes Service (AKS) o Azure Container Instances (ACI), mediante un flujo de trabajo que no requiere que escriba código. [Registrarse](https://aka.ms/asapreview1) para la versión preliminar

## <a name="c-custom-de-serializers"></a>Deserializador personalizado de C#
Los desarrolladores pueden aprovechar la eficacia de Azure Stream Analytics para procesar los datos en Protobuf, XML o cualquier formato personalizado. Puede implementar [deserializadores personalizados](custom-deserializer-examples.md) en C# para deserializar los eventos recibidos por Azure Stream Analytics.

## <a name="extensibility-with-c-custom-code"></a>Extensibilidad con código personalizado de C#

Los desarrolladores que crean módulos de Stream Analytics en la nube o en IoT Edge pueden escribir o volver a usar funciones personalizadas de C# e invocarlas directamente en la consulta a través de las [funciones definidas por el usuario](stream-analytics-edge-csharp-udf-methods.md).

## <a name="debug-query-steps-in-visual-studio"></a>Depuración de pasos de consulta en Visual Studio

Puede obtener fácilmente una vista previa del conjunto de filas intermedio en un diagrama de datos al realizar pruebas locales en las herramientas de Azure Stream Analytics para Visual Studio. 


## <a name="live-data-testing-in-visual-studio"></a>Prueba de datos activos en Visual Studio

Las herramientas de Visual Studio para Azure Stream Analytics mejoran la función de pruebas locales que le permite realizar pruebas en las consultas en comparación con los flujos de datos de eventos activos desde fuentes en la nube como un centro de eventos o un centro de IoT. Aprenda cómo realizar una [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-live-data-local-testing.md).

## <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code para Azure Stream Analytics

Los trabajos de Azure Stream Analytics se pueden crear en Visual Studio Code. Consulte el [tutorial de introducción de VS Code](./quick-create-visual-studio-code.md).

## <a name="local-testing-with-live-data-in-visual-studio-code"></a>Consultas de prueba con datos activos en Visual Studio Code

Puede probar las consultas con datos activos en el equipo local antes de enviar el trabajo a Azure. Cada iteración de prueba tarda menos de dos a tres segundos en promedio, lo que produce un proceso de desarrollo muy eficaz.

