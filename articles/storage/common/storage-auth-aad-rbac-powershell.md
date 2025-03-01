---
title: Uso de PowerShell para asignar un rol de Azure para el acceso a datos
titleSuffix: Azure Storage
description: Aprenda a usar el módulo de Azure PowerShell para asignar permisos a una entidad de seguridad de Azure Active Directory con el control de acceso basado en rol (Azure RBAC). Azure Storage admite roles integrados y personalizados de Azure para la autenticación mediante Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/10/2021
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 9e4fea52f56da9f2e84746daf0121df0b4355411
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100375959"
---
# <a name="use-powershell-to-assign-an-azure-role-for-access-to-blob-and-queue-data"></a>Uso de PowerShell para asignar un rol de Azure para el acceso a datos de blobs y colas

Azure Active Directory (Azure AD) autoriza derechos de acceso a recursos protegidos mediante el [control de acceso basado en rol de Azure (RBAC de Azure)](../../role-based-access-control/overview.md). Azure Storage define un conjunto de roles integrados de Azure que abarcan conjuntos comunes de permisos utilizados para acceder a los contenedores o las colas.

Cuando un rol de Azure se asigna a una entidad de seguridad de Azure AD, Azure concede a esa entidad de seguridad acceso a esos recursos. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación o una [identidad de servicio administrada para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

En este artículo se describe cómo usar Azure PowerShell para mostrar los roles integrados de Azure y asignarlos a usuarios. Para obtener más información sobre cómo usar Azure PowerShell, vea [Introducción a Azure PowerShell](/powershell/azure/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="azure-roles-for-blobs-and-queues"></a>Roles de Azure para blobs y colas

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Determinar el ámbito de recursos

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-azure-roles"></a>Lista de roles de Azure disponibles

Para obtener una lista de los roles integrados de Azure disponibles con Azure PowerShell, use el comando [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition):

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Aparecerá una lista con los roles de datos de Azure Storage integrados, así como otros roles integrados de Azure:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-azure-role-to-a-security-principal"></a>Asignación de un rol de Azure a una entidad de seguridad

Para asignar un rol de Azure a una entidad de seguridad, use el comando [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment). El formato del comando puede variar en función del ámbito de la asignación. Para ejecutar el comando, debe tener asignado el rol de propietario o colaborador en el ámbito correspondiente. Los ejemplos siguientes muestran cómo asignar un rol a un usuario en varios ámbitos, pero puede usar el mismo comando para asignar un rol a cualquier entidad de seguridad.

> [!IMPORTANT]
> Al crear una cuenta de Azure Storage, no se le asignan automáticamente permisos para tener acceso a datos a través de Azure AD. Tiene que asignarse a sí mismo de forma explícita un rol RBAC de Azure para el acceso a datos. Puede asignarlo al nivel de su suscripción, grupo de recursos, cuenta de almacenamiento o un contenedor o cola.
>
> Si la cuenta de almacenamiento se bloquea con un bloqueo de solo lectura de Azure Resource Manager, el bloqueo evita la asignación de roles de RBAC de Azure que tienen como ámbito la cuenta de almacenamiento o un contenedor de datos (contenedor de blobs o cola).

### <a name="container-scope"></a>Ámbito de contenedor

Para asignar un rol cuyo ámbito es un contenedor, especifique una cadena que contenga el ámbito del contenedor en el parámetro `--scope`. El ámbito de un contenedor tiene este formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

En el siguiente ejemplo se asigna el rol **Colaborador de datos de Storage Blob** a un usuario y el ámbito se establece un contenedor denominado *sample-container*. Asegúrese de reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes angulares por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Ámbito de cola

Para asignar un rol cuyo ámbito es una cola, especifique una cadena que contenga el ámbito de la cola en el parámetro `--scope`. El ámbito de una cola tiene este formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

En el siguiente ejemplo se asigna el rol **Colaborador de datos de la cola de Storage Blob** a un usuario y el ámbito se establece una cola denominada *sample-queue*. Asegúrese de reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes angulares por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Ámbito de cuenta de almacenamiento

Para asignar un rol cuyo ámbito es la cuenta de almacenamiento, especifique el ámbito del recurso de la cuenta de almacenamiento en el parámetro `--scope`. El ámbito de una cuenta de almacenamiento tiene este formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

En el siguiente ejemplo se muestra cómo establecer el ámbito del rol **Lector de datos de Storage Blob** en un usuario en el nivel de la cuenta de almacenamiento. Asegúrese de reemplazar los valores de ejemplo por sus propios valores: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Ámbito de grupo de recursos

Para asignar un rol cuyo ámbito es el grupo de recursos, especifique el identificador o el nombre del grupo de recursos en el parámetro `--resource-group`. En el siguiente ejemplo se asigna el rol **Lector de datos de la cola de Storage Blob** a un usuario en el nivel del grupo de recursos. Asegúrese de reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes angulares por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Ámbito de suscripción

Para asignar un rol cuyo ámbito es la suscripción, especifique el ámbito de la suscripción en el parámetro `--scope`. El ámbito de una suscripción tiene este formato:

```
/subscriptions/<subscription>
```

En el siguiente ejemplo se muestra cómo asignar el rol **Lector de datos de Storage Blob** a un usuario en el nivel de la cuenta de almacenamiento. Asegúrese de reemplazar los valores de ejemplo por sus propios valores: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación o eliminación de asignaciones de roles de Azure con el módulo de Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Uso de la CLI de Azure para asignar un rol de Azure para obtener acceso a datos de blobs y colas](storage-auth-aad-rbac-cli.md)
- [Uso de Azure Portal para asignar un rol de Azure para el acceso a datos de blobs y colas](storage-auth-aad-rbac-portal.md)
