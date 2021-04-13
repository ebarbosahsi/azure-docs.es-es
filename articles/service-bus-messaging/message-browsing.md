---
title: Exploración de mensajes de Azure Service Bus
description: Explorar e inspeccionar los mensajes de Service Bus permite a un cliente de Azure Service Bus enumerar todos los mensajes de una cola o suscripción.
ms.topic: article
ms.date: 03/29/2021
ms.openlocfilehash: f4943685f03eccb1c3b8da079973cf083bdcc416
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106090314"
---
# <a name="message-browsing"></a>Exploración de mensajes

La exploración de mensajes, o la inspección, permite a un cliente de Service Bus enumerar todos los mensajes de una cola o una suscripción con fines de diagnóstico o depuración.

La operación de inspección en una **cola** devuelve todos los mensajes de la cola, no solo los disponibles para la adquisición inmediata. La operación de inspección en una **suscripción** devuelve todos los mensajes excepto los mensajes programados en el registro de mensajes de la suscripción. 

Los mensajes consumidos y expirados se limpian mediante una ejecución de "recolección de elementos no utilizados" asincrónica. Este paso no se produce necesariamente inmediatamente después de que expiren los mensajes. Por eso, una operación interactiva puede devolver mensajes que ya han expirado. Estos mensajes se quitarán o se pondrán en la cola de mensajes fallidos cuando se invoque una operación de recepción en la cola o la suscripción la próxima vez. Tenga en cuenta este comportamiento al intentar recuperar los mensajes aplazados de la cola. Un mensaje expirado ya no es apto para la recuperación normal mediante ningún otro medio, incluso cuando se devuelve con Peek. Estos mensajes se devuelven por diseño dado que Peek es una herramienta de diagnóstico que refleja el estado actual del registro.

Peek también devuelve los mensajes que estaban bloqueados y que actualmente están procesando otros receptores. Sin embargo, dado que Peek devuelve una instantánea desconectada, el estado de bloqueo de un mensaje no se puede observar en los mensajes inspeccionados.

## <a name="peek-apis"></a>API de Peek
## <a name="azuremessagingservicebus"></a>[Azure.Messaging.ServiceBus](#tab/dotnet)
Los métodos [PeekMessageAsync](/dotnet/api/azure.messaging.servicebus.servicebusreceiver.peekmessageasync) y [PeekMessagesAsync](/dotnet/api/azure.messaging.servicebus.servicebusreceiver.peekmessagesasync) existen en los objetos de receptor: `ServiceBusReceiver` y `ServiceBusSessionReceiver`. Peek funciona en las colas y suscripciones y en sus respectivas colas de mensajes fallidos.

Cuando se llama de manera repetida, `PeekMessageAsync` enumera todos los mensajes que existen en el registro de la cola o suscripción, en orden, desde el número de secuencia más bajo disponible hasta el más alto. Este es el orden en que los mensajes se ponen en cola; no es el orden en que finalmente se podrían recuperar.
PeekMessagesAsync recupera varios mensajes y los devuelve como una enumeración. Si no hay mensajes disponibles, el objeto de enumeración está vacío, no es null.

También puede rellenar el parámetro [fromSequenceNumber](/dotnet/api/microsoft.servicebus.messaging.eventposition.fromsequencenumber) con un SequenceNumber en el que iniciar y, a continuación, llamar de nuevo al método sin especificar el parámetro que se va a enumerar más adelante. `PeekMessagesAsync` funciona de forma equivalente, pero recupera un conjunto de mensajes a la vez.


## <a name="microsoftazureservicebus"></a>[Microsoft.Azure.ServiceBus](#tab/dotnetold)
Los métodos [Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) y [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) existen en los objetos receptores: `MessageReceiver` y `MessageSession`. Peek funciona en las colas y suscripciones y en sus respectivas colas de mensajes fallidos.

Cuando se llama de manera repetida, `Peek` enumera todos los mensajes que existen en el registro de la cola o suscripción, en orden, desde el número de secuencia más bajo disponible hasta el más alto. Este es el orden en que los mensajes se ponen en cola; no es el orden en que finalmente se podrían recuperar.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) recupera varios mensajes y los devuelve como una enumeración. Si no hay mensajes disponibles, el objeto de enumeración está vacío, no es null.

También puede usar una sobrecarga del método con un valor de [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) con el que comenzar y, luego, llamar a la sobrecarga del método sin parámetros para enumerar más. **PeekBatch** funciona de forma equivalente, pero recupera un conjunto de mensajes a la vez.


---

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la mensajería de Service Bus, consulte los siguientes temas:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
