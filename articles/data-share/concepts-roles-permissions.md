---
title: Roles y requisitos de Azure Data Share
description: Obtenga información sobre los permisos necesarios para compartir y recibir datos con Azure Data Share.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: conceptual
ms.date: 03/24/2021
ms.openlocfilehash: a832c8956f7a3d4f8669209d7ed311e7555e1e75
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105644245"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Roles y requisitos de Azure Data Share 

En este artículo se describen los roles y los permisos necesarios para compartir y recibir datos mediante el servicio Azure Data Share. 

## <a name="roles-and-requirements"></a>Roles y requisitos

Con el servicio de Azure Data Share, puede compartir datos sin intercambiar credenciales entre el proveedor de datos y el consumidor. En el caso del uso compartido basado en instantáneas, el servicio Azure Data Share usa identidades administradas (anteriormente conocidas como MSI) para autenticarse en el almacén de datos de Azure. Se debe conceder a la identidad administrada del recurso de Azure Data Share acceso al almacén de datos de Azure para leer o escribir datos.

Para compartir o recibir datos de un almacén de datos de Azure, el usuario necesita al menos los permisos siguientes. 

* Permiso para escribir en el almacén de datos de Azure. Generalmente, este permiso existe en el rol **Colaborador**.

En el caso del uso compartido basado en instantáneas de almacenamiento y de lago de datos, también necesita permiso para crear la asignación de roles en el almacén de datos de Azure. Normalmente, el permiso para crear asignaciones de roles existe en el rol **Propietario**, en el rol Administrador de acceso de usuario o en un rol personalizado con el permiso *Microsoft.Authorization/asignación de roles/escritura* asignado. Este permiso no es necesario si ya se concedió a la identidad administrada del recurso compartido de datos acceso al almacén de datos de Azure. A continuación se muestra un resumen de los roles asignados a la identidad administrada del recurso compartido de datos:

|**Tipo de almacén de datos**|**Almacén de datos de origen del proveedor de datos**|**Almacén de datos de destino del consumidor de datos**|
|---|---|---|
|Azure Blob Storage| Lector de datos de blobs de almacenamiento | Colaborador de datos de blobs de almacenamiento
|Azure Data Lake Gen1 | Propietario | No compatible
|Azure Data Lake Gen2 | Lector de datos de blobs de almacenamiento | Colaborador de datos de blobs de almacenamiento
|

En el caso del uso compartido basado en instantáneas de SQL, se debe crear un usuario de SQL a partir de un proveedor externo de Azure SQL Database con el mismo nombre que el recurso de Azure Data Share. Se requiere el permiso de administrador de Azure Active Directory para crear este usuario. A continuación se muestra un resumen del permiso requerido por el usuario de SQL.

|**Tipo de base de datos SQL**|**Permiso de usuario de SQL del proveedor de datos**|**Permiso de usuario de SQL del consumidor de datos**|
|---|---|---|
|Azure SQL Database | db_datareader | db_datareader, db_datawriter, db_ddladmin
|Azure Synapse Analytics | db_datareader | db_datareader, db_datawriter, db_ddladmin
|

### <a name="data-provider"></a>Proveedor de datos
Si el uso compartido se basa en instantáneas de almacenamiento o de lago de datos, para agregar un conjunto de datos en Azure Data Share, se debe conceder a la identidad administrada del recurso compartido de datos del proveedor acceso al almacén de datos de Azure de origen. Por ejemplo, en el caso de la cuenta de almacenamiento, a la identidad administrada del recurso compartido de datos se le concede el rol *Lector de datos de Storage Blob*. Esto lo hace automáticamente el servicio de Azure Data Share cuando el usuario agrega un conjunto de datos a través de Azure Portal y tiene el permiso adecuado. Por ejemplo, el usuario es propietario del almacén de datos de Azure o miembro de un rol personalizado que tiene asignado el permiso *Microsoft.Authorization/asignación de roles/escritura*. 

Como alternativa, el usuario puede hacer que el propietario del almacén de datos de Azure agregue manualmente la identidad administrada del recurso compartido de datos al almacén de datos de Azure. Esta acción solo debe realizarse una vez por cada recurso compartido de datos. Para crear una asignación de roles manualmente para la identidad administrada del recurso compartido de datos, siga estos pasos.  

1. Vaya al almacén de datos de Azure.
1. Seleccione **Access Control (IAM)** .
1. Seleccione **Agregar una asignación de roles**.
1. En *Rol*, seleccione el rol en la tabla de asignación de roles anterior (por ejemplo, en cuenta de almacenamiento, seleccione *Lector de datos de blobs de almacenamiento*).
1. En *Seleccionar*, escriba el nombre del recurso de Azure Data Share.
1. Haga clic en *Save*(Guardar).

