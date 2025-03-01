---
title: Selección de una opción de migración de VMware con Azure Migrate Server Migration
description: Proporciona información general sobre las opciones para migrar máquinas virtuales de VMware a Azure con Azure Migrate Server Migration.
author: anvar-ms
ms.author: anvar
ms.manager: bsiva
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 7446b2050fdd7bbc7704953c053da0629231191c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "101715129"
---
# <a name="select-a-vmware-migration-option"></a>Selección de una opción de migración de VMware

Puede migrar las máquinas virtuales de VMware a Azure mediante la herramienta Azure Migrate Server Migration. Esta herramienta ofrece un par de opciones para la migración de máquinas virtuales de VMware:

- Migración mediante la replicación sin agente. Las máquinas virtuales se migran sin necesidad de instalar nada en ellas.
- Migración con un agente de replicación. Instale un agente en la máquina virtual para su replicación.


## <a name="compare-migration-methods"></a>Comparación de métodos de migración

Use estas comparaciones seleccionadas para que le ayuden a decidir qué método usar. También puede revisar los requisitos de compatibilidad completos para la migración [sin agente](migrate-support-matrix-vmware-migration.md#agentless-migration) y [basada en agente](migrate-support-matrix-vmware-migration.md#agent-based-migration).

**Configuración** | **Sin agente** | **Basado en agente**
--- | --- | ---
**Permisos de Azure** | Necesita permisos para crear un proyecto de Azure Migrate y para registrar aplicaciones de Azure AD creadas al implementar el dispositivo de Azure Migrate. | Necesita permisos de colaborador en la suscripción de Azure. 
**Replicación** | Se pueden replicar un máximo de 500 VM simultáneamente desde una instancia de vCenter Server. En el portal puede seleccionar hasta 10 máquinas virtuales a la vez para la replicación. Para replicar más máquinas, agregue lotes de 10.| La capacidad de replicación aumenta mediante el escalado del dispositivo de replicación.
**Implementación del dispositivo** | El [dispositivo de Azure Migrate](migrate-appliance.md) se implementa de forma local. | El [dispositivo de replicación de Azure Migrate](migrate-replication-appliance.md) se implementa de forma local.
**Compatibilidad con Site Recovery** | Compatible. | No se puede replicar con la migración del servidor de Azure Migrate si ha configurado la replicación para una máquina con Site Recovery.
**Disco de destino** | Discos administrados | Discos administrados
**Límites del disco** | Disco del sistema operativo: 2 TB<br/><br/> Disco de datos: 32 TB<br/><br/> Número máximo de discos: 60 | Disco del sistema operativo: 2 TB<br/><br/> Disco de datos: 32 TB<br/><br/> Número máximo de discos: 63
**Discos de acceso directo** | No compatible | Compatible
**Arranque UEFI** | Compatible. | Compatible.

## <a name="compare-deployment-steps"></a>Comparación de los pasos de implementación

Después de revisar las limitaciones, una descripción de los pasos necesarios para implementar cada solución puede ayudarle a decidir qué opción elegir.

**Task** | **Detalles** |**Sin agente** | **Basado en agente**
--- | --- | --- | ---
**Implementación del dispositivo de Azure Migrate** | Un dispositivo ligero que se ejecuta en una máquina virtual de VMware.<br/><br/> El dispositivo se usa para detectar y evaluar máquinas, así como para migrar máquinas mediante la migración sin agente. | Necesario.<br/><br/> Si ya ha configurado el dispositivo para valoración, puede usar el mismo dispositivo para la migración sin agente. | No se requiere.<br/><br/> Si ha configurado un dispositivo para valoración, puede dejarlo en su lugar o quitarlo si ya ha terminado con la valoración.
**Uso de la herramienta Server Assessment** | Valore las máquinas con la herramienta Server Assessment de Azure Migrate. | La valoración es opcional. | La valoración es opcional.
**Uso de la herramienta Server Migration** | Agregue la herramienta Azure Migrate Server Migration en el proyecto de Azure Migrate. | Obligatorio | Obligatorio
**Preparación de VMware para la migración** | Defina la configuración en máquinas virtuales y servidores de VMware. | Obligatorio | Obligatorio
**Instalación de Mobility Service en máquinas virtuales** | El servicio Mobility se ejecuta en cada máquina que quiera replicar. | No se requiere | Obligatorio
**Implementación del dispositivo de replicación** | El [dispositivo de replicación](migrate-replication-appliance.md) se usa para la migración basada en agente. Se conecta entre el servicio Mobility que se ejecuta en las máquinas virtuales y Server Migration. | No se requiere | Obligatorio
**Replicación de máquinas virtuales**, habilitación de la replicación de máquinas virtuales | Configure las opciones de replicación y seleccione las máquinas virtuales que se van a replicar. | Obligatorio | Obligatorio
**Ejecución de una migración de prueba** | Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada. | Obligatorio | Obligatorio
**Ejecución de una migración completa** | Migre las máquinas virtuales. | Obligatorio | Obligatorio



## <a name="next-steps"></a>Pasos siguientes

[Migración de VM de VMware](tutorial-migrate-vmware.md) con migración sin agentes.



