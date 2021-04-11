---
author: baanders
description: archivo de inclusión para cargar un modelo en la instancia de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 3/23/2021
ms.author: baanders
ms.openlocfilehash: 987409f070f34f0fd9ee212fab8cc57e889e0146
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "104950609"
---
El modelo tiene este aspecto:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

Para **cargar este modelo en la instancia del gemelo**, ejecute el siguiente comando de la CLI de Azure, que carga el modelo anterior como JSON insertado. Puede ejecutar el comando en [Azure Cloud Shell](../articles/cloud-shell/overview.md) en el explorador o en su máquina si tiene la CLI [instalada localmente](/cli/azure/install-azure-cli).

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

> [!Note]
> Si usa Cloud Shell en el entorno de PowerShell, puede que deba colocar el carácter de escape delante de las comillas en los campos de JSON insertado para que sus valores se analicen correctamente. Este es el comando para cargar el modelo con esta modificación:
>
> Cargar el modelo:
> ```azurecli-interactive
> az dt model create --models '{  \"@id\": \"dtmi:contosocom:DigitalTwins:Thermostat;1\",  \"@type\": \"Interface\",  \"@context\": \"dtmi:dtdl:context;2\",  \"contents\": [    {      \"@type\": \"Property\",      \"name\": \"Temperature\",      \"schema\": \"double\"    }  ]}' -n {digital_twins_instance_name}
> ```