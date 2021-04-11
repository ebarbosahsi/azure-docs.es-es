---
title: Incorporación de herramientas de evaluación en Azure Migrate
description: Obtenga información sobre como agregar herramientas de evaluación en Azure Migrate.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: 37f3748b4f0f3db47bbd6fbe9bc06a307781c2f8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104786811"
---
# <a name="add-assessment-tools"></a>Incorporación de herramientas de evaluación

En este artículo se describe cómo agregar herramientas de valoración en [Azure Migrate](./migrate-services-overview.md). 

- Si quiere agregar una herramienta de valoración y todavía no tiene un proyecto de Azure Migrate, consulte este [artículo](create-manage-projects.md).
- Si ha agregado una herramienta de ISV o Movere para la valoración, [siga los pasos](prepare-isv-movere.md) de preparación para trabajar con la herramienta.

## <a name="select-an-assessment-scenario"></a>Selección de un escenario de valoración

1. En el proyecto de Azure Migrate, haga clic en **Información general**.
2. Seleccione un escenario de valoración:

    - Para detectar, evaluar y migrar servidores (físicos o virtuales) desde su centro de recursos u otras nubes a Azure, seleccione **Detectar, evaluar y migrar**. Ahora también puede detectar y evaluar SQL Server desde su entorno de VMware con este objetivo de migración.
    - Para evaluar bases de datos de SQL Server locales, seleccione **Evaluar y migrar bases de datos**.
    - Para valorar o migrar aplicaciones web locales, seleccione **Explorar más** > **Aplicaciones web**.
    - Para valorar la infraestructura de escritorio virtual, seleccione **Explorar más** > **Infraestructura de escritorio virtual**.

    ![Opciones para seleccionar un escenario de valoración](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-discovery-and-assessment-tool"></a>Seleccione una herramienta de detección y evaluación 


1. Agregue una herramienta:

    - Si creó un proyecto de Azure Migrate a través de la opción **Evaluar y migrar servidores** en el portal, la herramienta de detección y evaluación de Azure Migrate se agrega automáticamente al proyecto. Para agregar más herramientas de evaluación, en **Windows, Linux y SQL Server**, junto a **Herramientas de evaluación**, seleccione **Agregar más herramientas**.

         ![Botón para agregar herramientas de evaluación adicionales](./media/how-to-assess/add-assessment-tool.png)

    - Si creó un proyecto con una opción diferente y aún no tiene ninguna herramienta de evaluación, en las herramientas de evaluación de **Windows, Linux y SQL Server** >  **,** seleccione **Haga clic aquí**.

        ![Botón para agregar la primera herramienta de evaluación](./media/how-to-assess/no-assessment-tool.png)

2. En **Azure Migrate** > **Agregar herramientas**, seleccione las herramientas que quiera agregar. A continuación, seleccione **Agregar herramienta**.

    ![Selección de una herramienta de evaluación de la lista](./media/how-to-assess/select-assessment-tool.png)



## <a name="select-a-database-assessment-tool"></a>Selección de una herramienta de valoración de base de datos

1. Agregue una herramienta:

    - Si creó un proyecto de Azure Migrate a través de la opción **Evaluar y migrar bases de datos** en el portal, la herramienta Database Assessment se agrega automáticamente al proyecto. Para agregar más herramientas de evaluación, en **Bases de datos**, junto a **Herramientas de evaluación**, seleccione **Agregar más herramientas**.

    - Si creó un proyecto con una opción diferente y aún no tiene ninguna herramienta de evaluación de bases de datos, en **Bases de datos** > **Herramientas de evaluación**, seleccione **Haga clic aquí**.

2. En **Azure Migrate** > **Agregar herramientas**, seleccione las herramientas que quiera agregar. A continuación, seleccione **Agregar herramienta**.

    ![Selección de una herramienta de evaluación de bases de datos de la lista](./media/how-to-assess/select-database-assessment-tool.png)


## <a name="select-a-web-app-assessment-tool"></a>Selección de una herramienta de valoración de aplicaciones web

Si creó un proyecto de Azure Migrate a través de la opción **Explorar más** > **Web Apps** en el portal, la herramienta Web App Assessment se agrega automáticamente al proyecto. 


1. Si la herramienta Web App Assessment no está en el proyecto, en **Web Apps** > **Herramientas de evaluación**, seleccione **Haga clic aquí**.
    
    ![Incorporación de herramientas e evaluación de aplicaciones web](./media/how-to-assess/no-web-app-assessment-tool.png)


2. En **Azure Migrate** > **Agregar herramientas**, seleccione la herramienta Web App Assessment. A continuación, seleccione **Agregar herramienta**.

    ![Selección de la herramienta de migración de bases de datos de la lista](./media/how-to-assess/select-web-app-assessment-tool.png)

 


## <a name="next-steps"></a>Pasos siguientes

Detecte servidores locales para evaluarlos a través de la herramienta de detección y evaluación de Azure Migrate para [VMware](./tutorial-discover-vmware.md), [Hyper-V](./tutorial-discover-hyper-v.md) o [servidores físicos](./tutorial-discover-physical.md)