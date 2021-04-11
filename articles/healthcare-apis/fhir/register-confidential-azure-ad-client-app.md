---
title: 'Registro de una aplicación cliente confidencial en Azure AD: Azure API for FHIR'
description: Registre una aplicación cliente confidencial en Azure Active Directory que se autentique en nombre de un usuario y solicite acceso a las aplicaciones de recursos.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: matjazl
ms.openlocfilehash: c10b27d375e2bfb8c64130eceb416a633241cf68
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2021
ms.locfileid: "107284451"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registro de una aplicación cliente confidencial en Azure Active Directory

En este tutorial, obtendrá información sobre cómo registrar una aplicación cliente confidencial en Azure Active Directory (Azure AD).  

Un registro de aplicación cliente es una representación Azure AD de una aplicación que se puede usar para autenticarse en nombre de un usuario y solicitar acceso a [las aplicaciones de recursos](register-resource-azure-ad-client-app.md). Una aplicación cliente confidencial es una aplicación en la que se puede confiar para almacenar un secreto y presentar dicho secreto cuando se solicitan tokens de acceso. Ejemplos de aplicaciones confidenciales son las aplicaciones de servidor. 

Para registrar una nueva aplicación cliente confidencial, consulte los pasos siguientes. 

## <a name="register-a-new-application"></a>Registro de una nueva aplicación

1. En [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**.

1. Seleccione **App registrations** (Registros de aplicaciones). 

    :::image type="content" source="media/how-to-aad/portal-aad-new-app-registration.png" alt-text="Azure Portal. Nuevo registro de aplicaciones.":::

1. Seleccione **Nuevo registro**.

1. Asigne a la aplicación un nombre para mostrar orientado al usuario.

1. En **tipos de cuenta compatibles**, seleccione quién puede usar la aplicación o acceder a la API.

1. Opta Proporcione un **URI de redirección**. Estos detalles se pueden cambiar más adelante, pero si conoce la dirección URL de respuesta de la aplicación, indíquelo ahora.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png" alt-text="Nuevo registro de la aplicación cliente confidencial.":::

1. Seleccione **Registrar**.

## <a name="api-permissions"></a>Permisos de API

Ahora que ha registrado la aplicación, debe seleccionar qué permisos de API debe solicitar esta aplicación en nombre de los usuarios.

1. Seleccione **Permisos de API**.

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png" alt-text="Cliente confidencial. Permisos de la API.":::

1. Seleccione **Agregar un permiso**.

    Si usa la API de Azure para FHIR, agregará un permiso a las API de Azure Healthcare buscando API de Azure **Healthcare** en las **API que usa mi organización**. El resultado de la búsqueda de la API de Azure Healthcare solo devolverá si ya ha [implementado la API de Azure para FHIR](fhir-paas-powershell-quickstart.md).

    Si hace referencia a una aplicación de recursos diferente, seleccione el registro de la aplicación de recursos de la [API de FHIR](register-resource-azure-ad-client-app.md) que creó anteriormente en **mis API**.


    :::image type="content" source="media/conf-client-app/confidential-client-org-api.png" alt-text="Cliente confidencial. API de mi organización" lightbox="media/conf-client-app/confidential-app-org-api-expanded.png":::.
    

1. Seleccione los ámbitos (permisos) que la aplicación cliente confidencial solicitará en nombre de un usuario. Seleccione **user_impersonation** y después **Agregar permisos**.

    :::image type="content" source="media/conf-client-app/confidential-client-add-permission.png" alt-text="Cliente confidencial. Permisos delegados":::.


## <a name="application-secret"></a>Secreto de aplicación

1. Seleccione **Certificados y secretos** y luego seleccione **Nuevo secreto de cliente**. 

    :::image type="content" source="media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png" alt-text="Cliente confidencial. Secreto de la aplicación.":::

1. Escriba una **descripción** del secreto de cliente. Seleccione el menú desplegable **Expires** para elegir un período de tiempo de expiración y, a continuación, haga clic en **Agregar**.

   :::image type="content" source="media/how-to-aad/add-a-client-secret.png" alt-text="Agregue un secreto de cliente.":::

1. Una vez creada la cadena de secreto de cliente, copie su **valor** e **identificador** y almacénelos en una ubicación segura de su elección.

   :::image type="content" source="media/how-to-aad/client-secret-string-password.png" alt-text="Cadena de secreto de cliente."::: 

> [!NOTE]
>La cadena de secreto de cliente solo es visible una vez en el Azure Portal. Cuando sale de la Página Web de certificados & Secrets y después vuelve a él, la cadena de valor se enmascara. Es importante hacer una copia de la cadena de secreto de cliente inmediatamente después de que se genere. Si no dispone de una copia de seguridad del secreto de cliente, debe repetir los pasos anteriores para regenerarlo.
 
## <a name="next-steps"></a>Pasos siguientes

En este artículo, se le guiará por los pasos necesarios para registrar una aplicación cliente confidencial en el Azure AD. También se le guiará por los pasos para agregar permisos de API a la API de Azure Healthcare. Por último, se mostró cómo crear un secreto de aplicación. Además, puede obtener información sobre cómo acceder al servidor de FHIR mediante Postman.
 
>[!div class="nextstepaction"]
>[Acceso a Azure API for FHIR con Postman](access-fhir-postman-tutorial.md)
