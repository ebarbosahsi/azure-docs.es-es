---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: 70b636b7bb508b71475a7464983b091d5d10e0e1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96026152"
---
 Los clústeres de proceso de Azure Machine Learning también admiten [identidades administradas](../articles/active-directory/managed-identities-azure-resources/overview.md) para autenticar el acceso a los recursos de Azure sin incluir credenciales en el código. Hay dos tipos de identidades administradas:

* Una **identidad administrada asignada por el sistema** se habilita directamente en el clúster de proceso de Azure Machine Learning. El ciclo de vida de una identidad asignada por el sistema está vinculado directamente al clúster de proceso. Si se elimina el clúster de proceso, Azure limpia automáticamente las credenciales y la identidad en Azure AD.
* Una **identidad administrada asignada por el usuario** es un recurso de Azure independiente que se proporciona a través del servicio de identidad administrada de Azure. Puede asignar una identidad administrada asignada por el usuario a varios recursos y se conserva durante tanto tiempo como necesite.