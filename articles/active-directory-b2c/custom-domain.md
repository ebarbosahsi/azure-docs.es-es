---
title: Habilitación de dominios personalizados de Azure AD B2C
titleSuffix: Azure AD B2C
description: Aprenda a habilitar dominios personalizados en las direcciones URL de redireccionamiento para Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/17/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 2de419885938b27ebce4a934db5ef966965b3dbd
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104580171"
---
# <a name="enable-custom-domains-for-azure-active-directory-b2c"></a>Habilitación de dominios personalizados para Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

En este artículo se describe cómo habilitar dominios personalizados en las direcciones URL de redireccionamiento de Azure Active Directory B2C (Azure AD B2C). El uso de un dominio personalizado con la aplicación proporciona una experiencia de usuario más fluida. Desde la perspectiva del usuario, permanecen en el dominio durante el proceso de inicio de sesión, en lugar de redirigirse al dominio predeterminado de Azure AD B2C *<nombre-de-inquilino>.b2clogin.com*.

![Experiencia de usuario de dominio personalizado](./media/custom-domain/custom-domain-user-experience.png)

## <a name="custom-domain-overview"></a>Información general sobre dominios personalizados

Puede habilitar dominios personalizados para Azure AD B2C mediante [Azure Front Door](https://azure.microsoft.com/services/frontdoor/). Azure Front Door es un punto de entrada global y escalable que usa la red perimetral global de Microsoft para crear aplicaciones web rápidas, seguras y muy escalables. Puede representar el contenido de Azure AD B2C detrás de Azure Front Door y, después, configurar una opción en Azure Front Door para ofrecer el contenido a través de un dominio personalizado en la dirección URL de la aplicación.

En el diagrama siguiente se muestra la integración de Azure Front Door:

1. Desde una aplicación, un usuario hace clic en el botón de inicio de sesión, que le lleva a la página de inicio de sesión de Azure AD B2C. Esta página especifica un nombre de dominio personalizado.
1. El explorador web resuelve el nombre de dominio personalizado en la dirección IP de Azure Front Door. Durante la resolución DNS, un registro de nombre canónico (CNAME) con un nombre de dominio personalizado apunta al host de front-end predeterminado de Front Door (por ejemplo, `contoso.azurefd.net`). 
1. El tráfico dirigido al dominio personalizado (por ejemplo, `login.contoso.com`) se enruta al host de front-end predeterminado de Front Door especificado (`contoso.azurefd.net`).
1. Azure Front Door invoca el contenido de Azure AD B2C mediante el dominio predeterminado de Azure AD B2C `<tenant-name>.b2clogin.com`. La solicitud al punto de conexión de Azure AD B2C incluye un encabezado HTTP personalizado que contiene el nombre de dominio personalizado original.
1. Azure AD B2C responde a la solicitud mostrando el contenido pertinente y el dominio personalizado original.

![Diagrama de redes de dominio personalizado](./media/custom-domain/custom-domain-network-flow.png)

> [!IMPORTANT]
> La conexión desde el explorador a Azure Front Door siempre debe utilizar IPv4 en lugar de IPv6.

Al utilizar dominios personalizados, tenga en cuenta lo siguiente:

- Puede configurar varios dominios personalizados. Para conocer el número máximo de dominios personalizados admitidos, consulte [Restricciones y límites del servicio Azure AD](../active-directory/enterprise-users/directory-service-limits-restrictions.md) para Azure AD B2C y [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-front-door-service-limits) para Azure Front Door.
- Azure Front Door es un servicio de Azure independiente, por lo que se aplicarán cargos adicionales. Para más información, consulte [Precios de Azure Front Door](https://azure.microsoft.com/pricing/details/frontdoor).
- Para utilizar el [firewall de aplicaciones web](../web-application-firewall/afds/afds-overview.md) en Azure Front Door, debe confirmar que la configuración y las reglas del firewall funcionen correctamente con los flujos de usuario de Azure AD B2C.
- Después de configurar dominios personalizados, los usuarios seguirán teniendo acceso al nombre de dominio predeterminado de Azure AD B2C *<nombre de inquilino>.b2clogin.com* (a menos que esté utilizando una directiva personalizada y [bloquee el acceso](#block-access-to-the-default-domain-name)).
- Si tiene varias aplicaciones, migre todas al dominio personalizado porque el explorador almacena la sesión de Azure AD B2C con el nombre de dominio que se está utilizando actualmente.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


## <a name="add-a-custom-domain-name-to-your-tenant"></a>Incorporación de nombres de dominio personalizados al inquilino

Siga las instrucciones para [agregar y asegurarse de su dominio personalizado en Azure AD](../active-directory/fundamentals/add-custom-domain.md). Una vez comprobado el dominio, elimine el registro TXT de DNS creado.

> [!IMPORTANT]
> En estos pasos, asegúrese de iniciar sesión en el inquilino de **Azure AD B2C** y seleccione el servicio **Azure Active Directory**.

Compruebe cada subdominio que vaya a utilizar. Comprobar solo el dominio de nivel superior no es suficiente. Por ejemplo, para poder iniciar sesión con *login.contoso.com* y *account.contoso.com*, debe comprobar ambos subdominios y no solo el dominio de nivel superior *contoso.com*.  

## <a name="create-a-new-azure-front-door-instance"></a>Creación de una nueva instancia de Azure Front Door

Siga los pasos a fin de [crear una instancia de Front Door para su aplicación](../frontdoor/quickstart-create-front-door.md#create-a-front-door-for-your-application) con la configuración predeterminada para el host de front-end y las reglas de enrutamiento. 

> [!IMPORTANT]
> En estos pasos, después de iniciar sesión en Azure Portal en el paso 1, seleccione **Directorio + suscripción** y elija el directorio que contiene la suscripción de Azure que desee usar para Azure Front Door. *No* debe ser el directorio que contiene el inquilino de Azure AD B2C. 

En el paso **Agregar un back-end**, utilice la siguiente configuración:

* En **Tipo de host de back-end**, seleccione **Host personalizado**.  
* En **Nombre de host de back-end**, seleccione el nombre de host para el punto de conexión de Azure AD B2C, <nombre-de-inquilino>.b2clogin.com. Por ejemplo, contoso.b2clogin.com. 
* En **Encabezado host de back-end**, seleccione el mismo valor que ha seleccionado para **Nombre de host de back-end**.

![Incorporación de un back-end](./media/custom-domain/add-a-backend.png)

Después de agregar el **back-end** al **grupo de back-end**, deshabilite los **sondeos de estado**.

![Adición de un grupo de back-end](./media/custom-domain/add-a-backend-pool.png)

## <a name="set-up-your-custom-domain-on-azure-front-door"></a>Configuración del dominio personalizado en Azure Front Door

Siga los pasos para [agregar un dominio personalizado a Front Door](../frontdoor/front-door-custom-domain.md). Al crear el registro `CNAME` para el dominio personalizado, use el nombre de dominio personalizado que ha comprobado anteriormente en el paso [Incorporación de un nombre de dominio personalizado a Azure AD](#add-a-custom-domain-name-to-your-tenant). 

Una vez comprobado el nombre de dominio personalizado, seleccione **Custom domain name HTTPS** (Nombre de dominio personalizado HTTPS). Después, en **Tipo de administración de certificados**, seleccione [Administración de Front Door](../frontdoor/front-door-custom-domain-https.md#option-1-default-use-a-certificate-managed-by-front-door) o [Usar mi propio certificado](../frontdoor/front-door-custom-domain-https.md#option-2-use-your-own-certificate). 

En la captura de pantalla siguiente se muestra cómo agregar un dominio personalizado y habilitar HTTPS mediante un certificado de Azure Front Door.

![Configuración de un dominio personalizado de Azure Front Door](./media/custom-domain/azure-front-door-add-custom-domain.png) 

## <a name="configure-cors"></a>Configuración de CORS

Si [personaliza la interfaz de usuario de Azure AD B2C](customize-ui-with-html.md) con una plantilla HTML, debe [configurar CORS](customize-ui-with-html.md?pivots=b2c-user-flow.md#3-configure-cors) con su dominio personalizado.

Para configurar Azure Blob Storage para el uso compartido de recursos entre orígenes (CORS), realice los siguientes pasos:

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.
1. En el menú, seleccione **CORS**.
1. En **Orígenes permitidos**, escriba `https://your-domain-name`. Reemplace `your-domain-name` por el nombre de dominio. Por ejemplo, `https://login.contoso.com`. Al escribir su nombre de inquilino, use solo minúsculas.
1. En **Métodos permitidos**, seleccione `GET` y `OPTIONS`.
1. En **Encabezados permitidos**, escriba un asterisco (*).
1. En **Encabezados expuestos**, escriba un asterisco (*).
1. Para **Antigüedad máxima**, introduzca 200.
1. Seleccione **Guardar**.

## <a name="configure-your-identity-provider"></a>Configuración del proveedor de identidades

Cuando un usuario elige iniciar sesión con un proveedor de identidades sociales, Azure AD B2C inicia una solicitud de autorización y lleva al usuario al proveedor de identidades seleccionado para completar el proceso de inicio de sesión. La solicitud de autorización especifica `redirect_uri` con el nombre de dominio predeterminado de Azure AD B2C: 

```http
https://<tenant-name>.b2clogin.com/<tenant-name>/oauth2/authresp
```

Si ha configurado la directiva para permitir el inicio de sesión con un proveedor de identidades externo, actualice los URI de redirección de OAuth con el dominio personalizado. La mayoría de los proveedores de identidades permiten registrar varios URI de redirección. Se recomienda agregar los URI de redirección en lugar de reemplazarlos para poder probar la directiva personalizada sin que afecte a las aplicaciones que utilizan el nombre de dominio predeterminado de Azure AD B2C. 

En el siguiente URI de redirección:

```http
https://<custom-domain-name>/<tenant-name>/oauth2/authresp
``` 

- Reemplace **<nombre-de-dominio-personalizado>** por el nombre de dominio personalizado.
- Especifique el **<nombre-de-inquilino>** con el nombre del inquilino o el identificador del inquilino.

En el ejemplo siguiente se muestra un URI de redirección de OAuth válido:

```http
https://login.contoso.com/contoso.onmicrosoft.com/oauth2/authresp
```

Si decide usar el [identificador de inquilino](#optional-use-tenant-id), un URI de redirección de OAuth válido tendría un aspecto similar al siguiente:

```http
https://login.contoso.com/11111111-1111-1111-1111-111111111111/oauth2/authresp
```

Los metadatos de los [proveedores de identidades de SAML](saml-identity-provider-technical-profile.md) tendrían el siguiente aspecto:

```http
https://<custom-domain-name>.b2clogin.com/<tenant-name>/<your-policy>/samlp/metadata?idptp=<your-technical-profile>
```

## <a name="test-your-custom-domain"></a>Prueba de un dominio personalizado

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Flujos de usuario (directivas)** .
1. Seleccione un flujo de usuario y luego **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Copiar al portapapeles**.

    ![Copia del URI de solicitud de autorización](./media/custom-domain/user-flow-run-now.png)

1. En la dirección URL **Ejecutar punto de conexión de flujo de usuario**, reemplace el dominio de Azure AD B2C (<nombre-de-inquilino>.b2clogin.com) por el dominio personalizado.  
    Por ejemplo, en lugar de:

    ```http
    https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1_susi&client_id=63ba0d17-c4ba-47fd-89e9-31b3c2734339&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login
    ```

    utilice:

    ```http
    https://login.contoso.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1_susi&client_id=63ba0d17-c4ba-47fd-89e9-31b3c2734339&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login    
    ```
1. Seleccione **Ejecutar flujo de usuario**. Se debería cargar la directiva de Azure AD B2C.
1. Inicie sesión tanto con la cuenta local como social.
1. Repita la prueba con el resto de las directivas.

## <a name="configure-your-application"></a>Configuración de la aplicación 

Después de configurar y probar el dominio personalizado, puede actualizar las aplicaciones para cargar la dirección URL que especifica su dominio personalizado como nombre de host en lugar del dominio de Azure AD B2C. 

La integración del dominio personalizado se aplica a los puntos de conexión de autenticación que usan directivas de Azure AD B2C (flujos de usuario o directivas personalizadas) para autenticar a los usuarios. Estos puntos de conexión pueden parecerse a lo siguiente:

- <code>https://\<custom-domain\>/\<tenant-name\>/<b>\<policy-name\></b>/v2.0/.well-known/openid-configuration</code>

- <code>https://\<custom-domain\>/\<tenant-name\>/<b>\<policy-name\></b>/oauth2/v2.0/authorize</code>

- <code>https://\<custom-domain\>/<tenant-name\>/<b>\<policy-name\></b>/oauth2/v2.0/token</code>

Sustituya:
- **dominio-personalizado** por el nombre de dominio personalizado.
- **nombre-de-inquilino** por el nombre o el identificador de inquilino.
- **nombre-de-directiva** por el nombre de la directiva. [Obtenga más información sobre las directivas de Azure AD B2C](technical-overview.md#identity-experiences-user-flows-or-custom-policies). 


Los metadatos del [proveedor de servicios de SAML](./saml-service-provider.md) pueden tener un aspecto similar al siguiente: 

```html
https://custom-domain-name/tenant-name/policy-name/Samlp/metadata
```

### <a name="optional-use-tenant-id"></a>(Opcional) Uso del identificador de inquilino

Puede reemplazar el nombre del inquilino de B2C en la dirección URL por el GUID del identificador de inquilino para quitar todas las referencias a "b2c" en la dirección URL. Puede encontrar el GUID del identificador de inquilino en la página Información general de B2C en Azure Portal.
Por ejemplo, cambie `https://account.contosobank.co.uk/contosobank.onmicrosoft.com/` a `https://account.contosobank.co.uk/<tenant ID GUID>/`.

Si decide utilizar el identificador de inquilino en lugar del nombre de inquilino, asegúrese de actualizar los **URI de redirección de OAuth** del proveedor de identidades en consecuencia. Para más información, consulte [Configuración del proveedor de identidades](#configure-your-identity-provider).

### <a name="token-issuance"></a>Emisión de token

La notificación del nombre del emisor del token (ISS) cambia en función del dominio personalizado que se utiliza. Por ejemplo:

```http
https://<domain-name>/11111111-1111-1111-1111-111111111111/v2.0/
```

::: zone pivot="b2c-custom-policy"

## <a name="block-access-to-the-default-domain-name"></a>Bloquear el acceso al nombre de dominio predeterminado

Después de agregar el dominio personalizado y configurar la aplicación, los usuarios podrán seguir accediendo al dominio <nombre-de-inquilino>.b2clogin.com. Para impedir el acceso, puede configurar la directiva para comprobar la solicitud de autorización "nombre de host" en una lista de dominios permitidos. El nombre de host es el nombre de dominio que aparece en la dirección URL. El nombre de host está disponible mediante [solucionadores de notificaciones](claim-resolver-overview.md) `{Context:HostName}`. A continuación, puede presentar un mensaje de error personalizado. 

1. Obtenga el ejemplo de una directiva de acceso condicional que comprueba el nombre de host de [GitHub](https://github.com/azure-ad-b2c/samples/blob/master/policies/check-host-name).
1. En cada archivo, reemplace la cadena `yourtenant` por el nombre del inquilino de Azure AD B2C. Por ejemplo, si el nombre del inquilino de B2C es *contosob2c*, todas las instancias de `yourtenant.onmicrosoft.com` se convierten en `contosob2c.onmicrosoft.com`.
1. Cargue los archivos de directivas en el orden siguiente: `B2C_1A_TrustFrameworkExtensions_HostName.xml` y luego `B2C_1A_signup_signin_HostName.xml`.

::: zone-end

## <a name="troubleshooting"></a>Solución de problemas

### <a name="azure-ad-b2c-returns-a-page-not-found-error"></a>Azure AD B2C devuelve un error de página no encontrada

- **Síntoma**: después de configurar un dominio personalizado, cuando intenta iniciar sesión con el dominio personalizado, recibe un mensaje de error HTTP 404.
- **Causas posibles**: este problema puede estar relacionado con la configuración de DNS o la configuración de back-end de Azure Front Door. 
- **Solución:**  
    1. Asegúrese de que el dominio personalizado esté [registrado y comprobado correctamente](#add-a-custom-domain-name-to-your-tenant) en el inquilino de Azure AD B2C.
    1. Asegúrese de que el [dominio personalizado](../frontdoor/front-door-custom-domain.md) esté correctamente configurado. El registro `CNAME` del dominio personalizado debe apuntar al host de front-end predeterminado de Azure Front Door (por ejemplo, contoso.azurefd.net).
    1. Asegúrese de que la [configuración del grupo de back-end de Azure Front Door](#set-up-your-custom-domain-on-azure-front-door) apunte al inquilino donde haya configurado el nombre de dominio personalizado y donde se almacenen el flujo de usuario o las directivas personalizadas.

### <a name="identify-provider-returns-an-error"></a>El proveedor de identidades devuelve un error

- **Síntoma**: después de configurar un dominio personalizado, podrá iniciar sesión con las cuentas locales. Sin embargo, cuando inicia sesión con credenciales de [proveedores de identidades sociales o empresariales](add-identity-provider.md) externos, los proveedores de identidades presentan un mensaje de error.
- **Causas posibles**: cuando Azure AD B2C toma el usuario para iniciar sesión con un proveedor de identidades federado, especifica el URI de redirección. El URI de redirección es el punto de conexión en el que el proveedor de identidades devuelve el token. El URI de redirección es el mismo dominio que utiliza la aplicación con la solicitud de autorización. Si el URI de redirección no está aún registrado en el proveedor de identidades, es posible que no confíe en el nuevo URI de redirección, lo que genera un mensaje de error. 
- **Solución**: siga los pasos descritos en [Configuración del proveedor de identidades](#configure-your-identity-provider) para agregar el nuevo URI de redirección. 


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-use-azure-front-door-advanced-configuration-such-as-web-application-firewall-rules"></a>¿Puedo usar la configuración avanzada de Azure Front Door, como las *reglas de firewall de aplicaciones web*? 
  
Aunque las opciones de configuración avanzada de Azure Front Door no se admiten oficialmente, puede utilizarlas bajo su responsabilidad. 

### <a name="when-i-use-run-now-to-try-to-run-my-policy-why-i-cant-see-the-custom-domain"></a>Cuando utilizo Ejecutar ahora para intentar ejecutar mi directiva, ¿por qué no veo el dominio personalizado?

Copie la dirección URL, cambie el nombre de dominio manualmente y péguelo de nuevo en el explorador.

### <a name="which-ip-address-is-presented-to-azure-ad-b2c-the-users-ip-address-or-the-azure-front-door-ip-address"></a>¿Qué dirección IP se presenta para Azure AD B2C? ¿La dirección IP del usuario o la dirección IP de Azure Front Door?

Azure Front Door pasa la dirección IP original del usuario. Esta es la dirección IP que verá en los informes de auditoría o en la directiva personalizada.

### <a name="can-i-use-a-third-party-web-application-firewall-waf-with-b2c"></a>¿Puedo utilizar un firewall de aplicaciones web (WAF) de terceros con B2C?

Para usar su propio firewall de aplicaciones web delante de Azure Front Door, debe configurar y asegurarse de que todo funciona correctamente con los flujos de usuario de Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [solicitudes de autorización de OAuth](protocols-overview.md).