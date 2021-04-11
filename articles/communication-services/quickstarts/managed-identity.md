---
title: Uso de identidades administradas en Communication Services
titleSuffix: An Azure Communication Services quickstart
description: Las identidades administradas le permiten autorizar a Azure Communication Services el acceso desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de funciones y otros recursos.
services: azure-communication-services
author: peiliu
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: peiliu
ms.reviewer: mikben
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: e4b71cc889615fd4784f11c9edd77b44ef421d9e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105110780"
---
# <a name="use-managed-identities"></a>Uso de identidades administradas
Comience a trabajar con Azure Communication Services mediante identidades administradas. Los SDK de SMS e identidad de Communication Services admiten la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

Este inicio rápido le muestra cómo autorizar el acceso a los SDK de identidad y SMS desde un entorno de Azure que admita identidades administradas. También se describe cómo probar el código en un entorno de desarrollo.

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/managed-identity-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/managed-identity-js.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/managed-identity-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/managed-identity-python.md)]
::: zone-end

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre el control de acceso basado en rol de Azure](../../../articles/role-based-access-control/index.yml).
- [Más información sobre la biblioteca de identidades de Azure para .NET](/dotnet/api/overview/azure/identity-readme)
- [Creación de tokens de acceso de usuario](../quickstarts/access-tokens.md)
- [Enviar un mensaje SMS](../quickstarts/telephony-sms/send.md)
- [Más información acerca de SMS](../concepts/telephony-sms/concepts.md)
