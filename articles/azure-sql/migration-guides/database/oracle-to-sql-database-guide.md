---
title: 'De Oracle a SQL Database: guía de migración'
description: Con esta guía aprenderá a migrar su esquema de Oracle a Azure SQL Database mediante SQL Server Migration Assistant para Oracle (SSMA para Oracle).
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 08/25/2020
ms.openlocfilehash: f00740de5a327858fd250a0cb561b07c32f3b726
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104655578"
---
# <a name="migration-guide-oracle-to-azure-sql-database"></a>Guía de migración: de Oracle a Azure SQL Database
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Con esta guía aprenderá a migrar su esquema de Oracle a Azure SQL Database mediante SQL Server Migration Assistant para Oracle.

Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Requisitos previos

Para migrar el esquema de Oracle a SQL Database, necesita: 

- Comprobar que el entorno de origen es compatible. 
- Descargar [SQL Server Migration Assistant (SSMA) para Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258). 
- Una instancia de [Azure SQL Database](../../database/single-database-create-quickstart.md) de destino. 
- Los [permisos necesarios para SSMA para Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) y el [proveedor](/sql/ssma/oracle/connect-to-oracle-oracletosql).
 

## <a name="pre-migration"></a>Antes de la migración

Una vez cumplidos los requisitos previos, estará a punto para detectar la topología de su entorno y evaluar la viabilidad de la migración. Esta parte del proceso implica la elaboración de un inventario de las bases de datos que se deben migrar, la evaluación de las bases de datos en busca de posibles problemas de migración o bloqueadores y la resolución de cualquier aspecto que pueda haber descubierto.



### <a name="assess"></a>Evaluar 


Use SQL Server Migration Assistant (SSMA) para Oracle para revisar los objetos y datos de base de datos, evaluar las bases de datos para la migración, migrar los objetos de base de datos a Azure SQL Database y, por último, migrar los datos a la base de datos. 


Para crear una evaluación, siga estos pasos: 


1. Abra [SQL Server Migration Assistant para Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258). 
1. Seleccione **Archivo** y, luego, elija **Nuevo proyecto**. 
1. Facilite el nombre del proyecto y una ubicación para guardarlo y, luego, seleccione Azure SQL Database como destino de la migración en la lista desplegable. Seleccione **Aceptar**.

   ![Nuevo proyecto](./media/oracle-to-sql-database-guide/new-project.png)


1. Seleccione **Connect to Oracle** (Conectarse a Oracle). Escriba los valores de los detalles de la conexión de Oracle en el cuadro de diálogo **Conectarse a Oracle**.

   ![Conectar con Oracle](./media/oracle-to-sql-database-guide/connect-to-oracle.png)

   Seleccione los esquemas de Oracle que quiera migrar: 

   ![Selección del esquema de Oracle](./media/oracle-to-sql-database-guide/select-schema.png)

1. Haga clic con el botón secundario en el esquema de Oracle que desea migrar en el **explorador de metadatos de Oracle** y, a continuación, elija **Crear informe**. Se generará un informe HTML. También puede elegir **Crear informe** en la barra de navegación después de seleccionar la base de datos.

   ![Crear informe](./media/oracle-to-sql-database-guide/create-report.png)

