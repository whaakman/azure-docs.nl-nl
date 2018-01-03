---
title: Routering in een Azure Virtual Network - PowerShell - klassiek beheren | Microsoft Docs
description: Meer informatie over het beheren van routering in VNets met behulp van PowerShell | Klassieke
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: e9564d223cb85529f1fa97bc398d35c6debcedae
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Beheren van Routering en het gebruik van virtuele apparaten (klassiek) met behulp van PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure-CLI](virtual-network-create-udr-arm-cli.md)
> * [Sjabloon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klassiek)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassiek)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Voordat u met Azure-resources gaat werken, is het belangrijk om te weten dat Azure momenteel twee implementatiemodellen heeft: Azure Resource Manager en het klassieke model. Zorg ervoor dat u begrijpt wat [implementatiemodellen en hulpprogramma's](../azure-resource-manager/resource-manager-deployment-model.md) zijn voordat u met een Azure-resource gaat werken. U kunt de documentatie voor verschillende hulpprogramma's bekijken door een optie boven aan dit artikel te selecteren. Dit artikel is van toepassing op het klassieke implementatiemodel.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het voorbeeld Azure PowerShell onderstaande opdrachten een eenvoudige omgeving al gemaakt verwacht op basis van het bovenstaande scenario. Als u uitvoeren van de opdrachten wilt zoals ze worden weergegeven in dit document, maakt u de omgeving wordt weergegeven in [maken van een VNet (klassiek) met behulp van PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor de front-end-subnet maken
Volg de onderstaande stappen voor het maken van de routetabel en de route die nodig zijn voor de front-end-subnet op basis van de bovenstaande scenario.

1. Voer de volgende opdracht voor het maken van een routetabel voor de front-end-subnet:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het subnet voor back-end (192.168.2.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel met de **FrontEnd** subnet:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor de back-end-subnet maken
Voor het maken van de routetabel en de route die nodig zijn voor de back-end-subnet op basis van het scenario, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht voor het maken van een routetabel voor de back-end-subnet:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. Voer de volgende opdracht voor het maken van een route in de routetabel alle verkeer dat is bestemd voor het front-end-subnet (192.168.1.0/24) te verzenden naar de **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel met de **back-end** subnet:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Doorsturen via IP op de VM FW1 inschakelen

Om in te schakelen IP doorsturen in de VM FW1, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht om de status van doorsturen via IP te controleren:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. Voer de volgende opdracht om in te schakelen doorsturen via IP voor de *FW1* VM:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
