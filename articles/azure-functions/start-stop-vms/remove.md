---
title: Eliminar información general sobre iniciar/detener máquinas virtuales v2 (versión preliminar)
description: En este artículo se describe cómo quitar la característica iniciar/detener máquinas virtuales V2 (versión preliminar).
services: azure-functions
ms.subservice: ''
ms.date: 03/30/2021
ms.topic: conceptual
ms.openlocfilehash: b4308be8f7494c1cb6f6b4839fc5a2e9717668e3
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2021
ms.locfileid: "106111343"
---
# <a name="how-to-remove-startstop-vms-v2-preview"></a>Cómo eliminar iniciar/detener máquinas virtuales v2 (versión preliminar)

Después de habilitar la característica iniciar/detener máquinas virtuales v2 (versión preliminar) para administrar el estado de ejecución de las máquinas virtuales de Azure, puede decidir dejar de usarla. La eliminación de esta característica puede realizarse eliminando el grupo de recursos dedicado para almacenar los siguientes recursos en la compatibilidad con las máquinas virtuales de inicio y detención V2 (versión preliminar):

- Las aplicaciones de Azure Functions
- Programación de Azure Logic Apps
- La instancia de Application Insights
- Cuenta de Azure Storage

## <a name="delete-the-dedicated-resource-group"></a>Eliminación del grupo de recursos dedicado

Para eliminar el grupo de recursos creado, siga los pasos descritos en el artículo [Eliminación de grupos de recursos y recursos en Azure Resource Manager](../../azure-resource-manager/management/delete-resource-group.md).

## <a name="next-steps"></a>Pasos siguientes

Para implementar esta característica, consulte [Implementación de iniciar/detener máquinas virtuales v2](deploy.md) (versión preliminar).