---
title: Copia de seguridad y restauración en Azure Database for MariaDB mediante la CLI de Azure
description: Copia de seguridad y restauración de un servidor en Azure Database for MariaDB mediante la CLI de Azure.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.devlang: azurecli
ms.topic: how-to
ms.date: 3/27/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: a6e46efd7f998437c3998df9a989ef9e1500e888
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98664841"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-cli"></a>Copia de seguridad y restauración de un servidor en Azure Database for MariaDB mediante la CLI de Azure

Periódicamente, se realizan copias de seguridad de los servidores de Azure Database for MariaDB para habilitar las características de restauración. Con esta característica, puede restaurar el servidor y todas sus bases de datos en un servidor nuevo a un momento dado anterior.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Necesita un [servidor y una base de datos de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Esta guía de procedimientos requiere la versión 2.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="set-backup-configuration"></a>Configuración de copia de seguridad

Elija la configuración del servidor para copias de seguridad con redundancia local o con redundancia geográfica en el momento de crear el servidor.

> [!NOTE]
> Después de crear un servidor, no se puede cambiar el tipo de redundancia elegido, redundancia geográfica o redundancia local.
>

Al crear un servidor mediante el comando `az mariadb server create`, el parámetro `--geo-redundant-backup` decide la opción de redundancia de copia de seguridad. Si `Enabled`, se toman las copias de seguridad con redundancia geográfica. O si `Disabled`, se toman las copias de seguridad con redundancia local.

El período de retención de la copia de seguridad se configura mediante el parámetro `--backup-retention`.

Para más información acerca de cómo establecer estos valores durante la creación, consulte la [Guía de inicio rápido del servidor de Azure Database for MariaDB mediante la CLI](quickstart-create-mariadb-server-database-using-azure-cli.md).

El período de retención de copia de seguridad de un servidor se puede cambiar de la forma siguiente:

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

En el ejemplo anterior se cambia el período de retención de copia de seguridad de mydemoserver a 10 días.

El período de retención de copia de seguridad rige durante cuánto tiempo se puede realizar una restauración a un momento dado, porque se basa en las copias de seguridad disponibles. La restauración a un momento dado se describe con más detalle en la sección siguiente.

## <a name="server-point-in-time-restore"></a>Restauración del servidor a un momento dado

Puede restaurar el servidor a un momento dado anterior. Los datos restaurados se copian en un nuevo servidor y el existente se queda tal cual. Por ejemplo, si una tabla se eliminó por error hoy a mediodía, puede restaurar hasta el momento justo antes del mediodía. Así podrá recuperar la tabla y los datos que faltan de la copia restaurada del servidor.

Para restaurar el servidor, utilice el comando [az mariadb server restore](/cli/azure/mariadb/server#az-mariadb-server-restore) de la CLI de Azure.

### <a name="run-the-restore-command"></a>Ejecutar el comando restore

Para restaurar el servidor, en el símbolo del sistema de la CLI de Azure, escriba el siguiente comando:

```azurecli-interactive
az mariadb server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

El comando `az mariadb server restore` requiere los siguientes parámetros:

| Configuración | Valor sugerido | Descripción  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupo de recursos donde existe el servidor de origen.  |
| name | mydemoserver-restored | Nombre del nuevo servidor que se crea mediante el comando de restauración. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Seleccione un momento dado anterior para restaurar. Esta fecha y hora debe estar dentro del período de retención de copia de seguridad del servidor de origen. Use el formato de fecha y hora ISO8601. Por ejemplo, puede usar su propia zona horaria, como `2018-03-13T05:59:00-08:00`. También puede utilizar el formato de hora Zulú UTC, por ejemplo, `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | Nombre o identificador del servidor de origen desde el que se va a restaurar. |

Cuando se restaura un servidor a un momento dado anterior, se crea un nuevo servidor. El servidor de origen y las bases de datos de ese momento dado anterior se copian en el servidor nuevo.

Los valores de ubicación y plan de tarifa del servidor restaurado son los mismos que los del servidor de origen. 

Una vez finalizada la restauración, busque el servidor nuevo y compruebe que los datos se restauraron según lo previsto. El nuevo servidor tiene el mismo nombre de inicio de sesión y contraseña de administrador del servidor que el servidor existente tenía cuando se inició la restauración. La contraseña se puede cambiar en la página **Información general** del nuevo servidor.

El servidor creado durante una restauración no tiene los puntos de conexión de servicio de red virtual que existían en el servidor original. Estas reglas deben configurarse por separado para este nuevo servidor. Se restauran las reglas de firewall del servidor original.

## <a name="geo-restore"></a>Restauración geográfica

Si ha configurado el servidor para copias de seguridad con redundancia geográfica, se puede crear un nuevo servidor a partir de la copia de seguridad de ese servidor existente. Este nuevo servidor puede crearse en cualquier región en la que Azure Database for MariaDB esté disponible.  

Para crear un servidor con una copia de seguridad con redundancia geográfica, use el comando `az mariadb server georestore` de la CLI de Azure.

> [!NOTE]
> Al crear por primera vez un servidor, puede que no esté disponible para la restauración geográfica inmediatamente. Los metadatos pueden tardar unas horas en rellenarse.
>

Para restaurar geográficamente el servidor, en el símbolo del sistema de la CLI de Azure, escriba el siguiente comando:

```azurecli-interactive
az mariadb server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen5_8
```

Este comando crea un nuevo servidor denominado *mydemoserver georestored* en la zona horaria del Este de EE. UU. que pertenecerá a *myresourcegroup*. Se trata de un servidor Gen 5 de uso general con ocho núcleos virtuales. El servidor se crea a partir de la copia de seguridad con redundancia geográfica de *mydemoserver*, que también está en el grupo de recursos *myresourcegroup*.

Si desea crear el nuevo servidor en otro grupo de recursos desde el servidor existente y, después, en el parámetro `--source-server` debería calificar el nombre del servidor como en el ejemplo siguiente:

```azurecli-interactive
az mariadb server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMariaDB/servers/mydemoserver" --location eastus --sku-name GP_Gen5_8

```

El comando `az mariadb server georestore` requiere los siguientes parámetros:

| Configuración | Valor sugerido | Descripción  |
| --- | --- | --- |
|resource-group| myresourcegroup | Nombre del grupo de recursos al que pertenece el nuevo servidor.|
|name | mydemoserver-georestored | Nombre del nuevo servidor. |
|source-server | mydemoserver | Nombre del servidor existente cuyas copias de seguridad con redundancia geográfica se usan. |
|ubicación | estado | Ubicación del nuevo servidor. |
|sku-name| GP_Gen5_8 | Este parámetro establece el plan de tarifa, la generación del proceso y el número de núcleos virtuales del nuevo servidor. GP_Gen5_8 se asigna a un servidor Gen 5 de uso general con ocho núcleos virtuales.|

Al crear un nuevo servidor mediante una restauración geográfica, hereda el mismo tamaño de almacenamiento y plan de tarifa que el servidor de origen. Estos valores no se pueden cambiar durante la creación. Después de crea el nuevo servidor, se puede escalar verticalmente su tamaño de almacenamiento.

Una vez finalizada la restauración, busque el servidor nuevo y compruebe que los datos se restauraron según lo previsto. El nuevo servidor tiene el mismo nombre de inicio de sesión y contraseña de administrador del servidor que el servidor existente tenía cuando se inició la restauración. La contraseña se puede cambiar en la página **Información general** del nuevo servidor.

El servidor creado durante una restauración no tiene los puntos de conexión de servicio de red virtual que existían en el servidor original. Estas reglas deben configurarse por separado para este nuevo servidor. Se restauran las reglas de firewall del servidor original.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [copias de seguridad](concepts-backup.md) del servicio.
- Más información sobre las [réplicas](concepts-read-replicas.md).
- Más información sobre las opciones de [continuidad del negocio](concepts-business-continuity.md).
