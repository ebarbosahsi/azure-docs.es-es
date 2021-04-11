---
title: Traducción de documentos - Método de obtención de formatos de documentos
titleSuffix: Azure Cognitive Services
description: El método de obtención de formatos de documentos devuelve una lista de los formatos de documentos admitidos.
services: cognitive-services
author: jann-skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/25/2021
ms.author: v-jansk
ms.openlocfilehash: 3cd74f70416d2049a0fb969371507c292dd47fdf
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105613819"
---
# <a name="document-translation-get-document-formats"></a>Traducción de documentos: Obtención de formatos de documentos

El método de obtención de formatos de documentos devuelve una lista de formatos de documentos admitidos por el servicio Traducción de documentos. La lista incluye la extensión de archivos común y el tipo de contenido si se usa la API de carga.

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `GET` a:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0-preview.1/documents/formats
```

Aprenda a encontrar su [nombre de dominio personalizado](../get-started-with-document-translation.md#find-your-custom-domain-name).

> [!IMPORTANT]
>
> * **Todas las solicitudes de API al servicio de traducción de documentos requieren un punto de conexión de dominio personalizado**.
> * No usará el punto de conexión que se encuentra en la página _Claves y punto de conexión_ del recurso en Azure Portal, ni el punto de conexión global del traductor (`api.cognitive.microsofttranslator.com`) para realizar solicitudes HTTP de traducción de documentos.

## <a name="request-headers"></a>Encabezados de solicitud

Los encabezados de solicitud son:

|encabezados|Descripción|
|-----|-----|
|Ocp-Apim-Subscription-Key|Encabezado de solicitud obligatorio|

## <a name="response-status-codes"></a>Códigos de estado de respuesta

A continuación se indican los códigos de estado HTTP posibles que devuelve una solicitud.

|Código de estado|Descripción|
|-----|-----|
|200|Aceptar. Devuelve la lista de formatos de archivo de documentos admitidos.|
|500|Error interno del servidor.|
|Otros códigos de estado|<ul><li>Demasiadas solicitudes</li><li>Servidor temporalmente no disponible</li></ul>|

## <a name="file-format-response"></a>Respuesta del formato de archivo

### <a name="successful-fileformatlistresult-response"></a>Respuesta correcta de fileFormatListResult

En una respuesta correcta se devuelve la información siguiente.

|Nombre|Tipo|Descripción|
|--- |--- |--- |
|value|FileFormat []|FileFormat[] contiene los detalles siguientes.|
|value.format|string[]|Tipos de contenido admitidos para este formato.|
|value.fileExtensions|string[]|Extensión de archivo admitido para este formato.|
|value.contentTypes|string[]|Nombre del formato.|
|value.versions|String[]|Versión admitida.|

### <a name="error-response"></a>Respuesta de error

|Nombre|Tipo|Descripción|
|--- |--- |--- |
 |código|string|Enumeraciones que contienen códigos de error generales. Valores posibles:<ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>No autorizado</li></ul>|
|message|string|Obtiene un mensaje de error general.|
|innerError|InnerErrorV2|Nuevo formato de error interno, que cumple las directrices de la API de Cognitive Services. Contiene las propiedades requeridas ErrorCode, mensajey las propiedades opcionales de destino, detalles (par clave-valor), error interno (puede estar anidado).|
|innerError.code|string|Obtiene la cadena del código de error.|
|innerError.message|string|Obtiene un mensaje de error general.|

## <a name="examples"></a>Ejemplos

### <a name="example-successful-response"></a>Ejemplo de respuesta correcta
Aquí se muestra un ejemplo de respuesta correcta.

Código de estado: 200

```JSON
{
  "value": [
    {
      "format": "PlainText",
      "fileExtensions": [
        ".txt"
      ],
      "contentTypes": [
        "text/plain"
      ],
      "versions": []
    },
    {
      "format": "PortableDocumentFormat",
      "fileExtensions": [
        ".pdf"
      ],
      "contentTypes": [
        "application/pdf"
      ],
      "versions": []
    },
    {
      "format": "OpenXmlPresentation",
      "fileExtensions": [
        ".pptx"
      ],
      "contentTypes": [
        "application/vnd.openxmlformats-officedocument.presentationml.presentation"
      ],
      "versions": []
    },
    {
      "format": "OpenXmlSpreadsheet",
      "fileExtensions": [
        ".xlsx"
      ],
      "contentTypes": [
        "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
      ],
      "versions": []
    },
    {
      "format": "OutlookMailMessage",
      "fileExtensions": [
        ".msg"
      ],
      "contentTypes": [
        "application/vnd.ms-outlook"
      ],
      "versions": []
    },
    {
      "format": "HtmlFile",
      "fileExtensions": [
        ".html"
      ],
      "contentTypes": [
        "text/html"
      ],
      "versions": []
    },
    {
      "format": "OpenXmlWord",
      "fileExtensions": [
        ".docx"
      ],
      "contentTypes": [
        "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
      ],
      "versions": []
    }
  ]
}
```

### <a name="example-error-response"></a>Ejemplo de respuesta con error

A continuación se presenta un ejemplo de respuesta con error. El esquema de otros códigos de error es el mismo.

Código de estado: 500

```JSON
{
  "error": {
    "code": "InternalServerError",
    "message": "Internal Server Error",
    "innerError": {
      "code": "InternalServerError",
      "message": "Unexpected internal server error has occurred"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Siga nuestro inicio rápido para obtener más información sobre el uso de Traducción de documentos y la biblioteca cliente.

> [!div class="nextstepaction"]
> [Introducción a la traducción de documentos](../get-started-with-document-translation.md)