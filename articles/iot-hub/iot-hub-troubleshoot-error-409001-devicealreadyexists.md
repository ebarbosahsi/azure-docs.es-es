---
title: Solución de problemas del error 409001 DeviceAlreadyExists de Azure IoT Hub
description: Corrección del error 409001 DeviceAlreadyExists
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: b075519fff2c7c328778d770ef68e9751922807b
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061135"
---
# <a name="409001-devicealreadyexists"></a>409001 DeviceAlreadyExists

En este artículo se describen las causas y las soluciones de los errores **409001 DeviceAlreadyExists**.

## <a name="symptoms"></a>Síntomas

Al intentar registrar un dispositivo en IoT Hub, la solicitud genera un error **409001 DeviceAlreadyExists**.

## <a name="cause"></a>Causa

Ya existe un dispositivo con el mismo identificador de dispositivo en el centro de IoT. 

## <a name="solution"></a>Solución

Use un identificador de dispositivo distinto e inténtelo de nuevo.