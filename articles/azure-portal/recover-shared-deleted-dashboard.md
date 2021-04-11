---
title: Recuperación de un panel eliminado en el Azure Portal
description: Si elimina un panel publicado en Azure Portal, puede recuperar el panel.
ms.date: 03/25/2021
ms.topic: troubleshooting
ms.openlocfilehash: 96d330872327f8719e7cc4287415d86fd0ae5fd4
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105559727"
---
# <a name="recover-a-deleted-dashboard-in-the-azure-portal"></a>Recuperación de un panel eliminado en el Azure Portal

Si se encuentra en la nube global de Azure y elimina un panel _publicado_ en Azure Portal, puede recuperar el panel en el plazo de 14 días a partir de la eliminación. Si se encuentra en la nube de Azure Government o el panel no está publicado, no puede recuperarlo y debe volver a crearlo. Para obtener más información sobre la publicación de un panel, consulte [Publicación del panel](azure-portal-dashboard-share-access.md#publish-a-dashboard). Siga estos pasos para recuperar un panel publicado:

1. En el menú de Azure Portal, seleccione **Grupos de recursos** y, a continuación, seleccione el grupo de recursos donde publicó el panel (de manera predeterminada, se denomina **paneles**).

1. En **Registro de actividad**, expanda la operación **Eliminar panel**. Seleccione la pestaña **Historial de cambios** y, a continuación, seleccione **\<deleted resource\>** .

    ![Captura de pantalla de la pestaña Historial de cambios](media/recover-shared-deleted-dashboard/change-history-tab.png)

1. Seleccione y copie el contenido del panel izquierdo y, a continuación, guárdelo en un archivo de texto con una extensión de archivo _.json_. El portal usa el archivo JSON para volver a crear el panel.

    ![Captura de pantalla de diferencias del Historial de cambios](media/recover-shared-deleted-dashboard/change-history-diff.png)

1. En el menú de Azure Portal, seleccione **Paneles** y, a continuación, seleccione **Cargar**.

    ![Captura de pantalla de Cargar en el panel](media/recover-shared-deleted-dashboard/dashboard-upload.png)

1. Seleccione el archivo JSON que guardó. El portal vuelve a crear el panel con el mismo nombre y los mismos elementos que el panel eliminado.

1. Seleccione **Compartir** para publicar el panel y volver a establecer el control de acceso apropiado.

    ![Captura de pantalla de Compartir en el panel](media/recover-shared-deleted-dashboard/dashboard-share.png)
