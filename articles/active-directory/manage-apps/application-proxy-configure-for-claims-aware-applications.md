---
title: 'Aplicaciones compatibles con notificaciones: proxy de aplicación de Azure AD | Microsoft Docs'
description: En este artículo se explica cómo publicar aplicaciones de ASP.NET locales que acepten notificaciones ADFS para proteger el acceso remoto a sus usuarios.
services: active-directory
documentationcenter: ''
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/08/2018
ms.author: kenwith
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: b90c2d47e70a4f7595ac535d5f8ba9506087eb72
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99258531"
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Trabajo con aplicaciones para notificaciones en Proxy de aplicación
Las [aplicaciones para notificaciones](/previous-versions/windows/desktop/legacy/bb736227(v=vs.85)) realizan un redireccionamiento al servicio de token de seguridad (STS). El STS solicita las credenciales del usuario a cambio de un token y, después, redirige al usuario a la aplicación. Hay varias maneras de habilitar el Proxy de aplicación para trabajar con estos redireccionamientos. Use este artículo para configurar la implementación en las aplicaciones para notificaciones. 

## <a name="prerequisites"></a>Prerrequisitos
Asegúrese de que el STS al que redirige la aplicación para notificaciones está disponible fuera de la red local. Puede habilitar el STS exponiéndolo a través de un proxy o permitiendo las conexiones externas. 

## <a name="publish-your-application"></a>Publicación de la aplicación

1. Publique la aplicación según las instrucciones de [Publicar aplicaciones con el proxy de aplicación](application-proxy-add-on-premises-application.md).
2. Vaya a la página de la aplicación en el portal y seleccione **Inicio de sesión único**.
3. Si elige **Azure Active Directory** como **Método de autenticación previa**, seleccione **Se desactivó el inicio de sesión único de Azure AD** como **Método de autenticación interno**. Si elige **Paso a través** como **Método de autenticación previa**, no necesita realizar ningún cambio.

## <a name="configure-adfs"></a>Configuración de AD FS

Puede configurar AD FS en aplicaciones para notificaciones de alguna de estas dos formas. La primera consiste en usar dominios personalizados. La segunda es con WS-Federation. 

### <a name="option-1-custom-domains"></a>Opción 1: Dominios personalizados

Si todas las direcciones URL internas de las aplicaciones son nombres de dominio completos, entonces puede configurar los [dominios personalizados](application-proxy-configure-custom-domain.md) de las aplicaciones. Use los dominios personalizados para crear direcciones URL externas que sean las mismas que las direcciones URL internas. Cuando las direcciones URL externas coinciden con las direcciones URL internas, las redirecciones de STS funcionan independientemente de que los usuarios estén en una ubicación local o remota. 

### <a name="option-2-ws-federation"></a>Opción 2: El certificado del proveedor de identidades de WS-Federation

1. Abra Administración de AD FS.
2. Vaya a **Relying Party Trusts** (Relaciones de confianza de usuarios de confianza), haga clic con el botón derecho en la aplicación que va a publicar con el proxy de aplicación y seleccione **Propiedades**.  

   ![Relaciones de confianza para usuario autenticado: haga clic con el botón derecho en el nombre de la aplicación (captura de pantalla)](./media/application-proxy-configure-for-claims-aware-applications/appproxyrelyingpartytrust.png)  

3. En la pestaña **Puntos de conexión**, en **Tipo de punto de conexión**, seleccione **WS-Federation**.
4. En **URL de confianza**, escriba la dirección URL especificada en el Proxy de aplicación en **Dirección URL externa** y haga clic en **Aceptar**.  

   ![Agregar un extremo: establezca el valor de Dirección URL de confianza - captura de pantalla](./media/application-proxy-configure-for-claims-aware-applications/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Pasos siguientes
* [Habilitación de las aplicaciones de cliente nativo para interactuar con el proxy de aplicación](application-proxy-configure-native-client-application.md)