---
title: Referencia de la API de administración de Azure SQL Managed Instance
description: Obtenga información sobre cómo crear y configurar instancias administradas de Azure SQL Managed Instance.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: devx-track-azurecli
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: cf100861705bf1654b7206445c884b5fe315b06d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92792640"
---
# <a name="managed-api-reference-for-azure-sql-managed-instance"></a>Referencia de la API de administración de Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Puede crear y administrar instancias administradas de Azure SQL Managed Instance mediante Azure Portal, PowerShell, la CLI de Azure, la API REST y Transact-SQL. En este artículo, puede encontrar información general sobre las funciones y la API que puede usar para crear y configurar instancias administradas.

## <a name="azure-portal-create-a-managed-instance"></a>Azure Portal: Creación de una instancia administrada

Para una guía de inicio rápido que muestra cómo crear una instancia administrada, consulte [Guía de inicio rápido: Creación de una instancia administrada](instance-create-quickstart.md).

## <a name="powershell-create-and-configure-managed-instances"></a>PowerShell: Creación y configuración de instancias administradas

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRM son esencialmente idénticos.

Para crear y administrar instancias administradas con Azure PowerShell, use los siguientes cmdlets de PowerShell. Si necesita instalar o actualizar PowerShell, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).

> [!TIP]
> Para los scripts de ejemplo de PowerShell, consulte [Script de inicio rápido: Creación de una instancia administrada mediante una biblioteca de PowerShell](/archive/blogs/sqlserverstorageengine/quick-start-script-create-azure-sql-managed-instance-using-powershell).

| Cmdlet | Descripción |
| --- | --- |
|[New-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance)|Crea una instancia administrada. |
|[Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance)|Devuelve información sobre una instancia administrada.|
|[Set-AzSqlInstance](/powershell/module/az.sql/set-azsqlinstance)|Establece las propiedades de una instancia administrada.|
|[Remove-AzSqlInstance](/powershell/module/az.sql/remove-azsqlinstance)|Elimina una instancia administrada.|
|[Get-AzSqlInstanceOperation](/powershell/module/az.sql/get-azsqlinstanceoperation)|Obtiene una lista de las operaciones de administración realizadas en la instancia administrada o en una operación específica.|
|[Stop-AzSqlInstanceOperation](/powershell/module/az.sql/stop-azsqlinstanceoperation)|Cancela la operación de administración específica realizada en la instancia administrada.|
|[New-AzSqlInstanceDatabase](/powershell/module/az.sql/new-azsqlinstancedatabase)|Crea una base de datos de SQL Managed Instance.|
|[Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase)|Devuelve información sobre una base de datos de SQL Managed Instance.|
|[Remove-AzSqlInstanceDatabase](/powershell/module/az.sql/remove-azsqlinstancedatabase)|Elimina una base de datos de SQL Managed Instance.|
|[Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase)|Restaura una base de datos de SQL Managed Instance.|

## <a name="azure-cli-create-and-configure-managed-instances"></a>CLI de Azure: Creación y configuración de instancias administradas

Para crear y configurar instancias administradas con la [CLI de Azure](/cli/azure), use los siguientes [comandos de la CLI de Azure para SQL Managed Instance](/cli/azure/sql/mi). Use [Azure Cloud Shell](../../cloud-shell/overview.md) para ejecutar la CLI en el explorador o [instálela](/cli/azure/install-azure-cli) en Windows, Linux o macOS.

> [!TIP]
> Para consultar un artículo de inicio rápido acerca de la CLI de Azure, consulte [Working with SQL Managed Instance using Azure CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) (Uso de Instancia administrada de Azure SQL Database mediante la CLI de Azure).

