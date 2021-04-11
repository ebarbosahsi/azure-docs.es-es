---
title: 'Tutorial: Protección de un centro virtual mediante Azure Firewall Manager'
description: En este tutorial aprenderá a proteger un centro virtual con Azure Firewall Manager mediante Azure Portal.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 03/19/2021
ms.author: victorh
ms.openlocfilehash: 14ec6fafbbadff2ecc37b229270c269f068a666f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104670467"
---
# <a name="tutorial-secure-your-virtual-hub-using-azure-firewall-manager"></a>Tutorial: Protección de un centro virtual mediante Azure Firewall Manager

Mediante Azure Firewall Manager puede crear centros virtuales protegidos y así proteger el tráfico en la nube destinado a direcciones IP privadas, PaaS de Azure e Internet. El enrutamiento del tráfico al firewall es automático, por lo que no es necesario crear rutas definidas por el usuario (UDR).

![protección de la red en la nube](media/secure-cloud-network/secure-cloud-network.png)

Firewall Manager también admite una arquitectura de red virtual de centro. Para ver una comparación entre los tipos de arquitectura de red virtual de centro y centro virtual protegido, consulte [¿Cuáles son las opciones de arquitectura de Azure Firewall Manager?](vhubs-and-vnets.md)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear la red virtual de tipo hub-and-spoke
> * Crear un centro virtual protegido
> * Conectar las redes virtuales de tipo hub-and-spoke
> * Enrutamiento del tráfico al centro
> * Implementación de los servidores
> * Creación de una directiva de firewall y protección del centro
> * Probar el firewall

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-hub-and-spoke-architecture"></a>Creación de una arquitectura en estrella tipo hub-and-spoke

En primer lugar, cree redes virtuales de radio en las que pueda colocar los servidores.

### <a name="create-two-spoke-virtual-networks-and-subnets"></a>Creación de dos redes virtuales de radio y subredes

Cada una de las dos redes virtuales tendrá un servidor de carga de trabajo y se protegerá con el firewall.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. Busque **Red virtual** y seleccione **Crear**.
2. En **Suscripción**, seleccione la suscripción.
1. En **Grupo de recursos**, seleccione **Crear nuevo** y escriba **fw-manager-rg** como nombre y seleccione **Aceptar**.
2. En **Nombre**, escriba **Spoke-01**.
3. En **Región**, seleccione **(EE. UU.) Este de EE. UU.** .
4. Seleccione **Siguiente: Direcciones IP**.
1. En **Espacio de direcciones**, escriba **10.0.0.0/16**.
3. En **Nombre de subred**, seleccione **predeterminada**.
4. En **Nombre de subred**, escriba **Workload-01-SN**.
5. En **Intervalo de direcciones de subred**, escriba **10.0.1.0/24**.
6. Seleccione **Guardar**.
1. Seleccione **Revisar + crear**.
2. Seleccione **Crear**.

Repita este procedimiento para crear otra red virtual parecida:

Nombre: **Spoke-02**<br>
Espacio de direcciones: **10.1.0.0/16**<br>
Nombre de subred: **Workload-02-SN**<br>
Intervalo de direcciones de subred: **10.1.1.0/24**

### <a name="create-the-secured-virtual-hub"></a>Creación del centro virtual protegido

Cree el centro virtual protegido con Firewall Manager.

1. En la página principal de Azure Portal, seleccione **Todos los servicios**.
2. En el cuadro de búsqueda, escriba **Firewall Manager** y seleccione **Firewall Manager**.
3. En la página **Firewall Manager**, seleccione **Ver los centros virtuales seguros**.
4. En la página **Firewall Manager | Centros virtuales protegidos**, seleccione **Crear un nuevo centro virtual protegido**.
5. En **Grupo de recursos**, seleccione **fw-manager-rg**.
7. En **Región**, seleccione **Este de EE. UU.** .
1. En **Nombre del centro virtual protegido**, escriba **Hub-01**.
2. En **Espacio de direcciones del concentrador**, escriba **10.2.0.0/16**.
3. Como nombre de la nueva WAN virtual, escriba **Vwan-01**.
4. Deje desactivada la casilla **Incluir VPN Gateway para habilitar asociados de seguridad de confianza**.
5. Seleccione **Siguiente: Azure Firewall**.
6. Acepte el valor predeterminado **Azure Firewall** **Habilitado** y seleccione **Siguiente: Asociados de seguridad de confianza**.
7. Acepte el valor predeterminado **Asociado de seguridad de confianza** **Deshabilitado** y seleccione **Siguiente: Review + create** (Revisar y crear).
8. Seleccione **Crear**. 

   Se tarda en realizar unos 30 minutos.

Puede obtener la dirección IP pública del firewall una vez completada la implementación.

