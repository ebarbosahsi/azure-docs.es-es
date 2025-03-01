---
title: Modelo de recurso de aplicación de Azure Service Fabric
description: En este artículo se proporciona información general sobre la administración de una aplicación de Azure Service Fabric mediante Azure Resource Manager.
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: 0019f154f301d2b688d4c16c9adb36ec386adef2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98790730"
---
# <a name="service-fabric-application-resource-model"></a>Modelo de recurso de aplicación de Service Fabric

Dispone de varias opciones a la hora de implementar aplicaciones de Azure Service Fabric en un clúster de Service Fabric y la más aconsejable es usar Azure Resource Manager, ya que de esa forma es posible describir las aplicaciones y los servicios en JSON y, después, implementarlos en la misma plantilla de Resource Manager en que se encuentre el clúster. A diferencia del uso de PowerShell o la CLI de Azure para implementar y administrar aplicaciones, si se utiliza Resource Manager, no es preciso esperar a que el clúster esté preparado; el registro, aprovisionamiento e implementación de las aplicaciones se pueden realizar en un solo paso. Resource Manager es la mejor opción para administrar el ciclo de vida de las aplicaciones en el clúster. Para obtener más información, consulte [Procedimientos recomendados: Infraestructura como código](service-fabric-best-practices-infrastructure-as-code.md#azure-service-fabric-resources).

La administración de las aplicaciones como recursos en Resource Manager puede ayudarle a obtener mejoras en estas áreas:

* Registro de auditoría: Resource Manager audita todas las operaciones y mantiene un registro de actividad detallado. Un registro de actividad puede ayudarle a realizar un seguimiento de los cambios realizados tanto en las aplicaciones como en el clúster.
* Control de acceso basado en rol: para administrar el acceso a los clústeres y a las aplicaciones implementadas en el clúster, utilice la misma plantilla de Resource Manager.
* Eficiencia de la administración: el uso de Resource Manager proporciona una ubicación individual (Azure Portal) desde la que se pueden administrar tanto el clúster como las implementaciones de aplicaciones críticas.

En este documento, aprenderá a:

> [!div class="checklist"]
>
> * Implementar recursos de aplicación mediante Resource Manager.
> * Actualizar recursos de aplicación mediante Resource Manager.
> * Eliminar recursos de aplicación.

## <a name="deploy-application-resources"></a>Implementación de recursos de aplicación

Estos son los pasos de alto nivel que se dan para implementar una aplicación y sus servicios mediante el modelo de recursos de aplicación de Resource Manager:
1. Empaquetar el código de la aplicación.
1. Cargar el paquete.
1. Hacer referencia a la ubicación del paquete en una plantilla de Resource Manager como un recurso de aplicación. 

Para obtener más información, consulte [Empaquetado de una aplicación](service-fabric-package-apps.md#create-an-sfpkg).

Después, cree una plantilla de Azure Resource Manager, actualice el archivo de parámetros con los detalles de la aplicación e implemente la plantilla en el clúster de Service Fabric. [Explore los ejemplos](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM).

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Para implementar una aplicación a partir de una plantilla de Resource Manager, es preciso tener una cuenta de almacenamiento. La cuenta de almacenamiento se usa para almacenar provisionalmente la imagen de la aplicación. 

Para almacenar provisionalmente aplicaciones se puede reutilizar una cuenta de almacenamiento existente, o bien se puede crear una. Si se usa una cuenta de almacenamiento existente, este paso se puede omitir. 

![Crear una cuenta de almacenamiento][CreateStorageAccount]

### <a name="configure-your-storage-account"></a>Configuración de una cuenta de almacenamiento

Después de crear la cuenta de almacenamiento cree un contenedor de blobs en el que se puedan almacenar provisionalmente las aplicaciones. En Azure Portal, vaya a la cuenta de Azure Storage en que desea almacenar las aplicaciones. Seleccione **Blobs** > **Agregar contenedor**. 

Para proteger los recursos del clúster es preciso establecer el nivel de acceso público en **privado**. Hay varias formas de conceder acceso:

* Autorizar el acceso a blobs y colas mediante [Azure Active Directory](../storage/common/storage-auth-aad-app.md).
* Conceder acceso a los datos de los blobs y las colas de Azure mediante [RBAC de Azure en Azure Portal](../storage/common/storage-auth-aad-rbac-portal.md).
* [Delegar el acceso mediante una firma de acceso compartido](/rest/api/storageservices/delegate-access-with-shared-access-signature).

El ejemplo de la siguiente captura de pantalla usa acceso de lectura anónimo para los blobs.

![Creación de un blob][CreateBlob]

### <a name="stage-the-application-in-your-storage-account"></a>Almacenamiento provisional de la aplicación en una cuenta de almacenamiento

Para poder implementar una aplicación, es preciso almacenarla temporalmente en un almacenamiento de blobs. En este tutorial, se crea el paquete de aplicaciones manualmente. Tenga en cuenta que este paso se puede automatizar. Para más información, consulte [Empaquetado de una aplicación](service-fabric-package-apps.md#create-an-sfpkg). 

En este tutorial, se usa la [aplicación de ejemplo de una votación](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart).

1. En Visual Studio, haga clic con el botón derecho en el proyecto **Voting** (Votación) y seleccione **Paquete**.

   ![Empaquetado de una aplicación][PackageApplication]  
1. Vaya al directorio *.\service-fabric-dotnet-quickstart\Voting\pkg\Debug*. Comprima el contenido en un archivo denominado *Voting.zip*. El archivo *ApplicationManifest.xml* debe estar en la raíz en el archivo zip.

   ![Comprimir la aplicación][ZipApplication]  
1. Cambie la extensión .zip por *.sfpkg*.

1. En Azure Portal, en el contenedor **apps** de su cuenta de almacenamiento, seleccione **Cargar** y, después, cargue **Voting.sfpkg**. 

   ![Cargar el paquete de la aplicación][UploadAppPkg]

La aplicación ya está almacenada provisionalmente y puede crear la plantilla de Resource Manager para implementar la aplicación.

### <a name="create-the-resource-manager-template"></a>Creación de la plantilla de Resource Manager

La aplicación de ejemplo contiene [plantillas de Azure Resource Manager](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM) que se pueden usar para implementar la aplicación. Los nombres de los archivos de plantilla son *UserApp.json* y *UserApp.Parameters.json*.

> [!NOTE]
> El archivo *UserApp.Parameters.json* debe actualizarse con el nombre del clúster.
>
>


| Parámetro              | Descripción                                 | Ejemplo                                                      | Comentarios                                                     |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| clusterName            | Nombre del clúster en el cual se realiza la implementación | sf-cluster123                                                |                                                              |
| application            | Nombre de la aplicación                 | Voting (Votación)                                                       |
| applicationTypeName    | Nombre del tipo de la aplicación           | VotingType                                                   | Debe coincidir con ApplicationManifest.xml                 |
| applicationTypeVersion | Versión del tipo de aplicación         | 1.0.0                                                        | Debe coincidir con ApplicationManifest.xml                 |
| serviceName            | El nombre del servicio         | Voting~VotingWeb                                             | Debe tener el formato ApplicationName~ServiceType            |
| serviceTypeName        | Nombre del tipo del servicio                | VotingWeb                                                    | Debe coincidir con ServiceManifest.xml                 |
| appPackageUrl          | Dirección URL del almacén de blobs de la aplicación     | https:\//servicefabricapps.blob.core.windows.net/apps/Voting.sfpkg | Dirección URL del paquete de aplicación en el almacén de blobs (el procedimiento para establecerla se describirá más adelante en este mismo artículo) |

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

### <a name="deploy-the-application"></a>Implementación de la aplicación

Ejecute el cmdlet **New-AzResourceGroupDeployment** para implementar la aplicación en el grupo de recursos que contiene el clúster:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "sf-cluster-rg" -TemplateParameterFile ".\UserApp.Parameters.json" -TemplateFile ".\UserApp.json" -Verbose
```

## <a name="upgrade-the-service-fabric-application-by-using-resource-manager"></a>Actualización de la aplicación de Service Fabric mediante Resource Manager


> [!IMPORTANT]
> Cualquier servicio implementado a través de la definición de JSON de ARM se debe quitar de la sección DefaultServices del archivo ApplicationManifest.xml correspondiente.


Cualquier aplicación implementada en un clúster de Service Fabric se puede actualizar por una de estas razones:

* Se agrega un nuevo servicio a la aplicación. Se debe agregar una definición de servicio a los archivos *service-manifest.xml* y *application-manifest.xml* cada vez que se agrega un servicio a la aplicación. Para reflejar una versión nueva de la aplicación, también debe cambiar la versión del tipo de aplicación de la 1.0.0 a la 1.0.1 en [UserApp.Parameters.json](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/blob/master/ARM/UserApp.Parameters.json):

    ```json
    "applicationTypeVersion": {
        "value": "1.0.1"
    },
    "serviceName2": {
        "value": "Voting~VotingData"
    },
    "serviceTypeName2": {
        "value": "VotingDataType"
    }
    ```

* Se agrega una nueva versión de un servicio existente a la aplicación. Entre los ejemplos se incluyen los cambios en el código de la aplicación y las actualizaciones de la versión y el nombre del tipo de aplicación. Para esta actualización, actualice UserApp.Parameters.json como se indica a continuación:

    ```json
     "applicationTypeVersion": {
        "value": "1.0.1"
    },
    ```

## <a name="delete-application-resources"></a>Eliminación de recursos de la aplicación

Para eliminar una aplicación implementada mediante el modelo de recursos de aplicación en Resource Manager:

1. Use el cmdlet [Get-AzResource](/powershell/module/az.resources/get-azresource) para obtener el identificador de recurso de la aplicación:

    ```powershell
    Get-AzResource  -Name <String> | f1
    ```

1. Use el cmdlet [Remove-AzResource](/powershell/module/az.resources/remove-azresource) para eliminar los recursos de la aplicación:

    ```powershell
    Remove-AzResource  -ResourceId <String> [-Force] [-ApiVersion <String>]
    ```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre el modelo de recurso de la aplicación:

* [Modelar una aplicación en Service Fabric](service-fabric-application-model.md)
* [Manifiestos de servicio y de aplicación de Service Fabric](service-fabric-application-and-service-manifests.md)
* [Procedimientos recomendados: Infraestructura como código](service-fabric-best-practices-infrastructure-as-code.md#azure-service-fabric-resources)
* [Administración de aplicaciones y servicios como recursos de Azure](service-fabric-best-practices-infrastructure-as-code.md)


<!--Image references-->
[CreateStorageAccount]: ./media/service-fabric-application-model/create-storage-account.png
[CreateBlob]: ./media/service-fabric-application-model/create-blob.png
[PackageApplication]: ./media/service-fabric-application-model/package-application.png
[ZipApplication]: ./media/service-fabric-application-model/zip-application.png
[UploadAppPkg]: ./media/service-fabric-application-model/upload-app-pkg.png
