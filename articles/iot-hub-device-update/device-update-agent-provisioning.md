---
title: Aprovisionamiento de Device Update para Azure IoT Hub Agent | Microsoft Docs
description: Aprovisionamiento de Device Update para Azure IoT Hub Agent
author: ValOlson
ms.author: valls
ms.date: 2/16/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: e778c7ee14d2115bf6d8cf7f12ceaa61e364a4a2
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2021
ms.locfileid: "106120203"
---
# <a name="device-update-agent-provisioning"></a>Aprovisionamiento del agente de actualización de dispositivos

El agente del módulo de actualización de dispositivos se puede ejecutar junto con otros procesos del sistema y [módulos IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules) que se conectan a la IoT Hub como parte del mismo dispositivo lógico. En esta sección se describe cómo aprovisionar el agente de actualización de dispositivos como una identidad de módulo. 


## <a name="module-identity-vs-device-identity"></a>Identidad de módulo e identidad de dispositivo

En IoT Hub, en cada identidad de dispositivo, puede crear hasta 50 identidades de módulos. Cada identidad de módulo genera implícitamente un módulo gemelo. En el lado del dispositivo, los SDK de dispositivo de IoT Hub le permiten crear módulos, donde cada uno abre una conexión independiente a IoT Hub. La identidad del módulo y el módulo gemelo proporcionan funcionalidades similares que la identidad del dispositivo y el dispositivo gemelo, pero con una mayor granularidad. [Más información sobre identidades de módulo en IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins)


## <a name="support-for-device-update"></a>Compatibilidad con Device Update

Actualmente se admiten los siguientes tipos de dispositivos de IoT con Device Update:

