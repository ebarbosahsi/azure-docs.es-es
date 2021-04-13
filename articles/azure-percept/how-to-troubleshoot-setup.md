---
title: Solución de problemas durante la experiencia de configuración de Azure Percept DK
description: Obtenga sugerencias para la solución de algunos de los problemas más comunes que pueden surgir durante la experiencia de configuración
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/25/2021
ms.custom: template-how-to
ms.openlocfilehash: 7ce13cedff9afc25900c0bf75359ae49cc29fe19
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105608500"
---
# <a name="azure-percept-dk-setup-experience-troubleshooting-guide"></a>Guía de solución de problemas de la experiencia de configuración de Azure Percept DK

Vea la tabla siguiente para obtener soluciones alternativas a los problemas comunes detectados durante la [experiencia de configuración de Azure Percept DK](./quickstart-percept-dk-set-up.md). Si el problema persiste, póngase en contacto con el servicio de asistencia al cliente de Azure.

|Problema|Motivo|Solución alternativa|
|:-----|:------|:----------|
|Al conectarse a las páginas de registro de la cuenta de Azure o a Azure Portal, puede iniciar sesión automáticamente con una cuenta almacenada en caché. Si esta no es la cuenta que pensaba usar, puede dar lugar a una experiencia incoherente con la documentación.|Esto suele deberse a un valor en el explorador para "recordar" una cuenta que ha utilizado anteriormente.|En la página de Azure, haga clic en el nombre de la cuenta en la esquina superior derecha y seleccione **Cerrar sesión**. Después podrá iniciar sesión con la cuenta correcta.|
|El punto de acceso Wi-Fi de Azure Percept DK (scz-xxxx o apd-xxxx) no aparece en la lista de redes Wi-Fi disponibles.|Suele tratarse de un problema temporal que se resuelve en 15 minutos.|Espere a que aparezca la red. Si no aparece después de más de 15 minutos, reinicie el dispositivo.|
|La conexión con el punto de acceso Wi-Fi de Azure Percept DK se desconecta con frecuencia.|Esto puede deberse a una mala conexión entre el dispositivo y el equipo host. También puede deberse a interferencias de otras conexiones Wi-Fi en el equipo host.|Asegúrese de que las antenas estén conectadas correctamente al kit de desarrollo. Si el kit de desarrollo está alejado del equipo host, intente acercarlo. Desactive cualquier otra conexión a Internet, como LTE/5G, si se están ejecutando en el equipo host.|
|El equipo host muestra una advertencia de seguridad sobre la conexión con el punto de acceso de Azure Percept DK.|Se trata de un problema conocido que se corregirá en una actualización posterior.|Es seguro continuar con la experiencia de configuración.|
|El punto de acceso Wi-Fi de Azure Percept DK (scz-xxxx o apd-xxxx) aparece en la lista de redes, pero no se conecta.|Esto puede deberse a un daño temporal del punto de acceso Wi-Fi del kit de desarrollo.|Reinicie el kit de desarrollo e inténtelo de nuevo.|
|No es posible conectarse a una red Wi-Fi durante la experiencia de instalación.|La red Wi-Fi debe tener conectividad a Internet para comunicarse con Azure. EAP[PEAP/MSCHAP], los portales cautivos y la conectividad de empresa EAP-TLS no se admiten de momento.|Asegúrese de que el tipo de red Wi-Fi es compatible y tiene conectividad a Internet.|