---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: kraigb
ms.service: app-service
ms.topic: include
ms.date: 09/24/2020
ms.author: kraigb
ms.custom: include file
ms.openlocfilehash: 11b28866a67f45d8d792e56d09b56c658dd1641f
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2021
ms.locfileid: "106382678"
---
# <a name="bash"></a>[Bash](#tab/bash)

```bash
# Linux systems only
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Git Bash on Windows only
py -3 -m venv .venv
source .venv\\scripts\\activate
pip install -r requirements.txt
```

Si se encuentra en un sistema Windows y ve un error que dice que el origen no se reconoce como un comando interno o externo, asegúrese de que trabaja en el shell de Bash de Git o use los comandos mostrados en la pestaña **Cmd** anterior.


# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
py -3 -m venv .venv
.venv\scripts\activate
pip install -r requirements.txt
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 -m venv .venv
.venv\scripts\activate
pip install -r requirements.txt
```

---   
