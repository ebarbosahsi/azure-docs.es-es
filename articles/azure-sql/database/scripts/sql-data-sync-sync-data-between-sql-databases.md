---
title: 'PowerShell: Sincronización entre varias bases de datos en Azure SQL Database'
description: Use un script de ejemplo de Azure PowerShell para sincronizar entre varias bases de datos de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: 3e6c7dd0a75a05f15fe6d59bbf5fa47b2940d86a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96919924"
---
# <a name="use-powershell-to-sync-data-between-multiple-databases-in-azure-sql-database"></a>Uso de PowerShell para sincronizar datos entre varias bases de datos en Azure SQL Database

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

En este ejemplo de Azure PowerShell, se configura SQL Data Sync para sincronizar datos entre varias bases de datos en Azure SQL Database.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar PowerShell de manera local, en este tutorial se requiere la versión 1.4.0 de Azure PowerShell o una posterior. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

Para obtener información general acerca de SQL Data Sync, consulte [Sincronización de datos entre varias bases de datos locales y de la nube con Azure SQL Data Sync](../sql-data-sync-data-sql-server-sql-database.md).

> [!IMPORTANT]
> SQL Data Sync no admite en este momento Instancia administrada de Azure SQL.

## <a name="prerequisites"></a>Requisitos previos

- Crear una base de datos de Azure SQL Database a partir de la base de datos de ejemplo AdventureWorksLT como base de datos central.
- Crear una base de datos de Azure SQL Database en la misma región que la base de datos de sincronización.
- Actualizar los marcadores de posición de parámetros antes de ejecutar el ejemplo.

## <a name="example"></a>Ejemplo

```powershell-interactive
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# hub database info
$subscriptionId = "<subscriptionId>"
$resourceGroupName = "<resourceGroupName>"
$serverName = "<serverName>"
$databaseName = "<databaseName>"

# sync database info
$syncDatabaseResourceGroupName = "<syncResourceGroupName>"
$syncDatabaseServerName = "<syncServerName>"
$syncDatabaseName = "<syncDatabaseName>"

# sync group info
$syncGroupName = "<syncGroupName>"
$conflictResolutionPolicy = "HubWin" # can be HubWin or MemberWin
$intervalInSeconds = 300 # sync interval in seconds (must be no less than 300)

# member database info
$syncMemberName = "<syncMemberName>"
$memberServerName = "<memberServerName>"
$memberDatabaseName = "<memberDatabaseName>"
$memberDatabaseType = "SqlServerDatabase" # can be AzureSqlDatabase or SqlServerDatabase
$syncDirection = "Bidirectional" # can be Bidirectional, Onewaymembertohub, Onewayhubtomember

# sync agent info
$syncAgentName = "<agentName>"
$syncAgentResourceGroupName = "<syncAgentResourceGroupName>"
$syncAgentServerName = "<syncAgentServerName>"

$syncMemberResourceId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/databases/$syncMemberDBName"

# temp file to save the sync schema
$tempFile = $env:TEMP+"\syncSchema.json"

# list of included columns and tables in quoted name
$includedColumnsAndTables =  "[SalesLT].[Address].[AddressID]",
                             "[SalesLT].[Address].[AddressLine2]",
                             "[SalesLT].[Address].[rowguid]",
                             "[SalesLT].[Address].[PostalCode]",
                             "[SalesLT].[ProductDescription]"
$metadataList = [System.Collections.ArrayList]::new($includedColumnsAndTables)

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

# use if it's safe to show password in script, otherwise use PromptForCredential
# $user = "username"
# $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
# $credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $user, $password

$credential = $Host.ui.PromptForCredential("Need credential",
              "Please enter your user name and password for server "+$serverName+".database.windows.net",
              "",
              "")

# create a new sync group (if you use private link, make sure to manually approve it)
Write-Host "Creating Sync Group "$syncGroupName"..."
New-AzSqlSyncGroup -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Name $syncGroupName `
    -SyncDatabaseName $syncDatabaseName -SyncDatabaseServerName $syncDatabaseServerName -SyncDatabaseResourceGroupName $syncDatabaseResourceGroupName `
    -ConflictResolutionPolicy $conflictResolutionPolicy -DatabaseCredential $credential -UsePrivateLinkConnection | Format-list

# use if it's safe to show password in script, otherwise use PromptForCredential
# $user = "username"
# $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
# $credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $user, $password

$credential = $Host.ui.PromptForCredential("Need credential",
              "Please enter your user name and password for server "+$serverName+".database.windows.net",
              "",
              "")

# add a new sync member (if you use private link, make sure to manually approve it)
Write-Host "Adding member"$syncMemberName" to the sync group..."
New-AzSqlSyncMember -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
    -SyncGroupName $syncGroupName -Name $syncMemberName -MemberDatabaseType $memberDatabaseType -SyncAgentResourceGroupName $syncAgentResourceGroupName `
    -SyncAgentServerName $syncAgentServerName -SyncAgentName $syncAgentName  -SyncDirection $syncDirection -SqlServerDatabaseID  $syncAgentInfo.DatabaseId `
    -SyncMemberAzureDatabaseResourceId $syncMemberResourceId -UsePrivateLinkConnection | Format-list

# update existing sync member to use private link connection 
Update-AzSqlSyncMember `
    -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -SyncGroupName $syncGroupName -Name $syncMemberName `
    -MemberDatabaseCredential $memberDatabaseCredential -SyncMemberAzureDatabaseResourceId $syncMemberResourceId -UsePrivateLinkConnection $true
    
# update existing sync group and remove private link connection
Update-AzSqlSyncGroup `
    -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Name $syncGroupName -UsePrivateLinkConnection $false

