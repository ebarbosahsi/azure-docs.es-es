---
title: Descarga del SDK de seguimiento de personas de Azure Kinect
description: Aprenda a descargar cada versión del SDK del Sensor de Azure Kinect en Windows o Linux.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 03/18/2021
ms.topic: conceptual
keywords: azure, kinect, sdk, actualización de descarga, más reciente, disponible, instalar, instalación, cuerpo, corporal, personas, seguimiento
ms.openlocfilehash: 60018df56433785f3cb723dd54cc96a8e5289b67
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654499"
---
# <a name="download-azure-kinect-body-tracking-sdk"></a>Descarga del SDK de seguimiento de personas de Azure Kinect

En este documento se proporcionan vínculos para instalar cada versión del SDK de seguimiento de personas de Azure Kinect.

## <a name="azure-kinect-body-tracking-sdk-contents"></a>Contenido del SDK de seguimiento de personas de Azure Kinect

- Encabezados y bibliotecas para compilar una aplicación de seguimiento de personas mediante Azure Kinect DK.
- Archivos DLL redistribuibles necesarios para las aplicaciones de seguimiento de personas que usan Azure Kinect DK.
- Aplicaciones de seguimiento de cuerpos de muestra.

## <a name="windows-download-links"></a>Vínculo de descarga para Windows

Versión       | Descargar
--------------|----------
1.1.0 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=102901)
1.0.1 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100942) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.1)
1.0.0 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100848) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.0)

## <a name="linux-installation-instructions"></a>Instrucciones de instalación para Linux

Actualmente, la única distribución admitida es Ubuntu 18.04 y 20.04. Para solicitar soporte técnico para otras distribuciones, consulte [esta página](https://aka.ms/azurekinectfeedback).

En primer lugar, deberá configurar el [repositorio de paquetes de Microsoft](https://packages.microsoft.com/), siguiendo las instrucciones de [este artículo](/windows-server/administration/linux-package-repository-for-microsoft-software).

El paquete `libk4abt<major>.<minor>-dev` contiene los encabezados y los archivos CMake que se van a compilar en `libk4abt`.
El paquete `libk4abt<major>.<minor>` contiene los objetos compartidos necesarios para ejecutar archivos ejecutables que dependen de `libk4abt`, así como el visor de ejemplos.

Los tutoriales básicos requieren el paquete `libk4abt<major>.<minor>-dev`. Para instalarlo, ejecute

`sudo apt install libk4abt<major>.<minor>-dev`

Si el comando se ejecuta correctamente, el SDK está listo para su uso.

> [!NOTE]
> Cuando instale el SDK, recuerde la ruta de acceso de instalación. Por ejemplo, "C:\Archivos de programa\Azure Kinect Body Tracking SDK 1.0.0". Encontrará las muestras a las que se hace referencia en los artículos en esta ruta de acceso.
> Los ejemplos de seguimiento de personas se encuentran en la carpeta [body-tracking-samples](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples) del repositorio Azure-Kinect-Samples. Encontrará las muestras a las que se hace referencia aquí.

## <a name="change-log"></a>Registro de cambios

### <a name="v110"></a>v1.1.0
* [Característica] Se agregó compatibilidad para la ejecución de DirectML (solo Windows) y TensorRT del modelo de estimación de supuestos. Consulte las preguntas más frecuentes sobre los nuevos entornos de ejecución.
* [Característica] Se agregó `model_path` a la estructura `k4abt_tracker_configuration_t`. Permite a los usuarios especificar el nombre de ruta del modelo de estimación de supuestos. El valor predeterminado es el modelo de estimación de supuestos estándar `dnn_model_2_0_op11.onnx` ubicado en el directorio actual.
* [Característica] Se incluyó el modelo de estimación de supuestos básico `dnn_model_2_0_lite_op11.onnx`. Este modelo negocia el aumento de rendimiento de aproximadamente el doble para una reducción de la precisión de aproximadamente el 5 %.
* [Característica] Se verificó la compilación de ejemplos con Visual Studio 2019 y se actualizaron los ejemplos para usar esta versión.
* [Cambio importante] Se actualizó a ONNX Runtime 1.6 con compatibilidad para los entornos de ejecución de CPU, CUDA 11.1, DirectML (solo Windows) y TensorRT 7.2.1. Requiere la actualización del controlador NVIDIA a R455 o superior.
* [Cambio importante] No hay ninguna instalación de NuGet.
* [Corrección de errores] Se agregó compatibilidad para las GPU de la serie 30xx de NVIDIA RTX [vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1481)
* [Corrección de errores] Se agregó compatibilidad para las GPU integradas de AMD e Intel (solo Windows) [vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1481)
* [Corrección de errores] Se actualizó a CUDA 11.1 [vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1125)
* [Corrección de errores] Se actualizó al SDK del sensor 1.4.1 [vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1248)

### <a name="v101"></a>v1.0.1
* [Corrección de errores] Se corrigió el problema por el que el SDK se bloquea si se carga onnxruntime.dll desde la ruta de acceso en Windows compilación 19025 o posterior: [Vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/932)

### <a name="v100"></a>v1.0.0
* [Característica] Se agrega el contenedor de C# el instalador de msi.
* [Corrección de errores] Se corrigió el problema por el que la rotación de encabezado no se puede detectar correctamente: [Vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/997)
* [Corrección de errores] Se corrigió el problema por el que el uso de CPU alcanza el 100 % en una máquina Linux: [Vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1007)
* [Muestras] Se agregaron dos muestras al repositorio de muestras. En la muestra 1 se demuestra cómo transformar los resultados del seguimiento de personas del espacio de profundidad al espacio de colores: [vínculo](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/camera_space_transform_sample); en la muestra 2 se demuestra cómo detectar el plano de planta: [vínculo](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/floor_detector_sample)

## <a name="next-steps"></a>Pasos siguientes

- [Información general de Azure Kinect DK](about-azure-kinect-dk.md)

- [Configuración de Azure Kinect DK](set-up-azure-kinect-dk.md)

- [Configuración del seguimiento corporal de Azure Kinect](body-sdk-setup.md)