---
title: Agregar y ejecutar fragmentos de código con código alineado
description: Aprenda a crear y ejecutar fragmentos de código mediante acciones de código alineado para tareas y flujos de trabajo automatizados que cree con Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: deli, logicappspm
ms.topic: article
ms.date: 12/07/2020
ms.custom: devx-track-js
ms.openlocfilehash: 3f88fa38d62778bc3c4c1e29571d1d0ae4eeb5ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98179612"
---
# <a name="add-and-run-code-snippets-by-using-inline-code-in-azure-logic-apps"></a>Adición y ejecución de fragmentos de código mediante código en línea en Azure Logic Apps

Si desea ejecutar un fragmento de código dentro de su aplicación lógica, puede agregar la acción Código en línea integrada como un paso en el flujo de trabajo de la aplicación lógica. Esta acción funciona mejor si desea ejecutar código que se adapta a este escenario:

* Se ejecuta en JavaScript. Próximamente más lenguajes.

* Termina de ejecutarse en cinco segundos o menos.

* Administra datos de hasta 50 MB de tamaño.

* No es necesario trabajar con [acciones de **variables**](../logic-apps/logic-apps-create-variables-store-values.md), ya que aún no están permitidas.

* Usa la versión 8.11.1 de Node.js. Para más información, consulte [Objetos integrados estándar](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

  > [!NOTE]
  > La función `require()` no es compatible con la acción Código en línea para la ejecución de JavaScript.

Esta acción ejecuta el fragmento de código y devuelve la salida de ese fragmento como un token denominado `Result`. Puede usar este token en acciones posteriores del flujo de trabajo de la aplicación lógica. Para otros escenarios en los que quiera crear una función para el código intente, en su lugar, [crear y llamar a una función mediante Azure Functions](../logic-apps/logic-apps-azure-functions.md) en la aplicación lógica.

En este artículo, la aplicación lógica de ejemplo se desencadena cuando un nuevo correo electrónico llega a una cuenta profesional o educativa. El fragmento de código extrae y devuelve todas las direcciones de correo electrónico que aparecen en el cuerpo del correo electrónico.

![Captura de pantalla que muestra un ejemplo de aplicación lógica](./media/logic-apps-add-run-inline-code/inline-code-example-overview.png)

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La aplicación lógica donde desea agregar el fragmento de código, incluido un desencadenador. Si no tiene una aplicación lógica, consulte [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

   En el ejemplo de este tema se usa el desencadenador de Office 365 Outlook denominado **Cuando llega un nuevo correo electrónico**.

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) vinculada a la aplicación lógica.

  * Asegúrese de usar una cuenta de integración adecuada para su caso de uso o escenario.

    Por ejemplo, las cuentas de integración de [nivel gratuito](../logic-apps/logic-apps-pricing.md#integration-accounts) están diseñadas solo para escenarios y cargas de trabajo exploratorias, no para escenarios de producción, están limitadas en el uso y el rendimiento, y no se admiten en un contrato de nivel de servicio (SLA). Otros niveles implican costos, pero incluyen compatibilidad con el contrato de nivel de servicio, ofrecen más rendimiento y tienen límites más altos. Obtenga más información sobre los [niveles](../logic-apps/logic-apps-pricing.md#integration-accounts) de la cuenta de integración, los [precios](https://azure.microsoft.com/pricing/details/logic-apps/) y los [límites](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

   * Si no quiere usar una cuenta de integración, puede utilizar [Azure Logic Apps en versión preliminar](logic-apps-overview-preview.md) y crear una aplicación lógica desde el tipo de recurso **Logic Apps (versión preliminar)** .

     En la versión preliminar de Azure Logic Apps, **Código en línea** se denomina ahora **Operaciones de código en línea** junto con estas otras diferencias:

     * **Ejecute código de JavaScript** ahora se llama **Ejecutar JavaScript en línea**.

     * Si usa macOS o Linux, las acciones de Operaciones de código en línea no están disponibles actualmente si se emplea la extensión Azure Logic Apps (versión preliminar) en Visual Studio Code.

     * Las acciones de Operaciones de código en línea tienen [límites actualizados](logic-apps-overview-preview.md#inline-code-limits).

     Puede empezar desde cualquiera de estas opciones:

     * Crear la aplicación lógica desde el tipo de recurso **Logic Apps (versión preliminar)** [mediante Azure Portal](create-stateful-stateless-workflows-azure-portal.md).

     * Crear un proyecto para la aplicación lógica [mediante Visual Studio Code y la extensión Azure Logic Apps (versión preliminar)](create-stateful-stateless-workflows-visual-studio-code.md).

## <a name="add-inline-code"></a>Adición de código en línea

1. Si aún no lo ha hecho, en [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el Diseñador de aplicación lógica.

1. En el diseñador, elija dónde agregar la acción Código en línea en el flujo de trabajo de la aplicación lógica.

   * Para agregar la acción al final del flujo de trabajo, seleccione **Nuevo paso**.

   * Para agregar la acción entre pasos, mueva el puntero del mouse sobre la flecha que conecta esos pasos. Seleccione el signo más ( **+** ) que aparece y elija **Agregar una acción**.

   Este ejemplo agrega la acción Código en línea en el desencadenador de Office 365 Outlook.

   ![Adición del nuevo paso en el desencadenador](./media/logic-apps-add-run-inline-code/add-new-step.png)

1. En **Elegir una acción**, en el cuadro de búsqueda, escriba `inline code`. En la lista de acciones, seleccione la acción **Ejecutar código de JavaScript**.

   ![Selección de la acción "Ejecutar código de JavaScript"](./media/logic-apps-add-run-inline-code/select-inline-code-action.png)

   La acción aparece en el diseñador y, de forma predeterminada, contiene código de ejemplo que incluye una instrucción `return`.

   ![Acción Código en línea con código de ejemplo predeterminado](./media/logic-apps-add-run-inline-code/inline-code-action-default.png)

1. En el cuadro **Código**, elimine el código de ejemplo y escriba su código. Escriba el código que colocaría dentro de un método, pero sin la firma del método.

   Si empieza a escribir una palabra clave reconocida, aparece la lista de Autocompletar para que pueda seleccionar entre las palabras clave disponibles, por ejemplo:

   ![Lista de Autocompletar de palabras clave](./media/logic-apps-add-run-inline-code/auto-complete.png)

   Este fragmento de código de ejemplo crea primero una variable que almacena una *expresión regular*, que especifica un patrón de coincidencia con el texto de entrada. A continuación, el código crea una variable que almacena los datos del cuerpo del correo electrónico desde el desencadenador.

   ![Creación de variables](./media/logic-apps-add-run-inline-code/save-email-body-variable.png)

   Para hacer referencia más fácilmente a los resultados del desencadenador y las acciones anteriores, aparece la lista de contenido dinámico mientras el cursor se encuentra dentro del cuadro **Código**. En este ejemplo, la lista muestra los resultados disponibles del desencadenador, incluido el token **Body**, que ahora puede seleccionar.

   Después de seleccionar el token **Body**, la acción de código en línea resuelve el token para un objeto `workflowContext` que hace referencia al valor de la propiedad `Body` del correo electrónico:

   ![Selección del resultado](./media/logic-apps-add-run-inline-code/inline-code-example-select-outputs.png)

   En el cuadro **Código**, el fragmento puede usar el objeto `workflowContext` de solo lectura como entrada. Este objeto incluye propiedades que proporcionan al código acceso a los resultados de los desencadenadores y acciones anteriores en el flujo de trabajo. Para más información, consulte [Referencia a los resultados de desencadenadores y acciones en el código](#workflowcontext) más adelante en este tema.

   > [!NOTE]
   > Si el fragmento de código hace referencia a nombres de acción que utilizan el operador punto (.), debe agregar esos nombres de acción al parámetro [**Acciones**](#add-parameters). Esas referencias también deben incluir los nombres de acción entre corchetes ([]) y comillas, por ejemplo:
   >
   > `// Correct`</br> 
   > `workflowContext.actions["my.action.name"].body`</br>
   >
   > `// Incorrect`</br>
   > `workflowContext.actions.my.action.name.body`

   La acción de código en línea no requiere una instrucción `return`, pero los resultados de una instrucción `return` están disponibles como referencia en acciones posteriores mediante el token **Result**. Por ejemplo, el fragmento de código devuelve el resultado mediante una llamada a la función `match()`, que busca coincidencias en el cuerpo del correo electrónico con la expresión regular. La acción **Redactar** usa el token **Result** para hacer referencia a los resultados de la acción de código en línea y crea un solo resultado.

   ![Aplicación lógica terminada](./media/logic-apps-add-run-inline-code/inline-code-complete-example.png)

1. Cuando haya terminado, guarde la aplicación lógica.

<a name="workflowcontext"></a>

### <a name="reference-trigger-and-action-results-in-your-code"></a>Referencia a los resultados de desencadenadores y acciones en el código

El objeto `workflowContext` tiene esta estructura, que incluye las subpropiedades `actions`, `trigger` y `workflow`:

```json
{
   "workflowContext": {
      "actions": {
         "<action-name-1>": @actions('<action-name-1>'),
         "<action-name-2>": @actions('<action-name-2>')
      },
      "trigger": {
         @trigger()
      },
      "workflow": {
         @workflow()
      }
   }
}
```

Esta tabla contiene más información acerca de estas subpropiedades:

| Propiedad | Tipo | Descripción |
|----------|------|-------|
| `actions` | Colección de objetos | Objetos de resultado de acciones que se ejecutan antes de que el fragmento de código se ejecute. Cada objeto tiene un par *clave-valor* donde la clave es el nombre de una acción y el valor es equivalente a llamar a la [función actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions) con `@actions('<action-name>')`. El nombre de la acción utiliza el mismo nombre de acción que se usa en la definición del flujo de trabajo subyacente, que reemplaza los espacios (" ") en el nombre de acción por caracteres de subrayado (_). Este objeto proporciona acceso a los valores de propiedad de la acción de la ejecución de la instancia de flujo de trabajo actual. |
| `trigger` | Object | Objeto de resultado del desencadenador, equivalente a llamar a la [función trigger()](../logic-apps/workflow-definition-language-functions-reference.md#trigger). Este objeto proporciona acceso a los valores de propiedad del desencadenador de la ejecución de la instancia de flujo de trabajo actual. |
| `workflow` | Object | Objeto de flujo de trabajo, equivalente a llamar a la [función workflow()](../logic-apps/workflow-definition-language-functions-reference.md#workflow). Este objeto proporciona acceso a los valores de propiedad de flujo de trabajo, como el nombre del flujo de trabajo, el identificador de ejecución, etc., de la ejecución de la instancia de flujo de trabajo actual. |
|||

En el ejemplo de este tema, el objeto `workflowContext` tiene estas propiedades a las que puede acceder el código:

```json
{
   "workflowContext": {
      "trigger": {
         "name": "When_a_new_email_arrives",
         "inputs": {
            "host": {
               "connection": {
                  "name": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/office365"
               }
            },
            "method": "get",
            "path": "/Mail/OnNewEmail",
            "queries": {
               "includeAttachments": "False"
            }
         },
         "outputs": {
            "headers": {
               "Pragma": "no-cache",
               "Content-Type": "application/json; charset=utf-8",
               "Expires": "-1",
               "Content-Length": "962095"
            },
            "body": {
               "Id": "AAMkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgBGAAAAAABmZwxUQtCGTqSPpjjMQeD",
               "DateTimeReceived": "2019-03-28T19:42:16+00:00",
               "HasAttachment": false,
               "Subject": "Hello World",
               "BodyPreview": "Hello World",
               "Importance": 1,
               "ConversationId": "AAQkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgAQ",
               "IsRead": false,
               "IsHtml": true,
               "Body": "Hello World",
               "From": "<sender>@<domain>.com",
               "To": "<recipient-2>@<domain>.com;<recipient-2>@<domain>.com",
               "Cc": null,
               "Bcc": null,
               "Attachments": []
            }
         },
         "startTime": "2019-05-03T14:30:45.971564Z",
         "endTime": "2019-05-03T14:30:50.1746874Z",
         "scheduledTime": "2019-05-03T14:30:45.8778117Z",
         "trackingId": "1cd5ffbd-f989-4df5-a96a-6e9ce31d03c5",
         "clientTrackingId": "08586447130394969981639729333CU06",
         "originHistoryName": "08586447130394969981639729333CU06",
         "code": "OK",
         "status": "Succeeded"
      },
      "workflow": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>",
         "name": "<logic-app-workflow-name>",
         "type": "Microsoft.Logic/workflows",
         "location": "<Azure-region>",
         "run": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>/runs/08586453954668694173655267965CU00",
            "name": "08586453954668694173655267965CU00",
            "type": "Microsoft.Logic/workflows/runs"
         }
      }
   }
}
```

<a name="add-parameters"></a>

## <a name="add-parameters"></a>Adición de parámetros

En algunos casos, es posible que deba requerir de forma explícita que la acción Código en línea incluya los resultados del desencadenador o de acciones específicas a las que el código hace referencia como dependencias agregando los parámetros **Desencadenador** o **Acciones**. Esta opción es útil para escenarios donde no se encuentran los resultados referenciados en tiempo de ejecución.

> [!TIP]
> Si piensa reutilizar el código, agregue referencias a las propiedades mediante el cuadro **Código** para que su código incluya las referencias del token resueltas, en lugar de agregar el desencadenador o las acciones como dependencias explícitas.

Por ejemplo, suponga que tiene un código que hace referencia al resultado **SelectedOption** de la acción **Enviar correo electrónico de aprobación** para el conector de Office 365 Outlook. En tiempo de creación, el motor de Logic Apps analiza el código para determinar si ha hecho referencia a cualquier resultado de desencadenador o acción e incluye esos resultados automáticamente. En tiempo de ejecución, en caso de obtener un error que indique que el resultado del desencadenador o la acción referenciado no está disponible en el objeto `workflowContext` especificado, puede agregar el desencadenador o la acción como una dependencia explícita. En este ejemplo, agregará el parámetro **Acciones** y especificará que la acción Código en línea incluye explícitamente el resultado de la acción **Enviar correo electrónico de aprobación**.

Para agregar estos parámetros, abra la lista **Agregar nuevo parámetro** y seleccione los parámetros deseados:

   ![Adición de parámetros](./media/logic-apps-add-run-inline-code/inline-code-action-add-parameters.png)

   | Parámetro | Descripción |
   |-----------|-------------|
   | **Acciones** | Incluya los resultados de las acciones anteriores. Consulte [Incluir resultados de acción](#action-results). |
   | **Desencadenador** | Incluya los resultados del desencadenador. Consulte [Incluir resultados de desencadenador](#trigger-results). |
   |||

<a name="trigger-results"></a>

### <a name="include-trigger-results"></a>Incluir resultados de desencadenador

Si selecciona **Desencadenadores**, se le preguntará si desea incluir resultados de desencadenador.

* De la lista **Desencadenador**, seleccione **Sí**.

<a name="action-results"></a>

### <a name="include-action-results"></a>Incluir resultados de acción

Si selecciona **Acciones**, se le preguntará qué acciones desea agregar. Sin embargo, antes de empezar a agregar acciones, necesitará la versión del nombre de acción que aparece en la definición del flujo de trabajo subyacente de la aplicación lógica.

* Esta funcionalidad no admite variables, bucles ni índices de iteración.

* Los nombres de la definición del flujo de trabajo de la aplicación lógica usan un carácter de subrayado (_), no un espacio.

* Para los nombres de acción que utilizan el operador punto (.), incluya esos operadores; por ejemplo:

  `My.Action.Name`

1. En la barra de herramientas del diseñador, seleccione **Vista Código** y busque dentro del atributo `actions` el nombre de acción.

   Por ejemplo, `Send_approval_email_` es el nombre JSON de la acción **Enviar correo electrónico de aprobación**.

   ![Buscar nombre de acción en JSON](./media/logic-apps-add-run-inline-code/find-action-name-json.png)

1. Para volver a la vista del diseñador, en la barra de herramientas de la vista de código, seleccione **Diseñador**.

1. Para agregar la primera acción, en el cuadro **Actions Item - 1** (Elemento de acciones - 1), escriba el nombre de la acción JSON.

   ![Escribir la primera acción](./media/logic-apps-add-run-inline-code/add-action-parameter.png)

1. Para agregar otra acción, seleccione **Agregar un nuevo elemento**.

## <a name="reference"></a>Referencia

Para más información sobre la estructura y la sintaxis de la acción **Ejecutar código de JavaScript** de la definición del flujo de trabajo subyacente de su aplicación lógica que usa el lenguaje de definición de flujo de trabajo, consulte la [sección de referencia](../logic-apps/logic-apps-workflow-actions-triggers.md#run-javascript-code) de esta acción.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [conectores para Azure Logic Apps](../connectors/apis-list.md)
