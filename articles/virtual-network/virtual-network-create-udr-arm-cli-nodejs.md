---
title: Beheren van Routering en virtuele apparaten met behulp van de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het beheren van Routering en virtuele apparaten met behulp van de Azure CLI 1.0.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 5f21bc7a4fcd9507ea9d6b2b752a2328a7b834f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-10"></a>Door de gebruiker gedefinieerde Routes (UDR) met behulp van de Azure CLI 1.0 maken

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Sjabloon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassiek)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassiek)](virtual-network-create-udr-classic-cli.md)

Maak aangepaste Routering en virtuele apparaten met de Azure CLI.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren 

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies: 

- [Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](virtual-network-create-udr-arm-cli.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

De Azure CLI Voorbeeldopdrachten onderstaande verwacht een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, moet u eerst de testomgeving verder door de implementatie [deze sjabloon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikt u op **implementeren in Azure**, vervangt u de standaardwaarden voor parameters indien nodig en volg de instructies in de portal.


## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor de front-end-subnet maken
Volg de onderstaande stappen voor het maken van de routetabel en de route die nodig zijn voor de front-end-subnet op basis van de bovenstaande scenario.

1. Voer de volgende opdracht voor het maken van een routetabel voor de front-end-subnet:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Uitvoer:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Parameters:
   
   * **-g (of --resourcegroep)**. Naam van de resourcegroep waar de UDR wordt gemaakt. In ons scenario *TestRG*.
   * **-l (of --locatie)**. Azure-regio waar de nieuwe UDR wordt gemaakt. In ons scenario *westus*.
   * **-n (of --naam)**. Naam voor de nieuwe UDR. In ons scenario *UDR FrontEnd*.
2. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het subnet voor back-end (192.168.2.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Uitvoer:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Parameters:
   
   * **-r (of--route tabelnaam)**. Naam van de routetabel waar de route wordt toegevoegd. In ons scenario *UDR FrontEnd*.
   * **-a (of --adresvoorvoegsel)**. Het adresvoorvoegsel voor het subnet waarin pakketten naar. In ons scenario *192.168.2.0/24*.
   * **-y (of--volgende hoptype)**. Type object verkeer ontvangt. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, of *geen*.
   * **-p (of--volgende hop-ip-adressen**). IP-adres voor de volgende hop. In ons scenario *192.168.0.4*.
3. Voer de volgende opdracht om te koppelen van de routetabel die eerder is gemaakt met de **FrontEnd** subnet:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Uitvoer:
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Parameters:
   
   * **-e (of--vnet naam)**. De naam van de VNet waar het subnet bevindt. In ons scenario *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken
Voor het maken van de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het bovenstaande scenario, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht voor het maken van een routetabel voor de back-end-subnet:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het front-end-subnet (192.168.1.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel met de **back-end** subnet:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Doorsturen via IP op FW1 inschakelen
Om in te schakelen doorsturen via IP in de NIC die wordt gebruikt door **FW1**, de volgende stappen uit:

1. Voer de opdracht die volgt en Let op de waarde voor **doorsturen via IP inschakelen**. Moet worden ingesteld op *false*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Uitvoer:
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Voer de volgende opdracht doorsturen via IP inschakelen:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Uitvoer:
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Parameters:
   
   * **-f (of--enable--doorsturen via ip)**. *de waarde True* of *false*.

