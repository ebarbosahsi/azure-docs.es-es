---
title: Visualizaciones para Application Change Analysis - Azure Monitor
description: Obtenga información sobre cómo usar las visualizaciones en Application Change Analysis en Azure Monitor.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 02/11/2021
ms.openlocfilehash: 8319885de26bf79f5e402c4d06b29e9dd94894de
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104655858"
---
# <a name="visualizations-for-application-change-analysis-preview"></a>Visualizaciones para Application Change Analysis (versión preliminar)

## <a name="standalone-ui"></a>UI independiente

En Azure Monitor, hay un panel independiente para que Change Analysis vea todos los cambios con información sobre los recursos de información y dependencias de aplicaciones.

Busque Change Analysis en la barra de búsqueda de Azure Portal para iniciar la experiencia.

![Captura de pantalla de búsqueda de Change Analysis en Azure Portal](./media/change-analysis/search-change-analysis.png)

Todos los recursos de una suscripción seleccionada se muestran con los cambios de las últimas 24 horas. Todos los cambios se muestran con el valor antiguo y el nuevo para proporcionar conclusiones a simple vista.

![Captura de pantalla de la hoja Change Analysis en Azure Portal](./media/change-analysis/change-analysis-standalone-blade.png)

Hacer clic en un cambio para ver el fragmento de código completo de Resource Manager y otras propiedades.

![Captura de pantalla de los detalles del cambio](./media/change-analysis/change-details.png)

Si tiene algún comentario, use el botón Enviar comentarios o envíe un correo electrónico a changeanalysisteam@microsoft.com.

![Captura de pantalla del botón de comentarios de la pestaña Change Analysis](./media/change-analysis/change-analysis-feedback.png)

### <a name="multiple-subscription-support"></a>Compatibilidad con varias suscripciones

La UI permite seleccionar varias suscripciones para ver los cambios de los recursos. Use el filtro de suscripción:

![Captura de pantalla del filtro de suscripción que admite la selección de varias suscripciones](./media/change-analysis/multiple-subscriptions-support.png)


## <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Application Change Analysis en la herramienta Diagnosticar y solucionar problemas

Application Change Analysis es un detector independiente en las herramientas de diagnóstico y solución de problemas de una aplicación web. También se agrega en **Application Crashes** (Bloqueos de la aplicación) y **Web App Down detectors** (Detectores de aplicación web fuera de servicio). Cuando entra en la herramienta Diagnosticar y solucionar problemas, el proveedor de recursos **Microsoft.ChangeAnalysis** se registrará automáticamente. Siga estas instrucciones para habilitar el seguimiento de cambios en el invitado de la aplicación web.

1. Seleccione **Availability and Performance** (Disponibilidad y rendimiento).

    ![Captura de pantalla de opción de solución de problemas "Disponibilidad y rendimiento"](./media/change-analysis/availability-and-performance.png)

2. Seleccione **Application Changes** (Cambios de la aplicación). Esta característica también está disponible en **Application Crashes** (Bloqueos de la aplicación).

   ![Captura de pantalla del botón "Application Crashes"](./media/change-analysis/application-changes.png)

3. El vínculo remite a la interfaz de usuario de Application Change Analysis en el ámbito de la aplicación web. Si el seguimiento de cambios en el invitado de la aplicación web no está habilitado, siga el banner para obtener los cambios de configuración de los archivos y aplicaciones.

   ![Captura de las opciones de "Application Crashes"](./media/change-analysis/enable-changeanalysis.png)

4. Active **Change Analysis** y seleccione **Guardar**. La herramienta muestra todas las aplicaciones web en un plan de App Service. Puede usar el modificador nivel de plan para activar Application Change Analysis para todas las aplicaciones web de un plan.

    ![Captura de pantalla de la interfaz de usuario "Habilitar Change Analysis"](./media/change-analysis/change-analysis-on.png)

