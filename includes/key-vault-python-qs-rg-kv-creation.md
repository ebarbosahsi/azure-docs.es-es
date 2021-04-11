---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 09/03/2020
ms.author: msmbaldwin
ms.openlocfilehash: 59359d37fe347c8568c7944f75accdbc04cddb93
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105967134"
---
1. Uso del comando `az group create` para crear un grupo de recursos:

    ```azurecli
    az group create --name KeyVault-PythonQS-rg --location eastus
    ```

    Si lo prefiere, puede cambiar "eastus" a una ubicación más próxima.

1. Use `az keyvault create` para crear el almacén de claves:

    ```azurecli
    az keyvault create --name <your-unique-keyvault-name> --resource-group KeyVault-PythonQS-rg
    ```

    Reemplace `<your-unique-keyvault-name>` por un nombre que sea único en todo Azure. Normalmente, se usa el nombre personal o de la empresa, junto con otros números e identificadores. 

1. Cree una variable de entorno que especifique el nombre del almacén de claves al código:

    ```bash
    export KEY_VAULT_NAME=<your-unique-keyvault-name>
    ```