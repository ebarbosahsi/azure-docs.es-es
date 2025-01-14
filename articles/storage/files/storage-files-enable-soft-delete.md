---
title: 'Habilitación de la eliminación temporal: recursos compartidos de archivos de Azure'
description: Obtenga información sobre cómo habilitar la eliminación temporal en recursos compartidos de archivos de Azure para la recuperación de datos y para evitar la eliminación accidental.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 12/01/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: 77381700f4257006b50e56ab7ffc037ef99d297c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102218560"
---
# <a name="enable-soft-delete-on-azure-file-shares"></a>Habilitación de la eliminación temporal en recursos compartidos de archivos de Azure

Azure Storage ofrece la posibilidad de eliminar temporalmente recursos compartidos de archivos, con el fin de que pueda recuperar más fácilmente los datos cuando una aplicación u otro usuario de la cuenta de almacenamiento los hayan eliminado por error. Para más información sobre la eliminación temporal, consulte [Cómo evitar la eliminación accidental de recursos compartidos de archivos de Azure](storage-files-prevent-file-share-deletion.md).

En las secciones siguientes se muestra cómo habilitar y usar la eliminación temporal para recursos compartidos de archivos de Azure en una cuenta de almacenamiento existente:

# <a name="portal"></a>[Portal](#tab/azure-portal)

## <a name="getting-started"></a>Introducción

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
1. Vaya a la cuenta de almacenamiento y seleccione **Eliminación temporal** en **Servicio de archivos**.
1. Seleccione **Habilitada** en **Eliminación temporal de recursos compartidos de archivos**.
1. Seleccione **File share retention period in days** (Período de retención del recurso compartido de archivos en días) y escriba el número que prefiera.
1. Seleccione **Guardar** para confirmar la configuración de retención de datos.

