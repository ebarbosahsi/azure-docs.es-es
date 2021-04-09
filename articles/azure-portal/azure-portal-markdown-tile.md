---
title: Uso de un icono de Markdown en los paneles de Azure
description: Aprenda a agregar un icono de Markdown a un panel de Azure para mostrar contenido estático
ms.date: 03/19/2021
ms.topic: how-to
ms.custom: devx-track-js
ms.openlocfilehash: 8324b736565cfa353e48cf49b76e2784866f47f7
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104774463"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Uso de un icono de Markdown en los paneles de Azure para mostrar contenido personalizado

Puede agregar un icono de Markdown a los paneles de Azure para mostrar contenido estático y personalizado. Por ejemplo, puede mostrar instrucciones básicas, una imagen o un conjunto de hipervínculos en un icono de Markdown.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Incorporación de un icono de Markdown al panel

1. Seleccione **Panel** en la barra lateral de Azure Portal.

   ![Captura de pantalla que muestra la barra lateral del portal](./media/azure-portal-markdown-tile/azure-portal-nav.png)

1. En la vista panel, seleccione el panel en el que debe aparecer el icono de Markdown personalizado y, después, seleccione **Editar**.

   ![Captura de pantalla de la vista de edición del panel](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

1. En la **Galería de iconos**, busque el denominado **Markdown** y seleccione **Agregar**. Este icono se agregará al panel y el panel **Editar Markdown** se abrirá.

1. Escriba los valores de **título** y **subtítulo** que se muestran en el icono después de moverse a otro campo.

   ![Captura de pantalla que muestra los resultados de escribir el título y el subtítulo](./media/azure-portal-markdown-tile/azure-portal-dashboard-enter-title.png)

1. Seleccione una de las opciones para incluir contenido Markdown: **Edición directa** o **Insertar contenido mediante URL**.

   - Seleccione **Edición directa** si quiere escribir contenido Markdown directamente.

      ![Captura de pantalla que muestra cuando se escribe contenido insertado](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-inline-content.png)

   - Seleccione **Insertar contenido mediante URL** si quiere usar contenido Markdown existente hospedado en línea.

      ![Captura de pantalla que muestra cuando se escribe a través de URL](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-url.png)

      > [!NOTE]
      > Para mayor seguridad, puede crear un archivo Markdown y almacenarlo en un [blob de cuenta de Azure Storage con el cifrado habilitado](../storage/common/storage-service-encryption.md) y, después, apuntar al archivo mediante la opción de dirección URL. El contenido Markdown se cifra mediante las opciones de cifrado de la cuenta de almacenamiento. Solo los usuarios con permisos para el archivo pueden ver el contenido Markdown en el panel. Puede que necesite establecer una regla de [uso compartido de recursos entre orígenes (CORS)](/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) en la cuenta de almacenamiento para que Azure Portal ( _https://portal.azure.com/_ ) pueda acceder al archivo Markdown en el blob.

1. Seleccione **Listo** para descartar el panel **Editar Markdown**. El contenido aparece en el icono de Markdown, cuyo tamaño puede cambiarse al arrastrar el manipulador de la esquina inferior derecha.

   ![Captura de pantalla del icono de Markdown personalizado](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Funcionalidades y limitaciones del contenido de Markdown

En el icono de Markdown puede usar cualquier combinación de texto sin formato, la sintaxis de Markdown y contenido HTML. Azure Portal usa una biblioteca de código abierto denominada _marked_ para transformar el contenido en HTML que se muestra en el icono. El código HTML generado por _marked_ se procesa previamente en el portal antes de representarse. Este paso ayuda a garantizar que la personalización no afecta a la seguridad o al diseño del portal. Durante ese procesamiento previo, se quita cualquier parte del código HTML que suponga una posible amenaza. En el portal no se permiten los siguientes tipos de contenido:

* JavaScript: `<script>`se quitan las etiquetas y las evaluaciones de JavaScript insertadas.
* iframes: `<iframe>` se quitan las etiquetas.
* `<style>`Se quitan las etiquetas de estilo. Oficialmente no se admiten los atributos de estilo insertados en los elementos HTML. Quizá algunos elementos insertados le pudieran ser de ayuda, pero si afectan al diseño del portal, dejarían de funcionar en cualquier momento. El icono de Markdown está pensado para contenido estático básico que usa los estilos predeterminados del portal.

## <a name="next-steps"></a>Pasos siguientes

* Para crear un panel personalizado, consulte [Creación y uso compartido de paneles en Azure Portal](../azure-portal/azure-portal-dashboards.md)
