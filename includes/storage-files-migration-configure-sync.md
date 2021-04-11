---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: 7aa3867fdc5de320c47a15737b655b8032f402a6
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075619"
---
Este paso une todos los recursos y carpetas que ha configurado en la instancia de Windows Server durante los pasos anteriores.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque el recurso del servicio de sincronización de almacenamiento.
1. Cree un nuevo *grupo de sincronización* en el recurso del servicio de sincronización de almacenamiento para cada recurso compartido de archivos de Azure. En la terminología de Azure File Sync, el recurso compartido de archivos de Azure se convertirá en *punto de conexión de la nube* en la topología de sincronización que describe con la creación de un grupo de sincronización. Al crear el grupo de sincronización, asígnele un nombre descriptivo, para que reconozca el conjunto de archivos que se sincronizan aquí. Asegúrese de hacer referencia al recurso compartido de archivos de Azure con un nombre coincidente.
1. Después de que se crea el grupo de sincronización, aparecerá una fila para él en la lista de grupos de sincronización. Seleccione el nombre (un vínculo) para mostrar el contenido del grupo de sincronización. Verá el recurso compartido de archivos de Azure en **Puntos de conexión en la nube**.
1. Busque el botón **+ Agregar punto de conexión de servidor**. La carpeta del servidor local que ha aprovisionado se convertirá en la ruta de acceso de este *punto de conexión del servidor*.
