---
title: Detección de instancias de SQL Server en un proyecto existente de Azure Migrate
description: Aprenda a detectar instancias de SQL Server en un proyecto existente de Azure Migrate.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: how-to
ms.date: 03/23/2021
ms.openlocfilehash: 2de60880b511e43ffb2949a15fec2cf2a94f62fa
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105567159"
---
# <a name="discover-sql-server-instances-in-an-existing-project"></a>Detección de instancias de SQL Server en un proyecto existente 

En este artículo se describe cómo detectar bases de datos e instancias de SQL Server en un proyecto de [Azure Migrate](./migrate-services-overview.md) creado antes de la versión preliminar de la característica de valoración de Azure SQL.

La detección de bases de datos e instancias de SQL Server que se ejecutan en máquinas locales ayuda a identificar una ruta de migración a Azure SQL y a adaptarla. Para realizar la detección, el dispositivo de Azure Migrate usa las credenciales de dominio o las credenciales de autenticación de SQL Server que tienen acceso a las bases de datos e instancias de SQL Server que se ejecutan en los servidores de destino. Este proceso de detección se realiza sin agente, es decir, no se instalada nada en los servidores de destino.

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha:
    - Creado un [proyecto de Azure Migrate](./create-manage-projects.md) antes de anunciar la característica de valoración de SQL para su región
    - Agregado la herramienta para [Azure Migrate: detección y valoración](./how-to-assess.md) a un proyecto
- Revise [la compatibilidad y los requisitos de la detección de aplicaciones](./migrate-support-matrix-vmware.md#vmware-requirements).
-  Asegúrese de que los servidores en los que se ejecuta la detección de aplicaciones tengan instaladas la versión de PowerShell 2.0 o posterior y las herramientas de VMware (posterior a 10.2.0).
- Compruebe los [requisitos ](./migrate-appliance.md) para implementar el dispositivo de Azure Migrate.
- Compruebe que tiene los [roles necesarios](./create-manage-projects.md#verify-permissions) en la suscripción para crear los recursos.
- Asegúrese de que el dispositivo tenga acceso a Internet.

## <a name="enable-discovery-of-sql-server-instances-and-databases"></a>Habilitar la detección de bases de datos e instancias de SQL Server

1. En el proyecto de Azure Migrate:
    - Seleccione **Sin habilitar** en el mosaico del centro, o bien :::image type="content" source="./media/how-to-discover-sql-existing-project/hub-not-enabled.png" alt-text="Mosaico del centro de Azure Migrate con la detección de SQL no habilitada":::.
    - Seleccione **Sin habilitar** en cualquiera de las entradas de la página de detección de servidores, en la columna de instancias de SQL :::image type="content" source="./media/how-to-discover-sql-existing-project/discovery-not-enabled.png" alt-text="Hoja de servidores detectados de Azure Migrate con la detección de SQL sin habilitar":::
2. En la página para detectar las bases de datos e instancias de SQL Server, siga los pasos que se indican:
    - Seleccione la opción de **Actualizar** para crear el recurso necesario.
        :::image type="content" source="./media/how-to-discover-sql-existing-project/discovery-upgrade-appliance.png" alt-text="Botón para actualizar el dispositivo de Azure Migrate":::
    - Compruebe que los servicios que se ejecutan en el dispositivo estén actualizados a las últimas versiones. Para ello, inicie el administrador de configuración del dispositivo desde el servidor de este y seleccione la opción para ver los servicios del dispositivo en el panel de configuración de requisitos previos.
        - El dispositivo y sus componentes se actualizan de forma automática :::image type="content" source="./media/how-to-discover-sql-existing-project/appliance-services-version.png" alt-text="Comprobar la versión del dispositivo":::
    - En el panel de administración de credenciales y orígenes de detección del administrador de configuración del dispositivo, agregue las credenciales de autenticación de SQL Server o de dominio que tengan acceso de administrador del sistema a las bases de datos y a la instancia de SQL Server que se van a detectar.
    Puede aprovechar la característica de asignación de credenciales automática del dispositivo o bien asignar las credenciales al servidor respectivo de forma manual, tal y como se resalta [aquí](./tutorial-discover-vmware.md#start-continuous-discovery).

    Algunos puntos a tener en cuenta:
    - Asegúrese de que el inventario de software ya esté habilitado o proporcione credenciales que sean de dominio o no para habilitarlo. Debe realizarse el inventario de software para detectar las instancias de SQL Server.
    - El dispositivo intentará validar las credenciales de dominio con AD, a medida que se agreguen. Asegúrese de que el servidor del dispositivo tenga una línea de visión de red al servidor de AD asociado a las credenciales. Las credenciales asociadas con la autenticación de SQL Server no se validan.

3. Una vez agregadas las credenciales que desee, seleccione Iniciar la detección para comenzar el examen.

> [!Note]
>Deje que la detección de SQL se ejecute durante un tiempo antes de crear valoraciones para Azure SQL. Si no se permite que se complete la detección de bases de datos e instancias de SQL Server, las instancias respectivas se marcan como **Desconocidas** en el informe de valoración.

## <a name="next-steps"></a>Pasos siguientes

- Aprender a crear una [valoración de Azure SQL](./how-to-create-azure-sql-assessment.md)
- Obtener más información sobre las [valoraciones de Azure SQL](./concepts-azure-sql-assessment-calculation.md)