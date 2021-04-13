---
title: 'Inicio rápido: Introducción a la creación de un área de trabajo de Synapse'
description: En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: a4fa902268d9a19cd0003a2fdaa4c5e58989a4ff
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2021
ms.locfileid: "106218947"
---
# <a name="creating-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

En este tutorial, aprenderá a crear un área de trabajo de Synapse, un grupo de SQL dedicado y un grupo de Apache Spark sin servidor. 

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este tutorial, es preciso tener acceso a un grupo de recursos para el que tenga asignado el rol de **propietario**. Cree el área de trabajo de Synapse en este grupo de recursos.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Creación de un área de trabajo de Synapse en Azure Portal

### <a name="start-the-process"></a>Inicio del proceso
1. Abra [Azure Portal](https://portal.azure.com) y escriba **Synapse** en la barra de búsqueda sin pulsar entrar.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics**.
1. Seleccione **Agregar** para crear un área de trabajo.

## <a name="basics-tab--project-details"></a>Pestaña Información básica > Detalles del proyecto
Rellene los campos siguientes:

1. **Suscripción**: seleccione cualquier suscripción.
1. **Grupo de recursos**: use cualquier grupo de recursos.
1. **Grupo de recursos administrados**: déjelo en blanco.

## <a name="basics-tab--workspace-details"></a>Pestaña Información básica > Detalles del área de trabajo
Rellene los campos siguientes:

1. **Nombre del área de trabajo**: seleccione cualquier nombre que sea globalmente único. Para este tutorial usaremos **myworkspace**.
1. **Región**: seleccione cualquier región.

En **Seleccionar Data Lake Storage Gen2**:

1. Junto a **Nombre de cuenta**, haga clic en **Crear nuevo** y asigne a la nueva cuenta de almacenamiento el nombre **contosolake** o uno similar, ya que este nombre debe ser único.
1. Junto a **Nombre del sistema de archivos**, haga clic en **Crear nuevo** y asigne el nombre **users**. Se creará un contenedor de almacenamiento llamado **users**. El área de trabajo usará esta cuenta de almacenamiento como cuenta de almacenamiento "principal" para las tablas de Spark y los registros de aplicaciones de Spark.
1. Active la casilla "Es necesario asignarme el rol Colaborador de datos de Storage Blob en la cuenta de Data Lake Storage Gen2". 

## <a name="completing-the-process"></a>Finalización del proceso
Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.

> [!NOTE]
> Para habilitar las características del área de trabajo de un grupo de SQL dedicado existente (anteriormente SQL DW), consulte [Habilitación de un área de trabajo para el grupo de SQL dedicado (anteriormente SQL DW)](./sql-data-warehouse/workspace-connected-create.md).


## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abra el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com) y, en la sección **Información general** del área de trabajo de Synapse, seleccione **Abrir** en el cuadro Abrir Synapse Studio.
* Vaya a `https://web.azuresynapse.net` e inicie sesión en el área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos con un grupo de SQL sin servidor](get-started-analyze-sql-on-demand.md)
