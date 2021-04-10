---
title: 'Inicio rápido: Implementación de un clúster de Azure Kubernetes Service mediante la CLI de Azure con nodos de computación confidencial'
description: En este inicio rápido aprenderá a crear un clúster de AKS con nodos confidenciales y a implementar una aplicación Hola mundo mediante la CLI de Azure.
author: agowdamsft
ms.service: container-service
ms.subservice: confidential-computing
ms.topic: quickstart
ms.date: 03/18/2020
ms.author: amgowda
ms.custom: contentperf-fy21q3
ms.openlocfilehash: 73770acefc8a153e4a2f2fde146f9afd4c319cd3
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105933141"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-with-confidential-computing-nodes-dcsv2-using-azure-cli"></a>Inicio rápido: Implementación de un clúster de Azure Kubernetes Service (AKS) con nodos de computación confidencial (DCsv2) mediante la CLI de Azure

Este inicio rápido está destinado a aquellos desarrolladores u operadores de clúster que desean crear rápidamente un clúster de AKS e implementar una aplicación que supervise las aplicaciones mediante el servicio Kubernetes administrado en Azure. También puede aprovisionar el clúster y agregar nodos de computación confidencial desde Azure Portal.

## <a name="overview"></a>Información general

En este inicio rápido, aprenderá a implementar un clúster de Azure Kubernetes Service (AKS) con nodos de computación confidencial mediante la CLI de Azure y ejecutar una aplicación Hola mundo sencilla en un enclave. AKS es un servicio de Kubernetes administrado que permite implementar y administrar clústeres rápidamente. Para más información, lea [Introducción a AKS](../aks/intro-kubernetes.md) e [Introducción a los nodos confidenciales de AKS](confidential-nodes-aks-overview.md).

> [!NOTE]
> Las máquinas virtuales de la serie DCsv2 de computación confidencial sacan provecho de hardware especializado que está sujeto a una mayor disponibilidad de precios y regiones. Para más información, consulte en la página de máquinas virtuales [las SKU disponibles y las regiones que se admiten](virtual-machine-solutions.md).

### <a name="confidential-computing-node-features-dcsv2"></a>Características de los nodos de computación confidencial (DCsv2)

1. Nodos de trabajo Linux que admiten solo contenedores Linux.
1. Máquina virtual de generación 2 con nodos de Virtual Machines de Ubuntu 18.04.
1. CPU basada en Intel SGX con memoria caché de páginas cifrada (EPC). Obtenga más información [aquí](./faq.md).
1. Soporte técnico para versión 1.16 y posteriores de Kubernetes.
1. Controlador Intel SGX DCAP preinstalado previamente en los nodos de AKS Obtenga más información [aquí](./faq.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

1. Una suscripción a Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
1. La versión 2.0.64 de la CLI de Azure, o cualquier versión posterior, instalada y configurada en la máquina de implementación (ejecute `az --version` para encontrar la versión). Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](../container-registry/container-registry-get-started-azure-cli.md).
1. Un mínimo de seis núcleos de **DCsv2** en su suscripción disponibles para su uso. De forma predeterminada, la cuota de núcleos de máquina virtual para la computación confidencial por suscripción de Azure es ocho núcleos. Si planea aprovisionar un clúster que requiera más de ocho núcleos, siga [estas](../azure-portal/supportability/per-vm-quota-requests.md) instrucciones para generar una incidencia de aumento de cuota.

## <a name="create-a-new-aks-cluster-with-confidential-computing-nodes-and-add-on"></a>Creación de un clúster de AKS con nodos y complementos de computación confidencial

Siga las instrucciones siguientes para agregar nodos compatibles con la computación confidencial con el complemento.

### <a name="create-an-aks-cluster-with-a-system-node-pool"></a>Creación de un clúster de AKS con un grupo de nodos del sistema