* Dispositivos Linux (dispositivos IoT Edge y que no son de IoT Edge):
    * Actualización de la imagen A/B:
        - Yocto-ARM64 (imagen de referencia), extensible a través de código abierto para [compilar sus propias imágenes](device-update-agent-provisioning.md#how-to-build-and-run-device-update-agent) para otra arquitectura según sea necesario.
        - Simulador de Ubuntu 18.04
       
    * El agente de paquete admite compilaciones para las siguientes plataformas/arquitecturas:
        - Agente del paquete de servidor 18.04 x64 de Ubuntu 
        - Debian 9 
        
* Dispositivos restringidos:
    * Ejemplos de agentes de actualización de dispositivos AzureRTOS: [Tutorial de actualización de dispositivos para Azure IoT Hub del sistema operativo en tiempo real de Azure](device-update-azure-real-time-operating-system.md)

* Dispositivos desconectados: 
    * [Información sobre la compatibilidad con la actualización de dispositivos desconectados](connected-cache-disconnected-device-update.md)


## <a name="prerequisites"></a>Requisitos previos  

Si está configurando el dispositivo de IoT Edge y dispositivo de IoT para [las actualizaciones basadas en paquetes](https://docs.microsoft.com/azure/iot-hub-device-update/understand-device-update#support-for-a-wide-range-of-update-artifacts), agregue packages.microsoft.com a los repositorios de la máquina siguiendo estos pasos:

1. Inicie sesión en el equipo o el dispositivo de IoT en el que desea instalar el agente de actualización de dispositivos.

1. Abra una ventana del terminal.

1. Instale la configuración del repositorio que coincida con el sistema operativo del dispositivo.
    ```shell
    curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
    ```
    
1. Copie la lista generada en el directorio sources.list.d.
    ```shell
    sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
    ```
    
1. Instale la clave pública de GPG de Microsoft.
    ```shell
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    ```

    ```shell
    sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
    ```

## <a name="how-to-provision-the-device-update-agent-as-a-module-identity"></a>Cómo aprovisionar el agente de actualización de dispositivos como una identidad de módulo

En esta sección se describe cómo aprovisionar el agente de actualización de dispositivos como una identidad de módulo en dispositivos IoT Edge habilitados, dispositivos IoT no perimetrales y otros dispositivos IoT.


### <a name="on-iot-edge-enabled-devices"></a>En dispositivos habilitados para IoT Edge

Siga estas instrucciones para aprovisionar el agente de actualización de dispositivos en los [dispositivos habilitados para IoT Edge](https://docs.microsoft.com/azure/iot-edge).

1. Siga las instrucciones que se indican en [Instalación y aprovisionamiento del entorno de ejecución de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge?view=iotedge-2020-11&preserve-view=true).

1. A continuación, instale el agente de actualización de dispositivos desde [Artifacts](https://github.com/Azure/iot-hub-device-update/releases) y ya está listo para iniciar el agente de actualización de dispositivos en el dispositivo IoT Edge.


### <a name="on-non-edge-iot-linux-devices"></a>En dispositivos de IoT Linux que no son Edge

Siga estas instrucciones para aprovisionar el agente de actualización de dispositivos en los dispositivos para IoT Linux.

1. Instale el servicio de identidad de IoT y agregue la versión más reciente al dispositivo de IoT. 
    1. Inicie sesión en el equipo o en el dispositivo IoT.
    1. Abra una ventana de terminal.
    1.  Instale el [servicio de identidad de IoT](https://github.com/Azure/iot-identity-service/blob/main/docs/packaging.md#installing-and-configuring-the-package) más reciente en el dispositivo IoT con este comando:
    
        ```shell
        sudo apt-get install aziot-identity-service
        ```
        
1. Aprovisionamiento del servicio de identidad de IoT para obtener la información del dispositivo IoT.
    * Cree una copia personalizada de la plantilla de configuración para poder agregar la información de aprovisionamiento. En un terminal, introduzca los siguientes comandos.
      
        ```shell
        sudo cp /etc/aziot/config.toml.template /etc/aziot/config.toml 
        ```
   
1. Después, edite el archivo de configuración para incluir la cadena de conexión del dispositivo que desea que actúe como el aprovisionador de este dispositivo o equipo. En un terminal, introduzca los siguientes comandos.

    ```shell
    sudo nano /etc/aziot/config.toml
    ```
   
1. Debe ver un mensaje similar al siguiente ejemplo:

    :::image type="content" source="media/understand-device-update/config.png" alt-text="Diagrama del archivo de configuración del servicio de identidad de IoT." lightbox="media/understand-device-update/config.png":::

    1. En la misma ventana de nano, busque el bloque con "aprovisionamiento manual con cadena de conexión".
    1. En la ventana, elimine el símbolo "#" antes de "aprovisionamiento"
    1. En la ventana, elimine el símbolo "#" antes de "origen" 
    1. En la ventana, elimine el símbolo "#" antes de 'connection_string'
    1. En la ventana, elimine la cadena entre comillas a la derecha de 'connection_string' y, a continuación, agregue la cadena de conexión allí 
    1. Guarde los cambios realizados en el archivo con 'Ctrl + X' y luego 'Y' y presione la tecla 'enter' para guardar los cambios. 
    
1.  Ahora aplique y reinicie el servicio de identidad de IoT con el siguiente comando. Ahora debería ver "Listo" impreso, lo que significa que ha configurado correctamente el servicio de identidad de IoT.

    > [!Note]
    > Actualmente, el servicio de identidad de IoT registra las identidades de módulo con IoT Hub utilizando claves simétricas.
    
    ```shell
    sudo aziotctl config apply
    ```
    
1.  Finalmente, instale el agente de actualización de dispositivos desde [Artifacts](https://github.com/Azure/iot-hub-device-update/releases) y ya está listo para iniciar el agente de actualización de dispositivos en el dispositivo IoT Edge.


### <a name="other-iot-devices"></a>Otros dispositivos IoT

El agente de actualización de dispositivos también puede configurarse sin el servicio de identidad de IoT para probar o en dispositivos restringidos. Siga los pasos que se indican a continuación para aprovisionar el agente de actualización de dispositivos mediante una cadena de conexión (desde el módulo o el dispositivo).

1.  Instale el agente de actualización de dispositivos desde [Artifacts](https://github.com/Azure/iot-hub-device-update/releases).

1.  Inicie sesión en el equipo o en un dispositivo IoT Edge o de IoT.
    
1.  Abra una ventana de terminal.

1.  Agregue la cadena de conexión en el [archivo de configuración de Device Update](device-update-configuration-file.md):
    1. Escriba lo siguiente en la ventana del terminal:
        - Uso de [actualizaciones de paquete](device-update-ubuntu-agent.md): sudo nano/etc/adu/adu-conf.txt
        - Uso de [actualizaciones de imagen](device-update-raspberry-pi.md): sudo nano/adu/adu-conf.txt
       
    1. Debería ver una ventana abierta con texto. Elimine la cadena completa después de 'connection_String=' la primera vez que aprovisione el agente de actualización de dispositivos en el dispositivo IoT. Solo se coloca el texto de marcador.
    
    1. En el terminal, reemplace <your-connection-string> por la cadena de conexión del dispositivo para la instancia del agente de actualización de dispositivos.
    
        > [!Important]
        > No agregue comillas para la cadena de conexión.
        
        - connection_string=<your-connection-string>
       
    1. Introdúzcalo y guárdelo.
    
1.  Ahora ya está listo para iniciar el agente de actualización de dispositivos en el dispositivo de IoT Edge. 


## <a name="how-to-start-the-device-update-agent"></a>Cómo iniciar el agente de actualización de dispositivos

En esta sección se describe cómo iniciar y comprobar el agente de actualización de dispositivos como una identidad de módulo que se ejecuta correctamente en el dispositivo de IoT.

1.  Inicie sesión en el equipo o dispositivo que tiene instalado el agente de actualización de dispositivos.

1.  Abra la ventana del terminal e introduzca el comando siguiente.
    ```shell
    sudo systemctl restart adu-agent
    ```
    
1.  Puede comprobar el estado del agente mediante el siguiente comando. Si ve algún problema, consulte esta guía de [solución de problemas](troubleshoot-device-update.md).
    ```shell
    sudo systemctl status adu-agent
    ```
    
    Debería ver el estado como Correcto.

1.  En el portal de IoT Hub, vaya a IoT Device o dispositivos de IoT Edge para buscar el dispositivo que configuró con el agente de actualización de dispositivos. Allí verá que el agente de actualización de dispositivos se ejecuta como un módulo. Por ejemplo:

    :::image type="content" source="media/understand-device-update/device-update-module.png " alt-text="Diagrama del nombre del módulo de actualización de dispositivos." lightbox="media/understand-device-update/device-update-module.png":::


## <a name="how-to-build-and-run-device-update-agent"></a>Cómo compilar y ejecutar el agente de actualización de dispositivos

También puede crear y modificar su propio agente de actualización de dispositivos de cliente.

Siga las instrucciones para [compilar](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-build-agent-code.md) Device Update Agent desde el código fuente.

Una vez que el agente se compila correctamente, es el momento de [ejecutarlo](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-run-agent.md).

Ahora, realice los cambios necesarios para incorporar el agente a la imagen.  Consulte las instrucciones sobre cómo [modificar](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-modify-the-agent-code.md) Device Update Agent.


## <a name="troubleshooting-guide"></a>Guía de solución de problemas

Si tiene problemas, revise la [guía de solución de problemas](troubleshoot-device-update.md) de Device Update para IoT Hub como ayuda para resolverlos y recopilar la información necesaria que debe proporcionar a Microsoft.


## <a name="next-steps"></a>Pasos siguientes

Puede usar los siguientes archivos binarios e imágenes pregeneradas para obtener una demostración sencilla de la actualización de dispositivos para IoT Hub:

- [Actualización de la imagen: introducción con Raspberry PI 3 B + referencia a la imagen de Yocto](device-update-raspberry-pi.md) extensible a través de código abierto para compilar sus propias imágenes para otra arquitectura según sea necesario.

- [Introducción al agente de referencia del simulador de Ubuntu (18.04 x64)](device-update-simulator.md)

- [Actualización basada en paquete: Introducción al agente de paquetes de Ubuntu Server 18.04 x64](device-update-ubuntu-agent.md)

- [Tutorial de actualización de dispositivos para Azure IoT Hub para sistemas operativos en tiempo real de Azure](device-update-azure-real-time-operating-system.md)

