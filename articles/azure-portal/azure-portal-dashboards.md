---
title: Crear un panel en el Azure Portal
description: En este artículo se describe cómo crear y personalizar un panel en Azure Portal.
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.topic: how-to
ms.date: 03/16/2021
ms.openlocfilehash: fa7f1813d86571b568d23d64cab5705f8a117faa
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104774658"
---
# <a name="create-a-dashboard-in-the-azure-portal"></a>Crear un panel en el Azure Portal

Los paneles son una vista centrada y organizada de los recursos de nube de Azure Portal. Use los paneles como un área de trabajo donde puede supervisar los recursos e iniciar tareas rápidamente para las operaciones diarias. Por ejemplo, puede crear paneles personalizados basados en proyectos, tareas o roles de usuario.

Azure Portal proporciona un panel predeterminado como punto de partida. Puede editar el panel predeterminado y crear y personalizar paneles adicionales. En este artículo se describe cómo crear un nuevo panel y personalizarlo. Para más información sobre cómo compartir paneles, consulte [Uso compartido de paneles de Azure mediante el control de acceso basado en rol de Azure](azure-portal-dashboard-share-access.md).

## <a name="create-a-new-dashboard"></a>Creación de un panel

En este ejemplo, se crea un nuevo panel privado y se le asigna un nombre. Para comenzar, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el menú de Azure Portal, seleccione **Panel**. Es posible que la vista predeterminada ya esté definida en el panel.

    ![Apertura del panel](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. Seleccione **Nuevo panel** y, a continuación, **Panel en blanco**.

    ![Captura de pantalla del nuevo panel](./media/azure-portal-dashboards/create-new-dashboard.png)

    Esta acción abre la **Galería de iconos**, desde la que podrá seleccionar los iconos, y una cuadrícula vacía, desde donde podrá organizarlos.

1. Seleccione el texto **Mi panel** en la etiqueta del panel y escriba un nombre que le ayude a identificar fácilmente el panel personalizado.

    ![Captura de pantalla de la galería de iconos y la cuadrícula vacía](./media/azure-portal-dashboards/dashboard-name.png)

1. En el encabezado de página, seleccione **Personalización finalizada** para salir del modo de edición y, a continuación, seleccione **Guardar**.

    :::image type="content" source="media/azure-portal-dashboards/dashboard-save.png" alt-text="Captura de pantalla del proceso de guardado del panel":::

Ahora, la vista de panel muestra el panel nuevo. Seleccione la flecha situada junto al nombre del panel para ver los paneles disponibles. La lista puede incluir paneles que otros usuarios hayan creado y compartido.

## <a name="edit-a-dashboard"></a>Edición de paneles

Ahora vamos a editar el panel para agregar y organizar los iconos que representan los recursos de Azure y cambiar su tamaño.

### <a name="add-tiles-from-the-tile-gallery"></a>Agregar iconos desde la galería de iconos

Para agregar iconos a un panel, siga estos pasos:

1. Seleccione ![icono de edición](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Editar** en el encabezado de la página.

    ![Captura de pantalla con el icono de edición resaltado](./media/azure-portal-dashboards/dashboard-edit.png)

1. Examine la **Galería de iconos** o use el campo de búsqueda para buscar el icono que quiera.

1. Seleccione **Agregar** para agregar el icono al panel con una ubicación y un tamaño predeterminados. O bien, arrastre el icono a la cuadrícula y colóquelo donde quiera. Agregue los iconos que desee, pero aquí tiene un par de ideas:

    - Agregue **Todos los recursos** para ver los recursos que ya ha creado.

    - Si trabaja con más de una organización, agregue el icono **Identidad de la organización** al panel para mostrar con claridad a qué organización pertenecen los recursos.

1. En el encabezado de página, seleccione **Guardar**.

### <a name="add-tiles-from-a-resource-page"></a>Adición de iconos de una página de recursos

Existe una forma alternativa de agregar iconos al panel. Muchas páginas de recursos incluyen un icono de marcador en la barra de comandos. Si selecciona el icono, se ancla un icono que representa la página de origen al panel activo. 

![Captura de pantalla de la barra de comandos de la página con el icono de anclaje](./media/azure-portal-dashboards/dashboard-pin-blade.png)

### <a name="resize-or-rearrange-tiles"></a>Reorganizar los iconos o cambiar su tamaño

Para cambiar el tamaño de un icono o para reorganizar los iconos de un panel, siga estos pasos:

1. Seleccione ![icono de edición](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Editar** en el encabezado de la página.

1. Seleccione el menú contextual en la esquina superior derecha de un icono. A continuación, elija un tamaño de icono. Los iconos que admiten cualquier tamaño también incluyen un "identificador" en la esquina inferior derecha que le permite arrastrar el icono al tamaño que quiera.

    ![Captura de pantalla del panel con el menú de tamaño de icono abierto](./media/azure-portal-dashboards/dashboard-tile-resize.png)

1. Seleccione un icono y arrástrelo a una nueva ubicación en la cuadrícula para organizar el panel.

### <a name="additional-tile-configuration"></a>Configuración de iconos adicional

Puede que sea necesario configurar adicionalmente algunos iconos para que muestren la información necesaria. Por ejemplo, el icono **Gráfico de métricas** debe configurarse para mostrar una métrica de Azure Monitor. También puede personalizar los datos del icono para invalidar la configuración de tiempo predeterminada del panel.

Cualquier icono que deba configurarse muestra un banner hasta que personaliza. En el **Gráfico de métricas**, el banner se **Edita en métricas**. Para personalizar el icono:

1. En el encabezado de página, seleccione **Guardar** para salir del modo de edición.

1. Seleccione el banner y, a continuación, realice la configuración necesaria.

    ![Captura de pantalla del icono que requiere configuración](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Un icono de Markdown le permite mostrar contenido estático personalizado en el panel. Podrían ser instrucciones básicas, una imagen, un conjunto de hipervínculos o incluso la información de contacto. Para más información sobre el uso de un icono de Markdown, vea [Uso de un icono de Markdown en los paneles de Azure para mostrar contenido personalizado](azure-portal-markdown-tile.md).

### <a name="customize-tile-data"></a>Personalizar los datos del icono

Los datos del panel muestran automáticamente la actividad de las últimas 24 horas. Para mostrar un intervalo de tiempo diferente solo para este icono, siga estos pasos:

1. Seleccione **Personalizar los datos del icono** en el menú contextual o el filtro ![icono de filtro](./media/azure-portal-dashboards/dashboard-filter.png) en la esquina superior izquierda del icono.

    ![Captura de pantalla del menú contextual del icono](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. Active la casilla para **Anular la configuración de tiempo del panel en el nivel de icono**.

    ![Captura de pantalla del cuadro de diálogo para configurar las opciones de tiempo del icono](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Elija el intervalo de tiempo que se mostrará para este icono. Puede elegir entre los últimos 30 minutos y los últimos 30 días o definir un intervalo personalizado.

1. Elija la granularidad de tiempo que se mostrará. Puede mostrar incrementos de entre un minuto y un mes.

1. Seleccione **Aplicar**.

## <a name="delete-a-tile"></a>Eliminar un icono

Para quitar un icono de un panel, siga estos pasos:

* Seleccione el menú contextual en la esquina superior derecha del icono y, a continuación, seleccione **Quitar del panel**. O bien,

* Seleccione ![icono de edición](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Editar** para acceder al modo de personalización. Mantenga el puntero en la esquina superior derecha del icono y, a continuación, seleccione el icono de eliminación ![icono de eliminación](./media/azure-portal-dashboards/dashboard-delete-icon.png) para quitar el icono del panel.

   ![Captura de pantalla que muestra cómo quitar el icono del panel](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Clonar un panel

Para usar un panel existente como plantilla para uno nuevo, siga estos pasos:

1. Asegúrese de que la vista de panel muestra el panel que quiere copiar.

1. En el encabezado de página, seleccione ![icono para clonar](./media/azure-portal-dashboards/dashboard-clone.png) **Clonar**.

1. Se abre una copia del panel denominada **Clon de** *nombre del panel* en modo de edición. Utilice los pasos anteriores de este artículo para cambiar el nombre del panel y personalizarlo.

## <a name="publish-and-share-a-dashboard"></a>Publicar y compartir un panel

Al crear un panel, será privado de forma predeterminada, lo que significa que será la única persona que puede verlo. Para poner paneles a disposición de otros, puede publicarlos y compartirlos. Para más información, consulte [Uso compartido de paneles de Azure mediante el control de acceso basado en rol de Azure](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Abrir un panel compartido

Para buscar y abrir un panel compartido, siga estos pasos:

1. Seleccione la flecha junto al nombre del panel.

1. Realice la selección a partir de la lista de paneles que se muestra. Si no se muestra el panel que desea abrir:

    1. seleccione **Examinar todos los paneles**.

        ![Captura de pantalla del menú de selección del panel](./media/azure-portal-dashboards/dashboard-browse.png)

    1. En el campo **Tipo**, seleccione **Paneles compartidos**.

        ![Captura de pantalla del menú de selección de todos los paneles](./media/azure-portal-dashboards/dashboard-browse-all.png)

    1. Seleccione una o varias suscripciones. También puede escribir texto para filtrar los paneles por nombre.

    1. Seleccione un panel en la lista de paneles compartidos.

## <a name="delete-a-dashboard"></a>Eliminar un panel

Para eliminar permanentemente un panel privado o compartido, siga estos pasos:

1. Seleccione el panel que quiera eliminar en la lista situada junto al nombre del panel.

1. Seleccione ![icono de eliminación](./media/azure-portal-dashboards/dashboard-delete-icon.png) **Eliminar** en el encabezado de página.

1. Para un panel privado, seleccione **Aceptar** en el cuadro de diálogo de confirmación para quitar el panel. Para un panel compartido, en el cuadro de diálogo de confirmación, active la casilla para confirmar que otros usuarios ya no podrán ver el panel publicado. Después, seleccione **Aceptar**.

    ![Captura de pantalla de confirmación de eliminación](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="recover-a-deleted-dashboard"></a>Recuperación de un panel eliminado

Si se encuentra en la nube global de Azure y elimina un panel _publicado_ en Azure Portal, puede recuperar el panel en el plazo de 14 días a partir de la eliminación. Para obtener más información, consulte [Recuperación de un panel eliminado en Azure Portal](recover-shared-deleted-dashboard.md).

## <a name="next-steps"></a>Pasos siguientes

* [Uso compartido de paneles de Azure mediante el control de acceso basado en rol de Azure](azure-portal-dashboard-share-access.md)
* [Creación mediante programación de paneles de Azure](azure-portal-dashboards-create-programmatically.md)