Si ya tiene un clúster de AKS que cumpla los requisitos anteriores, [vaya a la sección de clúster existente](#existing-cluster) para agregar un nuevo grupo de nodos de computación confidencial.

Primero, cree un grupo de recursos para el clúster con el comando [az group create][az-group-create]. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la región *westus2*:

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Ahora cree un clúster de AKS con el comando [az aks create][az-aks-create]:

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --generate-ssh-keys --enable-addon confcom
```

La operación anterior crea un clúster de AKS con un grupo de nodos del sistema con el complemento habilitado. A continuación, agregue un grupo de nodos de usuario con funcionalidades de computación confidencial al clúster de AKS.

### <a name="add-a-confidential-computing-node-pool-to-the-aks-cluster"></a>Adición de un grupo de nodos de computación confidencial al clúster de AKS 

Ejecute el siguiente comando para agregar un grupo de nodos de usuario de tamaño `Standard_DC2s_v2` con tres nodos. Aquí puede elegir otra SKU de la lista de [SKU y regiones compatibles de DCsv2](../virtual-machines/dcv2-series.md).

```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-vm-size Standard_DC2s_v2
```

Después de la ejecución, debe estar visible un nuevo grupo de nodos con **DCsv2** con los daemonsets del complemento de computación confidencial ([complemento de dispositivo SGX](confidential-nodes-aks-overview.md#sgx-plugin)).

### <a name="verify-the-node-pool-and-add-on"></a>Comprobación del complemento y del grupo de nodos

Obtenga las credenciales para el clúster de AKS mediante el comando [az aks get-credentials][az-aks-get-credentials]:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Compruebe que los nodos se crean correctamente y que los daemonsets relacionados con SGX se ejecutan en grupos de nodos de **DCsv2** mediante el comando kubectl get pods & nodes, como se muestra a continuación:

```console
$ kubectl get pods --all-namespaces

kube-system     sgx-device-plugin-xxxx     1/1     Running
```

Si el resultado es como el anterior, el clúster de AKS ya está listo para ejecutar aplicaciones confidenciales.

Vaya a la sección de implementación de [Hola mundo desde enclave](#hello-world) para probar una aplicación en un enclave. O bien, siga las instrucciones que encontrará a continuación si desea agregar grupos de nodos adicionales en AKS (AKS admite la mezcla de grupos de nodos de SGX y grupos de nodos que no son de SGX).

## <a name="add-a-confidential-computing-node-pool-to-an-existing-aks-cluster"></a>Adición de un grupo de nodos de computación confidencial a un clúster de AKS existente<a id="existing-cluster"></a>

En esta sección se supone que ya tiene un clúster de AKS que cumple con los criterios que se enumeran en la sección de requisitos previos (se aplica al complemento).

### <a name="enable-the-confidential-computing-aks-add-on-on-the-existing-cluster"></a>Habilitación del complemento de AKS de computación confidencial en el clúster existente

Ejecute el siguiente comando para habilitar el complemento de computación confidencial:

```azurecli-interactive
az aks enable-addons --addons confcom --name MyManagedCluster --resource-group MyResourceGroup 
```

### <a name="add-a-dcsv2-user-node-pool-to-the-cluster"></a>Agregue un grupo de nodos de usuario **DCsv2** al clúster

> [!NOTE]
> Para usar la funcionalidad de computación confidencial, el clúster de AKS existente debe tener al menos un grupo de nodos basado en una SKU de máquina virtual **DCsv2**. Para más información sobre la computación confidencial de las SKU de las máquinas virtuales DCs-v2 consulte [SKU disponibles y regiones admitidas](virtual-machine-solutions.md).

Ejecute el siguiente comando para crear un nuevo grupo de nodos:

```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-count 1 --node-vm-size Standard_DC4s_v2
```

Compruebe que se ha creado el nuevo grupo de nodos con el nombre confcompool1:

```azurecli-interactive
az aks nodepool list --cluster-name myAKSCluster --resource-group myResourceGroup
```

### <a name="verify-that-daemonsets-are-running-on-confidential-node-pools"></a>Comprobación de que los daemonsets se ejecutan en grupos de nodos confidenciales

Inicie sesión en el clúster de AKS existente para realizar la siguiente comprobación.

```console
kubectl get nodes
```

La salida debe mostrar el elemento confcompool1 recién agregado en el clúster de AKS. También puede ver otros daemonsets.

```console
$ kubectl get pods --all-namespaces

kube-system     sgx-device-plugin-xxxx     1/1     Running
```

Si el resultado es como el anterior, el clúster de AKS ya está listo para ejecutar aplicaciones confidenciales. Para implementar una aplicación de prueba, siga estas instrucciones.

## <a name="hello-world-from-isolated-enclave-application"></a>Hola mundo desde una aplicación de enclave aislada <a id="hello-world"></a>
Cree un archivo denominado *hello-world-enclave.yaml* y pegue el siguiente manifiesto YAML. Este código de aplicación de ejemplo basado en Open Enclave se puede encontrar en el [proyecto de Open Enclave](https://github.com/openenclave/openenclave/tree/master/samples/helloworld). En la siguiente implementación se supone que ha implementado el complemento "confcom".

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
  labels:
    app: sgx-test
spec:
  template:
    metadata:
      labels:
        app: sgx-test
    spec:
      containers:
      - name: sgxtest
        image: oeciteam/sgx-test:1.0
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 5 # This limit will automatically place the job into confidential computing node. Alternatively you can target deployment to nodepools
      restartPolicy: Never
  backoffLimit: 0
  ```

Ahora, use el comando kubectl apply para crear un trabajo de ejemplo que se iniciará en un enclave seguro, como se muestra en la siguiente salida de ejemplo:

```console
$ kubectl apply -f hello-world-enclave.yaml

job "sgx-test" created
```

Puede confirmar que la carga de trabajo ha creado correctamente un entorno de ejecución de confianza (enclave) mediante la ejecución de los siguientes comandos:

```console
$ kubectl get jobs -l app=sgx-test

NAME       COMPLETIONS   DURATION   AGE
sgx-test   1/1           1s         23s
```

```console
$ kubectl get pods -l app=sgx-test

NAME             READY   STATUS      RESTARTS   AGE
sgx-test-rchvg   0/1     Completed   0          25s
```

```console
$ kubectl logs -l app=sgx-test

Hello world from the enclave
Enclave called into host to print: Hello World!
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar los grupos de nodos asociados o eliminar el clúster de AKS, use los siguientes comandos:

### <a name="remove-the-confidential-computing-node-pool"></a>Eliminación del grupo de nodos de computación confidencial

```azurecli-interactive
az aks nodepool delete --cluster-name myAKSCluster --name myNodePoolName --resource-group myResourceGroup
```

### <a name="delete-the-aks-cluster"></a>Eliminación del clúster de AKS

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

## <a name="next-steps"></a>Pasos siguientes

* Ejecute las aplicaciones Python, Node, etc., de forma confidencial mediante contenedores confidenciales visitando [ejemplos de contenedores confidenciales](https://github.com/Azure-Samples/confidential-container-samples).

* Para ejecutar aplicaciones compatibles con enclave, visite [ejemplos de contenedores de Azure compatibles con enclave](https://github.com/Azure-Samples/confidential-computing/blob/main/containersamples/).

<!-- LINKS -->
[az-group-create]: /cli/azure/group#az_group_create
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials