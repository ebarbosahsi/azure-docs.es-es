---
title: Inicio rápido de la biblioteca cliente de Swift para el Lector inmersivo
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, va a crear una aplicación iOS desde cero y a agregar la funcionalidad de la API de Lector inmersivo.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: include
ms.date: 09/14/2020
ms.author: nitinme
ms.openlocfilehash: 5dc4e38eb0e29cc9fa272f6e740fcc7d1dbfe44a
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102619255"
---
[Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de manera inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, estudiantes de idiomas y personas con dificultades de aprendizaje, como la dislexia. Puede usar Lector inmersivo en sus aplicaciones para aislar el texto con el fin de mejorar la concentración, mostrar imágenes para palabras de uso frecuente, resaltar partes del texto, leer texto seleccionado en voz alta, traducir palabras y frases en tiempo real y mucho más.

En este inicio rápido, creará una aplicación iOS desde cero e integrará el Lector inmersivo. Se puede encontrar un completo ejemplo funcional [aquí](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/ios).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services)
* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)
* Un recurso del Lector inmersivo configurado para la autenticación de Azure Active Directory. Siga [estas instrucciones](../../how-to-create-immersive-reader.md) para realizar la configuración. Al configurar las propiedades del proyecto de ejemplo, necesitará algunos de los valores creados aquí. Guarde la salida de la sesión en un archivo de texto para futuras referencias.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Cree un proyecto nuevo en Xcode.

![Nuevo proyecto: Swift](../../media/ios/xcode-create-project.png)

Elija **Single View App** (Aplicación de vista única).

![Nueva aplicación de vista única: Swift](../../media/ios/xcode-single-view-app.png)

## <a name="set-up-authentication"></a>Configuración de la autenticación

En el menú superior, haga clic en **Product (Producto) > Scheme (Esquema) > Edit Scheme...(Editar esquema)** .

![Editar esquema: Swift](../../media/ios/quickstart-ios-edit-scheme.png)

En la vista **Run** (Ejecutar), haga clic en la pestaña **Arguments** (Argumentos).

![Editar variables de entorno de esquema: Swift](../../media/ios/quickstart-ios-env-vars.png)

En la sección **Environment Variables** (Variables de entorno), agregue los siguientes nombres y valores, y proporcione los valores especificados al crear el recurso del Lector inmersivo.

```text
TENANT_ID=<YOUR_TENANT_ID>
CLIENT_ID=<YOUR_CLIENT_ID>
CLIENT_SECRET<YOUR_CLIENT_SECRET>
SUBDOMAIN=<YOUR_SUBDOMAIN>
```

## <a name="set-up-the-app-to-run-without-a-storyboard"></a>Configuración de la aplicación para que se ejecute sin un guion gráfico

Abra *AppDelegate.swift* y reemplace el archivo por el siguiente código.

```swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?

    var navigationController: UINavigationController?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.

        window = UIWindow(frame: UIScreen.main.bounds)

        if let window = window {
            let mainViewController = LaunchViewController()
            navigationController = UINavigationController(rootViewController: mainViewController)
            window.rootViewController = navigationController
            window.makeKeyAndVisible()
        }
        return true
    }

    func applicationWillResignActive(_ application: UIApplication) {
        // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
        // Use this method to pause ongoing tasks, disable timers, and invalidate graphics rendering callbacks. Games should use this method to pause the game.
    }

    func applicationDidEnterBackground(_ application: UIApplication) {
        // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
        // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
    }

    func applicationWillEnterForeground(_ application: UIApplication) {
        // Called as part of the transition from the background to the active state; here you can undo many of the changes made on entering the background.
    }

    func applicationDidBecomeActive(_ application: UIApplication) {
        // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
    }

    func applicationWillTerminate(_ application: UIApplication) {
        // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
    }
}
```

## <a name="create-the-view-controllers-and-add-sample-content"></a>Creación de los controladores de vista e incorporación de contenido de ejemplo

Cambie el nombre de *ViewController.swift* a *LaunchViewController.swift* y reemplace el archivo por el código siguiente.

