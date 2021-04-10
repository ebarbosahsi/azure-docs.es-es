---
title: Compatibilidad y retirada de características
description: Defender para IoT seguirá dando soporte a C, C# y Edge hasta el 1 de marzo de 2022.
ms.date: 1/21/2021
ms.topic: how-to
ms.openlocfilehash: 782e2e8ab0c54e21da643ca73f647a7ea21e4223
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104784533"
---
# <a name="feature-support-and-retirement"></a>Compatibilidad y retirada de características

En este artículo se describen las características de Azure Defender para IoT y la compatibilidad con diferentes funcionalidades dentro de Defender para IoT.

## <a name="defender-for-iot-c-c-and-edge-defender-iot-micro-agent-deprecation"></a>Retirada de Defender-IoT-micro-agent de Defender para IoT de C, C# y Edge

El nuevo microagente reemplazará al agente Defender-IoT-micro-agent de C, C# y Edge.  

El nuevo microagente se basa en el conocimiento y la experiencia obtenidos no solo durante el desarrollo de Defender-IoT-micro-agent existente, sino también de los comentarios de los clientes y asociados, y tiene cuatro mejoras importantes: 

- **Valor de seguridad de profundidad**: el nuevo agente se ejecutará en el nivel de host, lo que proporcionará más visibilidad a las operaciones subyacentes del dispositivo y mayor cobertura de seguridad.

- **Mayor rendimiento de los dispositivos y menor superficie**: se consigue gracias a una RAM y a una superficie de memoria ROM pequeñas, así como un bajo consumo de la CPU.  

- **Plug and play**: el nuevo microagente ya no tiene dependencias en el nivel de kernel y todas sus dependencias de software se proporcionan como parte de su paquete. El microagente admite una arquitectura de CPU común.

- **Fácil de implementar**: El microagente admite diferentes modelos de distribución, a través de código fuente y en forma de paquete binario. 

### <a name="timeline"></a>Escala de tiempo 

Defender para IoT seguirá dando soporte a C, C# y Edge hasta el 1 de marzo de 2022. 

## <a name="micro-agent-preview-support"></a>Compatibilidad con la versión preliminar del microagente

Durante la versión preliminar, el microagente puede experimentar cambios importantes sin previo aviso.

## <a name="next-steps"></a>Pasos siguientes

Consulte [API del sensor y la consola de administración de Defender para IoT](references-work-with-defender-for-iot-apis.md).