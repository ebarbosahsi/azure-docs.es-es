---
title: Migración basada en agente en Azure Migrate Server
description: Proporciona información general sobre la migración de VM de VMware basada en agente en Azure Migrate.
author: rahulg1190
ms.author: rahugup
ms.manager: bsiva
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: f4f79725d0eda65ba00a44e9e7fc2a51c024eccf
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104864064"
---
# <a name="agent-based-migration-architecture"></a>Arquitectura de migración basada en agente

En este artículo se proporciona información general sobre la arquitectura y los procesos que se usan para la replicación de VM de VMware basada en agente con la herramienta [Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool).

Con Azure Migrate: Server Migration, puede replicar VM de VMware con un par de opciones:

- Migración de VM mediante la replicación basada en agente, como se describe en este artículo.
- Migración de VM de VMware con la migración sin agentes. Las VM se migran sin necesidad de instalar nada en ellas.

Obtenga más información sobre [seleccionar y comparar](server-migrate-overview.md) los métodos de migración para VM de VMware. 


## <a name="agent-based-migration"></a>Migración basada en agente

La migración basada en agente se usa para migrar las VM de VMware locales y los servidores físicos a Azure. También se puede usar para migrar otros servidores virtualizados locales, así como VM de nube privada y pública, incluidas instancias de AWS y VM de GCP. La migración basada en agente en Azure Migrate usa alguna funcionalidad de back-end del servicio [Azure Site Recovery](../site-recovery/site-recovery-overview.md).


## <a name="architectural-components"></a>Componentes de la arquitectura

En el diagrama se muestran los componentes implicados en la migración basada en agente.

![En el diagrama se muestran los componentes de la migración basada en agente, que se explican en una tabla.](./media/agent-based-replication-architecture/architecture.png)

En la tabla se resumen los componentes usados para la migración basada en agente.

**Componente** | **Detalles** | **Instalación**
--- | --- | ---
**Dispositivo de replicación** | El dispositivo de replicación (servidor de configuración o proceso) es un servidor local que sirve de puente entre el entorno local y Server Migration. El dispositivo detecta el inventario del servidor local, de modo que Server Migration puede orquestar la replicación y la migración. El dispositivo tiene dos componentes:<br/><br/> **Servidor de configuración**: Se conecta a Server Migration y coordina la replicación.<br/> **Servidor de proceso**: Controla la replicación de datos. El servidor de procesos recibe los datos del servidor los comprime y cifra, y los envía a Azure. En Azure, Server Migration escribe los datos en discos administrados. | De manera predeterminada, el servidor de procesos se instala junto con el servidor de configuración en el dispositivo de replicación.
**Servicio de movilidad** | El servicio Mobility es un agente que se instala en cada servidor que quiera replicar y migrar. Envía los datos de replicación desde el servidor al servidor de procesos. | Los archivos de instalación de las diferentes versiones del servicio Mobility se encuentran en el dispositivo de replicación. Descargue e instale el agente que necesita, de acuerdo con el sistema operativo y la versión del servidor que quiere replicar.

## <a name="mobility-service-installation"></a>Instalación de Mobility Service

Puede implementar Mobility Service con los métodos siguientes:

- **Instalación de inserción**: el servidor de procesos instala el servicio Mobility cuando usted habilita la protección para un servidor. 
- **Instalación manual**: puede instalar el servicio Mobility manualmente en cada servidor a través de la interfaz de usuario o del símbolo del sistema.

El servicio Mobility se comunica con el dispositivo de replicación y con los servidores replicados. Si tiene software antivirus que se ejecuta en el dispositivo de replicación, en los servidores de procesos o en los servidores que se están replicando, se deben excluir del análisis las siguientes carpetas:


- C:\Archivos de programa\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\Program Files (x86)\Microsoft Azure Site Recovery
- C:\ProgramData\ASR\agent (en servidores de Windows con el servicio Mobility instalado)

## <a name="replication-process"></a>Proceso de replicación

1. Cuando habilita la replicación para un servidor, comienza la replicación inicial en Azure.
2. Durante la replicación inicial, el servicio Mobility lee los datos de los discos de los servidores y los envía al servidor de procesos.
3. Estos datos se usan para inicializar una copia del disco en la suscripción de Azure. 
4. Una vez terminada la replicación inicial, comienza la de los cambios incrementales en Azure. La replicación es a nivel de bloque y casi continua.
4. El servicio Mobility intercepta las escrituras en la memoria de disco, al integrarse en el subsistema de almacenamiento del sistema operativo. Este método evita las operaciones de E/S de disco en el servidor de replicación para una replicación incremental. 
5. Los cambios marcados para un servidor se envían al servidor de procesos en el puerto entrante HTTPS 9443. Este puerto se puede modificar. El servidor de procesos los comprime y cifra, y los envía a Azure. 

## <a name="ports"></a>Puertos

