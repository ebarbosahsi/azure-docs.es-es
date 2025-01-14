---
title: Inserción de Teams en el marco de trabajo de la interfaz de usuario
titleSuffix: An Azure Communication Services quickstart
description: En este documento, obtendrá información sobre cómo se puede usar la funcionalidad de inserción de Teams en el marco de trabajo de la interfaz de usuario de Azure Communication Services para crear experiencias de llamada llave en mano.
author: tophpalmer
ms.author: chpalm
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: e55cfb1a4dff7bfda2323e68777d6f50514b1608
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105930241"
---
# <a name="teams-embed"></a>Inserción de Teams

[!INCLUDE [Public Preview Notice](../../includes/private-preview-include.md)]


La inserción de Teams es una funcionalidad de Azure Communication Services que se centra en las interacciones de llamada de negocio a consumidor y de negocio a negocio. El núcleo del sistema de inserción de Teams es la [llamada de vídeo y de voz](../voice-video-calling/calling-sdk-features.md), pero el sistema de inserción de Teams se crea en las primitivas de llamada de Azure para ofrecer una experiencia de usuario completa basándose en las reuniones de Microsoft Teams.

Los SDK de inserción de Teams son de código cerrado y permiten que estén disponibles en un formato compuesto y llave en mano. Se coloca la inserción de Teams en el lienzo de la aplicación y el SDK genera una experiencia de usuario completa. Dado que esta experiencia del usuario es muy similar a las reuniones de Microsoft Teams, puede aprovecharse de lo siguiente:

- Reducción del tiempo de desarrollo y complejidad de ingeniería
- Familiaridad del usuario final con Teams
- Capacidad de volver a usar el [contenido de aprendizaje del usuario final de Teams](https://support.microsoft.com/office/meetings-in-teams-e0b0ae21-53ee-4462-a50d-ca9b9e217b67)

La inserción de Teams proporciona la mayoría de las características admitidas en las reuniones de Teams, entre las que se incluyen:

- Una experiencia previa a la reunión en la que un usuario configura sus dispositivos de audio y vídeo
- Una experiencia dentro de la reunión para configurar dispositivos de audio y vídeo
- [Fondos del vídeo](https://support.microsoft.com/office/change-your-background-for-a-teams-meeting-f77a2381-443a-499d-825e-509a140f4780): permite que los participantes desenfoquen o reemplacen su fondo.
- [Varias opciones para la galería de vídeos](https://support.microsoft.com/office/using-video-in-microsoft-teams-3647fc29-7b92-4c26-8c2d-8a596904cdae): galería grande, modo juntos, enfoque, anclaje y Spotlight
- [Uso compartido de contenido](https://support.microsoft.comoffice/share-content-in-a-meeting-in-teams-fcc2bf59-aecd-4481-8f99-ce55dd836ce8#ID0EABAAA=Mobile): permite que los participantes compartan su pantalla.

Para más información sobre esta interfaz de usuario en comparación con otros SDK de comunicación de Azure, consulte la [Introducción al concepto del SDK de IU](ui-sdk-overview.md). 
