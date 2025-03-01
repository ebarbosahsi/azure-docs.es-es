---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 09/01/2020
ms.author: tomfitz
ms.openlocfilehash: 543aa50d72de5a06a9a1c7ac88ac5ecae993bc9d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98697941"
---
| Recurso | Límite |
| --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/management/overview.md#resource-groups) | Los recursos no están limitados por el grupo de recursos. En su lugar, están limitados por el tipo de recurso de un grupo de recursos. Consulte la fila siguiente. |
| Recursos por grupo de recursos, por tipo de recurso |800: algunos tipos de recursos pueden superar el límite de 800. Consulte [Resources not limited to 800 instances per resource group](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md) (Recursos no limitados a 800 instancias por grupo de recursos). |
| Implementaciones por grupo de recursos en el historial de implementaciones |800<sup>1</sup> |
| Recursos por implementación |800 |
| Bloqueos de administración por [ámbito](../articles/azure-resource-manager/management/overview.md#understand-scope) único  |20 |
| Número de etiquetas por recurso o grupo de recursos |50 |
| Longitud de la clave de etiqueta |512 |
| Longitud del valor de la etiqueta |256 |

<sup>1</sup>Las implementaciones se eliminan automáticamente del historial a medida se aproxima al límite. Eliminar una entrada del historial de implementaciones no afecta a los recursos implementados. Para obtener más información, consulte [Eliminaciones automáticas del historial de implementaciones](../articles/azure-resource-manager/templates/deployment-history-deletions.md).

#### <a name="template-limits"></a>Límites de plantilla

| Value | Límite |
| --- | --- |
| Parámetros |256 |
| variables |256 |
| Recursos (incluido el recuento de copia) |800 |
| Salidas |64 |
| Expresión de plantilla |24 576 caracteres |
| Recursos de plantillas exportadas |200 |
| Tamaño de la plantilla |4 MB |
| Tamaño del archivo de parámetros |64 KB |

Puede superar algunos límites de plantilla utilizando una plantilla anidada. Para más información, consulte [Uso de plantillas vinculadas al implementar recursos de Azure](../articles/azure-resource-manager/templates/linked-templates.md). Para reducir el número de parámetros, variables o salidas, puede combinar varios valores en un objeto. Para más información, consulte [Objetos como parámetros](/azure/architecture/guide/azure-resource-manager/advanced-templates/objects-as-parameters).