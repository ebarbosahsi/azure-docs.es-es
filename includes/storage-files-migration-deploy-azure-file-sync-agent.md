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
ms.openlocfilehash: 7c1ea3fbf8e5cef4b1e8f8a2b0963fdd66584f5a
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075607"
---
En esta sección se instala el agente de Azure File Sync en una instancia de Windows Server.

En la [guía de implementación](../articles/storage/files/storage-sync-files-deployment-guide.md) se indica que es preciso desactivar la **configuración de seguridad mejorada de Internet Explorer**. Esta medida de seguridad no se puede aplicar con Azure File Sync. Si la desactiva, puede autenticarse en Azure sin problemas.

Abra PowerShell e instale los módulos necesarios mediante los siguientes comandos. Asegúrese de instalar el módulo completo y el proveedor de NuGet cuando se le solicite.

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Si tiene problemas para conectarse a Internet desde el servidor, ahora es el momento de solucionarlos. Azure File Sync usa cualquier conexión de red disponible a Internet. También se admite la exigencia de un servidor proxy para tener acceso a Internet. Ya puede configurar un proxy en toda la máquina, o bien especificar un proxy que solo va a usar Azure File Sync durante la instalación del agente.

Si para configurar un proxy debe abrir los firewalls de este servidor, es posible que ese enfoque le resulte aceptable. Al final de la instalación del servidor, después de haber completado el registro del servidor, un informe de conectividad de red le mostrará las direcciones URL exactas de los puntos de conexión en Azure, con las que Azure File Sync necesita comunicarse en la región que ha seleccionado. El informe también indica el motivo por el que se necesita la comunicación. Puede usar el informe para bloquear los firewalls de este servidor en direcciones URL específicas.

También puede seguir un enfoque más conservador y no abrir totalmente los firewalls, sino limitar el servidor para que se comunique con espacios de nombres DNS de nivel superior. Para más información, consulte [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md). Siga sus propios procedimientos recomendados de redes.

Al final del Asistente para la instalación del servidor, se abrirá un Asistente para el registro del servidor. Registre el servidor en el recurso de Azure del servicio de sincronización de almacenamiento anterior.

Estos pasos se describen con más detalle en la guía de implementación, que incluye los módulos de PowerShell que se deben instalar primero: [Instalación de agente de Azure File Sync](../articles/storage/files/storage-sync-files-deployment-guide.md).

Use el agente más reciente. Puede descargarlo del Centro de descarga de Microsoft: [Agente de Azure File Sync](https://aka.ms/AFS/agent "Descarga del agente de Azure File Sync").

Después de una instalación y un registro del servidor correctos, puede comprobar que ha completado correctamente este paso. Vaya al recurso de Storage Sync Service en Azure Portal. En el menú de la izquierda, vaya a **Servidores registrados**. Verá que el servidor aparece en esa lista.
