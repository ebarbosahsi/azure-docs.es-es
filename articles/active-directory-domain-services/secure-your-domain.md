---
title: Protección de Azure AD Domain Services | Microsoft Docs
description: Aprenda a deshabilitar los cifrados poco seguros, los protocolos antiguos y la sincronización de hash de contraseñas de NTML para un dominio administrado de Azure Active Directory Domain Services.
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/08/2021
ms.author: justinha
ms.openlocfilehash: 5fa19e23767af0e121d07872970199a2a1705ea8
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "104951947"
---
# <a name="disable-weak-ciphers-and-password-hash-synchronization-to-secure-an-azure-active-directory-domain-services-managed-domain"></a>Deshabilitación de los cifrados poco seguros y de la sincronización de hash de contraseñas para proteger un dominio administrado de Azure Active Directory Domain Services

De forma predeterminada, Azure Active Directory Domain Services (Azure AD DS) permite el uso de cifrados tales como NTLM V1 y TLS v1. Aunque estos cifrados pueden ser necesarios para algunas aplicaciones heredadas, se consideran poco seguros y se pueden deshabilitar si no se necesitan. Si tiene conectividad híbrida local mediante Azure AD Connect, también puede deshabilitar la sincronización de los hash de contraseñas de NTLM.

En este artículo se muestra cómo deshabilitar los cifrados NTLM V1 y TLS v1 y la sincronización de hash de contraseñas de NTLM.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesita los siguientes recursos:

* Una suscripción de Azure activa.
    * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a su suscripción, ya sea sincronizado con un directorio en el entorno local o con un directorio solo en la nube.
    * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Un dominio administrado de Azure Active Directory Domain Services habilitado y configurado en su inquilino de Azure AD.
    * Si es necesario, [cree y configure un dominio administrado de Azure Active Directory Domain Services][create-azure-ad-ds-instance].

## <a name="use-security-settings-to-disable-weak-ciphers-and-ntlm-password-hash-sync"></a>Uso de la configuración de seguridad para deshabilitar los cifrados débiles y la sincronización de hash de contraseñas de NTLM

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En Azure Portal, seleccione **Azure AD Domain Services**.
1. Elija el dominio administrado como, por ejemplo, *aaddscontoso.com*.
1. En el lado izquierdo, seleccione **Configuración de seguridad**.
1. Haga clic en **Deshabilitar** para realizar la configuración siguiente:
   - **Modo de solo TLS 1.2**
   - **Autenticación NTLM**
   - **Sincronización de contraseñas de NTLM desde el entorno local**

   ![Captura de pantalla de la configuración de seguridad para deshabilitar los cifrados débiles y la sincronización de hash de contraseñas de NTLM](media/secure-your-domain/security-settings.png)

## <a name="use-powershell-to-disable-weak-ciphers-and-ntlm-password-hash-sync"></a>Uso de PowerShell para deshabilitar cifrados poco seguros y la sincronización de hash de contraseñas de NTLM

Si lo necesita, [instale y configure Azure PowerShell](/powershell/azure/install-az-ps). Asegúrese de que inicia sesión en su suscripción de Azure con el cmdlet [Connect-AzAccount][Connect-AzAccount]. 

Igualmente, [instale y configure Azure AD PowerShell](/powershell/azure/active-directory/install-adv2) si fuera necesario. Asegúrese de iniciar sesión en el inquilino de Azure AD mediante el cmdlet [Connect-AzureAD][Connect-AzureAD].

Para deshabilitar los conjuntos de cifrados poco seguros y la sincronización de hash de credenciales de NTLM, inicie sesión en su cuenta de Azure y, luego, obtenga el recurso de Azure AD DS con el cmdlet [Get-AzResource][Get-AzResource]:

> [!TIP]
> Si se produce un error al usar el comando [Get-AzResource][Get-AzResource] que se indica que el recurso *Microsoft.AAD/DomainServices* no existe, [eleve los privilegios de acceso para administrar todas las suscripciones y grupos de administración de Azure][global-admin].

```powershell
Login-AzAccount

$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"
```

Luego, defina *DomainSecuritySettings* para configurar las siguientes opciones de seguridad:

1. Deshabilite la compatibilidad con NTLM v1.
2. Deshabilitar la sincronización de códigos hash de contraseñas NTLM desde su instancia de Active Directory local.
3. Deshabilite TLS v1.

> [!IMPORTANT]
> Si deshabilita la sincronización de hash de contraseñas de NTML en el dominio administrado de Azure AD DS, los usuarios y las cuentas de servicio no pueden realizar enlaces simples LDAP. Si necesita realizar enlaces simples LDAP, no establezca la opción de configuración de seguridad *"SyncNtlmPasswords"="Disabled";* del siguiente comando.

```powershell
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}
```

Por último, aplique la configuración de seguridad definida al dominio administrado con el cmdlet [Set-AzResource][Set-AzResource]. Especifique el recurso de Azure AD DS del primer paso y la configuración de seguridad del paso anterior.

```powershell
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

La configuración de seguridad tarda unos minutos en aplicarse al dominio administrado.

> [!IMPORTANT]
> Después de desactivar NTLM, efectúe una sincronización completa de hash de contraseñas en Azure AD Connect para quitar todos los valores hash de contraseña del dominio administrado. Si desactiva NTLM pero no fuerza la sincronización de hash de contraseñas, los valores hash de contraseña de NTLM para una cuenta de usuario solo se eliminarán en el siguiente cambio de contraseña. Este comportamiento podría permitir a un usuario iniciar sesión si las credenciales están almacenadas en caché en un sistema que utiliza NTLM como método de autenticación.
>
> Cuando el hash de contraseña NTLM es diferente al hash de contraseña de Kerberos, recurrir a NTLM no será una alternativa. Las credenciales almacenadas en caché tampoco funcionarán si la máquina virtual tiene conectividad con el controlador de dominio administrado.  

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el proceso de sincronización, consulte [Cómo sincronizar objetos y credenciales en un dominio administrado][synchronization].

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[global-admin]: ../role-based-access-control/elevate-access-global-admin.md
[synchronization]: synchronization.md

<!-- EXTERNAL LINKS -->
[Get-AzResource]: /powershell/module/az.resources/Get-AzResource
[Set-AzResource]: /powershell/module/Az.Resources/Set-AzResource
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD
