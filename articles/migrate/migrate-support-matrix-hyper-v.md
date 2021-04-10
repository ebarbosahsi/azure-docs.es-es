---
title: Compatibilidad con la evaluación de Hyper-V en Azure Migrate
description: Obtenga información sobre la compatibilidad con la evaluación de Hyper-V con Azure Migrate Discovery and assessment.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: 8531d0b2252e6ddff75509046b5a4576b99d339f
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104870796"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Matriz de compatibilidad para la evaluación de Hyper-V

En este artículo se resumen los requisitos previos y los requisitos de soporte técnico al detectar y evaluar servidores locales que se ejecutan en un entorno de Hyper-V para la migración a Azure con la herramienta [Azure Migrate: Discovery and assessment](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool). Si quiere migrar servidores que se ejecutan en Hyper-V a Azure, revise la [matriz de compatibilidad para la migración](migrate-support-matrix-hyper-v-migration.md).

Para configurar la detección y la evaluación de servidores que se ejecutan en Hyper-V, debe crear un proyecto y agregarle la herramienta Azure Migrate: Discovery and assessment. Después de agregarla, implemente el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta los servidores locales de forma continuada y envía los metadatos de configuración y rendimiento de los servidores a Azure. Una vez completada la detección, recopile los servidores detectados en grupos y ejecute una evaluación de un grupo.

## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación** | Descubra y evalúe hasta 35 000 servidores en un solo [proyecto](migrate-support-matrix.md#project).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Además de los servidores en Hyper-V, un proyecto puede incluir servidores de VMware y servidores físicos, hasta sus límites de evaluación.
**Detección** | El dispositivo de Azure Migrate puede detectar hasta 5000 servidores en ejecución en Hyper-V.<br/><br/> Un dispositivo puede conectarse a un total de 300 hosts de Hyper-V.
**Valoración** | Puede agregar hasta 35 000 servidores en un solo grupo.<br/><br/> Puede evaluar hasta 35 000 servidores en una única evaluación para un grupo.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.

## <a name="hyper-v-host-requirements"></a>Requisitos del host de Hyper-V:

| **Soporte técnico**                | **Detalles**
| :-------------------       | :------------------- |
| **Host de Hyper-V**       | El host de Hyper-V puede ser independiente o implementarse en un clúster.<br/><br/> Asimismo, el host de Hyper-V puede ejecutar Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2. También se admite la instalación de Server Core de estos sistemas operativos. <br/>No puede evaluar servidores que se encuentran en hosts de Hyper-V que ejecutan Windows Server 2012.
| **Permisos**           | Necesita permisos de administrador en el host de Hyper-V. <br/> Si no quiere asignar permisos de administrador, cree una cuenta de usuario local o de dominio y agregue la cuenta del usuario a estos grupos: Usuarios de administración remota, Administradores de Hyper-V y Usuarios del monitor de sistema. |
| **Comunicación remota de PowerShell**   | La [comunicación remota de PowerShell](/powershell/module/microsoft.powershell.core/enable-psremoting) debe estar habilitada en cada host de Hyper-V. |
| **Réplica de Hyper-V**       | Si usa la réplica de Hyper-V (o tiene varios servidores con los mismos identificadores) y detecta los servidores originales y los replicados mediante Azure Migrate, es posible que la evaluación generada por Azure Migrate no sea precisa. |

## <a name="server-requirements"></a>Requisitos del servidor

| **Soporte técnico**                  | **Detalles**
| :----------------------------- | :------------------- |
| **Sistema operativo** | Todos los sistemas operativos se pueden evaluar para la migración.  |
| **Servicio de integración**       | Los [servicio de integración de Hyper-V](/virtualization/hyper-v-on-windows/reference/integration-services) deben ejecutarse en los servidores que evalúe, con el fin de capturar la información del sistema operativo. |
| **Storage** | Disco local, DAS, JBOD, espacios de almacenamiento, CSV, SMB. Se admite el almacenamiento de host de Hyper-V en el que se almacena VHD/VHDX. <br/> Se admiten controladores virtuales IDE y SCSI.|

## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. Puede implementar el dispositivo mediante un disco duro virtual de Hyper-V comprimido que se descarga en el portal o mediante un [script de PowerShell](deploy-appliance-script.md).

- Obtenga información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---hyper-v) para Hyper-V.
- Obtenga información sobre las direcciones URL a las que el dispositivo necesita acceder en nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
- En Azure Government, debe implementar el dispositivo [mediante el script](deploy-appliance-script-government.md).

## <a name="port-access"></a>Acceso a puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/><br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```<br/><br/> Conexiones salientes en el puerto 443 (HTTPS) para enviar metadatos de detección y rendimiento a Azure Migrate.
**Host o clúster de Hyper-V** | Conexión entrante en el puerto de WinRM 5985 (HTTP) o 5986 (HTTPS) para extraer metadatos y datos de rendimiento de los servidores de Hyper-V mediante una sesión de Modelo de información común (CIM).

