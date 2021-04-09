---
title: 'De MySQL a Azure SQL Database: guía de migración'
description: En esta guía se explica cómo migrar las bases de datos de MySQL a Azure SQL Database mediante SQL Server Migration Assistant para MySQL (SSMA para MySQL).
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: ff7b2115b396bf42cdeffa9c58bffb1802e980d1
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "104721892"
---
# <a name="migration-guide--mysql-to-azure-sql-database"></a>Guía de migración: de MySQL a Azure SQL Database
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

En esta guía se explica cómo migrar su base de datos de MySQL a Azure SQL Database mediante SQL Server Migration Assistant para MySQL (SSMA para MySQL). 

Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Requisitos previos

Para migrar la base de datos de MySQL a Azure SQL Database, necesita lo siguiente:

- Comprobar que el entorno de origen es compatible. Actualmente, se admiten MySQL 5.6 y 5.7. 
- [SQL Server Migration Assistant para MySQL](https://www.microsoft.com/download/confirmation.aspx?id=54257)


## <a name="pre-migration"></a>Antes de la migración 

Una vez cumplidos los requisitos previos, estará a punto para detectar la topología de su entorno y evaluar la viabilidad de la migración.

### <a name="assess"></a>Evaluar 

Mediante el uso de [SQL Server Migration Assistant para MySQL](https://www.microsoft.com/download/confirmation.aspx?id=54257), puede revisar los datos y los objetos de las bases de datos, y evaluarlas para la migración.

Para crear una valoración, siga estos pasos.

1. Abra SQL Server Migration Assistant para MySQL. 
1. Seleccione **Archivo** en el menú y, a continuación, elija **Nuevo proyecto**. Proporcione el nombre del proyecto y elija una ubicación para guardar el proyecto. Elija **Azure SQL Database** como destino de la migración. 

   ![Nuevo proyecto](./media/mysql-to-sql-database-guide/new-project.png)

1. Elija **Conectarse a MySQL** y proporcione los detalles de conexión para conectar el servidor MySQL. 

   ![Conectarse a MySQL](./media/mysql-to-sql-database-guide/connect-to-mysql.png)

1. Haga clic con el botón derecho en el esquema de MySQL del **Explorador de metadatos de MySQL** y elija **Crear informe**. Como alternativa, puede seleccionar **Crear informe** en la barra de navegación superior. 

   ![Crear informe](./media/mysql-to-sql-database-guide/create-report.png)

1. Revise el informe HTML para ver las estadísticas de conversión, y los errores y las advertencias. Analícelo para comprender los problemas de conversión y las soluciones. 

   También se puede acceder a este informe desde la carpeta de proyectos de SSMA, tal como se seleccionó en la primera pantalla. En el ejemplo anterior, busque el archivo report.xml en la siguiente ubicación:
 
   `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`
 
   Luego, ábralo en Excel para obtener un inventario de objetos de MySQL y conocer el esfuerzo necesario para realizar las conversiones de esquema.

    ![Informe de conversión](./media/mysql-to-sql-database-guide/conversion-report.png)

### <a name="validate-data-types"></a>Validación de los tipos de datos

Valide las asignaciones predeterminadas de los tipos de datos y cámbielas según los requisitos, si es necesario. Para hacerlo, siga estos pasos: 

1. Seleccione **Herramientas** en el menú principal. 
1. Seleccione **Configuración del proyecto**. 
1. Seleccione la pestaña **Type mappings** (Asignaciones de tipos). 

   ![Asignaciones de tipos](./media/mysql-to-sql-database-guide/type-mappings.png)

1. Puede cambiar la asignación de tipos de cada tabla seleccionando la tabla en el **explorador de metadatos de MySQL**. 

### <a name="convert-schema"></a>Convertir esquema 

Para convertir el esquema, siga estos pasos: 

1. (Opcional) Para convertir consultas dinámicas o ad hoc, haga clic con el botón derecho en el nodo y elija **Agregar instrucción**. 
1. Elija **Conectarse a Azure SQL Database** en la barra de navegación de la línea superior y proporcione los detalles de la conexión. Puede optar por conectarse a una base de datos existente o proporcionar un nombre nuevo, en cuyo caso se creará una base de datos en el servidor de destino.

   ![Conexión a SQL](./media/mysql-to-sql-database-guide/connect-to-sqldb.png)
 
1. Haga clic con el botón derecho en el esquema y elija **Convertir esquema**. 

   ![Convertir esquema](./media/mysql-to-sql-database-guide/convert-schema.png)

1. Una vez finalizada la conversión del esquema, compare el código convertido con el código original para detectar posibles problemas. 

   Compare objetos convertidos con objetos originales: 

   ![ Comparación y revisión de objetos ](./media/mysql-to-sql-database-guide/table-comparison.png)

   Compare los procedimientos convertidos con los originales: 

   ![Comparación y revisión del código de objetos](./media/mysql-to-sql-database-guide/procedure-comparison.png)


## <a name="migrate"></a>Migrar 

Cuando haya terminado de evaluar las bases de datos y de resolver las discrepancias, el siguiente paso consiste en ejecutar el proceso de migración. La migración implica dos pasos: publicar el esquema y migrar los datos. 

Para publicar el esquema y migrar los datos, siga estos pasos: 

1. Haga clic con el botón derecho en la base de datos del **Explorador de metadatos de Azure SQL Database** y elija **Sincronizar con base de datos**. Esta acción publica el esquema de MySQL en Azure SQL Database.

   ![Sincronización con la base de datos](./media/mysql-to-sql-database-guide/synchronize-database.png)

   Revise la asignación entre el proyecto de origen y el de destino:

   ![Sincronización con la revisión de la base de datos](./media/mysql-to-sql-database-guide/synchronize-database-review.png)

1. Haga clic con el botón derecho en el esquema de MySQL en el **Explorador de metadatos de MySQL** y elija **Migrar datos**. Como alternativa, puede seleccionar **Migrar datos** en la barra de navegación superior. 

   ![Migración de los datos](./media/mysql-to-sql-database-guide/migrate-data.png)

1. Una vez finalizada la migración, vea el informe de **migración de datos**: 

   ![Informe de migración de datos](./media/mysql-to-sql-database-guide/data-migration-report.png)

1.  Valide la migración revisando los datos y el esquema en Azure SQL Database mediante SQL Server Management Studio (SSMS).

    ![Validación en SSMA](./media/mysql-to-sql-database-guide/validate-in-ssms.png)



## <a name="post-migration"></a>Etapa posterior a la migración 

Cuando haya completado correctamente la fase de **migración**, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.

### <a name="perform-tests"></a>Realización de pruebas

El enfoque de prueba para la migración de bases de datos consiste en realizar las siguientes actividades:

1. **Desarrollar pruebas de validación**. para probar la migración de bases de datos, debe utilizar consultas SQL. Debe crear las consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.

2. **Configurar un entorno de prueba**. el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.

3. **Ejecutar pruebas de validación**. ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.

4. **Ejecutar pruebas de rendimiento**. ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.

### <a name="optimize"></a>Optimización

La fase posterior a la migración es fundamental para reconciliar cualquier problema de precisión de datos y comprobar su integridad, así como para solucionar problemas de rendimiento con la carga de trabajo.

Para obtener detalles adicionales sobre estos problemas y los pasos específicos para mitigarlos, consulte la [Guía de optimización y validación posterior a la migración](/sql/relational-databases/post-migration-validation-and-optimization-guide).

## <a name="migration-assets"></a>Recursos de migración

Para obtener más ayuda a fin de completar el escenario de migración, consulte los siguientes recursos, desarrollados para prestar apoyo en relación con un proyecto de migración real.

| Título/vínculo     | Descripción    |
| ---------------------------------------------- | ---------------------------------------------------- |
| [Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Esta herramienta proporciona las plataformas de destino de ajuste perfecto sugeridas, la preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo mediante un clic y una función de generación de informes que acelera las evaluaciones de gran extensión mediante un proceso de toma de decisiones uniforme y automatizado en relación con la plataforma de destino. |

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.

## <a name="next-steps"></a>Pasos siguientes 

- Asegúrese de consultar la [calculadora del costo total de propiedad (TCO) de Azure](https://aka.ms/azure-tco) para ayudar a calcular el ahorro de costos que puede obtener mediante la migración de sus cargas de trabajo a Azure.

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, así como con las tareas especializadas, consulte el artículo [Servicios y herramientas de migración de datos](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

- Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 

Para ver los vídeos, consulte: 
- [Información general sobre el recorrido de la migración y las herramientas y servicios recomendados para llevar a cabo la evaluación y la migración](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