```swift
import UIKit

class LaunchViewController: UIViewController {
    private var tenantId = ProcessInfo.processInfo.environment["TENANT_ID"]
    private var clientId = ProcessInfo.processInfo.environment["CLIENT_ID"]
    private var clientSecret = ProcessInfo.processInfo.environment["CLIENT_SECRET"]
    private var subdomain = ProcessInfo.processInfo.environment["SUBDOMAIN"]

    private var launchButton: UIButton!
    private var titleText: UILabel!
    private var bodyText: UILabel!
    private var sampleContent: Content!
    private var sampleChunk: Chunk!
    private var sampleOptions: Options!

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .white

        titleText = UILabel()
        titleText.text = "Geography"
        titleText.font = UIFont.boldSystemFont(ofSize: 30)
        titleText.lineBreakMode = .byWordWrapping
        titleText.numberOfLines = 0
        view.addSubview(titleText)

        bodyText = UILabel()
        bodyText.text = "The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers. Landforms are sometimes called physical features. It is important for students to know about the physical geography of Earth. The seasons, the atmosphere and all the natural processes of Earth affect where people are able to live. Geography is one of a combination of factors that people use to decide where they want to live.The physical features of a region are often rich in resources. Within a nation, mountain ranges become natural borders for settlement areas. In the U.S., major mountain ranges are the Sierra Nevada, the Rocky Mountains, and the Appalachians.Fresh water sources also influence where people settle. People need water to drink. They also need it for washing. Throughout history, people have settled near fresh water. Living near a water source helps ensure that people have the water they need. There was an added bonus, too. Water could be used as a travel route for people and goods. Many Americans live near popular water sources, such as the Mississippi River, the Colorado River and the Great Lakes.Mountains and deserts have been settled by fewer people than the plains areas. However, they have valuable resources of their own."
        bodyText.lineBreakMode = .byWordWrapping
        bodyText.numberOfLines = 0
        let screenSize = self.view.frame.height
        if screenSize <= 667 {
            // Font size for smaller iPhones.
            bodyText.font = bodyText.font.withSize(14)

         } else if screenSize <= 812 {
            // Font size for medium iPhones.
            bodyText.font = bodyText.font.withSize(15)

         } else if screenSize <= 896 {
            // Font size for larger iPhones.
            bodyText.font = bodyText.font.withSize(17)

         } else if screenSize <= 1024 {
            // Font size for iPads.
            bodyText.font = bodyText.font.withSize(25)
        } else {
            // Font size for large iPads.
            bodyText.font = bodyText.font.withSize(28)
        }
        view.addSubview(bodyText)

        launchButton = UIButton()
        launchButton.backgroundColor = .darkGray
        launchButton.contentEdgeInsets = UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
        launchButton.setTitleColor(.white, for: .normal)
        launchButton.setTitle("Immersive Reader", for: .normal)
        launchButton.addTarget(self, action: #selector(launchImmersiveReaderButton(sender:)), for: .touchUpInside)
        view.addSubview(launchButton)

        let layoutGuide = view.safeAreaLayoutGuide

        titleText.translatesAutoresizingMaskIntoConstraints = false
        titleText.topAnchor.constraint(equalTo: layoutGuide.topAnchor, constant: 20).isActive = true
        titleText.leadingAnchor.constraint(equalTo: layoutGuide.leadingAnchor, constant: 20).isActive = true
        titleText.trailingAnchor.constraint(equalTo: layoutGuide.trailingAnchor, constant: -20).isActive = true

        bodyText.translatesAutoresizingMaskIntoConstraints = false
        bodyText.topAnchor.constraint(equalTo: titleText.bottomAnchor, constant: 15).isActive = true
        bodyText.leadingAnchor.constraint(equalTo: layoutGuide.leadingAnchor, constant: 20).isActive = true
        bodyText.trailingAnchor.constraint(equalTo: layoutGuide.trailingAnchor, constant: -20).isActive = true

        launchButton.translatesAutoresizingMaskIntoConstraints = false
        launchButton.widthAnchor.constraint(equalToConstant: 200).isActive = true
        launchButton.heightAnchor.constraint(equalToConstant: 50).isActive = true
        launchButton.centerXAnchor.constraint(equalTo: layoutGuide.centerXAnchor).isActive = true
        launchButton.bottomAnchor.constraint(equalTo: layoutGuide.bottomAnchor, constant: -10).isActive = true

        // Create content and options.
        sampleChunk = Chunk(content: bodyText.text!, lang: nil, mimeType: nil)
        sampleContent = Content(title: titleText.text!, chunks: [sampleChunk])
        sampleOptions = Options(uiLang: nil, timeout: nil, uiZIndex: nil)
    }

    @IBAction func launchImmersiveReaderButton(sender: AnyObject) {
        launchButton.isEnabled = false

        // Callback to get token.
        getToken(onSuccess: {cognitiveToken in
            DispatchQueue.main.async {
                launchImmersiveReader(navController: self.navigationController!, token: cognitiveToken, subdomain: self.subdomain!, content: self.sampleContent, options: self.sampleOptions, onSuccess: {
                    self.launchButton.isEnabled = true
                }, onFailure: { error in
                    self.launchButton.isEnabled = true
                })
            }
        }, onFailure: { error in
            print("an error occured: \(error)")
        })
    }

    func getToken(onSuccess: @escaping (_ theToken: String) -> Void, onFailure: @escaping ( _ theError: String) -> Void) {
        let tokenForm = "grant_type=client_credentials&resource=https://cognitiveservices.azure.com/&client_id=" + self.clientId! + "&client_secret=" + self.clientSecret!
        let tokenUrl = "https://login.windows.net/" + self.tenantId! + "/oauth2/token"

        var responseTokenString: String = "0"

        let url = URL(string: tokenUrl)!
        var request = URLRequest(url: url)
        request.httpBody = tokenForm.data(using: .utf8)
        request.httpMethod = "POST"

        let task = URLSession.shared.dataTask(with: request) { data, response, error in
            guard let data = data,
                let response = response as? HTTPURLResponse,
                error == nil else {
                    onFailure("Error")
                    return
                }

            guard (200 ... 299) ~= response.statusCode else {
                onFailure(String(response.statusCode))
                return
            }

            let responseString = String(data: data, encoding: .utf8)

            let jsonResponse = try? JSONSerialization.jsonObject(with: data, options: [])
            guard let jsonDictonary = jsonResponse as? [String: Any] else {
                onFailure("Error parsing JSON response.")
                return
            }
            guard let responseToken = jsonDictonary["access_token"] as? String else {
                onFailure("Error retrieving token from JSON response.")
                return
            }
            responseTokenString = responseToken
            onSuccess(responseTokenString)
        }

        task.resume()
    }
}
```

