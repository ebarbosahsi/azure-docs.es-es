---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 03/04/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2459cf6c5055dcde83dccf37addc160aeeaa64ad
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198667"
---
Los dominios de eventos se explican más fácilmente con un ejemplo. Supongamos que dirige Contoso Construction Machinery, donde se encarga de fabricar tractores, equipos de excavación y otra maquinaria pesada. Entre las tareas de la empresa se incluye el envío de información en tiempo real a los clientes sobre el mantenimiento de los equipos, el estado de los sistemas y las actualizaciones de contratos. Toda esta información llega a diferentes puntos de conexión, entre los que están su aplicación, los puntos de conexión de los clientes y otras infraestructuras que estos han configurado.

Los dominios de eventos le permiten modelar Contoso Construction Machinery como una única entidad de creación de eventos. Cada uno de los clientes se representa como un tema dentro del dominio. La autenticación y la autorización se controlan mediante Azure Active Directory. Cada uno de los clientes puede suscribirse a su tema para recibir eventos relacionados. El acceso administrado a través del dominio de eventos garantiza que solo pueden acceder a su tema.

También le ofrece un punto de conexión único, en el que puede publicar todos sus eventos de cliente. Event Grid se encarga de asegurarse de que cada tema solo esté al tanto de los eventos asignados a su inquilino.

:::image type="content" source="./media/event-grid-domain-example-use-case/contoso-construction-example.png" alt-text="Ejemplo de Contoso Construction" lightbox="./media/event-grid-domain-example-use-case/contoso-construction-example.png":::