---
title: Routering in een klassiek virtueel netwerk van Azure - CLI - beheren | Microsoft Docs
description: Meer informatie over het beheren van routering in VNets met de Azure CLI in het klassieke implementatiemodel
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 8fcb98723e7e872c932908e3456dc8680deb0901
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Beheren van Routering en het gebruik van virtuele apparaten (klassiek) met de Azure CLI

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure-CLI](virtual-network-create-udr-arm-cli.md)
> * [Sjabloon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassiek)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassiek)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [beheren Routering en het gebruik van virtuele apparaten in het Resource Manager-implementatiemodel](virtual-network-create-udr-arm-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

De Azure CLI Voorbeeldopdrachten onderstaande verwacht een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u uitvoeren van de opdrachten wilt zoals ze worden weergegeven in dit document, maakt u de omgeving wordt weergegeven in [een VNet maken (klassiek) met de Azure CLI](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor de front-end-subnet maken
Volg de onderstaande stappen voor het maken van de routetabel en de route die nodig zijn voor de front-end-subnet op basis van de bovenstaande scenario.

1. De volgende opdracht uit te schakelen naar de klassieke modus:

    ```azurecli
    azure config mode asm
    ```

    Uitvoer:

        info:    New mode is asm

2. Voer de volgende opdracht voor het maken van een routetabel voor de front-end-subnet:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Uitvoer:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Parameters:
   
   * **-l (of --locatie)**. Azure-regio waar de nieuwe NSG wordt gemaakt. In ons scenario *westus*.
   * **-n (of --naam)**. Naam voor de nieuwe NSG. In ons scenario *NSG-FrontEnd*.
3. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het subnet voor back-end (192.168.2.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Uitvoer:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Parameters:
   
   * **-r (of--route tabelnaam)**. Naam van de routetabel waar de route wordt toegevoegd. In ons scenario *UDR FrontEnd*.
   * **-a (of --adresvoorvoegsel)**. Het adresvoorvoegsel voor het subnet waarin pakketten naar. In ons scenario *192.168.2.0/24*.
   * **-t (of--volgende hoptype)**. Type object verkeer ontvangt. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, of *geen*.
   * **-p (of--volgende hop-ip-adressen**). IP-adres voor de volgende hop. In ons scenario *192.168.0.4*.
4. Voer de volgende opdracht om te koppelen van de routetabel gemaakt met de **FrontEnd** subnet:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Uitvoer:
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    Parameters:
   
   * **-t (of--vnet naam)**. De naam van de VNet waar het subnet bevindt. In ons scenario *TestVNet*.
   * **-n (of--subnet naam**. De naam van het subnet in dat de routetabel worden toegevoegd aan. In ons scenario *FrontEnd*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken
Voor het maken van de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het scenario, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht voor het maken van een routetabel voor de back-end-subnet:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het front-end-subnet (192.168.1.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel met de **back-end** subnet:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

