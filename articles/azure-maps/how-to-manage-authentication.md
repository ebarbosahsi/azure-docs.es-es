---
title: Administración de la autenticación
titleSuffix: Azure Maps
description: Familiarícese con la autenticación de Azure Maps. Vea qué enfoque funciona mejor en cada escenario. Aprenda a usar el portal para ver la configuración de autenticación.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/12/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 955b541bdb4ae38066f1eb4d2f09363ec51be1d2
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104864081"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, se crean las claves y el identificador de cliente para admitir la autenticación de clave compartida y la autenticación de Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Después de crear la cuenta de Azure Maps, se generan las claves principal y secundaria. Es recomendable que use la clave principal como clave de suscripción cuando [llame a Azure Maps mediante la autenticación de clave compartida](./azure-maps-authentication.md#shared-key-authentication). Puede usar la clave secundaria, por ejemplo, para realizar cambios de clave graduales. Para más información, consulte [Autenticación con Azure Maps](./azure-maps-authentication.md).

Puede ver los datos de autenticación en Azure Portal. Una vez allí, vaya a su cuenta y seleccione **Autenticación** en el menú **Configuración**.

> [!div class="mx-imgBorder"]
> ![Detalles de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)

## <a name="discover-category-and-scenario"></a>Detección de categoría y escenario

