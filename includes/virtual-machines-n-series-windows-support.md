---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 5413a6acafa0ea54f98383fc8140a34aff0cf840
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98859817"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores NVIDIA Tesla (CUDA)

Los controladores NVIDIA Tesla (CUDA) para máquinas virtuales de las series NC, NCv2, NCv3, NCasT4_v3, ND y NDv2 (opcionales para la serie NV) solo se admiten en las distribuciones de sistemas operativos enumeradas en la tabla siguiente. Los vínculos de descarga de controladores están actualizados en el momento de la publicación. Para ver los controladores más recientes, visite el sitio web de [NVIDIA](https://www.nvidia.com/).

> [!TIP]
> Como alternativa a la instalación manual de controladores de CUDA en una máquina virtual de Windows Server, puede implementar una imagen de [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) de Azure. Las ediciones de DSVM para Windows Server 2016 preinstalan los controladores NVIDIA CUDA, la biblioteca CUDA Deep Neural Network Library y otras herramientas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2019 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (.exe) |
| Windows Server 2016 | [451.82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Controladores de NVIDIA GRID

Microsoft redistribuye los instaladores del controlador NVIDIA GRID para VM de las series NV y NVv3 que se emplean como estaciones de trabajo virtuales o para aplicaciones virtuales. Instale estos controladores GRID en VM de la serie NV de Azure y solo en los sistemas operativos enumerados en la tabla siguiente. Estos controladores incluyen licencias del software GRID Virtual GPU en Azure. No es necesario configurar un servidor de licencias de software vGPU NVIDIA.

Los controladores de GRID redistribuidos por Azure no funcionan en máquinas virtuales que no son de la serie NV, como las máquinas virtuales de las series NCv2, NCv3, ND y NDv2. La única excepción es la serie de VM NCas_T4_V3, donde los controladores de GRID habilitarán las funcionalidades de gráficos similares a las de la serie NV.

La serie NC con las GPU de NVIDIA K80 no admite aplicaciones de gráficos ni cuadrículas.  

Tenga en cuenta que la extensión de NVIDIA siempre instalará el controlador más reciente. Aquí se proporcionan vínculos a la versión anterior para los clientes que tienen dependencias de una versión anterior.

En Windows Server 2019, Windows Server 2016 1607, 1709 y Windows 10 (hasta la compilación 20H2):
- [GRID 12.0 (461.09)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe)
- [GRID 11.3 (452.77)](https://download.microsoft.com/download/f/d/5/fd5ad39b-89cb-4990-ae85-a6fd30475584/452.77_grid_win10_server2016_server2019_64bit_azure_swl.exe) (.exe) 

En Windows Server 2012 R2: 
- [GRID 12.0 (461.09)](https://download.microsoft.com/download/c/5/e/c5e7df99-364d-45f5-bff7-c253d59121f1/461.09_grid_server2012R2_64bit_azure_swl.exe) (.exe)
- [GRID 11.3 (452.77)](https://download.microsoft.com/download/5/4/3/54323644-3c84-4aa1-97ec-35491f94c866/452.77_grid_server2012R2_64bit_azure_swl.exe) (.exe) 


Para obtener la lista completa de los vínculos de todos los controladores de Nvidia GRID anteriores, visite [GitHub](https://github.com/Azure/azhpc-extensions/blob/master/NvidiaGPU/resources.json)