5. Los datos modificados también están disponibles en ciertos detectores de **Web App Down** (Aplicación web fuera de servicio) y **Application Crashes** (Bloqueo de aplicaciones). Verá un gráfico con un resumen del tipo de cambios a lo largo del tiempo, junto con detalles sobre estos cambios: De forma predeterminada, se muestran los cambios en las últimas 24 horas para ayudar a solucionar problemas inmediatos.

     ![Captura de pantalla de la vista de diferencias de cambios](./media/change-analysis/change-view.png)

## <a name="diagnose-and-solve-problems-tool"></a>Herramienta Diagnosticar y solucionar problemas
Change Analysis está disponible como una tarjeta de información en la herramienta Diagnosticar y solucionar problemas. Si un recurso experimenta problemas y se han detectado cambios en las últimas 72 horas, la tarjeta de información mostrará el número de cambios. Al hacer clic en el vínculo de visualización de detalles de cambios, se mostrará la vista filtrada de la interfaz de usuario independiente de Change Analysis.

![Captura de pantalla de visualización de la información de cambios en la herramienta Diagnosticar y solucionar problemas.](./media/change-analysis/change-insight-diagnose-and-solve.png)



## <a name="virtual-machine-diagnose-and-solve-problems"></a>Diagnóstico y solución de problemas de una máquina virtual

Vaya a la herramienta Diagnosticar y solucionar problemas para una máquina virtual.  Vaya a **Herramientas para la solución de problemas**, navegue hacia abajo de la página y seleccione **Analizar cambios recientes** para ver los cambios en la máquina virtual.

![Captura de pantalla de Diagnosticar y solucionar problemas de la máquina virtual](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Analizador de cambios en herramientas para la solución de problemas](./media/change-analysis/analyze-recent-changes.png)

## <a name="activity-log-change-history"></a>Historial de cambios en el registro de actividad

La característica [View change history](../essentials/activity-log.md#view-change-history) (Ver el historial de cambios) en el registro de actividad llama al back-end del servicio Application Change Analysis para obtener los cambios asociados a una operación. **Historial de cambios** antes llamaba directamente a [Azure Resource Graph](../../governance/resource-graph/overview.md), pero se cambió el back-end para llamar a Application Change Analysis, de modo que los cambios devueltos incluirán los cambios de nivel de recursos de [Azure Resource Graph](../../governance/resource-graph/overview.md), las propiedades de los recursos de [Azure Resource Manager](../../azure-resource-manager/management/overview.md) y los cambios en el invitado de los servicios de PaaS, como la aplicación web App Services. Para que el servicio Application Change Analysis pueda examinar en busca de cambios en las suscripciones de los usuarios, es necesario registrar un proveedor de recursos. La primera vez que acceda a la pestaña **Historial de cambios**, la herramienta comenzará automáticamente a registrar al proveedor de recursos **Microsoft.ChangeAnalysis**. Tras su registro, los cambios de **Azure Resource Graph** estarán disponibles inmediatamente y cubrirán los últimos 14 días. Los cambios de otros orígenes estarán disponibles después de aproximadamente 4 horas a partir de la incorporación de la suscripción.

![Integración del historial de cambios en el registro de actividad](./media/change-analysis/activity-log-change-history.png)

## <a name="vm-insights-integration"></a>Integración de Información de máquinas virtuales

Los usuarios que tienen habilitada [Información de máquinas virtuales](../vm/vminsights-overview.md) pueden ver lo que ha cambiado en sus máquinas virtuales que podría haber provocado picos en un gráfico de métricas, como CPU o memoria. Los datos modificados se integran en la barra de navegación lateral de Información de máquinas virtuales. El usuario puede ver si se han producido cambios en la VM y seleccionar **Investigate Changes** (Investigar cambios) para ver los detalles de los cambios en la interfaz de usuario independiente de Application Change Analysis.

[![Integración de Información de máquinas virtuales](./media/change-analysis/vm-insights.png)](./media/change-analysis/vm-insights.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [solucionar problemas de Change Analysis](change-analysis-troubleshoot.md).
