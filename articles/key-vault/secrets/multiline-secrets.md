---
title: Almacenamiento de un secreto multilínea en Azure Key Vault
description: Tutorial en el que se muestra cómo establecer secretos multilínea desde Azure Key Vault mediante la CLI de Azure y PowerShell
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurecli
ms.date: 03/17/2021
ms.author: mbaldwin
ms.openlocfilehash: e320795d5e8623dea9b97ac6371a0ffc6e6117f1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608675"
---
# <a name="store-a-multi-line-secret-in-azure-key-vault"></a>Almacenamiento de un secreto multilínea en Azure Key Vault

Tanto en el inicio rápido de la [CLI de Azure](quick-create-cli.md) como en el de [Azure PowerShell](quick-create-powershell.md) se muestra cómo almacenar un secreto de una sola línea.   También puede usar Key Vault para almacenar un secreto multilínea, como un archivo JSON o una clave privada RSA.

Los secretos multilínea no se pueden pasar al comando [az keyvault secret set](/cli/azure/keyvault/secret#az_keyvault_secret_set) de la CLI de Azure o al cmdlet [Set-AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret) de Azure PowerShell a través de la línea de comandos. En su lugar, debe almacenar primero el secreto multilínea como archivo de texto. 

Por ejemplo, puede crear un archivo de texto denominado "secretfile.txt" que contenga las siguientes líneas:

```bash
This is my
multi-line
secret
```

Después, puede pasar este archivo al comando [az keyvault secret set](/cli/azure/keyvault/secret#az_keyvault_secret_set) de la CLI de Azure mediante el `--file` parámetro.

```azurecli-interactive
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "MultilineSecret" --file "secretfile.txt"
```

Con Azure PowerShell, primero debe leer el archivo mediante el cmdlet [Get-Content](/powershell/module/microsoft.powershell.management/get-content) y luego convertirlo en una cadena segura mediante [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring). 

```azurepowershell-interactive
$RawSecret =  Get-Content "secretfile.txt" -Raw
$SecureSecret = ConvertTo-SecureString -String $RawSecret -AsPlainText -Force
```

Por último, se almacena el secreto mediante el cmdlet [set-AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret).

```azurepowershell-interactive
$secret = Set-AzKeyVaultSecret -VaultName "<your-unique-keyvault-name>" -Name "MultilineSecret" -SecretValue $SecureSecret
```

En todos los casos, luego puede ver el secreto almacenado mediante el comando [az keyvault secret show](/cli/azure/keyvault/secret#az_keyvault_secret_show) de la CLI de Azure o el cmdlet [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret) de Azure PowerShell.

```azurecli-interactive
az keyvault secret show --name "MultilineSecret" --vault-name "<your-unique-keyvault-name>" --query "value"
```

El secreto se devolverá con nuevas líneas insertadas:

```bash
"This is\nmy multi-line\nsecret"
```

## <a name="next-steps"></a>Pasos siguientes

- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Consulte el [inicio rápido de la CLI de Azure](quick-create-cli.md).
- Consulte los [comandos az keyvault de la CLI de Azure](/cli/azure/keyvault).
- Consulte el [inicio rápido de Azure PowerShell](quick-create-powershell.md)
- Consulte los [cmdlets Az.KeyVault de Azure PowerShell](/powershell/module/az.keyvault#key-vault)
