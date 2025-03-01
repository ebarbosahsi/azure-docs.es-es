---
title: Solución de problemas de microagente de Defender IoT (versión preliminar)
description: Aprenda a controlar errores imprevistos o inexplicables.
ms.date: 1/24/2021
ms.topic: reference
ms.openlocfilehash: 51550a4d3e5042fed7cadc4eac10a0074e954f19
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104782459"
---
# <a name="defender-iot-micro-agent-troubleshooting-preview"></a>Solución de problemas de microagente de Defender IoT (versión preliminar)

En caso de que se produzcan errores inesperados o inexplicables, utilice los siguientes métodos de solución de problemas para intentar resolverlos. Si necesita ayuda, también puede ponerse en contacto con el equipo de producto de Azure Defender para IoT.   

## <a name="service-status"></a>Estado del servicio 

Para ver el estado del servicio: 

1. Ejecute el siguiente comando.

    ```azurecli
    systemctl status defender-iot-micro-agent.service 
    ```

1. Compruebe que el servicio es estable. Para ello, debe asegurarse de que está `active` y de que el tiempo de actividad del proceso es el adecuado.

    :::image type="content" source="media/troubleshooting/active-running.png" alt-text="Asegúrese de que el servicio es estable; para ello, compruebe que está activo y que el tiempo de actividad sea el adecuado.":::

Si el agente no aparece en la lista como `inactive`, use el siguiente comando para iniciar el servicio:

```azurecli
systemctl start defender-iot-micro-agent.service 
```

Sabrá que el servicio se bloquea si el tiempo de actividad del proceso es demasiado corto. Para resolver este problema, debe revisar los registros.

## <a name="review-logs"></a>Revisión de registros 

Use el siguiente comando para comprobar que el servicio de microagente de Defender para IoT se ejecuta con privilegios raíz.

```azurecli
ps -aux | grep " defender-iot-micro-agent"
```

:::image type="content" source="media/troubleshooting/root-privileges.png" alt-text="Compruebe que el servicio de microagente de Defender para IoT se ejecuta con privilegios raíz.":::

Para ver los registros, use el siguiente comando:  

```azurecli
sudo journalctl -u defender-iot-micro-agent | tail -n 200 
```

Para reiniciar el servicio, use el siguiente comando: 

```azurecli
sudo systemctl restart defender-iot-micro-agent  
```

## <a name="next-steps"></a>Pasos siguientes

Consulte [Compatibilidad y retirada de características](edge-security-module-deprecation.md).
