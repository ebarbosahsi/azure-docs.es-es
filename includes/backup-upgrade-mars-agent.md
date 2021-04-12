---
title: Actualización del agente de Azure Backup
description: Aquí se explica por qué se debe actualizar el agente de Azure Backup y dónde se descarga la actualización.
services: backup
cloud: ''
suite: ''
author: v-amallick
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 03/03/2020
ms.author: v-amallick
ms.openlocfilehash: bf77103db93652e1df837f6b1032b5e53bd41e1f
ms.sourcegitcommit: af6eba1485e6fd99eed39e507896472fa930df4d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2021
ms.locfileid: "106294161"
---
## <a name="upgrade-the-mars-agent"></a>Actualización del agente de MARS

Las versiones del agente de Microsoft Azure Recovery Services (MARS) inferiores a la 2.0.9083.0 tienen una dependencia en Azure Access Control Service. El agente de MARS también se conoce como agente de Azure Backup.

En 2018, Azure [retiró Azure Access Control Service](../articles/active-directory/azuread-dev/active-directory-acs-migration.md). A partir del 19 de marzo de 2018, todas las versiones del agente MARS inferiores a la 2.0.9083.0 sufrirán errores de copia de seguridad. Para evitar o resolver dichos errores, [actualice el agente de MARS a la versión más reciente](https://support.microsoft.com/help/4538314/update-for-azure-backup-for-microsoft-azure-recovery-services-agent). Para identificar los servidores que necesitan una actualización del agente de MARS, siga los pasos de [Actualización del agente de Microsoft Azure Recovery Services (MARS)](../articles/backup/upgrade-mars-agent.md).

El agente de MARS se usa para hacer copias de seguridad de archivos, carpetas y datos de estado del sistema en Azure. System Center DPM y Azure Backup Server usan al agente de MARS para hacer una copia de seguridad de los datos en Azure.
