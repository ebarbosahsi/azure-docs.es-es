---
title: Notas de la versión de la herramienta Instantánea coherente de aplicaciones de Azure para Azure NetApp Files | Microsoft Docs
description: Ofrece notas de la versión a la herramienta Instantánea de Azure Application Consistent que puede usar con Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: Phil-Jensen
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2021
ms.author: phjensen
ms.openlocfilehash: 01fb93dcd0a1d5c1db615c47d7811a0baa863c9b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105111397"
---
# <a name="release-notes-for-azure-application-consistent-snapshot-tool-preview"></a>Notas de la versión de Aplicación de Azure herramienta de instantáneas coherentes (versión preliminar)

En esta página se enumeran los cambios importantes realizados en AzAcSnap para proporcionar una nueva funcionalidad o resolver los defectos.

## <a name="march-2021"></a>Marzo de 2021

### <a name="azacsnap-v50-preview-build2021031830771"></a>Versión preliminar de AzAcSnap v 5.0 (compilación: 20210318.30771)

La versión preliminar de AzAcSnap v 5.0 (compilación: 20210318.30771) se ha publicado con las siguientes correcciones y mejoras:

- Se ha quitado la necesidad de agregar el usuario AZACSNAP al SAP HANA bases de los inquilinos, consulte la sección [Habilitar la comunicación con SAP Hana](azacsnap-installation.md#enable-communication-with-sap-hana).
- Corrección para permitir una [restauración](azacsnap-cmd-ref-restore.md) con volúmenes configurados con QoS manual.
- Se ha agregado un control mutex para limitar las conexiones SSH para Azure Large Instance.
- Corrija el instalador para controlar los nombres de ruta de acceso con espacios y otros problemas relacionados.
- Como preparación para la compatibilidad con otros servidores de bases de datos, cambió el parámetro opcional "--hanasid" a "--dbsid".

Descargue la [versión más reciente](https://aka.ms/azacsnapdownload) del instalador y revise [cómo empezar](azacsnap-get-started.md).

## <a name="next-steps"></a>Pasos siguientes

- [Primeros pasos con la herramienta Azure Application Consistent Snapshot](azacsnap-get-started.md)
