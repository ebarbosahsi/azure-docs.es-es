---
title: Implementar el servicio Administrador de dispositivos de StorSimple en Azure | Microsoft Docs
description: Conozca los pasos necesarios para la creación, la eliminación, la migración del servicio y la administración de la clave de registro del servicio.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: 85d66b5e4f7e95d114cbf0b4c681d73eebb93a67
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106076571"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>Implementar el servicio Administrador de dispositivos de StorSimple para dispositivos de la serie StorSimple 8000

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Información general

El servicio StorSimple Device Manager se ejecuta en Microsoft Azure y se conecta a varios dispositivos de StorSimple. Después de crear el servicio, lo puede usar para administrar todos los dispositivos que están conectados al servicio Administrador de dispositivos de StorSimple desde una única ubicación central, con lo que se reduce la carga administrativa.

En este tutorial se describen los pasos necesarios para la creación, la eliminación, la migración del servicio y la administración de la clave de registro del servicio. La información contenida en este artículo se aplica solo a los dispositivos de la serie StorSimple 8000. Para más información sobre StorSimple Virtual Array, vaya a [Implementación del servicio Administrador de dispositivos de StorSimple en una instancia de StorSimple Virtual Array](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> -  Azure Portal admite dispositivos que ejecutan la versión Update 5.0 o posterior. Si el dispositivo no está actualizado, instale Update 5 inmediatamente. Para más información, vaya a [Instalación de Update 5](storsimple-8000-install-update-5.md). 
> - Si está usando StorSimple Cloud Appliance (8010/8020), no puede actualizar una aplicación en la nube. Utilice la versión más reciente del software para crear una nueva aplicación en la nube con la versión Update 5.0 y después realizar una conmutación por error en la nueva aplicación en la nube creada. 
> - Todos los dispositivos que ejecutan Update 4.0 o una versión anterior experimentarán una disminución en la funcionalidad de administración. 

## <a name="create-a-service"></a>Crear un servicio
Para crear un servicio Administrador de dispositivos de StorSimple, debe tener:

* Una suscripción con un contrato Enterprise
* Una cuenta de Almacenamiento de Microsoft Azure activa.
* La información de facturación que se usa para la administración de acceso

Solo se permiten las suscripciones con un contrato Enterprise. También puede generar una cuenta de almacenamiento predeterminada cuando se crea el servicio.

Un único servicio puede administrar varios dispositivos. Sin embargo, un dispositivo no puede abarcar varios servicios. Una gran empresa puede tener varias instancias de servicio para trabajar con distintas suscripciones, organizaciones o incluso las ubicaciones de implementación. 

> [!NOTE]
> Necesita instancias separadas del servicio StorSimple Device Manager para administrar instancias de StorSimple Virtual Array y dispositivos de StorSimple de la serie 8000.

Realice los siguientes pasos para crear un servicio.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Para cada servicio Administrador de dispositivos de StorSimple, existen los siguientes atributos:

* **Nombre**: El nombre asignado al servicio Administrador de dispositivos de StorSimple cuando se creó. **El nombre del servicio no se puede cambiar una vez creado el servicio. Esto también es cierto para otras entidades como dispositivos, volúmenes, contenedores de volúmenes y directivas de copia de seguridad que no se pueden cambiar en Azure Portal.**
* **Estado**: estado del servicio, que puede ser **Activo**, **Creando** o **En línea**.
* **Ubicación** : ubicación geográfica en la que se implementará el dispositivo StorSimple.
* **Suscripción** : suscripción de facturación asociada a su servicio.

## <a name="delete-a-service"></a>Eliminar un servicio

Antes de eliminar un servicio, asegúrese de que no lo esté utilizando ningún dispositivo conectado. Si se está utilizando el servicio, desactive los dispositivos conectados. La operación de desactivación eliminará la conexión entre el dispositivo y el servicio, pero conservará los datos del dispositivo en la nube.

> [!IMPORTANT]
> Después de eliminar un servicio, no se puede revertir la operación. Cualquier dispositivo que usara el servicio debe restablecerse a los valores de fábrica antes de que pueda ser usado con otro servicio. En este escenario, se pierden los datos locales del dispositivo, así como la configuración.

Realice los siguientes pasos para eliminar un servicio.

### <a name="to-delete-a-service"></a>Para eliminar un servicio

1. Busque el servicio que quiere eliminar. Haga clic en el icono **Recursos** y, después, especifique los términos adecuados para buscar. En los resultados de la búsqueda, haga clic en el servicio que quiere eliminar.

    ![Buscar servicio para eliminar](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Esto le llevará a la hoja del servicio Administrador de dispositivos de StorSimple. Haga clic en **Eliminar**.

    ![Eliminar servicio](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Haga clic en **Sí** en la notificación de confirmación. Es posible que el servicio tarde unos minutos en eliminarse.

    ![Confirmar eliminación](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Obtener la clave de registro del servicio

Después de haber creado correctamente un servicio, deberá registrar el dispositivo StorSimple con el servicio. Para registrar el primer dispositivo StorSimple, necesitará la clave de registro del servicio. Para registrar dispositivos adicionales con un servicio existente de StorSimple, necesita la clave de registro y la clave de cifrado de datos del servicio (que se genera en el primer dispositivo durante el registro). Para obtener más información acerca de la clave de cifrado de datos de servicio, consulte [Seguridad de StorSimple](storsimple-8000-security.md). Puede obtener la clave de registro accediendo a **Claves** en la hoja Administrador de dispositivos de StorSimple.

Realice los pasos siguientes para obtener la clave de registro del servicio.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Mantenga la clave de registro del servicio en una ubicación segura. Necesitará esta clave, así como la clave de cifrado de datos de servicio para registrar dispositivos adicionales con este servicio. Después de obtener la clave de registro del servicio, tiene que configurar el dispositivo a través de la interfaz de Windows PowerShell para StorSimple.

Para más información sobre cómo usar esta clave del registro, consulte [Paso 3: Configurar y registrar el dispositivo mediante Windows PowerShell para StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Volver a generar la clave de registro de servicio
Tiene que volver a generar una clave de registro del servicio si tiene que realizar la rotación de claves o si la lista de administradores de servicios ha cambiado. Cuando se regenera la clave, la nueva clave se utiliza solo para registrar dispositivos posteriores. Los dispositivos que ya se han registrado no se ven afectados por este proceso.

Realice los pasos siguientes para volver a generar una clave de registro de servicio.

### <a name="to-regenerate-the-service-registration-key"></a>Para volver a generar la clave de registro de servicio
1. En la hoja **Administrador de dispositivos de StorSimple**, vaya a **Administración &gt;** **Claves**.
    
    ![Ir a la hoja Claves](./media/storsimple-8000-manage-service/regenregkey2.png)

2. En la hoja **Claves**, haga clic en **Regenerar**.

    ![Haga clic en Regenerar](./media/storsimple-8000-manage-service/regenregkey3.png)
3. En la hoja **Regenerar clave de registro del servicio**, revise la acción necesaria cuando se regeneren las claves. Todos los dispositivos posteriores que están registrados en este servicio usan la nueva clave del registro. Haga clic en **Regenerar** para confirmarlo. Se le notifica una vez completada la regeneración.

    ![Confirmar regeneración](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Aparecerá una nueva clave de registro de servicio.

5. Copie esta clave y guárdela para registrar los dispositivos nuevos con este servicio.



## <a name="change-the-service-data-encryption-key"></a>Cambiar la clave de cifrado de datos de servicio
[!INCLUDE [storage-files-migration-generate-key](../../includes/storage-files-migration-generate-key.md)]

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Operaciones admitidas en dispositivos que ejecutan versiones anteriores a Update 5.0
En Azure Portal, solo se admiten los dispositivos de StorSimple que ejecuten Update 5.0 y versiones posteriores. Los dispositivos que ejecutan versiones anteriores tienen compatibilidad limitada. Después de haber migrado a Azure Portal, utilice la siguiente tabla para comprender las operaciones que se admiten en los dispositivos que ejecutan versiones anteriores a Update 5.0.

| Operación                                                                                                                       | Compatible      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Registrar un dispositivo                                                                                                               | Sí            |
| Configurar opciones de dispositivo como generales, de red y de seguridad                                                                | Sí            |
| Buscar, descargar e instalar actualizaciones                                                                                             | Sí            |
| Desactivar un dispositivo                                                                                                               | Sí            |
| Eliminar un dispositivo                                                                                                                   | Sí            |
| Crear, modificar y eliminar un contenedor de volúmenes                                                                                   | No             |
| Crear, modificar y eliminar un volumen                                                                                             | No             |
| Crear, modificar y eliminar una directiva de copia de seguridad                                                                                      | No             |
| Creación de una copia de seguridad manual                                                                                                            | No             |
| Realizar una copia de seguridad programada                                                                                                         | No aplicable |
| Restaurar desde un conjunto de copias de seguridad                                                                                                        | No             |
| Clonar en un dispositivo que ejecuta Update 3.0 y versiones posteriores <br> El dispositivo de origen está ejecutando una versión anterior a Update 3.0.                                | Sí            |
| Clonar en un dispositivo que ejecuta versiones anteriores a Update 3.0                                                                          | No             |
| Conmutación por error como dispositivo de origen <br> (desde un dispositivo que ejecuta la versión anterior a Update 3.0 a un dispositivo que ejecuta Update 3.0 y versiones posteriores)                                                               | Sí            |
| Conmutación por error como dispositivo de destino <br> (a un dispositivo que ejecuta una versión de software anterior a Update 3.0)                                                                                   | No             |
| Desactivar una alerta                                                                                                                  | Sí            |
| Ver las directivas de copia de seguridad, el catálogo de copias de seguridad, volúmenes, contenedores de volúmenes, gráficos de supervisión, trabajos y alertas creadas en el Portal clásico | Sí            |
| Activar y desactivar los controladores de dispositivo                                                                                              | Sí            |


## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre el [proceso de implementación de StorSimple](storsimple-8000-deployment-walkthrough-u2.md).
* Obtenga más información sobre cómo [administrar su cuenta de almacenamiento de StorSimple](storsimple-8000-manage-storage-accounts.md).
* Obtenga más información acerca de cómo [usar el servicio StorSimple Device Manager para administrar cualquier dispositivo StorSimple](storsimple-8000-manager-service-administration.md).
