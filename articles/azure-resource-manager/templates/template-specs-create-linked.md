---
title: Creación de una especificación de plantilla con plantillas vinculadas
description: Aprenda a crear una especificación de plantilla con plantillas vinculadas.
ms.topic: conceptual
ms.date: 01/05/2021
ms.openlocfilehash: 31764b9adb1d0fc28a170fa2e6b5073812533083
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104889848"
---
# <a name="tutorial-create-a-template-spec-with-linked-templates-preview"></a>Tutorial: Creación de una especificación de plantilla con plantillas vinculadas (versión preliminar)

Aprenda a crear una [especificación de plantilla](template-specs.md) con una plantilla principal y una [plantilla vinculada](linked-templates.md#linked-template). Las especificaciones de plantilla se usan para compartir plantillas de ARM con otros usuarios de la organización. En este artículo se muestra cómo crear una especificación de plantilla para empaquetar una plantilla principal y sus plantillas vinculadas con la propiedad `relativePath` del [recurso de implementación](/azure/templates/microsoft.resources/deployments).

## <a name="prerequisites"></a>Requisitos previos

Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

> [!NOTE]
> Las especificaciones de plantilla se encuentran actualmente en versión preliminar. Para usarlas con Azure PowerShell, debe instalar la [versión 5.0.0 o posterior](/powershell/azure/install-az-ps). Para usarlas con la CLI de Azure, utilice la [versión 2.14.2 o posterior](/cli/azure/install-azure-cli).

## <a name="create-linked-templates"></a>Creación de plantillas vinculadas

Cree la plantilla principal y la plantilla vinculada.

Para vincular una plantilla, agregue un [recurso de implementaciones](/azure/templates/microsoft.resources/deployments) a la plantilla principal. En la propiedad `templateLink`, especifique la ruta de acceso relativa de la plantilla vinculada de acuerdo con la ruta de acceso de la plantilla principal.

La plantilla vinculada se denomina **linkedTemplate.json** y se almacena en una subcarpeta denominada **artefactos** en la ruta de acceso donde se almacena la plantilla principal.  Puede usar uno de los valores siguientes para la ruta de acceso relativa:

- `./artifacts/linkedTemplate.json`
- `/artifacts/linkedTemplate.json`
- `artifacts/linkedTemplate.json`

La propiedad `relativePath` siempre es relativa al archivo de plantilla en el que se declara `relativePath`, por lo que si hay otro archivo linkedTemplate2.json que se llame desde linkedTemplate.json y linkedTemplate2.json se almacena en la misma subcarpeta de artefactos, la propiedad relativePath especificada en linkedTemplate.json es solo `linkedTemplate2.json`.

1. Cree la plantilla principal con el JSON siguiente. Guarde la plantilla principal como **azuredeploy.json** en el equipo local. En este tutorial se da por supuesto que se ha guardado en la ruta de acceso **c:\Templates\linkedTS\azuredeploy.json**, pero puede usar cualquier ruta de acceso.

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "westus2",
          "metadata":{
            "description": "Specify the location for the resources."
          }
        },
        "storageAccountType": {
          "type": "string",
          "defaultValue": "Standard_LRS",
          "metadata":{
            "description": "Specify the storage account type."
          }
        }
      },
      "variables": {
        "appServicePlanName": "[concat('plan', uniquestring(resourceGroup().id))]"
      },
      "resources": [
        {
          "type": "Microsoft.Web/serverfarms",
          "apiVersion": "2016-09-01",
          "name": "[variables('appServicePlanName')]",
          "location": "[parameters('location')]",
          "sku": {
            "name": "B1",
            "tier": "Basic",
            "size": "B1",
            "family": "B",
            "capacity": 1
          },
          "kind": "linux",
          "properties": {
            "perSiteScaling": false,
            "reserved": true,
            "targetWorkerCount": 0,
            "targetWorkerSizeId": 0
          }
        },
        {
          "type": "Microsoft.Resources/deployments",
          "apiVersion": "2020-10-01",
          "name": "createStorage",
          "properties": {
            "mode": "Incremental",
            "templateLink": {
              "relativePath": "artifacts/linkedTemplate.json"
            },
            "parameters": {
              "storageAccountType": {
                "value": "[parameters('storageAccountType')]"
              }
            }
          }
        }
      ]
    }
    ```

    > [!NOTE]
    > El valor de apiVersion de `Microsoft.Resources/deployments` debe ser 2020-06-01 o posterior.

1. Cree un directorio llamado **artefactos** en la carpeta donde se guarda la plantilla principal.
1. Cree la plantilla vinculada con el JSON siguiente:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountType": {
          "type": "string",
          "defaultValue": "Standard_LRS",
          "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS",
            "Premium_LRS"
          ],
          "metadata": {
            "description": "Storage Account type"
          }
        },
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        }
      },
      "variables": {
        "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "apiVersion": "2019-04-01",
          "name": "[variables('storageAccountName')]",
          "location": "[parameters('location')]",
          "sku": {
            "name": "[parameters('storageAccountType')]"
          },
          "kind": "StorageV2",
          "properties": {}
        }
      ],
      "outputs": {
        "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
        }
      }
    }
    ```

