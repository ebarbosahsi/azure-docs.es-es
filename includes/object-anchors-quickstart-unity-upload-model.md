---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 03/02/2021
ms.author: crtreasu
ms.openlocfilehash: d06a6ecd8af16da3e6df21e984fbf6a727fbc27e
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105510"
---
### <a name="upload-your-model"></a>Carga del modelo

Si aún no tiene un modelo de Object Anchors, siga las instrucciones de [Creación de un modelo](../articles/object-anchors/quickstarts/get-started-model-conversion.md) para crear uno. A continuación, vuelva aquí.

Con el dispositivo HoloLens conectado al Portal de dispositivos Windows, siga estos pasos para cargar un modelo y que lo use la aplicación:

1. En el Portal de dispositivos Windows, vaya a **Sistema > Explorador de archivos > LocalAppData**. Allí, debería aparecer la aplicación en la lista de aplicaciones.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localappdata.png" alt-text="Explorador de archivos":::

2. Abra la aplicación y haga clic en la carpeta `LocalState`.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localstate.png" alt-text="Abra la carpeta LocalState":::

3. Cargue el archivo de modelo en la carpeta `LocalState`.

    :::image type="content" source="./media/object-anchors-quickstarts/portal-upload-model.png" alt-text="Cargue el modelo en el portal":::

    Vuelva a iniciar la aplicación desde el dispositivo HoloLens. Ahora puede detectar objetos que coincidan con el modelo.