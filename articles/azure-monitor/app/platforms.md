---
title: 'Application Insights: lenguajes, plataformas e integraciones| Microsoft Docs'
description: Lenguajes, plataformas e integraciones disponibles para Application Insights
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: 399e57377a779622aa3073dfd3313cee1db345f8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100583858"
---
# <a name="supported-languages"></a>Idiomas compatibles

* [C#|VB (.NET)](./asp-net.md)
* [Java](./java-get-started.md)
* [JavaScript](./javascript.md)
* [Node.JS](./nodejs.md)
* [Python](./opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Plataformas y marcos compatibles

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentación de aplicaciones ya implementadas (sin código y basadas en agentes)
* [Conjuntos de escalado de máquinas virtuales de Azure y Azure VM](./azure-vm-vmss-apps.md)
* [Azure App Service](./azure-web-apps.md)
* [ASP.NET: para aplicaciones que ya están activas](./monitor-performance-live-website-now.md)
* [Azure Cloud Services](./cloudservices.md), incluidos los roles web y de trabajo
* [Funciones de Azure](../../azure-functions/functions-monitoring.md)
### <a name="instrumentation-through-code-sdks"></a>Instrumentación a través del código (SDK)
* [ASP.NET](./asp-net.md)
* [ASP.NET Core](./asp-net-core.md)
* [Android](../app/mobile-center-quickstart.md) (App Center)
* [iOS](../app/mobile-center-quickstart.md) (App Center)
* [Java EE](./java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](./opencensus-python.md)
* [Aplicación Windows universal](../app/mobile-center-quickstart.md) (App Center)
* [Aplicaciones, servicios y roles de trabajo del escritorio de Windows](./windows-desktop.md)
* [React](./javascript-react-plugin.md)
* [React Native](./javascript-react-native-plugin.md)

## <a name="logging-frameworks"></a>Marcos de registro
* [ILogger](./ilogger.md)
* [Log4Net, NLog o System.Diagnostics.Trace](./asp-net-trace-logs.md)
* [Java, Log4J o Logback](./java-trace-logs.md)
* [Complemento LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](/archive/blogs/msoms/application-insights-connector-in-oms)

## <a name="export-and-data-analysis"></a>Exportación y análisis de datos
* [Power BI](https://powerbi.microsoft.com/blog/explore-your-application-insights-data-with-power-bi/)
* [Stream Analytics](./export-power-bi.md)

## <a name="unsupported-sdks"></a>SDK no compatibles
Somos conscientes de que existen otros SDK con soporte de la comunidad. Sin embargo, Azure Monitor solo proporciona compatibilidad cuando se usan los SDK admitidos que se enumeran en esta página. Estamos evaluando constantemente oportunidades para ampliar nuestra compatibilidad con otros lenguajes, por lo que debe seguir nuestra página de [anuncios de GitHub](https://github.com/microsoft/ApplicationInsights-Announcements/issues) para recibir las noticias más recientes sobre los SDK. 

