---
title: Plataforma de Azure Industrial IoT
description: En este artículo se proporciona información general sobre la plataforma de Industrial IoT y sus componentes.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: conceptual
ms.date: 3/22/2021
ms.openlocfilehash: 24932b17c630190cb3121d5310a865758fa6a920
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104801560"
---
# <a name="what-is-the-industrial-iot-iiot-platform"></a>¿Qué es la plataforma de Industrial IoT (IIoT)?

La plataforma de Azure Industrial IoT es un conjunto de módulos y servicios de Microsoft que se implementan en Azure. Estos módulos y servicios tienen una versatilidad completamente desarrollada. En concreto, se aplica la oferta administrada de plataforma como servicio (PaaS) de Azure, software de código abierto con licencia de MIT, modelos internacionales abiertos para la comunicación (OPC UA, AMQP, MQTT, HTTP) e interfaces (Open API) y modelos de datos industriales abiertos (OPC UA) en el perímetro y en la nube.

## <a name="enabling-shopfloor-connectivity"></a>Habilitación de la conectividad de la planta 

La plataforma de Azure Industrial IoT abarca la conectividad industrial de los recursos de la planta (incluida la detección de recursos habilitados para OPC UA), normaliza sus datos en formato OPC UA y transmite los datos de telemetría de los recursos a Azure en el formato PubSub de OPC UA. Allí, almacena los datos de telemetría en una base de datos en la nube. Además, la plataforma permite el acceso seguro a los recursos de la planta mediante OPC UA desde la nube. Las funcionalidades de administración de dispositivos (incluida la configuración de seguridad) también están integradas. La funcionalidad de OPC UA se ha creado con la tecnología de contenedores de Docker para facilitar la implementación y administración. En el caso de los recursos que no son de OPC UA, nos hemos asociado con los principales proveedores de conectividad industrial y les hemos ayudado a portar el software del adaptador OPC UA a Azure IoT Edge. Estos adaptadores están disponibles en Azure Marketplace.

## <a name="industrial-iot-components-iot-edge-modules-and-cloud-microservices"></a>Componentes de Industrial IoT: módulos de IoT Edge y microservicios en la nube

Los servicios perimetrales se implementan como módulos de Azure IoT Edge y se ejecutan de forma local. Los microservicios en la nube se implementan como microservicios de ASP.NET con una interfaz de REST y se ejecutan en una instancia administrada de Azure Kubernetes Services o de forma independiente en Azure App Service. En el caso de los servicios perimetrales y en la nube, hemos proporcionado contenedores de Docker creados previamente en Microsoft Container Registry (MCR), lo que elimina este paso para el cliente. Los servicios perimetrales y en la nube se utilizan entre sí y deben usarse juntos. También hemos proporcionado scripts de implementación fáciles de usar que permiten implementar toda la plataforma con un solo comando.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido lo que es la plataforma de Azure Industrial IoT, puede obtener más información sobre OPC Publisher:

> [!div class="nextstepaction"]
> [¿Qué es OPC Publisher?](overview-what-is-opc-publisher.md)