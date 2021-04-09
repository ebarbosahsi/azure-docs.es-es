---
title: Creación de imágenes de máquina virtual a partir de imágenes generalizadas de un VHD de Windows para el dispositivo de GPU de Azure Stack Edge Pro
description: Describe cómo se crean imágenes de máquina virtual a partir de imágenes generalizadas desde un VHD o VHDX de Windows. Utilice esta imagen generalizada para crear imágenes de máquina virtual que se usarán con máquinas virtuales implementadas en el dispositivo de GPU de Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/18/2021
ms.author: alkohli
ms.openlocfilehash: 978099accd57d15c750a27f77b2e220f569a2dd0
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "104962378"
---
# <a name="use-generalized-image-from-windows-vhd-to-create-a-vm-image-for-your-azure-stack-edge-pro-device"></a>Uso de una imagen generalizada de un VHD de Windows para crear una imagen de máquina virtual para el dispositivo de Azure Stack Edge Pro

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Para implementar máquinas virtuales en el dispositivo Azure Stack Edge Pro, debe ser capaz de crear imágenes personalizadas de máquina virtual que puede usar para crear máquinas virtuales. En este artículo se describen los pasos necesarios para preparar un VHD o VHDX de Windows a fin de crear una imagen generalizada. Esta imagen generalizada se usa para crear una imagen de máquina virtual para el dispositivo de Azure Stack Edge Pro. 

## <a name="about-preparing-windows-vhd"></a>Acerca de la preparación de un VHD de Windows

Se puede utilizar un VHD o un VHDX de Windows para crear una imagen *generalizada* o una imagen *especializada*. En la tabla siguiente se resumen las diferencias principales entre las imágenes *generalizadas* y las *especializadas*.


|Tipo de imagen  |Generalizada  |Especializada  |
|---------|---------|---------|
|Destino     |Implementado en cualquier sistema         | Destinado a un sistema específico        |
|Configuración después del arranque     | Configuración requerida en el primer arranque de la máquina virtual.          | No se necesita configuración. <br> La plataforma enciende la máquina virtual.        |
|Configuración     |Nombre de host, administrador-usuario y otros valores específicos de la máquina virtual necesarios.         |Totalmente preconfigurado.         |
|Se usa al     |Crear varias máquinas virtuales nuevas a partir de la misma imagen.         |Migrar una máquina específica o restaurar una máquina virtual a partir de una copia de seguridad anterior.         |


En este artículo se describen los pasos necesarios para realizar la implementación a partir de una imagen generalizada. Para realizar la implementación a partir de una imagen especializada, consulte el artículo sobre [VHD de Windows especializado](azure-stack-edge-placeholder.md) para el dispositivo.

> [!IMPORTANT]
> Este procedimiento no cubre los casos en los que el VHD de origen se configura con configuraciones e instalaciones personalizadas. Por ejemplo, es posible que se necesiten acciones adicionales para generalizar un VHD que contenga reglas de firewall personalizadas o valores de proxy. Para más información sobre estas acciones adicionales, consulte el artículo sobre la [preparación de un VHD de Windows para cargarlo en Azure - Azure Virtual Machines](../virtual-machines/windows/prepare-for-upload-vhd-image.md).


## <a name="vm-image-workflow"></a>Flujo de trabajo de la imagen de máquina virtual

El flujo de trabajo de alto nivel para preparar un VHD de Windows para utilizarlo como una imagen generalizada consta de los siguientes pasos:

1. Convierta el VHD o VHDX de origen a un VHD de tamaño fijo.
1. Cree una máquina virtual en Hyper-V con el VHD fijo.
1. Conéctese a la máquina virtual de Hyper-V.
1. Generalice el VHD mediante la utilidad *sysprep*. 
1. Copie la imagen generalizada en Blob Storage.
1. Utilice la imagen generalizada para implementar máquinas virtuales en el dispositivo. Para más información, consulte cómo [implementar una máquina virtual mediante Azure Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md) o cómo [implementar una máquina virtual mediante PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md).


## <a name="prerequisites"></a>Requisitos previos

Antes de preparar un VHD de Windows para usarlo como imagen generalizada en Azure Stack Edge, asegúrese de que:

- Tiene un VHD o un VHDX que contenga una versión compatible de Windows. Consulte el artículo sobre [sistemas operativos invitados compatibles]() para su instancia de Azure Stack Edge Pro. 
- Tiene acceso a un cliente de Windows con el administrador de Hyper-V instalado. 
- Tiene acceso a una cuenta de Azure Blob Storage para almacenar el VHD una vez preparado.

## <a name="prepare-a-generalized-windows-image-from-vhd"></a>Preparación de una imagen de Windows generalizada a partir de un VHD

## <a name="convert-to-a-fixed-vhd"></a>Conversión a un VHD fijo 

En su dispositivo, necesitará VHD de tamaño fijo para crear imágenes de máquina virtual. Deberá convertir el VHD o el VHDX de Windows de origen a un VHD fijo. Siga estos pasos:

1. Abra el administrador de Hyper-V en el sistema cliente. Vaya a **Editar disco**.

    ![Apertura del administrador de Hyper-V](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-1.png)

1. En la página **Antes de comenzar**, seleccione **Siguiente>** .

