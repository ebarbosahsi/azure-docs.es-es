---
title: Migración desde Oracle
titleSuffix: Azure Database for PostgreSQL
description: En esta guía se explica cómo migrar el esquema de Oracle a Azure Database for PostgreSQL.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.subservice: migration-guide
ms.topic: how-to
ms.date: 03/18/2021
ms.openlocfilehash: ec6cf87b3fd326c905b4843dc30ae6ce15379305
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608670"
---
# <a name="migrate-oracle-to-azure-database-for-postgresql"></a>Migración de Oracle a Azure Database for PostgreSQL

En esta guía se explica cómo migrar el esquema de Oracle a Azure Database for PostgreSQL. 

Para obtener instrucciones de migración detalladas y completas, consulte los [recursos de la guía de migración](https://github.com/microsoft/OrcasNinjaTeam/blob/master/Oracle%20to%20PostgreSQL%20Migration%20Guide/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Guide.pdf). 

## <a name="prerequisites"></a>Requisitos previos

Para migrar el esquema de Oracle a Azure Database for PostgreSQL necesitará hacer lo siguiente: 

- Comprobar que el entorno de origen es compatible. 
- Descargar la última versión de [ora2pg](https://ora2pg.darold.net/). 
- Tener la última versión del [módulo DBD](https://www.cpan.org/modules/by-module/DBD/). 


## <a name="overview"></a>Información general

PostgreSQL es una de las bases de datos de código abierto más avanzadas del mundo. En este artículo se describe cómo usar la utilidad gratuita ora2pg para migrar una base de datos de Oracle a PostgreSQL. Puede usar la herramienta gratuita ora2pg para migrar una base de datos de Oracle o MySQL a un esquema compatible con PostgreSQL. La utilidad conecta la base de datos de Oracle, la examina automáticamente y extrae su estructura o datos. Después, ora2pg genera scripts SQL que se pueden cargar en la base de datos de PostgreSQL. ora2pg se puede usar para tareas de ingeniería inversa en una base de datos de Oracle, migraciones de bases de datos empresariales de gran tamaño o simplemente para la replicación de algunos datos de Oracle en una base de datos de PostgreSQL. Es fácil de usar y no requiere ningún conocimiento relacionado con la base de datos de Oracle que no sea la capacidad de proporcionar los parámetros necesarios para conectarse a la base de datos de Oracle.

> [!NOTE]
> Para obtener más información acerca del uso de la versión más reciente de ora2pg, consulte la [documentación de ora2pg](https://ora2pg.darold.net/documentation.html).

### <a name="typical-ora2pg-migration-architecture"></a>Arquitectura de migración típica de ora2pg

![Captura de pantalla de la arquitectura de migración de ora2pg.](media/howto-migrate-from-oracle/ora2pg-migration-architecture.png)

Después de aprovisionar la máquina virtual y Azure Database for PostgreSQL, se necesitan dos configuraciones para habilitar la conectividad entre ambos: **Permitir servicios de Azure** y **Aplicar conexión SSL**, como se muestra a continuación:

- Hoja **Seguridad de la conexión** ->**Permitir el acceso a servicios de Azure** -> ACTIVADO.

- Hoja **Seguridad de la conexión** -> : **Configuración de SSL** -> **Aplicar conexión SSL** -> DESHABILITADO

### <a name="recommendations"></a>Recomendaciones

- Para mejorar el rendimiento de las operaciones de evaluación o exportación en el servidor de Oracle, recopile las estadísticas de la siguiente manera:

   ```
   BEGIN
   
     DBMS_STATS.GATHER_SCHEMA_STATS
     DBMS_STATS.GATHER_DATABASE_STATS
     DBMS_STATS.GATHER_DICTIONARY_STATS
     END;
   ```

- Exporte datos con el comando COPY en lugar de INSERT.

- Evite exportar tablas con sus claves externas, restricciones e índices; si lo hace, la importación de los datos en PostgreSQL será más lenta.

- Cree vistas materializadas con la **cláusula no-data** y actualícela más tarde.

- Si es posible, implemente índices únicos en las vistas materializadas; de este modo, la actualización con la sintaxis `REFRESH MATERIALIZED VIEW CONCURRENTLY` será más rápida.



## <a name="pre-migration"></a>Antes de la migración 

Una vez que haya comprobado que el entorno de origen es compatible y que se haya asegurado de que se cumplieron los requisitos previos, está listo para iniciar la fase antes de la migración. Esta parte del proceso implica la elaboración de un inventario de las bases de datos que se deben migrar, la evaluación de las bases de datos en busca de posibles problemas de migración o bloqueadores y la resolución de cualquier aspecto que pueda haber descubierto. En el caso de las migraciones heterogéneas, como Oracle a Azure Database for PostgreSQL, esta fase también implica convertir los esquemas de las bases de datos de origen para que sean compatibles con el entorno de destino.

### <a name="discover"></a>Descubra

El objetivo de la fase de detección es identificar los orígenes de datos existentes y los detalles sobre las características que se usan para comprender y planear mejor la migración. Este proceso implica el examen de la red para identificar todas las instancias de Oracle de su organización junto con la versión y las características en uso.

Los scripts de evaluación previa de Oracle de Microsoft se ejecutan en la base de datos de Oracle. Los scripts de evaluación previa son un conjunto de consultas que interactúan con los metadatos de Oracle y proporcionan lo siguiente:

- Un inventario de la base de datos, incluidos los recuentos de objetos por esquema, tipo y estado.

- Una estimación aproximada de los datos sin procesar de cada esquema (basado en estadísticas).

- El tamaño de las tablas de cada esquema.

- El número de líneas de código por paquete, función, procedimiento, etc.

Descargue los scripts relacionados desde el [sitio web de ora2pg](http://ora2pg.darold.net/).

### <a name="assess"></a>Evaluar

Una vez completado el inventario de las bases de datos de Oracle, a fin de hacerse una idea del tamaño de la base de datos y de cuáles son los desafíos, el siguiente paso consiste en ejecutar la evaluación.

No es fácil calcular el costo de un proceso de migración de Oracle a PostgreSQL. Para obtener una buena valoración de este costo de migración, ora2pg inspeccionará todos los objetos de base de datos, todas las funciones y los procedimientos almacenados para detectar si todavía hay algún objeto y código PL/SQL que ora2pg no puede convertir automáticamente.

ora2pg tiene un modo de análisis de contenido que inspecciona la base de datos de Oracle para generar un informe de texto sobre el contenido de la base de datos de Oracle y qué parte de este no se puede exportar.

Para activar el modo de **análisis e informe**, use el tipo exportado `SHOW_REPORT` como se muestra en el siguiente comando:

```
ora2pg -t SHOW_REPORT
```

Una vez analizada la base de datos, ora2pg puede usar su capacidad para convertir el código SQL y PL/SQL de la sintaxis de Oracle a la de PostgreSQL para ir más lejos al estimar las dificultades del código y el tiempo necesario para realizar una migración completa de la base de datos.

Para estimar el costo de la migración en días-persona, ora2pg le permite usar una directiva de configuración denominada ESTIMATE_COST, que también se puede habilitar en la línea de comandos:

```
ora2pg -t SHOW_REPORT --estimate_cost
```

La unidad de migración predeterminada representa aproximadamente cinco minutos para un experto en PostgreSQL. Si esta es su primera migración, puede aumentar el valor con la directiva de configuración COST_UNIT_VALUE o la opción de línea de comandos --cost_unit_value.

La última línea del informe muestra el código de migración total estimado en días-persona después del número de unidades de migración estimadas para cada objeto.

Esta unidad de migración representa aproximadamente cinco minutos para un experto en PostgreSQL. Si esta es su primera migración, puede aumentar el valor predeterminado con la directiva de configuración COST_UNIT_VALUE o la opción de línea de comandos --cost_unit_value. A continuación encontrará algunas variaciones de evaluación a) evaluación de tablas; b) evaluación de columnas; c) evaluación de esquemas con el valor predeterminado de cost_unit (5 min); d) evaluación de esquemas con 10 min como unidad de costo.

```
ora2pg -t SHOW_TABLE -c c:\ora2pg\ora2pg_hr.conf > c:\ts303\hr_migration\reports\tables.txt ora2pg -t SHOW_COLUMN -c c:\ora2pg\ora2pg_hr.conf > c:\ts303\hr_migration\reports\columns.txt
ora2pg -t SHOW_REPORT -c c:\ora2pg\ora2pg_hr.conf --dump_as_html --estimate_cost > c:\ts303\hr
_migration\reports\report.html
ora2pg -t SHOW_REPORT -c c:\ora2pg\ora2pg_hr.conf –-cost_unit_value 10 --dump_as_html --estimate_cost > c:\ts303\hr_migration\reports\report2.html
```

A continuación se muestra la salida de la evaluación del esquema:

**Nivel de migración: B-5**

Niveles de migración:

A: Migración que se puede ejecutar automáticamente.

B: Migración con reescritura de código y un costo de hasta 5 días-persona.

C: Migración con reescritura de código y un costo de más de 5 días-persona.

Niveles técnicos:

1 = trivial: sin funciones almacenadas ni desencadenadores.

2 = fácil: sin funciones almacenadas, pero con desencadenadores. Sin reescritura manual.

3 = simple: funciones almacenadas o desencadenadores, sin reescritura manual.

4 = manual: no hay funciones almacenadas, pero sí desencadenadores o vistas con reescritura de código.

5 = difícil: funciones almacenadas o desencadenadores con reescritura de código.

La evaluación consiste en una letra (A o B) para especificar si la migración necesita una reescritura manual o no, y un número de 1 a 5 para indicar el nivel de dificultad técnica. Tiene una opción adicional, human_days_limit, para especificar el número límite de días-persona durante los que el nivel de migración debe estar establecido en C para indicar que se necesita una gran cantidad de trabajo y soporte técnico completo para la migración y administración del proyecto. El valor predeterminado es 10 días-persona. Puede usar la directiva de configuración HUMAN_DAYS_LIMIT para cambiar este valor predeterminado de manera permanente.

Esta característica se desarrolló para ayudarle a decidir qué base de datos se puede migrar primero y cuál es el equipo que se debe movilizar.

### <a name="convert"></a>Convert

Gracias a las migraciones con tiempo de inactividad mínimo, el origen que se migrará seguirá cambiando y se desfasará del destino en términos de datos y esquemas después de la migración única. Durante la fase de **Sincronización de datos** debe asegurarse de que todos los cambios en el origen se capturen y apliquen al destino casi en tiempo real. Después de comprobar que todos los cambios en el origen se han aplicado al destino, puede realizar una migración total del origen al entorno de destino.

En este paso de la migración se produce la conversión o traducción del código de Oracle + DDL a PostgreSQL. La herramienta ora2pg exporta automáticamente los objetos de Oracle en el formato de PostgreSQL. En el caso de los objetos generados, algunos no se compilan en la base de datos PostgreSQL sin cambios manuales.  
El proceso para comprender cuáles elementos necesitan intervención manual consiste en compilar los archivos generados por ora2pg en la base de datos PostgreSQL, comprobar el registro y realizar los cambios necesarios hasta que toda la estructura del esquema sea compatible con la sintaxis de PostgreSQL.


#### <a name="create-migration-template"></a>Creación de la plantilla de migración 

En primer lugar, se recomienda crear la plantilla de migración que ora2pg incluye de fábrica. Cuando se usan las dos opciones, --project_base e --init_project, indican a ora2pg que tiene que crear una plantilla de proyecto con un árbol de trabajo, un archivo de configuración y un script para exportar todos los objetos de la base de datos de Oracle. Para más información, consulte la [documentación de ora2pg](https://ora2pg.darold.net/documentation.html).

   Use el comando siguiente: 

   ```
   ora2pg --project_base /app/migration/ --init_project test_project
   
   ora2pg --project_base /app/migration/ --init_project test_project
   ```

Salida de ejemplo: 
   
   ```
   Creating project test_project. /app/migration/test_project/ schema/ dblinks/ directories/ functions/ grants/ mviews/ packages/ partitions/ procedures/ sequences/ synonyms/    tables/ tablespaces/ triggers/ types/ views/ sources/ functions/ mviews/ packages/ partitions/ procedures/ triggers/ types/ views/ data/ config/ reports/
   
   Generating generic configuration file
   
   Creating script export_schema.sh to automate all exports.
   
   Creating script import_all.sh to automate all imports.
   ```

El directorio sources/ contiene el código de Oracle, el directorio schema/ contiene el código trasladado a PostgreSQL. El directorio reports/ contiene los informes HTML con la evaluación del costo de la migración.


Una vez creada la estructura del proyecto, se crea un archivo de configuración genérico. Defina la conexión de base de datos de Oracle, así como los parámetros de configuración pertinentes en el archivo de configuración.  Consulte la documentación de ora2pg para saber cuáles elementos se pueden configurar en el archivo de configuración y cómo hacerlo.


#### <a name="export-oracle-objects"></a>Exportación de objetos de Oracle

A continuación, exporte los objetos de Oracle como objetos PostgreSQL. Para ello, ejecute el archivo export_schema.sh.

   ```
   cd /app/migration/mig_project
   ./export_schema.sh
   
   Run the following command manually:
   
   SET namespace="/app/migration/mig_project"
   
   ora2pg -t DBLINK -p -o dblink.sql -b %namespace%/schema/dblinks -c
   %namespace%/config/ora2pg.conf
   ora2pg -t DIRECTORY -p -o directory.sql -b %namespace%/schema/directories -c
   %namespace%/config/ora2pg.conf
   ora2pg -p -t FUNCTION -o functions2.sql -b %namespace%/schema/functions -c
   %namespace%/config/ora2pg.conf ora2pg -t GRANT -o grants.sql -b %namespace%/schema/grants -c %namespace%/config/ora2pg.conf ora2pg -t MVIEW -o mview.sql -b %namespace%/schema/   mviews -c %namespace%/config/ora2pg.conf
   ora2pg -p -t PACKAGE -o packages.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/packages -c
   ora2pg -p -t PARTITION -o partitions.sql %namespace%/config/ora2pg.conf -b %namespace%/schema/partitions -c
   ora2pg -p -t PROCEDURE -o procs.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/procedures -c
   ora2pg -t SEQUENCE -o sequences.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/sequences -c
   ora2pg -p -t SYNONYM -o synonym.sql -b %namespace%/schema/synonyms -c
   %namespace%/config/ora2pg.conf
   ora2pg -t TABLE -o table.sql -b %namespace%/schema/tables -c %namespace%/config/ora2pg.conf ora2pg -t TABLESPACE -o tablespaces.sql -b %namespace%/schema/tablespaces -c
   %namespace%/config/ora2pg.conf
   ora2pg -p -t TRIGGER -o triggers.sql -b %namespace%/schema/triggers -c
   %namespace%/config/ora2pg.conf ora2pg -p -t TYPE -o types.sql -b %namespace%/schema/types -c %namespace%/config/ora2pg.conf ora2pg -p -t VIEW -o views.sql -b %namespace%/   schema/views -c %namespace%/config/ora2pg.conf
   ```

   Use el siguiente comando para extraer los datos:

   ```
   ora2pg -t COPY -o data.sql -b %namespace/data -c %namespace/config/ora2pg.conf
   ```

#### <a name="compile-files"></a>Compilación de archivos

Por último, compile todos los archivos en el servidor de Azure Database for PostgreSQL. Ahora es posible cargar manualmente los archivos DDL generados, o puede usar el segundo script import_all.sh para importar esos archivos de manera interactiva.

   ```
   psql -f %namespace%\schema\sequences\sequence.sql -h server1-
   
   server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l
   
   %namespace%\ schema\sequences\create_sequences.log
   
   psql -f %namespace%\schema\tables\table.sql -h server1-server.postgres.database.azure.com p 5432 -U username@server1-server -d database -l    %namespace%\schema\tables\create_table.log
   ```

   Comando de importación de datos:

   ```
   psql -f %namespace%\data\table1.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l %namespace%\data\table1.log
   
   psql -f %namespace%\data\table2.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l %namespace%\data\table2.log
   ```

Durante la compilación de archivos, compruebe los registros y corrija las sintaxis necesarias que ora2pg no pudo convertir automáticamente.

Consulte las notas del producto [Soluciones alternativas para la migración de Oracle a Azure Database for PostgreSQL](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Workarounds.pdf) para recibir ayuda sobre cómo solucionar los problemas.

## <a name="migrate"></a>Migrar 

Una vez que haya cumplido los requisitos previos necesarios y completado las tareas asociadas a la fase **Antes de la migración**, estará a punto para ejecutar el esquema y la migración de datos.

### <a name="migrate-schema-and-data"></a>Migración de esquemas y datos

Una vez aplicadas las correcciones, estará lista una versión estable de la base de datos lista para la implementación.

En este momento, solo es necesario ejecutar los comandos de importación de *psql* de modo que apunten a los archivos que contienen el código modificado para compilar los objetos de base de datos en la base de datos de PostgreSQL e importar los datos.

En este paso, se puede implementar cierto nivel de paralelismo en la importación de datos.

### <a name="data-sync-and-cutover"></a>Sincronización y migración total de los datos

Gracias a las migraciones en línea (con tiempo de inactividad mínimo), el origen que se migrará seguirá cambiando y se desfasará del destino en términos de datos y esquemas después de la migración única. Durante la fase de **Sincronización de datos** debe asegurarse de que todos los cambios en el origen se capturen y apliquen al destino casi en tiempo real. Después de comprobar que todos los cambios en el origen se han aplicado al destino, puede realizar una migración total del origen al entorno de destino.

A partir de marzo de 2019, si quiere realizar una migración en línea, considere la posibilidad de usar Attunity Replicate for Microsoft Migrations o Striim.

En el caso de la migración *delta/incremental* con ora2pg, la técnica consiste en usar para cada tabla una consulta que aplique un filtro (corte) por fecha u hora, entre otros y, una vez finalizada la migración, aplicar una segunda consulta que migrará el resto de los datos (sobrante).

En la tabla de datos de origen, migre primero todos los datos históricos. Este es un ejemplo:

```
select * from table1 where filter_data < 01/01/2019
```

Puede ejecutar un comando similar al siguiente para consultar los cambios realizados desde la migración inicial:

```
select * from table1 where filter_data >= 01/01/2019
```

En este caso, se recomienda mejorar la validación al comprobar la paridad de los datos en ambos sitios: el origen y el destino.

## <a name="post-migration"></a>Etapa posterior a la migración 

Cuando haya completado correctamente la fase de **migración**, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.

### <a name="perform-tests"></a>Realización de pruebas

Después de migrar los datos al destino, realice pruebas en las bases de datos para comprobar que las aplicaciones funcionan bien en el destino después de la migración.

Para asegurarse de que el origen y el destino se hayan migrado correctamente, ejecute los scripts de validación de datos manuales en las bases de datos de origen de Oracle y de destino de PostgreSQL.

Idealmente, si las bases de datos de origen y destino tienen una ruta de acceso de red, se debe usar ora2pg para la validación de datos. La acción TEST le permite comprobar que se han creado todos los objetos de la base de datos de Oracle en PostgreSQL. Ejecute el comando como se muestra:

```
ora2pg -t TEST -c config/ora2pg.conf > migration_diff.txt
```

### <a name="optimize"></a>Optimización

La fase posterior a la migración es fundamental para reconciliar cualquier problema de precisión de datos y comprobar su integridad, así como para solucionar problemas de rendimiento con la carga de trabajo.

## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda a fin de completar el escenario de migración, consulte los siguientes recursos, desarrollados para dar apoyo en relación con proyecto de migración real:

| **Vínculo de título** | **Descripción**    |
| -------------- | ------------------ |
| [Oracle to Azure PostgreSQL Migration Cookbook](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf) (Instrucciones de migración de Oracle a Azure PostgreSQL) | El objetivo de este documento es proporcionar a los arquitectos, consultores, administradores de bases de datos y roles relacionados una guía para migrar rápidamente las cargas de trabajo de Oracle a Azure Database for PostgreSQL mediante la herramienta ora2pg. |
| [Soluciones alternativas para la migración de Oracle a Azure PostgreSQL](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Workarounds.pdf) | El objetivo de este documento es proporcionar a los arquitectos, consultores, administradores de bases de datos y roles relacionados una guía para solucionar rápidamente los errores durante la migración de cargas de trabajo de Oracle a Azure Database for PostgreSQL. |
| [Pasos para instalar ora2pg en Windows o Linux](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows%20and%20Linux.pdf)                       | Este documento está pensado para usarse como guía de instalación rápida para habilitar la migración de datos de y esquemas de Oracle a Azure Database for PostgreSQL mediante la herramienta ora2pg en Windows o Linux. Puede consultar todos los detalles sobre la herramienta en http://ora2pg.darold.net/documentation.html. |

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.


### <a name="contact-support"></a>Póngase en contacto con el soporte técnico.

Si necesita ayuda con las migraciones más allá de la herramienta ora2pg, póngase en contacto con el alias [@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) para obtener información sobre otras opciones de migración.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una matriz de los servicios o herramientas de Microsoft y de otros fabricantes que están disponibles para facilitar diversos escenarios de migración de datos y de bases de datos (y tareas específicas), consulte el artículo sobre [servicios y herramientas para la migración de datos](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

Para obtener más información, consulte: 
- [Documentación de Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/)
- [Documentación de ora2pg](https://ora2pg.darold.net/documentation.html)
- [Sitio web de PostgreSQL](https://www.postgresql.org/)
- [Compatibilidad con transacciones autónomas en PostgreSQL](http://blog.dalibo.com/2016/08/19/Autonoumous_transactions_support_in_PostgreSQL.html) 

Para ver contenido en vídeo: 
- [Información general sobre el recorrido de la migración y las herramientas y servicios recomendados para llevar a cabo la evaluación y la migración](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).