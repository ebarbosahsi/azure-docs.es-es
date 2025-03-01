---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con LinkedIn Sales Navigator | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y LinkedIn Sales Navigator.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: jeedes
ms.openlocfilehash: 3f4d4d1e0fc44b9fb031ce5e3e45219fa8169562
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92458513"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-sales-navigator"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con LinkedIn Sales Navigator

En este tutorial, aprenderá a integrar LinkedIn Sales Navigator con Azure Active Directory (Azure AD). Al integrar LinkedIn Sales Navigator con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a LinkedIn Sales Navigator.
* Permitir que los usuarios inicien sesión automáticamente en LinkedIn Sales Navigator con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en LinkedIn Sales Navigator.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* LinkedIn Sales Navigator admite el inicio de sesión único iniciado por **SP y IDP**.
* LinkedIn Sales Navigator admite el aprovisionamiento de usuarios **Just In Time**.
* LinkedIn Sales Navigator admite el aprovisionamiento de usuarios [**automatizado**](linkedinsalesnavigator-provisioning-tutorial.md).

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>Adición de LinkedIn Sales Navigator desde la galería

Para configurar la integración de LinkedIn Sales Navigator en Azure AD, deberá agregar LinkedIn Sales Navigator desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **LinkedIn Sales Navigator** en el cuadro de búsqueda.
1. Seleccione **LinkedIn Sales Navigator** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-sales-navigator"></a>Configuración y prueba del inicio de sesión único de Azure AD en LinkedIn Sales Navigator

Configure y pruebe el inicio de sesión único de Azure AD con LinkedIn Sales Navigator mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de LinkedIn Sales Navigator.

Para configurar y probar el inicio de sesión único de Azure AD con LinkedIn Sales Navigator, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en LinkedIn Sales Navigator](#configure-linkedin-sales-navigator-sso)** , para configurar el inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de LinkedIn Sales Navigator](#create-linkedin-sales-navigator-test-user)** , para tener un homólogo de B.Simon en LinkedIn Sales Navigator que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **LinkedIn Sales Navigator**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba el valor de **Id. de entidad**; este valor lo copiará de Linkedin Portal, que se explica más adelante en este tutorial.

    b. En el cuadro de texto **URL de respuesta**, escriba el valor de **Assertion Consumer Access (ACS) Url** (Dirección URL de Assertion Consumer Access [ACS]); este valor lo copiará de Linkedin Portal, que se explica más adelante en este tutorial.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

1. La aplicación LinkedIn Sales Navigator espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación LinkedIn Sales Navigator espera que se devuelvan algunos atributos más (se muestran a continuación) en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | --- | --- |
    | email| user.mail |
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |
    | Identificador de usuario único | user.mail |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up LinkedIn Sales Navigator** (Configurar LinkedIn Sales Navigator), copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a conceder a B.Simon acceso a LinkedIn Sales Navigator utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **LinkedIn Sales Navigator**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-linkedin-sales-navigator-sso"></a>Configuración del inicio de sesión único en LinkedIn Sales Navigator

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio web de **LinkedIn Sales Navigator**.

1. En **Account Center** (Centro de cuentas), haga clic en **Global Settings** (Configuración global) en **Settings** (Configuración). Además, seleccione **Sales Navigator** de la lista desplegable.

    ![Captura de pantalla que muestra la sección Application Settings (Configuración de la aplicación), donde puede seleccionar Sales Navigator.](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Haga clic en **OR Click Here to load and copy individual fields from the form** (O haga clic aquí para cargar y copiar campos individuales del formulario) y haga lo siguiente:

    ![Captura de pantalla que muestra Inicio de sesión único donde puede escribir los valores descritos.](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

    a. Copie el valor **Entity Id** (Id. de entidad) y péguelo en el cuadro de texto **Identificador** de la sección **Basic SAML Configuration** (Configuración básica de SAML) de Azure Portal.

    b. Copie el valor de **Assertion Consumer Access (ACS) Url** (Dirección URL de Assertion Consumer Access [ACS]) y péguelo en el cuadro de texto **URL de respuesta** de **Basic SAML Configuration** (Configuración básica de SAML) de Azure Portal.

1. Vaya a la sección **LinkedIn Admin Settings** (Configuración de administrador de LinkedIn). Cargue el archivo XML que ha descargado de Azure Portal haciendo clic en la opción **Upload XML file** (Cargar archivo XML).

    ![Captura de pantalla que muestra la sección Configure the LinkedIn service provider S S O settings (Configurar valores de inicio de sesión único del proveedor de servicios de LinkedIn) donde puede cargar un archivo X M L.](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. Haga clic en **On** (Activar) para habilitar SSO. El estado de SSO cambiará de **Not Connected** (No conectado) a **Connected** (Conectado).

    ![Captura de pantalla que muestra la opción Single Sign-On (Inicio de sesión único), donde puede habilitar Authenticate users with S S O (Autenticar usuarios con el inicio de sesión único).](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-sales-navigator-test-user"></a>Creación de un usuario de prueba de LinkedIn Sales Navigator

La aplicación LinkedIn Sales Navigator admite aprovisionamiento de usuarios Just-In-Time (JIT) y, tras la autenticación, los usuarios se crearán automáticamente en la aplicación. Active **Automatically assign licenses** (Asignar licencias automáticamente) para asignar una licencia al usuario.

   ![Creación de un usuario de prueba de Azure AD](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de LinkedIn Sales Navigator en el Panel de acceso, debería iniciar sesión automáticamente en la versión de LinkedIn Sales Navigator para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe LinkedIn sales Navigator con Azure AD](https://aad.portal.azure.com/)