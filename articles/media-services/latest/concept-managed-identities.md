---
title: Identidades administradas
description: Media Services puede usarse con identidades administradas de Azure.
keywords: ''
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: inhenkel
ms.openlocfilehash: 0bbfb54d6ba7483e96633bdf05bb580e5517d216
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2021
ms.locfileid: "106277749"
---
# <a name="managed-identities"></a>Identidades administradas

Un desafío común para los desarrolladores es la administración de secretos y credenciales para proteger la comunicación entre distintos servicios. En Azure, las identidades administradas eliminan la necesidad de que los desarrolladores administren credenciales, al proporcionar una identidad para el recurso de Azure en Azure AD y usarla para obtener tokens de Azure Active Directory (Azure AD).

Actualmente hay dos escenarios en los que las identidades administradas se pueden usar con Media Services:

- Usar la identidad administrada de la cuenta de Media Services para acceder a las cuentas de almacenamiento.

- Usar la identidad administrada de la cuenta de Media Services para acceder a las claves de cliente de una instancia de Key Vault.

En las dos secciones siguientes se describen los pasos de los dos escenarios.

## <a name="use-the-managed-identity-of-the-media-services-account-to-access-storage-accounts"></a>Uso de la identidad administrada de la cuenta de Media Services para acceder a las cuentas de almacenamiento

1. Cree una cuenta de Media Services con una identidad administrada.
1. Conceda a la entidad de seguridad administrada acceso a una cuenta de almacenamiento de su propiedad.
1. De este modo, Media Services puede acceder a la cuenta de almacenamiento en su nombre mediante la identidad administrada.

## <a name="use-the-managed-identity-of-the-media-services-account-to-access-key-vault-to-access-customer-keys"></a>Uso de la identidad administrada de la cuenta de Media Services para acceder a las claves de cliente de una instancia de Key Vault

1. Cree una cuenta de Media Services con una identidad administrada.
1. Conceda acceso a la entidad de seguridad administrada a una instancia de Key Vault de su propiedad.
1. Configure la cuenta de Media Services para usar el cifrado de cuenta basado en claves de cliente.
1. Media Services accede a Key Vault en su nombre mediante la identidad administrada.

Para obtener más información acerca de las claves administradas por el cliente y Key Vault, consulte [Bring Your Own Key (claves administradas por el cliente) con Media Services](concept-use-customer-managed-keys-byok.md)

## <a name="tutorials"></a>Tutoriales

Estos tutoriales incluyen los dos escenarios mencionados anteriormente.

- [Uso de Azure Portal para emplear claves administradas por el cliente o BYOK con Media Services](security-customer-managed-keys-portal-tutorial.md)
- [Uso de claves administradas por el cliente o BYOK con la API REST de Media Services](security-customer-managed-keys-rest-postman-tutorial.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las funciones de las identidades administradas y su uso con las aplicaciones de Azure, consulte [Identidades administradas de Azure AD](../../active-directory/managed-identities-azure-resources/overview.md).
