---
title: Cómo obtener los nombres de host de los nodos de clúster de Azure HDInsight
description: Obtenga información sobre cómo obtener los nombres de host y el nombre FQDN de los nodos de clúster de Azure HDInsight.
ms.service: hdinsight
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.date: 03/23/2021
ms.openlocfilehash: 676b4ccd52e8a6f1f378756a255ad55b74f18ebe
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105111413"
---
# <a name="find-the-host-names-of-cluster-nodes"></a>Buscar los nombres de host de los nodos de clúster

El clúster de HDInsight se crea con DNS público clustername.azurehdinsight.net. Cuando se usa SSH en nodos individuales o se establece la conexión con nodos de clúster con en la misma red virtual personalizada, es necesario usar el nombre de host o nombres de dominio completos (FQDN) de los nodos de clúster.

En este artículo, aprenderá a obtener los nombres de host de los nodos de clúster. Puede obtenerla manualmente a través de la interfaz de usuario Web de Ambari o automáticamente a través de la API de REST de Ambari.

> [!WARNING]
> Use los siguientes métodos recomendados para capturar los nombres de host de los nodos de clúster. Los números en el nombre de host no se garantizan en secuencia y HDInsight puede cambiar el formato de nombre de host para que se alinee con las máquinas virtuales con la actualización de versión. No tome la dependencia de ninguna convención de nomenclatura que exista actualmente. 
>

Puede obtener los nombres de host a través de la interfaz de usuario de Ambari o la API de REST de Ambari. 

## <a name="get-the-host-names-from-ambari-web-ui"></a>Obtención de los nombres de host de la interfaz de usuario Web de Ambari
Puede usar la interfaz de usuario Web de Ambari para obtener los nombres de host cuando se usa SSH en el nodo. La vista de hosts de la interfaz de usuario web de Ambari está disponible en su clúster de HDInsight en `https://CLUSTERNAME.azurehdinsight.net/#/main/hosts`, donde `CLUSTERNAME` es el nombre de su clúster.

![Obtener nombres de host en la interfaz de usuario de Ambari](.\media\find-host-name\find-host-name-in-ambari-ui.png)

## <a name="get-the-host-names-from-ambari-rest-api"></a>Obtener de los nombres de host de la API de REST de Ambari
Al compilar scripts de automatización, puede usar la API de REST de Ambari para obtener los nombres de host antes de realizar las conexiones a los hosts. Los números en el nombre de host no se garantizan en secuencia y HDInsight puede cambiar el formato de nombre de host para que se alinee con las máquinas virtuales con la actualización de versión. No tome la dependencia de ninguna convención de nomenclatura que exista actualmente. 

Estos son algunos ejemplos de cómo recuperar el FQDN de los nodos del clúster. Para obtener más información sobre la API REST de Ambari, consulte [Administrar clústeres de HDInsight mediante la API REST de Apache Ambari](.\hdinsight-hadoop-manage-ambari-rest-api.md)

El siguiente ejemplo usa [jq](https://stedolan.github.io/jq/) o [ConvertFrom-Json](/powershell/module/microsoft.powershell.utility/convertfrom-json) para analizar el documento de respuesta JSON y mostrar solo los nombres de host.

```bash
export password=''
export clusterName=''
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" \
| jq -r '.items[].Hosts.host_name'
```  

```powershell
$clusterName=''
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.Hosts.host_name
```