---
title: Defender-IoT-micro-agent y los dispositivos gemelos
description: Obtenga información sobre el concepto de los módulos gemelos de Defender-IoT-micro-agent y cómo se usan en Defender para IoT.
ms.topic: conceptual
ms.date: 07/24/2019
ms.openlocfilehash: 1ed6faf03d168ed7a2a2f07733cb7e238d234915
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104779178"
---
# <a name="defender-iot-micro-agent"></a>Defender-IoT-micro-agent

En este artículo se explica el modo en que Defender para IoT usa módulos y dispositivos gemelos.

## <a name="device-twins"></a>Dispositivos gemelos

En el caso de las soluciones de IoT integradas en Azure, los dispositivos gemelos desempeñan un rol clave tanto en la administración de dispositivos como en la automatización de procesos.

Defender para IoT ofrece integración completa con la plataforma de administración de dispositivos de IoT existente, lo que permite no solo administrar el estado de seguridad de los dispositivos, sino también hacer uso de las funcionalidades de control de dispositivos existentes. La integración se consigue usando el mecanismo gemelo de IoT Hub.

Obtenga más información sobre el concepto de [dispositivos gemelos](../iot-hub/iot-hub-devguide-device-twins.md) en Azure IoT Hub.

## <a name="defender-iot-micro-agent-twins"></a>Módulos gemelos de Defender-IoT-micro-agent

Defender para IoT mantiene un módulo gemelo de Defender-IoT-micro-agent para cada dispositivo en el servicio.
Este módulo gemelo contiene toda la información pertinente para la seguridad de cada dispositivo específico de la solución.
En un módulo gemelo de Defender-IoT-micro-agent dedicado se conservan las propiedades de seguridad del dispositivo para que la comunicación sea más segura y para permitir unas actualizaciones y un mantenimiento que requieren menos recursos.

Consulte [Creación de un módulo gemelo de Defender-IoT-micro-agent](quickstart-create-security-twin.md) y [Configuración de agentes de seguridad](how-to-agent-configuration.md) para obtener información sobre cómo crear, personalizar y configurar el módulo gemelo. Vea [Descripción de módulos gemelos](../iot-hub/iot-hub-devguide-module-twins.md) para obtener más información sobre el concepto de módulo gemelo en IoT Hub.

## <a name="see-also"></a>Consulte también

- [Información general de Defender para IoT](overview.md)
- [Implementación de agentes de seguridad](how-to-deploy-agent.md)
- [Métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md)