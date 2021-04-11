---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: ab22ad75b5b49588bbdcedf5fc995ce65fe4e690
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105002"
---
Para completar el inicio rápido de reconocimiento de la intención, deberá crear una cuenta de LUIS y un proyecto mediante el portal de la versión preliminar de LUIS. Este inicio rápido solo requiere una suscripción a LUIS. *No* se requiere una suscripción al servicio de voz.

Lo primero que debe hacer es crear una cuenta y una aplicación de LUIS mediante el portal de vista previa de LUIS. La aplicación de LUIS que cree usará un dominio precompilado para la automatización doméstica, que proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión de LUIS que se ejecuta en la nube al que puede llamar mediante el SDK de Voz. 

Siga estas instrucciones para crear una aplicación de LUIS:

* <a href="/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Inicio rápido: Compilación de una aplicación de un dominio creado previamente</a>

Cuando haya terminado, necesitará cuatro cosas:

* Volver a publicar con la **preparación para la voz** activada
* La **clave principal** de LUIS
* La **ubicación** de LUIS
* El **identificador de la aplicación** de LUIS

Aquí es donde puede encontrar esta información en el [portal de vista previa de LUIS](https://preview.luis.ai/):

1. En el portal de la versión preliminar de LUIS, seleccione la aplicación y, a continuación, seleccione el botón **Publicar**.

2. Seleccione el espacio de **producción**, si usa `en-US` cambie la opción **Preparación para la voz** a la posición **On**. A continuación, seleccione el botón **Publicar**.

    > [!IMPORTANT]
    > Se recomienda encarecidamente la **preparación para la voz**, ya que mejorará la precisión del reconocimiento de voz.

    > [!div class="mx-imgBorder"]
    > ![Publicación de LUIS en el punto de conexión](../../../media/luis/publish-app-popup.png)

3. En el portal de vista previa de LUIS, seleccione **Administrar** y, después, seleccione **Recursos de Azure**. En esta página, encontrará la clave y la ubicación de LUIS (que a veces se denomina _región_).

   > [!div class="mx-imgBorder"]
   > ![Clave y ubicación de LUIS](../../../media/luis/luis-key-region.png)

4. Una vez que tenga la clave y ubicación, necesitará el identificador de la aplicación. Seleccione **Configuración de la aplicación** (el identificador de la aplicación está disponible en esta página).

   > [!div class="mx-imgBorder"]
   > ![Identificador de la aplicación de LUIS](../../../media/luis/luis-app-id.png)