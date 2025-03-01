---
title: Lenguaje Bicep para plantillas de Azure Resource Manager
description: Describe el lenguaje Bicep para implementar la infraestructura en Azure a través de plantillas de Azure Resource Manager.
ms.topic: conceptual
ms.date: 03/23/2021
ms.openlocfilehash: 74028c682b48a492c2e8f13bef538d1694370cbd
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "104955914"
---
# <a name="what-is-bicep-preview"></a>¿Qué es Bicep (versión preliminar)?

Bicep es un lenguaje para implementar mediante declaración los recursos de Azure. Puede usar Bicep en lugar de JSON para desarrollar las plantillas de Azure Resource Manager (plantillas de ARM). Bicep simplifica la experiencia de creación gracias a su sintaxis concisa, una mejor compatibilidad con la reutilización de código y seguridad de tipos mejorada. Bicep es un lenguaje específico de dominio (DSL), lo que significa que está diseñado para un escenario o dominio en particular. Bicep no está pensado como lenguaje general de programación para escribir aplicaciones.

La sintaxis JSON para crear una plantilla puede ser detallada y requerir expresiones complicadas. Bicep mejora esa experiencia sin perder ninguna de las funcionalidades de la plantilla JSON. Se trata de una abstracción transparente en JSON para las plantillas de ARM. Cada archivo de Bicep se compila en una plantilla de ARM estándar. Los tipos de recursos, las versiones de API y las propiedades que son válidos en una plantilla de ARM son válidos en un archivo de Bicep. La versión actual tiene algunas [limitaciones conocidas](#known-limitations).

Bicep está actualmente en versión preliminar. Para realizar el seguimiento del estado del trabajo, consulte el [repositorio del proyecto Bicep](https://github.com/Azure/bicep).

Para obtener información sobre Bicep, consulte el vídeo siguiente.

> [!VIDEO https://www.youtube.com/embed/sc1kJfcRQgY]

## <a name="get-started"></a>Introducción

Para empezar con Bicep, [instale las herramientas](bicep-install.md).

Después de instalar las herramientas, pruebe el [tutorial de Bicep](./bicep-tutorial-create-first-bicep.md). La serie de tutoriales le guía a través de la estructura y las funcionalidades de Bicep. Implementa los archivos de Bicep y convierte una plantilla de ARM en el archivo de Bicep equivalente.

Para ver los archivos JSON y Bicep equivalentes en paralelo, consulte [Bicep Playground](https://aka.ms/bicepdemo).

Si ya tiene una plantilla de ARM que le gustaría convertir a Bicep, consulte [Conversión de plantillas de ARM entre JSON y Bicep](bicep-decompile.md).

## <a name="bicep-improvements"></a>Mejoras de Bicep

Bicep ofrece una sintaxis más sencilla y concisa en comparación con el JSON equivalente. No se usan expresiones `[...]`. En su lugar, se llama directamente a las funciones y se obtienen los valores de los parámetros y las variables. Se asigna un nombre simbólico a cada recurso implementado, lo que facilita la referencia a ese recurso en la plantilla.

Por ejemplo, el siguiente JSON devuelve un valor de salida de una propiedad de recurso.

```json
"outputs": {
  "hostname": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))).dnsSettings.fqdn]"
    },
}
```

La expresión de salida equivalente en Bicep es más fácil de escribir. En el ejemplo siguiente se devuelve la misma propiedad con el nombre simbólico **publicIP** para un recurso que está definido en la plantilla:

```bicep
output hostname string = publicIP.properties.dnsSettings.fqdn
```

Para ver una comparación completa de la sintaxis, consulte [Comparación de JSON y Bicep para plantillas](compare-template-syntax.md).

Bicep administra automáticamente las dependencias entre los recursos. Puede evitar configurar `dependsOn` cuando el nombre simbólico de un recurso se use en otra declaración de recursos.

Con Bicep, puede dividir el proyecto en varios módulos.

La estructura del archivo de Bicep es más flexible que la plantilla JSON. Puede declarar parámetros, variables y salidas en cualquier parte del archivo. En JSON, tiene que declarar todos los parámetros, variables y salidas dentro de las secciones correspondientes de la plantilla.

La extensión de VS Code para Bicep ofrece validación enriquecida e IntelliSense. Por ejemplo, puede usar la función IntelliSense de la extensión para obtener las propiedades de un recurso.

## <a name="known-limitations"></a>Restricciones conocidas

Actualmente existen los siguientes límites:

* No se puede establecer el modo ni el tamaño del lote en bucles de copia.
* No se pueden combinar bucles y condiciones.
* No se admiten objetos y matrices de una sola línea, como `['a', 'b', 'c']`.

## <a name="faq"></a>Preguntas más frecuentes

**¿Por qué crear un nuevo lenguaje en lugar de usar uno existente?**

Puede considerar Bicep como una revisión del lenguaje de plantillas de ARM existente en lugar de un nuevo lenguaje. La sintaxis ha cambiado, pero la funcionalidad básica y el entorno en tiempo de ejecución siguen siendo los mismos.

Antes de desarrollar Bicep, se consideró el uso de un lenguaje de programación existente. Se decidió que el público de destino encontraría más fácil aprender Bicep en lugar de empezar a trabajar con otro lenguaje.

**¿Por qué no centrar la energía en Terraform u otras ofertas de infraestructura como código de terceros?**

Los distintos usuarios prefieren distintos lenguajes y herramientas de configuración. Queremos asegurarnos de que todas estas herramientas ofrezcan una gran experiencia en Azure. Bicep es parte de ese esfuerzo.

Si está satisfecho con Terraform, no hay ninguna razón para cambiar. Microsoft se compromete a asegurarse de que Terraform en Azure sea lo mejor posible.

En el caso de los clientes que han seleccionado plantillas de ARM, creemos que Bicep mejora la experiencia de creación. Bicep también ayuda a realizar la transición de los clientes que no han adoptado una infraestructura como código.

**¿Está Bicep dirigido solo a Azure?**

Bicep es un DSL que se centra en la implementación de soluciones completas en Azure. Para satisfacer ese objetivo, es necesario trabajar con algunas API que están fuera de Azure. Esperamos proporcionar puntos de extensibilidad para esos escenarios.

**¿Qué ocurre con mis plantillas de ARM existentes?**

Siguen funcionando exactamente como siempre lo han hecho. No es necesario hacer ningún cambio. Seguiremos admitiendo el lenguaje JSON subyacente para las plantillas de ARM. Los archivos de Bicep se compilan en JSON, y ese JSON se envía a Azure para su implementación.

Cuando esté listo, puede [convertir los archivos JSON en Bicep](bicep-decompile.md).

## <a name="next-steps"></a>Pasos siguientes

Introducción al [tutorial de Bicep](./bicep-tutorial-create-first-bicep.md).
