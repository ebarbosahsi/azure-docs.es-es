---
title: Uso compartido de paneles de Azure Portal mediante el control de acceso basado en roles de Azure
description: En este artículo se explica cómo compartir un panel en Azure Portal mediante el control de acceso basado en roles de Azure.
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.topic: how-to
ms.date: 03/19/2021
ms.openlocfilehash: 336bfe9792c5ba4246458368e008a8c50833ed03
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104771554"
---
# <a name="share-azure-dashboards-by-using-azure-role-based-access-control"></a>Uso compartido de paneles de Azure mediante el control de acceso basado en rol de Azure

Después de configurar un panel, puede publicarlo y compartirlo con otros usuarios de su organización. Mediante el [control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/role-assignments-portal.md) puede permitir que otros usuarios vean su panel. Asigne un solo usuario o un grupo de usuarios a un rol. Ese rol define si esos usuarios pueden ver o modificar el panel publicado.

Todos los paneles publicados se implementan como recursos de Azure. Existen como elementos administrables dentro de su suscripción y están contenidos en un grupo de recursos. Desde una perspectiva de control de acceso, los paneles no son distintos de otros recursos, como una máquina virtual o una cuenta de almacenamiento. Los iconos individuales del panel tendrán sus propios requisitos de control de acceso en función de los recursos que muestran. Puede compartir un panel de manera generalizada y al mismo tiempo proteger los datos de iconos individuales.

## <a name="understanding-access-control-for-dashboards"></a>Descripción del control de acceso para los paneles

Mediante el control de acceso basado en roles de Azure (Azure RBAC), puede asignar a usuarios a los roles en tres niveles distintos de ámbito:

* subscription
* resource group
* resource

Los permisos que asigne se heredan desde la suscripción hasta el recurso. El panel publicado es un recurso. Es posible que ya tenga usuarios asignados a roles de la suscripción que se apliquen al panel publicado.

Supongamos que tiene una suscripción de Azure y que a varios miembros del equipo se les han asignado los roles de *propietario*, *colaborador* o *lector* en la suscripción. Los usuarios que son propietarios o colaboradores pueden enumerar, ver, crear, modificar o eliminar paneles dentro de la suscripción. Los usuarios que son lectores pueden enumerar y ver los paneles, pero no pueden modificarlos ni eliminarlos. Los usuarios con acceso de lector pueden realizar modificaciones locales en un panel publicado (por ejemplo, para solucionar un problema), pero no pueden publicar esos cambios en el servidor. Pueden realizar una copia privada del panel para sí mismos.

Podría asignar permisos al grupo de recursos que contenga varios paneles o a un solo panel. Por ejemplo, puede decidir que un grupo de usuarios tenga permisos limitados a través de la suscripción pero un mayor acceso a un panel determinado. Asigne a esos usuarios un rol de ese panel.

## <a name="publish-a-dashboard"></a>Publicación de un panel

Supongamos que configura un panel que quiere compartir con un grupo de usuarios de la suscripción. En los pasos siguientes se muestra cómo compartir un panel con un grupo llamado Administradores de almacenamiento. Puede asignar al grupo el nombre que desee. Para más información, consulte [Administración de grupos en Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Antes de asignar el acceso, debe publicar el panel.

1. En el panel, seleccione **Compartir**.

    ![seleccionar compartir para el panel](./media/azure-portal-dashboard-share-access/share-dashboard-for-access-control.png)

1. En **Uso compartido y control de acceso**, seleccione **Publicar**.

    ![publicar el panel](./media/azure-portal-dashboard-share-access/publish-dashboard-for-access-control.png)

     De forma predeterminada, se publica el panel en un grupo de recursos denominado **paneles**. Para seleccionar otro grupo de recursos, borre la casilla.

Tras esto, el panel ya se ha publicado. Si son adecuados los permisos heredados de la suscripción, ya no es necesario hacer nada más. Otros usuarios de la organización pueden acceder al panel y modificarlo según su rol de nivel de suscripción.

## <a name="assign-access-to-a-dashboard"></a>Asignación de acceso a un panel

Puede asignar un rol a un grupo de usuarios de ese panel.

1. Después de publicar el panel, seleccione **Administrar uso compartido**.

1. En **Control de acceso**, seleccione **Asignaciones de roles** para ver los usuarios existentes que ya tienen asignado un rol en este panel.

1. Para agregar un nuevo usuario o grupo, seleccione **Agregar** y, a continuación, **Agregar asignación de roles**.

    ![agregar un usuario para el acceso al panel](./media/azure-portal-dashboard-share-access/manage-users-existing-users.png)

1. Seleccione el rol que representa los permisos que se van a conceder, como **Colaborador**.

1. Seleccione el usuario o grupo al que quiere asignar el rol. Si no ve el usuario o grupo que busca en la lista, use el cuadro de búsqueda. La lista de grupos disponibles depende de los grupos que haya creado en Active Directory.

1. Cuando haya terminado de agregar usuarios o grupos, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una lista de roles, vea [Roles integrados de Azure](../role-based-access-control/built-in-roles.md).
* Para más información sobre cómo administrar recursos, consulte [Administración de recursos de Azure con Azure Portal](../azure-resource-manager/management/manage-resources-portal.md).