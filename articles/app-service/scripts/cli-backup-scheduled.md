---
title: 'CLI: creación de una copia de seguridad programada'
description: Aprenda a usar la CLI de Azure para automatizar la implementación y administración de la aplicación App Service. En este ejemplo se indica cómo crear una copia de seguridad programada para una aplicación.
author: msangapu-msft
tags: azure-service-management
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.reviewer: cephalin
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 500ac99cd35cfdf601be75a19a1d43f84795cbe8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97006436"
---
# <a name="create-a-scheduled-backup-for-an-app-service-app-using-cli"></a>Creación de una copia de seguridad programada para una aplicación de App Service mediante la CLI

En este script de ejemplo se crea una aplicación en App Service con sus recursos relacionados y, a continuación, se crea una copia de seguridad programada para ella. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, necesitará la CLI de Azure versión 2.0 o posterior. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-scheduled/backup-scheduled.sh?highlight=3-7 "Create a scheduled backup for an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [`az group create`](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [`az storage account create`](/cli/azure/storage/account#az-storage-account-create) | Crea una cuenta de almacenamiento. |
| [`az storage container create`](/cli/azure/storage/container#az-storage-container-create) | Crea un contenedor de Azure Storage. |
| [`az storage container generate-sas`](/cli/azure/storage/container#az-storage-container-generate-sas) | Genera un token de SAS para un contenedor de Azure Storage.  |
| [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) | Crea un plan de App Service, |
| [`az webapp create`](/cli/azure/webapp#az-webapp-create) | Crea una aplicación de App Service. |
| [`az webapp config backup update`](/cli/azure/webapp/config/backup#az-webapp-config-backup-update) | Configura una nueva programación de copia de seguridad para una aplicación de App Service. |
| [`az webapp config backup show`](/cli/azure/webapp/config/backup#az-webapp-config-backup-show) | Muestra la programación de copia de seguridad para una aplicación de App Service. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup#az-webapp-config-backup-list) | Obtiene una lista de las copias de seguridad para una aplicación de App Service. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Puede encontrar ejemplos de script adicionales de la CLI de App Service en la [documentación de Azure App Service](../samples-cli.md).
