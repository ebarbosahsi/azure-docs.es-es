---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/15/2020
ms.author: glenga
ms.openlocfilehash: 4cbf179658ddc13aca9bf30934dc28ab0cf5fd9d
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106081001"
---
| Propiedad | Descripción |
|--|--|
|**`<DESTINATION>`**| Destino al que se envían los registros. Los valores válidos son `AppInsights` y `Blob`.<br/>Cuando use `AppInsights`, asegúrese de que [Application Insights está habilitado en la aplicación de funciones](../articles/azure-functions/configure-monitoring.md#enable-application-insights-integration).<br/>Al establecer el destino en `Blob`, los registros se crean en un contenedor de blobs denominado `azure-functions-scale-controller` en la cuenta de almacenamiento predeterminada establecida en la configuración de la aplicación `AzureWebJobsStorage`. |
|**`<VERBOSITY>`** | Especifica el nivel de registro. Los valores admitidos son `None`, `Warning` y `Verbose`.<br/>Cuando se establece en `Verbose`, el controlador de escala registra una razón para cada cambio en el número de trabajos, así como información sobre los desencadenadores que se aplican a esas decisiones. Los registros detallados incluyen advertencias de desencadenador y los valores hash que usan los desencadenadores antes y después de que se ejecute el controlador de escala. |

> [!TIP]
> Tenga en cuenta que, aunque deje habilitado el registro del controlador de escala, afectará a los [costos potenciales de la supervisión de la aplicación de funciones](../articles/azure-functions/functions-monitoring.md#application-insights-pricing-and-limits). Tenga en cuenta habilitar el registro hasta que haya recopilado suficientes datos para entender cómo se comporta el controlador de escala y, a continuación, deshabilítelo.