1. Abra **Firewall Manager**.
2. Seleccione **Concentradores virtuales**.
3. Seleccione **hub-01**.
7. Seleccione **Configuración de IP pública**.
8. Anote la dirección IP pública para usarla más tarde.

### <a name="connect-the-hub-and-spoke-virtual-networks"></a>Conexión de las redes virtuales de tipo hub-and-spoke

Ahora, podrá emparejar las redes virtuales de tipo hub-and-spoke.

1. Seleccione el grupo de recursos **fw-manager.rg** y, después, seleccione la WAN virtual **Vwan-01**.
2. En **Conectividad**, seleccione **Conexiones de red virtual**.
3. Seleccione **Agregar conexión**.
4. En **Nombre de conexión**, escriba **hub-spoke-01**.
5. En **Centros**, seleccione **Hub-01**.
6. En **Grupo de recursos**, seleccione **fw-manager-rg**.
7. En **Red virtual**, seleccione **Spoke-01**.
8. Seleccione **Crear**.

Repita el procedimiento para conectar la red virtual **Spoke-02** con el nombre de conexión **hub-spoke-02**.

## <a name="deploy-the-servers"></a>Implementación de los servidores

1. En Azure Portal, seleccione **Crear un recurso**.
2. Seleccione **Windows Server 2016 Datacenter** en la lista **Popular**.
3. Especifique estos valores para la máquina virtual:

   |Configuración  |Value  |
   |---------|---------|
   |Resource group     |**fw-manager-rg**|
   |Nombre de la máquina virtual     |**Srv-workload-01**|
   |Region     |**(EE. UU.) Este de EE. UU.**|
   |Nombre de usuario del administrador     |Escriba un nombre de usuario|
   |Contraseña     |Escriba una contraseña|

4. En **Reglas de puerto de entrada**, para **Puertos de entrada públicos**, seleccione **Ninguno**.
6. Acepte los restantes valores predeterminados y seleccione **Siguiente: Discos**.
7. Acepte los valores predeterminados del disco y seleccione **Siguiente: Redes**.
8. Seleccione **Spoke-01** para la red virtual y seleccione **Workload-01-SN** para la subred.
9. En **IP pública**, seleccione **Ninguno**.
11. Acepte los restantes valores predeterminados y seleccione **Siguiente: Administración**.
12. Seleccione **Deshabilitar** para deshabilitar los diagnósticos de arranque. Acepte los restantes valores predeterminados y seleccione **Revisar y crear**.
13. Revise la configuración en la página de resumen y seleccione **Crear**.

Use la información de la tabla siguiente para configurar otra máquina virtual llamada **Srv-Workload-02**. El resto de la configuración es la misma que la de la máquina virtual **Srv-workload-01**.

|Configuración  |Value  |
|---------|---------|
|Virtual network|**Spoke-02**|
|Subnet|**Workload-02-SN**|

Una vez implementados los servidores, seleccione un recurso de servidor y en **Redes** tenga en cuenta la dirección IP privada de cada servidor.

## <a name="create-a-firewall-policy-and-secure-your-hub"></a>Creación de una directiva de firewall y protección del centro

Una directiva de firewall define colecciones de reglas para dirigir el tráfico en uno o varios centros virtuales protegidos. Creará la directiva de firewall y, a continuación, protegerá el centro.

1. En Firewall Manager, seleccione **Ver las directivas de Azure Firewall**.
2. Seleccione **Crear una directiva de Azure Firewall**.
1. En **Grupo de recursos**, seleccione **fw-manager-rg**.
1. En **Detalles de la directiva**, como **Nombre** escriba **Policy-01** y como **Región** seleccione **Este de EE. UU.**
1. Seleccione **Siguiente: Configuración DNS**.
1. Seleccione **Siguiente: Inspección de TLS (versión preliminar)** .
1. Seleccione **Siguiente: Reglas**.
1. En la pestaña **Reglas**, seleccione **Agregar una colección de reglas**.
1. En la página **Agregar una colección de reglas**, escriba **App-RC-01** en **Nombre**.
1. En **Tipo de colección de reglas**, seleccione **Aplicación**.
1. En **Prioridad**, escriba **100**.
1. Asegúrese de que el valor de **Rule collection action** (Acción de la colección de reglas) es **Permitir**.
1. Como **Nombre** de la regla, escriba **Allow-msft**.
1. Para **Tipo de origen**, seleccione **Dirección IP**.
1. Para **Origen**, escriba **\*** .
1. En **Protocolo**, escriba **http,https**.
1. Asegúrese de que **Tipo de destino** es **FQDN**.
1. En **Destino**, escriba **\*.microsoft.com**.
1. Seleccione **Agregar**.

Agregue una regla DNAT para poder conectar un escritorio remoto a la máquina virtual **Srv-Workload-01**.

