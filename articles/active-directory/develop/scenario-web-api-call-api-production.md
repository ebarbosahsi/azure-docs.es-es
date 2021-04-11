---
title: Mover llamada de API web y API web a producción | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a pasar a producción una API web que llama a las API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0ab5baef925b7c8589dd7852b6ff8058d67ba745
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104675879"
---
# <a name="a-web-api-that-calls-web-apis-move-to-production"></a>API web que llama a API web: Paso a producción

Una vez que haya adquirido un token para llamar a las API web, aquí hay algunas cosas que debe considerar al mover su aplicación a producción.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce los conceptos básicos de cómo llamar a las API web desde su propia API web, puede que esté interesado en el siguiente tutorial, que describe el código que se usa para compilar una API web protegida que llama a otras API web.

| Muestra | Plataforma | Descripción |
|--------|----------|-------------|
| Capítulo 1 de [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | API web de ASP.NET Core, escritorio (WPF) | La API web de ASP.NET Core llama a Microsoft Graph, a donde llama desde una aplicación de WPF con la Plataforma de identidad de Microsoft. |
