---
title: Solución de problemas del error 404001 DeviceNotFound de Azure IoT Hub
description: Corrección del error 404001 DeviceNotFound
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 09f3c00dadc6e95aae01fb8082fb746e155d4688
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061288"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

En este artículo se describen las causas y las soluciones de los errores **404001 DeviceNotFound**.

## <a name="symptoms"></a>Síntomas

Durante una comunicación de la nube al dispositivo (C2D), como el mensaje C2D, la actualización gemela o el método directo, la operación produce un error **404001 DeviceNotFound**.

## <a name="cause"></a>Causa

No se pudo realizar la operación porque IoT Hub no encuentra el dispositivo. El dispositivo no está registrado o está deshabilitado.

## <a name="solution"></a>Solución

Registre el identificador de dispositivo que usó e inténtelo de nuevo.