# run the following Get-AzSqlSyncGroup/ Get-AzSqlSyncMember commands to confirm that a private link has been setup for Data Sync, if you decide to use private link. 
# Get-AzSqlSyncMember returns information about one or more Azure SQL Database Sync Members. Specify the name of a sync member to see information for only that sync member.
Get-AzSqlSyncMember `
    -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -SyncGroupName $syncGroupName -Name $syncMemberName ` | Format-List
# Get-AzSqlSyncGroup returns information about one or more Azure SQL Database Sync Groups. Specify the name of a sync group to see information for only that sync group.
Get-AzSqlSyncGroup `
    -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName ` | Format-List
    
# approve private endpoint connection, if you decide to use private link
Approve-AzPrivateEndpointConnection `
    -Name myPrivateEndpointConnection -ResourceGroupName myResourceGroup -ServiceName myPrivateLinkService

# refresh database schema from hub database, specify the -SyncMemberName parameter if you want to refresh schema from the member database
Write-Host "Refreshing database schema from hub database..."
$startTime = Get-Date
Update-AzSqlSyncSchema -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -SyncGroupName $syncGroupName

# waiting for successful refresh
$startTime = $startTime.ToUniversalTime()
$timer=0
$timeout=90

# check the log and see if refresh has gone through
Write-Host "Check for successful refresh..."
$isSucceeded = $false
while ($isSucceeded -eq $false) {
    Start-Sleep -s 10
    $timer=$timer+10
    $details = Get-AzSqlSyncSchema -SyncGroupName $syncGroupName -ServerName $serverName -DatabaseName $databaseName -ResourceGroupName $resourceGroupName
    if ($details.LastUpdateTime -gt $startTime) {
        Write-Host "Refresh was successful"
        $isSucceeded = $true
    }
    if ($timer -eq $timeout) {
        Write-Host "Refresh timed out"
        break;
    }
}

# get the database schema
Write-Host "Adding tables and columns to the sync schema..."
$databaseSchema = Get-AzSqlSyncSchema -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
    -DatabaseName $DatabaseName -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "C:\Users\OnPremiseServer\AppData\Local\Temp\syncSchema.json"

$newSchema = [AzureSqlSyncGroupSchemaModel]::new()
$newSchema.Tables = [List[AzureSqlSyncGroupSchemaTableModel]]::new();

# add columns and tables to the sync schema
foreach ($tableSchema in $databaseSchema.Tables) {
    $newTableSchema = [AzureSqlSyncGroupSchemaTableModel]::new()
    $newTableSchema.QuotedName = $tableSchema.QuotedName
    $newTableSchema.Columns = [List[AzureSqlSyncGroupSchemaColumnModel]]::new();
    $addAllColumns = $false
    if ($MetadataList.Contains($tableSchema.QuotedName)) {
        if ($tableSchema.HasError) {
            $fullTableName = $tableSchema.QuotedName
            Write-Host "Can't add table $fullTableName to the sync schema" -foregroundcolor "Red"
            Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            continue;
        }
        else {
            $addAllColumns = $true
        }
    }
    foreach($columnSchema in $tableSchema.Columns) {
        $fullColumnName = $tableSchema.QuotedName + "." + $columnSchema.QuotedName
        if ($addAllColumns -or $MetadataList.Contains($fullColumnName)) {
            if ((-not $addAllColumns) -and $tableSchema.HasError) {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            }
            elseif ((-not $addAllColumns) -and $columnSchema.HasError) {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $columnSchema.ErrorId -foregroundcolor "Red"
            }
            else {
                Write-Host "Adding"$fullColumnName" to the sync schema"
                $newColumnSchema = [AzureSqlSyncGroupSchemaColumnModel]::new()
                $newColumnSchema.QuotedName = $columnSchema.QuotedName
                $newColumnSchema.DataSize = $columnSchema.DataSize
                $newColumnSchema.DataType = $columnSchema.DataType
                $newTableSchema.Columns.Add($newColumnSchema)
            }
        }
    }
    if ($newTableSchema.Columns.Count -gt 0) {
        $newSchema.Tables.Add($newTableSchema)
    }
}

# convert sync schema to JSON format
$schemaString = $newSchema | ConvertTo-Json -depth 5 -Compress

# work around a PowerShell bug
$schemaString = $schemaString.Replace('"Tables"', '"tables"').Replace('"Columns"', '"columns"').Replace('"QuotedName"', '"quotedName"').Replace('"MasterSyncMemberName"','"masterSyncMemberName"')

# save the sync schema to a temp file
$schemaString | Out-File $tempFile

# update sync schema
Write-Host "Updating the sync schema..."
Update-AzSqlSyncGroup -ResourceGroupName $resourceGroupName -ServerName $serverName `
    -DatabaseName $databaseName -Name $syncGroupName -Schema $tempFile

