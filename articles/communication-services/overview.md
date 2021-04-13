---
title: ¿Qué es Azure Communication Services?
description: Obtenga información sobre cómo Azure Communication Services le ayuda a desarrollar experiencias de usuario enriquecidas con comunicaciones en tiempo real.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 0efdf48e78d0cc48e288bea354f5de5f9635c760
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2021
ms.locfileid: "106106847"
---
# <a name="what-is-azure-communication-services"></a>¿Qué es Azure Communication Services?

> [!IMPORTANT]
> Las aplicaciones que se crean mediante Azure Communication Services pueden comunicarse con Microsoft Teams. Para más información, consulte nuestra documentación sobre [interoperabilidad de Teams](./quickstarts/voice-video-calling/get-started-teams-interop.md).


Azure Communication Services le permite agregar fácilmente características de comunicación multimedia de voz, vídeo y telefonía sobre IP en tiempo real a sus aplicaciones. Las bibliotecas del SDK de Communication Services también permiten agregar la funcionalidad de chat y SMS a las soluciones de comunicación.

<br>

> [!VIDEO https://www.youtube.com/embed/apBX7ASurgM]

<br>
<br>

Puede usar Communication Services para la comunicación de voz, vídeo, texto y datos en diversos escenarios:

- Comunicación de explorador a explorador, de explorador a aplicación y de aplicación a aplicación
- Usuarios que interactúan con bots u otros servicios
- Usuarios y bots que interactúan sobre la red de telefonía pública conmutada

Se admiten escenarios mixtos. Por ejemplo, una aplicación de Communication Services puede hacer que los usuarios hablen desde los exploradores y los dispositivos de telefonía tradicionales al mismo tiempo. Communication Services también se puede combinar con Azure Bot Service para compilar sistemas de respuesta interactiva de voz (IVR) basados en bot.

## <a name="common-scenarios"></a>Escenarios frecuentes

Los siguientes recursos son un buen punto de partida para empezar a trabajar con Azure Communication Services:
<br>

| Recurso                               |Descripción                           |
|---                                    |---                                   |
|**[Creación de un recurso de Communication Services](./quickstarts/create-communication-resource.md)**|Puede empezar a utilizar Azure Communication Services mediante Azure Portal o el SDK de Communication Services para aprovisionar su primer recurso de Communication Services. Una vez que tenga la cadena de conexión del recurso de Communication Services, puede aprovisionar los primeros tokens de acceso de usuario.|
|**[Obtención de un número de teléfono](./quickstarts/telephony-sms/get-phone-number.md)**|Puede usar Azure Communication Services para aprovisionar y liberar números de teléfono. Estos números de teléfono se pueden usar para iniciar llamadas salientes y compilar soluciones de comunicaciones en SMS.|

Después de crear un recurso de Communication Services, puede empezar a crear escenarios de cliente, como las llamadas de voz y vídeo o el chat de texto.

| Recurso                               |Descripción                           |
|---                                    |---                                   |
|**[Creación del primer token de acceso de usuario](./quickstarts/access-tokens.md)**|Los tokens de acceso de usuario se usan para autenticar los servicios en el recurso de Azure Communication Services. Estos tokens se aprovisionan y se vuelven a emitir mediante el SDK de Communication Services.|
|**[Introducción a las llamadas de voz y vídeo](./quickstarts/voice-video-calling/getting-started-with-calling.md)**| Azure Communication Services permite agregar llamadas de voz y vídeo a las aplicaciones mediante el SDK de llamadas. Esta biblioteca se basa en WebRTC y le permite establecer comunicaciones de punto a punto, multimedia y en tiempo real dentro de sus aplicaciones.|
|**[Incorporación de una aplicación de llamadas a una reunión de Teams](./quickstarts/voice-video-calling/get-started-teams-interop.md)**|Azure Communication Services se puede usar para crear experiencias de reunión personalizadas con interacción con Microsoft Teams. Los usuarios de sus soluciones de Communication Services pueden interactuar con los participantes de Teams mediante la voz, el vídeo, el chat y el uso compartido de la pantalla.|
|**[Introducción al chat](./quickstarts/chat/get-started.md)**|El SDK de chat de Azure Communication Services se puede usar para integrar chats en tiempo real a las aplicaciones.|
|**[Envío de un SMS desde la aplicación](./quickstarts/telephony-sms/send.md)**|El SDK de SMS de Azure Communication Services permite enviar y recibir mensajes SMS desde las aplicaciones de .NET y JavaScript.|

## <a name="samples"></a>Ejemplos

En los siguientes ejemplos se muestra el uso completo de los SDK de Azure Communication Services. No dude en usar estos ejemplos para arrancar sus propias soluciones de Communication Services.
<br>

| Nombre del ejemplo                               | Descripción                           |
|---                                    |---                                   |
|**[Muestra de elementos principales de una llamada grupal](./samples/calling-hero-sample.md)**|Aprenda cómo se pueden usar las bibliotecas del SDK de Communication Services para crear una experiencia de llamada de grupo.|
|**[Muestra de elementos principales de un chat grupal](./samples/chat-hero-sample.md)**|Aprenda cómo se pueden usar las bibliotecas del SDK de Communication Services para crear una experiencia de chat de grupo.|


## <a name="platforms-and-sdk-libraries"></a>Plataformas y bibliotecas del SDK

Los siguientes recursos le ayudarán a obtener información acerca de las bibliotecas del SDK de Azure Communication Services:

| Recurso                               | Descripción                           |
|---                                    |---                                   |
|**[Bibliotecas del SDK y API REST](./concepts/sdk-options.md)**|Las funcionalidades de Azure Communication Services están organizadas conceptualmente en seis áreas, cada una representada por un SDK. Puede decidir qué bibliotecas del SDK usar en función de sus necesidades de comunicación en tiempo real.|
|**[Introducción al SDK de llamadas](./concepts/voice-video-calling/calling-sdk-features.md)**|Consulte la información general del SDK de llamadas de Communication Services.|
|**[Introducción al SDK de chat](./concepts/chat/sdk-features.md)**|Consulte la información general del SDK de chat de Communication Services.|
|**[Introducción al SDK de SMS](./concepts/telephony-sms/sdk-features.md)**|Consulte la información general del SDK de SMS de Communication Services.|

## <a name="other-microsoft-communication-services"></a>Otros servicios de comunicación de Microsoft

Hay otros dos productos de comunicación de Microsoft que puede considerar aprovechar y que no pueden interactuar directamente con Communication Services en este momento:

 - [Las API de comunicación en la nube de Microsoft Graph](/graph/cloud-communications-concept-overview) permiten a las organizaciones crear experiencias de comunicación asociadas a usuarios de Azure Active Directory con licencias de Microsoft 365. Esto es idóneo para las aplicaciones asociadas a Azure Active Directory o en contextos de ampliación de la experiencia de productividad en Microsoft Teams. También hay API para compilar aplicaciones y personalización dentro de la [experiencia de equipos.](/microsoftteams/platform/?preserve-view=true&view=msteams-client-js-latest)

 - [Azure PlayFab Party](/gaming/playfab/features/multiplayer/networking/) simplifica la incorporación de comunicación de datos y chat de baja latencia a los juegos. Aunque puede potenciar el chat de juegos y los sistemas de red con Communication Services, PlayFab es una opción personalizada y gratuita de Xbox.


## <a name="next-steps"></a>Pasos siguientes

 - [Creación de un recurso de Communication Services](./quickstarts/create-communication-resource.md)
