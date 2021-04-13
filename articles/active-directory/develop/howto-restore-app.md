---
title: Restauración o eliminación de una aplicación recientemente eliminada con la Plataforma de identidad de Microsoft | Azure
titleSuffix: Microsoft identity platform
description: En este procedimiento, aprenderá a restaurar o eliminar permanentemente una aplicación eliminada recientemente registrada con la plataforma de Microsoft Identity.
services: active-directory
author: arcrowe
manager: dastrock
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 3/22/2021
ms.author: arcrowe
ms.custom: aaddev
ms.openlocfilehash: dbe3e16bf56c5480fbe4aff8dad78bbbb5591f67
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079917"
---
# <a name="restore-or-remove-a-recently-deleted-application-with-the-microsoft-identity-platform"></a>Restauración o eliminación de una aplicación recientemente eliminada con la Plataforma de Microsoft Identity
Después de eliminar el registro de una aplicación, esta permanece en estado de suspensión durante 30 días. Durante ese período de 30 días, el registro de la aplicación se puede restaurar, junto con todas sus propiedades. Una vez transcurrido ese período de 30 días, los registros de aplicaciones no se pueden restaurar y se puede iniciar automáticamente el proceso de eliminación permanente.  Esta funcionalidad solo se aplica a las aplicaciones asociadas a un directorio.  No está disponible para las aplicaciones de un cuenta Microsoft personal, que no se puede restaurar.

Puede ver las aplicaciones eliminadas, restaurar una aplicación eliminada o eliminar permanentemente una aplicación con la Registros de aplicaciones experiencia en Azure Active Directory (Azure AD) del Azure Portal.

Tenga en cuenta que ni usted ni el soporte al cliente de Microsoft pueden restaurar una aplicación eliminada permanentemente o una aplicación eliminada hace más de 30 días.

> [!IMPORTANT]
> La característica de interfaz de usuario del portal de aplicaciones eliminadas [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

## <a name="required-permissions"></a>Permisos necesarios
Si desea eliminar aplicaciones de manera permanente, debe tener uno de los roles siguientes.

- Administrador global
- Administrador de aplicaciones
- Administrador de aplicaciones en la nube
- Administrador de identidades híbridas
- Propietario de la aplicación

Si desea restaurar aplicaciones, debe tener uno de los roles siguientes.

- Administrador global
- Propietario de la aplicación

### <a name="view-your-deleted-applications"></a>Vista de aplicaciones eliminadas
Puede ver todas las aplicaciones en un estado de eliminación temporal.  Solo se pueden restaurar las aplicaciones eliminadas hace menos de 30 días.

#### <a name="to-view-your-restorable-applications"></a>Para ver las aplicaciones que se pueden restaurar
1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Busque y seleccione **Azure Active Directory**, seleccione **registros de aplicaciones** y, a continuación, seleccione la pestaña **aplicaciones eliminadas (versión preliminar)** .

Permite reorganizar la lista de aplicaciones. Solo se pueden restaurar las aplicaciones que se han eliminado en los últimos 30 días. Si usa la versión preliminar de búsqueda de Registros de aplicaciones, puede filtrar por la columna "fecha de eliminación" para ver solo estas aplicaciones.

## <a name="restore-a-recently-deleted-application"></a>Restauración de una aplicación recién eliminada

Cuando se elimina un registro de aplicación de la organización, la aplicación se encuentra en un estado suspendido y se conservan sus configuraciones. Al restaurar el registro de una aplicación, también se restauran sus configuraciones.  Sin embargo, si hay alguna configuración específica de la organización en **aplicaciones empresariales** para el inquilino principal de la aplicación, no se restaurarán.  

Esto se debe a que la configuración específica de la organización se almacena en un objeto independiente, denominado entidad de servicio.  La configuración contenida en la entidad de servicio incluye permisos y asignaciones de usuarios y grupos para una organización determinada. Estas configuraciones no se restaurarán cuando se restaure la aplicación. Para más información, consulte [Objetos de aplicación y de entidad de servicio](app-objects-and-service-principals.md). 


### <a name="to-restore-an-application"></a>Para restaurar una aplicación
1. En la pestaña **aplicaciones eliminadas (versión preliminar)** , busque y seleccione una de las aplicaciones eliminadas hace menos de 30 días.
2. Seleccione **Restaurar registro de aplicaciones**.

## <a name="permanently-delete-an-application"></a>Eliminar permanentemente una aplicación
Puede eliminar de forma manual una aplicación de la organización. Ni usted, ni otro administrador, ni la asistencia técnica de Microsoft pueden restaurar a una aplicación eliminada permanentemente.

### <a name="to-permanently-delete-an-application"></a>Eliminar permanentemente una aplicación

1. En la pestaña **aplicaciones eliminadas (versión preliminar)** , busque y seleccione una de las aplicaciones disponibles.
2. Seleccione **Eliminar permanentemente**.
3. Lea el texto de la advertencia y seleccione **Sí**.

## <a name="next-steps"></a>Pasos siguientes
Después de haber restaurado o eliminado la aplicación, puede hacer lo siguiente:

- [Agregar una aplicación](quickstart-register-app.md).
- Obtenga más información sobre los [objetos de aplicación y de entidad de servicio](app-objects-and-service-principals.md) en la plataforma de identidad de Microsoft.
