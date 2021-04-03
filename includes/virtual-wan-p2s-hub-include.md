---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 134f1dc7cb6e53c181b2f518055e5cb758fccf31
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91812712"
---
1. En la red WAN virtual, seleccione Centros de conectividad y seleccione **+Nuevo centro de conectividad**.

   :::image type="content" source="media/virtual-wan-p2s-hub/new-hub.jpg" alt-text="Nuevo centro de conectividad":::

1. En la página Crear centro de conectividad virtual, complete los siguientes campos.

   * **Región**: seleccione la región en la que quiere implementar el centro de conectividad virtual.
   * **Nombre**: escriba el nombre que quiere asignar al centro de conectividad virtual.
   * **Espacio de direcciones privadas del centro de conectividad**: intervalo de direcciones del centro de conectividad en la notación CIDR.

   :::image type="content" source="media/virtual-wan-p2s-hub/create-hub.jpg" alt-text="Crear centro de conectividad virtual":::

1. En la pestaña "De punto a sitio", rellene los siguientes campos:

   * **Unidades de escalado de puerta de enlace**: que representa la capacidad agregada de la puerta de enlace de VPN del usuario.
   * **Configuración de punto a sitio**: la que creó en el paso anterior.
   * **Grupo de direcciones de clientes**: para los usuarios remotos.
   * **Dirección IP de servidor DNS personalizado**.

   :::image type="content" source="media/virtual-wan-p2s-hub/hub-with-p2s.png" alt-text="centro de conectividad con configuración de punto a sitio":::

1. Seleccione **Revisar + crear**.
1. En la página **Validación superada**, seleccione **Crear**.