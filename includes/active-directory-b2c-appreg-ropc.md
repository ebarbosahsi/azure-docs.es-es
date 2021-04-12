---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 38261556579fb5db86fb231b0edf3412eed51d35
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106073323"
---
Para registrar una aplicación en su inquilino de Azure AD B2C, puede usar la nueva experiencia unificada **Registros de aplicaciones**, o bien la experiencia anterior **Aplicaciones (heredado)** . [Más información acerca de la nueva experiencia](../articles/active-directory-b2c/app-registrations-training-guide.md).

#### <a name="app-registrations"></a>[Registros de aplicaciones](#tab/app-reg-ga/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *ROPC_Auth_app*.
1. Deje los demás valores como están y seleccione **Registrar**.
1. Anote el **Id. de aplicación (cliente)** para usarlo en un paso posterior.
1. En **Administrar**, seleccione **Autenticación**.
1. Seleccione **Probar la nueva experiencia** (si se muestra).
1. En **Tipo de cliente predeterminado**, seleccione **Sí** para tratar el cliente como un cliente público. Esta opción de configuración es necesaria para el flujo de ROPC.
1. Seleccione **Guardar**.
1. En el menú de la izquierda, seleccione **Manifiesto** para abrir el editor de manifiestos. 
1. Establezca el atributo **oauth2AllowImplicitFlow** en *true*:
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. Seleccione **Guardar**.

#### <a name="applications-legacy"></a>[Applications (Legacy)](#tab/applications-legacy/) (Aplicaciones [heredadas])

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones (heredado)** y, a continuación, **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *ROPC_Auth_app*.
1. En **Cliente nativo**, seleccione **Sí**.
1. Deje los demás valores como están y seleccione **Crear**.
1. Anote el valor de **ID. DE APLICACIÓN** para usarlo en un paso posterior.