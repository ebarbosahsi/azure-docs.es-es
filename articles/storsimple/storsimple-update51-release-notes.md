---
title: Notas de la versión de la serie StorSimple 8000 Update 5.1
description: Describe las nuevas características, problemas y soluciones alternativas de la serie StorSimple 8000 Update 5.1.
author: alkohli
ms.assetid: ''
ms.service: storsimple
ms.topic: conceptual
ms.date: 03/18/2021
ms.author: alkohli
ms.openlocfilehash: cdb971851ba678ce18f5a1c7954e5620740f3a4c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657576"
---
# <a name="storsimple-8000-series-update-51-release-notes"></a>Notas de la versión de la serie StorSimple 8000 Update 5.1

## <a name="overview"></a>Información general

Las siguientes notas de la versión describen las características nuevas e identifican los problemas críticos abiertos de la serie StorSimple 8000 Update 5.1. También contienen una lista de las actualizaciones de software de StorSimple incluidas en esta versión.

Update 5.1 puede aplicarse a cualquier dispositivo de StorSimple que ejecute Update 5. Si usa una versión inferior a la 5, aplique primero Update 5 y, después, Update 5.1. La versión de dispositivo asociada a Update 5.1 es 6.3.9600.17885.

Revise la información contenida en las notas de la versión antes de implementar la actualización de la solución de StorSimple.

> [!IMPORTANT]
>
> * Update 5.1 es una actualización obligatoria y debe instalarse inmediatamente. Para más información, consulte cómo [aplicar Update 5.1](storsimple-8000-install-update-51.md).
> * Update 5.1 solo incluye actualizaciones de seguridad. Para instalar esta actualización, se necesitan en torno 30 minutos. Le recomendamos encarecidamente que aplique Update 5.1 para garantizar el funcionamiento del dispositivo.
> * Para las nuevas versiones, no podrán ver las actualizaciones de inmediato porque hacemos una implementación por fases de las actualizaciones. Espere unos días y luego busque actualizaciones de nuevo, ya que estas estarán disponibles pronto.

## <a name="whats-new-in-update-51"></a>Novedades de Update 5.1

Se han realizado las siguientes mejoras clave y correcciones de errores en Update 5.1:

* **TLS 1.2**: esta actualización de StorSimple aplicará TLS 1.2 en todos los clientes. Se trata de una actualización obligatoria para todos los dispositivos de la serie StorSimple 8000.

   Si ve la siguiente advertencia, debe actualizar el software del dispositivo antes de continuar:

   Uno o varios dispositivos StorSimple utilizan una versión antigua del software. Es obligatorio utilizar la actualización más reciente disponible de TLS 1.2, por lo que debe instalarse inmediatamente en estos dispositivos. El protocolo TLS 1.2 se usa en todas las comunicaciones de Azure Portal. Sin esta actualización, el dispositivo no podrá comunicarse con el servicio StorSimple.

## <a name="known-issues-in-update-51-from-previous-releases"></a>Problemas conocidos en Update 5.1 de las versiones anteriores

No hay ningún problema conocido en Update 5.1. Si desea ver una lista de los problemas existentes en Update 5.1 que provienen de versiones anteriores, consulte las [notas de la versión de Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="storsimple-cloud-appliance-updates-in-update-51"></a>Actualizaciones de StorSimple Cloud Appliance en Update 5.1

Esta actualización no se puede aplicar a StorSimple Cloud Appliance (también conocido como el dispositivo virtual). Los dispositivos en la nube nuevos deben crearse con la imagen de Update 5.1. Para más información sobre cómo crear una instancia de StorSimple Cloud Appliance, vaya a [Implementación y administración de una instancia de StorSimple Cloud Appliance en Azure (Update 3 y versiones posteriores)](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Paso siguiente

Obtenga información sobre cómo [instalar Update 5.1](storsimple-8000-install-update-51.md) en el dispositivo StorSimple.
