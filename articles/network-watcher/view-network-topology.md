---
title: Met Azure virtual network-topologie weergeven | Microsoft Docs
description: Leer hoe u om de resources in een virtueel netwerk en de relaties tussen de resources weer te geven.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: jdial
ms.openlocfilehash: 1725a3d6a4eb82ca57078f648efa14866d2fe390
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2018
ms.locfileid: "35916337"
---
# <a name="view-the-topology-of-an-azure-virtual-network"></a>De topologie van een Azure-netwerk weergeven

In dit artikel leert u hoe u resources weergeven in een virtueel netwerk van Microsoft Azure, en de relaties tussen de resources. Bijvoorbeeld, bevat een virtueel netwerk subnetten. Subnetten bevatten resources, zoals Azure Virtual Machines (VM). Virtuele machines hebben een of meer netwerkinterfaces. Elk subnet kan zijn voor een netwerkbeveiligingsgroep en een routetabel die zijn gekoppeld. De mogelijkheid van de topologie van Azure Network Watcher kunt u alle resources in een virtueel netwerk, de resources weergeven die zijn gekoppeld aan resources in een virtueel netwerk en de relaties tussen de resources.

U kunt de [Azure-portal](#azure-portal), wordt de [Azure CLI](#azure-cli), of [PowerShell](#powershell) om een topologie weer te geven.

## <a name = "azure-portal"></a>Topologie weergeven - Azure portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account met de benodigde [machtigingen](required-rbac-permissions.md).
2. Bovenaan op de linkerbovenhoek van de portal, selecteer **alle services**.
3. In de **alle services** filter vak, voer *Network Watcher*. Selecteer **Network Watcher** in de resultaten.
4. Selecteer **topologie**. Genereren van een topologie vereist een network watcher in dezelfde regio die het virtuele netwerk dat u wilt genereren van de topologie voor deel uitmaakt. Als u geen een network watcher ingeschakeld in de regio waarin het virtuele netwerk dat u wilt genereren van een topologie voor zich bevindt, worden netwerk-watchers automatisch voor u gemaakt in alle regio's. De netwerk-watchers zijn gemaakt in een resourcegroep met de naam **NetworkWatcherRG**.
5. Selecteer een abonnement, de resourcegroep van een virtueel netwerk dat u wilt weergeven van de topologie voor, en selecteer vervolgens het virtuele netwerk. In de volgende afbeelding, een topologie wordt weergegeven voor een virtueel netwerk met de naam *MyVnet*, in de resourcegroep met de naam *MyResourceGroup*:

    ![Topologie weergeven](./media/view-network-topology/view-topology.png)

    Zoals u in de vorige afbeelding ziet, bevat het virtuele netwerk drie subnetten. Eén subnet heeft een VM die erin zijn geïmplementeerd. De virtuele machine heeft een netwerkinterface die is gekoppeld aan deze en een openbaar IP-adres zijn gekoppeld. De andere twee subnetten hebben een routetabel gekoppeld aan. Elke routetabel bevat twee routes. Een subnet heeft een netwerkbeveiligingsgroep zijn gekoppeld. Informatie over de topologie wordt alleen weergegeven voor resources die zijn:
    
    - Binnen de dezelfde resourcegroep en regio bevinden als de *myVnet* virtueel netwerk. Bijvoorbeeld, een netwerkbeveiligingsgroep die deel uitmaakt van een resourcegroep dan *MyResourceGroup*, niet wordt weergegeven, zelfs als de netwerkbeveiligingsgroep is gekoppeld aan een subnet in de *MyVnet* virtueel netwerk .
    - In het geval is, of gekoppeld aan resources binnen de *myVnet* virtueel netwerk. Bijvoorbeeld, een netwerkbeveiligingsgroep die niet is gekoppeld aan een subnet of netwerkinterface in de *myVnet* virtueel netwerk wordt niet weergegeven, zelfs als de netwerkbeveiligingsgroep in de *MyResourceGroup* resourcegroep.

  De topologie die wordt weergegeven in de afbeelding is voor het virtuele netwerk gemaakt na de implementatie van de **routeren van verkeer via een netwerk virtueel apparaat-voorbeeldscript**, die u kunt implementeren met behulp van de [Azure CLI](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), of [PowerShell](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

6. Selecteer **topologie downloaden** voor het downloaden van de afbeelding als een bestand worden bewerkt in SVG-indeling.

De resources die worden weergegeven in het diagram vormen een subset van de netwerkonderdelen in het virtuele netwerk. Bijvoorbeeld, terwijl een netwerkbeveiligingsgroep wordt weergegeven, worden de beveiligingsregels in het niet weergegeven in het diagram. Hoewel het niet allemaal een andere in het diagram, de regels vertegenwoordigen een van twee relaties: *Containment* of *die zijn gekoppeld*. De volledige lijst van resources in het virtuele netwerk en het type van de relatie tussen de bronnen wilt bekijken, genereren de topologie met [PowerShell](#powershell) of de [Azure CLI](#azure-cli).

## <a name = "azure-cli"></a>Topologie - Azure CLI bekijken

U kunt de opdrachten in de volgende stappen uitvoeren:
- In de Azure Cloud Shell, door te selecteren **uitproberen** aan de bovenkant van elke opdracht. De Azure Cloud Shell is een gratis interactieve shell die algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account heeft.
- Door het uitvoeren van de CLI van de computer. Als u de CLI vanaf uw computer uitvoert, de stappen in dit artikel Azure CLI versie 2.0.31 vereisen of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u gebruikt ze beschikken over de benodigde [machtigingen](required-rbac-permissions.md).

1. Als u al een network watcher in dezelfde regio als het virtuele netwerk dat u wilt maken van een topologie voor hebt, gaat u naar stap 3. Maak een resourcegroep om een network watcher met [az-groep maken](/cli/azure/group#az_group_create). Het volgende voorbeeld wordt de resourcegroep in de *eastus* regio:

    ```azurecli-interactive
    az group create --name NetworkWatcherRG --location eastus
    ```

2. Maken van een network watcher met [az network watcher configureren](/cli/azure/network/watcher#az-network-watcher-configure). Het volgende voorbeeld wordt een network watcher in de *eastus* regio:

    ```azurecli-interactive
    az network watcher configure \
      --resource-group NetworkWatcherRG \
      --location eastus \
      --enabled true
    ```

3. Weergeven van de topologie met [az network watcher show-topologie](/cli/azure/network/watcher#az-network-watcher-show-topology). Het volgende voorbeeld bekijkt de topologie voor een resourcegroep met de naam *MyResourceGroup*:

    ```azurecli-interactive
    az network watcher show-topology --resource-group MyResourceGroup
    ```

    Informatie over de topologie wordt alleen geretourneerd voor resources die zich in dezelfde resourcegroep bevinden als de *MyResourceGroup* resourcegroep en dezelfde regio als de netwerk-watcher. Bijvoorbeeld, een netwerkbeveiligingsgroep die deel uitmaakt van een resourcegroep dan *MyResourceGroup*, niet wordt weergegeven, zelfs als de netwerkbeveiligingsgroep is gekoppeld aan een subnet in de *MyVnet* virtueel netwerk .

  Meer informatie over de [relaties](#relationhips) en [eigenschappen](#properties) in de resulterende uitvoer. Als u een bestaand virtueel netwerk om weer te geven van een topologie voor geen hebt, kunt u maken met behulp van één de [verkeer routeren via een virtueel netwerkapparaat](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) voorbeeldscript. Een diagram van de topologie weergeven en downloaden in een bestand bewerkt, gebruikt u de [portal](#azure-portal).

## <a name = "powershell"></a>Topologie weergeven - PowerShell

U kunt de opdrachten in de volgende stappen uitvoeren:
- In de Azure Cloud Shell, door te selecteren **uitproberen** aan de bovenkant van elke opdracht. De Azure Cloud Shell is een gratis interactieve shell die algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account heeft.
- Door te voeren PowerShell vanaf uw computer. Als u PowerShell vanaf uw computer uitvoeren, de stappen in dit artikel is versie 5.7.0 vereist of hoger van de AzureRm-module. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

Het account dat u gebruikt ze beschikken over de benodigde [machtigingen](required-rbac-permissions.md).

1. Als u al een network watcher in dezelfde regio als het virtuele netwerk dat u wilt maken van een topologie voor hebt, gaat u naar stap 3. Maak een resourcegroep om een network watcher met [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Het volgende voorbeeld wordt de resourcegroep in de *eastus* regio:

    ```azurepowershell-interactive
    New-AzureRmResourceGroup -Name NetworkWatcherRG -Location EastUS
    ```

2. Maken van een network watcher met [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher). Het volgende voorbeeld wordt een network watcher in de regio VS-Oost:

    ```azurepowershell-interactive
    New-AzureRmNetworkWatcher `
      -Name NetworkWatcher_eastus `
      -ResourceGroupName NetworkWatcherRG
    ```

3. Ophalen van een Network Watcher-exemplaar met [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher). Het volgende voorbeeld wordt een network watcher in de regio VS-Oost:

    ```azurepowershell-interactive
    $nw = Get-AzurermResource `
      | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "EastUS" }
    $networkWatcher = Get-AzureRmNetworkWatcher `
      -Name $nw.Name `
      -ResourceGroupName $nw.ResourceGroupName
    ```

4. Ophalen van een topologie met [Get-AzureRmNetworkWatcherTopology](/powershell/module/azurerm.network/get-azurermnetworkwatchertopology). Het volgende voorbeeld wordt een topologie voor een virtueel netwerk in de resourcegroep met de naam *MyResourceGroup*:

    ```azurepowershell-interactive
    Get-AzureRmNetworkWatcherTopology `
      -NetworkWatcher $networkWatcher `
      -TargetResourceGroupName MyResourceGroup
    ```

   Informatie over de topologie wordt alleen geretourneerd voor resources die zich in dezelfde resourcegroep bevinden als de *MyResourceGroup* resourcegroep en dezelfde regio als de netwerk-watcher. Bijvoorbeeld, een netwerkbeveiligingsgroep die deel uitmaakt van een resourcegroep dan *MyResourceGroup*, niet wordt weergegeven, zelfs als de netwerkbeveiligingsgroep is gekoppeld aan een subnet in de *MyVnet* virtueel netwerk .

  Meer informatie over de [relaties](#relationhips) en [eigenschappen](#properties) in de resulterende uitvoer. Als u een bestaand virtueel netwerk om weer te geven van een topologie voor geen hebt, kunt u maken met behulp van één de [verkeer routeren via een virtueel netwerkapparaat](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) voorbeeldscript. Een diagram van de topologie weergeven en downloaden in een bestand bewerkt, gebruikt u de [portal](#azure-portal).

## <a name="relationships"></a>Relaties

Alle resources die worden geretourneerd in een topologie met hebben een van de volgende typen relatie met een andere resource:

| Relatietype | Voorbeeld                                                                                                |
| ---               | ---                                                                                                    |
| Containment       | Een virtueel netwerk bevat een subnet. Een subnet bevat een netwerkinterface.                            |
| Dat is gekoppeld        | Een netwerkinterface is gekoppeld aan een virtuele machine. Een openbaar IP-adres is gekoppeld aan een netwerkinterface. |

## <a name="properties"></a>Eigenschappen

Alle resources die worden geretourneerd in een topologie met hebben de volgende eigenschappen:

- **Naam**: de naam van de resource
- **Id**: de URI van de resource.
- **Locatie**: de Azure-regio die de resource deel uitmaakt.
- **Koppelingen**: een lijst met koppelingen naar het object waarnaar wordt verwezen. Elke koppeling heeft de volgende eigenschappen:
    - **AssociationType**: verwijst naar de relatie tussen het onderliggende object en het bovenliggende item. Geldige waarden zijn *bevat* of *gekoppelde*.
    - **Naam**: de naam van de bron waarnaar wordt verwezen.
    - **ResourceId**:-de URI van de resource waarnaar wordt verwezen in de koppeling.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [een netwerkprobleem-verkeersfilter vaststellen naar of van een virtuele machine](diagnose-vm-network-traffic-filtering-problem.md) mogelijkheden met behulp van Network Watcher-IP-stroom controleren
- Meer informatie over het [een netwerkprobleem voor het routeren van verkeer van een virtuele machine vaststellen](diagnose-vm-network-routing-problem.md) met behulp van de volgende hop-mogelijkheden van Network Watcher
