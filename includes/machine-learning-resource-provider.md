---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 8fd774f8a3a73ceaffa7902b35e1b1dff12ef5af
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "75893965"
---
Al crear un área de trabajo de Azure Machine Learning o un recurso usado por el área de trabajo, puede recibir un mensaje de error similar a los siguientes:

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

Muchos proveedores de recursos se registran automáticamente, aunque no todos. Si recibe este mensaje, debe registrar el proveedor mencionado.

Para obtener más información sobre cómo registrar un proveedor de recursos, consulte [Registro del proveedor de recursos](../articles/azure-resource-manager/templates/error-register-resource-provider.md).