1. Revise el informe en HTML para comprender las estadísticas de conversión, y los errores o advertencias. También puede abrir el informe en Excel para obtener un inventario de objetos de Oracle y conocer el esfuerzo necesario para realizar las conversiones de esquema. La ubicación predeterminada del informe es la carpeta de informes dentro de SSMAProjects.

   Por ejemplo: `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2020_11_12T02_47_55\`

   ![Informe de evaluación](./media/oracle-to-sql-database-guide/assessment-report.png) 



### <a name="validate-data-types"></a>Validación de los tipos de datos

Valide las asignaciones predeterminadas de los tipos de datos y cámbielas según los requisitos, si es necesario. Para hacerlo, siga estos pasos:

1. Seleccione **Herramientas** en el menú principal. 
1. Seleccione **Configuración del proyecto**. 
1. Seleccione la pestaña **Type mappings** (Asignaciones de tipos). 

   ![Asignaciones de tipos](./media/oracle-to-sql-database-guide/type-mappings.png)

1. Puede cambiar la asignación de tipos de cada tabla seleccionando la tabla en el **explorador de metadatos de Oracle**.

### <a name="convert-schema"></a>Convertir esquema

Para convertir el esquema, siga estos pasos: 

1. (Opcional) Agregue consultas dinámicas o ad hoc a las instrucciones. Haga clic con el botón derecho en el nodo y elija **Add statements** (Agregar instrucciones).
1. Seleccione **Conectar a Azure SQL Database**. 
    1. Escriba los detalles de la conexión para conectar la base de datos en Azure SQL Database.
    1. Elija la instancia de SQL Database de destino en la lista desplegable.
    1. Seleccione **Conectar**.

    ![Conectarse a SQL Database](./media/oracle-to-sql-database-guide/connect-to-sql-database.png)


1. Haga clic con el botón derecho en el esquema de Oracle en **Oracle Metadata Explorer** (Explorador de metadatos de Oracle) y elija **Convert Schema** (Convertir esquema). También puede elegir **Convertir esquema** en la barra de navegación superior después de seleccionar el esquema.

   ![Convertir esquema](./media/oracle-to-sql-database-guide/convert-schema.png)

1. Una vez finalizada la conversión, compare y revise los objetos convertidos con los objetos originales para identificar posibles problemas y solucionarlos en función de las recomendaciones.

   ![Revisar el esquema de recomendaciones](./media/oracle-to-sql-database-guide/table-mapping.png)

   Compare el texto de Transact-SQL convertido con el código original y revise las recomendaciones: 

   ![Revisión de recomendaciones](./media/oracle-to-sql-database-guide/procedure-comparison.png)

1. Guarde el proyecto localmente para un ejercicio de corrección de esquema sin conexión. Seleccione **Guardar proyecto** en el menú **Archivo**.

## <a name="migrate"></a>Migrar

Cuando haya terminado de evaluar las bases de datos y de resolver las discrepancias, el siguiente paso consiste en ejecutar el proceso de migración. La migración implica dos pasos: publicar el esquema y migrar los datos. 

Para publicar el esquema y migrar los datos, siga estos pasos:

1. Publique el esquema: haga clic con el botón derecho en la base de datos desde el nodo **Bases de datos** del **Explorador de metadatos de Azure SQL Database** y elija **Sincronizar con base de datos**.

   ![Sincronización con la base de datos](./media/oracle-to-sql-database-guide/synchronize-with-database.png)

   Revise la asignación entre el proyecto de origen y el de destino:

   ![Sincronización con la revisión de la base de datos](./media/oracle-to-sql-database-guide/synchronize-with-database-review.png)


1. Migre los datos: haga clic con el botón derecho en el **explorador de metadatos de Oracle** y elija **Migrar datos**. Como alternativa, puede seleccionar **Migrate Data** (Migrar datos) en la barra de navegación superior tras seleccionar el esquema. 

   ![Migración de los datos](./media/oracle-to-sql-database-guide/migrate-data.png)

1. Especifique los detalles de la conexión para las instancias de Oracle y Azure SQL Database.
1. Vea el **Informe de migración de datos**.

   ![Informe de migración de datos](./media/oracle-to-sql-database-guide/data-migration-report.png)

1. Conéctese a la instancia de Azure SQL Database mediante [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms), y revise los datos y el esquema para validar la migración.

   ![Validación en SSMA](./media/oracle-to-sql-database-guide/validate-data.png)

Como alternativa, también puede usar SQL Server Integration Services (SSIS) para realizar la migración. Para obtener más información, consulte: 

- [SQL Server Migration Assistant: procedimiento para evaluar y migrar datos de plataformas de datos que no son de Microsoft a SQL Server](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/)
- [Introducción a SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services)
- [SQL Server Integration Services: SSIS para Azure y traslado de datos híbridos](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx)


## <a name="post-migration"></a>Etapa posterior a la migración 

Cuando haya completado correctamente la fase de **migración**, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.

[Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) es una extensión para Visual Studio Code que le permite analizar el código fuente Java, y detectar las llamadas y consultas de API de acceso a datos, lo que le proporciona una vista en un solo panel de lo que debe solucionarse para admitir el nuevo back-end de base de datos. Para obtener más información, consulte el blog [Migración de la aplicación Java desde Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727). 



### <a name="perform-tests"></a>Realización de pruebas

El enfoque de prueba para la migración de bases de datos consiste en realizar las siguientes actividades:

1.  **Desarrollar pruebas de validación**. para probar la migración de bases de datos, debe utilizar consultas SQL. Debe crear las consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.

2.  **Configurar un entorno de prueba**. el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.

3.  **Ejecutar pruebas de validación**. ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.

4.  **Ejecutar pruebas de rendimiento**. ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.


### <a name="optimize"></a>Optimización

La fase posterior a la migración es fundamental para reconciliar cualquier problema de precisión de datos y comprobar su integridad, así como para solucionar problemas de rendimiento con la carga de trabajo.

> [!NOTE]
> Para obtener detalles adicionales sobre estos problemas y los pasos específicos para mitigarlos, consulte la [Guía de optimización y validación posterior a la migración](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda a fin de completar el escenario de migración, consulte los siguientes recursos, desarrollados para prestar apoyo a un proyecto de migración real.

| **Título/vínculo**                                                                                                                                          | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Esta herramienta proporciona sugerencias de plataformas de destino "ideales", preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo mediante un clic y una función de generación de informes que acelera las evaluaciones de gran extensión mediante un proceso de toma de decisiones uniforme y automatizado en relación con la plataforma de destino.                                                          |
| [Artefactos de script de inventario de Oracle](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Este recurso incluye una consulta PL/SQL que visita las tablas del sistema de Oracle y proporciona un recuento de objetos por tipo de esquema, tipo de objeto y estado. También proporciona una estimación aproximada de "datos sin procesar" en cada esquema y el dimensionamiento de las tablas de cada esquema, con resultados almacenados en formato CSV.                                                                                                               |
| [Automatización de la recopilación y consolidación de evaluaciones de SSMA para Oracle](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Este conjunto de recursos usa un archivo .csv como entrada (sources.csv en las carpetas del proyecto) para generar los archivos XML necesarios para ejecutar la evaluación de SSMA en modo de consola. El cliente proporciona el archivo source.csv basado en un inventario de las instancias de Oracle existentes. Los archivos de salida son AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml y VariableValueFile.xml.|
| [Errores comunes de SSMA para Oracle y procedimiento para corregirlos](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | Con Oracle, puede asignar una condición no escalar en la cláusula WHERE. Sin embargo, SQL Server no admite este tipo de condición. Como resultado, SQL Server Migration Assistant (SSMA) para Oracle no convierte las consultas con una condición no escalar en la cláusula WHERE, sino que se genera un error O2SS0001. Estas notas del producto proporcionan más detalles sobre el problema y las maneras de resolverlo.          |
| [Manual de migración de Oracle a SQL Server](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | Este documento se centra en las tareas asociadas a la migración de un esquema de Oracle a la versión más reciente de SQL Server. Si la migración requiere cambios en las características o funcionalidades, se debe tener muy en cuenta el posible impacto de cada cambio en las aplicaciones que usan la base de datos.                                                     |

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, así como con las tareas especializadas, consulte el artículo [Servicios y herramientas de migración de datos](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

- Para más información acerca de Azure SQL Database, consulte: 
  - [¿Qué es Azure SQL Database?](../../database/sql-database-paas-overview.md)
  - [Calculadora del costo total de propiedad (TCO) de Azure](https://azure.microsoft.com/en-us/pricing/tco/calculator/)


- Para más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- En cuanto al contenido de vídeo, consulte: 
    - [Información general sobre el recorrido de la migración y las herramientas y servicios recomendados para llevar a cabo la evaluación y la migración](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)


