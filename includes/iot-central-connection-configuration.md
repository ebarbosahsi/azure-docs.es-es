---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 11/03/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: e4f9fd537d7743a5bbb9d129b21c4bf0a529d32d
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491155"
---
Al ejecutar la aplicación de dispositivo de ejemplo más adelante en este tutorial, necesitará los siguientes valores de configuración:

* Ámbito de identificador: En la aplicación de IoT Central, vaya a **Administration > Device Connection** (Administración > Conexión de dispositivos). Anote el valor de **Ámbito de id**.
* Clave principal del grupo: En la aplicación de IoT Central, vaya a **Administration > Device Connection > SAS-IoT-Devices** (Administración > Conexión de dispositivos > SAS-IoT-Devices). Tome nota del valor de **Primary key** (Clave principal) de la firma de acceso compartido.

Use Cloud Shell para generar una clave de dispositivo a partir de la clave principal que ha recuperado:

```azurecli-interactive
az extension add --name azure-iot
az iot central device compute-device-key --device-id sample-device-01 --pk <the group primary key value>
```

Anote la clave de dispositivo generada, ya que la usará más adelante en este tutorial.
