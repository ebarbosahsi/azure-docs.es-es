---
title: 'Cifrado doble de infraestructura: Azure Portal: Azure Database for PostgreSQL'
description: Obtenga información sobre cómo configurar y administrar el cifrado doble de infraestructura para Azure Database for PostgreSQL.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 03/14/2021
ms.openlocfilehash: 0b1079a084072556ae79a18a12f6b35f74c92775
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104771658"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-postgresql"></a>Cifrado doble de infraestructura para Azure Database for PostgreSQL

Obtenga información sobre cómo configurar y administrar el cifrado doble de infraestructura para Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Requisitos previos

* Debe tener una suscripción de Azure y ser un administrador en esa suscripción.

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---portal"></a>Creación de un servidor de Azure Database for PostgreSQL con el cifrado doble de infraestructura: portal

Siga estos pasos para crear un servidor de Azure Database for PostgreSQL con el cifrado doble de infraestructura desde Azure Portal:

1. En la esquina superior izquierda del portal, seleccione **Crear un recurso** (+).

2. Seleccione **Bases de datos** > **Azure Database for PostgreSQL**. También puede escribir PostgreSQL en el cuadro de búsqueda para encontrar el servicio. Se habilitó la opción de implementación de **servidor único**.

   :::image type="content" source="./media/quickstart-create-database-portal/1-create-database.png" alt-text="Azure Database for PostgreSQL en el menú":::

3. Proporcione la información básica del servidor. Seleccione **Configuración adicional** y marque la casilla **Cifrado doble de infraestructura** para establecer el parámetro.

    :::image type="content" source="./media/howto-infrastructure-double-encryption/infrastructure-encryption-selected.png" alt-text="Selecciones de Azure Database for PostgreSQL":::

4. Seleccione **Revisar y crear** para realizar el aprovisionamiento del servidor.

    :::image type="content" source="./media/howto-infrastructure-double-encryption/infrastructure-encryption-summary.png" alt-text="Resumen de Azure Database for PostgreSQL":::

5. Una vez creado el servidor, puede validar el cifrado doble de infraestructura comprobando el estado en la hoja del servidor **Cifrado de datos**.

    :::image type="content" source="./media/howto-infrastructure-double-encryption/infrastructure-encryption-validation.png" alt-text="Validación de Azure Database for MySQL":::

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---cli"></a>Creación de un servidor de Azure Database for PostgreSQL con el cifrado doble de infraestructura: CLI

Siga estos pasos para crear un servidor de Azure Database for PostgreSQL con el cifrado doble de infraestructura desde la CLI:

En este ejemplo se crea un grupo de recursos denominado `myresourcegroup` en la ubicación `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
En el ejemplo siguiente se crea un servidor PostgreSQL 11 en la región Oeste de EE. UU. llamado `mydemoserver` en el grupo de recursos `myresourcegroup` con el inicio de sesión de administrador de servidor `myadmin`. Se trata de un servidor **Gen 4** de **uso general** con **dos núcleos virtuales**. Asimismo, habilitará el cifrado doble de infraestructura para el servidor creado. Sustituya `<server_admin_password>` por su propio valor.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 11 --infrastructure-encryption >Enabled/Disabled>
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el cifrado de datos, consulte [Cifrado doble de infraestructura de datos de Azure Database for PostgreSQL](concepts-Infrastructure-double-encryption.md).

