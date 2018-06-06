---
title: Maken, wijzigen of verwijderen van een Azure routetabel | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een routetabel.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 93ecd0264413e0eb719c9d33f0a0b756bcee6552
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726445"
---
# <a name="create-change-or-delete-a-route-table"></a>Maken, wijzigen of verwijderen van een routetabel

Azure automatisch routeert netwerkverkeer tussen Azure subnetten, virtuele netwerken en on-premises netwerken. Als u wijzigen van Azure standaard routering wilt, kunt u dat doen door het maken van een routetabel. Als u geen ervaring met routering in virtuele netwerken, kunt u meer informatie over het in de [Routering overzicht](virtual-networks-udr-overview.md) of door te voeren een [zelfstudie](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure met, moet worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-route-table"></a>Een routetabel maken

Er is een limiet aan hoeveel routetabellen per Azure-locatie en abonnement kunt u. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Selecteer in de linkerbovenhoek van de portal **+ maken van een resource**.
2. Selecteer **Networking**, selecteer daarna **routetabel**.
3. Voer een **naam** voor de routetabel, selecteert u uw **abonnement**, maak een nieuwe **resourcegroep**, of Selecteer een bestaande resourcegroep, selecteer een **locatie** , selecteer daarna **maken**. De **uitschakelen BGP route doorgeven** optie kiest, kunnen lokale routes van via BGP wordt doorgegeven aan de netwerkinterfaces in elk subnet dat de routetabel is gekoppeld. Als het virtuele netwerk niet is verbonden met de gateway van een Azure-netwerk (VPN- of ExpressRoute), laat u de optie *uitgeschakelde*.

**Opdrachten**

- Azure CLI: [az netwerk routetabel maken](/cli/azure/network/route-table/route#az_network_route_table_create)
- PowerShell: [nieuwe AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)

## <a name="view-route-tables"></a>De routetabellen weergeven

Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze. De routetabellen die zijn opgenomen in uw abonnement worden weergegeven.

**Opdrachten**

- Azure CLI: [az netwerk routetabel lijst](/cli/azure/network/route-table/route#az_network_route_table_list)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="view-details-of-a-route-table"></a>Details weergeven van een routetabel

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de routetabel in de lijst die u wilt bekijken voor. Onder **instellingen**, vindt u de **Routes** in de routetabel en de **subnetten** de routetabel is gekoppeld aan.
3. Zie voor meer informatie over algemene instellingen voor Azure, de volgende informatie:
    *   [Activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

- Azure CLI: [az netwerk routetabel weergeven](/cli/azure/network/route-table/route#az_network_route_table_show)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="change-a-route-table"></a>Een routetabel wijzigen

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de routetabel die u wilt wijzigen. De meest voorkomende wijzigingen zijn [toe te voegen](#create-a-route) of [verwijderen](#delete-a-route) routes en [koppelen](#associate-a-route-table-to-a-subnet) routetabellen, of [dissociating](#dissociate-a-route-table-from-a-subnet) routetabellen uit subnetten.

**Opdrachten**

- Azure CLI: [az netwerk routetabel update](/cli/azure/network/route-table/route#az_network_route_table_update)
- PowerShell: [Set-AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Een subnet kan nul of één routetabel gekoppeld hebben. Een routetabel kan worden gekoppeld aan nul of meerdere subnetten. Aangezien routetabellen niet gekoppeld aan virtuele netwerken, moet u een routetabel met elk subnet dat u wilt dat de routetabel gekoppeld aan koppelen. Uitgaand van het subnet alle verkeer wordt doorgestuurd op basis van de routes die u hebt gemaakt in routetabellen, [routes standaard](virtual-networks-udr-overview.md#default), en routes doorgegeven vanuit een on-premises netwerk als het virtuele netwerk is verbonden met een gateway virtuele Azure-netwerk ( ExpressRoute of VPN als BGP met een VPN-gateway). U kunt alleen een routetabel met subnetten in virtuele netwerken die bestaan in de dezelfde Azure-locatie en het abonnement als de routetabel koppelen.

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer het virtuele netwerk in de lijst met het subnet dat u wilt een routetabel te koppelen.
3. Selecteer **subnetten** onder **instellingen**.
4. Selecteer het subnet dat u wilt de routetabel te koppelen.
5. Selecteer **routetabel**, selecteert u de routetabel die u wilt koppelen aan het subnet en selecteer vervolgens **opslaan**.

Als het virtuele netwerk is verbonden met een Azure VPN-gateway, koppel geen een routetabel aan de [gatewaysubnet](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) die een route met bestemming 0.0.0.0/0 bevat. In dat geval kan voorkomen dat de gateway niet goed werken. Zie voor meer informatie over het gebruik van 0.0.0.0/0 in een route [virtuele netwerk voor verkeersroutering](virtual-networks-udr-overview.md#default-route).

**Opdrachten**

- Azure CLI: [az network vnet subnet bijwerken](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Een routetabel van een subnet ontkoppelen

Wanneer u een routetabel van een subnet ontkoppelen, Azure routeert verkeer op basis van de [routes standaard](virtual-networks-udr-overview.md#default).

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer het virtuele netwerk waarin het subnet dat u wilt ontkoppelen van een routetabel uit.
3. Selecteer **subnetten** onder **instellingen**.
4. Selecteer het subnet dat u wilt ontkoppelen van een routetabel.
5. Selecteer **routetabel**, selecteer **geen**, selecteer daarna **opslaan**.

**Opdrachten**

- Azure CLI: [az network vnet subnet bijwerken](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) 

## <a name="delete-a-route-table"></a>Een routetabel verwijderen

Als een routetabel gekoppeld aan subnetten is, kan niet worden verwijderd. [Ontkoppelen](#dissociate-a-route-table-from-a-subnet) een routetabel van alle subnetten voordat u probeert te verwijderen.

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer **...**  aan de rechterkant van de routetabel die u wilt verwijderen.
3. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az netwerk routetabel verwijderen](/cli/azure/network/route-table/route#az_network_route_table_delete)
- PowerShell: [verwijderen AzureRmRouteTable](/powershell/module/azurerm.network/delete-azurermroutetable) 

## <a name="create-a-route"></a>Een route maken

Er is een limiet voor het aantal routes per routetabel per Azure-locatie en abonnement kunnen maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de routetabel in de lijst die u wilt toevoegen van een route naar.
3. Selecteer **Routes**onder **instellingen**.
4. Selecteer **+ Toevoegen**.
5. Voer een unieke **naam** voor de route binnen de routetabel.
6. Voer de **adresvoorvoegsel**, in CIDR-notatie, die u verkeer om te leiden wilt. Het voorvoegsel kan niet worden gedupliceerd in meer dan één route in de routetabel, hoewel het voorvoegsel op dat binnen een ander voorvoegsel kan zijn. Als u 10.0.0.0/16 gedefinieerd als een voorvoegsel in één route, kunt u bijvoorbeeld nog steeds een andere route met het adresvoorvoegsel 10.0.0.0/24 definiëren. Azure selecteert een route voor verkeer op basis van het langste voorvoegsel overeen. Zie voor meer informatie over hoe Azure routes selecteert, [Routering-overzicht](virtual-networks-udr-overview.md#how-azure-selects-a-route).
7. Selecteer een **volgende hop type**. Zie voor een gedetailleerde beschrijving van alle volgende hoptypen [Routering-overzicht](virtual-networks-udr-overview.md).
8. Geef een IP-adres voor **adres van volgende hop**. U kunt alleen een adres invoeren als u hebt geselecteerd *virtueel apparaat* voor **volgende hop type**.
9. Selecteer **OK**.

**Opdrachten**

- Azure CLI: [az routetabel netwerkroute maken](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_create)
- PowerShell: [New-AzureRmRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)

## <a name="view-routes"></a>Routes weergeven

Een routetabel bevat geen of meerdere routes. Zie voor meer informatie over welke gegevens worden weergegeven wanneer de routes weer te geven, [Routering-overzicht](virtual-networks-udr-overview.md).

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de routetabel in de lijst die u wilt weergeven van routes voor.
3. Selecteer **Routes** onder **instellingen**.

**Opdrachten**

- Azure CLI: [az netwerk routetabel routelijst](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_list)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="view-details-of-a-route"></a>Details van een route weergeven

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de gewenste details wilt weergeven van een route voor routetabel.
3. Selecteer **Routes**.
4. Selecteer de gewenste details wilt weergeven van route.

**Opdrachten**

- Azure CLI: [az netwerk routetabel route weergeven](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_show)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="change-a-route"></a>Een route wijzigen

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de routetabel die u wilt wijzigen van een route voor.
3. Selecteer **Routes**.
4. Selecteer de route die u wilt wijzigen.
5. Bestaande instellingen op de nieuwe instellingen te wijzigen en selecteer vervolgens **opslaan**.

**Opdrachten**

- Azure CLI: [az netwerk routetabel route bijwerken](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_update)
- PowerShell: [Set-AzureRmRouteConfig](/powershell/module/azurerm.network/set-azurermrouteconfig)

## <a name="delete-a-route"></a>Een route verwijderen

1. Voer in het zoekvak boven aan de portal *routetabellen* in het zoekvak. Wanneer **routetabellen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de routetabel die u wilt verwijderen van een route voor.
3. Selecteer **Routes**.
4. Selecteer in de lijst met routes **...**  aan de rechterkant van de route die u wilt verwijderen.
5. Selecteer **verwijderen**, selecteer daarna **Ja**.

**Opdrachten**

- Azure CLI: [az netwerk routetabel route verwijderen](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_delete)
- PowerShell: [Remove-AzureRmRouteConfig](/powershell/module/azurerm.network/remove-azurermrouteconfig)

## <a name="view-effective-routes"></a>De effectieve routes weergeven

De effectieve routes voor elke netwerkinterface die is gekoppeld aan een virtuele machine zijn een combinatie van routetabellen die u hebt gemaakt, Azure standaardroutes en eventuele routes doorgegeven vanuit de on-premises netwerken via BGP via een gateway virtuele Azure-netwerk. Inzicht in de effectieve routes voor een netwerkinterface is handig bij het oplossen van problemen met de routering. U kunt de effectieve routes voor een netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer de naam van een virtuele machine die u wilt weergeven van effectieve routes voor in het zoekvak boven aan de portal. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak. Wanneer **virtuele machines** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze en selecteer een virtuele machine in de lijst.
2. Selecteer **Networking** onder **instellingen**.
3. Selecteer de naam van een netwerkinterface.
4. Selecteer **effectieve routes** onder **ondersteuning + probleemoplossing**.
5. Bekijk de lijst met effectieve routes om te bepalen of de juiste route bestaat voor waar u verkeer te routeren. Meer informatie over de volgende hoptypen die u in de lijst in het ziet [Routering-overzicht](virtual-networks-udr-overview.md).

**Opdrachten**

- Azure CLI: [az netwerk nic-ingang-route-tabel weergeven](/cli/azure/network/nic?view=azure-cli-latest#az_network_nic_show_effective_route_table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/remove-azurermrouteconfig) 

## <a name="validate-routing-between-two-endpoints"></a>Routering tussen de twee eindpunten valideren

Het volgende hoptype tussen een virtuele machine en het IP-adres van een andere Azure-resource, een on-premises resource of een bron op het Internet, kunt u bepalen. Bepalende Azure routering is handig bij het oplossen van problemen met de routering. Als u wilt deze taak uitvoeren, moet u een bestaande netwerk-Watcher hebben. Als u een bestaande netwerk-Watcher niet hebt, maken door de stappen in [maken van een exemplaar van de netwerk-Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Voer in het zoekvak boven aan de portal *netwerk-watcher* in het zoekvak. Selecteer **Network Watcher** in de zoekresultaten.
2. Selecteer **volgende hop** onder **NETWERKDIAGNOSEHULPMIDDELEN**.
3. Selecteer uw **abonnement** en de **resourcegroep** van de virtuele bronmachine die u wilt valideren van routering.
4. Selecteer de **virtuele machine**, **netwerkinterface** gekoppeld aan de virtuele machine en **IP-bronadres** toegewezen aan de netwerkinterface die u wilt valideren Routering van.
5. Voer de **doel-IP-adres** die u wilt valideren routering naar.
6. Selecteer **volgende hop**.
7. Na een korte wachten, wordt informatie geretourneerd die leest u het volgende hoptype en de ID van de route die het verkeer wordt gerouteerd. Meer informatie over de volgende hoptypen die u ziet in geretourneerd [Routering-overzicht](virtual-networks-udr-overview.md).

**Opdrachten**

- Azure CLI: [az netwerk-watcher weergeven volgende hop](/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_show_next_hop)
- PowerShell: [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) 

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op routetabellen en routes, moet uw account worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

| Bewerking                                                          |   Naam                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Een routetabel lezen                                    |
| Microsoft.Network/routeTables/write                             |   Maken of bijwerken van een routetabel                        |
| Microsoft.Network/routeTables/delete                            |   Een routetabel verwijderen                                  |
| Microsoft.Network/routeTables/join/action                       |   Een routetabel aan een subnet koppelen                   |
| Microsoft.Network/routeTables/routes/read                       |   Een route lezen                                          |
| Microsoft.Network/routeTables/routes/write                      |   Een route maken of bijwerken                              |
| Microsoft.Network/routeTables/routes/delete                     |   Een route verwijderen                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   De effectieve routetabel voor een netwerkinterface ophalen |
| Microsoft.Network/networkWatchers/nextHop/action                |   De volgende hop van een virtuele machine opgehaald                           |

## <a name="next-steps"></a>Volgende stappen

- Maak een route tabel met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) steekproef scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen van [Azure beleid](policy-samples.md) voor virtuele netwerken
