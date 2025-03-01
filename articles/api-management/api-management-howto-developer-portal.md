---
title: Información general del portal para desarrolladores en Azure API Management
titleSuffix: Azure API Management
description: Obtenga información sobre el portal para desarrolladores de API Management, un sitio web personalizable, donde los consumidores de API pueden explorarlas.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/15/2020
ms.author: apimpm
ms.openlocfilehash: 30487218fc95be75d22b5a9ea5a6dbc224ffd025
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93074804"
---
# <a name="overview-of-the-developer-portal"></a>Información general del portal para desarrolladores

El portal para desarrolladores es un sitio web totalmente personalizable que se genera automáticamente con la documentación de las API. Aquí, los consumidores de API pueden detectar las API, aprender a usarlas, solicitar acceso y probarlas.

En este artículo se describen las diferencias entre las versiones autohospedadas y administradas del portal para desarrolladores de API Management. Asimismo, también proporciona respuestas a preguntas frecuentes.

![API Management developer portal](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="migration-from-the-legacy-portal"></a>Migración desde el portal heredado

> [!IMPORTANT]
> El portal para desarrolladores heredado está en desuso y solo recibirá actualizaciones de seguridad. Puede seguir utilizándolo como de costumbre hasta su retirada en octubre de 2023; entonces se quitará de todos los servicios de API Management.

La migración al nuevo portal para desarrolladores se describe en el [artículo de documentación dedicada](developer-portal-deprecated-migration.md).

## <a name="customization-and-styling"></a>Personalización y aplicación de estilos

El portal para desarrolladores se puede personalizar y puede aplicarle un estilo mediante el editor visual integrado para arrastrar y colocar elementos. Consulte [este tutorial](api-management-howto-developer-portal-customize.md) para obtener más información.

## <a name="extensibility"></a><a name="managed-vs-self-hosted"></a> Extensibilidad

El servicio de API Management incluye un portal para desarrolladores integrado y **administrado** que siempre está actualizado. Puede obtener acceso al mismo desde la interfaz de Azure Portal.

Si necesita ampliarlo con una lógica personalizada que no se admite de forma integrada, puede modificar su código base. El código base del portal está [disponible en un repositorio de GitHub][1]. Por ejemplo, puede implementar un nuevo widget que se integra con un sistema de soporte de terceros. Al implementar una nueva funcionalidad, puede elegir una de las siguientes opciones:

- **Autohospedaje** del portal resultante fuera del servicio API Management. Cuando se autohospeda el portal, se convierte en su administrador y será responsable de sus actualizaciones. La ayuda del Soporte técnico de Azure solo está limitada a la configuración básica de portales autohospedados, tal como se detalla en la [sección Wiki del repositorio][2].
- Abra una solicitud de incorporación de cambios para que el equipo de API Management combine la nueva funcionalidad con el código base del portal **administrado**.

Para obtener información detallada e instrucciones de extensibilidad, consulte el [repositorio de GitHub][1] y [los tutoriales para la implementación de un widget][3]. En el [tutorial para personalizar el portal administrado](api-management-howto-developer-portal-customize.md) obtendrá una serie de pasos que le ayudarán a usar el panel administrativo del portal, que es común para las versiones **administradas** y **autohospedadas**.

## <a name="frequently-asked-questions"></a><a name="faq"></a> Preguntas más frecuentes

En esta sección, respondemos preguntas frecuentes generales sobre el portal para desarrolladores. Si tiene preguntas específicas de la versión autohospedada, consulte la [sección wiki del repositorio de GitHub](https://github.com/Azure/api-management-developer-portal/wiki).

### <a name="how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"></a> ¿Cómo puedo migrar desde la versión preliminar del portal?

Cuando se inicia por primera vez la versión preliminar del portal para desarrolladores, se aprovisiona el contenido predeterminado de la versión preliminar en el servicio API Management. El contenido predeterminado se ha modificado significativamente en la versión de disponibilidad general. Por ejemplo, la versión preliminar del contenido predeterminado no incluye los botones de OAuth en las páginas de inicio de sesión, usa widgets diferentes para mostrar las API y se basa en funcionalidades limitadas para estructurar las páginas del portal para desarrolladores. Aunque hay diferencias en el contenido, el motor del portal (incluidos los widgets subyacentes) se actualiza automáticamente cada vez que se publica el portal para desarrolladores.

Si ha personalizado en gran medida el portal en función de la versión preliminar del contenido, puede seguir con su uso tal cual y colocar nuevos widgets manualmente en las páginas del portal. De lo contrario, se recomienda reemplazar el contenido del portal con el nuevo contenido predeterminado.

Para restablecer el contenido en un portal administrado, seleccione **Restablecer contenido** en la sección **Operaciones** del menú. Esta operación eliminará todo el contenido del portal y aprovisionará el nuevo contenido predeterminado. Perderá todas las personalizaciones y los cambios del portal para desarrolladores. **No es posible deshacer esta acción**.

![Restablecimiento del contenido del portal](media/api-management-howto-developer-portal/reset-content.png)

Si usa la versión autohospedada, ejecute los scripts `scripts.v2/cleanup.bat` y `scripts.v2/generate.bat` del repositorio de GitHub para eliminar el contenido existente y aprovisionar el nuevo. Asegúrese de actualizar con antelación el código del portal a la versión más reciente del repositorio de GitHub.

Si ha accedido al portal por primera vez después del anuncio de la disponibilidad general en noviembre de 2019, ya debería ofrecer el nuevo contenido predeterminado y no es necesario realizar ninguna otra acción.

### <a name="functionality-i-need-isnt-supported-in-the-portal"></a>No se admite la funcionalidad que necesito en el portal

Puede abrir una solicitud de característica en el [repositorio de GitHub][1] o [implementar por sí mismo la funcionalidad que falta][3]. Consulte la sección sobre **extensibilidad** anterior para obtener más información.

### <a name="how-can-i-automate-portal-deployments"></a><a id="automate"></a> ¿Cómo puedo automatizar las implementaciones del portal?

Puede acceder al contenido del portal para desarrolladores y administrarlo mediante programación a través de la API de REST, independientemente de si está usando una versión administrada o autohospedada.

La API se documenta en [la sección wiki del repositorio de GitHub][2]. Se puede usar para automatizar las migraciones del contenido del portal entre entornos; por ejemplo, de un entorno de prueba al de producción. Puede obtener más información sobre este proceso [en este artículo de la documentación](https://aka.ms/apimdocs/migrateportal) en GitHub.

### <a name="how-do-i-move-from-the-managed-to-the-self-hosted-version"></a>¿Cómo se puede pasar de la versión administrada a la versión autohospedada?

Consulte el artículo detallado en la [sección Wiki del repositorio del portal para desarrolladores en GitHub][2].

### <a name="can-i-have-multiple-developer-portals-in-one-api-management-service"></a>¿Puedo tener varios portales para desarrolladores en un servicio API Management?

Puede tener un portal administrado y varios portales autohospedados. El contenido de todos los portales se almacena en el mismo servicio API Management, por lo que serán idénticos. Si quiere diferenciar la apariencia y la funcionalidad de los portales, puede autohospedarlos con sus propios widgets personalizados, que personalizan dinámicamente las páginas en runtime, por ejemplo, en función de la dirección URL.

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>¿Admite el portal plantillas de Azure Resource Manager o es compatible con el kit de recursos de DevOps de API Management?

No.

### <a name="is-the-portals-content-saved-with-the-backuprestore-functionality-in-api-management"></a>¿Se guarda el contenido del portal con la funcionalidad de copia de seguridad y restauración en API Management?

No.

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-managed-portal-dependencies"></a>¿Debo habilitar una conectividad de red virtual adicional para las dependencias del portal administrado?

En la mayoría de los casos, no.

Si el servicio API Management se encuentra en una red virtual interna, solo se puede acceder al portal para desarrolladores desde dentro de la red. El nombre de host del punto de conexión de administración debe resolverse en la dirección VIP interna del servicio desde la máquina que se usa para acceder a la interfaz administrativa del portal. Asegúrese de que el punto de conexión de administración está registrado en el DNS. En caso de que haya una configuración incorrecta, verá un error: `Unable to start the portal. See if settings are specified correctly in the configuration (...)`.

Si el servicio API Management se encuentra en una red virtual interna y accede a él a través de Application Gateway desde Internet, no olvide habilitar la conectividad con el portal para desarrolladores y los puntos de conexión de administración de API Management. Es posible que tenga que deshabilitar las reglas de Web Application Firewall. Consulte [este artículo de la documentación](api-management-howto-integrate-internal-vnet-appgateway.md) para obtener más detalles.

### <a name="i-have-assigned-a-custom-api-management-domain-and-the-published-portal-doesnt-work"></a>He asignado un dominio de API Management personalizado y el portal publicado no funciona

Después de actualizar el dominio, debe [volver a publicar el portal](api-management-howto-developer-portal-customize.md#publish) para que los cambios surtan efecto.

### <a name="i-have-added-an-identity-provider-and-i-cant-see-it-in-the-portal"></a>He agregado un proveedor de identidades y no lo veo en el portal

Después de configurar un proveedor de identidades (por ejemplo, Azure AD o Azure AD B2C), debe [volver a publicar el portal](api-management-howto-developer-portal-customize.md#publish) para que los cambios surtan efecto. Asegúrese de que las páginas del portal para desarrolladores incluyen el widget con los botones de OAuth.

### <a name="i-have-set-up-delegation-and-the-portal-doesnt-use-it"></a>He configurado la delegación y el portal no la usa

Después de configurar la delegación, debe [volver a publicar el portal](api-management-howto-developer-portal-customize.md#publish) para que los cambios surtan efecto.

### <a name="my-other-api-management-configuration-changes-havent-been-propagated-in-the-developer-portal"></a>Los demás cambios de configuración de API Management no se han propagado en el portal para desarrolladores

La mayoría de los cambios de configuración (por ejemplo, la red virtual, el inicio de sesión y los términos del producto) requieren [volver a publicar el portal](api-management-howto-developer-portal-customize.md#publish).

### <a name="im-getting-a-cors-error-when-using-the-interactive-console"></a><a name="cors"></a> Aparece un error de CORS cuando uso la consola interactiva

La consola interactiva realiza una solicitud de API del lado cliente desde el explorador. Resuelva el problema de CORS incorporando [una directiva de CORS](api-management-cross-domain-policies.md#CORS) en las API.

Puede comprobar el estado de la directiva de CORS en la sección **Portal overview** (Información general del portal) del servicio API Management en Azure Portal. Un cuadro de advertencia indica que falta una directiva o que está mal configurada.

![Captura de pantalla que muestra dónde puede comprobar el estado de la directiva de CORS.](media/api-management-howto-developer-portal/cors-azure-portal.png)

Aplique automáticamente la directiva de CORS haciendo clic en el botón **Habilitar CORS**.

También puede habilitar CORS manualmente.

1. Haga clic en el vínculo **Manually apply it on the global level** (Aplicarlo manualmente en el nivel global) para ver el código generado de la directiva.
2. Vaya a **Todas las API** en la sección **API** del servicio API Management en Azure Portal.
3. Haga clic en el icono **</>** en la sección **Procesamiento de entrada**.
4. Inserte la directiva en la sección **<inbound>** del archivo XML. Asegúrese de que el valor de **<origin>** coincide con el dominio del portal para desarrolladores.

> [!NOTE]
> 
> Si aplica la directiva de CORS en el ámbito del producto, en lugar del ámbito de las API, y la API usa la autenticación de clave de suscripción a través de un encabezado, la consola no funcionará.
>
> El explorador emite automáticamente una solicitud HTTP OPTIONS, que no contiene un encabezado con la clave de suscripción. Como falta la clave de suscripción, API Management no puede asociar la llamada a OPTIONS con ningún producto, por lo que no puede aplicar la directiva de CORS.
>
> Como solución alternativa, puede pasar la clave de suscripción en un parámetro de consulta.

> [!NOTE]
> 
> Solo se ejecuta una directiva de CORS. Si especificó varias directivas de CORS (por ejemplo, en el nivel de API y en el nivel de todas las API), es posible que la consola interactiva no funcione según lo esperado.

### <a name="what-permissions-do-i-need-to-edit-the-developer-portal"></a>¿Qué permisos necesito para editar el portal para desarrolladores?

Si ve el error `Oops. Something went wrong. Please try again later.` al abrir el portal en el modo administrativo, es posible que no tenga los permisos necesarios (RBAC de Azure).

Los portales heredados requieren el permiso `Microsoft.ApiManagement/service/getssotoken/action` en el ámbito de servicio (`/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>`) para permitir que el administrador de usuarios tenga acceso a los portales. El nuevo portal requiere el permiso `Microsoft.ApiManagement/service/users/token/action` en el ámbito `/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1`.

Puede usar el siguiente script de PowerShell para crear un rol con el permiso necesario. Recuerde cambiar el parámetro `<subscription-id>`. 

```powershell
#New Portals Admin Role 
Import-Module Az 
Connect-AzAccount 
$contributorRole = Get-AzRoleDefinition "API Management Service Contributor" 
$customRole = $contributorRole 
$customRole.Id = $null
$customRole.Name = "APIM New Portal Admin" 
$customRole.Description = "This role gives the user ability to log in to the new Developer portal as administrator" 
$customRole.Actions = "Microsoft.ApiManagement/service/users/token/action" 
$customRole.IsCustom = $true 
$customRole.AssignableScopes.Clear() 
$customRole.AssignableScopes.Add('/subscriptions/<subscription-id>') 
New-AzRoleDefinition -Role $customRole 
```
 
Una vez creado el rol, se puede conceder a cualquier usuario de la sección **Control de acceso (IAM)** de Azure Portal. Al asignar este rol a un usuario, se asignará el permiso en el ámbito del servicio. El usuario podrá generar tokens de SAS en nombre de *cualquier* usuario en el servicio. Como mínimo, este rol debe asignarse al administrador del servicio. El siguiente comando de PowerShell muestra cómo asignar el rol a un usuario `user1` en el ámbito más bajo para evitar la concesión de permisos innecesarios al usuario: 

```powershell
New-AzRoleAssignment -SignInName "user1@contoso.com" -RoleDefinitionName "APIM New Portal Admin" -Scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1" 
```

Una vez concedidos los permisos a un usuario, este debe cerrar sesión e iniciar sesión de nuevo en Azure Portal para que los nuevos permisos surtan efecto.

### <a name="im-seeing-the-unable-to-start-the-portal-see-if-settings-are-specified-correctly--error"></a>Veo el error `Unable to start the portal. See if settings are specified correctly (...)`

Este error se muestra cuando se produce un error en una llamada `GET` a `https://<management-endpoint-hostname>/subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.ApiManagement/service/xxx/contentTypes/document/contentItems/configuration?api-version=2018-06-01-preview`. La interfaz administrativa del portal emite la llamada desde el explorador.

Si el servicio API Management se encuentra en una red virtual, consulte la pregunta de conectividad de la red virtual anterior.

El error de llamada también puede deberse a un certificado TLS/SSL, que se asigna a un dominio personalizado y no es de confianza para el explorador. Como medida de mitigación, puede quitar el dominio personalizado del punto de conexión de administración (API Management revertirá al punto de conexión predeterminado con un certificado de confianza).

### <a name="whats-the-browser-support-for-the-portal"></a>¿Cuál es la compatibilidad del explorador con el portal?

| Browser                     | Compatible       |
|-----------------------------|-----------------|
| Apple Safari                | Sí<sup>1</sup> |
| Google Chrome               | Sí<sup>1</sup> |
| Microsoft Edge              | Sí<sup>1</sup> |
| Microsoft Internet Explorer | No              |
| Mozilla Firefox             | Sí<sup>1</sup> |

 <small><sup>1</sup> Se admite en las dos últimas versiones de producción.</small>

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del nuevo portal para desarrolladores:

- [Acceso a la versión administrada del portal para desarrolladores y su personalización](api-management-howto-developer-portal-customize.md)
- [Configuración de la versión autohospedada del portal][2]
- [Implementación de su propio widget][3]

Examine otros recursos:

- [Repositorio de GitHub con el código fuente][1]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://aka.ms/apimdevportal/extend
