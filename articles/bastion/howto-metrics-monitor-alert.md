---
title: Configuración de la supervisión y las métricas con Azure Monitor
titleSuffix: Azure Bastion
description: Conozca la supervisión de Azure Bastion y las métricas mediante el uso de Azure Monitor, la solución para métricas, alertas y registros de diagnóstico en Azure.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: how-to
ms.date: 03/12/2021
ms.author: mialdrid
ms.openlocfilehash: 3a5beba3938b5a845a378ede155f2f64e6baac7a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103417950"
---
# <a name="how-to-configure-monitoring-and-metrics-for-azure-bastion-using-azure-monitor"></a>Configuración de la supervisión y las métricas para Azure Bastion mediante Azure Monitor

Este artículo le ayuda a trabajar con la supervisión y las métricas de Azure Bastion mediante Azure Monitor.

>[!NOTE]
>No se recomienda el uso de **métricas clásicas**.
>

## <a name="about-metrics"></a>Acerca de las métricas

Azure Bastion tiene varias métricas que están disponibles. En la tabla siguiente se muestran la categoría y las dimensiones de cada métrica disponible.

|**Métrica**|**Categoría**|**Dimensiones**|
| --- | --- | --- |
|Estado de comunicación de Bastión**|[Disponibilidad](#availability)|N/D|
|Memoria total|[Disponibilidad](#availability)|Instancia|
|CPU usada|[Tráfico](#traffic)|Instancia
|Memoria usada|[Tráfico](#traffic)|Instancia
|Número de sesiones|[Rendimiento](#performance)|Instancia|

**El estado de comunicación de Bastión solo se aplica a los hosts bastión implementados después de noviembre de 2020.

### <a name="availability-metrics"></a><a name="availability"></a>Métricas de disponibilidad

#### <a name="bastion-communication-status"></a><a name="communication-status"></a>Estado de comunicación de Bastion

Puede ver el estado de comunicación de Azure Bastion, agregado en todas las instancias que comprenden el host bastión.

* Un valor de **1** indica que el bastión está disponible.
* Un valor de **0** indica que el servicio bastión no está disponible.

:::image type="content" source="./media/metrics-monitor-alert/communication-status.png" alt-text="Captura de pantalla que muestra el estado de la comunicación":::

#### <a name="total-memory"></a><a name="total-memory"></a>Memoria total

Puede ver la memoria total de Azure Bastion, dividida en cada instancia de bastión.

:::image type="content" source="./media/metrics-monitor-alert/total-memory.png" alt-text="Captura de pantalla que muestra la memoria total":::

### <a name="traffic-metrics"></a><a name="traffic"></a>Métricas de tráfico

#### <a name="used-cpu"></a><a name="used-cpu"></a>CPU usada

Puede ver el uso de CPU de Azure Bastion, dividido en cada instancia de bastión. La supervisión de esta métrica le ayudará a medir la disponibilidad y la capacidad de las instancias que componen Azure Bastion.

:::image type="content" source="./media/metrics-monitor-alert/used-cpu.png" alt-text="Captura de pantalla que muestra la CPU usada.":::

#### <a name="used-memory"></a><a name="used-memory"></a>Memoria usada

Puede ver el uso de memoria en cada instancia del bastión dividida en cada instancia de bastión. La supervisión de esta métrica le ayudará a medir la disponibilidad y la capacidad de las instancias que componen Azure Bastion.

:::image type="content" source="./media/metrics-monitor-alert/used-memory.png" alt-text="Captura de pantalla que muestra la memoria usada":::

### <a name="performance-metrics"></a><a name="performance"></a>Métricas de rendimiento

#### <a name="session-count"></a>Número de sesiones

Puede ver el número de sesiones activas por instancia de bastión, agregadas en cada tipo de sesión (RDP y SSH). Cada instancia de Azure Bastion puede admitir una serie de sesiones de RDP y SSH activas. La supervisión de esta métrica le ayudará a comprender si necesita ajustar el número de instancias que ejecutan el servicio de bastión. Para más información sobre el número de sesiones que puede admitir Azure Bastion, consulte las [preguntas frecuentes sobre Azure Bastión](bastion-faq.md).

:::image type="content" source="./media/metrics-monitor-alert/session-count.png" alt-text="Captura de pantalla que muestra el recuento de sesiones":::

## <a name="how-to-view-metrics"></a><a name="metrics"></a>Visualización de métricas

1. Para ver las métricas, vaya al host bastión.
1. En la lista **Supervisión**, seleccione **Métricas**.
1. Seleccione los parámetros. Si no se ha establecido ninguna métrica, haga clic en **Agregar métrica** y, a continuación, seleccione los parámetros.

   * **Ámbito:** de forma predeterminada, el ámbito se establece en el host bastión.
   * **Espacio de nombres de métrica:** métricas estándar.
   * **Métrica:** seleccione la métrica que desea ver.

1. Una vez seleccionada una métrica, se aplicará la agregación predeterminada. Opcionalmente, puede aplicar la división, que mostrará la métrica con diferentes dimensiones.

## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
  
