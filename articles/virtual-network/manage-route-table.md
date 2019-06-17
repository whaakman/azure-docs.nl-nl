---
title: Maken, wijzigen of verwijderen van een Azure routetabel
titlesuffix: Azure Virtual Network
description: Informatie over het maken, wijzigen of verwijderen van een routetabel.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: kumud
ms.openlocfilehash: a39d9f9c5a138ece5d40cc5afe1d1dcdd8e7a41a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65849802"
---
# <a name="create-change-or-delete-a-route-table"></a>Maken, wijzigen of verwijderen van een routetabel

Azure routeert automatisch verkeer tussen Azure-subnetten, virtuele netwerken en on-premises netwerken. Als u wijzigen van het Azure-standaard routering wilt, kunt u dat doen door het maken van een routetabel. Als u geen ervaring met routering in virtuele netwerken, kunt u meer informatie over het in de [overzicht routering](virtual-networks-udr-overview.md) of door te voeren een [zelfstudie](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

* Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).<br>
* Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.<br>
* Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. In deze zelfstudie vereist de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.<br>
* Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-route-table"></a>Een routetabel maken

Er kan maar een beperkt aantal routetabellen worden gemaakt voor elke Azure-locatie en elk Azure-abonnement. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Selecteer in de linkerbovenhoek van de portal **+ een resource maken**.
1. Selecteer **netwerken**en selecteer vervolgens **routetabel**.
1. Voer een **naam** voor de routetabel, selecteert u uw **abonnement**, maakt u een nieuw **resourcegroep**, of Selecteer een bestaande resourcegroep, selecteert u een **locatie** en selecteer vervolgens **maken**. Als u van plan bent om te koppelen van de routetabel aan een subnet in een virtueel netwerk dat is verbonden met uw on-premises netwerk via een VPN-gateway en u uitschakelen **doorgifte van Virtual network gateway route**, uw on-premises routes worden niet doorgegeven aan de netwerkinterfaces in het subnet.

### <a name="create-route-table---commands"></a>Routetabel - opdrachten maken

* Azure CLI: [az network route-table maken](/cli/azure/network/route-table/route)<br>
* PowerShell: [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)

## <a name="view-route-tables"></a>Routetabellen weergeven

Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze. De routetabellen die zijn opgenomen in uw abonnement worden weergegeven.

### <a name="view-route-table---commands"></a>Weergave routetabel - opdrachten

