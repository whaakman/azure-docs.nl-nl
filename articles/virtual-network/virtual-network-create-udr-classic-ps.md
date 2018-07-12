---
title: Beheren van routering in een Azure Virtual Network - PowerShell - klassiek | Microsoft Docs
description: Meer informatie over het beheren van routering in vnet's met behulp van PowerShell | Klassiek
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: 930676a396ae316ec761ba5d03ad1a1d0fd7a425
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232563"
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Beheren van Routering en het gebruik van virtuele apparaten (klassiek) met behulp van PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](tutorial-create-route-table-powershell.md)
> * [Azure-CLI](tutorial-create-route-table-cli.md)
> * [PowerShell (klassiek)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassiek)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> Voordat u met Azure-resources gaat werken, is het belangrijk om te weten dat Azure momenteel twee implementatiemodellen heeft: Azure Resource Manager en het klassieke model. Zorg ervoor dat u begrijpt wat [implementatiemodellen en hulpprogramma's](../azure-resource-manager/resource-manager-deployment-model.md) zijn voordat u met een Azure-resource gaat werken. U kunt de documentatie voor verschillende hulpprogramma's bekijken door een optie aan de bovenkant van dit artikel te selecteren. Dit artikel is van toepassing op het klassieke implementatiemodel.
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het voorbeeld Azure PowerShell onderstaande opdrachten kunt verwachten van een eenvoudige omgeving al gemaakt op basis van bovenstaand scenario. Als u wilt dat de opdrachten uitvoeren zoals ze worden weergegeven in dit document, maakt u de omgeving wordt weergegeven in [maken van een VNet (klassiek) met behulp van PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor het front-end-subnet maken
Voor het maken van de routetabel en route die nodig zijn voor de front-end-subnet op basis van de bovenstaande scenario, de volgende stappen uit te voeren.

1. Voer de volgende opdracht om te maken van een routetabel voor de front-end-subnet:

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. Voer de volgende opdracht om te maken van een route in de routetabel voor het verzenden van al het verkeer dat bestemd is voor de back-end-subnet (192.168.2.0/24) naar de **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel, met de **FrontEnd** subnet:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor het back-end-subnet maken
Voor het maken van de routetabel en route die nodig zijn voor de back-end-subnet op basis van het scenario, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht om te maken van een routetabel voor de back-end-subnet:

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. Voer de volgende opdracht om te maken van een route in de routetabel voor het verzenden van al het verkeer dat bestemd is voor de front-end-subnet (192.168.1.0/24) naar de **FW1** VM (192.168.0.4):

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel, met de **back-end** subnet:

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Doorsturen via IP op de VM FW1 inschakelen

Als u wilt doorsturen via in de VM FW1 IP inschakelen, voert u de volgende stappen uit:

1. Voer de volgende opdracht uit om te controleren of de status van doorsturen via IP:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. Voer de volgende opdracht uit om doorsturen via IP voor de *FW1* VM:

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