1. En la página **Localizar disco duro virtual**, busque la ubicación del VHD o el VHDX de origen de Windows que desee convertir. Seleccione **Siguiente>** .

    ![Página Localizar disco duro virtual](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-2.png)

1. En la página **Elegir acción**, seleccione **Convertir** y, después, **Siguiente>** .

    ![Página Elegir acción](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-3.png)

1. En la página **Elegir formato de disco**, seleccione el formato **VHD** y, a continuación, **Siguiente>** .

   ![Página Elegir formato de disco](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-4.png)


1. En la página **Elegir tipo de disco**, seleccione **Tamaño fijo** y, después, **Siguiente>** .

   ![Página Elegir tipo de disco](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-5.png)


1. En la página **Configurar disco**, busque la ubicación y especifique un nombre para el disco VHD de tamaño fijo. Seleccione **Siguiente>** .

   ![Página Configurar disco](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-6.png)


1. Revise el resumen y seleccione **Finalizar**. La conversión de VHD o VHDX tarda unos minutos. El tiempo de conversión depende del tamaño del disco de origen. 

<!--
1. Run PowerShell on your Windows client.
1. Run the following command:

    ```powershell
    Convert-VHD -Path <source VHD path> -DestinationPath <destination-path.vhd> -VHDType Fixed 
    ```
-->
Utilizará este VHD fijo para todos los pasos posteriores de este artículo.


## <a name="create-a-hyper-v-vm-from-fixed-vhd"></a>Creación de una máquina virtual de Hyper-V a partir de un VHD fijo

1. En el **Administrador de Hyper-V**, en el panel de ámbito, haga clic con el botón derecho en el nodo del sistema para abrir el menú contextual y seleccione luego **Nuevo** > **Máquina virtual**.

    ![Selección de una nueva máquina virtual en el panel de ámbito](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-1.png)

1. En la página **Antes de comenzar** del Asistente para nueva máquina virtual, seleccione **Siguiente**.

1. En la página **Especificar nombre y ubicación**, escriba un **nombre** y una **ubicación** para la máquina virtual. Seleccione **Next** (Siguiente).

    ![Especificación del nombre y de la ubicación de la máquina virtual](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-2.png)

1. En la página **Especificar generación**, elija **Generación 1** como tipo de imagen del dispositivo .vhd y haga clic en **Siguiente**.    

    ![Especificación de la generación](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-3.png)

1. Asigne las configuraciones de red y memoria que desee.

1. En la página **Conectar disco duro virtual**, elija **Usar un disco duro virtual existente**, especifique la ubicación del VHD fijo de Windows anteriormente creado y haga clic en **Siguiente**.

    ![Página Conectar disco duro virtual](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-4.png)

1. Revise la sección **Resumen** y seleccione luego **Finalizar** para crear la máquina virtual.

La creación de la máquina virtual tarda varios minutos.
     

## <a name="connect-to-the-hyper-v-vm"></a>Conexión a la máquina virtual de Hyper-V

La máquina virtual se muestra en la lista de máquinas virtuales del sistema cliente. 


1. Seleccione la máquina virtual, haga clic con el botón derecho y seleccione **Iniciar**. 

    ![Selección e inicio de la máquina virtual](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/connect-virtual-machine-2.png)

2. La máquina virtual aparece como **Ejecutando**. Seleccione la máquina virtual, haga clic con el botón derecho y seleccione **Conectar**.

    ![Conexión con una máquina virtual](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/connect-virtual-machine-4.png)

Una vez conectado a la máquina virtual, complete el Asistente para la configuración del equipo y, a continuación, inicie sesión en la máquina virtual.


## <a name="generalize-the-vhd"></a>Generalización del disco duro virtual  

Use la utilidad *sysprep* para generalizar el VHD. 

1. Dentro de la máquina virtual, abra un símbolo del sistema.
1. Ejecute el siguiente comando para generalizar el VHD. 

    ```
    c:\windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm
    ```
    Para más información, consulte [Introducción a sysprep (preparación del sistema)](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).
1.  Una vez completado el comando, la máquina virtual se apagará. **No reinicie la VM**.

## <a name="upload-the-vhd-to-azure-blob-storage"></a>Carga del VHD en Azure Blob Storage

El VHD se puede usar ahora para crear una imagen generalizada en Azure Stack Edge. 

1. Cargue el VHD en Azure Blob Storage. Consulte las instrucciones detalladas en el artículo sobre [carga de un VHD mediante el Explorador de Azure Storage](../devtest-labs/devtest-lab-upload-vhd-using-storage-explorer.md).
1. Una vez completada la carga, puede utilizar la imagen cargada para crear imágenes de máquina virtual y máquinas virtuales. 

<!-- this should be added to deploy VM articles - If you experience any issues creating VMs from your new image, you can use VM console access to help troubleshoot. For information on console access, see [link].-->



## <a name="next-steps"></a>Pasos siguientes

En función de la naturaleza de la implementación, puede elegir uno de los procedimientos siguientes.

- [Implementación de máquinas virtuales a partir de una imagen generalizada mediante Azure Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md)
- [Implementación de máquinas virtuales a partir de una imagen generalizada mediante Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md)  
