---
title: Migración de máquinas locales con Azure Migrate
description: En este artículo se resume cómo migrar máquinas locales a Azure y se recomienda Azure Migrate.
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2020
ms.author: raynew
ms.openlocfilehash: e0e60ee346d20113b2ec7970390d9874522cc770
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "87847319"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migración de máquinas locales a Azure

En este artículo se describen las opciones para migrar máquinas locales a Azure. 

## <a name="migrate-with-azure-migrate"></a>Migración con Azure Migrate

Se recomienda migrar las máquinas a Azure mediante el servicio [Azure Migrate](../migrate/migrate-services-overview.md). Azure Migrate está diseñado específicamente para la migración de servidores. Azure Migrate proporciona un centro centralizado para la detección, evaluación y migración de máquinas locales a Azure.

Siga estos vínculos para migrar con Azure Migrate:

- [Información](../migrate/server-migrate-overview.md) sobre las opciones de migración para VM de VMware y, a continuación, migre las VM a Azure con la migración [sin agente](../migrate/tutorial-migrate-vmware.md) o [basada en agentes](../migrate/tutorial-migrate-vmware-agent.md).
- [Migración de VM de Hyper-V](../migrate/tutorial-migrate-hyper-v.md) a Azure.
- Migración de [servidores físicos u otras VM](../migrate/tutorial-migrate-physical-virtual-machines.md), incluidas [instancias de AWS](../migrate/tutorial-migrate-aws-virtual-machines.md) a Azure.

## <a name="migrate-with-site-recovery"></a>Migración con Site Recovery
Site Recovery debe usarse solo para la recuperación ante desastres, no para la migración.

Si ya utiliza Azure Site Recovery y desea seguir usándolo para la migración, siga los mismos pasos que usa para la recuperación ante desastres.

- Máquinas virtuales de VMware: [Prepare Azure](tutorial-prepare-azure.md) y [VMware](vmware-azure-tutorial-prepare-on-premises.md), comience a [replicar las máquinas](vmware-azure-tutorial.md), [compruebe](tutorial-dr-drill-azure.md) que todo funcione y [ejecute una conmutación por error](vmware-azure-tutorial-failover-failback.md).
- Máquinas virtuales de Hyper-V: [Prepare Azure](tutorial-prepare-azure-for-hyperv.md) y [Hyper-V](hyper-v-prepare-on-premises-tutorial.md), comience a [replicar las máquinas](hyper-v-azure-tutorial.md), [compruebe](tutorial-dr-drill-azure.md) que todo funcione y [ejecute una conmutación por error](hyper-v-azure-failover-failback-tutorial.md).
- Servidores físicos: [Siga el tutorial](physical-azure-disaster-recovery.md) para preparar Azure, preparar las máquinas para la recuperación ante desastres y configurar la replicación.

> [!NOTE]
> Cuando ejecuta una conmutación por error para la recuperación ante desastres, como último paso, confirma la conmutación por error. Al migrar máquinas locales, la opción **Confirmar** no es pertinente. En su lugar, seleccione la opción **Completar la migración**. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Revise las preguntas habituales](../migrate/resources-faq.md) sobre Azure Migrate.

  