## <a name="agent-based-dependency-analysis-requirements"></a>Requisitos para el análisis de dependencia con agentes

El [análisis de dependencias](concepts-dependency-visualization.md) le ayuda a identificar las dependencias entre los servidores locales que quiere evaluar y migrar a Azure. En esta tabla se resumen los requisitos para configurar el análisis de dependencia con agentes. Actualmente, Hyper-V solo admite la visualización de dependencias basadas en agentes.

**Requisito** | **Detalles**
--- | --- 
**Antes de la implementación** | Debe tener un proyecto implementado al que se le debe haber agregado la herramienta Azure Migrate: Discovery and assessment.<br/><br/>  La visualización de dependencias se implementa después de configurar un dispositivo de Azure Migrate para detectar los servidores locales.<br/><br/> [Aprenda](create-manage-projects.md) a crear un proyecto por primera vez.<br/> [Aprenda](how-to-assess.md) a agregar la herramienta Azure Migrate: Discovery and assessment a un proyecto existente.<br/> Aprenda a configurar el dispositivo para la detección y evaluación de [servidores en Hyper-V](how-to-set-up-appliance-hyper-v.md).
**Azure Government** | La visualización de dependencias no está disponible en Azure Government.
**Log Analytics** | Azure Migrate utiliza la solución [Service Map](../azure-monitor/vm/service-map.md) de los [registros de Azure Monitor](../azure-monitor/logs/log-query-overview.md) para la visualización de dependencias.<br/><br/> Puede asociar un área de trabajo de Log Analytics nueva o existente con un proyecto. El área de trabajo de un proyecto no se puede modificar una vez que se ha agregado. <br/><br/> El área de trabajo debe estar en la misma suscripción que el proyecto.<br/><br/> El área de trabajo debe residir en las regiones Este de EE. UU., Sudeste Asiático u Oeste de Europa. Las áreas de trabajo de otras regiones no se pueden asociar a un proyecto.<br/><br/> El área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> En Log Analytics, el área de trabajo asociada con Azure Migrate se etiqueta con la clave del proyecto de migración y el nombre del proyecto.
**Agentes necesarios** | En cada servidor que quiera analizar, instale los siguientes agentes:<br/><br/> [Microsoft Monitoring agent (MMA)](../azure-monitor/agents/agent-windows.md).<br/> [Dependency Agent](../azure-monitor/agents/agents-overview.md#dependency-agent).<br/><br/> Si los servidores locales no están conectados a Internet, tiene que descargar e instalar la puerta de enlace de Log Analytics en ellas.<br/><br/> Conozca más información sobre la instalación de [Dependency Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) y [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Área de trabajo de Log Analytics** | El área de trabajo debe estar en la misma suscripción que el proyecto.<br/><br/> Azure Migrate admite áreas de trabajo que residen en las regiones Este de EE. UU., Sudeste Asiático y Oeste de Europa.<br/><br/>  El área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> El área de trabajo de un proyecto no se puede modificar una vez que se ha agregado.
**Costos** | La solución Service Map no genera ningún gasto durante los primeros 180 días (a partir del día que asocie el área de trabajo de Log Analytics con el proyecto).<br/><br/> Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.<br/><br/> Si se usa una solución que no sea Service Map en el área de trabajo de Log Analytics asociada generará los [gastos estándar](https://azure.microsoft.com/pricing/details/log-analytics/) de Log Analytics.<br/><br/> Cuando se elimina el proyecto, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, el uso de Service Map no será gratuito y cada nodo se facturará conforme al nivel de pago del área de trabajo de Log Analytics.<br/><br/>Si tiene proyectos creados antes de que Azure Migrate estuviera disponible con carácter general (el 28 de febrero de 2018), puede que haya incurrido en cargos de Service Map adicionales. Para garantizar el pago solo después de 180 días, es recomendable que cree un nuevo proyecto, ya que las áreas de trabajo existentes antes de la disponibilidad general siguen siendo facturables.
**Administración** | Al registrar agentes en el área de trabajo, use el identificador y la clave proporcionados por el proyecto.<br/><br/> Puede usar el área de trabajo de Log Analytics fuera de Azure Migrate.<br/><br/> Si elimina el proyecto asociado, el área de trabajo no se elimina automáticamente. [Elimínela manualmente](../azure-monitor/logs/manage-access.md).<br/><br/> No elimine el área de trabajo creada por Azure Migrate, a menos que elimine el proyecto. Si lo hace, la característica de visualización de dependencias no funcionará según lo previsto.
**Conectividad de Internet** | Si los servidores no están conectados a Internet, debe instalar la puerta de enlace de Log Analytics en ellos.
**Azure Government** | El análisis de dependencias basado en agente no se admite.

## <a name="next-steps"></a>Pasos siguientes

[Preparar la evaluación de servidores en ejecución en Hyper-V](./tutorial-discover-hyper-v.md)