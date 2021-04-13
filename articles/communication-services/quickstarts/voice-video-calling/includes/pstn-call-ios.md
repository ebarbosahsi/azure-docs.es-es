---
author: nikuklic
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: nikuklic
ms.openlocfilehash: 43e3463a3284f57825073888146b38fa14cbf5d3
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105109050"
---
[!INCLUDE [Emergency Calling Notice](../../../includes/emergency-calling-notice-include.md)]
## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Un recurso implementado de Communication Services. [Cree un recurso de Communication Services](../../create-communication-resource.md).
- Un número de teléfono adquirido en el recurso de Communication Services. [Obtención de un número de teléfono](../../telephony-sms/get-phone-number.md).
- `User Access Token` para habilitar el cliente de llamada. Para más información sobre [cómo obtener `User Access Token`](../../access-tokens.md)
- Complete el inicio rápido para [empezar a agregar llamadas a su aplicación](../getting-started-with-calling.md)

### <a name="prerequisite-check"></a>Comprobación de requisitos previos

- Para ver los números de teléfono asociados a su recurso de Communication Services, inicie sesión en [Azure Portal](https://portal.azure.com/), busque el recurso de Communication Services y abra la pestaña **números de teléfono** en el panel de navegación izquierdo.
- Puede compilar y ejecutar la aplicación con el SDK de llamadas de Azure Communication Services para iOS:

## <a name="setting-up"></a>Instalación

## <a name="start-a-call-to-phone"></a>Inicio de una llamada telefónica

Especifique el número de teléfono que ha adquirido en el recurso de Communication Services y que se usará para iniciar la llamada:
> [!WARNING]
> Tenga en cuenta que los números de teléfono se deben proporcionar en formato estándar internacional E.164. (por ejemplo: +12223334444)

Modifique el controlador de eventos `startCall` que se llevará a cabo cuando se pulsa el botón *Iniciar llamada*:

```swift
func startCall() {
    // Ask permissions
    AVAudioSession.sharedInstance().requestRecordPermission { (granted) in
        if granted {
            let startCallOptions = ACSStartCallOptions()
            startCallOptions!.alternateCallerID = PhoneNumber(phoneNumber: "+12223334444")
            self.call = self.callAgent!.startCall([PhoneNumber(phoneNumber: self.callee)], options: startCallOptions)
            self.callDelegate = CallDelegate(self)
            self.call!.delegate = self.callDelegate
        }
    }
}
```

## <a name="run-the-code"></a>Ejecución del código

Para compilar y ejecutar la aplicación en el simulador de iOS, seleccione **Producto** > **Ejecutar** o use el método abreviado de teclado (&#8984;-R).

![Aspecto final de la aplicación de inicio rápido](../media/ios/quick-start-make-call.png)

Puede realizar una llamada a un teléfono si proporciona un número de teléfono en el campo de texto agregado y hace clic en el botón **Iniciar llamada**.
> [!WARNING]
> Tenga en cuenta que los números de teléfono se deben proporcionar en formato estándar internacional E.164. (por ejemplo: +12223334444)

> [!NOTE]
> La primera vez que realice una llamada, el sistema le solicitará acceso al micrófono. En una aplicación de producción, debe usar la API `AVAudioSession`, [comprobar el estado del permiso](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources) y actualizar correctamente el comportamiento de la aplicación cuando no se conceda el permiso.
