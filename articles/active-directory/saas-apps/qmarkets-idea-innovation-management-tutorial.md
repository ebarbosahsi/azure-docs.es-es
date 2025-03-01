---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Qmarkets Idea & Innovation Management | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Qmarkets Idea & Innovation Management.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/20/2019
ms.author: jeedes
ms.openlocfilehash: 89be6d631d56e9b3368a351a4f0c521ca327bf3f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92522196"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-qmarkets-idea--innovation-management"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Qmarkets Idea & Innovation Management

En este tutorial, aprenderá a integrar Qmarkets Idea & Innovation Management con Azure Active Directory (Azure AD). Al integrar Qmarkets Idea & Innovation Management con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Qmarkets Idea & Innovation Management.
* Permitir que los usuarios inicien sesión automáticamente en Qmarkets Idea & Innovation Management con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción de Qmarkets Idea & Innovation Management habilitada para el inicio de sesión único (SSO).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.



* Qmarkets Idea & Innovation Management admite el inicio de sesión único iniciado por **SP e IDP**.
* Qmarkets Idea & Innovation Management admite el aprovisionamiento de usuarios **Just-In-Time**.


## <a name="adding-qmarkets-idea--innovation-management-from-the-gallery"></a>Adición de Qmarkets Idea & Innovation Management desde la galería

Para configurar la integración de Qmarkets Idea & Innovation Management en Azure AD, es preciso agregar Qmarkets Idea & Innovation Management desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Qmarkets Idea & Innovation Management** en el cuadro de búsqueda.
1. Seleccione **Qmarkets Idea & Innovation Management** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-qmarkets-idea--innovation-management"></a>Configuración y prueba del inicio de sesión único de Azure AD para Qmarkets Idea & Innovation Management

Configure y pruebe el inicio de sesión único de Azure AD con Qmarkets Idea & Innovation Management con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Qmarkets Idea & Innovation Management.

Para configurar y probar el inicio de sesión único de Azure AD con Qmarkets Idea & Innovation Management, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Qmarkets Idea & Innovation Management](#configure-qmarkets-idea--innovation-management-sso)**: para configurar los valores del inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Qmarkets Idea & Innovation Management](#create-qmarkets-idea--innovation-management-test-user)**: para tener un homólogo de B.Simon en Qmarkets Idea & Innovation Management que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Qmarkets Idea & Innovation Management**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<app_url>/sso/saml2/metadata/qmarkets_sp_<endpoint_id>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<app_url>/sso/saml2/acs/qmarkets_sp_<endpoint_id>`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<app_url>/sso/saml2/endpoint/qmarkets_sp_<endpoint_id>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de Qmarkets Idea & Innovation Management](mailto:support@qmarkets.net). También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a B. Simon para que use el inicio de sesión único de Azure mediante la concesión de acceso a Qmarkets Idea & Innovation Management.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Qmarkets Idea & Innovation Management**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-qmarkets-idea--innovation-management-sso"></a>Configuración del inicio de sesión único de Qmarkets Idea & Innovation Management

Para configurar el inicio de sesión único en **Qmarkets Idea & Innovation Management**, es preciso enviar la **dirección URL de metadatos de Federación de la aplicación** al [equipo de soporte técnico de Qmarkets Idea & Innovation Management](mailto:support@qmarkets.net). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-qmarkets-idea--innovation-management-test-user"></a>Creación de un usuario de prueba de Qmarkets Idea & Innovation Management

En esta sección, se crea un usuario llamado Britta Simon en Qmarkets Idea & Innovation Management. Qmarkets Idea & Innovation Management admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe aún en Qmarkets Idea & Innovation Management, se crea uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Qmarkets Idea & Innovation Management en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Qmarkets Idea & Innovation Management para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe Qmarkets Idea & Innovation Management con Azure AD](https://aad.portal.azure.com/)