$syncLogStartTime = Get-Date

# trigger sync manually
Write-Host "Trigger sync manually..."
Start-AzSqlSyncGroupSync -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -SyncGroupName $syncGroupName

# check the sync log and wait until the first sync succeeded
Write-Host "Check the sync log..."
$isSucceeded = $false
for ($i = 0; ($i -lt 300) -and (-not $isSucceeded); $i = $i + 10) {
    Start-Sleep -s 10
    $syncLogEndTime = Get-Date
    $syncLogList = Get-AzSqlSyncGroupLog -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
        -SyncGroupName $syncGroupName -StartTime $syncLogStartTime.ToUniversalTime() -EndTime $syncLogEndTime.ToUniversalTime()

    if ($synclogList.Length -gt 0) {
        foreach ($syncLog in $syncLogList) {
            if ($syncLog.Details.Contains("Sync completed successfully")) {
                Write-Host $syncLog.TimeStamp : $syncLog.Details
                $isSucceeded = $true
            }
        }
    }
}

if ($isSucceeded) {
    # enable scheduled sync
    Write-Host "Enable the scheduled sync with 300 seconds interval..."
    Update-AzSqlSyncGroup  -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
        -Name $syncGroupName -IntervalInSeconds $intervalInSeconds
}
else {
    # output all log if sync doesn't succeed in 300 seconds
    $syncLogEndTime = Get-Date
    $syncLogList = Get-AzSqlSyncGroupLog  -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
        -SyncGroupName $syncGroupName -StartTime $syncLogStartTime.ToUniversalTime() -EndTime $syncLogEndTime.ToUniversalTime()

    if ($synclogList.Length -gt 0) {
        foreach ($syncLog in $syncLogList) {
            Write-Host $syncLog.TimeStamp : $syncLog.Details
        }
    }
}
```

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, puede ejecutar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados a él.

```powershell
Remove-AzResourceGroup -ResourceGroupName $ResourceGroupName
Remove-AzResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzSqlSyncAgent](/powershell/module/az.sql/New-azSqlSyncAgent) |  Crea un agente de sincronización. |
| [New-AzSqlSyncAgentKey](/powershell/module/az.sql/New-azSqlSyncAgentKey) |  Genera la clave del agente asociada al agente de sincronización. |
| [Get-AzSqlSyncAgentLinkedDatabase](/powershell/module/az.sql/Get-azSqlSyncAgentLinkedDatabase) |  Permite obtener toda la información del agente de sincronización. |
| [New-AzSqlSyncMember](/powershell/module/az.sql/New-azSqlSyncMember) |  Agrega un nuevo miembro al grupo de sincronización. |
| [Update-AzSqlSyncSchema](/powershell/module/az.sql/Update-azSqlSyncSchema) |  Actualiza la información de esquema de la base de datos. |
| [Get-AzSqlSyncSchema](/powershell/module/az.sql/Get-azSqlSyncSchema) |  Obtiene la información de esquema de la base de datos. |
| [Update-AzSqlSyncGroup](/powershell/module/az.sql/Update-azSqlSyncGroup) |  Actualiza el grupo de sincronización. |
| [Start-AzSqlSyncGroupSync](/powershell/module/az.sql/Start-azSqlSyncGroupSync) | Desencadena una sincronización. |
| [Get-AzSqlSyncGroupLog](/powershell/module/az.sql/Get-azSqlSyncGroupLog) |  Comprueba el registro de sincronización. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../powershell-script-content-guide.md).

