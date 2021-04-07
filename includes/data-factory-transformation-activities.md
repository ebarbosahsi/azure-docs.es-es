---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 19927fd274cbb7337248fb2ae8cf9d882612d570
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92369254"
---
Azure Data Factory admite las siguientes actividades de transformación que se pueden agregar a las canalizaciones tanto individualmente como encadenadas a otra actividad.

| Actividad de transformación de datos | Entorno de procesos |
|:--- |:--- |
| [Hive](../articles/data-factory/v1/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](../articles/data-factory/v1/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [de Hadoop](../articles/data-factory/v1/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Streaming](../articles/data-factory/v1/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/v1/data-factory-spark.md) | HDInsight [Hadoop] |
| [Actividades de Estudio de Azure Machine Learning (clásico): ejecución de lotes y recurso de actualización](../articles/data-factory/v1/data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Procedimiento almacenado](../articles/data-factory/v1/data-factory-stored-proc-activity.md) |Azure SQL, Azure Synapse Analytics o SQL Server |
| [U-SQL de análisis con Data Lake](../articles/data-factory/v1/data-factory-usql-activity.md) |Análisis con Azure Data Lake |
| [DotNet](../articles/data-factory/v1/data-factory-use-custom-activities.md) |HDInsight [Hadoop] o Azure Batch |

> [!NOTE]
> Puede usar la actividad MapReduce para ejecutar programas Spark en su clúster de HDInsight Spark. Consulte [Invocar programas Spark desde Azure Data Factory](../articles/data-factory/v1/data-factory-spark.md) para obtener más información.
> Puede crear una actividad personalizada para ejecutar scripts de R en su clúster de HDInsight con R instalado. Consulte [Run R Script using Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample)(Ejecutar script de R con Data Factory de Azure).
> 
> 

