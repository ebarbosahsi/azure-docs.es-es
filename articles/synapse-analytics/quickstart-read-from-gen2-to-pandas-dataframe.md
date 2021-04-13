---
title: 'Inicio rápido: Lectura de datos de ADLS Gen2 en una trama de datos de Pandas'
description: Lea datos de una cuenta de Azure Data Lake Storage Gen2 en una trama de datos de Pandas mediante el uso de Python en Synapse Studio, en Azure Synapse Analytics.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: quickstart
ms.reviewer: jrasnick, garye, negust
ms.date: 03/23/2021
author: garyericson
ms.author: garye
ms.openlocfilehash: b7358c522cf12e7856496ad71fda393394e7ceab
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105969581"
---
# <a name="quickstart-read-data-from-adls-gen2-to-pandas-dataframe-in-azure-synapse-analytics"></a>Inicio rápido: Lectura de datos de ADLS Gen2 en una trama de datos de Pandas en Azure Synapse Analytics

En esta guía de inicio rápido, aprenderá a usar fácilmente Python para leer datos de Azure Data Lake Storage (ADLS) Gen2 en una trama de datos de Pandas, en Azure Synapse Analytics.

Desde un cuaderno de Synapse Studio, podrá hacer lo siguiente:

- Conectarse a un contenedor de Data Lake Storage Gen2 que esté vinculado al área de trabajo de Azure Synapse Analytics
- Leer los datos de un cuaderno de PySpark mediante `spark.read.load`
- Convertir los datos en una trama de datos de Pandas mediante `.toPandas()`

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/).
- Área de trabajo de Synapse Analytics con Data Lake Storage Gen2 configurado como almacenamiento predeterminado: el usuario debe ser el **colaborador de datos de blob de almacenamiento** del sistema de archivos de Data Lake Storage Gen2 con el que trabaja. Para más información sobre cómo crear un área de trabajo, consulte la [creación de un área de trabajo de Synapse](get-started-create-workspace.md).
- Grupo de Apache Spark en el área de trabajo: consulte la [creación de un grupo de Apache Spark sin servidor](get-started-analyze-spark.md#create-a-serverless-apache-spark-pool).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="upload-sample-data-to-adls-gen2"></a>Carga de datos de ejemplo en ADLS Gen2

1. En Azure Portal, cree un contenedor en la misma instancia de Data Lake Storage Gen2 que usa Synapse Studio. Puede omitir este paso si desea usar la cuenta de almacenamiento vinculada predeterminada en el área de trabajo de Azure Synapse Analytics.

1. En Synapse Studio, haga clic en **Datos**, seleccione la pestaña **vinculada** y el contenedor en **Azure Data Lake Storage Gen2**.

1. Descargue el archivo de ejemplo [RetailSales.csv](https://github.com/Azure-Samples/Synapse/blob/main/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/Retail/RetailData/RetailSales.csv) y cárguelo en el contenedor.

1. Seleccione el archivo cargado, haga clic en **Propiedades** y copie el valor de la **ruta de acceso ABFSS**.

## <a name="read-data-from-adls-gen2-into-a-pandas-dataframe"></a>Lectura de datos de ADLS Gen2 en una trama de datos de Pandas

1. En el panel izquierdo, haga clic en **Desarrollar**.

1. Haga clic en **+** y seleccione "Notebook" (Cuaderno) para crear un cuaderno nuevo.

1. En **Attach to** (Adjuntar a), seleccione el grupo de Apache Spark. Si no tiene uno, haga clic en **Create Apache Spark pool** (Crear un grupo de Apache Spark).

1. En la celda de código del cuaderno, pegue el siguiente código de Python e inserte la ruta de acceso de ABFSS que copió anteriormente:

   ```python
   %%pyspark
   data_path = spark.read.load('<ABFSS Path to RetailSales.csv>', format='csv', header=True)
   data_path.show(10)
   
   print('Converting to Pandas.')
   
   pdf = data_path.toPandas()
   print(pdf)
   ```

1. Ejecute la celda.

Después de unos minutos, el texto mostrado debe ser similar al siguiente.

```text
Command executed in 25s 324ms by gary on 03-23-2021 17:40:23.481 -07:00

Job execution Succeeded Spark 2 executors 8 cores

+-------+-----------+--------+-----------+-----------+-----+------------+--------------------+
|storeId|productCode|quantity|logQuantity|advertising|price|weekStarting|                  id|
+-------+-----------+--------+-----------+-----------+-----+------------+--------------------+
|      2| surface.go|     105|9.264828557|          1|  159|   6/15/2017|d6bd47a7-2ad6-4f0...|
|      2| surface.go|      80|8.987196821|          0|  269|   7/27/2017|64cc74c2-c7da-4e1...|
|      2| surface.go|      68|8.831711918|          1|  209|    8/3/2017|9a2d164b-5e44-44d...|
|      2| surface.go|      28|7.965545573|          0|  209|   8/10/2017|b8cd9987-1d5a-4f4...|
|      2| surface.go|      16|7.377758908|          0|  209|   8/24/2017|ac0ec099-e102-4bf...|
|      2| surface.go|     253| 10.1402973|          1|  189|   8/31/2017|3d22c002-b04c-409...|
|      2| surface.go|     107|9.282847063|          0|  189|    9/7/2017|b6e19699-d684-449...|
|      2| surface.go|      66|8.803273983|          0|  189|   9/14/2017|e89a5838-fb8f-413...|
|      2| surface.go|      65|8.793612072|          0|  179|   9/21/2017|c3278682-16c0-483...|
|      2| surface.go|      17|7.454719949|          0|  269|  10/12/2017|f40190c1-b2ed-46f...|
+-------+-----------+--------+-----------+-----------+-----+------------+--------------------+
only showing top 10 rows

Converting to Pandas.
      storeId  ...                                    id
0           2  ...  d6bd47a7-2ad6-4f0a-b8de-ed1386cae5ea
1           2  ...  64cc74c2-c7da-4e12-af64-c95bdf429934
2           2  ...  9a2d164b-5e44-44d7-9837-cf9ae6566c99
3           2  ...  b8cd9987-1d5a-4f4f-9346-719d73b1f7f0
4           2  ...  ac0ec099-e102-4bfc-9775-983b151dcd03
...       ...  ...                                   ...
28942     137  ...  6af00133-7015-415d-831b-ddf05bb5828c
28943     137  ...  1e0d3a21-ab43-49c4-89e2-49d202821807
28944     137  ...  5cc7e50a-6aa4-419b-a933-905a667aa2df
28945     137  ...  650ca506-7a4f-46f8-b2e1-e52ceffadf16
28946     137  ...  9bb216f6-04ec-4b61-9e68-34772b814c44

[28947 rows x 8 columns]
```

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Synapse Analytics?](overview-what-is.md)
- [Introducción a Azure Synapse Analytics](get-started.md)
- [Creación de un grupo de Apache Spark sin servidor](get-started-analyze-spark.md#create-a-serverless-apache-spark-pool)