Para más información acerca de SQL Data Sync, consulte:

- Introducción: [Sincronización de datos entre varias bases de datos locales y en la nube con Azure SQL Data Sync](../sql-data-sync-data-sql-server-sql-database.md)
- Configuración de Data Sync
    - Uso de Azure Portal: [Tutorial: Configuración de SQL Data Sync para sincronizar datos entre Azure SQL Database y SQL Server](../sql-data-sync-sql-server-configure.md)
    - Uso de PowerShell: [Uso de PowerShell para sincronizar datos entre una base de datos de Azure SQL Database y SQL Server](sql-data-sync-sync-data-between-azure-onprem.md)
- Data Sync Agent: [Data Sync Agent para Azure SQL Data Sync](../sql-data-sync-agent-overview.md)
- Procedimientos recomendados: [Procedimientos recomendados para Azure SQL Data Sync](../sql-data-sync-best-practices.md)
- Supervisión: [Monitor SQL Data Sync with Azure Monitor logs](../monitor-tune-overview.md) (Supervisión de SQL Data Sync con registros de Azure Monitor)
- Solución de problemas: [Solución de problemas con Azure SQL Data Sync](../sql-data-sync-troubleshoot.md)
- Actualización del esquema de sincronización
    - Uso de Transact-SQL: [Automatización de la replicación de los cambios de esquema en Azure SQL Data Sync](../sql-data-sync-update-sync-schema.md)
    - Uso de PowerShell: [Uso de PowerShell para actualizar el esquema de sincronización en un grupo de sincronización existente](update-sync-schema-in-sync-group.md)

Para obtener más información sobre SQL Database, vea:

- [Información general de SQL Database](../sql-database-paas-overview.md)
- [Administración del ciclo de vida de las aplicaciones](/previous-versions/sql/sql-server-guides/jj907294(v=sql.110))
