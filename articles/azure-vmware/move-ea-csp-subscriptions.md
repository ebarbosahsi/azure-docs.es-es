---
title: Traslado de las suscripciones de soluciones de Contrato Enterprise y CSP para Azure VMware Solution
description: Obtenga información acerca de cómo trasladar la nube privada de una suscripción a otra. El traslado se puede realizar por varias razones, como la facturación.
ms.topic: how-to
ms.date: 03/15/2021
ms.openlocfilehash: 608f46dbd84d6bb899a3e7fcd1f8a63b3a5e85fb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103555134"
---
# <a name="move-ea-and-csp-azure-vmware-solution-subscriptions"></a>Traslado de las suscripciones de soluciones de Contrato Enterprise y CSP para Azure VMware Solution

En este artículo, aprenderá acerca de cómo trasladar la nube privada de una suscripción a otra. El traslado se puede realizar por varias razones, como la facturación. 

>[!IMPORTANT]
>Debe tener al menos derechos de colaborador en las suscripciones de origen y de destino. La red virtual y la puerta de enlace de red virtual no se pueden trasladar de una suscripción a otra. Además, el traslado de las suscripciones no tiene ningún impacto en la administración y las cargas de trabajo, como las máquinas virtuales de vCenter, NSX y carga de trabajo.

1. Inicie sesión en Azure Portal y seleccione la nube privada que quiere trasladar.

1. Seleccione el vínculo **Suscripción (cambiar)** .

   :::image type="content" source="media/private-cloud-overview-subscription-id.png" alt-text="Captura de pantalla que muestra los detalles de la nube privada.":::

1. Proporcione los detalles de la suscripción para **Destino** y seleccione **Siguiente**.

   :::image type="content" source="media/move-resources-subscription-target.png" alt-text="Captura de pantalla del recurso de destino." lightbox="media/move-resources-subscription-target.png":::

1. Confirme la validación de los recursos que seleccionó para trasladar y seleccione **Siguiente**. 

   :::image type="content" source="media/confirm-move-resources-subscription-target.png" alt-text="Captura de pantalla que muestra el recurso que se trasladará." lightbox="media/confirm-move-resources-subscription-target.png":::

1. Active la casilla para indicar que entiende que las herramientas y los scripts asociados no funcionarán hasta que los actualice para usar los nuevos identificadores de recurso. Luego, seleccione **Mover**.

   :::image type="content" source="media/review-move-resources-subscription-target.png" alt-text="Captura de pantalla que muestra el resumen del recurso seleccionado que se trasladará." lightbox="media/review-move-resources-subscription-target.png":::

   Una vez que se completa el traslado de recursos, aparece una notificación. La nueva suscripción aparece en la Información general de la nube privada.

   :::image type="content" source="media/moved-subscription-target.png" alt-text="Captura de pantalla que muestra una nueva suscripción." lightbox="media/moved-subscription-target.png":::

