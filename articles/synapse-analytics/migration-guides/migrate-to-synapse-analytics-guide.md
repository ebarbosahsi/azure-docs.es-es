---
title: 'Azure Synapse Analytics: Guía de migración'
description: Siga esta guía para migrar sus bases de datos al grupo de SQL dedicado de Azure Synapse Analytics.
ms.service: synapse-analytics
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: julieMSFT
ms.author: jrasnick
ms.reviewer: jrasnick
ms.date: 03/10/2021
ms.openlocfilehash: aa32083d48c868435ce77114f3a036b8bec47201
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608711"
---
# <a name="migrating-a-data-warehouse-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Migración de un almacenamiento de datos a un grupo de SQL dedicado en Azure Synapse Analytics 
Las siguientes secciones describen los aspectos relativos a la migración de una solución de almacenamiento de datos existente al grupo de SQL dedicado de Azure Synapse Analytics.

## <a name="overview"></a>Información general
Antes de efectuar la migración, debe comprobar que Azure Synapse Analytics es la solución idónea para su carga de trabajo. Azure Synapse Analytics es un sistema distribuido diseñado para analizar grandes volúmenes de datos. La migración a Azure Synapse Analytics requiere algunos cambios en el diseño que no son difíciles de entender, pero que pueden tardar algún tiempo en implementarse. Si su negocio requiere un almacenamiento de datos de clase empresarial, las ventajas merecen la pena. Aun así, si no necesita la eficacia de Azure Synapse Analytics, es más rentable usar [SQL Server](https://docs.microsoft.com/sql/sql-server/) o [Azure SQL Database](https://docs.microsoft.com/azure/azure-sql/).

Considere la posibilidad de usar Azure Synapse Analytics en los casos siguientes:
- Tiene uno o varios terabytes de datos.
- Tiene previsto analizar en grandes volúmenes de datos.
- Necesita escalar los recursos de proceso y almacenamiento.
- Desea ahorrar costos mediante la interrupción de los recursos de proceso cuando no los necesite.

En lugar de usar Azure Synapse Analytics, valore otras opciones para las cargas de trabajo operativas (OLTP) que tienen las siguientes características:
- Operaciones de lectura y escritura muy frecuentes.
- Un número elevado de selecciones de base de datos única.
- Un elevado número de inserciones de fila única.
- Requisitos de procesamiento fila por fila.
- Formatos incompatibles (JSON, XML).

## <a name="azure-synapse-pathway"></a>Azure Synapse Pathway
Uno de los principales obstáculos a los que se enfrentan los clientes es la conversión de los objetos de base de datos al migrar de un sistema a otro. [Azure Synapse Pathway](https://docs.microsoft.com/sql/tools/synapse-pathway/azure-synapse-pathway-overview) le ayuda a realizar la actualización a una plataforma moderna de almacenamiento de datos mediante la automatización de la conversión de objetos del almacenamiento de datos existente. Es una herramienta gratuita, intuitiva y fácil de usar que automatiza la traducción de código, lo que permite una migración más rápida a Azure Synapse Analytics.

## <a name="prerequisites"></a>Prerrequisitos
# <a name="migrate-from-sql-server"></a>[Migración desde SQL Server](#tab/migratefromSQLServer)
Para migrar el almacenamiento de datos de SQL Server a Azure Synapse Analytics, asegúrese de que cumple los siguientes requisitos previos: 

- Una carga de trabajo de análisis o de almacenamiento de datos. 
- Descargue la versión más reciente de la herramienta [Azure Synapse Pathway](https://www.microsoft.com/en-us/download/details.aspx?id=102787) para migrar objetos de SQL Server a objetos de Azure Synapse.
- Un [grupo de SQL dedicado](../get-started-create-workspace.md) en el área de trabajo de Azure Synapse. 

# <a name="migrate-from-netezza"></a>[Migración desde Netezza](#tab/migratefromNetezza)
Para migrar el almacenamiento de datos de Netezza a Azure Synapse Analytics, asegúrese de que cumple los siguientes requisitos previos: 

- Descargue la versión más reciente de la herramienta [Azure Synapse Pathway](https://www.microsoft.com/en-us/download/details.aspx?id=102787) para migrar objetos de SQL Server a objetos de Azure Synapse.
- Un [grupo de SQL dedicado](../get-started-create-workspace.md) en el área de trabajo de Azure Synapse.

Para obtener más información, visite [Soluciones y migración de Azure Synapse Analytics en Netezza](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/analytics/analytics-solutions-netezza).

# <a name="migrate-from-snowflake"></a>[Migración desde Snowflake](#tab/migratefromSnowflake)
Para migrar el almacenamiento de datos de Snowflake a Azure Synapse Analytics, asegúrese de que cumple los siguientes requisitos previos: 

- Descargue la versión más reciente de la herramienta [Azure Synapse Pathway](https://www.microsoft.com/en-us/download/details.aspx?id=102787) para migrar objetos de Snowflake a objetos de Azure Synapse.
- Un [grupo de SQL dedicado](../get-started-create-workspace.md) en el área de trabajo de Azure Synapse. 

# <a name="migrate-from-oracle"></a>[Migración desde Oracle](#tab/migratefromOracle)
Para migrar el almacenamiento de datos de Oracle a Azure Synapse Analytics, asegúrese de que cumple los siguientes requisitos previos: 

- Una carga de trabajo de análisis o de almacenamiento de datos. 
- Descargue SSMA para Oracle para convertir objetos de Oracle a SQL Server. Consulte [Migración de bases de datos de Oracle a SQL Server (OracleToSQL)](https://docs.microsoft.com/sql/ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql) para obtener más información. 
- Descargue la versión más reciente de la herramienta [Azure Synapse Pathway](https://www.microsoft.com/download/details.aspx?id=102787) para migrar objetos de SQL Server a objetos de Azure Synapse.
- Un [grupo de SQL dedicado](../get-started-create-workspace.md) en el área de trabajo de Azure Synapse. 

Para más información, visite [Soluciones y migración de Azure Synapse Analytics para un almacenamiento de datos de Oracle](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/analytics/analytics-solutions-exadata).

---

## <a name="pre-migration"></a>Antes de la migración
Después de tomar la decisión de migrar una solución existente a Azure Synapse Analytics, es importante que planee la migración antes de empezar. Un objetivo primordial de la planeación es garantizar que los datos, los esquemas de tabla y el código sean compatibles con Azure Synapse Analytics. Existen algunas diferencias de compatibilidad entre su sistema actual y Azure Synapse Analytics que tendrá que solucionar. Además, la migración de grandes cantidades de datos a Azure lleva tiempo. Una planeación cuidadosa acelerará el proceso de obtención de los datos en Azure. Otro objetivo importante de la planeación es ajustar el diseño para garantizar que la solución aproveche al máximo el alto rendimiento de las consultas que el diseño de Azure Synapse Analytics facilita. El diseño de almacenamientos de datos con fines de escalabilidad presenta diferentes patrones de diseño, por lo que los enfoques tradicionales no son siempre los mejores. Aunque después de la migración se pueden hacer algunos ajustes en el diseño, llevarlos acabo antes en el proceso le ahorrará tiempo posteriormente.

## <a name="migrate"></a>Migrar
Para que la migración sea satisfactoria, es necesario migrar los esquemas de tabla, el código y los datos. Para obtener directrices detalladas, consulte los artículos sobre los siguientes temas:
- Artículo sobre la [consideración del diseño de tablas](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-overview).
- Artículo sobre la [consideración del cambio de código](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop#development-recommendations-and-coding-techniques).
- [Migración de los datos](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading).
- Artículo sobre la [consideración de la administración de cargas de trabajo](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-workload-management).

## <a name="additional-resources"></a>Recursos adicionales 
- El equipo de asesoramiento al cliente (CAT) ofrece una excelente orientación sobre Azure Synapse Analytics (anteriormente SQL DW) a través de entradas de blog. Consulte el artículo sobre la [migración de datos a Azure SQL Data Warehouse en la práctica](https://docs.microsoft.com/archive/blogs/sqlcat/migrating-data-to-azure-sql-data-warehouse-in-practice) para obtener más directrices de migración.

## <a name="migration-assets-from-real-world-engagements"></a>Recursos de migración para compromisos reales
Para obtener más ayuda a fin de completar el escenario de migración, consulte los siguientes recursos, desarrollados para dar apoyo en relación con proyecto de migración real:

| Título/vínculo                              | Descripción                                                                                                                       |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Esta herramienta proporciona las "mejores" plataformas de destino recomendadas, el nivel de preparación de la nube y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo mediante un clic y una función de generación de informes que acelera las evaluaciones de gran extensión mediante un proceso de toma de decisiones uniforme y automatizado en relación con la plataforma de destino. |
| [Administración de problemas de codificación al cargar datos en Azure Synapse Analytics](https://azure.microsoft.com/en-us/blog/handling-data-encoding-issues-while-loading-data-to-sql-data-warehouse/) | Este blog está pensado para ofrecer perspectivas sobre algunos de los problemas de codificación de datos que puede encontrar al usar PolyBase para cargar datos en SQL Data Warehouse. Este artículo también ofrece algunas opciones que puede usar para solucionar los problemas y cargar los datos correctamente. |
| [Obtención de los tamaños de tabla en el grupo de SQL dedicado de Azure Synapse Analytics](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Getting%20table%20sizes%20in%20SQL%20DW.pdf) | Una de las principales tareas que un arquitecto debe llevar a cabo consiste en obtener métricas sobre un nuevo entorno después de la migración. Por ejemplo, recopilar los tiempos de carga del entorno local a la nube o los tiempos de carga de PolyBase. Entre estas tareas, una de las más importantes es determinar el tamaño de almacenamiento en SQL Data Warehouse en comparación con la plataforma existente del cliente. |
| [Utilidad para migrar los inicios de sesión de SQL Server del entorno local a Azure Synapse Analytics](https://github.com/Microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) | Un script de PowerShell que crea un script de comandos de T-SQL para volver a crear los inicios de sesión y seleccionar usuarios de base de datos de una instancia local de SQL Server para un servicio PaaS de Azure SQL. La herramienta permite asignar automáticamente cuentas de Windows AD a Azure AD. También puede hacer búsquedas del nombre principal de usuario para cada inicio de sesión en la instancia local de Windows Active Directory. Además la herramienta migra los inicios de sesión nativos de SQL Server. Los roles de servidor y de base de datos personalizados se incluyen en scripts, además de la pertenencia a roles, el rol de base de datos y los permisos de usuario. Las bases de datos independientes todavía no se admiten y solo un subconjunto de posibles permisos de SQL Server se incluyen en el script, es decir, no se admite la concesión de permisos Grant (árboles de permisos complejos). Hay más detalles disponibles en la documentación complementaria y el script contiene comentarios para facilitar la comprensión. |

> [!NOTE]
> Los recursos anteriores se desarrollaron como parte del programa Data Migration Jumpstart (DM Jumpstart), patrocinado por el equipo de ingeniería de Azure Data Group. El aspecto principal del programa DM Jumpstart es facilitar y acelerar la compleja modernización y las oportunidades de migración de las plataformas de datos de la competencia a la plataforma de datos de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa DM Jumpstart, póngase en contacto con su equipo de cuentas y pídale que envíen una propuesta.

## <a name="videos"></a>Vídeos
- Vea cómo [Walgreens migró su sistema de inventario minorista](https://www.youtube.com/watch?v=86dhd8N1lH4) con unos 100 TB de datos de Netezza a Azure Synapse Analytics (anteriormente SQL DW) en un tiempo récord. 