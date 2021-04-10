---
title: Sincronización de atributos en Azure AD para la asignación
description: Al configurar el aprovisionamiento de usuarios para aplicaciones SaaS, use la característica de extensión de directorio para agregar atributos de origen que no están sincronizados de manera predeterminada.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 03/17/2021
ms.author: kenwith
ms.openlocfilehash: 52f34cdafac76a9bca2b4ff0b00e0b3efaa63f5d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104579440"
---
# <a name="syncing-extension-attributes-attributes"></a>Sincronización de atributos de extensión

Al personalizar las asignaciones de atributos para el aprovisionamiento de usuarios, es posible que el atributo que quiera asignar no aparezca en la lista **Atributo de origen**. En este artículo se muestra cómo agregar el atributo que falta mediante su sincronización de Active Directory (AD) local a Azure Active Directory (Azure AD) o mediante la creación de los atributos de extensión en Azure AD para un usuario solo en la nube. 

Azure AD debe contener todos los datos necesarios para crear un perfil de usuario al aprovisionar cuentas de usuario de Azure AD en una aplicación SaaS. En algunos casos, para que los datos estén disponibles es posible que tenga que sincronizar atributos de AD local en Azure AD. Azure AD Connect sincroniza automáticamente determinados atributos en Azure AD, pero no todos. Además, es posible que algunos atributos (por ejemplo, SAMAccountName) que se sincronizan de manera predeterminada no puedan exponerse a través de Azure AD Graph API. En estos casos, puede usar la característica de extensión de directorio de Azure AD Connect para sincronizar el atributo en Azure AD. De este modo, el atributo será visible para Azure AD Graph API y el servicio de aprovisionamiento de Azure AD. Si los datos que necesita para el aprovisionamiento están en Active Directory pero no están disponibles para el aprovisionamiento debido a las razones descritas anteriormente, puede usar Azure AD Connect para crear atributos de extensión. 

Aunque es probable que la mayoría de los usuarios sean usuarios híbridos sincronizados desde Active Directory, también puede crear extensiones en usuarios solo en la nube sin usar Azure AD Connect. Con PowerShell o Microsoft Graph puede ampliar el esquema de un usuario solo en la nube. 

## <a name="create-an-extension-attribute-using-azure-ad-connect"></a>Creación de un atributo de extensión mediante Azure AD Connect

