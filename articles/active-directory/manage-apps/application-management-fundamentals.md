---
title: 'Administración de aplicaciones: Procedimientos recomendados y recomendaciones | Microsoft Docs'
description: Obtenga información sobre los procedimiento recomendados y recomendaciones para administrar aplicaciones en Azure Active Directory. Aprenda a usar el aprovisionamiento automático y la publicación de aplicaciones locales con el proxy de aplicación.
services: active-directory
documentationcenter: ''
author: kenwith
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2019
ms.subservice: app-mgmt
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23c688d9b2e118ef29303d435bb83ef02ad36105
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99259141"
---
# <a name="application-management-best-practices"></a>Procedimientos recomendados de administración de aplicaciones

Este artículo contiene recomendaciones y procedimientos recomendados para administrar aplicaciones en Azure Active Directory (Azure AD), usar el aprovisionamiento automático y publicar aplicaciones locales con el proxy de aplicación.

## <a name="cloud-app-and-single-sign-on-recommendations"></a>Recomendaciones para la aplicación en la nube y el inicio de sesión único
| Recomendación | Comentarios |
| --- | --- |
| Consultar la galería de aplicaciones de Azure AD para aplicaciones  | Azure AD dispone de una galería que contiene miles de aplicaciones previamente integradas que se habilitan con el inicio de sesión único empresarial (SSO). Para obtener instrucciones sobre la configuración específica de la aplicación, consulte la [Lista de tutoriales de aplicaciones SaaS](../saas-apps/tutorial-list.md).  | 
| Usar SSO basado en SAML federado  | Cuando una aplicación lo admita, use el SSO basado en SAML federado con Azure AD en lugar de SSO basado en contraseña y ADFS.  | 
| Usar SHA-256 para la firma de certificados  | Azure AD utiliza el algoritmo predeterminado SHA-256 para firmar la respuesta SAML. Use SHA-256 a menos que la aplicación requiera SHA-1 (consulte [Opciones de firma de certificados](certificate-signing-options.md) y [Problema de inicio de sesión de la aplicación](application-sign-in-problem-application-error.md)).  | 
| Requerir la asignación de usuario  | De forma predeterminada, los usuarios pueden acceder a sus aplicaciones empresariales sin tener que asignarlas. Sin embargo, si la aplicación expone roles o si quiere que aparezca en la página Aplicaciones de un usuario, exija la asignación de usuarios.  | 
| Implementación de la página Aplicaciones en los usuarios | La página [Aplicaciones](end-user-experiences.md) en `https://myapps.microsoft.com` es un portal web que proporciona a los usuarios un único punto de entrada para las aplicaciones basadas en la nube asignadas. A medida que se agreguen funcionalidades adicionales, como la administración de grupos y el autoservicio de restablecimiento de contraseña, los usuarios pueden encontrarlas en dicho portal. Consulte [Planeamiento de una implementación de Aplicaciones](my-apps-deployment-plan.md).
| Usar asignación de un grupo  | Si se incluye en la suscripción, asigne grupos a una aplicación para que pueda delegar la administración de acceso continuo al propietario del grupo.  | 
| Establecer un proceso para administrar certificados | La duración máxima de un certificado de firma es de tres años. Para evitar o minimizar la interrupción debido a la expiración de un certificado, use las listas de distribución de correo electrónico y roles para asegurarse de que las notificaciones de cambios relacionadas con los certificados se supervisan con atención. |

## <a name="provisioning-recommendations"></a>Aprovisionar las recomendaciones
| Recomendación | Comentarios |
| --- | --- |
| Usar tutoriales para configurar el aprovisionamiento con aplicaciones en la nube | Consulte la [Lista de tutoriales de aplicaciones SaaS](../saas-apps/tutorial-list.md) para obtener instrucciones paso a paso sobre cómo configurar el aprovisionamiento de la aplicación de la galería que desea agregar. |
| Usar registros de aprovisionamiento (versión preliminar) para supervisar el estado | Los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) proporcionan detalles sobre todas las acciones que ha llevado a cabo el servicio de aprovisionamiento, incluido el estado de los usuarios individuales. |
| Asignación de un grupo de distribución al correo electrónico de notificación de aprovisionamiento | Para aumentar la visibilidad de las alertas críticas enviadas por el servicio de aprovisionamiento, asigne un grupo de distribución a la configuración de los correos electrónicos de notificación. |


## <a name="application-proxy-recommendations"></a>Recomendaciones sobre el proxy de aplicación
| Recomendación | Comentarios |
| --- | --- |
| Uso del proxy de aplicación para el acceso remoto a recursos internos | Se recomienda el proxy de aplicación para dar a los usuarios remotos acceso a recursos internos, reemplazando la necesidad de una VPN o un proxy inverso. No está diseñado para tener acceso a recursos desde dentro de la red corporativa porque podría agregar latencia.
| Uso de dominios personalizados | Configure dominios personalizados para las aplicaciones (consulte [Configurar dominios personalizados](application-proxy-configure-custom-domain.md)) para que las direcciones URL de los usuarios y entre las aplicaciones funcionen desde dentro o fuera de la red. También podrá controlar la personalización de marca y personalizar las direcciones URL.  Al usar nombres de dominio personalizados, planee adquirir un certificado público de una entidad de certificación de confianza que no sea de Microsoft. Azure Application Proxy admite certificados estándar, ([comodín](application-proxy-wildcard.md)) o basados en SAN. (Consulte [Planeación de Application Proxy](application-proxy-deployment-plan.md)). |
| Sincronizar usuarios antes de implementar Application Proxy | Antes de implementar Application Proxy, sincronice las identidades de usuario desde un directorio local o créelos directamente en Azure AD. La Sincronización de identidades permite a Azure AD realizar una autenticación previa de los usuarios antes de concederles acceso a aplicaciones publicadas en App Proxy. También proporciona la información de identificador de usuario necesaria para realizar el inicio de sesión único (SSO). (Consulte [Planeación de Application Proxy](application-proxy-deployment-plan.md)). |
| Siga nuestras sugerencias para lograr alta disponibilidad y equilibrio de carga | Para saber cómo fluye el tráfico entre los usuarios, los conectores de Application Proxy y los servidores de aplicaciones de back-end, y obtener sugerencias para optimizar el rendimiento y el equilibrio de carga, consulte [alta disponibilidad y equilibrio de carga de los conectores y las aplicaciones Application Proxy](application-proxy-high-availability-load-balancing.md). |
| Usar varios conectores | Use dos o más conectores Application Proxy para obtener una mayor resistencia, disponibilidad y escala (consulte [Conectores de Application Proxy](application-proxy-connectors.md)). Cree grupos de conectores y asegúrese de que cada grupo de conectores tiene al menos dos conectores (tres conectores es óptimo). |
| Ubicar los servidores de conectores cerca de los servidores de aplicaciones y asegurarse de que están en el mismo dominio | Para optimizar el rendimiento, ubique físicamente el servidor de conector cerca de los servidores de aplicación (consulte [Consideraciones sobre la topología de red](application-proxy-network-topology.md)). También, el servidor del conector y los servidores de aplicaciones web deben pertenecer al mismo dominio de Active Directory o abarcar dominios de confianza. Esta configuración es necesaria para SSO con la autenticación integrada de Windows (IWA) y la delegación restringida de Kerberos (KCD). Si los servidores están en dominios diferentes, deberá utilizar la delegación basada en recursos para SSO (consulte [KCD para el inicio de sesión único con Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)). |
| Habilitar actualizaciones automáticas para conectores | Habilite las actualizaciones automáticas de los conectores para obtener las últimas características y correcciones de errores. Microsoft proporciona soporte técnico directo para la versión más reciente del conector y una versión anterior. (Consulte [Historial de versiones de Application Proxy](application-proxy-release-version-history.md)). |
| Omitir el proxy local | Para un mantenimiento más sencillo, configure el conector para omitir el proxy local para que se conecte directamente a los servicios de Azure. (Consulte [Conectores de Application Proxy y servidores proxy](application-proxy-configure-connectors-with-proxy-servers.md)). |
| Usar Azure AD Application Proxy sobre Web Application Proxy | Use Azure AD Application Proxy para la mayoría de los escenarios locales. Web Application Proxy solo se prefiere en los escenarios que requieran un servidor proxy para AD FS y donde no pueda usar dominios personalizados en Azure Active Directory. (Consulte [Migración de Application Proxy](application-proxy-migration.md)). |