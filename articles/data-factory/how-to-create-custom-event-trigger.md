---
title: Creación de desencadenadores de eventos personalizados en Azure Data Factory
description: Aprenda a crear un desencadenador personalizados en Azure Data Factory que ejecute una canalización en respuesta a un evento personalizado publicado en Event Grid.
ms.service: data-factory
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: 2d2f26b24e7fa10d9244de8f99d78c64a44b3d61
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104785655"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-a-custom-event-preview"></a>Creación de un desencadenador que ejecuta una canalización en respuesta a un evento personalizado (Versión preliminar)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describen los desencadenadores de eventos personalizados que se pueden crear en las canalizaciones de Data Factory.

La arquitectura dirigida por eventos (EDA) es un patrón de integración de datos común que implica la producción, detección, consumo y reacción a los eventos. Los escenarios de integración de datos a menudo requieren clientes de Data Factory que desencadenen canalizaciones basadas en la ocurrencia de ciertos eventos. La integración nativa de Data Factory con [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) ahora abarca [eventos personalizados](../event-grid/custom-topics.md): los clientes envían eventos arbitrarios a un tema de Event Grid y Data Factory se suscriben y escuchan el tema y desencadena canalizaciones en consecuencia.

> [!NOTE]
> La integración descrita en este artículo depende de [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Asegúrese de que el proveedor de la suscripción se registra con el proveedor de recursos de Event Grid. Para más información, consulte [Tipos y proveedores de recursos](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). Debe poder realizar la acción *Microsoft.EventGrid/eventSubscriptions/* *. Esta acción forma parte del rol integrado Colaborador de EventSubscription EventGrid.

Además, la combinación de los parámetros de canalización y el desencadenador de eventos personalizados, los clientes pueden analizar y hacer referencia a carga de _datos_ personalizada en ejecuciones de canalización El campo de _datos_ de la carga de eventos personalizados es una estructura de clave-valor JSON de forma libre, que permite a los clientes tener el máximo control sobre las ejecuciones de canalización basadas en eventos.

> [!IMPORTANT]
> Es posible que en ocasiones falte una clave referenciada en la parametrización en la carga de eventos personalizados. La _ejecución del desencadenador_ producirá un error, lo que indica que no se puede evaluar la expresión porque la propiedad _keyName_ no existe. El evento no desencadenará __ninguna__ _ejecución de canalización_.

## <a name="setup-event-grid-custom-topic"></a>Configuración de temas personalizados de Event Grid

Para usar el desencadenador de evento personalizado en Data Factory, _primero_ debe configurar un [tema personalizado en Event Grid](../event-grid/custom-topics.md). El flujo de trabajo es diferente del desencadenador de eventos de almacenamiento, donde Data Factory configurará el tema. Aquí necesita navegar por el Azure Event Grid y crear el tema. Para obtener más información sobre cómo crear el tema personalizado, consulte [Tutoriales del portal](../event-grid/custom-topics.md#azure-portal-tutorials) de Azure Event Grid y [tutoriales de la CLI](../event-grid/custom-topics.md#azure-cli-tutorials)

Las fábricas de datos esperan que los eventos sigan [el esquema de eventos de Event Grid](../event-grid/event-schema.md). Asegúrese de que las cargas de evento tienen los siguientes campos.

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory

En esta sección se muestra cómo crear un desencadenador de eventos de almacenamiento dentro de la interfaz de usuario de Azure Data Factory.

1. Cambie a la pestaña **Edit** (Editar), que se muestra con un símbolo de lápiz.

1. Seleccione **Trigger** (Desencadenador) en el menú y, después, seleccione **New/Edit** (Nuevo/Editar).

1. En la página **Add Triggers** (Agregar desencadenadores), seleccione **Choose trigger...** (Elegir desencadenador) y, después, seleccione **+New** (+Nuevo).

1. Seleccionar el tipo de desencadenador **Eventos personalizados**

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-1-creation.png" alt-text="Captura de pantalla de la página de autor para crear un nuevo desencadenador de eventos personalizado en la interfaz de usuario de Data Factory." lightbox="media/how-to-create-custom-event-trigger/custom-event-1-creation-expanded.png":::

1. Seleccione el tema personalizado en la lista desplegable suscripción de Azure o introduzca manualmente el ámbito del tema del evento.

   > [!NOTE]
   > Para crear un desencadenador de eventos personalizados o modificar uno existente, la cuenta de Azure usada para iniciar sesión en Data Factory y publicar el desencadenador de evento de almacenamiento debe tener el permiso control de acceso basado en rol (Azure RBAC) adecuado al tema. No se requiere ningún permiso adicional: la entidad de servicio para Azure Data Factory _no_ necesita permisos especiales para Event Grid. Para obtener más información sobre el control de acceso, consulte [Control de acceso basado en roles](#role-based-access-control).

1. El **Asunto comienza con** y los **extremos de asunto con** propiedades le permiten filtrar los eventos para los que desea desencadenar la canalización. Todas las propiedades son opcionales.

1. Use **+ New** para agregar los **tipos de evento** por los que desea filtrar. El desencadenar de eventos personalizados emplea una relación OR para la lista: si un evento personalizado tiene una propiedad _eventType_ que coincide con cualquiera de las que aparecen en esta lista, de desencadenará una ejecución de canalización. El tipo de evento distingue mayúsculas de minúsculas. Por ejemplo, en la siguiente captura de pantalla, el desencadenador coincide con todos los eventos _copycompleted_ o _copysucceeded_ cuyo asunto empiece por _fábricas_

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-2-properties.png" alt-text="Captura de pantalla de la página Editar desencadenador para explicar los tipos de eventos y el filtrado de asunto en la interfaz de usuario de Data Factory.":::

1. El desencadenador de eventos personalizado puede analizar y enviar la carga de _datos_ personalizada a la canalización. En primer lugar, cree los parámetros de canalización y rellene los valores en la página **Parámetros**. Use el formato **@triggerBody().event.data. _keyName_** para analizar la carga de datos y pasar los valores a los parámetros de canalización. Para obtener una explicación detallada, consulte [Referencia de metadatos de desencadenador en canalizaciones](how-to-use-trigger-parameterization.md) y [Variables del sistema en desencadenador de eventos personalizado](control-flow-system-variables.md#custom-event-trigger-scope)

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-4-trigger-values.png" alt-text="Captura de pantalla de la configuración de parámetros de canalización.":::

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-3-parameters.png" alt-text="Captura de pantalla de la página de parámetros para hacer referencia a la carga de datos en el evento personalizado.":::

1. Cuando haya terminado, haga clic en **Aceptar**.

## <a name="json-schema"></a>Esquema JSON

En la tabla siguiente se proporciona información general acerca de los elementos de esquema que están relacionados con los desencadenadores de eventos personalizados:

| **Elemento de JSON** | **Descripción** | **Tipo** | **Valores permitidos** | **Obligatorio** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | El identificador de recursos de Azure Resource Manager del tema de cuadrícula del evento. | String | Identificador de Azure Resource Manager | Sí |
| **eventos** | El tipo de eventos que provocan la activación de este desencadenador. | Matriz de cadenas    |  | Sí, se espera al menos un valor |
| **subjectBeginsWith** | El campo de asunto del blob debe comenzar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, `factories` solo activa el desencadenador para el asunto del evento a partir de `factories`. | String   | | No |
| **subjectEndsWith** | El campo de asunto del blob debe terminar con el patrón proporcionado para que se active el desencadenador. | String   | | No |

## <a name="role-based-access-control"></a>Control de acceso basado en rol

Azure Data Factory usa el control de acceso basado en roles de Azure (RBAC de Azure) para asegurarse de que el acceso no autorizado a la escucha, la suscripción a las actualizaciones de y el desencadenamiento de canalizaciones vinculadas a eventos personalizados, están estrictamente prohibidos.

* Para crear correctamente un desencadenador de evento personalizado existente o actualizarlo, la cuenta de Azure con la sesión iniciada en Data Factory debe tener el acceso adecuado a la cuenta de almacenamiento pertinente. De lo contrario, se _Deniega el acceso_ a la operación con error.
* Data Factory no necesita ningún permiso especial en el Event Grid y _no_ es necesario asignar un permiso de RBAC de Azure especial a Data Factory entidad de servicio para la operación.

En concreto, el cliente necesita el permiso _Microsoft.EventGrid/EventSubscriptions/Write_ en _/subscriptions/####/resourceGroups//####/providers/Microsoft.EventGrid/topics/someTopics_

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información detallada acerca de los desencadenadores, consulte el artículo [Pipeline execution and triggers](concepts-pipeline-execution-triggers.md#trigger-execution) (Ejecución de canalizaciones y desencadenadores).
* Aprenda a hacer referencia a los metadatos de desencadenador en la canalización; para ello, consulte [Referencia a los metadatos de desencadenador en las ejecuciones de canalización](how-to-use-trigger-parameterization.md).
