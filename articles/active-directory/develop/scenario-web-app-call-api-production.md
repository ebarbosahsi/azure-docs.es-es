---
title: Migrar a producción una aplicación web que llama a API web | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo migrar a producción una aplicación web que llama a API web.
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
ms.openlocfilehash: cf32274a49cb1b790e9d872efe36f2e1cb188d1d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104675947"
---
# <a name="a-web-app-that-calls-web-apis-move-to-production"></a>Una aplicación web que llama a API web: Paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, aquí tiene algunos aspectos que deben tenerse en cuenta al trasladar la aplicación a producción.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información, pruebe el tutorial completo y progresivo de aplicaciones web de ASP.NET Core. Este tutorial:

- Muestra cómo iniciar la sesión de los usuarios en varias audiencias o nubes nacionales, o con identidades sociales.
- Llama a Microsoft Graph.
- Llama a varias API de Microsoft.
- Controla el consentimiento incremental.
- Llama a sus propias API web.

[Tutorial de una aplicación web de ASP.NET Core](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial#scope-of-this-tutorial)

<!--- Removing this diagram as it's already shown from the next step linked tutorial

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
