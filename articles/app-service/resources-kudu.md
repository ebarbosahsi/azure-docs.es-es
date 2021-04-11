---
title: Introducción al servicio Kudu
description: Obtenga información sobre el motor que impulsa la implementación continua en App Service y sus características.
ms.date: 03/17/2021
ms.topic: reference
ms.openlocfilehash: ad1456f1ca123127f3d84aa8195c99fc34872aee
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608742"
---
# <a name="kudu-service-overview"></a>Introducción al servicio Kudu

Kudu es el motor que subyace a una serie de características de [Azure App Service](overview.md) relacionadas con la implementación basada en el control de código fuente y otros métodos de implementación como Dropbox y la sincronización de OneDrive. 

## <a name="access-kudu-for-your-app"></a>Acceso a Kudu para la aplicación
Cada vez que cree una aplicación, App Service creará una aplicación complementaria que está protegida por HTTPS. Se puede acceder a esta aplicación de Kudu en:

- Si la aplicación no está en el nivel aislado: `https://<app-name>.scm.azurewebsites.net`
- Si la aplicación está en el nivel aislado (App Service Environment): `https://<app-name>.scm.<ase-name>.p.azurewebsites.net`

Para más información, consulte [Acceso al servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).

## <a name="kudu-features"></a>Características de Kudu

Kudu proporciona información útil sobre la aplicación de App Service, como:

- Configuración de la aplicación
- Cadenas de conexión
- Variables de entorno
- Variables de servidor
- Encabezados HTTP

También proporciona otras características, como:

- Ejecución de comandos en la [consola de Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console).
- Descarga de volcados de diagnóstico de IIS o registros de Docker.
- Administración de los procesos de IIS y las extensiones del sitio.
- Adición de webhooks de implementación para aplicaciones Windows.
- Permitir la interfaz de usuario de implementación mediante ZIP con `/ZipDeploy`.
- Generación de [scripts de implementación personalizados](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
- Permitir el acceso con la [API REST](https://github.com/projectkudu/kudu/wiki/REST-API).

## <a name="more-resources"></a>Más recursos

Kudu es un [proyecto de código abierto](https://github.com/projectkudu/kudu) y tiene su documentación en la [Wiki de Kudu](https://github.com/projectkudu/kudu/wiki).

