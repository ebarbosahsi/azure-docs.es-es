---
title: Autenticación en Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga información sobre las distintas formas en que una aplicación o un servicio pueden autenticarse en Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 0146ff9ce3ec4821bee7ce34700ca4198bb23ddc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104598871"
---
# <a name="authenticate-to-azure-communication-services"></a>Autenticación en Azure Communication Services

Se debe autenticar cada interacción del cliente con Azure Communication Services. En una arquitectura típica, vea [Arquitectura de cliente y servidor](./client-and-server-architecture.md); para la autenticación, se usan *claves de acceso* o *identidades administradas*.

Otro tipo de autenticación usa *tokens de acceso de usuario* para autenticarse en los servicios que requieren la participación de los usuarios. Por ejemplo, el servicio de chat o de llamada utiliza los *tokens de acceso de usuario* para permitir que los usuarios se agreguen a un subproceso y tengan conversaciones entre sí.

## <a name="authentication-options"></a>Opciones de autenticación

En la tabla siguiente se muestran las bibliotecas cliente de Azure Communication Services y sus opciones de autenticación:

| Biblioteca cliente    | Opciones de autenticación.                               |
| ----------------- | ----------------------------------------------------|
| Identidad          | Clave de acceso o identidad administrada                      |
| SMS               | Clave de acceso o identidad administrada                      |
| Números de teléfono     | Clave de acceso o identidad administrada                      |
| Llamar           | Token de acceso de usuario                                   |
| Chat              | Token de acceso de usuario                                   |

Cada opción de autorización se describe brevemente a continuación:

### <a name="access-key"></a>Clave de acceso

La autenticación con clave de acceso es adecuada para las aplicaciones de servicio que se ejecutan en un entorno de servicio de confianza. La clave de acceso se puede encontrar en el portal de Azure Communication Services. La aplicación de servicio la usa como credencial para inicializar las bibliotecas cliente correspondientes. Vea un ejemplo de cómo se usa en la [biblioteca cliente de identidades](../quickstarts/access-tokens.md). 

Como la clave de acceso forma parte de la cadena de conexión del recurso, la autenticación con una cadena de conexión equivale a la autenticación con una clave de acceso.

Si desea llamar a las API de ACS manualmente mediante una clave de acceso, deberá firmar la solicitud. La firma de la solicitud se explica con detalle en este [tutorial](../tutorials/hmac-header-tutorial.md).

### <a name="managed-identity"></a>Identidad administrada

Las identidades administradas proporcionan una mayor seguridad y facilidad de uso con otras opciones de autorización. Por ejemplo, con Azure AD evita tener que almacenar la clave de acceso de la cuenta con el código, como se hace con la autorización mediante la clave de acceso. Aunque puede seguir usando la autorización mediante la clave de acceso con las aplicaciones de Communication Services, Microsoft recomienda cambiar a Azure AD siempre que sea posible. 

Para configurar una identidad administrada, [cree una aplicación registrada a partir de la CLI de Azure](../quickstarts/managed-identity-from-cli.md). A continuación, el punto de conexión y las credenciales se pueden usar para autenticar las bibliotecas cliente. Vea ejemplos de cómo se usa la [identidad administrada](../quickstarts/managed-identity.md).

### <a name="user-access-tokens"></a>Tokens de acceso de usuario

Los tokens de acceso de usuario se generan mediante la biblioteca cliente de identidades y se asocian a los usuarios creados en la biblioteca cliente de identidades. Vea un ejemplo de cómo [crear usuarios y generar tokens](../quickstarts/access-tokens.md). A continuación, los tokens de acceso de usuario se usan para autenticar a los participantes agregados a las conversaciones en el SDK de chat y llamadas. Para más información, vea [Incorporación de chat a una aplicación](../quickstarts/chat/get-started.md). La autenticación con token de acceso de usuario difiere de la autenticación con clave de acceso e identidad administrada, ya que se usa para autenticar a un usuario en lugar de a un recurso seguro de Azure.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación y administración de recursos de Communication Services](../quickstarts/create-communication-resource.md)
> [Creación de una aplicación de identidad administrada de Azure Active Directory mediante la CLI de Azure](../quickstarts/managed-identity-from-cli.md)
> [Creación de tokens de acceso de usuario](../quickstarts/access-tokens.md)

Para más información, consulte los siguientes artículos.
- [Información sobre la arquitectura de cliente y servidor](../concepts/client-and-server-architecture.md)
