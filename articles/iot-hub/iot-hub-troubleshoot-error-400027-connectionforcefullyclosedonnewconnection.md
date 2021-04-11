---
title: Solución del error 400027 ConnectionForcefullyClosedOnNewConnection de Azure IoT Hub
description: Corrección del error 400027 ConnectionForcefullyClosedOnNewConnection
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- mqtt
- fasttrack-edit
- iot
ms.openlocfilehash: cd531abe1a10abead26055c6b0d4dd328cd3e836
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061373"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

En este artículo se describen las causas y las soluciones de los errores **400027 ConnectionForcefullyClosedOnNewConnection**.

## <a name="symptoms"></a>Síntomas

El dispositivo se desconecta con **Communication_Error** como **ConnectionStatusChangeReason** con el SDK de .NET y el tipo de transporte MQTT.

La operación gemela del dispositivo a la nube (como las propiedades indicadas de lectura o revisión) o la invocación del método directo producen un error con el código **400027**.

## <a name="cause"></a>Causa

Otro cliente creó una conexión nueva para IoT Hub con las mismas credenciales, por lo que IoT Hub cerró la conexión anterior. IoT Hub no permite que más de un cliente se conecte con el mismo conjunto de credenciales.

## <a name="solution"></a>Solución

Asegúrese de que cada cliente se conecta a IoT Hub mediante su propia identidad.