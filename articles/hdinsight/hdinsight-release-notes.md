---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
ms.custom: references_regions
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/23/2021
ms.openlocfilehash: a648ff3aa0c042aaefe16eaae0f9d73953241b3d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106065504"
---
# <a name="azure-hdinsight-release-notes"></a>Notas de la versión de Azure HDInsight

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

Si quiere suscribirse a las notas de la versión, vea las versiones de [este repositorio de GitHub](https://github.com/hdinsight/release-notes/releases).

## <a name="release-date-03242021"></a>Fecha de lanzamiento: 24/03/2021

Esta versión se aplica a HDInsight 3.6 y HDInsight 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

## <a name="new-features"></a>Nuevas características
### <a name="spark-30-preview"></a>Versión preliminar de Spark 3.0
HDInsight agregó compatibilidad con [Spark 3.0.0](https://spark.apache.org/docs/3.0.0/) a HDInsight 4.0 como una característica en vista previa. 

### <a name="kafka-24-preview"></a>Versión preliminar de Kafka 2.4
HDInsight agregó compatibilidad con [Kafka 2.4.1](http://kafka.apache.org/24/documentation.html) a HDInsight 4.0 como una característica en vista previa.

### <a name="eav4-series-support"></a>Compatibilidad con la serie Eav4
HDInsight ha agregado compatibilidad con la serie Eav4 en esta versión. Obtenga más información sobre la [serie Dav4 aquí](../virtual-machines/eav4-easv4-series.md). La serie está disponible en las siguientes regiones: 

* Este de Australia
* Sur de Brasil
* Centro de EE. UU.
* Este de Asia
* Este de EE. UU.
* Japón Oriental
* Sudeste de Asia
* Sur de Reino Unido
* Oeste de Europa
* Oeste de EE. UU. 2

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. El servicio se migra gradualmente a [conjuntos de escalado de máquinas virtuales de Azure](../virtual-machine-scale-sets/overview.md). Todo el proceso puede tardar meses. Después de migrar las regiones y las suscripciones, los clústeres de HDInsight recién creados se ejecutarán en conjuntos de escalado de máquinas virtuales sin acciones del cliente. No se espera ningún cambio importante.

## <a name="deprecation"></a>Desuso
No hay desuso en esta versión.

## <a name="behavior-changes"></a>Cambios de comportamiento
### <a name="default-cluster-version-is-changed-to-40"></a>La versión predeterminada del clúster cambiará a la 4.0
La versión predeterminada del clúster de HDInsight cambiará de la 3.6 a la 4.0. Para más información sobre las versiones disponibles, consulte [este artículo](./hdinsight-component-versioning.md). Más información sobre las novedades de [HDInsight 4.0](./hdinsight-version-release.md).

### <a name="default-cluster-vm-sizes-are-changed-to-ev3-series"></a>Los tamaños de la máquina virtual del clúster predeterminado cambian en la serie Ev3 
Los tamaños de la máquina virtual del clúster predeterminado cambia de la serie D a la serie Ev3. Este cambio se aplica a los nodos principales y los nodos de trabajo. Para evitar que este cambio afecte a los flujos de trabajo probados, especifique los tamaños de máquina virtual que desea usar en la plantilla de Resource Manager.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>El recurso de la interfaz de red no es visible para clústeres que se ejecutan en conjuntos de escalado de máquinas virtuales de Azure.
HDInsight se está migrando gradualmente a conjuntos de escalado de máquinas virtuales de Azure. Los clientes ya no pueden ver las interfaces de red de las máquinas virtuales de los clústeres que usan conjuntos de escalado de máquinas virtuales de Azure.

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios.

### <a name="os-version-upgrade"></a>Actualización de la versión del sistema operativo
HDInsight estará actualizando la versión del sistema operativo de Ubuntu 16.04 a 18.04. La actualización se completará antes de abril de 2021.

### <a name="basic-support-for-hdinsight-36-starting-july-1-2021"></a>Soporte técnico Basic para HDInsight 3.6 a partir del 1 de julio de 2021
A partir del 1 de julio de 1, 2021, Microsoft ofrecerá [soporte técnico Basic](hdinsight-component-versioning.md#support-options-for-hdinsight-versions) para determinados tipos de clúster de HDInsight 3.6. El plan de soporte técnico Basic estará disponible hasta el 3 de abril de 2022. Se inscribirá automáticamente en soporte técnico Basic a partir del 1 de julio de 2021. No es necesario realizar ninguna acción para participar. Consulte [nuestra documentación](hdinsight-36-component-versioning.md) sobre los tipos de clúster que se incluyen en soporte técnico Basic. 

No se recomienda crear nuevas soluciones en HDInsight 3.6, inmovilice los cambios en entornos existentes de 3.6. Se recomienda [migrar los clústeres a HDInsight 4.0](hdinsight-version-release.md#how-to-upgrade-to-hdinsight-40). Más información sobre [las novedades de HDInsight 4.0](hdinsight-version-release.md#whats-new-in-hdinsight-40).

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
Se ha agregado compatibilidad con Spark 3.0.0 y Kafka 2.4.1 como versión preliminar. En [este documento](./hdinsight-component-versioning.md) puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.

## <a name="recommanded-features"></a>Características recomendadas
### <a name="service-tags"></a>Etiquetas de servicio
Las etiquetas de servicio simplifican la restricción del acceso de red a los servicios de Azure para las máquinas virtuales y las redes virtuales de Azure. Las etiquetas de servicio en las reglas del grupo de seguridad de red (NSG) permiten o impiden el tráfico a un servicio específico de Azure. La regla se puede establecer globalmente o por región de Azure. Azure proporciona el mantenimiento de las direcciones IP subyacentes a cada etiqueta. Las etiquetas de servicio de HDInsight de los grupos de seguridad de red (NSG) son grupos de direcciones IP para los servicios de mantenimiento y administración. Estos grupos ayudan a minimizar la complejidad de la creación de reglas de seguridad. Los clientes de HDInsight pueden habilitar la etiqueta de servicio mediante Azure Portal, PowerShell y la API de REST. Para más información, consulte [Etiquetas de servicio del grupo de seguridad de red (NSG) para Azure HDInsight](./hdinsight-service-tags.md).
