---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 04/16/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 8833bb75f5f50372deda3e71c1df90bed9a20054
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95996360"
---
## <a name="enable-event-grid-resource-provider"></a>Habilitación del proveedor de recursos de Event Grid

Si aún no ha usado anteriormente Event Grid en su suscripción de Azure, puede que tenga que registrar el proveedor de recursos de Event Grid.

En Azure Portal:

1. Seleccione **Suscripciones** en el menú de la izquierda.
1. Seleccione la suscripción que está usando para Event Grid.
1. En el menú de la izquierda, en **Configuración**, seleccione **Proveedores de recursos**.
1. Busque **Microsoft.EventGrid**.
1. Si no está registrado, seleccione **Registrar**. 

Puede tardar unos instantes en finalizarse el registro. Seleccione **Actualizar**  para actualizar el estado. Cuando el **estado** es **Registrado**, ya está listo para continuar.