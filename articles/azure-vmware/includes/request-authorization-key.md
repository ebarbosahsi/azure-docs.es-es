---
title: Solicitud de la clave de autorización para ExpressRoute
description: Pasos para solicitar una clave de autorización para ExpressRoute.
ms.topic: include
ms.date: 03/15/2021
ms.openlocfilehash: 99d9fba33d64fca1d9c5b960041fbabe1f9060db
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105027018"
---
<!-- used in expressroute-global-reach-private-cloud.md and create-ipsec-tunnel.md -->

1. En Azure Portal, en la sección **Conectividad**, en la pestaña **ExpressRoute**, seleccione **+ Solicitar una clave de autorización**. 

   :::image type="content" source="../media/expressroute-global-reach/start-request-authorization-key.png" alt-text="Captura de pantalla que muestra cómo solicitar una clave de autorización de ExpressRoute." border="true" lightbox="../media/expressroute-global-reach/start-request-authorization-key.png":::

1. Proporciónele un nombre y luego seleccione **Crear**. 
      
   La clave puede tardar unos 30 segundos en crearse. Una vez creada, la nueva clave aparece en la lista de claves de autorización para la nube privada.

   :::image type="content" source="../media/expressroute-global-reach/show-global-reach-auth-key.png" alt-text="Captura de pantalla que muestra la clave de autorización de ExpressRoute Global Reach.":::
  
1. Copie la clave de autorización y el identificador de ExpressRoute. Los usará para completar el emparejamiento.  

   > [!NOTE]
   > La clave de autorización desaparece tras un tiempo, por lo que debe copiarla en cuanto aparezca.