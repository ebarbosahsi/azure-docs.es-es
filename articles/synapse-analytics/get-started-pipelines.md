---
title: 'Tutorial: Introducción a la integración con las canalizaciones'
description: En este tutorial, aprenderá a integrar las canalizaciones y las actividades mediante Synapse Studio.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 2ea7c3c440fcf95e4512464333efe8461788bceb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98219409"
---
# <a name="integrate-with-pipelines"></a>Integración con las canalizaciones

En este tutorial, aprenderá a integrar las canalizaciones y las actividades mediante Synapse Studio. 

## <a name="overview"></a>Información general

En Azure Synapse, puede organizar una gran variedad de tareas.

1. En Synapse Studio, vaya al centro **Integrate** (Integración).
1. Seleccione **+**  > **Canalización** para crear una canalización. Haga clic en el nuevo objeto de canalización para abrir el diseñador de canalizaciones.
1. En **Actividades**, expanda la carpeta **Synapse** y arrastre un objeto **Cuaderno** al diseñador.
1. Seleccione la pestaña **Configuración** de las propiedades de la actividad del cuaderno. Use la lista desplegable para seleccionar un cuaderno del área de trabajo actual de Synapse. 
1. En la canalización, seleccione **Agregar desencadenador** > **Nuevo/editar**.
1. En **Choose trigger** (Elegir desencadenador), seleccione **New** (Nuevo) y establezca el valor de **Recurrence** (Periodicidad) en "cada 1 hora".
1. Seleccione **Aceptar**. 
1. Seleccione **Publish All** (Publicar todo). 


## <a name="monitor-pipeline"></a>Supervisión de la canalización

1. Una vez que se publica la canalización, para que se ejecute inmediatamente sin esperar a la próxima hora, seleccione **Agregar desencadenador** > **Trigger now** (Desencadenar ahora).
1. En Synapse Studio, vaya al centro **Supervisión** y seleccione **Pipeline runs** (Ejecuciones de canalización) para supervisar el progreso de la ejecución de la canalización.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Visualización de datos con Power BI](get-started-visualize-power-bi.md)