Agregue un nuevo archivo a la carpeta raíz del proyecto denominado *ImmersiveReaderViewController.swift* y agregue el código siguiente.

```swift
import UIKit
import Foundation
import WebKit

@available(iOS 11.0, *)
public class ImmersiveReaderWebView: WKWebView {

    init(frame: CGRect, contentController: WKUserContentController) {
        let conf = WKWebViewConfiguration()
        conf.userContentController = contentController
        super.init(frame: frame, configuration: conf)
    }

    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

public class ImmersiveReaderViewController: UIViewController, WKUIDelegate, WKNavigationDelegate {
    let tokenToSend: String
    let subdomainToSend: String
    let contentToSend: Content
    let optionsToSend: Options?
    let onSuccessImmersiveReader: (() -> Void)?
    let onFailureImmersiveReader: ((_ error: Error) -> Void)?
    let onTimeout: ((_ timeoutValue: TimeInterval) -> Void)?
    let onError: ((_ error: String) -> Void)?

    let startTime = Date().timeIntervalSince1970*1000
    var src: String
    var webView: WKWebView!
    var timer: Timer!
    var timeoutValue: TimeInterval!

    public init(tokenToPass: String, subdomainToPass: String, contentToPass: Content, optionsToPass: Options?, onSuccessImmersiveReader: @escaping () -> Void, onFailureImmersiveReader: @escaping (_ status: Error) -> Void, onTimeout: @escaping (_ timeoutValue: TimeInterval) -> Void, onError: @escaping (_ error: String) -> Void) {
        self.tokenToSend = tokenToPass
        self.subdomainToSend = subdomainToPass
        self.contentToSend = contentToPass
        self.optionsToSend = optionsToPass
        self.onSuccessImmersiveReader = onSuccessImmersiveReader
        self.onFailureImmersiveReader = onFailureImmersiveReader
        self.onTimeout = onTimeout
        self.onError = onError
        self.src = "https://" + subdomainToPass + ".cognitiveservices.azure.com/immersivereader/webapp/v1.0/reader"
        super.init(nibName: nil, bundle: nil)
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    override public func viewDidLoad() {
        super.viewDidLoad()

        // If uiLang options are set update src to reflect this.
        switch optionsToSend?.uiLang {
        case .none: break
        case .some(let value):
            src = src + "?omkt=" + value
        }

        // Set timeout to default or value user specifies.
        switch optionsToSend?.timeout {
        case .none:
            timeoutValue = 15
        case .some(let value):
            timeoutValue = value
        }

        view.backgroundColor = .white
        webView = WKWebView()

        let contentController = WKUserContentController()
        if #available(iOS 11.0, *) {
            webView = ImmersiveReaderWebView(frame: .zero, contentController: contentController)
        } else {
            // Fallback on earlier versions
            webView = WKWebView()
            let config = WKWebViewConfiguration()
            config.userContentController = contentController
            webView = WKWebView(frame: .zero, configuration: config)
        }
        webView.navigationDelegate = self
        webView.uiDelegate = self

        contentController.add(self, name: "readyForContent")
        contentController.add(self, name: "launchSuccessful")
        contentController.add(self, name: "tokenExpired")
        contentController.add(self, name: "throttled")

        view.addSubview(webView)
        webView.translatesAutoresizingMaskIntoConstraints = false

        if #available(iOS 11.0, *) {
            let layoutGuide = view.safeAreaLayoutGuide
            webView.leadingAnchor.constraint(equalTo: layoutGuide.leadingAnchor).isActive = true
            webView.trailingAnchor.constraint(equalTo: layoutGuide.trailingAnchor).isActive = true
            webView.topAnchor.constraint(equalTo: layoutGuide.topAnchor).isActive = true
            webView.bottomAnchor.constraint(equalTo: layoutGuide.bottomAnchor).isActive = true

        } else {
            webView.leadingAnchor.constraint(equalTo: view.leadingAnchor).isActive = true
            webView.trailingAnchor.constraint(equalTo: view.trailingAnchor).isActive = true
            webView.topAnchor.constraint(equalTo: view.topAnchor).isActive = true
            webView.bottomAnchor.constraint(equalTo: view.bottomAnchor).isActive = true
        }
        // Get path to JavaScript file.
        guard let scriptPath = Bundle.main.path(forResource: "iFrameMessaging", ofType: "js") else {
            onError!("Could not create script path from resource.")
            return
        }
        do {
            let scriptSource = try String(contentsOfFile: scriptPath)
            let userScript = WKUserScript(source: scriptSource, injectionTime: .atDocumentStart, forMainFrameOnly: true)
            contentController.addUserScript(userScript)
        } catch {
            onError!("Could not parse JavaScript file.")
            return
        }

        // Start the timer.
        timer = Timer.scheduledTimer(timeInterval: timeoutValue, target: self, selector: #selector(self.timedOut), userInfo: nil, repeats: false)

        // Load the iframe from HTML.
        webView.loadHTMLString("<!DOCTYPE html><html style='width: 100%; height: 100%; margin: 0; padding: 0;'><head><meta name='viewport' content='width=device-width, initial-scale=1, shrink-to-fit=no'></head><body style='width: 100%; height: 100%; margin: 0; padding: 0;'><iframe id='immersiveReaderIframe' src = '\(src)' width='100%' height='100%' style='border: 0'></iframe></body></html>", baseURL: URL(string: "test://learningtools.onenote.com/learningtoolsapp/cognitive/reader"))
    }

    @objc func timedOut(_ timer: AnyObject) {
        onTimeout!(timeoutValue)
    }

    public func webView(_ webView: WKWebView, decidePolicyFor navigationAction: WKNavigationAction, decisionHandler: @escaping (WKNavigationActionPolicy) -> Void) {
        decisionHandler(.allow)
    }

    public func webView(_ webView: WKWebView, decidePolicyFor navigationResponse: WKNavigationResponse, decisionHandler: @escaping (WKNavigationResponsePolicy) -> Void ) {
        decisionHandler(.allow)
    }
}

extension ImmersiveReaderViewController: WKScriptMessageHandler {
    public func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {
        if message.name == "readyForContent" {
            // Stop the timer.
            timer.invalidate()

            // Create the message variable
            let message = Message(cogSvcsAccessToken: tokenToSend, cogSvcsSubdomain: subdomainToSend, resourceName: nil, request: contentToSend, launchToPostMessageSentDurationInMs: Int(Date().timeIntervalSince1970*1000 - startTime))
            do {
                let jsonData = try JSONEncoder().encode(message)
                let jsonString = String(data: jsonData, encoding: .utf8)
                self.webView.evaluateJavaScript("sendContentToReader(\(jsonString!))") { (result, error) in
                    if error != nil {
                        self.onError!("Error evaluating JavaScript \(String(describing: error))")
                    }
                }
            } catch { print(error)}
        }

        if message.name == "launchSuccessful" {
            onSuccessImmersiveReader!()
        }

        if message.name == "tokenExpired" {
            let tokenExpiredError = Error(code: "TokenExpired", message: "The access token supplied is expired.")
            onFailureImmersiveReader!(tokenExpiredError)
        }

        if message.name == "throttled" {
            let throttledError = Error(code: "Throttled", message: "You have exceeded the call rate limit.")
            onFailureImmersiveReader!(throttledError)
        }
    }
}
```

