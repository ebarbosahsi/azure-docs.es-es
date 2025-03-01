---
title: Recomendaciones y procedimientos recomendados de Azure Active Directory B2B
description: Obtenga información sobre los procedimientos recomendados y las recomendaciones para el acceso de usuarios invitados de negocio a negocio (B2B) en Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94fd488ceb7ddb3724dd576c97c9070481e95147
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100365640"
---
# <a name="azure-active-directory-b2b-best-practices"></a>Procedimientos recomendados de Azure Active Directory B2B
Este artículo contiene recomendaciones y procedimientos recomendados para la colaboración de negocio a negocio (B2B) en Azure Active Directory (Azure AD).

   > [!IMPORTANT]
   > **A partir de octubre de 2021**, Microsoft dejará de admitir el canje de invitaciones mediante la creación de inquilinos y cuentas de Azure AD no administrada ("virales" o "Just-In-Time") para escenarios de colaboración B2B. En ese momento, la característica de código de acceso de un solo uso por correo electrónico se activará para todos los inquilinos existentes, y se habilitará de forma predeterminada para los nuevos. Vamos a habilitar la característica de código de acceso de un solo uso por correo electrónico, ya que proporciona un método eficaz de autenticación de reserva para usuarios invitados. No obstante, puede deshabilitar esta característica si prefiere no utilizarla. Para más información, consulte [Autenticación con código de acceso de un solo uso por correo electrónico](one-time-passcode.md).


## <a name="b2b-recommendations"></a>Recomendaciones de B2B
| Recomendación | Comentarios |
| --- | --- |
| Para una experiencia de inicio de sesión óptima, realice la federación con proveedores de identidades. | Siempre que sea posible, realice la federación directamente con los proveedores de identidades, para permitir que los usuarios invitados inicien sesión en las aplicaciones y recursos compartidos sin tener que crear cuentas de Microsoft (MSA) o cuentas de Azure AD. Puede usar la [característica de federación de Google](google-federation.md) para permitir que los usuarios invitados de B2B inicien sesión con sus cuentas de Google. O bien, puede usar la [característica de federación directa (versión preliminar)](direct-federation.md) para configurar la federación directa con cualquier organización cuyo proveedor de identidades (IdP) admita los protocolos SAML 2.0 o WS-FED. |
| Uso de la característica de código de acceso de un solo uso por correo electrónico para invitados B2B que no se pueden autenticar por otros medios. | La característica de [código de acceso de un solo uso por correo electrónico](one-time-passcode.md) autentica los usuarios invitados de B2B cuando no pueden autenticarse por otros medios, como Azure AD, una cuenta de Microsoft (MSA) o la federación de Google. Cuando el usuario invitado canjea una invitación o accede a un recurso compartido, puede solicitar un código temporal, que se envía a su dirección de correo electrónico. A continuación, escribe este código para continuar con el inicio de sesión. |
| Adición de personalización de marca a la página de inicio de sesión | Puede personalizar la página de inicio de sesión de forma que resulte más intuitiva para los usuarios invitados de B2B. Consulte cómo [agregar personalización de marca de la empresa en las páginas de inicio de sesión y del Panel de acceso](../fundamentals/customize-branding.md). |
| Agregue la declaración de privacidad a la experiencia de canje del usuario invitado de B2B. | Puede agregar la dirección URL de la declaración de privacidad de la organización al proceso de canje de invitación por primera vez, de modo que un usuario invitado deba dar su consentimiento a los términos de privacidad para continuar. Consulte [ Incorporación de información de privacidad de su organización en Azure Active Directory](../fundamentals/active-directory-properties-area.md). |
| Use la característica de invitación en bloque (versión preliminar) para invitar a varios usuarios de B2B al mismo tiempo. | Invite a varios usuarios a su organización al mismo tiempo mediante la característica en versión preliminar de invitación en bloque de Azure Portal. Esta característica permite cargar un archivo CSV para crear usuarios invitados de B2B y enviar invitaciones en bloque. Consulte el [tutorial para invitar en bloque a usuarios de B2B](tutorial-bulk-invite.md). |
| Aplique directivas de acceso condicional para Multi-Factor Authentication (MFA). | Se recomienda aplicar las directivas de MFA en las aplicaciones que desee compartir con los usuarios de B2B de asociados. De este modo, MFA se aplicará de forma coherente en las aplicaciones del inquilino, independientemente de si la organización asociada usa MFA. Consulte [Acceso condicional para usuarios de colaboración B2B](conditional-access.md). |
| Si va a aplicar directivas de acceso condicional basado en el dispositivo, use listas de exclusión para permitir el acceso a los usuarios de B2B. | Si la organización tiene habilitadas directivas de acceso condicional basado en el dispositivo, los dispositivos de los usuarios invitados de B2B se bloquearán porque no están administrados por su organización. Puede crear listas de exclusión que contengan usuarios de asociados específicos para excluirlos de la directiva de acceso condicional basado en el dispositivo. Consulte [Acceso condicional para usuarios de colaboración B2B](conditional-access.md). |
| Use una dirección URL específica del inquilino cuando proporcione vínculos directos a los usuarios invitados de B2B. | Como alternativa a la invitación por correo electrónico, puede darle al invitado un vínculo directo a la aplicación o al portal. Este vínculo directo debe ser específico del cliente, por lo que debe incluir un identificador de inquilino o dominio comprobado, de manera que el invitado se pueda autenticar en el inquilino donde se encuentra la aplicación compartida. Consulte [Experiencia de invitación de colaboración B2B de Azure Active Directory](redemption-experience.md). |
| Al desarrollar una aplicación, utilice UserType para determinar la experiencia del usuario invitado.  | Si está desarrollando una aplicación y desea proporcionar diferentes experiencias para usuarios de inquilinos y usuarios invitados, use la propiedad UserType. La notificación UserType no está incluida actualmente en el token. Las aplicaciones deben usar Microsoft Graph API para consultar el usuario en el directorio y obtener su valor de UserType. |
| Cambie la propiedad UserType *solo* si cambia la relación del usuario con la organización. | Aunque es posible usar PowerShell para convertir la propiedad UserType de un usuario de miembro a invitado (y viceversa), solo debe cambiar esta propiedad si cambia la relación del usuario con la organización. Consulte [Propiedades de un usuario de colaboración B2B de Azure Active Directory](user-properties.md).|

## <a name="next-steps"></a>Pasos siguientes

[Habilitación de la colaboración externa B2B y administración de quién puede invitar a otros usuarios](delegate-invitations.md)