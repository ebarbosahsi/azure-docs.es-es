---
title: 'Recuperación ante desastres geográfica: Azure Event Hubs| Microsoft Docs'
description: Cómo usar regiones geográficas para conmutar por error y llevar a cabo una recuperación ante desastres en Azure Event Hubs
ms.topic: article
ms.date: 02/10/2021
ms.openlocfilehash: f3b74b89f47582fbb3f1640f315f413ab86b26b5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104602645"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Event Hubs: recuperación ante desastres geográfica 

Muchas empresas y, en algunos casos, las normativas del sector imponen como requisito la resistencia frente a interrupciones desastrosas de los recursos de procesamiento de datos. 

Azure Event Hubs ya reparte el riesgo de errores catastróficos de equipos individuales, o incluso bastidores completos, entre clústeres que abarcan varios dominios de error en un centro de recursos. Además, implementa mecanismos transparentes de detección de errores y conmutación por error, de modo que el servicio siga funcionando dentro de los niveles de servicio garantizados y normalmente sin interrupciones apreciables en caso de que se produzcan tales errores. Si se ha creado un espacio de nombres de Event Hubs con la opción habilitada para [zonas de disponibilidad](../availability-zones/az-overview.md), el riesgo de interrupción se reparte entre tres instalaciones separadas físicamente, y el servicio tiene suficientes reservas de capacidad para hacer frente de inmediato a la pérdida completa y catastrófica de toda la instalación. 

El modelo de clúster todo activo de Azure Event Hubs con compatibilidad para zonas de disponibilidad proporciona resistencia frente a errores graves de hardware e incluso pérdidas catastróficas de instalaciones de centros de datos completas. Aun así, puede haber situaciones graves con una destrucción física generalizada frente a las que ni siquiera estas medidas puedan ofrecer una protección suficiente. 

La característica de recuperación ante desastres geográfica de Event Hubs está diseñada para que sea más fácil recuperarse ante un desastre de esta magnitud y abandonar para siempre una región de Azure con errores, sin necesidad de cambiar las opciones de configuración de la aplicación. El abandono de una región de Azure suele implicar varios servicios. Esta característica se centra principalmente en ayudar a mantener la integridad de la configuración de la aplicación compuesta.  

La característica de recuperación ante desastres geográfica garantiza que toda la configuración de un espacio de nombres (Event Hubs, grupos de consumidores y parámetros) se replique continuamente de un espacio de nombres principal a uno secundario cuando se emparejan. Además, permite iniciar un movimiento de conmutación por error solo una vez del espacio de nombres principal al secundario en cualquier momento. El movimiento de conmutación por error volverá a apuntar el nombre de alias elegido para el espacio de nombres al espacio de nombres secundario y, luego, interrumpirá el emparejamiento. La conmutación por error es casi instantánea una vez que se ha iniciado. 

> [!IMPORTANT]
> La característica permite la continuidad instantánea de las operaciones con la misma configuración, pero **no replica los datos de eventos**. A menos que el desastre ocasione la pérdida de todas las zonas, los datos de eventos que se conservan en el centro de eventos principal después de la conmutación por error podrán recuperarse y se podrán obtener los eventos históricos de ahí una vez restaurado el acceso. Para replicar los datos de eventos y operar los espacios de nombres correspondientes en configuraciones de tipo activo/activo a fin de hacer frente a interrupciones y desastres, no se incline por este conjunto de características de recuperación ante desastres geográfica. En su lugar, siga la [guía de replicación](event-hubs-federation-overview.md).  

## <a name="outages-and-disasters"></a>Interrupciones y desastres

Es importante tener en cuenta la distinción entre "interrupciones" y "desastres". Una **interrupción** es la falta de disponibilidad temporal de Azure Event Hubs y puede afectar a algunos componentes del servicio, como un almacén de mensajes o incluso todo el centro de datos. Sin embargo, después de corregir el problema, Event Hubs está de nuevo disponible. Normalmente, una interrupción no provoca la pérdida de mensajes ni otros datos. Un ejemplo de una interrupción de este tipo podría ser un error de corriente en el centro de datos. Algunas interrupciones son solo breves pérdidas de conexión debido a problemas transitorios o de red. 

