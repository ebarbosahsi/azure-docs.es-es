---
title: 'Tutorial: Integración de Azure Active Directory con 123FormBuilder SSO | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y 123FormBuilder SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: jeedes
ms.openlocfilehash: aa4bab2f7ecb90c61e22de46b01a5ed81342a408
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92319175"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-123formbuilder-sso"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con 123FormBuilder SSO

En este tutorial aprenderá a integrar 123FormBuilder SSO con Azure Active Directory (Azure AD). Al integrar 123FormBuilder SSO con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a 123FormBuilder SSO.
* Permitir que los usuarios inicien sesión automáticamente en 123FormBuilder SSO con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en 123FormBuilder SSO.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* 123FormBuilder SSO admite el inicio de sesión único iniciado por **SP e IDP**.
* 123FormBuilder SSO admite el aprovisionamiento de usuarios **Just-In-Time**.
* Una vez configurado 123FormBuilder SSO, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-123formbuilder-sso-from-the-gallery"></a>Incorporación de 123FormBuilder SSO desde la galería

Para configurar la integración de 123FormBuilder SSO en Azure AD, es preciso agregar 123FormBuilder SSO desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **123FormBuilder SSO** en el cuadro de búsqueda.
1. Seleccione **123FormBuilder SSO** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-123formbuilder-sso"></a>Configuración y prueba del inicio de sesión único de Azure AD para 123FormBuilder SSO

Configure y pruebe el inicio de sesión único de Azure AD con 123FormBuilder SSO mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de 123FormBuilder SSO.

Para configurar y probar el inicio de sesión único de Azure AD con 123FormBuilder SSO, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de 123FormBuilder SSO](#configure-123formbuilder-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de 123FormBuilder SSO](#create-123formbuilder-sso-test-user)** , para tener un homólogo de B.Simon en 123FormBuilder SSO vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **123FormBuilder SSO**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/metadata`


    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/acs`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizar este valor con las direcciones URL y el identificador reales, como se explica más adelante en el tutorial.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar 123FormBuilder SSO**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

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

En esta sección va a permitir que B.Simon acceda a 123FormBuilder SSO mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **123FormBuilder SSO**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-123formbuilder-sso"></a>Configuración del inicio de sesión único de 123FormBuilder SSO

1. Para configurar el inicio de sesión único en **123FormBuilder SSO**, vaya a [https://www.123formbuilder.com/form-2709121/](https://www.123formbuilder.com/form-2709121/) y siga estos pasos:

    ![Captura de pantalla que muestra la pantalla de configuración SSO SAML - Identity Provider (SSO de SAML: proveedor de identidades).](./media/123formbuilder-tutorial/submit.png) 

    a. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario, en el ejemplo `B.Simon@Contoso.com`.

    b. Haga clic en **Upload** (Cargar) y vaya al archivo XML de metadatos que ha descargado de Azure Portal.

    c. Haga clic en **ENVIAR FORMULARIO**.

2. En **Microsoft Azure AD - Inicio de sesión único - Configurar las opciones de la aplicación**, realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/123formbuilder-tutorial/url3.png)

    a. Si desea configurar la aplicación en el **modo iniciado por IDP**, copie el valor de **IDENTIFIER** (IDENTIFICADOR) de la instancia y péguelo en el cuadro de texto **Identificador** en la sección **Configuración básica de SAML** en Azure Portal.

    b. Si desea configurar la aplicación en el **modo iniciado por IDP**, copie el valor de **REPLY URL** (URL DE RESPUESTA) de la instancia y péguelo en el cuadro de texto **URL de respuesta** en la sección **Configuración básica de SAML**  en Azure Portal.

    c. Si desea configurar la aplicación en el **modo iniciado por SP**, copie el valor de **SIGN ON URL** (URL DE INICIO DE SESIÓN) de la instancia y péguelo en el cuadro de texto **URL de inicio de sesión** en la sección **Configuración básica de SAML**  en Azure Portal.

### <a name="create-123formbuilder-sso-test-user"></a>Creación de un usuario de prueba de 123FormBuilder SSO

En esta sección, se crea un usuario llamado Britta Simon en 123FormBuilder SSO. 123FormBuilder SSO admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe ya en 123FormBuilder SSO, se crea después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de 123FormBuilder SSO en el Panel de acceso, debería iniciar sesión automáticamente en la versión de 123FormBuilder SSO para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe 123FormBuilder SSO con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)