---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: 979bc3ea6a01d31e64aa503216b8fd69164eadc6
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450208"
---
Azure Defender proporciona protección contra amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Azure Defender proporciona protección contra amenazas en diferentes niveles: 

* **Nivel de host (proporcionado por Azure Defender para servidores)** : con el mismo agente de Log Analytics que Security Center usa en otras máquinas virtuales, Azure Defender supervisa los nodos de AKS de Linux en busca de actividades sospechosas, como la detección de shell web y la conexión con direcciones IP sospechosas conocidas. El agente también supervisa análisis específicos de contenedores, como la creación de contenedores con privilegios, las actividades sospechosas de acceso a los servidores de API y los servidores de Secure Shell (SSH) que se ejecutan dentro de un contenedor de Docker.

    Si decide no instalar los agentes en los hosts, solo recibirá un subconjunto de las ventajas de la protección contra amenazas y las alertas de seguridad. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.

    >[!IMPORTANT]
    > Actualmente no se admite la instalación del agente de Log Analytics en los clústeres de Azure Kubernetes Service que se ejecutan en conjuntos de escalado de máquinas virtuales.

    Para obtener una lista de las alertas de nivel de host de AKS, consulte la [Tabla de referencia de alertas](../articles/security-center/alerts-reference.md#alerts-containerhost).


* **Nivel de clúster de AKS (proporcionado por Azure Defender para Kubernetes)** : en el nivel de clúster, la protección contra amenazas se basa en analizar los registros de auditoría de Kubernetes. Para habilitar esta supervisión **sin agente**, habilite Azure Defender. Para generar alertas en este nivel, Azure Defender supervisa los servicios que administra AKS con los registros que recupera el mismo AKS. Entre los ejemplos de eventos en este nivel se incluyen los paneles de Kubernetes expuestos y la creación de roles con privilegios elevados y de montajes confidenciales.

    >[!NOTE]
    > Igualmente, Security Center genera alertas de seguridad para las acciones y las implementaciones de Azure Kubernetes Service que se producen después de que la opción de Kubernetes esté habilitada en la configuración de la suscripción. 

    Para obtener una lista de las alertas de nivel de clúster de AKS, consulte la [Tabla de referencia de alertas](../articles/security-center/alerts-reference.md#alerts-akscluster).

Además, nuestro equipo global de investigadores de seguridad supervisa constantemente el panorama de las amenazas. Agregan alertas específicas del contenedor y vulnerabilidades a medida que se detectan.

> [!TIP]
> Puede simular las alertas de contenedor al seguir las instrucciones de [esta entrada de blog](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).