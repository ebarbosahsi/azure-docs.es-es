---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: d8670127f871ded2cf01450609bf779deb3f2d9c
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "103021656"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las instancias administradas de SQL deben usar claves administradas por el cliente para cifrar los datos en reposo](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F048248b0-55cd-46da-b1ff-39efd52db260) |La implementación de Cifrado de datos transparente (TDE) con una clave propia proporciona una mayor transparencia y control sobre el protector de TDE, ofrece mayor seguridad con un servicio externo respaldado con HSM y permite la separación de tareas. Esta recomendación se aplica a las organizaciones con un requisito de cumplimiento relacionado. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |
|[Los servidores SQL deben usar claves administradas por el cliente para cifrar los datos en reposo](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0d134df8-db83-46fb-ad72-fe0c9428c8dd) |La implementación de Cifrado de datos transparente (TDE) con una clave propia proporciona una mayor transparencia y control sobre el protector de TDE, ofrece mayor seguridad con un servicio externo respaldado con HSM y permite la separación de tareas. Esta recomendación se aplica a las organizaciones con un requisito de cumplimiento relacionado. |AuditIfNotExists, Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |
|[El cifrado de datos transparente en bases de datos SQL debe estar habilitado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17k78e20-9358-41c9-923c-fb736d382a12) |El cifrado de datos transparente debe estar habilitado para proteger los datos en reposo y satisfacer los requisitos de cumplimiento. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlDBEncryption_Audit.json) |
