---
title: Azure virtuele netwerktopologie weergeven | Microsoft Docs
description: Informatie over het weergeven van de resources in een virtueel netwerk en de relaties tussen de bronnen.
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
ms.openlocfilehash: 6ef165ddc481bf84c6189635e36b97eb9518261e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077890"
---
# <a name="view-the-topology-of-an-azure-virtual-network"></a>De topologie van een virtuele Azure-netwerk weergeven

In dit artikel leert u hoe resources weergeven in een virtuele Microsoft Azure-netwerk en de relaties tussen de bronnen. Een virtueel netwerk bevat bijvoorbeeld subnetten. Subnetten bevatten resources, zoals Azure virtuele Machines (VM). Virtuele machines hebben een of meer netwerkinterfaces. Een netwerkbeveiligingsgroep en een routetabel die zijn gekoppeld aan hebben elk subnet. De mogelijkheid van de topologie van de netwerk-Watcher Azure kunt u alle resources in een virtueel netwerk, de resources weergeven die zijn gekoppeld aan resources in een virtueel netwerk en de relaties tussen de bronnen.

U kunt de [Azure-portal](#azure-portal), wordt de [Azure CLI](#azure-cli), of [PowerShell](#powershell) om een topologie weer te geven.

## <a name = "azure-portal"></a>Weergave-topologie - Azure-portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account met de benodigde [machtigingen](required-rbac-permissions.md).
2. Bovenaan de linkerbovenhoek van de portal, selecteert **alle services**.
3. In de **alle services** vak filteren, voer *netwerk-Watcher*. Wanneer **netwerk-Watcher** wordt weergegeven in de resultaten, selecteer deze.
4. Selecteer **topologie**. Genereren van een topologie met vereist een netwerk-watcher in dezelfde regio als het virtuele netwerk dat u wilt genereren van de topologie voor bestaat in. Als u niet ingeschakeld in de regio die het virtuele netwerk dat u wilt een topologie voor genereren hebt in een netwerk-watcher, worden netwerk watchers automatisch voor u gemaakt in alle regio's. Watchers netwerk worden gemaakt in een resourcegroep met de naam **NetworkWatcherRG**.
5. Selecteer een abonnement, de resourcegroep van een virtueel netwerk dat u wilt weergeven van de topologie voor, en selecteer vervolgens het virtuele netwerk. In de volgende afbeelding een topologie wordt weergegeven voor een virtueel netwerk met de naam *MyVnet*, in de resourcegroep met de naam *MyResourceGroup*:

    ![Topologie weergeven](./media/view-network-topology/view-topology.png)

    Zoals u in de vorige afbeelding zien kunt, is het virtuele netwerk bevat drie subnetten. Een subnet heeft een virtuele machine in het geïmplementeerd. De virtuele machine heeft één netwerkinterface is gekoppeld en een openbare IP-adres dat is gekoppeld. De twee subnetten hebben een routetabel die hieraan zijn gekoppeld. Elke routetabel bevat twee routes. Eén subnet heeft een netwerkbeveiligingsgroep gekoppeld. Gegevens over de topologie wordt alleen weergegeven voor resources die zijn: - binnen de dezelfde resourcegroep en de regio bevinden als de *myVnet* virtueel netwerk. Bijvoorbeeld, een netwerkbeveiligingsgroep die anders dan in een resourcegroep voorkomt *MyResourceGroup*, niet wordt weergegeven, zelfs als de netwerkbeveiligingsgroep is gekoppeld aan een subnet in de *MyVnet* virtueel netwerk .
        -Binnen of gekoppeld aan resources binnen de *myVnet* virtueel netwerk. Bijvoorbeeld, een netwerkbeveiligingsgroep die niet is gekoppeld aan een subnet of netwerk interface in het *myVnet* virtueel netwerk wordt niet weergegeven, zelfs als de netwerkbeveiligingsgroep is in de *MyResourceGroup* resourcegroep.

    De topologie weergegeven in de afbeelding voor het virtuele netwerk gemaakt na de implementatie is de **routeren van verkeer via een netwerk virtueel apparaat-voorbeeldscript**, die u kunt implementeren met behulp van de [Azure CLI](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), of [PowerShell](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

6. Selecteer **downloaden topologie** de installatiekopie te downloaden als een bestand worden bewerkt in SVG-indeling.

De bronnen die u in het diagram zijn een subset van de netwerkonderdelen in het virtuele netwerk. Bijvoorbeeld, terwijl een netwerkbeveiligingsgroep wordt weergegeven, worden de beveiligingsregels voor verbindingen binnen deze niet weergegeven in het diagram. Hoewel differentiated niet in het diagram, de regels vertegenwoordigen een van twee relaties: *Containment* of *gekoppeld*. De volledige lijst van resources in het virtuele netwerk en het type van de relatie tussen de bronnen wilt bekijken, genereren de topologie met [PowerShell](#powershell) of de [Azure CLI](#azure-cli).

## <a name = "azure-cli"></a>Topologie - Azure CLI weergeven

U kunt de opdrachten uitvoeren in de stappen volgen:
- In de Azure-Cloud-Shell, door te selecteren **probeert het** boven rechts van elke opdracht. De Azure-Cloud-Shell is een gratis interactieve shell met algemene Azure-hulpprogramma's geïnstalleerd en geconfigureerd voor gebruik met uw account.
- Door het uitvoeren van de CLI vanaf uw computer. Als u de CLI vanaf uw computer uitvoert, de stappen in dit artikel de Azure CLI versie 2.0.31 vereisen of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

De account die u gebruikt moet beschikken over de benodigde [machtigingen](required-rbac-permissions.md).

1. Als u al een netwerk-watcher in dezelfde regio als het virtuele netwerk dat u wilt maken van een topologie voor hebt, gaat u naar stap 3. Maak een resourcegroep te bevatten met een netwerk-watcher [az groep maken](/cli/azure/group#az_group_create). Het volgende voorbeeld wordt de resourcegroep in de *eastus* regio:

    ```azurecli-interactive
    az group create --name NetworkWatcherRG --location eastus
    ```

2. Maken van een netwerk-watcher met [az netwerk-watcher configureren](/cli/azure/network/watcher#az-network-watcher-configure). Het volgende voorbeeld wordt een netwerk-watcher in de *eastus* regio:

    ```azurecli-interactive
    az network watcher configure \
      --resource-group NetworkWatcherRG \
      --location eastus \
      --enabled true
    ```

3. Weergeven van de topologie met [az netwerktopologie watcher weergeven-](/cli/azure/network/watcher#az-network-watcher-show-topology). De topologie voor een resourcegroep met de naam van het volgende voorbeeld-weergaven *MyResourceGroup*:

    ```azurecli-interactive
    az network watcher show-topology --resource-group MyResourceGroup
    ```

    Topologische informatie wordt alleen geretourneerd voor resources die zich in dezelfde resourcegroep bevinden als de *MyResourceGroup* resourcegroep en dezelfde regio bevinden als de netwerk-watcher. Bijvoorbeeld, een netwerkbeveiligingsgroep die anders dan in een resourcegroep voorkomt *MyResourceGroup*, niet wordt weergegeven, zelfs als de netwerkbeveiligingsgroep is gekoppeld aan een subnet in de *MyVnet* virtueel netwerk .

  Meer informatie over de [relaties](#relationhips) en [eigenschappen](#properties) in de resulterende uitvoer. Als u een bestaand virtueel netwerk om weer te geven van een topologie voor geen hebt, kunt u één die gebruikmaakt van de [verkeer routeren via een virtueel netwerkapparaat](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) voorbeeldscript. Als u wilt een diagram weergeven van de topologie en deze in een bewerkbaar bestand downloaden, gebruiken de [portal](#azure-portal).

## <a name = "powershell"></a>Topologie - PowerShell weergeven

U kunt de opdrachten uitvoeren in de stappen volgen:
- In de Azure-Cloud-Shell, door te selecteren **probeert het** boven rechts van elke opdracht. De Azure-Cloud-Shell is een gratis interactieve shell met algemene Azure-hulpprogramma's geïnstalleerd en geconfigureerd voor gebruik met uw account.
- Door te voeren PowerShell vanaf uw computer. Als u PowerShell van uw computer uitvoeren, de stappen in dit artikel is versie 5.7.0 vereist of hoger van de module AzureRm. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

De account die u gebruikt moet beschikken over de benodigde [machtigingen](required-rbac-permissions.md).

1. Als u al een netwerk-watcher in dezelfde regio als het virtuele netwerk dat u wilt maken van een topologie voor hebt, gaat u naar stap 3. Maak een resourcegroep te bevatten met een netwerk-watcher [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Het volgende voorbeeld wordt de resourcegroep in de *eastus* regio:

    ```azurepowershell-interactive
    New-AzureRmResourceGroup -Name NetworkWatcherRG -Location EastUS
    ```

2. Maken van een netwerk-watcher met [nieuw AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher). Het volgende voorbeeld maakt een netwerk-watcher in eastus regio:

    ```azurepowershell-interactive
    New-AzureRmNetworkWatcher `
      -Name NetworkWatcher_eastus `
      -ResourceGroupName NetworkWatcherRG
    ```

3. Ophalen van een netwerk-Watcher-exemplaar met [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher). Het volgende voorbeeld wordt een netwerk-watcher in de regio VS-Oost opgehaald:

    ```azurepowershell-interactive
    $nw = Get-AzurermResource `
      | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "EastUS" }
    $networkWatcher = Get-AzureRmNetworkWatcher `
      -Name $nw.Name `
      -ResourceGroupName $nw.ResourceGroupName
    ```

4. Ophalen van een topologie met [Get-AzureRmNetworkWatcherTopology](/powershell/module/azurerm.network/get-azurermnetworkwatchertopology). Het volgende voorbeeld wordt een topologie voor een virtueel netwerk in de resourcegroep met de naam opgehaald *MyResourceGroup*:

    ```azurepowershell-interactive
    Get-AzureRmNetworkWatcherTopology `
      -NetworkWatcher $networkWatcher `
      -TargetResourceGroupName MyResourceGroup
    ```

   Topologische informatie wordt alleen geretourneerd voor resources die zich in dezelfde resourcegroep bevinden als de *MyResourceGroup* resourcegroep en dezelfde regio bevinden als de netwerk-watcher. Bijvoorbeeld, een netwerkbeveiligingsgroep die anders dan in een resourcegroep voorkomt *MyResourceGroup*, niet wordt weergegeven, zelfs als de netwerkbeveiligingsgroep is gekoppeld aan een subnet in de *MyVnet* virtueel netwerk .

  Meer informatie over de [relaties](#relationhips) en [eigenschappen](#properties) in de resulterende uitvoer. Als u een bestaand virtueel netwerk om weer te geven van een topologie voor geen hebt, kunt u één die gebruikmaakt van de [verkeer routeren via een virtueel netwerkapparaat](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) voorbeeldscript. Als u wilt een diagram weergeven van de topologie en deze in een bewerkbaar bestand downloaden, gebruiken de [portal](#azure-portal).

## <a name="relationships"></a>Relaties

Alle resources die worden geretourneerd in een topologie hebben een van de volgende typen van de relatie met een andere resource:

| Relatietype | Voorbeeld                                                                                                |
| ---               | ---                                                                                                    |
| Containment       | Een virtueel netwerk bevat een subnet. Een subnet bevat een netwerkinterface.                            |
| Dat is gekoppeld        | Een netwerkinterface is gekoppeld aan een virtuele machine. Een openbaar IP-adres is gekoppeld aan een netwerkinterface. |

## <a name="properties"></a>Eigenschappen

Alle resources die worden geretourneerd in een topologie met hebben de volgende eigenschappen:

- **Naam**: de naam van de resource
- **Id**: de URI van de resource.
- **Locatie**: de bron bestaat in de Azure-regio.
- **Koppelingen**: een lijst met koppelingen naar het object waarnaar wordt verwezen. Elke koppeling heeft de volgende eigenschappen:
    - **AssociationType**: verwijst naar de relatie tussen het onderliggende object en het bovenliggende item. Geldige waarden zijn *bevat* of *gekoppelde*.
    - **Naam**: de naam van de bron waarnaar wordt verwezen.
    - **ResourceId**:-de URI van de bron waarnaar wordt verwezen in de koppeling.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [een verkeer filter netwerkprobleem naar of van een virtuele machine vaststellen](diagnose-vm-network-traffic-filtering-problem.md) mogelijkheid met behulp van netwerk-Watcher IP-stroom controleren
- Meer informatie over hoe [een netwerkprobleem voor het doorsturen van verkeer van een virtuele machine vaststellen](diagnose-vm-network-routing-problem.md) met de volgende hop mogelijkheid netwerk-Watcher