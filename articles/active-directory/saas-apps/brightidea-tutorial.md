---
title: 'Tutorial: Integración de Azure Active Directory con Brightidea | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Brightidea.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/23/2019
ms.author: jeedes
ms.openlocfilehash: 9967f349011b52a2218681956885c33456ba1d46
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97672770"
---
# <a name="tutorial-azure-active-directory-integration-with-brightidea"></a>Tutorial: Integración de Azure Active Directory con Brightidea

En este tutorial, aprenderá a integrar Brightidea con Azure Active Directory (Azure AD).
La integración de Brightidea con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Brightidea.
* Puede permitir que los usuarios inicien sesión automáticamente en Brightidea (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con Brightidea, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Brightidea

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.


* Brightidea admite el inicio de sesión único iniciado por **SP e IDP**.
* Brightidea admite el aprovisionamiento de usuarios **Just-In-Time**.


## <a name="adding-brightidea-from-the-gallery"></a>Incorporación de Brightidea desde la galería

Para configurar la integración de Brightidea en Azure AD, es preciso agregar Brightidea desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Brightidea desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Brightidea**, seleccione **Brightidea** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Brightidea en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Brightidea con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Brightidea.

Para configurar y probar el inicio de sesión único de Azure AD con Brightidea, realice los pasos siguientes:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Brightidea](#configure-brightidea-single-sign-on)** : para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Brightidea](#create-brightidea-test-user)** : para tener un homólogo de Britta Simon en Brightidea que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Brightidea, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Brightidea**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios** y desea realizar la configuración en el modo iniciado por **IDP**, realice los pasos siguientes:

    a. Haga clic en **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Elección del archivo de metadatos](common/browse-upload-metadata.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección Brightidea:

    ![Captura de pantalla que muestra la configuración básica de SAML, donde se puede escribir el identificador y la dirección U R L de respuesta y seleccionar Guardar.](common/idp-intiated.png)

    > [!Note]
    > Si los valores **Identificador** y **URL de respuesta** no se rellenan automáticamente, hágalo manualmente según sus necesidades.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Captura de pantalla que muestra Establecer direcciones U R L adicionales donde puede escribir una U R L de inicio de sesión.](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.brightidea.com`

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Brightidea** (Configurar Brightidea), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-brightidea-single-sign-on"></a>Configuración del inicio de sesión único de Brightidea

1. En una ventana diferente del explorador web, inicie sesión en Brightidea mediante las credenciales de administrador.

2. Para ir a la característica de inicio de sesión único en el sistema Brightidea, vaya a **Enterprise Setup** -> **Authentication Tab** (Configuración de empresa > pestaña Autenticación). Habrá dos subpestañas: Auth Selection (Selección automática) y SAML Profiles (Perfiles de SAML)

    ![Captura de pantalla que muestra el sitio de Brightidea con la pestaña Authentication (Autenticación) seleccionada.](./media/brightidea-tutorial/configure1.png)

3. Seleccione **Auth Selection** (Selección automática). De forma predeterminada, solo se muestran dos métodos estándar: Brightidea Login (Inicio de sesión en Brightidea) y Registration (Registro). Cuando se agrega un método de inicio de sesión único, aparece en la lista.

    ![Captura de pantalla que muestra la pestaña Authentication (Autenticación) de Brightidea con la opción Auth Selection (Selección de autenticación) seleccionada.](./media/brightidea-tutorial/configure2.png)

4. Seleccione **SAML Profiles** (Perfiles de SAML) y realice los siguientes pasos:

    ![Captura de pantalla que muestra la pestaña Authentication (Autenticación) de Brightidea con SAML Profiles (Perfiles de SAML) seleccionado, que proporciona las opciones Download Metadata (Descargar metadatos) y Add New (Agregar nuevo).](./media/brightidea-tutorial/configure3.png)

    a. Haga clic en **Download Metadata** (Descargar metadatos) y cargue la sección **Basic SAML Configuration** (Configuración básica de SAML) en Azure Portal.

    b. Haga clic en el botón **Add New** (Agregar nuevo) en **Identity Provider Setting** (Configuración del proveedor de identidades), realice los pasos siguientes:

    ![Captura de pantalla que muestra Identity Provider Setting (Configuración del proveedor de identidades) de Brightidea para escribir información.](./media/brightidea-tutorial/configure4.png)

   * Escriba el **nombre del perfil de SAML**, por ejemplo, `Azure Ad SSO`.

   * Para **cargar metadatos**, haga clic para elegir el archivo y cargue el archivo de metadatos descargado de Azure Portal.

     > [!NOTE]
     > Después de cargar el archivo de metadatos, los campos restantes **Single Sign-on Service (Servicio de inicio de sesión único), Identity Provider Issuer (Emisor del proveedor de identidades), Upload Public Key (Cargar clave pública)** se rellenan automáticamente.

   * En el cuadro de texto **Email** (Correo electrónico), escriba el valor como `mail`.

   * En el cuadro de texto **Screen Name** (Nombre de pantalla), escriba el valor como `givenName`.

   * Haga clic en **Guardar cambios**.  

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.

    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Brightidea para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **Brightidea**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Brightidea**.

    ![El vínculo a Brightidea en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-brightidea-test-user"></a>Creación de un usuario de prueba de Brightidea

En esta sección, se crea un usuario llamado a Britta Simon en Brightidea. Brightidea admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe ya en Brightidea, se crea uno después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Brightidea en el panel de acceso, debería iniciar sesión automáticamente en la versión de Brightidea para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)