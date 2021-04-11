---
title: Paso de tokens de autenticación a Azure Media Services v3 | Microsoft Docs
description: Obtenga información sobre cómo enviar tokens de autenticación desde el cliente al servicio de entrega de claves de Azure Media Services v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 590309ad392876ba3c5cb6d21abe777ab5a93efb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105572300"
---
# <a name="pass-tokens-to-the-azure-media-services-v3-key-delivery-service"></a>Paso de tokens al servicio de entrega de claves de Azure Media Services v3.

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Los clientes suelen preguntar cómo un reproductor puede pasar tokens al servicio de entrega de claves de Azure Media Services para la verificación, a fin de que el reproductor pueda obtener la clave. Media Services admite los formatos de token web simple (SWT) y de JSON Web Token (JWT). La autenticación de token puede aplicarse a cualquier tipo de clave, con independencia de que use el cifrado común o el cifrado de sobre Estándar de cifrado avanzado (AES) en el sistema.

 En función del reproductor y de la plataforma de destino, puede pasar el token con su reproductor de las siguientes formas:

## <a name="pass-a-token-through-the-http-authorization-header"></a>Paso de un token a través del encabezado de autorización HTTP

> [!NOTE]
> Las especificaciones de OAuth 2.0 esperan el prefijo "Portador". Para definir el origen de vídeo, elija **AES (token de JWT)** o **AES (token de SWT)** . El token se pasa a través del encabezado de autorización.

## <a name="pass-a-token-via-the-addition-of-a-url-query-parameter-with-tokentokenvalue"></a>Paso de un token mediante la adición de un parámetro de consulta de URL con "token=tokenvalue".

> [!NOTE]
> No se espera el prefijo "Portador". Puesto que el token se envía a través de una dirección URL, debe proteger la cadena de token. Este es un código de ejemplo de C# que muestra cómo hacerlo:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

## <a name="pass-a-token-through-the-customdata-field"></a>Paso de un token a través del campo CustomData

Esta opción se usa para la adquisición de la licencia de PlayReady, mediante el campo CustomData del desafío de adquisición de licencias de PlayReady. En este caso, el token debe estar dentro del documento XML que se describe a continuación:

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```

Coloque el token de autenticación en el elemento Token.