| Cmdlet | Descripción |
| --- | --- |
|[az sql mi create](/cli/azure/sql/mi#az-sql-mi-create) |Crea una instancia administrada.|
|[az sql mi list](/cli/azure/sql/mi#az-sql-mi-list)|Enumera las instancias administradas disponibles.|
|[az sql mi show](/cli/azure/sql/mi#az-sql-mi-show)|Obtiene los detalles de una instancia administrada.|
|[az sql mi update](/cli/azure/sql/mi#az-sql-mi-update)|Actualiza una instancia administrada.|
|[az sql mi delete](/cli/azure/sql/mi#az-sql-mi-delete)|Elimina una instancia administrada.|
|[az sql mi op list](/cli/azure/sql/mi/op#az_sql_mi_op_list)|Obtiene una lista de las operaciones de administración realizadas en la instancia administrada.|
|[az sql mi op show](/cli/azure/sql/mi/op#az_sql_mi_op_show)|Obtiene la operación de administración específica realizada en la instancia administrada.|
|[az sql mi op cancel](/cli/azure/sql/mi/op#az_sql_mi_op_cancel)|Cancela la operación de administración específica realizada en la instancia administrada.|
|[az sql midb create](/cli/azure/sql/midb#az-sql-midb-create) |Crea una base de datos administrada.|
|[az sql midb list](/cli/azure/sql/midb#az-sql-midb-list)|Enumera las bases de datos administradas disponibles.|
|[az sql midb restore](/cli/azure/sql/midb#az-sql-midb-restore)|Restaura una base de datos administrada.|
|[az sql midb delete](/cli/azure/sql/midb#az-sql-midb-delete)|Elimina una base de datos administrada.|

## <a name="transact-sql-create-and-configure-instance-databases"></a>Transact-SQL: Creación y configuración de bases de datos de instancia

Para crear y configurar bases de datos de instancia después de crear la instancia administrada, use los comandos de T-SQL siguientes. Puede emitir estos comandos mediante Azure Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](/sql/azure-data-studio/what-is), [Visual Studio Code](https://code.visualstudio.com/docs) o cualquier otro programa que pueda conectarse a un servidor y pasar comandos de Transact-SQL.

> [!TIP]
> Para consultar artículos de inicio rápido que describan la configuración y conexión a una instancia administrada mediante SQL Server Management Studio en Microsoft Windows, consulte [Inicio rápido: Configuración de una máquina virtual de Azure para conectarse a Azure SQL Managed Instance](connect-vm-instance-configure.md) e [Inicio rápido: Configuración de una conexión de punto a sitio a Azure SQL Managed Instance desde el entorno local](point-to-site-p2s-configure.md).

> [!IMPORTANT]
> No se puede crear ni eliminar una instancia administrada mediante Transact-SQL.

| Get-Help | Descripción |
| --- | --- |
|[CREATE DATABASE](/sql/t-sql/statements/create-database-transact-sql?preserve-view=true&view=azuresqldb-mi-current)|Crea una nueva base de datos de instancia en SQL Managed Instance. Debe estar conectado a la base de datos maestra para crear una base de datos.|
| [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?preserve-view=true&view=azuresqldb-mi-current) |Modifica una base de datos de instancia en SQL Managed Instance.|

## <a name="rest-api-create-and-configure-managed-instances"></a>API REST: Creación y configuración de instancias administradas

Para crear y administrar instancias administradas, use estas solicitudes de la API REST.

| Get-Help | Descripción |
| --- | --- |
|[Managed Instances - Create Or Update](/rest/api/sql/managedinstances/createorupdate)|Crea o actualiza una instancia administrada.|
|[Managed Instances - Delete](/rest/api/sql/managedinstances/delete)|Elimina una instancia administrada.|
|[Managed Instances - Get](/rest/api/sql/managedinstances/get)|Obtiene una instancia administrada.|
|[Managed Instances - List](/rest/api/sql/managedinstances/list)|Devuelve una lista de las instancias administradas de una suscripción.|
|[Managed Instances - List By Resource Group](/rest/api/sql/managedinstances/listbyresourcegroup)|Devuelve una lista de las instancias administradas de un grupo de recursos.|
|[Managed Instances - Update](/rest/api/sql/managedinstances/update)|Actualiza una instancia administrada.|
|[Operaciones de instancia administrada: mostrar por instancia administrada](/rest/api/sql/managedinstanceoperations/listbymanagedinstance)|Obtiene una lista de las operaciones de administración realizadas en la instancia administrada.|
|[Operaciones de instancia administrada: obtener](/rest/api/sql/managedinstanceoperations/get)|Obtiene la operación de administración específica realizada en la instancia administrada.|
|[Operaciones de instancia administrada: cancelar](/rest/api/sql/managedinstanceoperations/cancel)|Cancela la operación de administración específica realizada en la instancia administrada.|

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo migrar una base de datos de SQL Server a Azure, vea [Migración a Azure SQL Database](../database/migrate-to-database-from-sql-server.md).
- Para obtener información sobre las características admitidas, consulte el [artículo que trata sobre dicho tema](../database/features-comparison.md).