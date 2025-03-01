---
title: 'Uso compartido externo de Microsoft 365 y colaboración B2B: Azure AD'
description: Se describe el uso compartido de recursos con asociados externos mediante Microsoft 365 y la colaboración B2B de Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/04/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: d003008ea5b0d2591574f6f488b0145ee6f08a5e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100008135"
---
# <a name="microsoft-365-external-sharing-and-azure-active-directory-azure-ad-b2b-collaboration"></a>Uso compartido externo de Microsoft 365 y colaboración B2B de Azure Active Directory (Azure AD)

En el uso compartido externo de Microsoft 365 y la colaboración B2B de Azure AD (OneDrive, SharePoint Online, grupos unificados, etc.), la autenticación de los usuarios externos se realiza mediante Azure AD B2B.

## <a name="how-does-azure-ad-b2b-differ-from-external-sharing-in-sharepoint-online"></a>¿En qué se diferencia Azure AD B2B del uso compartido externo de SharePoint Online?

OneDrive y SharePoint Online tiene un administrador de invitaciones independiente. La compatibilidad con el uso compartido externo en OneDrive o SharePoint Online comenzó antes que Azure AD desarrollara su compatibilidad. Con el tiempo, el uso compartido externo de OneDrive o SharePoint Online ha acumulado varias características y muchos millones de usuarios que usan el patrón de uso compartido integrado del producto. Sin embargo, existen algunas diferencias sutiles entre cómo funciona el uso compartido externo de OneDrive y SharePoint Online y la colaboración B2B de Azure AD. Puede aprender más sobre el uso compartido externo de OneDrive o SharePoint Online en [Información general sobre el uso compartido externo](/sharepoint/external-sharing-overview). En general, el proceso difiere de Azure AD B2B de las siguientes maneras:

- OneDrive y SharePoint Online agregan usuarios al directorio después de que los usuarios han canjeado sus invitaciones. Por lo tanto, antes del canje, el usuario no se muestra en el portal de Azure AD. Si, mientras tanto, un usuario recibe una invitación de otro sitio, se genera una nueva invitación. Sin embargo, cuando se usa la colaboración B2B de Azure AD, los usuarios se agregan inmediatamente cuando se les invita por lo que se muestran en todas partes.

- La experiencia de canje en OneDrive y SharePoint Online parece diferente de la experiencia de colaboración B2B de Azure AD. Después de que un usuario canjea una invitación, las experiencias se asemejan.

- Los usuarios invitados a la colaboración B2B de Azure AD se pueden seleccionar en los cuadros de diálogo de uso compartido de OneDrive y SharePoint Online. Los usuarios invitados a OneDrive y SharePoint Online también se muestran en Azure AD después de que canjean sus invitaciones.

- Los requisitos de concesión de licencia son diferentes. Para más información sobre licencias, consulte [Concesión de licencias de Azure AD External Identities](./external-identities-pricing.md) y la [introducción al uso compartido externo de SharePoint Online](/sharepoint/external-sharing-overview).
Para administrar el uso compartido externo en OneDrive o SharePoint Online con la colaboración B2B de Azure AD, establezca la configuración de uso compartido externo de OneDrive o SharePoint Online en **Allow sharing only with the external users that already exist in your organization's directory** (Permitir uso compartido solo con los usuarios externos que ya existan en el directorio de la organización). Los usuarios pueden acceder a los sitios compartidos externamente y elegir entre colaboradores externos que haya agregado el administrador. El administrador puede agregar los colaboradores externos a través de la API de invitación de colaboración B2B.


![Configuración de uso compartido externo de OneDrive y SharePoint](media/o365-external-user/odsp-sharing-setting.png)

Después de habilitar el uso compartido externo, la posibilidad de buscar usuarios invitados existentes en el selector de personas de SharePoint Online (SPO) está desactivada de manera predeterminada para así coincidir con el comportamiento heredado.

Para habilitarla, use la configuración "ShowPeoplePickerSuggestionsForGuestUsers" en el nivel de inquilino y colección de sitios. Para configurarla, use los cmdlets Set-SPOTenant y Set-SPOSite, que permiten que los miembros busquen a todos los usuarios invitados existentes en el directorio. Los cambios en el ámbito del inquilino no afectan los sitios de SPO que ya se aprovisionaron.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
* [Incorporación de usuarios de colaboración B2B a un rol](add-guest-to-role.md)
* [Delegación de las invitaciones de colaboración B2B](delegate-invitations.md)
* [Grupos dinámicos y colaboración B2B](use-dynamic-groups.md)
* [Solución de problemas de colaboración B2B de Azure Active Directory](troubleshoot.md)
