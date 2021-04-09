---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: d01cf90c42efbe611748027d0ea47ff8af3e5621
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "94553167"
---
1. Inicio de sesión en el dispositivo Data Box. Asegúrese de que está desbloqueado.

    ![Captura de pantalla que muestra el panel con el dispositivo mostrado como desbloqueado.](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Vaya a **Establecer interfaces de red**. Anote la dirección IP del dispositivo de la interfaz de red utilizada para conectar con el cliente.

    ![Captura de pantalla que muestra la configuración de red donde puede ver la dirección IP.](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Vaya a **Conectar y copiar** y haga clic en **REST**.

    ![Captura de pantalla que muestra el panel Conectar y copiar, donde puede seleccionar REST como opción de acceso.](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. En el cuadro de diálogo **Acceder a la cuenta de almacenamiento y cargar datos**, copie el valor de **Punto de conexión de Blob service**.

    ![Captura de pantalla que muestra el cuadro de diálogo Acceder a la cuenta de almacenamiento y cargar datos, en el que puede copiar el punto de conexión de Blob Service.](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Inicie el **Bloc de notas** como administrador y abra el archivo **hosts** que se encuentra en `C:\Windows\System32\Drivers\etc`.
6. Agregue la entrada siguiente a su archivo **hosts**: `<device IP address> <Blob service endpoint>`
7. Como referencia, utilice la siguiente imagen. Guarde el archivo **hosts**.

    ![Captura de pantalla que muestra un documento del Bloc de notas con la dirección IP y el punto de conexión de Blob Service agregados.](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