Un *desastre* se define como la pérdida permanente o a largo plazo de un clúster de Event Hubs, una región de Azure o un centro de datos. La región o el centro de datos no volverá necesariamente a estar disponible, o puede que esté fuera de servicio durante horas o días. Algunos ejemplos de esos desastres son los incendios, las inundaciones o los terremotos. Un desastre que se convierte en permanente podría provocar la pérdida de algunos mensajes, eventos u otros datos. Sin embargo, en la mayoría de los casos, no debe producirse una pérdida de datos y se pueden recuperar los mensajes una vez que se realiza la copia de seguridad del centro de datos.

La característica de recuperación ante desastres con localización geográfica de Azure Event Hubs es una solución de recuperación ante desastres. Los conceptos y el flujo de trabajo descritos en este artículo se aplican a situaciones catastróficas y no a interrupciones transitorias o temporales. Para obtener una explicación detallada de la recuperación ante desastres en Microsoft Azure, consulte [este artículo](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Términos y conceptos básicos

La característica de recuperación ante desastres implementa la recuperación ante desastres de metadatos y depende de espacios de nombres de recuperación ante desastres principales y secundarios. 

La característica de recuperación ante desastres geográfica solo está disponible para las [SKU estándar y dedicadas](https://azure.microsoft.com/pricing/details/event-hubs/). No es necesario realizar ningún cambio de la cadena de conexión, ya que la conexión se realiza a través de un alias.

Los siguientes términos se utilizan en este artículo:

-  *Alias*: el nombre para una configuración de recuperación ante desastres que ha configurado. El alias proporciona una sola cadena de conexión estable de nombre de dominio completo (FQDN). Las aplicaciones usan esta cadena de conexión de alias para conectarse a un espacio de nombres. 

-  *Espacio de nombres principal o secundario*: los espacio de nombres que corresponden al alias. El espacio de nombres principal está "activo" y recibe mensajes (puede ser un espacio de nombres existente o uno nuevo). El espacio de nombres secundario es "pasivo" y no recibe mensajes. Los metadatos entre ambos están sincronizados, por lo que ambos pueden aceptar sin problemas mensajes sin ningún cambio de código de la aplicación o cadena de conexión. Para asegurarse de que solo el espacio de nombres activo recibe mensajes, tiene que utilizar el alias.
-  *Metadatos*: entidades como centros de eventos y grupos de consumidores; y sus propiedades del servicio que están asociadas con el espacio de nombres. Solo las entidades y sus valores se replican automáticamente. No se replican los mensajes ni los eventos. 
-  *Conmutación por error*: el proceso de activación del espacio de nombres secundario.

## <a name="supported-namespace-pairs"></a>Pares de espacios de nombres admitidos
Se admiten las siguientes combinaciones de espacios de nombres principales y secundarios:  

| Espacio de nombres principal | Espacio de nombres secundario | Compatible | 
| ----------------- | -------------------- | ---------- |
| Estándar | Estándar | Sí | 
| Estándar | Dedicado | Sí | 
| Dedicado | Dedicado | Sí | 
| Dedicado | Estándar | No | 

> [!NOTE]
> No se pueden emparejar espacios de nombres que se encuentran en el mismo clúster dedicado. Puede emparejar espacios de nombres que se encuentran en clústeres independientes. 

## <a name="setup-and-failover-flow"></a>Flujo de conmutación por error y configuración

La siguiente sección contiene información general del proceso de conmutación por error y explica cómo configurar la conmutación por error inicial. 

![1][]

### <a name="setup"></a>Configurar

En primer lugar cree un espacio de nombres principal o use uno ya existente, y un nuevo espacio de nombres secundario, luego emparéjelos. Este emparejamiento le proporciona un alias que puede usar para conectarse. Al usar un alias, no es necesario que cambie las cadenas de conexión. Solo pueden agregarse nuevos espacios de nombres al emparejamiento de la conmutación por error. 

1. Cree el espacio de nombres principal.
1. Cree el espacio de nombres secundario en una región diferente. Este paso es opcional. Puede crear el espacio de nombres secundario mientras crea el emparejamiento en el paso siguiente. 
1. En Azure Portal, vaya al espacio de nombres principal.
1. Seleccione **Recuperación geográfica** en el menú de la izquierda e **Iniciar el emparejamiento** en la barra de herramientas. 

    :::image type="content" source="./media/event-hubs-geo-dr/primary-namspace-initiate-pairing-button.png" alt-text="Inicio del emparejamiento desde el espacio de nombres principal":::    
1. En la página **Iniciar el emparejamiento**, siga estos pasos:
    1. Seleccione un espacio de nombres secundario existente o cree uno en otra región. En este ejemplo, se ha seleccionado un espacio de nombres existente.  
    1. En **Alias**, escriba un alias para el emparejamiento de recuperación ante desastres con localización geográfica. 
    1. Seleccione **Crear**. 

    :::image type="content" source="./media/event-hubs-geo-dr/initiate-pairing-page.png" alt-text="Selección del espacio de nombres secundario":::        
1. Debería ver la página **Geo-DR Alias** (Alias de recuperación ante desastres geográfica). Para navegar a esta página desde el espacio de nombres principal, también puede seleccionar la opción **Recuperación geográfica** en el menú de la izquierda.

    :::image type="content" source="./media/event-hubs-geo-dr/geo-dr-alias-page.png" alt-text="Página Geo-DR alias (Alias de recuperación ante desastres geográfica)":::    
1. En la página **Geo-DR Alias** (Alias de recuperación ante desastres geográfica), seleccione **Directivas de acceso compartido** para acceder a la cadena de conexión principal del alias. Use esta cadena de conexión en lugar de usar directamente la cadena de conexión al espacio de nombres principal o secundario. 
1. En esta página de **Información general**, puede realizar las siguientes acciones: 
    1. Dividir el emparejamiento entre los espacios de nombres principal y secundario. Seleccione **Interrumpir el emparejamiento** en la barra de herramientas. 
    1. Conmutar por error manualmente al espacio de nombres secundario. Seleccione **Conmutación por error** en la barra de herramientas. 
    
        > [!WARNING]
        > La conmutación por error activará el espacio de nombres secundario y quitará el espacio de nombres principal del emparejamiento de la recuperación ante desastres geográfica. Cree otro espacio de nombres para tener un nuevo par de recuperación ante desastres geográfica. 

Por último, debe agregar alguna supervisión para detectar si es necesario realizar una conmutación por error. En la mayoría de los casos, el servicio forma parte de un ecosistema mayor, por lo que las conmutaciones por error automáticas raramente son posibles, ya que, a menudo, las conmutaciones por error tienen que realizarse en sincronía con el subsistema o infraestructura restantes.

### <a name="example"></a>Ejemplo

En un ejemplo de este escenario, se considera una solución de punto de venta (POS) que emite mensajes o eventos. Event Hubs pasa esos eventos a alguna solución de asignación o formato, que reenvía los datos asignados a otros sistema para continuar el procesamiento. En ese momento, todos estos sistemas se pueden hospedar en la misma región de Azure. La decisión sobre cuándo y en qué parte se realizará la conmutación por error depende del flujo de datos de su infraestructura. 

Puede automatizar la conmutación por error con la supervisión de sistemas, o con soluciones de supervisión personalizadas. Sin embargo, dicha automatización necesita planeamiento y trabajo extra que se encuentran fuera del ámbito de este artículo.

### <a name="failover-flow"></a>Flujo de conmutación por error

Si inicia la conmutación por error, se requieren dos pasos:

1. En caso de otra interrupción, tiene que poder volver a realizar la conmutación por error. Por lo tanto, configure un segundo espacio de nombres pasivo y actualice el emparejamiento. 

2. Extraiga mensajes del anterior espacio de nombres primario una vez que vuelva a estar disponible. Después de eso, utilice ese espacio de nombres para la mensajería regular fuera de la configuración de recuperación con localización geográfica, o elimine el espacio de nombres principal antiguo.

> [!NOTE]
> Se admite solo la semántica de conmutación de reenvío. En este escenario, se realiza la conmutación por error y, a continuación, se vuelve a emparejar con un nuevo espacio de nombres. No se admite la conmutación por recuperación, por ejemplo en un clúster de SQL. 

![2][]

## <a name="management"></a>Administración

Si ha cometido algún error; por ejemplo, ha emparejado regiones incorrectas durante la configuración inicial, puede interrumpir el emparejamiento de los dos espacios de nombres en cualquier momento. Si desea usar los espacios de nombres emparejados como espacios de nombres normales, elimine el alias.

## <a name="samples"></a>Ejemplos

En el [ejemplo de GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) se muestra cómo configurar e iniciar una conmutación por error. En este ejemplo se demuestran los siguientes conceptos:

- La configuración necesaria en Azure Active Directory para usar Azure Resource Manager con Event Hubs. 
- Pasos necesarios para ejecutar el código de ejemplo. 
- Envío y recepción desde el espacio de nombres principal actual. 

## <a name="considerations"></a>Consideraciones

Tenga en cuenta y recuerde las siguientes consideraciones:

1. Por motivos de diseño, la recuperación ante desastres geográfica de Event Hubs no replica datos y, por lo tanto, no se puede volver a usar el valor de desplazamiento anterior del centro de eventos principal en el centro de eventos secundario. Se recomienda reiniciar el receptor de eventos con uno de los siguientes métodos:

   - *EventPosition.FromStart()* : si quiere leer todos los datos en el centro de eventos secundario.
   - *EventPosition.FromEnd()* : si quiere leer todos los datos nuevos desde el momento de conexión con el centro de eventos secundario.
   - *EventPosition.FromEnqueuedTime(dateTime)* : si quiere leer todos los datos recibidos en el centro de eventos secundario a partir de una hora y fecha determinadas.

2. En el planeamiento de la conmutación por error, también debe considerar el factor de tiempo. Por ejemplo, si se pierde la conectividad durante más de 15 a 20 minutos, puede decidir iniciar la conmutación por error. 
 
3. El hecho de que no se repliquen datos significa que las sesiones activas en la actualidad no se han replicado. Además, la detección de duplicados y mensajes programados puede no funcionar. Funcionarán las nuevas sesiones, los mensajes programados y los duplicados nuevos. 

4. Conmutar por error una compleja infraestructura distribuida debe [ensayarse](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) al menos una vez. 

5. La sincronización de entidades puede tardar algún tiempo, aproximadamente 50-100 entidades por minuto.

## <a name="availability-zones"></a>Zonas de disponibilidad 

La SKU de Event Hubs estándar es compatible con [Availability Zones](../availability-zones/az-overview.md), lo que proporciona ubicaciones con aislamiento de errores dentro de una región de Azure. 

> [!NOTE]
> La compatibilidad de Availability Zones con Azure Event Hubs estándar solo está disponible en aquellas [regiones de Azure](../availability-zones/az-region.md) en las que hay zonas de disponibilidad.

Solo puede habilitar Availability Zones en los espacios de nombres nuevos mediante Azure Portal. Event Hubs no admite la migración de espacios de nombres existentes. No se puede deshabilitar la redundancia de zona después de habilitarla en el espacio de nombres.

Al utilizar zonas de disponibilidad, tanto los metadatos como los datos (eventos) se replican entre centros de datos en la zona de disponibilidad. 

![3][]

## <a name="private-endpoints"></a>Puntos de conexión privados
En esta sección se proporcionan más aspectos que hay que tener en cuenta cuando se usa la recuperación ante desastres geográfica con espacios de nombres que emplean puntos de conexión privados. Para obtener información sobre el uso de puntos de conexión privados con Event Hubs en general, consulte [Configuración de puntos de conexión privados](private-link-service.md).

### <a name="new-pairings"></a>Nuevos emparejamientos
Si intenta crear un emparejamiento entre un espacio de nombres primario con un punto de conexión privado y un espacio de nombres secundario sin un punto de conexión privado, se producirá un error. El emparejamiento solo se realizará correctamente si los espacios de nombres primario y secundario tienen puntos de conexión privados. Se recomienda utilizar las mismas configuraciones en los espacios de nombres principal y secundario y en las redes virtuales en las que se creen los puntos de conexión privados.  

> [!NOTE]
> Al intentar emparejar el espacio de nombres principal con un punto de conexión privado y un espacio de nombres secundario, el proceso de validación solo comprueba si existe un punto de conexión privado en el espacio de nombres secundario. No comprueba si el punto de conexión funciona ahora o funcionará después de la conmutación por error. Es su responsabilidad asegurarse de que el espacio de nombres secundario con el punto de conexión privado funcionará según lo esperado después de la conmutación por error.
>
> Para probar que las configuraciones de punto de conexión privado son iguales en los espacios de nombres primario y secundario, envíe una solicitud de lectura (por ejemplo: [Get Event Hub](/rest/api/eventhub/get-event-hub)) al espacio de nombres secundario desde fuera de la red virtual y compruebe que recibe un mensaje de error del servicio.

### <a name="existing-pairings"></a>Emparejamientos existentes
Si ya existe un emparejamiento entre el espacio de nombres primario y el secundario, se producirá un error en la creación del punto de conexión privado en el espacio de nombres primario. Para resolverlo, cree primero un punto de conexión privado en el espacio de nombres secundario y, a continuación, cree uno para el espacio de nombres primario.

> [!NOTE]
> Aunque se permite el acceso de solo lectura al espacio de nombres secundario, no se permiten las actualizaciones de las configuraciones de punto de conexión privado. 

### <a name="recommended-configuration"></a>Configuración recomendada
Al crear una configuración de recuperación ante desastres para la aplicación y los espacios de nombres de Event Hubs, debe crear puntos de conexión privados para los espacios de nombres de Event Hubs primario y secundario en las redes virtuales que hospeden las instancias primarias y secundarias de la aplicación. 

Supongamos que tiene dos redes virtuales, VNET-1 y VNET-2, y estos espacios de nombres primario y secundario: EventHubs-Namespace1-Primary, EventHubs-Namespace2-Secondary. Debe seguir estos pasos: 

- En EventHubs-Namespace1-Primary, cree dos puntos de conexión privados que usen subredes de VNET-1 y VNET-2.
- En EventHubs-Namespace2-Secondary, cree dos puntos de conexión privados que usen las mismas subredes de VNET-1 y VNET-2. 

![Puntos de conexión privados y redes virtuales](./media/event-hubs-geo-dr/private-endpoints-virtual-networks.png)

La ventaja de este enfoque es que la conmutación por error puede producirse en el nivel de aplicación independiente del espacio de nombres de Event Hubs. Considere los casos siguientes: 

**Conmutación por error solo de la aplicación:** En este caso, la aplicación no existirá en VNET-1, sino que se moverá a VNET-2. Como ambos puntos de conexión privados están configurados tanto en VNET-1 como en VNET-2 para los espacios de nombres primario y secundario, la aplicación funcionará. 

**Conmutación por error solo del espacio de nombres de Event Hubs**: en este caso, dado que los dos puntos de conexión privados están configurados en ambas redes virtuales para los espacios de nombres primario y secundario, la aplicación funcionará. 

> [!NOTE]
> Para obtener instrucciones sobre la recuperación ante desastres con localización geográfica de una red virtual, consulte [Virtual Network: continuidad del negocio](../virtual-network/virtual-network-disaster-recovery-guidance.md).
 
## <a name="next-steps"></a>Pasos siguientes

* El [ejemplo en GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/GeoDRClient) le guía a través de un flujo de trabajo simple que crea un emparejamiento geográfico e inicia una conmutación por error para un escenario de recuperación ante desastres.
* La [referencia de la API de REST](/rest/api/eventhub/) describe las API para llevar a cabo la configuración de recuperación de desastres con localización geográfica.

Para obtener más información acerca de Event Hubs, visite los vínculos siguientes:

- Introducción a Event Hubs
    - [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
    - [Java](event-hubs-java-get-started-send.md)
    - [Python](event-hubs-python-get-started-send.md)
    - [JavaScript](event-hubs-node-get-started-send.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
* [Aplicaciones de ejemplo que usan Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
