---
title: Escalado automático de clústeres de Azure HDInsight
description: Use la característica de escalabilidad automática para escalar automáticamente los clústeres de Azure HDInsight en función de una programación o de métricas de rendimiento.
ms.service: hdinsight
ms.topic: how-to
ms.custom: contperf-fy21q1, contperf-fy21q2
ms.date: 12/14/2020
ms.openlocfilehash: 5dabae76308f32da7968d8cfa89b95f1eb19c142
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104863775"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Escalado automático de clústeres de Azure HDInsight

La característica gratuita de escalabilidad automática de Azure HDInsight puede aumentar o disminuir automáticamente el número de nodos de trabajo del clúster en función de los criterios establecidos previamente. La característica de escalabilidad automática funciona mediante el escalado del número de nodos dentro de los límites preestablecidos en función de las métricas de rendimiento o de una programación de las operaciones de escalado y reducción vertical.

## <a name="how-it-works"></a>Funcionamiento

La característica de escalabilidad automática emplea dos tipos de condiciones para desencadenar eventos de escalado: umbrales para varias métricas de rendimiento del clúster (lo que se conoce como *escalado basado en la carga*) y desencadenadores basados en el tiempo (lo que se conoce como *escalado basado en la programación*). El escalado basado en cargas cambia el número de nodos del clúster, dentro de un intervalo establecido, para garantizar el uso de CPU óptimo y minimizar el costo de ejecución. El escalado basado en programación cambia el número de nodos del clúster en función de una programación de operaciones de escalado y reducción vertical.

En el vídeo siguiente se proporciona información general sobre los desafíos que puede resolver la escalabilidad automática y cómo puede ayudarle a controlar los costos con HDInsight.

