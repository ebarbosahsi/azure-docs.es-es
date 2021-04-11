---
title: Aplicación de un bloqueo de Azure Resource Manager a una cuenta de almacenamiento
titleSuffix: Azure Storage
description: Obtenga información sobre cómo aplicar un bloqueo de Azure Resource Manager a una cuenta de almacenamiento.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/09/2021
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 79b88ad58a2eb95a48a140b3b98d606af495cb94
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104799792"
---
# <a name="apply-an-azure-resource-manager-lock-to-a-storage-account"></a>Aplicación de un bloqueo de Azure Resource Manager a una cuenta de almacenamiento

Microsoft recomienda aplicar un bloqueo de Azure Resource Manager a todas las cuentas de almacenamiento a fin de impedir su eliminación accidental o malintencionada. Hay dos tipos de bloqueos de los recursos de Azure Resource Manager:

- Un bloqueo **CannotDelete** impide que los usuarios eliminen una cuenta de almacenamiento, pero permite leer y modificar su configuración.
- Un bloqueo **ReadOnly** impide que los usuarios eliminen una cuenta de almacenamiento o modifiquen su configuración, pero permite leerla.

Para más información sobre los bloqueos de Azure Resource Manager, consulte [Bloqueo de recursos para impedir cambios](../../azure-resource-manager/management/lock-resources.md).

> [!CAUTION]
> El bloqueo de una cuenta de almacenamiento no protege los contenedores o blobs en esa cuenta para que no se eliminen o se sobrescriban. Para obtener más información sobre cómo proteger los datos de blobs, consulte la [Información general sobre la protección de datos](../blobs/data-protection-overview.md).

## <a name="configure-an-azure-resource-manager-lock"></a>Configuración de un bloqueo de Azure Resource Manager

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Siga estos pasos para configurar el bloqueo de una cuenta de almacenamiento con Azure Portal:

1. Vaya a la cuenta de almacenamiento en Azure Portal.
1. En la sección **Configuración**, seleccione **Bloqueos**.
1. Seleccione **Agregar**.
1. Proporcione un nombre para el bloqueo del recurso y especifique el tipo de bloqueo. Si lo desea, agregue una nota sobre el bloqueo.

    :::image type="content" source="media/lock-account-resource/lock-storage-account.png" alt-text="Captura de pantalla que muestra cómo bloquear una cuenta de almacenamiento con un bloqueo CannotDelete":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Si desea configurar un bloqueo en una cuenta de almacenamiento con PowerShell, primero asegúrese de que instaló el [módulo Az de PowerShell](https://www.powershellgallery.com/packages/Az). Luego, llame al comando [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) y especifique el tipo de bloqueo que quiere crear, tal como se muestra en el ejemplo siguiente:

```azurepowershell
New-AzResourceLock -LockLevel CanNotDelete `
    -LockName <lock> `
    -ResourceName <storage-account> `
    -ResourceType Microsoft.Storage/storageAccounts `
    -ResourceGroupName <resource-group>
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Si desea configurar un bloqueo en una cuenta de almacenamiento con al CLI de Azure, llame al comando [az lock create](/cli/azure/lock#az_lock_create) y especifique el tipo de bloqueo que quiere crear, tal como se muestra en el ejemplo siguiente:

```azurecli
az lock create \
    --name <lock> \
    --resource-group <resource-group> \
    --resource <storage-account> \
    --lock-type CanNotDelete \
    --resource-type Microsoft.Storage/storageAccounts
```

---

## <a name="authorizing-data-operations-when-a-readonly-lock-is-in-effect"></a>Autorización de operaciones de datos cuando se usa un bloqueo ReadOnly

Cuando se aplica un bloqueo **ReadOnly** a una cuenta de almacenamiento, la operación [Enumerar claves](/rest/api/storagerp/storageaccounts/listkeys) se bloquea para esa cuenta de almacenamiento. La operación **Enumerar claves** es una operación HTTPS POST y se impiden todas las operaciones POST cuando se configura un bloqueo **ReadOnly** para la cuenta. La operación **Enumerar claves** devuelve las claves de acceso de la cuenta, que luego se puede usar para leer y escribir en los datos de la cuenta de almacenamiento.

Si un cliente posee las claves de acceso de la cuenta en el momento en que se aplica el bloqueo a la cuenta de almacenamiento, ese cliente puede seguir usando las claves para acceder a los datos. Sin embargo, los clientes que no tienen acceso a las claves deberán usar credenciales de Azure  Active Directory (Azure AD) para acceder a los datos de blobs o colas de la cuenta de almacenamiento.

Los usuarios de Azure Portal pueden verse afectados cuando se aplica un bloqueo **ReadOnly** si han accedido anteriormente a los datos de blobs o colas en el portal con las claves de acceso de la cuenta. Una vez que se aplica el bloqueo, los usuarios del portal deberán usar credenciales de Azure AD para acceder a los datos de blobs o colas en el portal. Para ello, un usuario debe tener al menos dos roles RBAC asignados: el rol Lector de Azure Resource Manager, como mínimo, y uno de los roles de acceso a datos de Azure Storage. Para obtener más información, vea uno de los artículos siguientes:

- [Elección de la forma de autorizar el acceso a los datos de blob en Azure Portal](../blobs/authorize-data-operations-portal.md)
- [Elección de la forma de autorizar el acceso a los datos de cola en Azure Portal](../queues/authorize-data-operations-portal.md)

Es posible que los clientes que accedieron previamente a los datos de Azure Files o Table service con las claves de la cuenta ya no puedan acceder a ellos. Como procedimiento recomendado, si debe aplicar un bloqueo **ReadOnly** a una cuenta de almacenamiento, mueva las cargas de trabajo de Azure Files y Table service a una cuenta de almacenamiento que no tenga aplicado un bloqueo **ReadOnly**.

## <a name="next-steps"></a>Pasos siguientes

- [Información general sobre la protección de datos](../blobs/data-protection-overview.md)
- [Bloqueo de recursos para impedir cambios](../../azure-resource-manager/management/lock-resources.md)