Agregue un nuevo archivo a la carpeta raíz del proyecto denominado *LaunchImmersiveReader.swift* y agregue el código siguiente.

```swift
import UIKit
import Foundation

var navigationController: UINavigationController?

public struct Content: Encodable {
    var title: String
    var chunks: [Chunk]

    public init(title: String, chunks: [Chunk]) {
        self.title = title
        self.chunks = chunks
    }
}

public struct Chunk: Encodable {
    var content: String
    var lang: String?
    var mimeType: String?

    public init(content: String, lang: String?, mimeType: String?) {
        self.content = content
        self.lang = lang
        self.mimeType = mimeType
    }
}

public struct Options {
    var uiLang: String?
    var timeout: TimeInterval?

    public init(uiLang: String?, timeout: TimeInterval?, uiZIndex: NSNumber?) {
        self.uiLang = uiLang
        self.timeout = timeout
    }
}

public struct Error {
    public var code: String
    public var message: String

    public init(code: String, message: String) {
        self.code = code
        self.message = message
    }
}

struct Message: Encodable {
    let cogSvcsAccessToken: String
    let cogSvcsSubdomain: String
    let resourceName: String?
    let request: Content
    let launchToPostMessageSentDurationInMs: Int

    init(cogSvcsAccessToken: String, cogSvcsSubdomain: String, resourceName: String?, request: Content, launchToPostMessageSentDurationInMs: Int) {
        self.cogSvcsAccessToken = cogSvcsAccessToken
        self.cogSvcsSubdomain = cogSvcsSubdomain
        self.resourceName = resourceName
        self.request = request
        self.launchToPostMessageSentDurationInMs = launchToPostMessageSentDurationInMs
    }
}

public func launchImmersiveReader(navController: UINavigationController, token: String, subdomain: String, content: Content, options: Options?, onSuccess: @escaping () -> Void, onFailure: @escaping (_ error: Error) -> Void) {
    if (content.chunks.count == 0) {
        let badArgumentError = Error(code: "BadArgument", message: "Chunks must not be empty.")
        onFailure(badArgumentError)
    }

    navigationController = navController
    let immersiveReaderViewController = ImmersiveReaderViewController(tokenToPass: token, subdomainToPass: subdomain, contentToPass: content, optionsToPass: options, onSuccessImmersiveReader: {
        onSuccess()
    }, onFailureImmersiveReader: { error in
        onFailure(error)
    }, onTimeout: { timeout in
        navigationController?.popViewController(animated: true)
        let timeoutError = Error(code: "Timeout", message: "Page failed to load after timeout \(timeout) ms.")
        onFailure(timeoutError)
    }, onError: { error in
        navigationController?.popViewController(animated: true)
        let errorMessage = Error(code: "Internal Error", message: error)
        onFailure(errorMessage)
    })
    navigationController!.pushViewController(immersiveReaderViewController, animated: true)
}
```

