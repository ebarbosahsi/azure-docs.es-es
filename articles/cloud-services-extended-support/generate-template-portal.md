---
title: Habilitación de plantilla de ARM para Cloud Services (soporte extendido) mediante Azure Portal
description: Genere y descargue la plantilla de ARM y el archivo de parámetros para Cloud Services (soporte extendido) mediante Azure Portal
ms.topic: how-to
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachandw
ms.date: 03/07/2021
ms.custom: ''
ms.openlocfilehash: 9d40bbd7e08d8d3869166827a22f3f08536532bb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104590711"
---
# <a name="generate-arm-template-for-cloud-services-extended-support-using-the-azure-portal"></a>Habilitación de plantilla de ARM para Cloud Services (soporte extendido) mediante Azure Portal

En este artículo se explica cómo descargar la plantilla de ARM y el archivo de parámetros desde [Azure Portal](https://portal.azure.com) una vez implementado el servicio en la nube (soporte extendido). La plantilla de ARM y el archivo de parámetros se pueden usar en implementaciones futuras para actualizar un servicio en la nube (soporte extendido).

## <a name="get-arm-template-via-portal"></a>Obtención de una plantilla de ARM mediante el portal

  1. Vaya al grupo de recursos y seleccione las implementaciones.
  :::image type="content" source="media/generate-template-portal-1.png" alt-text="Imagen que muestra la selección de implementaciones, en el grupo de recursos, en Azure Portal.":::
  
  2. Seleccione el servicio en la nube (soporte extendido) y haga clic en la plantilla.
  :::image type="content" source="media/generate-template-portal-2.png" alt-text="Imagen que muestra la selección de la plantilla en el servicio en la nube (soporte extendido) en Azure Portal.":::
  
  3. Descargue la plantilla y los archivos de parámetros. Se pueden usar para futuras implementaciones a través de PowerShell.
  :::image type="content" source="media/generate-template-portal-3.png" alt-text="Imagen que muestra la descarga del archivo de plantilla en Azure Portal.":::
  
## <a name="next-steps"></a>Pasos siguientes 
- Vea las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Implementación de un servicio en la nube (soporte extendido) mediante [Azure Portal](deploy-portal.md)
  
