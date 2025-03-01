---
author: baanders
description: archivo de inclusión para los requisitos previos de permisos en la instalación de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: 919cc934920d922a82ceb9e30630006627af44e9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92205507"
---
## <a name="prerequisites-permission-requirements"></a>Requisitos previos: Requisitos de permisos

Para poder realizar todos los pasos de este artículo, debe tener un [rol en la suscripción](../articles/role-based-access-control/rbac-and-directory-admin-roles.md) que tenga los permisos siguientes:
* Crear y administrar recursos de Azure
* Administrar el acceso de los usuarios a los recursos de Azure (incluida la concesión y delegación de permisos)

Los roles comunes que cumplen este requisito son *propietario*, *administrador de cuentas* o la combinación de *administrador de acceso de usuarios* y *colaborador*. Puede encontrar una explicación completa de los roles y permisos, como, por ejemplo, los permisos que se incluyen con otros roles, en [*Roles de administrador de la suscripción clásica, roles de Azure y roles de Azure AD*](../articles/role-based-access-control/rbac-and-directory-admin-roles.md) en la documentación de RBAC de Azure.

Para ver el rol de la suscripción, visite la [página de suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en Azure Portal (puede usar este vínculo o buscar *Suscripciones* con la barra de búsqueda del portal). Busque el nombre de la suscripción que usa y vea el rol que tiene en la columna *Mi rol*:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/subscriptions-role.png" alt-text="Vista de la página Suscripciones en Azure Portal que muestra el usuario como propietario" lightbox="../articles/digital-twins/media/how-to-set-up-instance/portal/subscriptions-role.png":::

Si observa que el valor es *Colaborador* u otro rol que no tenga los permisos necesarios descritos anteriormente, puede ponerse en contacto con el usuario de la suscripción que *tenga* estos permisos (por ejemplo, propietario de la suscripción o administrador de cuentas) y continuar de una de las siguientes maneras:
* Solicite que realicen los pasos de este artículo en su nombre.
* Solicite que eleven los privilegios de su rol en la suscripción para que tenga los permisos necesarios para continuar por su cuenta. Que esto sea adecuado depende de la organización y del rol que tenga.