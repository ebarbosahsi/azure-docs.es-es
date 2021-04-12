---
title: 'Cómo recortar archivos de vídeo con Media Services: .NET | Microsoft Docs'
description: Recortar es el proceso que consiste en la selección de una ventana rectangular en el fotograma de vídeo y la codificación solo de los píxeles que están dentro de esa ventana. En este tema se muestra cómo recortar archivos de vídeo con Media Services mediante .NET.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/24/2021
ms.author: inhenkel
ms.openlocfilehash: 43d0e1e3b8c0eaa37a3b09557b333c3abafe8b63
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105572259"
---
# <a name="how-to-crop-video-files-with-media-services---net"></a>Cómo recortar archivos de vídeo con Media Services: .NET

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Puede usar Media Services para recortar un vídeo de entrada. Recortar es el proceso que consiste en la selección de una ventana rectangular en el fotograma de vídeo y la codificación solo de los píxeles que están dentro de esa ventana. El diagrama siguiente puede ayudarle a ilustrar el proceso.

## <a name="pre-processing-stage"></a>Fase previa al procesamiento

La opción para recortar es una fase previa al procesamiento y, por ello, los *parámetros de recorte* del valor predeterminado de codificación se aplican al vídeo de *entrada* original. La codificación es una fase posterior y los valores de anchura y altura se aplicarán al vídeo *previamente procesado* y no al vídeo original. Al diseñar el valor preestablecido, haga lo siguiente:

1. Seleccione los parámetros de recorte basados en el vídeo de entrada original.
1. Seleccione la configuración de codificación en función del vídeo recortado.

> [!WARNING]
> Si no coincide la configuración de la codificación con el vídeo recortado, el resultado no será el esperado.

Por ejemplo, suponga que tiene como entrada un vídeo con una resolución de 1920 x 1080 píxeles (relación de aspecto 16:9), pero que tiene barras negras (formato pillarbox) a izquierda y derecha, por lo que sólo una ventana de 4:3 o 1440 x 1080 píxeles contiene el vídeo activo. Puede recortar las barras negras y codificar la región de 1440 x 1080 píxeles.

## <a name="transform-code"></a>Código de la transformación

En el fragmento de código siguiente se muestra cómo escribir una transformación en .NET para recortar vídeos.  En el código se supone que tiene un archivo local con el que trabajar.

- "Izquierda" es la ubicación más a la izquierda del recorte.
- "Superior" es la ubicación superior del recorte.
- "Ancho" es el ancho final del recorte.
- "Alto" es el alto final del recorte.

```dotnet
var preset = new StandardEncoderPreset

    {

        Filters = new Filters

        {                   

            Crop = new Rectangle

            {

                Left = "200",

                Top = "200",

                Width = "1280",

                Height = "720"

            }

        },

        Codecs =

        {

            new AacAudio(),

            new H264Video()

            {

                Layers =

                {                           

                    new H264Layer

                    {

                        Bitrate = 1000000,

                        Width = "1280",

                        Height = "720"

                    }

                }

            }

        },

        Formats =

        {

            new Mp4Format

            {

                FilenamePattern = "{Basename}_{Bitrate}{Extension}"

            }

        }

    }

```