:::image type="content" source="media/storage-how-to-recover-deleted-account/enable-soft-delete-files.png" alt-text="Captura de pantalla del panel de configuración de la eliminación temporal de la cuenta de almacenamiento. Resalte la sección de recursos compartidos de archivos, habilite la alternancia, establezca un período de retención y guárdelo. De esta forma, habilitará la eliminación temporal para todos los recursos compartidos de archivos de la cuenta de almacenamiento.":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Los cmdlets de eliminación temporal están disponibles en la versión 2.1.3 y posteriores del [módulo de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="getting-started-with-cli"></a>Introducción a la CLI

Para habilitar la eliminación temporal, debe actualizar las propiedades de servicio de un cliente de archivo. En el ejemplo siguiente se habilita la eliminación temporal para todos los recursos compartidos de archivos en una cuenta de almacenamiento:

```azurecli
az storage account file-service-properties update --enable-delete-retention true -n yourStorageaccount -g yourResourceGroup
```

Puede comprobar si la eliminación temporal está habilitada y ver su directiva de retención con el siguiente comando:

```azurecli
az storage account file-service-properties show -n yourStorageaccount -g yourResourceGroup
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="prerequisite"></a>Requisito previo

Los cmdlets de eliminación temporal están disponibles en la versión 4.8.0 y posteriores del módulo Az.Storage. 

## <a name="getting-started-with-powershell"></a>Introducción a PowerShell

Para habilitar la eliminación temporal, debe actualizar las propiedades de servicio de un cliente de archivo. En el ejemplo siguiente se habilita la eliminación temporal para todos los recursos compartidos de archivos en una cuenta de almacenamiento:

```azurepowershell-interactive
$rgName = "yourResourceGroupName"
$accountName = "yourStorageAccountName"

Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $true -ShareRetentionDays 7
```

Puede comprobar si la eliminación temporal está habilitada y ver su directiva de retención con el siguiente comando:

```azurepowershell-interactive
Get-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName
```
---

## <a name="restore-soft-deleted-file-share"></a>Restauración del recurso compartido de archivos eliminado temporalmente

# <a name="portal"></a>[Portal](#tab/azure-portal)

Para restaurar un recurso compartido de archivos eliminado temporalmente:

1. Vaya a la cuenta de almacenamiento y seleccione **Recursos compartidos de archivos**.
1. En la hoja del recurso compartido de archivos, habilite **Show deleted shares** (Mostrar recursos compartidos eliminados) para mostrar los recursos compartidos que se han eliminado temporalmente.

    Se mostrarán los recursos compartidos que se encuentren en estado **Eliminado**.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/undelete-file-share.png" alt-text="Si la columna de estado, situada junto a la columna de nombre, se establece en Eliminado, el recurso compartido de archivos se encuentra en un estado de eliminación temporal. Se eliminará de forma permanente después del período de retención especificado.":::

1. Seleccione el recurso compartido y **recuperar**; de esta forma se restaurará el recurso compartido.

    Puede confirmar que el recurso compartido se haya restaurado desde que su estado cambia a **Activo**.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/restored-file-share.png" alt-text="Si la columna de estado, la columna situada junto a la columna de nombre, se establece en Activo, el recurso compartido de archivos se ha restaurado.":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Los cmdlets de eliminación temporal están disponibles en la versión 2.1.3 de la CLI de Azure. Para restaurar un recurso compartido de archivos eliminado temporalmente, primero debe obtener el valor `--deleted-version` del recurso compartido. Para obtener ese valor, use el siguiente comando para enumerar todos los recursos compartidos eliminados de la cuenta de almacenamiento:

```azurecli
az storage share-rm list --storage-account yourStorageaccount --include-deleted
```

Una vez que haya identificado el recurso compartido que desea restaurar, puede usarlo con el siguiente comando para restaurarlo:

```azurecli
az storage share-rm restore -n deletedshare --deleted-version 01D64EB9886F00C4 -g yourResourceGroup --storage-account yourStorageaccount
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Los cmdlets de eliminación temporal están disponibles en la versión 4.8.0 y posteriores del módulo Az.Storage. Para restaurar un recurso compartido de archivos eliminado temporalmente, primero debe obtener el valor `-DeletedShareVersion` del recurso compartido. Para obtener ese valor, use el siguiente comando para enumerar todos los recursos compartidos eliminados de la cuenta de almacenamiento:

```azurepowershell-interactive
Get-AzRmStorageShare -ResourceGroupName $rgname -StorageAccountName $accountName -IncludeDeleted
```

Una vez que haya identificado el recurso compartido que desea restaurar, puede usarlo con el siguiente comando para restaurarlo:

```azurepowershell-interactive
Restore-AzRmStorageShare -ResourceGroupName $rgname -StorageAccountName $accountName -DeletedShareVersion 01D5E2783BDCDA97
```
---

## <a name="disable-soft-delete"></a>Deshabilitación de la eliminación temporal

Si desea detener la eliminación temporal o eliminar permanentemente un recurso compartido de archivos, siga estas instrucciones:

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Vaya a la cuenta de almacenamiento y seleccione **Eliminación temporal** en **Configuración**.
1. En **Recursos compartidos de archivos** seleccione **Deshabilitado** en **Soft delete for file shares** (Eliminación temporal para recursos compartidos de archivos).
1. Seleccione **Guardar** para confirmar la configuración de retención de datos.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/disable-soft-delete-files.png" alt-text="Deshabilitar la eliminación temporal le permitirá eliminar de forma inmediata y permanente todos los recursos compartidos de archivos de la cuenta de almacenamiento cuando descanse.":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Los cmdlets de eliminación temporal están disponibles en la versión 2.1.3 de la CLI de Azure. Puede usar el siguiente comando para deshabilitar la eliminación temporal en la cuenta de almacenamiento:

```azurecli
az storage account file-service-properties update --enable-delete-retention false -n yourStorageaccount -g yourResourceGroup
```
# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Los cmdlets de eliminación temporal están disponibles en la versión 4.8.0 y posteriores del módulo Az.Storage. Puede usar el siguiente comando para deshabilitar la eliminación temporal en la cuenta de almacenamiento:

```azurepowershell-interactive
Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $false
```
---

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre otra forma de protección y recuperación de datos, consulte nuestro artículo [Introducción a las instantáneas de recursos compartidos de Azure Files](storage-snapshots-files.md).