**Dispositivo** | **Connection**
--- | --- 
**Replicando servidores** | La instancia de Mobility Service que se ejecuta en las VM se comunica con el dispositivo de replicación local en el puerto HTTPS 443 entrante, para la administración de la replicación.<br/><br/> Los servidores envían los datos de replicación al servidor de procesos en el puerto entrante HTTPS 9443. Este puerto se puede modificar.
**Dispositivo de replicación** | El dispositivo de replicación organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
**Servidor de proceso** | El servidor de procesos recibe datos de replicación, los optimiza y los cifra para enviarlos después a Azure Storage a través del puerto 443 de salida.


## <a name="performance-and-scaling"></a>Rendimiento y escalado

De forma predeterminada, se implementa un único dispositivo de replicación que ejecuta tanto el servidor de configuración como el servidor de procesos. Si solo va a replicar algunos servidores, esta implementación es suficiente. Sin embargo, si va a replicar y migrar cientos de servidores, es posible que un único servidor de procesos no pueda controlar todo el tráfico de replicación. En este caso, puede implementar servidores de procesos de escalabilidad horizontal adicionales.

### <a name="plan-vmware-deployment"></a>Planificación de la implementación de VMware

Si va a replicar VM de VMware, puede usar [Site Recovery Deployment Planner para VMware](../site-recovery/site-recovery-deployment-planner.md) para ayudar a determinar los requisitos de rendimiento, incluida la frecuencia diaria de cambio de datos y los servidores de procesos que necesita.

### <a name="replication-appliance-capacity"></a>Capacidad del dispositivo de replicación

Use los valores de esta tabla para averiguar si necesita un servidor de procesos adicional en la implementación.

- Si la frecuencia de cambio diaria (abandono) es superior a 2 TB, implemente un servidor de procesos adicional.
- Si va a replicar más de 200 servidores, implemente un dispositivo de replicación adicional.

**CPU** | **Memoria** | **Espacio libre para el almacenamiento de datos en caché** | **Tasa de renovación** | **Límites de replicación**
--- | --- | --- | --- | ---
8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB o menos | Menos de 100 servidores 
12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz) | 18 GB | 600 GB | De 501 GB a 1 TB | 100-150 servidores.
16 vCPU (2 sockets * 8 núcleos \@ 2,5 GHz) | 32 GB |  1 TB | 1 TB a 2 TB | 151-200 servidores.

### <a name="sizing-scale-out-process-servers"></a>Servidores de procesos de escalabilidad horizontal

Si necesita implementar un servidor de procesos de escalabilidad horizontal, use esta tabla para averiguar el tamaño del servidor.

**Servidor de proceso** | **Espacio libre para el almacenamiento de datos en caché** | **Tasa de renovación** | **Límites de replicación**
--- | --- | --- | --- 
4 vCPU (2 sockets * 2 núcleos \@ 2,5 GHz), 8 GB de memoria | 300 GB | 250 GB o menos | Hasta 85 servidores 
8 vCPU (2 sockets * 4 núcleos \@ 2,5 GHz), 12 GB de memoria | 600 GB | De 251 GB a 1 TB | 86-150 servidores.
12 vCPU (2 sockets * 6 núcleos \@ 2,5 GHz), 24 GB de memoria | 1 TB | 1 - 2 TB | 151-225 servidores.

## <a name="throttle-upload-bandwidth"></a>Limite el ancho de banda de carga.

el tráfico de VMware que se replica en Azure pasa por un servidor de procesos específico. Puede limitar el rendimiento de la carga limitando el ancho de banda en los servidores que se ejecutan como servidores de procesos. Puede influir en el ancho de banda con esta clave del Registro:

- El valor del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM especifica el número de subprocesos que se utilizan para la transferencia de datos (replicación inicial o diferencial) de un disco. Un valor mayor aumenta el ancho de banda de red utilizado para la replicación. El valor predeterminado es cuatro. El valor máximo es 32. Supervise el tráfico para optimizar el valor.
- Además, puede limitar el ancho de banda en el servidor de procesos de la siguiente manera:

    1. En el servidor de procesos, abra el complemento MMC de Azure Backup. Hay un acceso directo en el escritorio o en la carpeta C:\Archivos de Programa\Microsoft Azure Recovery Services Agent\bin. 
    2. En el complemento, seleccione **Cambiar propiedades**.
    3. En **Limitación**, seleccione **Habilitar límite de uso del ancho de banda de Internet para las operaciones de copia de seguridad**. Establezca los límites para las horas laborables y no laborables. Los intervalos válidos van de 512 Kbps a 1023 Mbps.


## <a name="next-steps"></a>Pasos siguientes

Pruebe la [migración basada en agente](tutorial-migrate-vmware-agent.md) para [VMware](tutorial-migrate-vmware-agent.md) o [servidores físicos](tutorial-migrate-physical-virtual-machines.md).
