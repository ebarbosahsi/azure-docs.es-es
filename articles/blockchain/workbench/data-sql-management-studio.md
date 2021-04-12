---
title: Consulta de los datos de Azure Blockchain Workbench mediante SQL Server Management Studio
description: Obtenga información sobre cómo conectarse a la base de datos de SQL de Azure Blockchain desde SQL Server Management Studio.
ms.date: 11/20/2019
ms.topic: how-to
ms.service: azure-blockchain
ms.reviewer: mmercuri
ms.openlocfilehash: d34affa93af65acf5fd7d6558e2d8077a2efc360
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105625167"
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Usar los datos de Azure Workbench Blockchain con SQL Server Management Studio

Microsoft SQL Server Management Studio proporciona la capacidad para escribir y probar consultas rápidamente en la base de datos de SQL de Azure Blockchain Workbench. En esta sección se incluye un tutorial paso a paso sobre cómo conectarse a la base de datos de SQL Database de Azure Blockchain Workbench desde SQL Server Management Studio.

## <a name="prerequisites"></a>Requisitos previos

* Descargue [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms).

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>Conectar SQL Server Management Studio a los datos de Azure Blockchain Workbench

1. Abra SQL Server Management Studio y seleccione **Conectar**.
2. Seleccione **Motor de base de datos**.

    ![Motor de base de datos](./media/data-sql-management-studio/database-engine.png)

3. En el cuadro de diálogo **Conectar al servidor**, escriba el nombre del servidor y las credenciales de la base de dataos.

    Si usa las credenciales creadas por el proceso de implementación de Azure Blockchain Workbench, el nombre de usuario es **dbadmin** y la contraseña es la que proporcionó durante la implementación.

    ![Especificar las credenciales de SQL](./media/data-sql-management-studio/sql-creds.png)

   1. SQL Server Management Studio muestra la lista y vistas de bases de datos, los procedimientos almacenados en la base de datos de Azure Blockchain Workbench.

      ![Lista de base de datos](./media/data-sql-management-studio/db-list.png)

5. Para ver los datos asociados a cualquiera de las vistas de base de datos, puede generar automáticamente una instrucción select mediante los siguientes pasos.
6. Haga clic con el botón derecho en cualquiera de las vistas de base de datos en el Explorador de objetos.
7. Seleccione **Incluir vista como**.
8. Elija **SELECT To**.
9. Seleccione **Nueva ventana del Editor de consultas**.
10. Una consulta nueva se puede crear al seleccionar **Nueva consulta**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vistas de base de datos de Azure Blockchain Workbench](database-views.md)
