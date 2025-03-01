---
title: Configuración de un entorno de desarrollo de Windows
description: Instale las herramientas, el SDK y el motor en tiempo de ejecución y cree un clúster de desarrollo local. Después de completar esta instalación, estará listo para compilar aplicaciones en Windows.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: sfrev, devx-track-azurepowershell
ms.openlocfilehash: 4568b791db07eaa7a513c42066b22df04b24cb6d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103010310"
---
# <a name="prepare-your-development-environment-on-windows"></a>Preparación del entorno de desarrollo en Windows

> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [Mac OS X](service-fabric-get-started-mac.md)
>
>

Para compilar y ejecutar [aplicaciones de Azure Service Fabric][1] en la máquina de desarrollo Windows, debe instalar el entorno en tiempo de ejecución de Service Fabric, el SDK y las herramientas. También es preciso que [habilite la ejecución de los scripts de Windows PowerShell](#enable-powershell-script-execution) que se incluyen en el SDK.

## <a name="prerequisites"></a>Requisitos previos

### <a name="supported-operating-system-versions"></a>Versiones de sistemas operativos admitidos

Se admiten las siguientes versiones de sistemas operativos para desarrollo:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Compatibilidad con Windows 7:
> - Windows 7 solo incluye de forma predeterminada Windows PowerShell 2.0. Los cmdlets de PowerShell de Service Fabric requieren PowerShell 3.0 o superior. Puede [descargar Windows PowerShell 5.1][powershell5-download] desde el Centro de descarga de Microsoft.
> - El proxy inverso de Service Fabric no está disponible en Windows 7.

## <a name="install-the-sdk-and-tools"></a>Instalación de SDK y herramientas

El Instalador de plataforma Web (WebPI) es la manera recomendada para instalar el SDK y las herramientas. Si recibe errores en tiempo de ejecución utilizando WebPI, también puede encontrar vínculos directos a los instaladores en las notas de la versión para una versión específica de Service Fabric. Las notas de la versión se pueden encontrar en los anuncios de lanzamiento en el [blog del equipo de Service Fabric](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric).

> [!NOTE]
> No se admiten actualizaciones locales del clúster de desarrollo de Service Fabric.

### <a name="to-use-visual-studio-2017-or-2019"></a>Cómo usar Visual Studio 2017 o 2019

Las herramientas de Service Fabric forman parte de la carga de trabajo de Azure Development de Visual Studio 2017 y 2019. Habilite esta carga de trabajo durante la instalación de Visual Studio.
Además, debe instalar el SDK de Microsoft Azure Service Fabric y el entorno en tiempo de ejecución mediante el Instalador de plataforma web.

* [Instale el SDK de Microsoft Azure Service Fabric][core-sdk].

### <a name="sdk-installation-only"></a>Para instalar solamente el SDK

Si únicamente necesita el SDK, puede instalar este paquete:

* [Instale el SDK de Microsoft Azure Service Fabric][core-sdk].

Las versiones actuales son:

* SDK y herramientas de Service Fabric 4.2.477
* Runtime de Service Fabric 7.2.477

Para obtener una lista de las versiones admitidas, consulte [Versiones de Service Fabric](service-fabric-versions.md).

> [!NOTE]
> Los clústeres de máquina única (OneBox) no se admiten para las actualizaciones de aplicación o de clúster; elimine el clúster OneBox y vuelva a crearlo si necesita realizar una actualización de clúster, o tiene algún problema al realizar una actualización de la aplicación. 

## <a name="enable-powershell-script-execution"></a>Habilitar la ejecución del script de PowerShell

Service Fabric usa scripts de Windows PowerShell para crear un clúster de desarrollo local e implementar aplicaciones desde Visual Studio. De forma predeterminada, Windows bloquea la ejecución de estos scripts. Para habilitarlos, debe modificar la directiva de ejecución de PowerShell. Abra PowerShell como administrador y escriba el siguiente comando:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="install-docker-optional"></a>Instalar Docker (opcional)

[Service Fabric es un orquestador de contenedores](service-fabric-containers-overview.md) que implementa microservicios en un clúster de máquinas. Para ejecutar aplicaciones contenedoras de Windows en el clúster de desarrollo local, primero debe instalar Docker para Windows. Obtener [Docker CE para Windows (estable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Después de instalar e iniciar Docker, haga clic con el botón derecho en el icono de la bandeja y seleccione **Switch to Windows containers** (Conmutar a contenedores de Windows), Este paso es necesario para ejecutar imágenes de Docker basadas en Windows.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha terminado la configuración del entorno de desarrollo, puede empezar a compilar y ejecutar aplicaciones.

* [Aprenda a crear, implementar y administrar aplicaciones](service-fabric-tutorial-create-dotnet-app.md)
* [Más información sobre los modelos de programación: Reliable Services y Reliable Actors](service-fabric-choose-framework.md)
* [Consulta de los ejemplos de código de Service Fabric en GitHub](/samples/browse/?products=azure)
* [Visualización del clúster mediante el Explorador de Service Fabric](service-fabric-visualizing-your-cluster.md)
* [Preparación del entorno de desarrollo Linux en Windows](service-fabric-local-linux-cluster-windows.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)

[1]: https://azure.microsoft.com/campaigns/service-fabric/ "Página de campaña de Service Fabric"
[2]: https://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Vínculo de WebPI de VS 2015"
[full-bundle-dev15]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Vínculo de WebPI de Dev15"
[core-sdk]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Vínculo de WebPI de SDK de núcleo"
[powershell5-download]:https://www.microsoft.com/download/details.aspx?id=54616