> [!VIDEO https://www.youtube.com/embed/UlZcDGGFlZ0?WT.mc_id=dataexposed-c9-niner]

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Elección del escalado basado en carga o basado en programación

Al elegir un tipo de escalado, tenga en cuenta los siguientes factores:

* Varianza de carga: ¿la carga del clúster sigue un patrón coherente a horas específicas en días específicos? Si no es así, la programación basada en carga es una mejor opción.
* Requisitos de SLA: el escalado de escalabilidad automática es reactivo en lugar de predictivo. ¿Habrá una demora suficiente entre el momento en que la carga empieza a aumentar y el momento en que el clúster tiene que alcanzar su tamaño de destino? Si los requisitos de SLA son estrictos y la carga es un patrón fijo conocido, la mejor opción es "basado en programación".

### <a name="cluster-metrics"></a>Métricas del clúster

La escalabilidad automática supervisa continuamente el clúster y recopila las métricas siguientes:

|Métrica|Descripción|
|---|---|
|Total de CPU pendiente|El número total de núcleos necesarios para iniciar la ejecución de todos los contenedores pendientes.|
|Total de memoria pendiente|La memoria total (en MB) necesaria para iniciar la ejecución de todos los contenedores pendientes.|
|Total de CPU libre|La suma de todos los núcleos sin usar en los nodos de trabajo activos.|
|Total de memoria libre|La suma de la memoria sin usar (en MB) en los nodos de trabajo activos.|
|Memoria usada por nodo|La carga en un nodo de trabajo. Un nodo de trabajo donde se usan 10 GB de memoria se considera bajo más carga que un trabajo con 2 GB de memoria usada.|
|Número de patrones de aplicación por nodo|El número de contenedores de patrones de aplicación (AM) que se ejecutan en un nodo de trabajo. Un nodo de trabajo que hospeda dos contenedores de AM se considera más importante que un nodo de trabajo que no hospeda ninguno de estos contenedores.|

Las métricas anteriores se comprueban cada 60 segundos. Puede configurar las operaciones de escalado para el clúster mediante cualquiera de estas métricas.

### <a name="load-based-scale-conditions"></a>Condiciones de escalado basado en la carga

Cuando se detectan las condiciones siguientes, Escalabilidad automática emite una solicitud de escalado:

|Escalabilidad vertical|Reducción vertical|
|---|---|
|El total de CPU pendiente es mayor que el total de CPU libre durante más de 3 minutos.|El total de CPU pendiente es menor que el total de CPU libre durante más de 10 minutos.|
|El total de memoria pendiente es mayor que el total de memoria libre durante más de 3 minutos.|El total de memoria pendiente es menor que el total de memoria libre durante más de 10 minutos.|

En el caso del escalado vertical, la Escalabilidad automática emite una solicitud para agregar el número necesario de nodos. El escalado vertical se basa en el número de nodos de trabajo nuevos que son necesarios para satisfacer los requisitos de CPU y memoria actuales.

En el caso de la reducción vertical, la Escalabilidad automática emite una solicitud para quitar un cierto número de nodos. La reducción vertical se basa en el número de contenedores AM en cada nodo y en los requisitos actuales de CPU y memoria. El servicio detecta también qué nodos son candidatos para la eliminación en función de la ejecución del trabajo actual. En primer lugar, la operación de reducción vertical retira los nodos y, luego, los quita del clúster.

### <a name="cluster-compatibility"></a>Compatibilidad con el clúster

> [!Important]
> La característica de escalado automático de Azure HDInsight se lanzó con carácter general el 7 de noviembre de 2019 para los clústeres de Spark y Hadoop, e incluía mejoras que no están disponibles en la versión preliminar de la característica. Si creó un clúster de Spark antes del 7 de noviembre de 2019 y quiere usar la característica de escalado automático en el clúster, la ruta recomendada es crear un nuevo clúster y habilitar el escalado automático en el nuevo clúster.
>
> La escalabilidad automática de Interactive Query (LLAP) se publicó en disponibilidad general para HDI 4.0 el 27 de agosto de 2020. Los clústeres de HBase todavía están en versión preliminar. El escalado automático solo está disponible en los clústeres de Spark, Hadoop, Interactive Query y HBase.

En la tabla siguiente se describen las versiones y los tipos de clúster que son compatibles con la característica de escalabilidad automática.

| Versión | Spark | Hive | Interactive Query | HBase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| HDInsight 3.6 sin ESP | Sí | Sí | Sí | Sí* | No | No | No |
| HDInsight 4.0 sin ESP | Sí | Sí | Sí | Sí* | No | No | No |
| HDInsight 3.6 sin ESP | Sí | Sí | Sí | Sí* | No | No | No |
| HDInsight 4.0 con ESP | Sí | Sí | Sí | Sí* | No | No | No |

\* Los clústeres de HBase solo se pueden configurar para escalado basado en programación, no basado en cargas.

## <a name="get-started"></a>Introducción

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Creación de un clúster con Escalabilidad automática basada en carga

Para habilitar la característica Escalabilidad automática con escalado basado en carga, complete estos pasos como parte del proceso de creación de clúster normal:

1. En la pestaña **Configuración y precios**, marque la casilla **Habilitar escalabilidad automática**.
1. Seleccione **Load-based** (Basada en carga) en **Tipo de escalabilidad automática**.
1. Escriba los valores previstos para estas propiedades:  

    * **Número de nodos** inicial para **Nodo de trabajo**.
    * Número **Mín.** de nodos de trabajo.
    * Número **Máx.** de nodos de trabajo.

    :::image type="content" source="./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-autoscale.png" alt-text="Habilitación de la escalabilidad automática basada en carga del nodo de trabajo":::

El número inicial de nodos de trabajo debe estar comprendido entre los valores mínimo y máximo, ambos inclusives. Este valor define el tamaño inicial del clúster durante su creación. El número mínimo de nodos de trabajo debe establecerse en tres o más. El escalado del clúster a menos de tres nodos puede hacer que se quede atascado en el modo seguro debido a una replicación de archivos insuficiente.  Para obtener más información, consulte [Bloqueo en modo seguro](./hdinsight-scaling-best-practices.md#getting-stuck-in-safe-mode).

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Creación de un clúster con Escalabilidad automática basada en programación

Para habilitar la característica Escalabilidad automática con escalado basado en programación, complete estos pasos como parte del proceso de creación de clúster normal:

1. En la pestaña **Configuración y precios**, active la casilla **Habilitar escalabilidad automática**.
1. Especifique el **Número de nodos** para **Nodo de trabajo**, que controla el límite para escalar verticalmente el clúster.
1. Seleccione la opción **Schedule-based** (Basada en programación) en **Tipo de escalabilidad automática**.
1. Seleccione **Configurar** para abrir la ventana **Configuración de la escalabilidad automática**.
1. Seleccione la zona horaria y haga clic en **+ Agregar condición**.
1. Seleccione los días de la semana en que se deberá aplicar la condición nueva.
1. Edite la hora en que debería aplicarse la condición y el número de nodos a los que se debe escalar el clúster.
1. Si es necesario, agregue más condiciones.

    :::image type="content" source="./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png" alt-text="Habilitación de la creación basada en programación del nodo de trabajo":::

El número de nodos debe estar entre 3 y el número máximo de nodos de trabajo especificado antes de agregar condiciones.

### <a name="final-creation-steps"></a>Pasos finales de creación

Seleccione el tipo de máquina virtual de los nodos de trabajo; para ello, seleccione una máquina virtual en la lista desplegable de **Tamaño del nodo**. Después de elegir el tipo de máquina virtual para cada tipo de nodo, puede ver el intervalo de costo estimado para todo el clúster. Ajuste los tipos de máquina virtual que se ajusten a su presupuesto.

:::image type="content" source="./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-vmsize.png" alt-text="Habilitación de la escalabilidad automática del tamaño del nodo basada en la programación del nodo de trabajo":::

La suscripción tiene una cuota de capacidad para cada región. El número total de núcleos de los nodos principales y el máximo de nodos de trabajo no puede superar la cuota de capacidad. Sin embargo, esta cuota tiene un límite flexible; sencillamente puede crear una incidencia de soporte técnico en cualquier momento para que la aumenten.

> [!Note]  
> Si se supera el límite de cuota de núcleos total, recibirá un mensaje de error que dice "the maximum node exceeded the available cores in this region, please choose another region or contact the support to increase the quota" (El nodo máximo superó los núcleos disponibles en esta región, elija otra región o póngase en contacto con soporte técnico para aumentar la cuota).

Para obtener más información sobre la creación de clústeres de HDInsight con Azure Portal, consulte [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Creación de un clúster con una plantilla del Administrador de recursos

#### <a name="load-based-autoscaling"></a>Escalado automático basado en carga

Para crear un clúster de HDInsight con el escalado automático basado en carga de una plantilla de Azure Resource Manager, agregue un nodo `autoscale` a la sección `computeProfile` > `workernode` con las propiedades `minInstanceCount` y `maxInstanceCount`, tal como se muestra en el fragmento de código JSON a continuación. Para obtener una plantilla de Resource Manager completa, consulte [Plantilla de inicio rápido: Implementación de un clúster de Spark con la escalabilidad automática basada en carga habilitada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-autoscale-loadbased).

```json
{
  "name": "workernode",
  "targetInstanceCount": 4,
  "autoscale": {
      "capacity": {
          "minInstanceCount": 3,
          "maxInstanceCount": 10
      }
  },
  "hardwareProfile": {
      "vmSize": "Standard_D13_V2"
  },
  "osProfile": {
      "linuxOperatingSystemProfile": {
          "username": "[parameters('sshUserName')]",
          "password": "[parameters('sshPassword')]"
      }
  },
  "virtualNetworkProfile": null,
  "scriptActions": []
}
```

#### <a name="schedule-based-autoscaling"></a>Escalado automático basado en programación

Para crear un clúster de HDInsight con el escalado automático basado en programación de una plantilla de Azure Resource Manager, agregue un nodo `autoscale` a la sección `computeProfile` > `workernode`. El nodo `autoscale` contiene una `recurrence` que tiene una `timezone` y una `schedule` que describe cuándo se aplicará el cambio. Para obtener una plantilla de Resource Manager completa, consulte [Implementación de un clúster de Spark con la escalabilidad automática basada en programación habilitada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-autoscale-schedulebased).

```json
{
  "autoscale": {
    "recurrence": {
      "timeZone": "Pacific Standard Time",
      "schedule": [
        {
          "days": [
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday"
          ],
          "timeAndCapacity": {
            "time": "11:00",
            "minInstanceCount": 10,
            "maxInstanceCount": 10
          }
        }
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Habilitación y deshabilitación de escalabilidad automática para un clúster en ejecución

#### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Para habilitar la escalabilidad automática en un clúster en ejecución, seleccione **Tamaño del clúster** en **Configuración**. Después, seleccione **Habilitar escalado automático**. Seleccione el tipo de escalabilidad automática que quiere y especifique las opciones de escalado basado en carga o basado en programación. Por último, seleccione **Guardar**.

:::image type="content" source="./media/hdinsight-autoscale-clusters/azure-portal-settings-autoscale.png" alt-text="Habilitación de la escalabilidad automática de la ejecución del clúster basada en programación del nodo de trabajo":::

#### <a name="using-the-rest-api"></a>Uso de la API de REST

Para habilitar o deshabilitar la Escalabilidad automática en un clúster en ejecución mediante la API REST, realice una solicitud POST al punto de conexión de escalado automático:

```
https://management.azure.com/subscriptions/{subscription Id}/resourceGroups/{resourceGroup Name}/providers/Microsoft.HDInsight/clusters/{CLUSTERNAME}/roles/workernode/autoscale?api-version=2018-06-01-preview
```

Use los parámetros adecuados en la carga de solicitud. La carga de JSON siguiente podría usarse para habilitar el escalado automático. Use la carga `{autoscale: null}` para deshabilitar el escalado automático.

```json
{ "autoscale": { "capacity": { "minInstanceCount": 3, "maxInstanceCount": 5 } } }
```

Vea la sección anterior sobre [cómo habilitar el escalado automático basado en la carga](#load-based-autoscaling) para obtener una descripción completa de todos los parámetros de carga.

## <a name="monitoring-autoscale-activities"></a>Supervisión de actividades de escalabilidad automática

### <a name="cluster-status"></a>Estado del clúster

El estado del clúster que aparece en Azure Portal puede ayudarlo a supervisar las actividades de escalabilidad automática.

:::image type="content" source="./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png" alt-text="Habilitación de la escalabilidad automática del estado del clúster basada en la carga del nodo de trabajo":::

Todos los mensajes de estado del clúster que podría ver se explican en la lista siguiente.

| Estado del clúster | Descripción |
|---|---|
| En ejecución | El clúster funciona normalmente. Todas las actividades de Escalabilidad automática anteriores se completaron correctamente. |
| Actualizando  | La configuración de Escalabilidad automática del clúster se está actualizando.  |
| Configuración de HDInsight  | Una operación de escalado o de reducción verticales de un clúster está en curso.  |
| Error de actualización  | HDInsight experimentó problemas durante la actualización de la configuración de Escalabilidad automática. Los clientes pueden elegir si reintentan la actualización o deshabilitan la escalabilidad automática.  |
| Error  | Error en el clúster, que no se puede usar. Elimine este clúster y cree uno nuevo.  |

Para ver el número actual de nodos en el clúster, vaya al gráfico **Tamaño del clúster** en la página de **información general** del clúster. O bien, seleccione **Tamaño del clúster** en **Configuración**.

### <a name="operation-history"></a>Historial de operaciones

Puede ver el historial de escalado y reducción verticales del clúster como parte de las métricas del clúster. También puede enumerar todas las acciones de escalado durante el último día, semana u otro período de tiempo.

Seleccione **Métricas** en **Supervisión**. Luego, seleccione en **Agregar métrica** y **Número de trabajos activos** en la lista desplegable **Métrica**. Seleccione el botón que se encuentra en la esquina superior derecha para cambiar el intervalo de tiempo.

:::image type="content" source="./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png" alt-text="Habilitación de la escalabilidad automática de la métrica basada en programación del nodo de trabajo":::

## <a name="best-practices"></a>Procedimientos recomendados

### <a name="consider-the-latency-of-scale-up-and-scale-down-operations"></a>Consideración de la latencia de las operaciones de escalado y reducción verticales

Una operación de escalado puede tardar entre 10 y 20 minutos en completarse. Cuando configure una programación personalizada, planee esta demora. Por ejemplo, si necesita que el tamaño del clúster sea 20 a las 9:00 a.m., establezca el desencadenador de programación más temprano, a eso de las 8:30 a.m., para que la operación de escalado se complete antes de las 9:00 a.m.

### <a name="prepare-for-scaling-down"></a>Preparación para la reducción vertical

Durante el proceso de reducción vertical de un clúster, la característica Escalabilidad automática retira los nodos para cumplir con el tamaño de destino. Si las tareas están en ejecución en esos nodos, la Escalabilidad automática espera hasta que se completan las tareas para clústeres Spark y Hadoop. Como cada nodo de trabajo también tiene un rol en HDFS, los datos temporales se desplazan a los nodos restantes. Asegúrese de que haya espacio suficiente en los nodos restantes para hospedar todos los datos temporales.

Los trabajos seguirán en ejecución. Los trabajos pendientes esperarán una programación con menos nodos de trabajo disponibles.

### <a name="be-aware-of-the-minimum-cluster-size"></a>Consideración del tamaño mínimo del clúster

No reduzca verticalmente el clúster a menos de tres nodos. El escalado del clúster a menos de tres nodos puede hacer que se quede atascado en el modo seguro debido a una replicación de archivos insuficiente. Para más información, vea [Bloqueo en modo seguro](hdinsight-scaling-best-practices.md#getting-stuck-in-safe-mode).

### <a name="increase-the-number-of-mappers-and-reducers"></a>Aumento del número de asignadores y reductores

La escalabilidad automática para clústeres de Hadoop también supervisa el uso de HDFS. Si HDFS está ocupado, se supone que el clúster aún necesita los recursos actuales. Cuando hay datos masivos implicados en la consulta, puede aumentar el número de asignadores y reductores para aumentar el paralelismo y acelerar las operaciones de HDFS. De este modo, se activará la reducción vertical adecuada cuando haya recursos adicionales. 

### <a name="set-the-hive-configuration-maximum-total-concurrent-queries-for-the-peak-usage-scenario"></a>Establecimiento del número máximo de consultas simultáneas totales de configuración de Hive para el escenario de uso máximo

Los eventos de escalabilidad automática no cambian la configuración de Hive de *Número máximo total de consultas simultáneas* en Ambari. Esto significa que el servicio interactivo del servidor de Hive 2 solo puede controlar el número dado de consultas simultáneas en cualquier momento, incluso si el número de demonios de cola interactiva se escala y reduce verticalmente en función de la carga y programación. La recomendación general es establecer esta configuración para el escenario de uso máximo para evitar la intervención manual.

Sin embargo, es posible que experimente un error de reinicio del servidor de Hive 2 si solo hay un número pequeño de nodos de trabajo y el valor configurado del máximo de consultas simultáneas totales es demasiado alto. Como mínimo, se necesita el número mínimo de nodos de trabajo que pueden dar cabida al número especificado de AM de Tez (igual a la configuración de consultas simultáneas máximas en total). 

## <a name="limitations"></a>Limitaciones

### <a name="node-label-file-missing"></a>Ausencia del archivo de etiqueta de nodo

La escalabilidad automática de HDInsight usa un archivo de etiqueta de nodo para determinar si un nodo está listo para ejecutar tareas. El archivo de etiqueta de nodo se almacena en HDFS con tres réplicas. Si el tamaño del clúster experimenta una reducción vertical drástica y hay una gran cantidad de datos temporales, existe una pequeña probabilidad de que se puedan eliminar las tres réplicas. Si esto sucede, el clúster entra en un estado de error.

### <a name="interactive-query-daemons-count"></a>Recuento de demonios de Interactive Query

En el caso de los clústeres de Interactive Query habilitados para la escalabilidad automática, el evento de escalar o reducir verticalmente también escala vertical u horizontalmente el número de demonios de Interactive Query al número de nodos de trabajo activos. El cambio en el número de demonios no se conserva en la configuración `num_llap_nodes` de Ambari. Si los servicios de Hive se reinician manualmente, el número de demonios de Interactive Query se restablece según la configuración de Ambari.

Si el servicio Interactive Query se reinicia manualmente, es necesario cambiar manualmente la configuración `num_llap_node` (el número de nodos necesarios para ejecutar el demonio de Interactive Query de Hive) en *Advanced hive-interactive-env* para que coincida con el número actual de nodos de trabajo activos.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las instrucciones para el escalado manual de clústeres en las [Instrucciones de escalado](hdinsight-scaling-best-practices.md)
