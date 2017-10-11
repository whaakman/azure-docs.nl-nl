---
title: Beheren van Routering en virtuele apparaten met behulp van de Azure CLI 2.0 | Microsoft Docs
description: Informatie over het beheren van Routering en virtuele apparaten met behulp van de Azure CLI 2.0.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: e5d9519998346619093f443b740c8904283f76e8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-20"></a>Door de gebruiker gedefinieerde Routes (UDR) met behulp van de Azure CLI 2.0 maken

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Sjabloon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassieke implementatie)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassieke implementatie)](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren 

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies: 

- [Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel 
- [Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -onze volgende generatie CLI voor de resource management-implementatiemodel (in dit artikel)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

De Azure CLI Voorbeeldopdrachten onderstaande verwacht een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, moet u eerst de testomgeving verder door de implementatie [deze sjabloon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikt u op **implementeren in Azure**, vervangt u de standaardwaarden voor parameters indien nodig en volg de instructies in de portal.


## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor de front-end-subnet maken
Volg de onderstaande stappen voor het maken van de routetabel en de route die nodig zijn voor de front-end-subnet op basis van de bovenstaande scenario.

1. Maken van een routetabel voor het front-end-subnet met de [az netwerk routetabel maken](/cli/azure/network/route-table#create) opdracht:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    Uitvoer:

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. Maken van een route dat al het verkeer naar de back-end-subnet (192.168.2.0/24) verzendt naar de **FW1** VM (192.168.0.4) met behulp van de [az routetabel netwerkroute maken](/cli/azure/network/route-table/route#create) opdracht:

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    Uitvoer:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    Parameters:

    * **--route tabelnaam**. Naam van de routetabel waar de route wordt toegevoegd. In ons scenario *UDR FrontEnd*.
    * **--adresvoorvoegsel**. Het adresvoorvoegsel voor het subnet waarin pakketten naar. In ons scenario *192.168.2.0/24*.
    * **--volgende hoptype**. Type object verkeer ontvangt. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, of *geen*.
    * **--volgende hop-ip-adressen**. IP-adres voor de volgende hop. In ons scenario *192.168.0.4*.

3. Voer de [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) opdracht voor het koppelen van de routetabel die eerder is gemaakt met de **FrontEnd** subnet:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    Uitvoer:

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    Parameters:
    
    * **--vnet naam**. De naam van de VNet waar het subnet bevindt. In ons scenario *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken

Voor het maken van de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het bovenstaande scenario, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht voor het maken van een routetabel voor de back-end-subnet:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het front-end-subnet (192.168.1.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel met de **back-end** subnet:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Doorsturen via IP op FW1 inschakelen

Om in te schakelen doorsturen via IP in de NIC die wordt gebruikt door **FW1**, de volgende stappen uit:

1. Voer de [az netwerk nic weergeven](/cli/azure/network/nic#show) opdracht met een filter JMESPATH om weer te geven van de huidige **enable--doorsturen via ip** waarde voor **doorsturen via IP inschakelen**. Moet worden ingesteld op *false*.

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    Uitvoer:

        false

2. Voer de volgende opdracht doorsturen via IP inschakelen:

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    Bekijk de uitvoer naar de console gestreamd of alleen testen voor de specifieke **enableIpForwarding** waarde:

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    Uitvoer:

        true

    Parameters:

    **---doorsturen via ip**: *true* of *false*.