Para más información sobre las asignaciones de roles, consulte [Asignación de roles mediante Azure Portal](../role-based-access-control/role-assignments-portal.md). Consulte [Asignación de roles de Azure mediante la API REST](../role-based-access-control/role-assignments-rest.md) si comparte datos empleando las API REST y quiere crear una asignación de roles mediante la API. 

Si el uso compartido se basa en instantáneas de SQL, se debe crear un usuario de SQL a partir de un proveedor externo en SQL Database con el mismo nombre que el recurso de Azure Data Share al conectarse a la base de datos SQL mediante la autenticación de Azure Active Directory. Se debe conceder a este usuario el permiso *db_datareader*. Puede encontrar un script de ejemplo junto con otros requisitos previos para el uso compartido basado en SQL en el tutorial [Uso compartido desde Azure SQL Database o Azure Synapse Analytics](how-to-share-from-sql.md). 

### <a name="data-consumer"></a>Consumidor de datos
Para recibir datos en la cuenta de almacenamiento, se debe conceder a la identidad administrada del recurso compartido de datos del consumidor acceso a la cuenta de almacenamiento de destino. A la identidad administrada del recurso compartido de datos se le debe conceder el rol *Lector de datos de Storage Blob*. El servicio Azure Data Share hace esto automáticamente si el usuario especifica una cuenta de almacenamiento de destino mediante Azure Portal y tiene el permiso adecuado. Por ejemplo, el usuario es propietario de la cuenta de almacenamiento o es miembro de un rol personalizado que tiene asignado el permiso *Microsoft.Authorization/asignación de roles/escritura*. 

Como alternativa, el usuario puede hacer que el propietario de la cuenta de almacenamiento agregue manualmente la identidad administrada del recurso compartido de datos a dicha cuenta. Esta acción solo debe realizarse una vez por cada recurso compartido de datos. Para crear una asignación de roles manualmente para la identidad administrada del recurso compartido de datos, siga estos pasos. 

1. Vaya al almacén de datos de Azure.
1. Seleccione **Access Control (IAM)** .
1. Seleccione **Agregar una asignación de roles**.
1. En *Rol*, seleccione el rol en la tabla de asignación de roles anterior (por ejemplo, en cuenta de almacenamiento, seleccione *Lector de datos de blobs de almacenamiento*).
1. En *Seleccionar*, escriba el nombre del recurso de Azure Data Share.
1. Haga clic en *Save*(Guardar).

Para más información sobre las asignaciones de roles, consulte [Asignación de roles mediante Azure Portal](../role-based-access-control/role-assignments-portal.md). Consulte [Asignación de roles de Azure mediante la API REST](../role-based-access-control/role-assignments-rest.md) si recibe datos empleando las API REST y quiere crear una asignación de roles mediante la API. 

En el caso de destinos basados en SQL, se debe crear un usuario de SQL a partir de un proveedor externo en SQL Database con el mismo nombre que el recurso de Azure Data Share a conectar a SQL Database mediante la autenticación de Azure Active Directory. Se debe conceder a este usuario el permiso *db_datareader, db_datawriter, db_ddladmin*. Puede encontrar un script de ejemplo junto con otros requisitos previos para el uso compartido basado en SQL en el tutorial [Uso compartido desde Azure SQL Database o Azure Synapse Analytics](how-to-share-from-sql.md). 

## <a name="resource-provider-registration"></a>Registro del proveedor de recursos 

Es posible que tenga que registrar manualmente el proveedor de recursos Microsoft.DataShare en la suscripción de Azure en los escenarios siguientes: 

* Consulte la invitación de Azure Data Share por primera vez en su inquilino de Azure.
* Opción para compartir datos de un almacén de datos de Azure en una suscripción de Azure diferente del recurso de Azure Data Share
* Recepción de datos en un almacén de datos de Azure en una suscripción de Azure diferente del recurso de Azure Data Share

Siga estos pasos para registrar el proveedor de recursos Microsoft.DataShare en la suscripción de Azure. Necesita acceso de *Colaborador* a la suscripción de Azure para registrar el proveedor de recursos.

1. En Azure Portal, vaya a **Suscripciones**.
1. Seleccione la suscripción que va a usar para Azure Data Share.
1. Haga clic en **Proveedores de recursos**
1. Busque Microsoft.DataShare.
1. Haga clic en **Registrar**.
 
Para más información sobre los proveedores de recursos, vea [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información acerca de los roles en Azure: [Descripción de las definiciones de roles de Azure](../role-based-access-control/role-definitions.md)