Dependiendo de las necesidades de la aplicación, hay rutas específicas para protegerla. Azure AD define categorías para admitir una amplia gama de flujos de autenticación. Consulte las [categorías de aplicación](../active-directory/develop/authentication-flows-app-scenarios.md#application-categories) para saber qué categoría se adapta a la aplicación.

> [!NOTE]
> Incluso si usa la autenticación de clave compartida, la descripción de las categorías y los escenarios le ayudará a proteger la aplicación.

## <a name="determine-authentication-and-authorization"></a>Definición de la autenticación y autorización

En la tabla siguiente se describen los escenarios comunes de autenticación y autorización en Azure Maps. La tabla proporciona una comparación de los tipos de protección que ofrece cada escenario.

> [!IMPORTANT]
> Microsoft recomienda implementar Azure Active Directory (Azure AD) con el control de acceso basado en roles (Azure RBAC) para las aplicaciones de producción.

| Escenario                                                                                    | Authentication | Authorization | Esfuerzo para desarrollo | Esfuerzo operativo |
| ------------------------------------------------------------------------------------------- | -------------- | ------------- | ------------------ | ------------------ |
| [Demonio de confianza o aplicación cliente no interactiva](./how-to-secure-daemon-app.md)        | Clave compartida     | N/D           | Media             | Alto               |
| [Demonio de confianza o aplicación cliente no interactiva](./how-to-secure-daemon-app.md)        | Azure AD       | Alto          | Bajo                | Media             |
| [Aplicación de una sola página web con inicio de sesión único interactivo](./how-to-secure-spa-users.md) | Azure AD       | Alto          | Media             | Media             |
| [Aplicación de una sola página web con inicio de sesión no interactivo](./how-to-secure-spa-app.md)      | Azure AD       | Alto          | Media             | Media             |
| [Aplicación web con inicio de sesión único interactivo](./how-to-secure-webapp-users.md)          | Azure AD       | Alto          | Alto               | Media             |
| [Dispositivo IoT/dispositivo restringido de entrada](./how-to-secure-device-code.md)                     | Azure AD       | Alto          | Media             | Media             |

Los vínculos de la tabla muestran información de configuración detallada para cada escenario.

## <a name="view-role-definitions"></a>Visualización de definiciones de roles

Para ver los roles de Azure que están disponibles en Azure Maps, vaya al **Control de acceso (IAM)** . Seleccione **Roles** y busque los roles que comienzan con *Azure Maps*. Esos son los roles de Azure Maps a los que puede conceder acceso.

> [!div class="mx-imgBorder"]
> ![Visualización de roles disponibles](./media/how-to-manage-authentication/how-to-view-avail-roles.png)

## <a name="view-role-assignments"></a>Visualización de asignaciones de roles

Para ver los usuarios y las aplicaciones a los que se ha concedido acceso para Azure Maps, vaya a **Control de acceso (IAM)** . Una vez allí, seleccione **Asignaciones de rol** y filtre los resultados por **Azure Maps**.

> [!div class="mx-imgBorder"]
> ![Visualización de los usuarios y aplicaciones a los que se les ha concedido acceso](./media/how-to-manage-authentication/how-to-view-amrbac.png)

## <a name="request-tokens-for-azure-maps"></a>Solicitud de tokens de Azure Maps

Solicite un token del punto de conexión de token de Azure AD. Use los siguientes datos en la solicitud de Azure AD:

| Entorno de Azure      | Punto de conexión del token de Azure AD             | Identificador de recurso de Azure              |
| ---------------------- | ----------------------------------- | ------------------------------ |
| Nube pública de Azure     | `https://login.microsoftonline.com` | `https://atlas.microsoft.com/` |
| Nube de Azure Government | `https://login.microsoftonline.us`  | `https://atlas.microsoft.com/` |

Para obtener más información sobre la solicitud de tokens de acceso de Azure AD para usuarios y entidades de servicio, consulte [Escenarios de autenticación de Azure AD](../active-directory/develop/authentication-vs-authorization.md) y consulte los escenarios específicos en la tabla [Escenarios](./how-to-manage-authentication.md#determine-authentication-and-authorization).

## <a name="manage-and-rotate-shared-keys"></a>Administración y giro de las claves compartidas

Las claves de suscripción de Azure Maps son similares a la contraseña raíz de la cuenta de Azure Maps. Tenga en cuenta que siempre debe proteger las claves de la suscripción. Use Azure Key Vault para administrar y rotar las claves de forma segura. Evite distribuirlas a otros usuarios, codificarlas de forma rígida o guardarlas en un archivo de texto sin formato al que puedan acceder otros usuarios. Rote sus claves si cree que se han puesto en peligro.

> [!NOTE]
> Microsoft recomienda el uso de Azure Active Directory (Azure AD) para autorizar solicitudes, si es posible, en lugar de la clave compartida. Azure AD proporciona una mayor seguridad y facilidad de uso a través de la clave compartida.

### <a name="manually-rotate-subscription-keys"></a>Rotación manual de las claves de suscripción

Microsoft recomienda rotar las claves de suscripción periódicamente para ayudar a proteger la cuenta de Azure Maps. De ser posible, use Azure Key Vault para administrar las claves de acceso. Si no está usando Key Vault, deberá rotar las claves manualmente.

Para poder rotar las claves, se asignan dos claves de suscripción. De este modo, se garantiza que la aplicación mantiene el acceso a Azure Maps a lo largo del proceso.

Para rotar las claves de suscripción de Azure Maps en Azure Portal:

1. Actualice el código de aplicación para hacer referencia a la clave secundaria de la cuenta de Azure Maps e implementarla.
2. Vaya a la cuenta de Azure Maps en [Azure Portal](https://portal.azure.com/).
3. En **Configuración**, seleccione **Autenticación**.
4. Para regenerar la clave principal de la cuenta de Azure Maps, seleccione el botón **Regenerar** que se encuentra junto a la clave principal.
5. Actualice el código de aplicación para hacer referencia a la nueva clave principal e impleméntela.
6. Vuelva a generar la clave secundaria de la misma forma.

> [!WARNING]
> Microsoft recomienda usar solo una de las claves en todas las aplicaciones al mismo tiempo. Si utiliza la "Clave 1" en algunos lugares y la "Clave 2" en otros, no podrá rotar las claves sin que alguna aplicación pierda el acceso.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte el artículo sobre el [SDK web de Azure AD y Azure Maps](./how-to-use-map-control.md).

Descubra las métricas de uso de API de la cuenta de Azure Maps:
> [!div class="nextstepaction"]
> [Visualización de métricas de uso](how-to-view-api-usage.md)

Explore ejemplos acerca de cómo integrar Azure AD con Azure Maps:

> [!div class="nextstepaction"]
> [Muestras de autenticación de Azure AD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)