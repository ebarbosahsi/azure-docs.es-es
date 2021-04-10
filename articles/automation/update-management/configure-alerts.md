---
title: Cómo crear alertas para Azure Automation Update Management
description: En este artículo se explica cómo configurar alertas de Azure para notificar el estado de las implementaciones o evaluaciones de las actualizaciones.
services: automation
ms.subservice: update-management
ms.date: 03/15/2021
ms.topic: conceptual
ms.openlocfilehash: 224a7b5457a099fd763ac657349fc5497824ab76
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104601430"
---
# <a name="how-to-create-alerts-for-update-management"></a>Cómo crear alertas para Update Management

Las alertas de Azure le notifican proactivamente los resultados de los trabajos de runbook, los problemas de estado del servicio u otros escenarios relacionados con su cuenta de Automation. Azure Automation no incluye las reglas de alerta preconfiguradas, pero puede crear las suyas propias en función de los datos que recopila. En este artículo se proporcionan instrucciones sobre cómo crear reglas de alertas mediante las métricas incluidas en Update Management.

## <a name="available-metrics"></a>Métricas disponibles

Azure Automation crea dos métricas de plataforma distintas relacionadas con Update Management que se recopilan y reenvían a Azure Monitor. Estas métricas están disponibles para su análisis mediante el [Explorador de métricas](../../azure-monitor/essentials/metrics-charts.md) y para enviar alertas con una [regla de alertas de métricas](../../azure-monitor/alerts/alerts-metric.md).

Las dos métricas emitidas son:

* Ejecuciones de máquina de implementación de actualizaciones totales
* Ejecuciones de implementación de actualizaciones totales

Cuando se usan en las alertas, ambas métricas admiten dimensiones que contienen información adicional que le permitirá definir el ámbito de la alerta en función de un detalle específico de la implementación de actualizaciones. En la tabla siguiente se muestran los detalles sobre la métrica y las dimensiones disponibles al configurar una alerta.

|Nombre de señal|Dimensions|Descripción
|---|---|---|
|`Total Update Deployment Runs`|- Nombre de implementación de actualizaciones<br>- Estado | Muestra una alerta sobre el estado general de una implementación de actualizaciones.|
|`Total Update Deployment Machine Runs`|- Nombre de implementación de actualizaciones</br>- Estado</br>- Equipo de destino</br>- Identificador de ejecución de implementación de actualizaciones    |Muestra una alerta sobre el estado de una implementación de actualizaciones dirigida a máquinas específicas.|

## <a name="create-alert"></a>Crear una alerta

Realice los pasos siguientes para configurar alertas que le permitan saber el estado de una implementación de actualizaciones. Si no está familiarizado con las alertas de Azure, consulte la [información general sobre alertas de Azure](../../azure-monitor/alerts/alerts-overview.md).

1. En la cuenta de Automation, seleccione **Alertas** en la opción de **supervisión** y, a continuación, seleccione **Nueva regla de alerta**.

1. En la página de **creación de reglas de alertas**, su cuenta de Automation aparece seleccionada como recurso. Si quiere cambiar esto, seleccione la opción para **editar recursos**.

1. En la página para seleccionar un recurso, elija **Cuentas de Automation** en la lista desplegable **Filtrar por tipo de recurso**.

1. Seleccione la cuenta de Automation que quiera usar y, a continuación, haga clic en **Listo**.

1. Seleccione **Agregar condición** para elegir la señal adecuada para sus requisitos.

1. En el caso de las dimensiones, seleccione un valor válido de la lista. Si el valor que quiere usar no está en la lista, seleccione el signo **\+** situado junto a la dimensión y escriba el nombre personalizado. A continuación, seleccione el valor que quiere buscar. Si quiere seleccionar todos los valores de una dimensión, haga clic en el botón **Seleccionar \*** . Si no elige ningún valor para una dimensión, Update Management no tendrá en cuenta esa dimensión.

    ![Configuración de la lógica de señal](./media/manage-updates-for-vm/signal-logic.png)

1. En **Lógica de alerta**, escriba los valores en los campos **Agregación de tiempo** y **Umbral**, y seleccione **Listo**.

1. En el siguiente panel, escriba un nombre y una descripción para la alerta.

1. En el campo **Gravedad**, seleccione **Información (gravedad 2)** para una ejecución correcta, o bien **Información (gravedad 1)** para una ejecución con errores.

    ![Captura de pantalla que muestra la sección Definir detalles de la alerta con los campos Nombre de la regla de alertas, Descripción y Gravedad resaltados.](./media/manage-updates-for-vm/define-alert-details.png)

1. Seleccione **Sí** para habilitar la regla de alerta.

## <a name="configure-action-groups-for-your-alerts"></a>Configuración de grupos de acciones para las alertas

Una vez que haya configurado las alertas, puede configurar un grupo de acciones para usarlas con varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks y mucho más. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../../azure-monitor/alerts/action-groups.md).

1. Seleccione una alerta y, a continuación, seleccione **Add action groups** (Agregar grupos de acciones) en **Acciones**. Esto hará que aparezca el panel **Seleccionar un grupo de acciones para asociar a esta regla de alerta**.

   :::image type="content" source="./media/manage-updates-for-vm/select-an-action-group.png" alt-text="Uso y costos estimados.":::

1. Active la casilla correspondiente al grupo de acciones que desea adjuntar y presione Seleccionar.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [alertas en Azure Monitor](../../azure-monitor/alerts/alerts-overview.md).

* Obtenga información sobre las [consultas de registros](../../azure-monitor/logs/log-query-overview.md) para recuperar y analizar datos provenientes de un área de trabajo de Log Analytics.

* En [Administrar el uso y los costos con los registros de Azure Monitor](../../azure-monitor/logs/manage-cost-storage.md) se describe cómo controlar los costos cambiando el período de retención de datos, y cómo analizar y alertar sobre el uso de los datos.