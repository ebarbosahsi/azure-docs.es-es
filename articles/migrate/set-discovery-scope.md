---
title: Establezca el ámbito de detección de servidores en VMware con Azure Migrate
description: Describe cómo establecer el ámbito de detección para los servidores hospedados en la evaluación y migración de VMware con Azure Migrate.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: how-to
ms.date: 03/13/2021
ms.openlocfilehash: 29ac42da6560a717f12cd256fdd71282e0bd313f
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104775364"
---
# <a name="set-discovery-scope-for-servers-in-vmware-environment"></a>Establecer el ámbito de detección para servidores en el entorno de VMware

En este artículo se describe cómo limitar el ámbito de detección de servidores en el entorno de VMware cuando:

- La detección de servidores con el [dispositivo Azure Migrate](migrate-appliance-architecture.md) cuando usa Azure Migrate: herramienta de detección y evaluación.
- La detección de servidores con el [dispositivo Azure Migrate](migrate-appliance-architecture.md) cuando usa la herramienta Azure Migrate: Server Migration, para la migración sin agentes de servidores del entorno VMware a Azure.

Al configurar el dispositivo, se conecta a vCenter Server e inicia la detección. Antes de conectar el dispositivo a vCenter Server, puede limitar la detección a centros de datos, clústeres, una carpeta de clústeres, hosts, una carpeta de hosts o servidores individuales. Para establecer el ámbito, asigne permisos en la cuenta que usa el dispositivo para tener acceso a vCenter Server.

## <a name="before-you-start"></a>Antes de comenzar

Si no ha configurado una cuenta de usuario de vCenter que Azure Migrate usa para la detección, hágalo ahora para la [evaluación](./tutorial-discover-vmware.md#prepare-vmware) o la [migración sin agente](./migrate-support-matrix-vmware-migration.md#agentless-migration).


## <a name="assign-permissions-and-roles"></a>Asignación de permisos y roles

Puede asignar permisos en objetos de inventario VMware mediante uno de estos dos métodos:

- En la cuenta usada por el dispositivo, asigne un rol con los permisos necesarios en los objetos que desee incluir en el ámbito.
- Como alternativa, puede asignar un rol a la cuenta en el nivel de centro de trabajo y propagarlo a los objetos secundarios. A continuación, conceda a la cuenta el rol **Sin acceso** para cada objeto que no quiera incluir en el ámbito. No se recomienda este enfoque porque resulta complicado y podría exponer los controles de acceso, ya que a cada objeto secundario nuevo se le concede automáticamente acceso heredado del objeto primario.

No se puede limitar el ámbito de la detección de inventario en el nivel de carpeta de servidor de vCenter. Si necesita limitar el ámbito de la detección a los servidores en una carpeta, cree un usuario y conceda acceso de forma individual a cada servidor requerido. Se admiten las carpetas de clúster y de host.


### <a name="assign-a-role-for-assessment"></a>Asignación de un rol para su evaluación

1. En la cuenta de la aplicación de vCenter que usa para la detección, aplique el rol de **Solo lectura** para todos los objetos primarios que hospedan servidores que quiere detectar y evaluar (host, clúster, carpeta de hosts, carpeta de clústeres, hasta centro de trabajo).
2. Propague estos permisos a los objetos secundarios de la jerarquía.

    ![Asignación de permisos](./media/tutorial-assess-vmware/assign-perms.png)

### <a name="assign-a-role-for-agentless-migration"></a>Asignación de un rol para la migración sin agente

1. En la cuenta de la aplicación de vCenter que usa para la migración, aplique un rol definido por el usuario que tenga los [permisos necesarios](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) para todos los objetos primarios que hospedan los servidores que quiere detectar y migrar.
2. Puede asignar un nombre al rol con algo que sea más fácil de identificar. Por ejemplo, <em>Azure_Migrate</em>.

## <a name="work-around-for-server-folder-restriction"></a>Solución alternativa para la restricción de carpeta de servidor

Actualmente, la herramienta de Azure Migrate: Detección y Evaluación no puede detectar servidores si se concede el acceso en el nivel de carpeta de servidor vCenter. Si quiere limitar el ámbito de la detección y la evaluación por carpetas de servidor, use esta solución alternativa.

1. Asigne permisos de solo lectura en todas los servidores situados en las carpetas para las que quiere limitar el ámbito de la detección y evaluación.
2. Conceda acceso de solo lectura a todos los objetos primarios que hospedan el host de servidores, el clúster, la carpeta de hosts, la carpeta de clústeres, hasta el centro de datos. No es necesario propagar los permisos a todos los objetos secundarios.
3. Para usar las credenciales para la detección, seleccione el centro de datos como **Ámbito del grupo**.


El control de acceso basado en rol configurado garantiza que la cuenta de usuario correspondiente de vCenter solo tenga acceso a los servidores específicos del inquilino.


## <a name="next-steps"></a>Pasos siguientes

[Configuración del dispositivo](how-to-set-up-appliance-vmware.md)