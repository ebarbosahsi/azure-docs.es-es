---
title: Introducción a Azure Application Insights Agent | Microsoft Docs
description: Guía de inicio rápido para Application Insights Agent. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 01/22/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 21184e1623fd47e8367d4c5dfbc2c85debe93124
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100587386"
---
# <a name="get-started-with-azure-monitor-application-insights-agent-for-on-premises-servers"></a>Introducción a Azure Application Insights Agent para servidores locales

Este artículo contiene los comandos de inicio rápido que se espera que funcionen para la mayoría de los entornos.
Las instrucciones dependen de la Galería de PowerShell para distribuir las actualizaciones.
Estos comandos admiten el parámetro de PowerShell `-Proxy`.

Para obtener una explicación de estos comandos, las instrucciones de personalización e información sobre cómo solucionar problemas, consulte las [instrucciones detalladas](status-monitor-v2-detailed-instructions.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="download-and-install-via-powershell-gallery"></a>Descarga e instalación a través de la Galería de PowerShell

### <a name="install-prerequisites"></a>Requisitos previos de instalación

> [!NOTE]
> Desde abril de 2020, TLS 1.1 y 1.0 están en desuso para Galería de PowerShell.
>
> Para ver los requisitos previos adicionales que podría necesitar, consulte [Compatibilidad de TLS con Galería de PowerShell](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support).
>

Ejecute PowerShell como administrador.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
Cierre PowerShell.

### <a name="install-application-insights-agent"></a>Instalación de Application Insights Agent
Ejecute PowerShell como administrador.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>Habilitar supervisión
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -ConnectionString xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>Descarga e instalación manual (opción sin conexión)
### <a name="download-the-module"></a>Descara del módulo
Descargue manualmente la versión más reciente del módulo desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).

### <a name="unzip-and-install-application-insights-agent"></a>Descompresión e instalación de Application Insights Agent
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>Habilitar supervisión
```powershell
Enable-ApplicationInsightsMonitoring -ConnectionString xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```



## <a name="next-steps"></a>Pasos siguientes

 Vea la telemetría:

- [Explore las métricas](../essentials/metrics-charts.md) para supervisar el rendimiento y el uso.
- [Busque en los eventos y los registros](./diagnostic-search.md) para diagnosticar problemas.
- [Use Analytics](../logs/log-query-overview.md) para consultas más avanzadas.
- [Cree paneles](./overview-dashboard.md).

 Agregue más telemetría:

- [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](./javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](./asp-net.md) para que pueda insertar llamadas de seguimiento y registro.

Haga mucho más con Application Insights Agent:

- Revise las [instrucciones detalladas](status-monitor-v2-detailed-instructions.md) para obtener una explicación de los comandos que se encuentran aquí.
- Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) de Application Insights Agent.

