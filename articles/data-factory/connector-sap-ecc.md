---
title: Copia de datos desde SAP ECC
description: Obtenga información sobre cómo copiar datos desde SAP ECC en almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
ms.author: jingwang
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/28/2020
ms.openlocfilehash: a3e701f3d433b5b52d8992035ac4ad75b78cb795
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100386703"
---
# <a name="copy-data-from-sap-ecc-by-using-azure-data-factory"></a>Copia de datos de SAP ECC mediante Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos de SAP Enterprise Central Component (ECC). Para obtener más información, consulte la [información general sobre la actividad de copia](copy-activity-overview.md).

>[!TIP]
>Para obtener información sobre la compatibilidad general de ADF con el escenario de integración de datos de SAP, consulte el [informe técnico sobre la integración de datos de SAP mediante Azure Data Factory](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) que contiene una introducción detallada con comparaciones y una guía sobre cada conector de SAP.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector SAP ECC es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de SAP ECC en cualquier almacén de datos de receptor admitido. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector SAP ECC admite las siguientes funcionalidades:

- Copia de datos de SAP ECC en SAP NetWeaver versión 7.0 y versiones posteriores.
- Copia de datos de los objetos expuestos por los servicios SAP ECC OData, como:

  - Tablas o vistas de SAP.
  - Objetos de la interfaz de programación de aplicaciones empresariales (BAPI).
  - Extractores de datos.
  - Datos o documentos intermedios (IDOC) enviados a la integración de procesos (PI) de SAP que se pueden recibir como OData a través de adaptadores relativos.

- Copia de datos mediante la autenticación básica.

La versión 7.0 o posterior hace referencia a la versión de SAP NetWeaver en lugar de a la versión de SAP ECC. Por ejemplo, SAP ECC 6.0 EHP 7 en general tiene una versión de NetWeaver >= 7.4. En caso de que no esté seguro de su entorno, estos son los pasos para confirmar la versión del sistema de SAP:

1. Use la GUI de SAP para conectarse al sistema de SAP. 
2. Vaya a **System** -> **Status**. 
3. Compruebe la versión de SAP_BASIS, asegúrese de que sea mayor o igual que 701.  
      ![Comprobar SAP_BASIS](./media/connector-sap-table/sap-basis.png)

>[!TIP]
>Para copiar datos de SAP ECC a través de una tabla o una vista de SAP, use el conector de [tabla de SAP](connector-sap-table.md), que es más rápido y escalable.

## <a name="prerequisites"></a>Requisitos previos

Para usar este conector SAP ECC, debe exponer las entidades de SAP ECC mediante servicios OData a través de SAP Gateway. Más concretamente:

- **Configurar la puerta de enlace SAP**. Para los servidores con versiones de SAP NetWeaver posteriores a 7.4, la puerta de enlace de SAP ya está instalada. En el caso de versiones anteriores, debe instalar la puerta de enlace de SAP insertada o el sistema de centro de puerta de enlace de SAP antes de exponer los datos de SAP ECC a través de los servicios de OData. Para configurar la puerta de enlace, consulte la [guía de instalación](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Activar y configurar el servicio SAP OData**. Puede activar el servicios de OData a través de TCODE SICF en cuestión de segundos. También puede configurar qué objetos se deben exponer. Para obtener más información, consulte la [guía paso a paso](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir las entidades de Data Factory específicas del conector SAP ECC.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado SAP ECC:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| `type` | La propiedad `type` debe establecerse en `SapEcc`. | Sí |
| `url` | Dirección URL del servicio SAP ECC OData. | Sí |
| `username` | Nombre de usuario usado para conectarse a SAP ECC. | No |
| `password` | Contraseña de texto no cifrado que se usa para conectarse a SAP ECC. | No |
| `connectVia` | El [entorno de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Obtenga más información en la sección [Requisitos previos](#prerequisites). Si no especifica un entorno de ejecución, se usará la opción predeterminada de Azure Integration Runtime. | No |

### <a name="example"></a>Ejemplo

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData URL, e.g., http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of integration runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si quiere ver una lista completa de las secciones y las propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos](concepts-datasets-linked-services.md). En la sección siguiente se proporciona una lista de las propiedades que admite el conjunto de datos de SAP ECC.

Para copiar datos desde SAP ECC, establezca la propiedad `type` del conjunto de datos en `SapEccResource`.

Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| `path` | Ruta de acceso de la entidad de SAP ECC OData. | Sí |

### <a name="example"></a>Ejemplo

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typeProperties": {
            "path": "<entity path, e.g., dd04tentitySet>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si quiere ver una lista completa de las secciones y las propiedades disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). En la sección siguiente se proporciona una lista de las propiedades que admite el origen de SAP ECC.

### <a name="sap-ecc-as-a-source"></a>SAP ECC como origen

Para copiar datos de SAP ECC, establezca la propiedad `type` de la sección `source` de la actividad de copia en `SapEccSource`.

Se admiten las siguientes propiedades en la sección `source` de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| `type` | La propiedad `type` de la sección `source` de la actividad de copia debe establecerse en `SapEccSource`. | Sí |
| `query` | Opciones de consulta de OData para filtrar los datos. Por ejemplo:<br/><br/>`"$select=Name,Description&$top=10"`<br/><br/>El conector de SAP ECC copia datos de la dirección URL combinada:<br/><br/>`<URL specified in the linked service>/<path specified in the dataset>?<query specified in the copy activity's source section>`<br/><br/>Para más información, consulte el artículo sobre [componentes de URL de OData](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | No |
| `sapDataColumnDelimiter` | El único carácter que se usa como delimitador que se pasa al RFC de SAP para dividir los datos de salida. | No |
| `httpRequestTimeout` | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. Si no se especifica, el valor predeterminado es **00:30:00** (30 minutos). | No |

### <a name="example"></a>Ejemplo

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mappings-for-sap-ecc"></a>Asignaciones de tipos de datos para SAP ECC

Al copiar datos de SAP ECC, se usan las siguientes asignaciones de tipos de datos de OData para los datos de SAP ECC en los tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para más información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo de datos de OData | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| `Edm.Binary` | `String` |
| `Edm.Boolean` | `Bool` |
| `Edm.Byte` | `String` |
| `Edm.DateTime` | `DateTime` |
| `Edm.Decimal` | `Decimal` |
| `Edm.Double` | `Double` |
| `Edm.Single` | `Single` |
| `Edm.Guid` | `String` |
| `Edm.Int16` | `Int16` |
| `Edm.Int32` | `Int32` |
| `Edm.Int64` | `Int64` |
| `Edm.SByte` | `Int16` |
| `Edm.String` | `String` |
| `Edm.Time` | `TimeSpan` |
| `Edm.DateTimeOffset` | `DateTimeOffset` |

> [!NOTE]
> Actualmente no se admiten tipos de datos complejos.

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
