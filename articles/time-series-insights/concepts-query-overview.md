---
title: 'Consulta de datos: Azure Time Series Insights Gen2 | Microsoft Docs'
description: Información general sobre los conceptos de consulta de datos y la API REST de Azure Time Series Insights Gen2.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/22/2021
ms.custom: seodec18
ms.openlocfilehash: b1b055fa7f083bd8bccda16498e2894d5d67eace
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100374140"
---
# <a name="querying-data-from-azure-time-series-insights-gen2"></a>Consulta de datos de Azure Time Series Insights Gen2

Azure Time Series Insights Gen2 permite consultar datos en los eventos y los metadatos almacenados en el entorno a través de las API de superficie públicas. Estas API también se usan en el [Explorador de Azure Time Series Insights TSI](./concepts-ux-panels.md).

Hay tres categorías principales de API en Azure Time Series Insights Gen2:

* **API de entorno**: estas API permiten consultas en el propio entorno de Azure Time Series Insights Gen2. Se pueden usar para recopilar una lista de los entornos a los que el autor de la llamada tiene acceso y los metadatos del entorno.
* **API de consulta de modelo de serie temporal (TSM-Q)** : permite crear, leer, actualizar y eliminar (CRUD) operaciones en metadatos almacenados en el modelo de serie temporal del entorno. Se pueden usar para acceder a las instancias, los tipos y las jerarquías, así como editarlos.
* **API de consulta de serie temporal (TSQ)** : permite la recuperación de datos de telemetría o eventos a medida que se registran desde el proveedor de origen, así como realizar cálculos y agregaciones de rendimiento de los datos usando funciones escalares y de agregado avanzadas.

Azure Time Series Insights Gen2 usa un lenguaje de expresiones enriquecido basado en cadenas, [Time Series Expression (TSX)](/rest/api/time-series-insights/reference-time-series-expression-syntax), para expresar los cálculos de [variables de serie temporal](./concepts-variables.md).

## <a name="azure-time-series-insights-gen2-apis-overview"></a>Introducción a las API de Azure Time Series Insights Gen2

Se admiten los siguientes tipos de API principal.

