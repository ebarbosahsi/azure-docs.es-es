---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/31/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 175c630e8fc3959844e11def5f82475ac1d4fb26
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106097221"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82339799-d096-41ae-8538-b108becf0970) |Azure Database for MySQL le permite elegir la opción de redundancia para el servidor de bases de datos. Se puede establecer en un almacenamiento de copia de seguridad con redundancia geográfica donde los datos no solo se almacenan dentro de la región en la que se hospeda el servidor, sino que también se replican en una región emparejada para proporcionar la opción de recuperación en caso de que se produzca un error en la región. La configuración de almacenamiento con redundancia geográfica para copia de seguridad solo se permite durante la creación del servidor. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMySQL_Audit.json) |
