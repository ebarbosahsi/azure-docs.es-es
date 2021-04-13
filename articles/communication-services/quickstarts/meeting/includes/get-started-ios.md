---
title: 'Inicio rápido: Agregar la unión a una reunión de Teams en una aplicación de iOS mediante Azure Communication Services'
description: En este inicio rápido, aprenderá a usar la biblioteca de integración de Teams de Azure Communication Services para iOS.
author: palatter
ms.author: palatter
ms.date: 01/25/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 4d28864d41d6540afc87126daf589ed2929f891d
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104803146"
---
En este inicio rápido, aprenderá a unirse a una reunión de Teams mediante la biblioteca de integración de Teams de Azure Communication Services para iOS.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este inicio rápido, debe cumplir los siguientes requisitos previos:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Mac con [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532), junto con un certificado de desarrollador válido instalado en el Llavero.
- Un recurso de Communication Services implementado. [Cree un recurso de Communication Services](../../create-communication-resource.md).
- Un [token de acceso de usuario](../../access-tokens.md) para su instancia de Azure Communication Services.
- Tener instalado [CocoaPods](https://cocoapods.org/) para el entorno de compilación.

## <a name="setting-up"></a>Instalación

### <a name="creating-the-xcode-project"></a>Creación del proyecto de Xcode

En Xcode, cree un proyecto de iOS y seleccione la plantilla **App** (Aplicación). Usaremos guiones gráficos de UIKit. Durante este inicio rápido, no se van a crear pruebas. No dude en desactivar **Incluir pruebas**.

:::image type="content" source="../media/ios/xcode-new-project-template-select.png" alt-text="Captura de pantalla que muestra la selección de la plantilla New Project (Nuevo proyecto) en Xcode.":::

Dé un nombre al proyecto `TeamsEmbedGettingStarted`.

:::image type="content" source="../media/ios/xcode-new-project-details.png" alt-text="Captura de pantalla que muestra los detalles del nuevo proyecto en Xcode.":::

### <a name="install-the-package-and-dependencies-with-cocoapods"></a>Instalación del paquete y las dependencias con CocoaPods

1. Cree un Podfile para la aplicación:

```
platform :ios, '12.0'
use_frameworks!

target 'TeamsEmbedGettingStarted' do
    pod 'AzureCommunication', '~> 1.0.0-beta.8'
end

azure_libs = [
'AzureCommunication',
'AzureCore']

post_install do |installer|
    installer.pods_project.targets.each do |target|
    if azure_libs.include?(target.name)
        puts "Adding BUILD_LIBRARY_FOR_DISTRIBUTION to #{target.name}"
        target.build_configurations.each do |config|
        xcconfig_path = config.base_configuration_reference.real_path
        File.open(xcconfig_path, "a") {|file| file.puts "BUILD_LIBRARY_FOR_DISTRIBUTION = YES"}
        end
    end
    end
end
```

2. Ejecute `pod install`.
3. Abra el archivo `.xcworkspace` generado con Xcode.

### <a name="request-access-to-the-microphone-camera-etc"></a>Solicite acceso al micrófono, la cámara, etc.

Para acceder al hardware del dispositivo, actualice la lista de propiedades de información de la aplicación. Establezca el valor asociado al elemento `string` que se incluirá en el cuadro de diálogo que el sistema usa para solicitar al usuario acceso.

Haga clic con el botón derecho en la entrada `Info.plist` del árbol del proyecto y seleccione **Abrir como** > **Código fuente**. Agregue las líneas siguientes a la sección `<dict>` de nivel superior y guarde el archivo.

```xml
<key>NSBluetoothAlwaysUsageDescription</key>
<string></string>
<key>NSBluetoothPeripheralUsageDescription</key>
<string></string>
<key>NSCameraUsageDescription</key>
<string></string>
<key>NSContactsUsageDescription</key>
<string></string>
<key>NSMicrophoneUsageDescription</key>
<string></string>
```

### <a name="add-the-teams-embed-framework"></a>Adición del marco de integración de Teams

1. Descargue el marco.
2. Cree una carpeta `Frameworks` en la raíz del proyecto. Por ejemplo, `\TeamsEmbedGettingStarted\Frameworks\`
3. Copie los marcos descargados `TeamsAppSDK.framework` y `MeetingUIClient.framework` en esta carpeta.
4. Agregue `TeamsAppSDK.framework` y `MeetingUIClient.framework` al destino del proyecto en la pestaña general. Use `Add Other` ->  `Add Files...` para desplazarse a los archivos del marco y agregarlos.

:::image type="content" source="../media/ios/xcode-add-frameworks.png" alt-text="Captura de pantalla que muestra los marcos agregados en Xcode.":::

5. Si todavía no está, agregue `$(PROJECT_DIR)/Frameworks` a `Framework Search Paths` en la pestaña de configuración de la compilación de destino del proyecto. Para encontrar la configuración, debe cambiar el filtro de `basic` a `all`; también puede usar la barra de búsqueda de la derecha.

:::image type="content" source="../media/ios/xcode-add-framework-search-path.png" alt-text="Captura de pantalla que muestra la ruta de búsqueda del marco en Xcode.":::

### <a name="turn-off-bitcode"></a>Desactivación de Bitcode

Establezca la opción `Enable Bitcode` en `No` en la configuración de la compilación del proyecto. Para encontrar la configuración, debe cambiar el filtro de `basic` a `all`; también puede usar la barra de búsqueda de la derecha.

:::image type="content" source="../media/ios/xcode-bitcode-option.png" alt-text="Captura de pantalla que muestra la opción Bitcode en Xcode.":::

### <a name="add-framework-signing-script"></a>Adición del script de firma del marco

Seleccione el destino de la aplicación y elija la pestaña `Build Phases`. A continuación, haga clic en `+`, seguido de `New Run Script Phase` . Asegúrese de que esta nueva fase se produce después de las fases `Embed Frameworks`.



:::image type="content" source="../media/ios/xcode-build-script.png" alt-text="Captura de pantalla que muestra cómo agregar el script de compilación en Xcode.":::

```bash
#!/bin/sh
if [ -d "${TARGET_BUILD_DIR}"/"${PRODUCT_NAME}".app/Frameworks/TeamsAppSDK.framework/Frameworks ]; then
    pushd "${TARGET_BUILD_DIR}"/"${PRODUCT_NAME}".app/Frameworks/TeamsAppSDK.framework/Frameworks
    for EACH in *.framework; do
        echo "-- signing ${EACH}"
        /usr/bin/codesign --force --deep --sign "${EXPANDED_CODE_SIGN_IDENTITY}" --entitlements "${TARGET_TEMP_DIR}/${PRODUCT_NAME}.app.xcent" --timestamp=none $EACH
        echo "-- moving ${EACH}"
        mv -nv ${EACH} ../../
    done
    rm -rf "${TARGET_BUILD_DIR}"/"${PRODUCT_NAME}".app/Frameworks/TeamsAppSDK.framework/Frameworks
    popd
    echo "BUILD DIR ${TARGET_BUILD_DIR}"
fi
```

### <a name="turn-on-voice-over-ip-background-mode"></a>Active el modo en segundo plano de voz sobre IP.

Seleccione el destino de la aplicación y haga clic en la pestaña Capabilities (Funcionalidades).

Para activar `Background Modes`, haga clic en `+ Capabilities` en la parte superior y seleccione `Background Modes`.

Active la casilla `Voice over IP`.

:::image type="content" source="../media/ios/xcode-background-voip.png" alt-text="Captura de pantalla que muestra VOIP habilitado en Xcode.":::

### <a name="add-a-window-reference-to-appdelegate"></a>Adición de una referencia de ventana a AppDelegate

Abra el archivo **AppDelegate.swift** del proyecto y agregue una referencia para "window" (ventana).

```swift
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
```

### <a name="add-a-button-to-the-viewcontroller"></a>Adición de un botón a ViewController

Cree un botón en la devolución de llamada `viewDidLoad` en **ViewController.swift**.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    let button = UIButton(frame: CGRect(x: 100, y: 100, width: 200, height: 50))
    button.backgroundColor = .black
    button.setTitle("Join Meeting", for: .normal)
    button.addTarget(self, action: #selector(joinMeetingTapped), for: .touchUpInside)
    
    button.translatesAutoresizingMaskIntoConstraints = false
    self.view.addSubview(button)
    button.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    button.centerYAnchor.constraint(equalTo: view.centerYAnchor).isActive = true
```

Cree una salida para el botón en **ViewController.swift**.

```swift
@IBAction func joinMeetingTapped(_ sender: UIButton) {

}
```

### <a name="set-up-the-app-framework"></a>Instalación del marco de la aplicación

Abra el archivo **ViewController.swift** del proyecto y agregue una declaración `import` en la parte superior del archivo para importar los elementos `AzureCommunication library` y `MeetingUIClient`. 

```swift
import UIKit
import AzureCommunication
import MeetingUIClient
```

Reemplace la implementación de la clase `ViewController` por un sencillo botón para permitir al usuario unirse a una reunión. En este inicio rápido, asociaremos la lógica de negocios al botón.

```swift
class ViewController: UIViewController {

    private var meetingUIClient: MeetingUIClient?
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Initialize meetingClient
    }

    @IBAction func joinMeetingTapped(_ sender: UIButton) {
        joinMeeting()
    }
    
    private func joinMeeting() {
        // Add join meeting logic
    }
}
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales de la biblioteca de integración de Teams de Azure Communication Services:

| Nombre                                  | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| MeetingUIClient | MeetingUIClient es el punto de entrada principal a la biblioteca de integración de Teams. |
| MeetingUIClientDelegate | MeetingUIClientDelegate se usa para recibir eventos, como cambios en el estado de la llamada. |
| MeetingJoinOptions | MeetingJoinOptions se usan para las opciones configurables, como el nombre para mostrar. | 
| CallState | CallState se utiliza para notificar los cambios en el estado de la llamada. Las opciones son: connecting, waitingInLobby, connected y ended. |

## <a name="create-and-authenticate-the-client"></a>Creación y autenticación del cliente

Inicialice una instancia de `MeetingUIClient` con un token de acceso de usuario que permita la unión a reuniones. Agregue el código siguiente a la devolución de llamada `viewDidLoad` en **ViewController.swift**:

```swift
do {
    let communicationTokenRefreshOptions = CommunicationTokenRefreshOptions(initialToken: "<USER_ACCESS_TOKEN>", refreshProactively: true, tokenRefresher: fetchTokenAsync(completionHandler:))
    let credential = try CommunicationTokenCredential(with: communicationTokenRefreshOptions)
    meetingUIClient = MeetingUIClient(with: credential)
}
catch {
    print("Failed to create communication token credential")
}
```

Reemplace `<USER_ACCESS_TOKEN>` por un token de acceso de usuario válido para el recurso. Consulte la documentación relativa al [token de acceso de usuario](../../access-tokens.md) si aún no tiene ningún token disponible.

### <a name="setup-token-refreshing"></a>Actualización del token de instalación

Cree un método `fetchTokenAsync`; Después, agregue la lógica `fetchToken` para obtener el token de usuario.

```swift
private func fetchTokenAsync(completionHandler: @escaping TokenRefreshOnCompletion) {
    func getTokenFromServer(completionHandler: @escaping (String) -> Void) {
        completionHandler("<USER_ACCESS_TOKEN>")
    }
    getTokenFromServer { newToken in
        completionHandler(newToken, nil)
    }
}
```

Reemplace `<USER_ACCESS_TOKEN>` por un token de acceso de usuario válido para el recurso.

## <a name="join-a-meeting"></a>Unirse a una reunión

El método `joinMeeting` se establece como la acción que se llevará a cabo cuando se pulse el botón *Unirse a la reunión*. Actualice la implementación para unirse a una reunión con `MeetingUIClient`:

```swift
private func joinMeeting() {
    let meetingJoinOptions = MeetingJoinOptions(displayName: "John Smith")
        
    meetingUIClient?.join(meetingUrl: "<MEETING_URL>", meetingJoinOptions: meetingJoinOptions, completionHandler: { (error: Error?) in
        if (error != nil) {
            print("Join meeting failed: \(error!)")
        }
    })
}
```

Reemplace `<MEETING URL>` por un vínculo a la reunión de Teams.

### <a name="get-a-teams-meeting-link"></a>Obtención de un vínculo a la reunión de Teams

El vínculo a la reunión de Teams se puede recuperar mediante instancias de Graph API. Esto se detalla en la [documentación de Graph](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta&preserve-view=true).
El SDK de llamada de Communication Services acepta un vínculo a toda la reunión de Teams. Este vínculo se devuelve como parte del recurso `onlineMeeting`, al que se accede bajo la propiedad [`joinWebUrl`](/graph/api/resources/onlinemeeting?view=graph-rest-beta&preserve-view=true). También puede obtener la información necesaria de la reunión de la dirección URL **Unirse a la reunión** de la propia invitación a la reunión de Teams.

## <a name="run-the-code"></a>Ejecución del código

Para compilar y ejecutar la aplicación en el simulador de iOS, seleccione **Producto** > **Ejecutar** o use el método abreviado de teclado (&#8984;-R).

:::image type="content" source="../media/ios/quick-start-join-meeting.png" alt-text="Aspecto final de la aplicación de inicio rápido":::

:::image type="content" source="../media/ios/quick-start-meeting-joined.png" alt-text="Apariencia final cuando se ha unido la reunión":::

> [!NOTE]
> La primera vez que se una a una reunión, el sistema le solicitará acceso al micrófono. En una aplicación de producción, debe usar la API `AVAudioSession` para [comprobar el estado del permiso](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources) y actualizar correctamente el comportamiento de la aplicación cuando no se conceda el permiso.

## <a name="add-localization-support-based-on-your-app"></a>Adición de compatibilidad con la localización en función de la aplicación

El SDK de Microsoft Teams admite más de 100 cadenas y recursos. El paquete del marco contiene los idiomas base e inglés. El resto se incluyen en el archivo `Localizations.zip` que viene con el paquete.

#### <a name="add-localizations-to-the-sdk-based-on-what-your-app-supports"></a>Agregue localizaciones al SDK en función de lo que admita la aplicación.

1. Determine el tipo de localizaciones que admite la aplicación. Para ello, vaya al proyecto de Xcode de la aplicación, seleccione la pestaña de información y consulte la lista de localizaciones.
2. Descomprima el archivo Localizations.zip incluido con el paquete.
3. Copie las carpetas de localización de la carpeta descomprimida en función de lo que admita la aplicación en la raíz de TeamsAppSDK.framework.

## <a name="preparation-for-app-store-upload"></a>Preparación de la carga de App Store

Quite las arquitecturas i386 y x86_64 de los marcos en caso de que se archiven.

Agregue las arquitecturas `i386` y `x86_64` a las fases de compilación y quite el script antes de la fase de firma de código del marco si quiere archivar la aplicación.

En el explorador de proyectos, seleccione el proyecto. En el panel del editor, vaya a Build Phases (Fases de compilación), haga clic en + sign (+ firmar) y seleccione Create a New Run Script Phase (Crear una fase de script de ejecución).

```bash
echo "Target architectures: $ARCHS"
APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"
find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
do
FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"
echo $(lipo -info "$FRAMEWORK_EXECUTABLE_PATH")
FRAMEWORK_TMP_PATH="$FRAMEWORK_EXECUTABLE_PATH-tmp"
# remove simulator's archs if location is not simulator's directory
case "${TARGET_BUILD_DIR}" in
*"iphonesimulator")
    echo "No need to remove archs"
    ;;
*)
    if $(lipo "$FRAMEWORK_EXECUTABLE_PATH" -verify_arch "i386") ; then
    lipo -output "$FRAMEWORK_TMP_PATH" -remove "i386" "$FRAMEWORK_EXECUTABLE_PATH"
    echo "i386 architecture removed"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_TMP_PATH" "$FRAMEWORK_EXECUTABLE_PATH"
    fi
    if $(lipo "$FRAMEWORK_EXECUTABLE_PATH" -verify_arch "x86_64") ; then
    lipo -output "$FRAMEWORK_TMP_PATH" -remove "x86_64" "$FRAMEWORK_EXECUTABLE_PATH"
    echo "x86_64 architecture removed"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_TMP_PATH" "$FRAMEWORK_EXECUTABLE_PATH"
    fi
    ;;
esac
echo "Completed for executable $FRAMEWORK_EXECUTABLE_PATH"
echo $(lipo -info "$FRAMEWORK_EXECUTABLE_PATH")
done
```

## <a name="sample-code"></a>Código de ejemplo

Puede descargar la aplicación de ejemplo de [GitHub](https://github.com/Azure-Samples/teams-embed-ios-getting-started).