1. Abra el asistente de Azure AD Connect, elija Tareas y, a continuación, elija **Personalizar las opciones de sincronización**.

   ![Página de tareas adicionales del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Inicie sesión como administrador global de Azure AD. 

3. En la página **Características opcionales**, seleccione **Sincronización de atributos de las extensiones de directorios**.
 
   ![Página de características opcionales del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Seleccione los atributos que quiere extender a Azure AD.
   > [!NOTE]
   > La búsqueda en **Atributos disponibles** distingue mayúsculas de minúsculas.

   ![Captura de pantalla en la que se muestra la página de selección "Extensiones de directorio".](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Finalice el asistente de Azure AD Connect y permita que se ejecute un ciclo de sincronización completo. Cuando se complete el ciclo, se extiende el esquema y los nuevos valores se sincronizan entre AD local y Azure AD.
 
6. En Azure Portal, mientras [edita las asignaciones de atributos de usuario](customize-application-attributes.md), la lista **Atributo de origen** ahora contendrá el atributo agregado en el formato `<attributename> (extension_<appID>_<attributename>)`. Seleccione el atributo y asígnelo a la aplicación de destino para el aprovisionamiento.

   ![Página de selección de las extensiones de directorios del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> La capacidad de aprovisionar atributos de referencia desde AD local, como **managedby** o **DN/DistinguishedName**, no se admite actualmente. Puede solicitar esta característica en [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory). 

## <a name="create-an-extension-attribute-on-a-cloud-only-user"></a>Creación de un atributo de extensión en un usuario solo en la nube
Los clientes pueden usar Microsoft Graph y PowerShell para ampliar el esquema de usuario. Estos atributos de extensión se detectan automáticamente en la mayoría de los casos, pero los clientes con más de 1000 entidades de servicio pueden encontrar las extensiones que faltan en la lista de atributos de origen. Si un atributo creado con los pasos siguientes no aparece automáticamente en la lista de atributos de origen, compruebe mediante el gráfico si el atributo de extensión se creó correctamente y agréguelo al esquema [manualmente](https://docs.microsoft.com/azure/active-directory/app-provisioning/customize-application-attributes#editing-the-list-of-supported-attributes). Al realizar las solicitudes de gráficos siguientes, haga clic en Más información para comprobar los permisos necesarios para realizar las solicitudes. Puede usar el [Explorador de gráficos](https://docs.microsoft.com/graph/graph-explorer/graph-explorer-overview) para realizar las solicitudes. 

### <a name="create-an-extension-attribute-on-a-cloud-only-user-using-microsoft-graph"></a>Creación de un atributo de extensión en un usuario solo en la nube mediante Microsoft Graph
Deberá usar una aplicación para ampliar el esquema de los usuarios. Enumere las aplicaciones del inquilino para identificar el id. de la aplicación que quiere usar para ampliar el esquema de usuario. [Más información.](https://docs.microsoft.com/graph/api/application-list?view=graph-rest-1.0&tabs=http)

```json
GET https://graph.microsoft.com/v1.0/applications
```

Cree el atributo de extensión. Reemplace la propiedad **id** siguiente por el **id.** recuperado en el paso anterior. Tendrá que usar el atributo **"id"** en lugar de "appId". [Más información.](https://docs.microsoft.com/graph/api/application-post-extensionproperty?view=graph-rest-1.0&tabs=http)
```json
POST https://graph.microsoft.com/v1.0/applications/{id}/extensionProperties
Content-type: application/json

{
    "name": "extensionName",
    "dataType": "string",
    "targetObjects": [
        "User"
    ]
}
```

La solicitud anterior creó un atributo de extensión con el formato "extension_appID_extensionName". Actualice un usuario con el atributo de extensión. [Más información.](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0&tabs=http)
```json
PATCH https://graph.microsoft.com/v1.0/users/{id}
Content-type: application/json

{
  "extension_inputAppId_extensionName": "extensionValue"
}
```
Compruebe el usuario para asegurarse de que el atributo se actualizó correctamente. [Más información.](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0&tabs=http#example-3-users-request-using-select)

```json
GET https://graph.microsoft.com/v1.0/users/{id}?$select=displayName,extension_inputAppId_extensionName
```


### <a name="create-an-extension-attribute-on-a-cloud-only-user-using-powershell"></a>Creación de un atributo de extensión en un usuario solo en la nube mediante PowerShell
Cree una extensión personalizada mediante PowerShell y asigne un valor a un usuario. 

```
#Connect to your Azure AD tenant   
Connect-AzureAD

#Create an application (you can instead use an existing application if you would like)
$App = New-AzureADApplication -DisplayName “test app name” -IdentifierUris https://testapp

#Create a service principal
New-AzureADServicePrincipal -AppId $App.AppId

#Create an extension property
New-AzureADApplicationExtensionProperty -ObjectId $App.ObjectId -Name “TestAttributeName” -DataType “String” -TargetObjects “User”

#List users in your tenant to determine the objectid for your user
Get-AzureADUser

#Set a value for the extension property on the user. Replace the objectid with the id of the user and the extension name with the value from the previous step
Set-AzureADUserExtension -objectid 0ccf8df6-62f1-4175-9e55-73da9e742690 -ExtensionName “extension_6552753978624005a48638a778921fan3_TestAttributeName”

#Verify that the attribute was added correctly.
Get-AzureADUser -ObjectId 0ccf8df6-62f1-4175-9e55-73da9e742690 | Select -ExpandProperty ExtensionProperty

```

## <a name="next-steps"></a>Pasos siguientes

* [Definir quién está en el ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
