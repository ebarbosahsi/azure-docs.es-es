---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/21/2019
ms.author: alkohli
ms.openlocfilehash: 0c6845f081ccbe42e70964eaa939d58597e3b69b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96185550"
---
1. Vaya a la interfaz de usuario web local del dispositivo e inicie sesión en el dispositivo. Asegúrese de que el dispositivo está desbloqueado.

2. Vaya a la página **Configuración de red**. Anote la dirección IP del dispositivo de la interfaz de red utilizada para conectar con el cliente.

3. Si está trabajando con un cliente Windows remoto, inicie el **Bloc de notas** como administrador y, a continuación, abra el archivo de hosts que se encuentra en `C:\Windows\System32\Drivers\etc`.

4. Agregue la entrada siguiente a su archivo hosts: `<Device IP address> <Blob service endpoint>`

    Obtuvo el punto de conexión de Blob service desde la cuenta de almacenamiento de Edge que creó en Azure Portal. Solo usará el sufijo del punto de conexión de Blob service.

    Como referencia, utilice la siguiente imagen. Guarde el archivo `hosts`.

    ![Modifique el archivo de hosts en el cliente Windows](media/azure-stack-edge-gateway-add-device-ip-address-blob-service-endpoint/hosts-file-1.png)