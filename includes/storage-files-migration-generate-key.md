---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: a142de62f1ab7046d47c29753863b16864d97536
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075626"
---
Las claves de cifrado de datos del servicio se usan para cifrar datos confidenciales de los clientes, como las credenciales de la cuenta de almacenamiento, que se envían desde el servicio StorSimple Manager al dispositivo de StorSimple. Necesitará cambiar estas claves periódicamente si su organización de TI tiene una directiva de rotación de claves en los dispositivos de almacenamiento. El proceso de cambio de clave puede ser ligeramente diferente dependiendo de si el servicio StorSimple Manager administra uno o varios dispositivos. Para más información, vaya a [StorSimple security and data protection](../articles/storsimple/storsimple-8000-security.md) (Protección de datos y seguridad de StorSimple).

El cambio de la clave de cifrado de datos del servicio se realiza en 3 pasos:

1. Mediante scripts de Windows PowerShell para Azure Resource Manager, autorice a un dispositivo cambiar la clave de cifrado de datos del servicio.
2. En Windows PowerShell para StorSimple, inicie el cambio de claves de cifrado de datos del servicio.
3. Si tiene más de un dispositivo de StorSimple, actualice la clave de cifrado de datos del servicio en los demás dispositivos.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Paso 1: Usar un script de Windows PowerShell para autorizar a un dispositivo a cambiar la clave de cifrado de datos del servicio
Normalmente, el administrador de dispositivos solicitará que el administrador de servicios autorice que un dispositivo cambie las claves de cifrado de datos del servicio. A continuación, el administrador de servicios autorizará que el dispositivo cambie la clave.

Este paso se realiza con el script basado en Azure Resource Manager. El administrador de servicios puede seleccionar un dispositivo que sea apto para la autorización. A continuación, se autoriza que el dispositivo inicie el proceso de cambio de las claves de cifrado de datos del servicio. 

Para más información sobre el uso del script, vaya a [Authorize-ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>¿Qué dispositivos se pueden autorizar para que cambien las claves de cifrado de datos del servicio?
Para poder autorizar que un dispositivo inicie los cambios de las claves de cifrado de datos del servicio debe cumplir los siguientes criterios:

* Para que sea válido para la autorización de cambio de claves de cifrado de datos del servicio, el dispositivo debe estar en línea.
* Si el cambio de claves no se ha iniciado, es posible autorizar el mismo dispositivo 30 minutos después.
* Se puede autorizar otro dispositivo, siempre que el dispositivo autorizado anteriormente no haya iniciado el cambio de claves. Una vez que se haya autorizado el nuevo dispositivo, el anterior no puede iniciar el cambio.
* No se puede autorizar un dispositivo mientras la sustitución de la clave de cifrado de datos del servicio esté en curso.
* Se puede autorizar un dispositivo cuando algunos de los dispositivos registrados en el servicio hayan sustituido el cifrado, mientras que otros no lo hayan hecho. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Paso 2: Usar Windows PowerShell para StorSimple a fin de iniciar el cambio de claves de cifrado de datos del servicio
Este paso se realiza en la interfaz de Windows PowerShell para StorSimple del dispositivo de StorSimple autorizado.

> [!NOTE]
> Hasta que se complete la sustitución de claves no se pueden realizar operaciones en Azure Portal del servicio StorSimple Manager.


Si utiliza la consola serie del dispositivo para conectarse a la interfaz de Windows PowerShell, realice los pasos siguientes.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Para iniciar el cambio de claves de cifrado de datos del servicio
1. Seleccione la opción 1 para iniciar sesión con acceso total.
2. En el símbolo del sistema, escriba:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Una vez que se haya completado correctamente el cmdlet, obtendrá una nueva clave de cifrado de datos del servicio. Copie y guarde esta clave para usarla en el paso 3 de este proceso. Esta clave se utilizará para actualizar todos los dispositivos restantes registrados en el servicio StorSimple Manager.
   
   > [!NOTE]
   > Este proceso debe iniciarse en las cuatro horas siguientes a la autorización de un dispositivo de StorSimple.
   > 
   > 
   
   A continuación, esta nueva clave se envía al servicio de inserción en todos los dispositivos que están registrados en el servicio. Seguidamente, aparecerá una alerta en el panel del servicio. El servicio deshabilitará todas las operaciones en los dispositivos registrados y, a continuación, el administrador de dispositivos tendrá que actualizar la clave de cifrado de datos del servicio en el resto de dispositivos. Sin embargo, las E/S (hosts que envían datos a la nube) no se interrumpirán.
   
   Si tiene un único dispositivo registrado en el servicio, el proceso de sustitución habrá finalizado y puede omitir el paso siguiente. Si tiene varios dispositivos registrados en el servicio, vaya al paso 3.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Paso 3: Actualizar la clave de cifrado de datos del servicio en otros dispositivos de StorSimple
Estos pasos deben realizarse en la interfaz de Windows PowerShell del dispositivo de StorSimple si tiene varios dispositivos registrados en el servicio StorSimple Manager. La clave que obtuvo en el paso 2 se debe usar para actualizar todos los dispositivos de StorSimple restantes registrados con el servicio StorSimple Manager.

Realice los pasos siguientes para actualizar el cifrado de datos del servicio en el dispositivo.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>Para actualizar la clave de cifrado de datos del servicio en dispositivos físicos
1. Use Windows PowerShell para StorSimple para conectarse a la consola. Seleccione la opción 1 para iniciar sesión con acceso total.
2. En el símbolo del sistema, escriba: `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Proporcione la clave de cifrado de datos del servicio que obtuvo en [Paso 2: Usar Windows PowerShell para StorSimple a fin de iniciar el cambio de claves de cifrado de datos del servicio](#to-initiate-the-service-data-encryption-key-change).

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>Para actualizar la clave de cifrado de datos del servicio en todas las aplicaciones en la nube 8010/8020
1. Descargue y configure el script de PowerShell [Update-CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1). 
2. Abra PowerShell y, en el símbolo del sistema, escriba:`Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Este script asegurará de que esa clave de cifrado de datos del servicio se establece en todas las aplicaciones en la nube 8010/8020 en el administrador de dispositivos.