[![Información general sobre la consulta de serie temporal](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>API de entorno

* [Get Environments API](/rest/api/time-series-insights/management(gen1/gen2)/accesspolicies/listbyenvironment): Devuelve la lista de entornos a los que el autor de la llamada puede obtener acceso.
* [Get Environments Availability API](/rest/api/time-series-insights/dataaccessgen2/query/getavailability): Devuelve la distribución del recuento de eventos a través de la marca de tiempo `$ts` del evento. Esta API ayuda a determinar si hay eventos en el entorno, devolviendo para ello el recuento de eventos (si los hay) desglosado en intervalos.
* [Get Event Schema API](/rest/api/time-series-insights/dataaccessgen2/query/geteventschema): Devuelve los metadatos de esquema de eventos para un intervalo de búsqueda determinado. Esta API le ayuda a recuperar todos los metadatos y propiedades disponibles en el esquema para el intervalo de búsqueda determinado.

## <a name="time-series-model-query-tsm-q-apis"></a>API de consulta de modelo de serie temporal (TSM-Q)

La mayoría de estas API admite la operación de ejecución por lotes para permitir operaciones CRUD de lote en varias entidades de modelo de serie temporal:

* [Model Settings API](/rest/api/time-series-insights/reference-model-apis): permite operaciones *GET* y *PATCH* en el tipo predeterminado y el nombre del modelo del entorno.
* [Types API](/rest/api/time-series-insights/reference-model-apis#types-api): Habilita CRUD en tipos de serie temporal y en sus variables asociadas.
* [Hierarchies API](/rest/api/time-series-insights/reference-model-apis#hierarchies-api): Habilita CRUD en jerarquías de serie temporal y en sus variables asociadas.
* [Instances API](/rest/api/time-series-insights/reference-model-apis#instances-api): Habilita CRUD en jerarquías de serie temporal y en sus campos de instancias asociados. Además, Instances API admite las siguientes operaciones:
  * [Buscar](/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/search): recupera una lista parcial de aciertos en la búsqueda de instancias de serie temporal según los atributos de instancia.
  * [Sugerir](/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/suggest): busca y sugiere una lista parcial de aciertos en la búsqueda de instancias de serie temporal según los atributos de instancia.

## <a name="time-series-query-tsq-apis"></a>API de consulta de serie temporal (TSQ)

Estas API están disponibles en ambos almacenes (almacenamiento intermedio y en frío) en nuestra solución de almacenamiento multicapa. 

* [Get Events API](/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents): Permite la consulta y recuperación de los datos sin procesar y las marcas de tiempo de evento correspondientes que se registran en la instancia de Azure Time Series Insights Gen2 del proveedor de origen. Esta API permite la recuperación de eventos sin procesar según un intervalo de búsqueda y un identificador de serie temporal dados. Esta API admite la paginación para recuperar el conjunto de datos de respuesta completo de la entrada seleccionada.

  > [!IMPORTANT]
  > Como parte de los [próximos cambios en las reglas de aplanamiento y escape de JSON](./ingestion-rules-update.md), las matrices se almacenarán como tipo **dinámico**. Las propiedades de carga almacenadas como este tipo **solo serán accesibles a través de Get Events API**.

* [Get Series API](/rest/api/time-series-insights/dataaccessgen2/query/execute#getseries): Permite la consulta y recuperación de los valores calculados y las marcas de tiempo de evento correspondientes, aplicando cálculos definidos por variables en los eventos sin procesar. Estas variables se pueden definir en el modelo de serie temporal o proporcionarse en línea en la consulta. Esta API admite la paginación para recuperar el conjunto de datos de respuesta completo de la entrada seleccionada.

* [Aggregate Series API](/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries): Permite la consulta y recuperación de los valores agregados y las marcas de tiempo de intervalo correspondientes, aplicando cálculos definidos por variables en los eventos sin procesar. Estas variables se pueden definir en el modelo de serie temporal o proporcionarse en línea en la consulta. Esta API admite la paginación para recuperar el conjunto de datos de respuesta completo de la entrada seleccionada.
  
  En un intervalo de búsqueda especificado, esta API devuelve una respuesta agregada por intervalo y por variable de un identificador de serie temporal. El número de intervalos en el conjunto de datos de respuesta se calcula contando los tics de época (número de milisegundos transcurrido desde la época Unix, esto es, desde el 1 de enero 1970) y dividiendo los tics entre el tamaño del intervalo especificado en la consulta.

  Las marcas de tiempo devueltas en el conjunto de respuestas corresponden a los límites de intervalo que quedan, no a los eventos muestreados del intervalo.


### <a name="selecting-store-type"></a>Selección del tipo de tienda

Las API anteriores solo pueden ejecutarse en uno de los dos tipos de almacenamiento (en reposo o intermedio) en una sola llamada. Los parámetros de dirección URL de consulta se usan para especificar el [tipo de almacén](/rest/api/time-series-insights/dataaccessgen2/query/execute#uri-parameters) en el que se debe ejecutar la consulta. 

Si no se especifica ningún parámetro, la consulta se ejecutará en el almacén en reposo de forma predeterminada. Si una consulta abarca un intervalo de tiempo que superpone el almacén en reposo e intermedio, se recomienda enrutar la consulta al almacén en reposo para obtener la mejor experiencia, ya que el almacén intermedio solo contendrá datos parciales. 

El [Explorador de Azure Time Series Insights](./concepts-ux-panels.md) y el [conector de Power BI](./how-to-connect-power-bi.md) realizan llamadas a las API anteriores y seleccionarán automáticamente el parámetro storeType correcto si procede. 


## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre las distintas variables que se pueden definir en el [modelo de serie temporal](./concepts-model-overview.md).
* Obtenga más información sobre cómo consultar datos desde el [Explorador de Azure Time Series Insights](./concepts-ux-panels.md).
