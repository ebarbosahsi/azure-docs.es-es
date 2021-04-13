---
author: craigshoemaker
ms.service: static-web-apps
ms.topic: include
ms.date: 03/25/2021
ms.author: cshoe
ms.openlocfilehash: 806a30214e0307b8fa101c0de51ed2f16622d433
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105543571"
---
| Propiedad | Descripción | Ejemplo | Obligatorio |
|---|---|---|---|
| `app_location` | Ubicación del código de la aplicación. | Escriba `/` si el código fuente de la aplicación se encuentra en la raíz del repositorio, o `/app` si el código de la aplicación se encuentra en un directorio denominado `app`. | Sí |
| `api_location` | Ubicación del código de Azure Functions. | Escriba `/api` si el código de la aplicación se encuentra en una carpeta denominada `api`. Si no se detecta ninguna aplicación de Azure Functions en la carpeta, no se produce un error en la compilación, sino que el flujo de trabajo supone que no quiere una API. | No |
| `output_location` | Ubicación del directorio de salida de la compilación en relación con `app_location`. | Si el código fuente de la aplicación se encuentra en `/app` y el script de compilación genera archivos de salida en la carpeta `/app/build`, establezca `build` como valor de `output_location`. | No |