---
title: 'De Oracle a SQL Server en Azure VM: guía de migración'
description: En esta guía se explica cómo migrar los esquemas de Oracle a SQL Server en Azure VM mediante SQL Server Migration Assistant para Oracle.
ms.service: virtual-machines-sql
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: 1767f1f990326e513393b8ce47e1ed8485f73849
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656652"
---
# <a name="migration-guide-oracle-to-sql-server-on-azure-vm"></a>Guía de migración: Oracle a SQL Server en Azure VM
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

En esta guía se explica cómo migrar los esquemas de Oracle a SQL Server en Azure VM mediante SQL Server Migration Assistant para Oracle. 

Para ver otros escenarios, consulte la [Guía de migración de bases de datos](https://datamigration.microsoft.com/).

## <a name="prerequisites"></a>Requisitos previos 

Para migrar el esquema de Oracle a SQL Server en Azure VM, necesita lo siguiente:

- Comprobar que el entorno de origen es compatible.
- Descargar [SQL Server Migration Assistant (SSMA) para Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258).
- Una [VM con SQL Server](../../virtual-machines/windows/sql-vm-create-portal-quickstart.md) de destino.
- Los [permisos necesarios para SSMA para Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) y el [proveedor](/sql/ssma/oracle/connect-to-oracle-oracletosql).

## <a name="pre-migration"></a>Antes de la migración

Cuando prepare la migración a la nube, compruebe que el entorno de origen sea compatible y que haya satisfecho los requisitos previos. Esto ayudará a garantizar una migración correcta y eficaz.

Esta parte del proceso implica la elaboración de un inventario de las bases de datos que se deben migrar, la evaluación de las bases de datos en busca de posibles problemas de migración o bloqueadores, y la resolución de cualquier aspecto que pueda haber descubierto. 

### <a name="discover"></a>Descubra

Use [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883) para identificar los orígenes de datos existentes y los detalles sobre las características que su empresa utiliza para comprender y planear mejor la migración. Este proceso implica el examen de la red para identificar todas las instancias de Oracle de su organización junto con la versión y las características en uso.

Para usar MAP Toolkit para hacer un análisis de inventario, siga estos pasos: 

1. Abra [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883).
1. Seleccione **Create/Select database** (Crear o seleccionar base de datos).

   ![Seleccionar base de datos](./media/oracle-to-sql-on-azure-vm-guide/select-database.png)

1. Seleccione **Create an inventory database** (Crear una base de datos de inventario), escriba un nombre para la base de datos de inventario que va a crear, proporcione una breve descripción y, después, seleccione **OK** (Aceptar). 

   :::image type="content" source="media/oracle-to-sql-on-azure-vm-guide/create-inventory-database.png" alt-text="Creación de una base de datos de inventario":::

1. Seleccione **Collect inventory data** (Recopilar datos de inventario) para abrir **Inventory and Assessment Wizard** (Asistente para el inventario y evaluación). 

   :::image type="content" source="media/oracle-to-sql-on-azure-vm-guide/collect-inventory-data.png" alt-text="Recopilación de datos de inventario":::

1. En **Inventory and Assessment Wizard** (Asistente para el inventario y evaluación), elija **Oracle** y, a continuación, seleccione **Next** (Siguiente). 

   ![Selección de Oracle](./media/oracle-to-sql-on-azure-vm-guide/choose-oracle.png)

1. Elija la opción de búsqueda de equipos que mejor se adapte a su entorno y a sus necesidades empresariales y, a continuación, seleccione **Next** (Siguiente): 

   ![Selección de la opción de búsqueda de equipos más útil para las necesidades empresariales](./media/oracle-to-sql-on-azure-vm-guide/choose-search-option.png)

1. Escriba las credenciales o cree unas nuevas para los sistemas que quiera explorar y, a continuación, seleccione **Next** (Siguiente).

    ![Escribir credenciales](./media/oracle-to-sql-on-azure-vm-guide/choose-credentials.png)

1. Establezca el orden de las credenciales y, a continuación, seleccione **Next** (Siguiente). 

   ![Establecimiento del orden de las credenciales](./media/oracle-to-sql-on-azure-vm-guide/set-credential-order.png)  

1. Especifique las credenciales de cada equipo que quiera detectar. Puede usar unas credenciales únicas para cada equipo o máquina, o bien usar la lista **All Computer Credentials** (Todas las credenciales del equipo).  


   ![Especifique las credenciales de cada equipo que quiera detectar:](./media/oracle-to-sql-on-azure-vm-guide/specify-credentials-for-each-computer.png)


1. Compruebe el resumen de la selección y, a continuación, seleccione **Finish** (Finalizar).

   ![Revisión del resumen](./media/oracle-to-sql-on-azure-vm-guide/review-summary.png)

1. Una vez completado el análisis, vea el informe de resumen **Data Collection** (Recopilación de datos). El análisis tarda unos minutos y depende del número de bases de datos. Cuando haya terminado, seleccione **Close** (Cerrar). 

   ![Informe del resumen de la recopilación](./media/oracle-to-sql-on-azure-vm-guide/collection-summary-report.png)


1. Seleccione **Options** (Opciones) para generar un informe sobre los detalles de la base de datos y la evaluación de Oracle. Seleccione ambas opciones (una a una) para generar el informe.


### <a name="assess"></a>Evaluar

Después de identificar los orígenes de datos, use [SQL Server Migration Assistant (SSMA) para Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258) para evaluar las instancias de Oracle que se van a migrar a VM con SQL Server, de modo que comprenda las diferencias entre ambos. Con el asistente para la migración, puede revisar los datos y los objetos de base de datos, evaluar las bases de datos para la migración, migrar objetos de base de datos a SQL Server y, a continuación, migrar datos a SQL Server.

Para crear una evaluación, siga estos pasos: 

1. Abra [SQL Server Migration Assistant (SSMA) para Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258). 
1. Seleccione **Archivo** y, luego, elija **Nuevo proyecto**. 
1. Facilite el nombre del proyecto y una ubicación para guardarlo y, luego, seleccione un destino de la migración de SQL Server en la lista desplegable. Seleccione **Aceptar**. 

   ![Nuevo proyecto](./media/oracle-to-sql-on-azure-vm-guide/new-project.png)

1. Seleccione **Connect to Oracle** (Conectarse a Oracle). Escriba los valores de los detalles de la conexión de Oracle en el cuadro de diálogo **Conectarse a Oracle**.

   ![Conectar con Oracle](./media/oracle-to-sql-on-azure-vm-guide/connect-to-oracle.png)

   Seleccione los esquemas de Oracle que quiera migrar: 

   ![Selección del esquema de Oracle](./media/oracle-to-sql-on-azure-vm-guide/select-schema.png)

1. Haga clic con el botón secundario en el esquema de Oracle que desea migrar en **Oracle Metadata Explorer** (Explorador de metadatos de Oracle) y, a continuación, elija **Crear informe**. Se generará un informe HTML. También puede elegir **Crear informe** en la barra de navegación después de seleccionar la base de datos.

   ![Crear informe](./media/oracle-to-sql-on-azure-vm-guide/create-report.png)

1. En **Oracle Metadata Explorer** (Explorador de metadatos de Oracle), seleccione el esquema de Oracle y, a continuación, seleccione **Crear informe** para generar un informe HTML con estadísticas de conversión y errores y advertencias, si los hay.
1. Revise el informe HTML para ver las estadísticas de conversión, y los errores y las advertencias. Analícelo para comprender los problemas de conversión y las soluciones.

   También se puede acceder a este informe desde la carpeta de proyectos de SSMA, tal como se seleccionó en la primera pantalla. En el ejemplo anterior, busque el archivo report.xml en: 

   `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`

    Luego, ábralo en Excel para obtener un inventario de objetos de Oracle y conocer el esfuerzo necesario para realizar las conversiones de esquema.

   ![Informe de conversión](./media/oracle-to-sql-on-azure-vm-guide/conversion-report.png)



### <a name="validate-data-types"></a>Validación de los tipos de datos

Valide las asignaciones predeterminadas de los tipos de datos y cámbielas según los requisitos, si es necesario. Para hacerlo, siga estos pasos: 

1. Seleccione **Herramientas** en el menú principal. 
1. Seleccione **Configuración del proyecto**. 
1. Seleccione la pestaña **Type mappings** (Asignaciones de tipos). 

   ![Asignaciones de tipos](./media/oracle-to-sql-on-azure-vm-guide/type-mappings.png)

1. Puede cambiar la asignación de tipos de cada tabla seleccionando la tabla en el **Oracle Metadata Explorer** (Explorador de metadatos de Oracle). 



### <a name="convert-schema"></a>Convertir esquema

Para convertir el esquema, siga estos pasos: 

1. (Opcional) Para convertir consultas dinámicas o ad hoc, haga clic con el botón derecho en el nodo y elija **Agregar instrucción**.
1. Seleccione **Conectarse al servidor SQL Server** en la barra de navegación superior y proporcione los detalles de conexión de SQL Server en Azure VM. Puede optar por conectarse a una base de datos existente o proporcionar un nombre nuevo, en cuyo caso se creará una base de datos en el servidor de destino.

   ![Conexión a SQL](./media/oracle-to-sql-on-azure-vm-guide/connect-to-sql-vm.png)

1. Haga clic con el botón derecho en el esquema de Oracle en **Oracle Metadata Explorer** (Explorador de metadatos de Oracle) y elija **Convert Schema** (Convertir esquema).

   ![Convertir esquema](./media/oracle-to-sql-on-azure-vm-guide/convert-schema.png)

1. Una vez finalizada la conversión del esquema, compare y revise la estructura del esquema para identificar posibles problemas.

   ![Revisión de recomendaciones](./media/oracle-to-sql-on-azure-vm-guide/table-mapping.png)

   Compare el texto de Transact-SQL convertido al código original y revise las recomendaciones: 

   ![Revisión del código de recomendaciones](./media/oracle-to-sql-on-azure-vm-guide/procedure-comparison.png)

   Puede guardar el proyecto localmente para un ejercicio de corrección de esquema sin conexión. Para ello, seleccione **Guardar proyecto** en el menú **Archivo**. Esto le ofrece la oportunidad de evaluar los esquemas de origen y de destino sin conexión, y hacer correcciones para poder publicar el esquema en SQL Server.


## <a name="migrate"></a>Migrar

Una vez que haya satisfecho los requisitos previos necesarios y completado las tareas asociadas a la fase **previa a la migración**, estará a punto para ejecutar el esquema y la migración de datos. La migración implica dos pasos: publicar el esquema y migrar los datos. 


Para publicar el esquema y migrar los datos, siga estos pasos: 

1. Haga clic con el botón derecho en la base de datos en el **Explorador de metadatos de SQL Server** y elija **Synchronize with Database** (Sincronizar con base de datos). Esta acción publica el esquema de Oracle en SQL Server en Azure VM. 

   ![Sincronización con la base de datos](./media/oracle-to-sql-on-azure-vm-guide/synchronize-database.png)

   Revise el estado de sincronización: 

   ![Revisión del estado de sincronización](./media/oracle-to-sql-on-azure-vm-guide/synchronize-database-review.png)


1. Haga clic con el botón derecho en el esquema de Oracle en **Oracle Metadata Explorer** (Explorador de metadatos de Oracle) y elija **Migrar datos**. Como alternativa, puede seleccionar Migrar datos en la barra de navegación superior.

   ![Migración de los datos](./media/oracle-to-sql-on-azure-vm-guide/migrate-data.png)

1. Proporcione los detalles de conexión de Oracle y SQL Server en Azure VM en el cuadro de diálogo.
1. Una vez finalizada la migración, vea el informe de migración de datos:

    ![Informe de migración de datos](./media/oracle-to-sql-on-azure-vm-guide/data-migration-report.png)

1. Conéctese a su SQL Server mediante en Azure VM mediante [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) para revisar los datos y el esquema en la instancia de SQL Server. 

   ![Validación en SSMA](./media/oracle-to-sql-on-azure-vm-guide/validate-in-ssms.png)




Además de usar SSMA, también puede usar SQL Server Integration Services (SSIS) para migrar los datos. Para obtener más información, consulte: 
- El artículo [Introducción a SQL Server Integration Services](https://docs.microsoft.com//sql/integration-services/sql-server-integration-services).
- Las notas del producto [SQL Server Integration Services: SSIS para Azure y el movimiento de datos híbridos](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx).



## <a name="post-migration"></a>Etapa posterior a la migración 

Cuando haya completado correctamente la fase de **migración**, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.

[Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) es una extensión para Visual Studio Code que le permite analizar el código fuente Java, y detectar las llamadas y consultas de API de acceso a datos, lo que le proporciona una vista en un solo panel de lo que debe solucionarse para admitir el nuevo back-end de base de datos. Para obtener más información, consulte el blog [Migración de la aplicación Java desde Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727). 

### <a name="perform-tests"></a>Realización de pruebas

El enfoque de prueba para la migración de bases de datos consiste en realizar las siguientes actividades:

1. **Desarrollar pruebas de validación**. para probar la migración de bases de datos, debe utilizar consultas SQL. Debe crear las consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.

2. **Configurar un entorno de prueba**. el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.

3. **Ejecutar pruebas de validación**. ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.

4. **Ejecutar pruebas de rendimiento**. ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.

### <a name="optimize"></a>Optimización

La fase posterior a la migración es fundamental para reconciliar cualquier problema de precisión de datos y comprobar su integridad, así como para solucionar problemas de rendimiento con la carga de trabajo.

> [!Note]
> Para obtener detalles adicionales sobre estos problemas y los pasos específicos para mitigarlos, consulte la [Guía de optimización y validación posterior a la migración](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda a fin de completar el escenario de migración, consulte los siguientes recursos, desarrollados para prestar apoyo a un proyecto de migración real.

| **Título/vínculo**                                                                                                                                          | **Descripción**                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Esta herramienta proporciona sugerencias de plataformas de destino "ideales", preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo mediante un clic y una función de generación de informes que acelera las evaluaciones de gran extensión mediante un proceso de toma de decisiones uniforme y automatizado en relación con la plataforma de destino.                                                          |
| [Artefactos de script de inventario de Oracle](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Este recurso incluye una consulta PL/SQL que visita las tablas del sistema de Oracle y proporciona un recuento de objetos por tipo de esquema, tipo de objeto y estado. También proporciona una estimación aproximada de "datos sin procesar" en cada esquema y el dimensionamiento de las tablas de cada esquema, con resultados almacenados en formato CSV.                                                                                                               |
| [Automatización de la recopilación y consolidación de evaluaciones de SSMA para Oracle](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Este conjunto de recursos usa un archivo .csv como entrada (sources.csv en las carpetas del proyecto) para generar los archivos XML necesarios para ejecutar la evaluación de SSMA en modo de consola. El cliente proporciona el archivo source.csv basado en un inventario de las instancias de Oracle existentes. Los archivos de salida son AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml y VariableValueFile.xml.|
| [Errores comunes de SSMA para Oracle y procedimiento para corregirlos](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | Con Oracle, puede asignar una condición no escalar en la cláusula WHERE. Sin embargo, SQL Server no admite este tipo de condición. Como resultado, SQL Server Migration Assistant (SSMA) para Oracle no convierte las consultas con una condición no escalar en la cláusula WHERE, sino que se genera un error O2SS0001. Estas notas del producto proporcionan más detalles sobre el problema y las maneras de resolverlo.          |
| [Manual de migración de Oracle a SQL Server](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | Este documento se centra en las tareas asociadas a la migración de un esquema de Oracle a la versión más reciente de SQL Server. Si la migración requiere cambios en las características o funcionalidades, se debe tener muy en cuenta el posible impacto de cada cambio en las aplicaciones que usan la base de datos.                                                     |

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.

## <a name="next-steps"></a>Pasos siguientes

- Para comprobar la disponibilidad de los servicios aplicables a SQL Server, consulte el [centro de infraestructura global de Azure](https://azure.microsoft.com/global-infrastructure/services/?regions=all&amp;products=synapse-analytics,virtual-machines,sql-database).

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, así como con las tareas especializadas, consulte el artículo [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para obtener más información acerca de Azure SQL, consulte:
   - [Opciones de implementación](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [SQL Server en máquinas virtuales de Azure](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 


- Para más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Para más información acerca de las licencias, consulte:
   - [Enfoque "traiga su propia licencia" con la Ventaja híbrida de Azure](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [Obtención de soporte extendido gratuito de SQL Server 2008 y SQL Server 2008 R2](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)


- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para más información sobre cómo realizar pruebas A/B en la capa de acceso a datos, consulte [Información general del Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).