1. Seleccione **Agregar o agregar recopilación de reglas**.
1. En **Nombre**, escriba **dnat-rdp**.
1. En **Tipo de colección de reglas**, seleccione **DNAT**.
1. En **Prioridad**, escriba **100**.
1. Como **Nombre** de la regla, escriba **Allow-rdp**.
1. Para **Tipo de origen**, seleccione **Dirección IP**.
1. Para **Origen**, escriba **\*** .
1. En **Protocolo**, seleccione **TCP**.
1. En **Puertos de destino**, escriba **3389**.
1. En **Tipo de destino**, seleccione **Dirección IP**.
1. En **Destino**, escriba la dirección IP pública del firewall que anotó anteriormente.
1. En **Dirección traducida**, escriba la dirección IP privada para **Srv-Workload-01** que anotó anteriormente.
1. En **Puerto traducido**, escriba **3389**.
1. Seleccione **Agregar**.

Agregue una regla de red para conectar un escritorio remoto de **Srv-Workload-01** a **Srv-Workload-02**.

1. Seleccione **Agregar una colección de reglas**.
2. En **Nombre**, escriba **vnet-rdp**.
3. En **Tipo de colección de reglas**, seleccione **Red**.
4. En **Prioridad**, escriba **100**.
1. En **Acción de recopilación de reglas**, seleccione **Denegar**.
1. Como **Nombre** de la regla, escriba **Allow-vnet**.
1. Para **Tipo de origen**, seleccione **Dirección IP**.
1. Para **Origen**, escriba **\*** .
1. En **Protocolo**, seleccione **TCP**.
1. En **Puertos de destino**, escriba **3389**.
1. En **Tipo de destino**, seleccione **Dirección IP**.
1. En **Destino**, escriba la dirección IP privada de **Srv-Workload-02** que anotó anteriormente.
1. Seleccione **Agregar**.
1. Seleccione **Revisar y crear**.
1. Seleccione **Crear**.

## <a name="associate-policy"></a>Asociación de directiva

Asocie la directiva de firewall con el concentrador.

1. En Firewall Manager, seleccione **Directivas de Azure Firewall**.
1. Active la casilla de **Policy-01**.
1. Seleccione **Manage associations/Associate hubs** (Administrar asociaciones/Asociar concentradores).
1. Seleccione **hub-01**.
1. Seleccione **Agregar**.

## <a name="route-traffic-to-your-hub"></a>Enrutamiento del tráfico al centro

Ahora debe asegurarse de que el tráfico de red se enruta a través del firewall.

1. En Firewall Manager, seleccione **Concentradores virtuales**.
2. Seleccione **Hub-01**.
3. En **Ajustes**, seleccione **Configuración de seguridad**.
4. En **Tráfico de Internet**, seleccione **Azure Firewall**.
5. En **Private traffic** (Tráfico privado), seleccione **Send via Azure Firewall** (Enviar a través de Azure Firewall).
1. Seleccione **Guardar**.

   Se tarda unos minutos en actualizar las tablas de rutas.
1. Compruebe que las dos conexiones muestran que Azure Firewall protege el tráfico de Internet y el privado.

## <a name="test-the-firewall"></a>Probar el firewall

Para probar las reglas de firewall, deberá conectar un escritorio remoto mediante la dirección IP pública del firewall, que se conecta mediante NAT a **Srv-Workload-01**. Desde allí, deberá usar un explorador para probar la regla de aplicación y conectar un escritorio remoto a **Srv-Workload-02** para probar la regla de red.

### <a name="test-the-application-rule"></a>Prueba de la regla de aplicación

Ahora, pruebe las reglas de firewall para confirmar que funcionan según lo previsto.

1. Conecte un escritorio remoto a la dirección IP pública del firewall e inicie sesión.

3. Abra Internet Explorer y vaya a `https://www.microsoft.com`.
4. Seleccione **Aceptar** > **Cerrar** en las alertas de seguridad de Internet Explorer.

   Debería ver la página principal de Microsoft.

5. Vaya a `https://www.google.com`.

   El firewall debería bloquearle.

Con ello, ha comprobado que la regla de aplicación de firewall funciona:

* Puede navegar al FQDN permitido pero no a ningún otro.

### <a name="test-the-network-rule"></a>Prueba de la regla de red

Ahora pruebe la regla de red.

- Desde Srv-Workload-01, abra un escritorio remoto conectado a la dirección IP privada de Srv-Workload-02.

   Un escritorio remoto debe conectarse a Srv-Workload-02.

Con ello, ha comprobado que la regla de red de firewall funciona:
* Puede conectar un escritorio remoto a un servidor ubicado en otra red virtual.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de probar los recursos de firewall, elimine el grupo de recursos **fw-manager-rg** para eliminar todos los recursos relacionados con el firewall.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre los asociados de seguridad de confianza](trusted-security-partners.md)