Agregue un archivo a la carpeta *Resources* (Recursos) denominado *iFrameMessaging.js* y agregue el código siguiente.

```javascript
window.addEventListener("message", function(message) {
    if(message.data == "ImmersiveReader-ReadyForContent") {
        window.webkit.messageHandlers.readyForContent.postMessage(null);
    }

    if(message.data == "ImmersiveReader-LaunchSuccessful") {
        window.webkit.messageHandlers.launchSuccessful.postMessage(null);
    }

    if(message.data == "ImmersiveReader-TokenExpired") {
        window.webkit.messageHandlers.tokenExpired.postMessage(null);
    }

    if(message.data == "ImmersiveReader-Throttled") {
        window.webkit.messageHandlers.throttled.postMessage(null);
    }
});

function sendContentToReader(message) {
    document.getElementById('immersiveReaderIframe').contentWindow.postMessage(JSON.stringify({messageType:'Content', messageValue: message}), '*');
}
```

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Seleccione como destino un simulador o un dispositivo para establecer el esquema de archivo en Xcode.

![Esquema de archivo: Swift](../../media/ios/xcode-archive-scheme.png)

![Seleccionar destino: Swift](../../media/ios/xcode-select-target.png)

En Xcode, presione **Ctrl + R** o haga clic en el botón de reproducción para ejecutar el proyecto. La aplicación debe iniciarse en el simulador o el dispositivo especificados.

En la aplicación, debe ver lo siguiente:

![Aplicación de ejemplo: Swift](../../media/ios/sample-app-ipad.png)

Al hacer clic en el botón **Lector inmersivo**, verá que se inicia dicha herramienta con el contenido de la aplicación.

![Lector inmersivo: Swift](../../media/ios/immersive-reader-ipad.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un recurso y configurar AAD](../../how-to-create-immersive-reader.md)