* Azure CLI: [az network route-table list](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="view-details-of-a-route-table"></a>Details van een routetabel weergeven

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de routetabel in de lijst die u wilt weergeven van details voor. Onder **instellingen**, vindt u de **Routes** in de routetabel en het **subnetten** de routetabel is gekoppeld aan.
1. Zie voor meer informatie over algemene Azure-instellingen, de volgende informatie:

    * [Activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md)<br>
    * [Toegangsbeheer (IAM)](../role-based-access-control/overview.md)<br>
    * [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Locks](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Automatiseringsscript](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)

### <a name="view-details-of-route-table---commands"></a>Details van de routetabel - opdrachten weergeven

* Azure CLI: [az network route-table show](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="change-a-route-table"></a>Een routetabel wijzigen

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de routetabel die u wilt wijzigen. De meest voorkomende wijzigingen zijn [toe te voegen](#create-a-route) of [verwijderen](#delete-a-route) routes en [koppelen](#associate-a-route-table-to-a-subnet) routetabellen, of [ontkoppelen](#dissociate-a-route-table-from-a-subnet) routetabellen uit subnetten.

### <a name="change-a-route-table---commands"></a>Een routetabel - opdrachten wijzigen

* Azure CLI: [az network route-table update](/cli/azure/network/route-table/route)<br>
* PowerShell: [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Een subnet kan worden gekoppeld aan geen enkele of één routetabel. Een routetabel kan worden gekoppeld aan geen enkele of meerdere subnetten. Aangezien routetabellen niet worden gekoppeld aan virtuele netwerken, moet u afzonderlijke routetabellen koppelen aan de gewenste subnetten. Al het verkeer het subnet verlaat wordt doorgestuurd op basis van routes die u hebt gemaakt in routetabellen, [standaardsysteemroutes](virtual-networks-udr-overview.md#default), en routes doorgegeven vanuit een on-premises netwerk, als het virtuele netwerk is verbonden met een virtueel Azure-netwerk ()-gateway ExpressRoute of VPN). U kunt routetabellen alleen koppelen aan subnetten in virtuele netwerken die zich op dezelfde Azure-locatie en in hetzelfde abonnement bevinden als de routetabel.

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer het virtuele netwerk in de lijst met het subnet dat u wilt een routetabel aan koppelen.
1. Selecteer **subnetten** onder **instellingen**.
1. Selecteer het subnet dat u wilt de routetabel te koppelen.
1. Selecteer **routetabel**, selecteert u de routetabel die u wilt koppelen aan het subnet, en selecteer vervolgens **opslaan**.

Als uw virtuele netwerk is verbonden met een Azure VPN-gateway, koppelt u geen routetabel aan het [gatewaysubnet](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) met een route die als bestemming heeft: 0.0.0.0/0. Als u dit wel doet, functioneert de gateway mogelijk niet juist. Zie voor meer informatie over het gebruik van 0.0.0.0/0 in een route [routering van verkeer van virtuele netwerken](virtual-networks-udr-overview.md#default-route).

### <a name="associate-a-route-table---commands"></a>Koppel een routetabel - opdrachten

* Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Een routetabel ontkoppelen van een subnet

Wanneer u een routetabel van een subnet ontkoppelen, stuurt Azure verkeer op basis van de [standaardsysteemroutes](virtual-networks-udr-overview.md#default).

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer het virtuele netwerk waarin het subnet dat u wilt ontkoppelen van een routetabel uit.
1. Selecteer **subnetten** onder **instellingen**.
1. Selecteer het subnet dat u wilt ontkoppelen van de routetabel van.
1. Selecteer **routetabel**, selecteer **geen**en selecteer vervolgens **opslaan**.

### <a name="dissociate-a-route-table---commands"></a>Ontkoppelen van een routetabel - opdrachten

* Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="delete-a-route-table"></a>Een routetabel verwijderen

Als er een routetabel is gekoppeld aan subnetten, kan deze niet worden verwijderd. [Koppel een routetabel los](#dissociate-a-route-table-from-a-subnet) van alle subnetten voordat u deze probeert te verwijderen.

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer **...**  aan de rechterkant van de routetabel die u wilt verwijderen.
1. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

### <a name="delete-a-route-table---commands"></a>Een routetabel - opdrachten verwijderen

* Azure CLI: [az network route-table verwijderen](/cli/azure/network/route-table/route)<br>
* PowerShell: [Remove-AzRouteTable](/powershell/module/az.network/remove-azroutetable)

## <a name="create-a-route"></a>Een route maken

Er is een limiet aan het aantal routes per routetabel per Azure-locatie en abonnement kunnen maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de routetabel in de lijst die u wilt toevoegen een route naar.
1. Selecteer **Routes**onder **instellingen**.
1. Selecteer **+ Toevoegen**.
1. Voer een unieke **naam** voor de route in de routetabel.
1. Voer de **adresvoorvoegsel**, in CIDR-notatie, waarop u wilt dat verkeer naar routeert. Het voorvoegsel kan niet in meer dan één route in de routetabel worden gedupliceerd, hoewel het voorvoegsel zich wel in een ander voorvoegsel kan bevinden. Als u 10.0.0.0/16 gedefinieerd als een voorvoegsel in één route, kunt u bijvoorbeeld nog steeds een andere route met het adresvoorvoegsel 10.0.0.0/24 definiëren. Azure selecteert een route voor verkeer op basis van de langste voorvoegselovereenkomst. Zie voor meer informatie over hoe Azure routes selecteert, [routeringoverzicht](virtual-networks-udr-overview.md#how-azure-selects-a-route).
1. Selecteer een **volgende hoptype**. Zie voor een gedetailleerde beschrijving van alle volgende hoptypen [routeringoverzicht](virtual-networks-udr-overview.md).
1. Voer een IP-adres voor **adres van volgende hop**. U kunt alleen een adres invoeren als u hebt geselecteerd *virtueel apparaat* voor **volgende hoptype**.
1. Selecteer **OK**.

### <a name="create-a-route---commands"></a>Een route - opdrachten maken

* Azure CLI: [az network route-table route maken](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)

## <a name="view-routes"></a>Routes weergeven

Een routetabel bevat nul of meerdere routes. Zie voor meer informatie over de gegevens die worden vermeld bij het weergeven van routes, [routeringoverzicht](virtual-networks-udr-overview.md).

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de routetabel in de lijst die u wilt weergeven van routes voor.
1. Selecteer **Routes** onder **instellingen**.

### <a name="view-routes---commands"></a>Routes weergeven - opdrachten

* Azure CLI: [az network route-table route list](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="view-details-of-a-route"></a>Details van een route weergeven

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de gewenste details wilt weergeven van een route voor de routetabel.
1. Selecteer **Routes**.
1. Selecteer de route die u wilt weergeven van details van.

### <a name="view-details-of-a-route---commands"></a>Details van een route - opdrachten weergeven

* Azure CLI: [az network route-table route show](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="change-a-route"></a>Een route wijzigen

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de routetabel die u wilt een route voor wijzigen.
1. Selecteer **Routes**.
1. Selecteer de route die u wilt wijzigen.
1. Bestaande instellingen wijzigen met de nieuwe instellingen en selecteer vervolgens **opslaan**.

### <a name="change-a-route---commands"></a>Wijzigen van een route - opdrachten

* Azure CLI: [az network route-table route update](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Set-AzRouteConfig](/powershell/module/az.network/set-azrouteconfig)

## <a name="delete-a-route"></a>Een route verwijderen

1. Voer in het zoekvak boven aan de portal, *routetabellen* in het zoekvak in. Wanneer **routetabellen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
1. Selecteer de routetabel die u wilt een route voor verwijderen.
1. Selecteer **Routes**.
1. Selecteer in de lijst met routes **...**  aan de rechterkant van de route die u wilt verwijderen.
1. Selecteer **verwijderen**en selecteer vervolgens **Ja**.

### <a name="delete-a-route---commands"></a>Een route - opdrachten verwijderen

* Azure CLI: [az network route-table route delete](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Remove-AzRouteConfig](/powershell/module/az.network/remove-azrouteconfig)

## <a name="view-effective-routes"></a>Effectieve routes weergeven

De effectieve routes voor elke netwerkinterface die is gekoppeld aan een virtuele machine zijn een combinatie van routetabellen die u hebt gemaakt, de standaardroutes van Azure en routes doorgegeven van on-premises netwerken via BGP via de gateway van een Azure-netwerk. Informatie over de effectieve routes voor een netwerkinterface is handig bij het oplossen van problemen met de routering. U kunt de effectieve routes voor elke netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer de naam van een virtuele machine die u wilt weergeven van effectieve routes voor in het zoekvak boven aan de portal. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak in. Wanneer **virtuele machines** worden weergegeven in de lijst met zoekresultaten, selecteert u deze en selecteer een virtuele machine in de lijst.
1. Selecteer **netwerken** onder **instellingen**.
1. Selecteer de naam van een netwerkinterface.
1. Selecteer **effectieve routes** onder **ondersteuning + probleemoplossing**.
1. Bekijk de lijst met effectieve routes om te bepalen of de juiste route bestaat voor waar u het routeren van verkeer op. Meer informatie over de volgende hoptypen die u in de lijst in het ziet [routeringoverzicht](virtual-networks-udr-overview.md).

### <a name="view-effective-routes---commands"></a>Effectieve routes weergeven - opdrachten

* Azure CLI: [az network nic show-effectief-route-table](/cli/azure/network/nic?view=azure-cli-latest)<br>
* PowerShell: [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)

## <a name="validate-routing-between-two-endpoints"></a>Routering tussen de twee eindpunten valideren

Het volgende hoptype tussen een virtuele machine en het IP-adres van een andere Azure-resource, een on-premises resource of een resource op het Internet, kunt u bepalen. Bepalen van het Azure-routering is nuttig bij het oplossen van problemen met de routering. Als u wilt deze taak hebt voltooid, moet u een bestaande Network Watcher hebben. Als u een bestaande Network Watcher hebt, maakt u een via de stappen in [maken van een exemplaar van Network Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Voer in het zoekvak boven aan de portal, *netwerk-watcher* in het zoekvak in. Selecteer **Network Watcher** in de zoekresultaten.
1. Selecteer **van volgende hop** onder **diagnostische hulpprogramma's voor NETWORK**.
1. Selecteer uw **abonnement** en de **resourcegroep** van de virtuele machine die u wilt valideren routering van.
1. Selecteer de **virtuele machine**, **netwerkinterface** gekoppeld aan de virtuele machine en **bron-IP-adres** toegewezen aan de netwerkinterface die u wilt valideren Routering van.
1. Voer de **doel-IP-adres** die u wilt valideren routering naar.
1. Selecteer **van volgende hop**.
1. Na enkele ogenblikken wordt informatie geretourneerd waarin staat u het volgende hoptype en de ID van de route die het verkeer gerouteerd. Meer informatie over de volgende hoptypen die u ziet in geretourneerd [routeringoverzicht](virtual-networks-udr-overview.md).

### <a name="validate-routing-between-two-endpoints---commands"></a>Routering tussen de twee eindpunten - opdrachten valideren

* Azure CLI: [az network watcher show volgende hop](/cli/azure/network/watcher?view=azure-cli-latest)<br>
* PowerShell: [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop)

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op routetabellen en routes, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

| Bewerking                                                          |   Name                                                  |
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

* Maak een route tabel met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)<br>
* Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken
