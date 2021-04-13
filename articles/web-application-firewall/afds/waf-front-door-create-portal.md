---
title: 'Tutorial: Creación de una directiva de firewall de aplicaciones web para Azure Front Door: Azure Portal'
description: En este tutorial aprenderá a crear una directiva de firewall de aplicaciones web (WAF) mediante Azure Portal.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 03/31/2021
ms.author: victorh
ms.openlocfilehash: e7b4544530dc9c0c894ae7a0f2b1d2830f895928
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122263"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Tutorial: Creación de una directiva de firewall de aplicaciones web en Azure Front Door mediante Azure Portal

En este tutorial se muestra cómo crear una directiva de Azure Web Application Firewall (WAF) básica y aplicarla a un host de front-end en Azure Front Door.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una directiva WAF
> * Asociarla a un host de front-end
> * Configurar las reglas de WAF

## <a name="prerequisites"></a>Prerrequisitos

Cree una instancia de [Front Door](../../frontdoor/quickstart-create-front-door.md) o un perfil de [Front Door Estándar o Premium](../../frontdoor/standard-premium/create-front-door-portal.md). 

## <a name="create-a-web-application-firewall-policy"></a>Creación de una directiva de firewall de aplicaciones web

En primer lugar, en el portal, cree una directiva WAF con un conjunto de reglas predeterminado (DRS) administrado. 

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso**, busque **WAF**, seleccione **Firewall de aplicaciones web (WAF)** y seleccione **Crear**.

1. En la pestaña **Datos básicos** de la página **Crear una directiva WAF**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Directiva de              | Seleccione **WAF global (Front Door)** . |
    | SKU de Front Door          | Seleccione entre la SKU Básica, Estándar y Premium. |
    | Subscription            | Seleccione el nombre de la suscripción a Front Door.|
    | Resource group          | Seleccione el nombre del grupo de recursos de Front Door.|
    | Nombre de la directiva             | Escriba un nombre único para la directiva WAF.|
    | Estado de directiva            | Establézcalo como **Habilitado**. | 

   :::image type="content" source="../media/waf-front-door-create-portal/basic.png" alt-text="Captura de pantalla de la página Crear una directiva de W A F, con el botón Revisar y crear, y cuadros de lista para la suscripción, el grupo de recursos y el nombre de la directiva.":::

1. En la pestaña **Association** (Asociación) de la página **Create a WAF policy** (Crear una directiva WAF), seleccione **+ Associate a Front Door profile** (+ Asociar un perfil de Front Door), escriba la siguiente configuración y, a continuación, seleccione **Add** (Agregar):

    | Configuración                 | Valor                                              |
    | ---                     | ---                                                |
    | Perfil de Front Door              | Seleccione el nombre de su perfil de Front Door. |
    | Dominios          | Seleccione los dominios a los que desea asociar la directiva WAF y, a continuación, seleccione **Agregar**. |

    :::image type="content" source="../media/waf-front-door-create-portal/associate-profile.png" alt-text="Captura de pantalla de la página para asociar un perfil de Front Door.":::
    
    > [!NOTE]
    > Si el dominio está asociado a una directiva WAF, se muestra como atenuado. Debe quitar primero el dominio de la directiva asociada y, a continuación, volver a asociarlo a una nueva directiva WAF.

1. Seleccione **Revisar y crear** y, luego, **Crear**.

## <a name="configure-web-application-firewall-rules-optional"></a>Configuración de las reglas del firewall de aplicaciones web (opcional)

### <a name="change-mode"></a>Cambio del modo

Cuando se crea una directiva WAF, de forma predeterminada, esta está en modo **Detección**. En el modo **Detección**, WAF no bloquea las solicitudes, sino que las solicitudes que coinciden con las reglas de WAF se registran en los registros de WAF.
Para ver WAF en acción, puede cambiar la configuración del modo de **Detección** a **Prevención**. En el modo **Prevención**, las solicitudes que coinciden con las reglas que se definen en el conjunto de reglas predeterminado (DRS) se bloquean y se registran en los registros de WAF.

 :::image type="content" source="../media/waf-front-door-create-portal/policy.png" alt-text="Captura de pantalla de la sección de configuración de la directiva. El conmutador Modo está establecido en Prevención.":::

### <a name="custom-rules"></a>Reglas personalizadas

Puede crear una regla personalizada si selecciona **Agregar regla personalizada** bajo la sección **Reglas personalizadas**. De esta forma se inicia la página de configuración de reglas personalizadas. 

:::image type="content" source="../media/waf-front-door-create-portal/custom-rules.png" alt-text="Captura de pantalla de la página de reglas personalizadas.":::

A continuación se presenta un ejemplo de cómo configurar una regla personalizada para bloquear una solicitud si la cadena de consulta contiene **blockme**.

:::image type="content" source="../media/waf-front-door-create-portal/customquerystring2.png" alt-text="Captura de pantalla de la página de configuración de regla personalizada que muestra la configuración de una regla que comprueba si la variable QueryString contiene el valor blockme.":::

### <a name="default-rule-set-drs"></a>Conjunto de reglas predeterminado (DRS)

El conjunto de reglas predeterminado administrado por Azure está habilitado de forma predeterminada. La versión predeterminada actual es DefaultRuleSet_1.0. En WAF, en **Reglas administradas**, **Asignar**, el conjunto de reglas Microsoft_DefaultRuleSet_1.1 está disponible en la lista desplegable.

Para deshabilitar una regla individual, seleccione la **casilla** situada junto al número de la regla y seleccione **Deshabilitar** en la parte superior de la página. Para cambiar los tipos de acciones para reglas individuales dentro del conjunto de reglas, active la casilla situada junto al número de la regla y, a continuación, seleccione la opción **Cambiar acción** en la parte superior de la página.

:::image type="content" source="../media/waf-front-door-create-portal/managed-rules.png" alt-text="Captura de pantalla de la página de reglas administradas que muestra un conjunto de reglas, grupos de reglas, reglas y los botones Habilitar, Deshabilitar y Cambiar acción." lightbox="../media/waf-front-door-create-portal/managed-rules-expanded.png":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos y todos los recursos relacionados.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre Azure Front Door](../../frontdoor/front-door-overview.md)
> [Más información sobre Azure Front Door Estándar y Premium](../../frontdoor/standard-premium/overview.md)
