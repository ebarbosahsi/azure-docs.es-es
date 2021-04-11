---
title: 'Máquinas virtuales con Windows 7 en Windows Virtual Desktop (clásico): Azure'
description: Cómo resolver incidencias en máquinas virtuales con Windows 7 en un entorno de Windows Virtual Desktop (clásico).
author: Heidilohr
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 99c1c76092714c58fd13d9ce51ef850971d80c86
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2021
ms.locfileid: "106444129"
---
# <a name="troubleshoot-windows-7-virtual-machines-in-windows-virtual-desktop-classic"></a>Solución de problemas de las máquinas virtuales con Windows 7 en Windows Virtual Desktop (clásico)

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop (clásico), que no admite objetos de Windows Virtual Desktop para Azure Resource Manager.

Use este artículo para solucionar los problemas que tiene al configurar las máquinas de virtuales (VM) de host de sesión de Windows Virtual Desktop.

## <a name="known-issues"></a>Problemas conocidos

Windows 7 en instancias de Windows Virtual Desktop no admite las siguientes características:

- Aplicaciones virtualizadas (RemoteApps)
- Redirección de zona horaria
- Escalado automático de PPP

Windows Virtual Desktop solo puede virtualizar escritorios completos para Windows 7.

Aunque no se admite el escalado automático de PPP, puede cambiar manualmente la resolución de la máquina virtual; para ello, haga clic con el botón derecho en el icono del cliente de Escritorio remoto y seleccione **Resolución**.

## <a name="error-cant-access-the-remote-desktop-user-group"></a>Error: No se puede acceder al grupo Usuario de Escritorio remoto

Si Windows Virtual Desktop no puede encontrarle o no puede encontrar las credenciales de los usuarios en el grupo Usuario de Escritorio remoto, puede que vea uno de los siguientes mensajes de error:

- "This user is not a member of the Remote Desktop User group" (Este usuario no es miembro del grupo Usuario de Escritorio remoto)
- "You must be granted permissions to sign in through Remote Desktop Services" (Se le deben conceder permisos para iniciar sesión a través de Servicios de Escritorio remoto)

Para corregir este error, agregue el usuario al grupo Usuario de Escritorio remoto:

1. Abra Azure Portal.
2. Seleccione la máquina virtual en la que vio el mensaje de error.
3. Seleccione **Ejecutar un comando**.
4. Ejecute el siguiente comando, pero reemplace `<username>` por el nombre del usuario que desea agregar:

   ```cmd
   net localgroup "Remote Desktop Users" <username> /add
   ```