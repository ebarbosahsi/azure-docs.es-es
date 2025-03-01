---
title: Habilitación de la extensión de VM mediante la CLI de Azure
description: En este artículo se describe cómo implementar extensiones de máquina virtual en servidores habilitados para Azure Arc que se ejecutan en entornos de nube híbrida mediante la CLI de Azure.
ms.date: 01/05/2021
ms.topic: conceptual
ms.custom: devx-track-azurecli
ms.openlocfilehash: 6edb7d55e542f963c75693d535fa3b50dc5b827b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97916208"
---
# <a name="enable-azure-vm-extensions-using-the-azure-cli"></a>Habilitación de las extensiones de VM de Azure mediante la CLI de Azure

En este artículo se muestra cómo implementar y desinstalar extensiones de VM, compatibles con servidores habilitados para Azure Arc, en una máquina híbrida Linux o Windows mediante la CLI de Azure.

[!INCLUDE [Azure CLI Prepare your environment](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="install-the-azure-cli-extension"></a>Instalación de la extensión de la CLI de Azure

Los comandos de ConnectedMachine no se incluyen como parte de la CLI de Azure. Antes de usar la CLI de Azure para administrar las extensiones de VM en el servidor híbrido administrado por los servidores habilitados para Arc, debe cargar la extensión ConnectedMachine. Ejecute el siguiente comando para obtenerla:

```azurecli
az extension add --name connectedmachine
```

## <a name="enable-extension"></a>Habilitación de una extensión

Para habilitar una extensión de VM en el servidor habilitado para Arc, use [az connectedmachine extension create](/cli/azure/ext/connectedmachine/connectedmachine/extension#ext_connectedmachine_az_connectedmachine_extension_create) con los parámetros `--machine-name`, `--extension-name`, `--location`, `--type`, `settings` y `--publisher`.

En el ejemplo siguiente se habilita la extensión de máquina virtual de Log Analytics en un servidor habilitado para Arc:

```azurecli
az connectedmachine extension create --machine-name "myMachineName" --name "OmsAgentForLinux or MicrosoftMonitoringAgent" --location "eastus" --settings '{\"workspaceId\":\"myWorkspaceId\"}' --protected-settings '{\"workspaceKey\":\"myWorkspaceKey\"}' --resource-group "myResourceGroup" --type-handler-version "1.13" --type "OmsAgentForLinux or MicrosoftMonitoringAgent" --publisher "Microsoft.EnterpriseCloud.Monitoring" 
```

En el ejemplo siguiente se habilita la extensión de script personalizado en un servidor habilitado para Arc:

```azurecli
az connectedmachine extension create --machine-name "myMachineName" --name "CustomScriptExtension" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.Compute" --settings "{\"commandToExecute\":\"powershell.exe -c \\\"Get-Process | Where-Object { $_.CPU -gt 10000 }\\\"\"}" --type-handler-version "1.10" --resource-group "myResourceGroup"
```

En el ejemplo siguiente se habilita la extensión de máquina virtual de Key Vault (versión preliminar) en un servidor habilitado para Arc:

```azurecli
az connectedmachine extension create --resource-group "resourceGroupName" --machine-name "myMachineName" --location "regionName" --publisher "Microsoft.Azure.KeyVault" --type "KeyVaultForLinux or KeyVaultForWindows" --name "KeyVaultForLinux or KeyVaultForWindows" --settings '{"secretsManagementSettings": { "pollingIntervalInS": "60", "observedCertificates": ["observedCert1"] }, "authenticationSettings": { "msiEndpoint": "http://localhost:40342/metadata/identity" }}'
```

## <a name="list-extensions-installed"></a>Enumeración de extensiones instaladas

Para obtener una lista de las extensiones de VM en el servidor habilitado para Arc, use [az connectedmachine extension list](/cli/azure/ext/connectedmachine/connectedmachine/extension#ext_connectedmachine_az_connectedmachine_extension_list) con los parámetros `--machine-name` y `--resource-group`.

Ejemplo:

```azurecli
az connectedmachine extension list --machine-name "myMachineName" --resource-group "myResourceGroup"
```

De forma predeterminada, la salida de los comandos de la CLI de Azure está en JSON (notación de objetos JavaScript). Para cambiar la salida predeterminada a una lista o tabla, por ejemplo, use [az configure --output](/cli/azure/reference-index). También puede agregar `--output` a cualquier comando para un cambio específico del formato de salida.

En el ejemplo siguiente se muestra la salida JSON parcial desde el comando `az connectedmachine extension -list`:

```json
[
  {
    "autoUpgradingMinorVersion": "false",
    "forceUpdateTag": null,
    "id": "/subscriptions/subscriptionId/resourceGroups/resourceGroupName/providers/Microsoft.HybridCompute/machines/SVR01/extensions/DependencyAgentWindows",
    "location": "eastus",
    "name": "DependencyAgentWindows",
    "namePropertiesInstanceViewName": "DependencyAgentWindows",
```

## <a name="remove-an-installed-extension"></a>Eliminación de una extensión instalada

Para quitar una extensión de VM instalada en el servidor habilitado para Arc, use [az connectedmachine extension delete](/cli/azure/ext/connectedmachine/connectedmachine/extension#ext_connectedmachine_az_connectedmachine_extension_delete) con los parámetros `--extension-name`, `--machine-name` y `--resource-group`.

Por ejemplo, para quitar la extensión de VM de Log Analytics para Linux, ejecute el siguiente comando:

```azurecli
az connectedmachine extension delete --machine-name "myMachineName" --name "OmsAgentForLinux" --resource-group "myResourceGroup"
```

## <a name="next-steps"></a>Pasos siguientes

- Puede implementar, administrar y quitar extensiones de VM con [Azure PowerShell](manage-vm-extensions-powershell.md), desde [Azure Portal](manage-vm-extensions-portal.md) o con [plantillas de Azure Resource Manager](manage-vm-extensions-template.md).

- Puede encontrar información de solución de problemas en la [guía de solución de problemas de las extensiones de máquina virtual](troubleshoot-vm-extensions.md).

- Consulte el artículo de [introducción](/cli/azure/ext/connectedmachine/connectedmachine/extension) a las extensiones de VM de la CLI de Azure para obtener más información sobre los comandos.
