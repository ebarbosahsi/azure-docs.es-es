---
title: Creación de consultas semánticas
titleSuffix: Azure Cognitive Search
description: Establezca un tipo de consulta semántica para asociar los modelos de aprendizaje profundo al procesamiento de consultas, y así inferir la intención y el contexto dentro de la clasificación y la relevancia de la búsqueda.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: c33739124092a17acf0590f00b2f9c3c09bf894e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104654669"
---
# <a name="create-a-query-for-semantic-captions-in-cognitive-search"></a>Creación de una consulta para leyendas semánticas en Cognitive Search

> [!IMPORTANT]
> La búsqueda semántica se encuentra en versión preliminar pública y solo está disponible mediante la API REST en versión preliminar y Azure Portal. Las características en vista previa (GB) se ofrecen tal cual, en [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Estas características son facturables. Para más información, consulte [Disponibilidad y precios](semantic-search-overview.md#availability-and-pricing).

En este artículo, aprenderá a formular una solicitud de búsqueda que usa la clasificación semántica y devuelve leyendas semánticas (y, opcionalmente, [respuestas semánticas](semantic-answers.md)), con resaltado en los términos y frases más pertinentes. Las leyendas y las respuestas se devuelven en las consultas formuladas con el tipo de consulta "semantic".

Las leyendas y las respuestas se extraen literalmente del texto del documento de búsqueda. El subsistema semántico determina qué parte del contenido tiene las características de una leyenda o respuesta, pero no crea oraciones ni frases. Por esta razón, el contenido que incluye explicaciones o definiciones funciona mejor para la búsqueda semántica.

## <a name="prerequisites"></a>Prerrequisitos

+ Un servicio de búsqueda en un nivel Estándar (S1, S2, S3), ubicado en una de estas regiones: Centro-norte de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Este de EE. UU. 2, Europa del Norte y Oeste de Europa. Si tiene un servicio S1 o superior en una de estas regiones, puede solicitar acceso sin tener que crear un servicio.

+ Acceso a la versión preliminar de la búsqueda semántica: [registro](https://aka.ms/SemanticSearchPreviewSignup)

+ Un índice de búsqueda existente con contenido en inglés

+ Un cliente de búsqueda para el envío de consultas

  El cliente de búsqueda debe admitir las API REST en versión preliminar en la solicitud de consulta. Puede usar [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) o código que realice llamadas REST a las API en versión preliminar. También puede usar el [Explorador de búsqueda](search-explorer.md) de Azure Portal para enviar consultas semánticas.

+ Una [solicitud de consulta](/rest/api/searchservice/preview-api/search-documents) debe incluir la opción semántica y otros parámetros descritos en este artículo.

## <a name="whats-a-semantic-query"></a>¿Qué es una consulta semántica?

En Cognitive Search, una consulta es una solicitud parametrizada que determina el procesamiento de las consultas y la forma de las respuestas. Una *consulta semántica* agrega parámetros que invocan el modelo de reclasificación semántica. Este algoritmo puede evaluar el contexto y el significado de los resultados coincidentes, promover los mejores a los primeros puestos, y devolver respuestas semánticas y subtítulos.

La solicitud siguiente es representativa de una consulta semántica mínima (sin respuestas).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us"  
}
```

Como sucede con todas las consultas de Cognitive Search, la solicitud va dirigida a la colección de documentos de un índice único. Además, las consultas semánticas pasan por la misma secuencia de análisis, examen y puntuación que una consulta no semántica. 

La diferencia radica en la relevancia y la puntuación. Tal como se define en esta versión preliminar, una consulta semántica es aquella cuyos *resultados* se vuelven a clasificar mediante un modelo de lenguaje semántico, lo que proporciona una manera de mostrar las coincidencias que el clasificador semántico considera de más importancia, en lugar de las puntuaciones que asigna el algoritmo predeterminado de clasificación de similitudes.

Solo las 50 primeras coincidencias de los resultados iniciales se pueden clasificar semánticamente y todas incluyen subtítulos en la respuesta. Opcionalmente, puede especificar un parámetro **`answer`** en la solicitud para extraer una posible respuesta. Para obtener más información, vea [Respuestas semánticas](semantic-answers.md).

## <a name="query-with-search-explorer"></a>Consulta con el Explorador de búsqueda

El [Explorador de búsqueda](search-explorer.md) se ha actualizado para incluir las opciones de las consultas semánticas. Estas opciones se vuelven visibles en el portal después de completar los pasos siguientes:

1. [Regístrese](https://aka.ms/SemanticSearchPreviewSignup) y logre la admisión del servicio de búsqueda en el programa de versión preliminar.

1. Abra el portal con esta sintaxis: `https://portal.azure.com/?feature.semanticSearch=true`.

Las opciones de consulta incluyen modificadores para habilitar consultas semánticas, searchFields y corrección ortográfica. También puede pegar los parámetros de consulta necesarios en la cadena de consulta.

:::image type="content" source="./media/semantic-search-overview/search-explorer-semantic-query-options.png" alt-text="Opciones de consulta en el Explorador de búsqueda" border="true":::

## <a name="query-using-rest"></a>Consulta mediante REST

Use [Buscar en documentos (versión preliminar REST)](/rest/api/searchservice/preview-api/search-documents) para formular la solicitud mediante programación.

Una respuesta incluye subtítulos y resaltado de forma automática. Si quiere que en la respuesta se incluya la corrección ortográfica o respuestas, agregue un parámetro **`speller`** o **`answers`** opcional en la solicitud.

En el ejemplo siguiente se usa hotels-sample-index para crear una solicitud de consulta semántica con respuestas y subtítulos semánticos:

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview      
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Category,Description",
    "speller": "lexicon",
    "answers": "extractive|count-3",
    "highlightPreTag": "<strong>",
    "highlightPostTag": "</strong>",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

En la tabla siguiente se resumen los parámetros de consulta que se usan en una consulta semántica para que pueda verlos holísticamente. Puede encontrar una lista de todos los parámetros en [Search Documents (versión preliminar de REST)](/rest/api/searchservice/preview-api/search-documents).

| Parámetro | Tipo | Descripción |
|-----------|-------|-------------|
| queryType | String | Los valores válidos son simple, full y semantic. Para las consultas semánticas, se requiere un valor de "semantic". |
| queryLanguage | String | Necesario para las consultas semánticas. Actualmente, solo está implementado "en-us". |
| searchFields | String | Una lista delimitada por comas de campos que permiten búsqueda. Especifica los campos en los que se produce la clasificación semántica, de los que se extraen las leyendas y las respuestas. </br></br>A diferencia de los tipos de consulta simple y completa, el orden en que se muestran los campos determina la precedencia. Para ver más instrucciones de uso, vea [Paso 2: Establecer searchFields](#searchfields). |
| Corrector ortográfico | String | Parámetro opcional, no específico de las consultas semánticas, que corrige términos mal escritos antes de que lleguen al motor de búsqueda. Para obtener más información, vea [Agregar corrección ortográfica a las consultas](speller-how-to-add.md). |
| answers |String | Parámetros opcionales que especifican si el resultado incluye las respuestas semánticas. Actualmente, solo está implementado "extractive". Las respuestas se pueden configurar para que se devuelvan cinco como máximo. El valor predeterminado es uno. En este ejemplo se muestra un recuento de tres respuestas: "extractive\|count3". Para obtener más información, consulte [Devolución de respuestas semánticas](semantic-answers.md).|

### <a name="formulate-the-request"></a>Formulación de la solicitud

En esta sección se describen los parámetros de consulta necesarios para la búsqueda semántica.

#### <a name="step-1-set-querytype-and-querylanguage"></a>Paso 1: Establecer queryType y queryLanguage

Agregue los siguientes parámetros al resto. Ambos parámetros son obligatorios.

```json
"queryType": "semantic",
"queryLanguage": "en-us",
```

El parámetro queryLanguage debe ser coherente con los [analizadores del lenguaje](index-add-language-analyzers.md) asignados a las definiciones de campo en el esquema de índice. Si queryLanguage es "en-us", todos los analizadores del lenguaje también deben tener una variante inglesa ("en.microsoft" o "en.lucene"). Los analizadores independientes del lenguaje, como Keyword o Simple, no tienen ningún conflicto con los valores de queryLanguage.

En una solicitud de consulta, si usa también la [corrección ortográfica](speller-how-to-add.md), el valor de queryLanguage que establezca se aplica por igual al corrector ortográfico, a las respuestas y a los subtítulos. No hay invalidaciones para partes individuales. 

Aunque el contenido de un índice de búsqueda puede estar redactado en varios idiomas, es más probable que la entrada de la consulta lo esté en uno. El motor de búsqueda no comprueba la compatibilidad de queryLanguage, el analizador del lenguaje ni el idioma en que está redactado el contenido, por lo que debe asegurarse de establecer el ámbito de las consultas en consecuencia para evitar que se generen resultados incorrectos.

<a name="searchfields"></a>

#### <a name="step-2-set-searchfields"></a>Paso 2: Establecer searchFields

El parámetro searchFields se usa para identificar los pasajes que se van a evaluar para determinar su "similitud semántica" con la consulta. En el caso de la versión preliminar, no se recomienda dejar searchFields en blanco, ya que el modelo requiere una sugerencia sobre qué campos son más importantes para procesar.

El orden de searchFields es crítico. Si ya usa searchFields en el código de las consultas de Lucene simples o completas existentes, vuelva a visitar este parámetro para comprobar el orden de los campos al cambiar a un tipo de consulta semántica.

En el caso de dos o más searchFields:

+ Incluya solo campos de cadena y campos de cadena de nivel superior en las colecciones. Si se incluyen campos que no son de cadena o campos de nivel inferior en una colección, no se produce ningún error, pero esos campos no se usarán en la clasificación semántica.

+ El primer campo siempre debe ser conciso (por ejemplo, un título o un nombre). Lo ideal es que tenga menos de 25 palabras.

+ Si el índice tiene un campo de dirección URL que es textual (legible por el usuario, como `www.domain.com/name-of-the-document-and-other-details`, y no enfocado a la máquina, como `www.domain.com/?id=23463&param=eis`), colóquelo en segundo lugar en la lista (o en primer lugar si no hay ningún campo de título conciso).

+ Siga esos campos por los campos descriptivos en los que se pueda encontrar la respuesta a las consultas semánticas, como el contenido principal de un documento.

Si solo se especifica un campo, use un campo descriptivo en el que se pueda encontrar la respuesta a las consultas semánticas, como el contenido principal de un documento. 

#### <a name="step-3-remove-orderby-clauses"></a>Paso 3: Quitar las cláusulas orderBy

Quite todas las cláusulas orderBy, si las hay en una solicitud existente. La puntuación semántica se usa para ordenar los resultados y, si incluye la lógica de ordenación explícita, se devuelve un error HTTP 400.

#### <a name="step-4-add-answers"></a>Paso 4: Agregar respuestas

De manera opcional, agregue "respuestas" si quiere incluir un procesamiento adicional que proporcione una respuesta. Las respuestas (y los subtítulos) se extraen a partir de los pasajes que se encuentran en los campos enumerados en searchFields. Asegúrese de incluir campos con abundante contenido en searchFields para obtener las mejores respuestas en una respuesta. Para obtener más información, consulte [Devolución de respuestas semánticas](semantic-answers.md).

#### <a name="step-5-add-other-parameters"></a>Paso 5: Agregar otros parámetros

Especifique cualquier otro parámetro que quiera incluir en la solicitud. Parámetros, como [speller](speller-how-to-add.md), [select](search-query-odata-select.md) y count mejoran la calidad de la solicitud y la legibilidad de la respuesta.

También tiene la opción de personalizar el estilo de resaltado que se aplica a los subtítulos. Los subtítulos aplican el formato de resaltado sobre los pasajes principales del documento que resumen la respuesta. El valor predeterminado es `<em>`. Si quiere especificar el tipo de formato (por ejemplo, fondo amarillo), puede establecer los parámetros highlightPreTag y highlightPostTag.

## <a name="evaluate-the-response"></a>Evaluación de la respuesta

Al igual que con todas las consultas, una respuesta consta de todos los campos marcados como recuperables, o solo de los campos enumerados en el parámetro SELECT. Incluye la puntuación de relevancia original y también puede incluir un recuento o resultados por lotes, en función de cómo haya formulado la solicitud.

En una consulta semántica, la respuesta tiene elementos adicionales: una nueva puntuación de relevancia clasificada semánticamente, subtítulos en texto sin formato y con resaltados y, opcionalmente, una respuesta.

En una aplicación cliente puede estructurar la página de búsqueda para incluir un subtítulo como la descripción de la coincidencia, en lugar de todo el contenido de un campo específico. Esto resulta útil cuando los campos individuales son demasiado densos para la página resultados de la búsqueda.

La respuesta de la consulta de ejemplo anterior devuelve la siguiente coincidencia como elección principal. Los subtítulos se devuelven automáticamente, con texto sin formato y versiones resaltadas. Las respuestas se omiten en el ejemplo porque no se pudo determinar ninguna para esta consulta y corpus en particular.

```json
"@odata.count": 35,
"@search.answers": [],
"value": [
    {
        "@search.score": 1.8810667,
        "@search.rerankerScore": 1.1446577133610845,
        "@search.captions": [
            {
                "text": "Oceanside Resort. Luxury. New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
                "highlights": "<strong>Oceanside Resort.</strong> Luxury. New Luxury Hotel. Be the first to stay.<strong> Bay</strong> views from every room, location near the pier, rooftop pool, waterfront dining & more."
            }
        ],
        "HotelName": "Oceanside Resort",
        "Description": "New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
        "Category": "Luxury"
    },
```

## <a name="next-steps"></a>Pasos siguientes

Recuerde que la clasificación y las respuestas semánticas se crean sobre un conjunto de resultados inicial. Cualquier lógica que mejore la calidad de los resultados iniciales hará avanzar la búsqueda semántica. El paso siguiente sería revisar las características que contribuyen a los resultados iniciales, como los analizadores que afectan al modo en que se acortan las cadenas, los perfiles de puntuación que pueden optimizar los resultados y el algoritmo de relevancia predeterminado.

+ [Analizadores para el procesamiento de texto](search-analyzers.md)
+ [Algoritmo de clasificación de similitud](index-similarity-and-scoring.md)
+ [Perfiles de puntuación](index-add-scoring-profiles.md)
+ [Introducción a la búsqueda semántica](semantic-search-overview.md)
+ [Algoritmo de clasificación semántica](semantic-ranking.md)
