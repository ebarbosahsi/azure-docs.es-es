---
title: 'De Access a Azure SQL Database: guía de migración'
description: En esta guía se explica cómo migrar las bases de datos de Microsoft Access a Azure SQL Database mediante SQL Server Migration Assistant para Access (SSMA para Access).
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: e323b1c15d78da4e8c1a82ae8848df7f59b0dd87
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657363"
---
# <a name="migration-guide-access-to-azure-sql-database"></a>Guía de migración: de Access a Azure SQL Database

En esta guía de migración se explica cómo migrar las bases de datos de Microsoft Access a Azure SQL Database mediante SQL Server Migration Assistant para Access.

Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Requisitos previos

Para migrar la base de datos de Access a Azure SQL Database, necesita lo siguiente:

- Comprobar que el entorno de origen es compatible. 
- [SQL Server Migration Assistant para Access](https://www.microsoft.com/download/details.aspx?id=54255). 

## <a name="pre-migration"></a>Antes de la migración

Una vez cumplidos los requisitos previos, estará a punto para detectar la topología de su entorno y evaluar la viabilidad de la migración.


### <a name="assess"></a>Evaluar 

Cree una valoración mediante [SQL Server Migration Assistant para Access](https://www.microsoft.com/download/details.aspx?id=54255). 

Para crear una evaluación, siga estos pasos: 

1. Abra SQL Server Migration Assistant para Access. 
1. Seleccione **Archivo** y, luego, elija **Nuevo proyecto**. Proporcione un nombre para el proyecto de migración. 

   ![Elección de nuevo proyecto](./media/access-to-sql-database-guide/new-project.png)

1. Seleccione **Agregar bases de datos** y elija las bases de datos que se van a agregar al nuevo proyecto. 

   ![Elección de agregar bases de datos](./media/access-to-sql-database-guide/add-databases.png)

1. En el **Explorador de metadatos de Access**, haga clic con el botón derecho en la base de datos y, a continuación, elija **Crear informe**. 

   ![Haga clic con el botón derecho en la base de datos y elija Crear informe.](./media/access-to-sql-database-guide/create-report.png)

1. Revise la valoración de ejemplo. Por ejemplo: 

   ![Revise la evaluación del informe de ejemplo.](./media/access-to-sql-database-guide/sample-assessment.png)

### <a name="validate-data-types"></a>Validación de los tipos de datos

Valide las asignaciones predeterminadas de los tipos de datos y cámbielas según los requisitos, si es necesario. Para hacerlo, siga estos pasos:

1. Seleccione **Herramientas** en el menú principal. 
1. Seleccione **Configuración del proyecto**. 
1. Seleccione la pestaña **Type mappings** (Asignaciones de tipos). 

   ![Asignaciones de tipos](./media/access-to-sql-database-guide/type-mappings.png)

1. Puede cambiar la asignación de tipos de cada tabla seleccionando la tabla en el **Explorador de metadatos de Access**.


### <a name="convert-schema"></a>Convertir esquema

Para convertir objetos de base de datos, siga estos pasos: 

1. Seleccione **Conectarse a Azure SQL Database** y proporcione los detalles de la conexión.

   ![Conexión a Azure SQL Database](./media/access-to-sql-database-guide/connect-to-sqldb.png)

1. Haga clic con el botón derecho en la base de datos del **Explorador de metadatos de Access** y elija **Convertir esquema**. Como alternativa, puede seleccionar **Convert Schema** (Convertir esquema) en la barra de navegación superior tras seleccionar la base de datos:

   ![Haga clic con el botón derecho en la base de datos y elija Convert Schema (Convertir esquema).](./media/access-to-sql-database-guide/convert-schema.png)

   Compare las consultas convertidas con consultas originales: 

   ![Las consultas convertidas se pueden comparar con el código fuente.](./media/access-to-sql-database-guide/query-comparison.png)

   Compare objetos convertidos con objetos originales: 

   ![Los objetos convertidos se pueden comparar con el origen.](./media/access-to-sql-database-guide/table-comparison.png)

1. (Opcional) Para convertir un objeto individual, haga clic con el botón derecho en el objeto y elija **Convertir esquema**. Los objetos convertidos aparecen en negrita en el **Explorador de metadatos de Access**: 

   ![Objetos ya convertidos en el Explorador de metadatos, en negrita](./media/access-to-sql-database-guide/converted-items.png)
 
1. Seleccione **Revisar resultados** en el panel de resultados y revise los errores del panel **Lista de errores**. 


## <a name="migrate"></a>Migrar

Cuando haya terminado de evaluar las bases de datos y de resolver las discrepancias, el siguiente paso consiste en ejecutar el proceso de migración. La migración de datos es una operación de carga masiva que mueve filas de datos a Azure SQL Database en transacciones. El número de filas que se van a cargar en Azure SQL Database en cada transacción se configura en la configuración del proyecto.

Para migrar datos mediante SSMA para Access, siga estos pasos: 

1. Si aún no lo ha hecho, seleccione **Conectarse a Azure SQL Database** y proporcione los detalles de la conexión. 
1. Haga clic con el botón derecho en la base de datos del **Explorador de metadatos de Azure SQL Database** y elija **Sincronizar con base de datos**. Esta acción publica el esquema de MySQL en Azure SQL Database.

   ![Sincronización con la base de datos](./media/access-to-sql-database-guide/synchronize-with-database.png)

   Revise la asignación entre el proyecto de origen y el de destino:

   ![Revisión de la sincronización con la base de datos](./media/access-to-sql-database-guide/synchronize-with-database-review.png)

1. Use el **Explorador de metadatos de Access** para activar las casillas situadas junto a los elementos que quiera migrar. Si quiere migrar toda la base de datos, active la casilla situada junto a la base de datos. 
1. Haga clic con el botón derecho en la base de datos u objeto que quiera migrar y elija **Migrar datos**. 
   Para migrar datos de toda una base de datos, active la casilla situada junto al nombre de la base de datos. Para migrar datos de tablas concretas, expanda la base de datos, expanda Tablas y, a continuación, active la casilla situada junto a la tabla. Para omitir datos de tablas concretas, desactive la casilla.

    ![Migración de los datos](./media/access-to-sql-database-guide/migrate-data.png)

    Elimine los datos migrados: 

    ![Revisión de la migración de los datos](./media/access-to-sql-database-guide/migrate-data-review.png)

1. Conéctese a la instancia de Azure SQL Database mediante [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms), y revise los datos y el esquema para validar la migración.

   ![Validación en SSMA](./media/access-to-sql-database-guide/validate-data.png)



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

Para obtener más ayuda a fin de completar el escenario de migración, consulte los siguientes recursos, desarrollados para prestar apoyo a un proyecto de migración real.

| **Título/vínculo**                                                                                                                                          | **Descripción**                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Esta herramienta proporciona sugerencias de plataformas de destino "ideales", preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo mediante un clic y una función de generación de informes que acelera las evaluaciones de gran extensión mediante un proceso de toma de decisiones uniforme y automatizado en relación con la plataforma de destino. |


Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, además de las tareas especializadas, consulte [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para más información acerca de Azure SQL Database, consulte:
   - [Información general sobre SQL Database](../../database/sql-database-paas-overview.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 


- Para más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para más información sobre cómo realizar pruebas A/B en la capa de acceso a datos, consulte [Información general del Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).