1. Guarde la plantilla como **linkedTemplate.json** en la carpeta **artefactos**.

## <a name="create-template-spec"></a>Creación de una especificación de plantilla

Las especificaciones de plantilla se almacenan en grupos de recursos.  Cree un grupo de recursos y, a continuación, cree una especificación de plantilla con el script siguiente. El nombre de la especificación de plantilla es **webSpec**.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroup `
  -Name templateSpecRG `
  -Location westus2

New-AzTemplateSpec `
  -Name webSpec `
  -Version "1.0.0.0" `
  -ResourceGroupName templateSpecRG `
  -Location westus2 `
  -TemplateFile "c:\Templates\linkedTS\azuredeploy.json"
```

# <a name="cli"></a>[CLI](#tab/azure-cli)

```azurecli
az group create \
  --name templateSpecRG \
  --location westus2

az ts create \
  --name webSpec \
  --version "1.0.0.0" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "<path-to-main-template>"
```

---

Cuando haya terminado, puede ver la especificación de plantilla desde Azure Portal o mediante el cmdlet siguiente:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Get-AzTemplateSpec -ResourceGroupName templatespecRG -Name webSpec
```

# <a name="cli"></a>[CLI](#tab/azure-cli)

```azurecli
az ts show --name webSpec --resource-group templateSpecRG --version "1.0.0.0"
```

---

## <a name="deploy-template-spec"></a>Implementación de la especificación de plantilla

Ahora puede implementar la especificación de plantilla. La implementación de la especificación de plantilla es similar a la implementación de la plantilla que contiene, con la excepción de que se pasa el identificador de recurso de la especificación de plantilla. Puede usar los mismos comandos de implementación y, si es necesario, pasar los valores de parámetro para la especificación de plantilla.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroup `
  -Name webRG `
  -Location westus2

$id = (Get-AzTemplateSpec -ResourceGroupName templateSpecRG -Name webSpec -Version "1.0.0.0").Versions.Id

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName webRG
```

# <a name="cli"></a>[CLI](#tab/azure-cli)

```azurecli
az group create \
  --name webRG \
  --location westus2

id = $(az ts show --name webSpec --resource-group templateSpecRG --version "1.0.0.0" --query "id")

az deployment group create \
  --resource-group webRG \
  --template-spec $id
```

> [!NOTE]
> Existe un problema conocido al obtener el identificador de la especificación de plantilla y luego asignárselo a una variable en Windows PowerShell.

---

## <a name="next-steps"></a>Pasos siguientes

Para información sobre la implementación de una especificación de plantilla como una plantilla vinculada, consulte [Tutorial: Implementación de una especificación de plantilla como una plantilla vinculada](template-specs-deploy-linked-template.md).
