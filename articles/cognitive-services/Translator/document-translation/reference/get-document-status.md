---
title: Traducción de documentos - Método de obtención del estado de un documento
titleSuffix: Azure Cognitive Services
description: El método de obtención del estado de un documento devuelve el estado de un documento específico.
services: cognitive-services
author: jann-skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/25/2021
ms.author: v-jansk
ms.openlocfilehash: 79bc3d076c1a7e164cab9c3231b29be84370e04a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105613816"
---
# <a name="document-translation-get-document-status"></a>Traducción de documentos: Obtención del estado de un documento

El método de obtención del estado de un documento devuelve el estado de un documento específico. El método devuelve el estado de la traducción de un documento específico en función del identificador de la solicitud y del identificador del documento.

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `GET` a:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0-preview.1/batches/{id}/documents/{documentId}
```

Aprenda a encontrar su [nombre de dominio personalizado](../get-started-with-document-translation.md#find-your-custom-domain-name).

> [!IMPORTANT]
>
> * **Todas las solicitudes de API al servicio de traducción de documentos requieren un punto de conexión de dominio personalizado**.
> * No usará el punto de conexión que se encuentra en la página _Claves y punto de conexión_ del recurso en Azure Portal, ni el punto de conexión global del traductor (`api.cognitive.microsofttranslator.com`) para realizar solicitudes HTTP de traducción de documentos.

## <a name="request-parameters"></a>Parámetros de solicitud

Los parámetros de solicitud que se pasaron en la cadena de consulta son:

|Parámetro de consulta|Obligatorio|Descripción|
|--- |--- |--- |
|documentId|Verdadero|El identificador del documento.|
|id|Verdadero|Id. del lote.|
## <a name="request-headers"></a>Encabezados de solicitud

Los encabezados de solicitud son:

|encabezados|Descripción|
|--- |--- |
|Ocp-Apim-Subscription-Key|Encabezado de solicitud obligatorio|

## <a name="response-status-codes"></a>Códigos de estado de respuesta

A continuación se indican los códigos de estado HTTP posibles que devuelve una solicitud.

|Código de estado|Descripción|
|--- |--- |
|200|Aceptar. Solicitud correcta y aceptada por el servicio. Se devuelven los detalles de la operación. HeadersRetry-After: integerETag: cadena|
|401|No autorizado. Compruebe sus credenciales.|
|404|Not Found. No se encuentra el recurso.|
|500|Error interno del servidor.|
|Otros códigos de estado|<ul><li>Demasiadas solicitudes</li><li>Servidor temporalmente no disponible</li></ul>|

## <a name="get-document-status-response"></a>Respuesta de obtención del estado del documento

### <a name="successful-get-document-status-response"></a>Respuesta correcta de obtención del estado del documento

|Nombre|Tipo|Descripción|
|--- |--- |--- |
|path|string|Ubicación del documento o carpeta.|
|createdDateTimeUtc|string|Fecha y hora de creación de la operación.|
|lastActionDateTimeUtc|string|Fecha y hora en que se actualizó el estado de la operación.|
|status|String|Lista de posibles estados del trabajo o documento: <ul><li>Canceled</li><li>Cancelling</li><li>Con error</li><li>NotStarted</li><li>En ejecución</li><li>Correcto</li><li>ValidationFailed</li></ul>|
|to|string|Código de idioma de dos letras del idioma de destino. Vea la lista de idiomas.|
|progreso|number|Progreso de la traducción, si está disponible.|
|id|string|Identificador del documento.|
|characterCharged|integer|Caracteres cargados por la API.|

### <a name="error-response"></a>Respuesta de error

|Nombre|Tipo|Descripción|
|--- |--- |--- |
|código|string|Enumeraciones que contienen códigos de error generales. Valores posibles:<br/><ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>No autorizado</li></ul>|
|message|string|Obtiene un mensaje de error general.|
|innerError|InnerErrorV2|Nuevo formato de error interno, que cumple las directrices de la API de Cognitive Services. Contiene las propiedades requeridas ErrorCode, mensaje y las propiedades opcionales de destino, detalles (par clave-valor), error interno (puede estar anidado).|
|innerError.code|string|Obtiene la cadena del código de error.|
|innerError.message|string|Obtiene un mensaje de error general.|

## <a name="examples"></a>Ejemplos

### <a name="example-successful-response"></a>Ejemplo de respuesta correcta
El siguiente objeto JSON es un ejemplo de una respuesta correcta.

```JSON
{
  "path": "https://myblob.blob.core.windows.net/destinationContainer/fr/mydoc.txt",
  "createdDateTimeUtc": "2020-03-26T00:00:00Z",
  "lastActionDateTimeUtc": "2020-03-26T01:00:00Z",
  "status": "Running",
  "to": "fr",
  "progress": 0.1,
  "id": "273622bd-835c-4946-9798-fd8f19f6bbf2",
  "characterCharged": 0
}
```

### <a name="example-error-response"></a>Ejemplo de respuesta con error

El siguiente objeto JSON es un ejemplo de una respuesta con error. El esquema de otros códigos de error es el mismo.

Código de estado: 401

```JSON
{
  "error": {
    "code": "Unauthorized",
    "message": "User is not authorized",
    "target": "Document",
    "innerError": {
      "code": "Unauthorized",
      "message": "Operation is not authorized"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Siga nuestro inicio rápido para obtener más información sobre el uso de Traducción de documentos y la biblioteca cliente.

> [!div class="nextstepaction"]
> [Introducción a la traducción de documentos](../get-started-with-document-translation.md)
