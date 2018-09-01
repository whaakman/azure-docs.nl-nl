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
ms.openlocfilehash: 7ef57960b81e13eefebfab4430eec4db0c1eb2e8
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344890"
---
# <a name="create-change-or-delete-a-route-table"></a>Maken, wijzigen of verwijderen van een routetabel

Azure automatisch verkeer routeert tussen Azure-subnetten, virtuele netwerken en on-premises netwerken. Als u wijzigen van het Azure-standaard routering wilt, kunt u dat doen door het maken van een routetabel. Als u geen ervaring met routering in virtuele netwerken, kunt u meer informatie over het in de [overzicht routering](virtual-networks-udr-overview.md) of door te voeren een [zelfstudie](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-route-table"></a>Een routetabel maken

Er is een limiet aan het aantal routetabellen u per Azure-locatie en abonnement kunt maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Selecteer in de linkerbovenhoek van de portal **+ een resource maken**.
2. Selecteer **netwerken**en selecteer vervolgens **routetabel**.
3. Voer een **naam** voor de routetabel, selecteert u uw **abonnement**, maakt u een nieuw **resourcegroep**, of Selecteer een bestaande resourcegroep, selecteert u een **locatie** en selecteer vervolgens **maken**. Als u van plan bent om te koppelen van de routetabel aan een subnet in een virtueel netwerk dat is verbonden met uw on-premises netwerk via een VPN-gateway en u uitschakelen **doorgifte van BGP-route**, uw on-premises routes worden niet doorgegeven aan de netwerkinterfaces in het subnet.

**Opdrachten**

- Azure CLI: [az network route-table maken](/cli/azure/network/route-table/route#az_network_route_table_create)
- PowerShell: [nieuwe AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)

## <a name="view-route-tables"></a>Routetabellen weergeven

Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze. De routetabellen die zijn opgenomen in uw abonnement worden weergegeven.

**Opdrachten**

- Azure CLI: [az network route-table list](/cli/azure/network/route-table/route#az_network_route_table_list)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="view-details-of-a-route-table"></a>Details van een routetabel weergeven

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de routetabel in de lijst die u wilt weergeven van details voor. Onder **instellingen**, vindt u de **Routes** in de routetabel en het **subnetten** de routetabel is gekoppeld aan.
3. Zie voor meer informatie over algemene Azure-instellingen, de volgende informatie:
    *   [Activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

- Azure CLI: [az network route-table show](/cli/azure/network/route-table/route#az_network_route_table_show)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="change-a-route-table"></a>Een routetabel wijzigen

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de routetabel die u wilt wijzigen. De meest voorkomende wijzigingen zijn [toe te voegen](#create-a-route) of [verwijderen](#delete-a-route) routes en [koppelen](#associate-a-route-table-to-a-subnet) routetabellen, of [ontkoppelen](#dissociate-a-route-table-from-a-subnet) routetabellen uit subnetten.

**Opdrachten**

- Azure CLI: [az network route-table update](/cli/azure/network/route-table/route#az_network_route_table_update)
- PowerShell: [Set-AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Een subnet kan nul of één routetabel die is gekoppeld aan deze hebben. Een routetabel kan worden gekoppeld aan nul of meerdere subnetten. Aangezien routetabellen niet gekoppeld aan virtuele netwerken zijn, moet u een routetabel aan elk subnet dat u wilt dat de tabel van de route die is gekoppeld aan koppelen. Al het verkeer het subnet verlaat wordt doorgestuurd op basis van routes die u hebt gemaakt in routetabellen, [standaardsysteemroutes](virtual-networks-udr-overview.md#default), en routes doorgegeven vanuit een on-premises netwerk, als het virtuele netwerk is verbonden met een virtueel Azure-netwerk ()-gateway ExpressRoute of VPN-, als het gebruik van BGP met een VPN-gateway). U kunt alleen een routetabel aan subnetten in virtuele netwerken die zijn opgenomen in de dezelfde Azure-locatie en abonnement als de routetabel koppelen.

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer het virtuele netwerk in de lijst met het subnet dat u wilt een routetabel aan koppelen.
3. Selecteer **subnetten** onder **instellingen**.
4. Selecteer het subnet dat u wilt de routetabel te koppelen.
5. Selecteer **routetabel**, selecteert u de routetabel die u wilt koppelen aan het subnet, en selecteer vervolgens **opslaan**.

Als uw virtuele netwerk is verbonden met een Azure VPN-gateway, koppelt u geen routetabel aan het [gatewaysubnet](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) met een route die als bestemming heeft: 0.0.0.0/0. Als u dit wel doet, functioneert de gateway mogelijk niet juist. Zie voor meer informatie over het gebruik van 0.0.0.0/0 in een route [routering van verkeer van virtuele netwerken](virtual-networks-udr-overview.md#default-route).

**Opdrachten**

- Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Een routetabel van een subnet ontkoppelen

Wanneer u een routetabel van een subnet ontkoppelen, stuurt Azure verkeer op basis van de [standaardsysteemroutes](virtual-networks-udr-overview.md#default).

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer het virtuele netwerk waarin het subnet dat u wilt ontkoppelen van een routetabel uit.
3. Selecteer **subnetten** onder **instellingen**.
4. Selecteer het subnet dat u wilt ontkoppelen van de routetabel van.
5. Selecteer **routetabel**, selecteer **geen**en selecteer vervolgens **opslaan**.

**Opdrachten**

- Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) 

## <a name="delete-a-route-table"></a>Een routetabel verwijderen

Als u een routetabel is gekoppeld aan subnetten, kan niet worden verwijderd. [Ontkoppelen](#dissociate-a-route-table-from-a-subnet) een routetabel van alle subnetten voordat u probeert te verwijderen.

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer **...**  aan de rechterkant van de routetabel die u wilt verwijderen.
3. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az network route-table verwijderen](/cli/azure/network/route-table/route#az_network_route_table_delete)
- PowerShell: [Remove-AzureRmRouteTable](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermroutetable?view=azurermps-6.8.1) 

## <a name="create-a-route"></a>Een route maken

Er is een limiet aan het aantal routes per routetabel per Azure-locatie en abonnement kunnen maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de routetabel in de lijst die u wilt toevoegen een route naar.
3. Selecteer **Routes**onder **instellingen**.
4. Selecteer **+ Toevoegen**.
5. Voer een unieke **naam** voor de route in de routetabel.
6. Voer de **adresvoorvoegsel**, in CIDR-notatie, waarop u wilt dat verkeer naar routeert. Het voorvoegsel kan niet worden gedupliceerd in meer dan één route in de routetabel, hoewel het voorvoegsel kan zich in een ander voorvoegsel. Als u 10.0.0.0/16 gedefinieerd als een voorvoegsel in één route, kunt u bijvoorbeeld nog steeds een andere route met het adresvoorvoegsel 10.0.0.0/24 definiëren. Azure selecteert een route voor verkeer op basis van de langste voorvoegselovereenkomst. Zie voor meer informatie over hoe Azure routes selecteert, [routeringoverzicht](virtual-networks-udr-overview.md#how-azure-selects-a-route).
7. Selecteer een **volgende hoptype**. Zie voor een gedetailleerde beschrijving van alle volgende hoptypen [routeringoverzicht](virtual-networks-udr-overview.md).
8. Voer een IP-adres voor **adres van volgende hop**. U kunt alleen een adres invoeren als u hebt geselecteerd *virtueel apparaat* voor **volgende hoptype**.
9. Selecteer **OK**.

**Opdrachten**

- Azure CLI: [az network route-table route maken](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_create)
- PowerShell: [New-AzureRmRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)

## <a name="view-routes"></a>Routes weergeven

Een routetabel bevat nul of meerdere routes. Zie voor meer informatie over de gegevens die worden vermeld bij het weergeven van routes, [routeringoverzicht](virtual-networks-udr-overview.md).

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de routetabel in de lijst die u wilt weergeven van routes voor.
3. Selecteer **Routes** onder **instellingen**.

**Opdrachten**

- Azure CLI: [az network route-table route list](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_list)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="view-details-of-a-route"></a>Details van een route weergeven

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de gewenste details wilt weergeven van een route voor de routetabel.
3. Selecteer **Routes**.
4. Selecteer de route die u wilt weergeven van details van.

**Opdrachten**

- Azure CLI: [az network route-table route show](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_show)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="change-a-route"></a>Een route wijzigen

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de routetabel die u wilt een route voor wijzigen.
3. Selecteer **Routes**.
4. Selecteer de route die u wilt wijzigen.
5. Bestaande instellingen wijzigen met de nieuwe instellingen en selecteer vervolgens **opslaan**.

**Opdrachten**

- Azure CLI: [az network route-table route update](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_update)
- PowerShell: [Set-AzureRmRouteConfig](/powershell/module/azurerm.network/set-azurermrouteconfig)

## <a name="delete-a-route"></a>Een route verwijderen

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de routetabel die u wilt een route voor verwijderen.
3. Selecteer **Routes**.
4. Selecteer in de lijst met routes **...**  aan de rechterkant van de route die u wilt verwijderen.
5. Selecteer **verwijderen**en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az network route-table route delete](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_delete)
- PowerShell: [Remove-AzureRmRouteConfig](/powershell/module/azurerm.network/remove-azurermrouteconfig)

## <a name="view-effective-routes"></a>Effectieve routes weergeven

De effectieve routes voor elke netwerkinterface die is gekoppeld aan een virtuele machine zijn een combinatie van routetabellen die u hebt gemaakt, de standaardroutes van Azure en routes doorgegeven van on-premises netwerken via BGP via de gateway van een Azure-netwerk. Informatie over de effectieve routes voor een netwerkinterface is handig bij het oplossen van problemen met de routering. U kunt de effectieve routes voor elke netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer de naam van een virtuele machine die u wilt weergeven van effectieve routes voor in het zoekvak boven aan de portal. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak in. Wanneer **virtuele machines** worden weergegeven in de lijst met zoekresultaten, selecteert u deze en selecteer een virtuele machine in de lijst.
2. Selecteer **netwerken** onder **instellingen**.
3. Selecteer de naam van een netwerkinterface.
4. Selecteer **effectieve routes** onder **ondersteuning + probleemoplossing**.
5. Bekijk de lijst met effectieve routes om te bepalen of de juiste route bestaat voor waar u het routeren van verkeer op. Meer informatie over de volgende hoptypen die u in de lijst in het ziet [routeringoverzicht](virtual-networks-udr-overview.md).

**Opdrachten**

- Azure CLI: [az network nic show-effectief-route-table](/cli/azure/network/nic?view=azure-cli-latest#az_network_nic_show_effective_route_table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) 

## <a name="validate-routing-between-two-endpoints"></a>Routering tussen de twee eindpunten valideren

Het volgende hoptype tussen een virtuele machine en het IP-adres van een andere Azure-resource, een on-premises resource of een resource op het Internet, kunt u bepalen. Bepalen van het Azure-routering is nuttig bij het oplossen van problemen met de routering. Als u wilt deze taak hebt voltooid, moet u een bestaande Network Watcher hebben. Als u een bestaande Network Watcher hebt, maakt u een via de stappen in [maken van een exemplaar van Network Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Voer in het zoekvak boven aan de portal, *netwerk-watcher* in het zoekvak in. Selecteer **Network Watcher** in de zoekresultaten.
2. Selecteer **van volgende hop** onder **diagnostische hulpprogramma's voor NETWORK**.
3. Selecteer uw **abonnement** en de **resourcegroep** van de virtuele machine die u wilt valideren routering van.
4. Selecteer de **virtuele machine**, **netwerkinterface** gekoppeld aan de virtuele machine en **bron-IP-adres** toegewezen aan de netwerkinterface die u wilt valideren Routering van.
5. Voer de **doel-IP-adres** die u wilt valideren routering naar.
6. Selecteer **van volgende hop**.
7. Na enkele ogenblikken wordt informatie geretourneerd waarin staat u het volgende hoptype en de ID van de route die het verkeer gerouteerd. Meer informatie over de volgende hoptypen die u ziet in geretourneerd [routeringoverzicht](virtual-networks-udr-overview.md).

**Opdrachten**

- Azure CLI: [az network watcher show volgende hop](/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_show_next_hop)
- PowerShell: [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) 

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op routetabellen en routes, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

| Bewerking                                                          |   Naam                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Een routetabel lezen                                    |
| Microsoft.Network/routeTables/write                             |   Maken of bijwerken van een routetabel                        |
| Microsoft.Network/routeTables/delete                            |   Een routetabel verwijderen                                  |
| Microsoft.Network/routeTables/join/action                       |   Een routetabel aan een subnet koppelen                   |
| Microsoft.Network/routeTables/routes/read                       |   Een route lezen                                          |
| Microsoft.Network/routeTables/routes/write                      |   Maken of bijwerken van een route                              |
| Microsoft.Network/routeTables/routes/delete                     |   Een route verwijderen                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   De tabel effectieve route voor een netwerkinterface ophalen |
| Microsoft.Network/networkWatchers/nextHop/action                |   De volgende hop van een virtuele machine opgehaald                           |

## <a name="next-steps"></a>Volgende stappen

- Maak een route tabel met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken
