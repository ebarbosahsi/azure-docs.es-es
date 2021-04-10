---
title: Obtener el estado de la operación en la traducción de documentos
titleSuffix: Azure Cognitive Services
description: El método Obtener el estado de la operación devuelve el estado de una solicitud de traducción de documentos.
services: cognitive-services
author: jann-skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/25/2021
ms.author: v-jansk
ms.openlocfilehash: e8aa9b93dc089cc05dd5157a7ac84cceb5f6fcd5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105613804"
---
# <a name="document-translation-get-operation-status"></a>Traducción de documentos: Obtener el estado de la operación

El método Obtener el estado de documentos de la operación devuelve el estado de una solicitud de traducción de documentos. El estado incluye el estado general de la solicitud y el estado de los documentos que se traducen como parte de esa solicitud.

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `GET` a:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0-preview.1/batches/{id}
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
|id|Verdadero|El identificador de la operación.|

## <a name="request-headers"></a>Encabezados de solicitud

Los encabezados de solicitud son:

|encabezados|Descripción|
|--- |--- |
|Ocp-Apim-Subscription-Key|Encabezado de solicitud obligatorio|

## <a name="response-status-codes"></a>Códigos de estado de respuesta

A continuación se indican los códigos de estado HTTP posibles que devuelve una solicitud.

|Código de estado|Descripción|
|--- |--- |
|200|Aceptar. Solicitud correcta y devuelve el estado de la operación de traducción por lotes. HeadersRetry-After: integerETag: string|
|401|No autorizado. Compruebe sus credenciales.|
|404|No se encuentra el recurso.|
|500|Error interno del servidor.|
|Otros códigos de estado|<ul><li>Demasiadas solicitudes</li><li>Servidor temporalmente no disponible</li></ul>|

## <a name="get-operation-status-response"></a>Respuesta a Obtener el estado de la operación

### <a name="successful-get-operation-status-response"></a>Respuesta correcta a Obtener el estado de la operación

En una respuesta correcta se devuelve la información siguiente.

|Nombre|Tipo|Descripción|
|--- |--- |--- |
|id|string|Identificador de la operación.|
|createdDateTimeUtc|string|Fecha y hora de creación de la operación.|
|lastActionDateTimeUtc|string|Fecha y hora en que se ha actualizado el estado de la operación.|
|status|String|Lista de posibles estados de un trabajo o documento: <ul><li>Canceled</li><li>Cancelling</li><li>Con error</li><li>NotStarted</li><li>En ejecución</li><li>Correcto</li><li>ValidationFailed</li></ul>|
|Resumen|StatusSummary|Resumen que contiene los detalles que se muestran a continuación.|
|summary.total|integer|Número total.|
|summary.failed|integer|Recuento de elementos con errores.|
|summary.success|integer|Número de elementos correctos.|
|summary.inProgress|integer|Número de elementos en curso.|
|summary.notYetStarted|integer|Recuento de operaciones no iniciadas aún.|
|summary.cancelled|integer|Número de cancelaciones.|
|summary.totalCharacterCharged|integer|Caracteres totales cargados por la API.|

###<a name="error-response"></a>Respuesta de error

|Nombre|Tipo|Descripción|
|--- |--- |--- |
|código|string|Enumeraciones que contiene códigos de error de alto nivel. Valores posibles:<br/><ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>No autorizado</li></ul>|
|message|string|Obtiene un mensaje de error de alto nivel.|
|Destino|string|Obtiene el origen del error. Por ejemplo, sería "documentos" o "id. de documento" para un documentación no válido.|
|innerError|InnerErrorV2|Nuevo formato de error interno, que cumple las directrices de la API de Cognitive Services. Contiene las propiedades requeridas ErrorCode, Message y las propiedades opcionales Target, Details (par clave-valor), error interno (se puede anidar).|
|innerError.code|string|Obtiene la cadena de error de código.|
|innerError.message|string|Obtiene un mensaje de error de alto nivel.|

## <a name="examples"></a>Ejemplos

### <a name="example-successful-response"></a>Ejemplo de respuesta correcta

El siguiente objeto JSON es un ejemplo de una respuesta correcta.

```JSON
{
  "id": "727bf148-f327-47a0-9481-abae6362f11e",
  "createdDateTimeUtc": "2020-03-26T00:00:00Z",
  "lastActionDateTimeUtc": "2020-03-26T01:00:00Z",
  "status": "Succeeded",
  "summary": {
    "total": 10,
    "failed": 1,
    "success": 9,
    "inProgress": 0,
    "notYetStarted": 0,
    "cancelled": 0,
    "totalCharacterCharged": 0
  }
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
