---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 089ad704a466590a9649107fef245a88d6a4d6e3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244944"
---
La siguiente configuración se usó para los pasos siguientes:

- Equipo: Ubuntu Server 18.04
- Dependencias: strongSwan


Ejecute los comandos siguientes para instalar la configuración necesaria de strongSwan:

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

Use el siguiente comando para instalar la interfaz de la línea de comandos de Azure:

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[Instrucciones adicionales sobre cómo instalar la CLI de Azure](/cli/azure/install-azure-cli-apt)