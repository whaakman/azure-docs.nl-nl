---
title: Diagnose van een virtuele machine routering netwerkprobleem - Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe een virtuele machine-netwerk routering probleem met de volgende hop-mogelijkheden van Azure-netwerk-Watcher op te sporen.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f793a201b3fbf57ac2f420c4f4e57a230bc11468
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Diagnose van een virtuele machine routering netwerkprobleem - Azure PowerShell

In dit artikel wordt een virtuele machine (VM) implementeren, en worden vervolgens controleren communicatie naar een IP-adres en een URL. U oorzaak de van een communicatiefout is opgetreden en hoe u het kunt oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de AzureRM PowerShell moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om na te gaan welke versie er is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-vm"></a>Een virtuele machine maken

Voordat u een virtuele machine maken kunt, moet u een resourcegroep te bevatten van de virtuele machine maken. Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Maken van de virtuele machine met [nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

```azurepowershell-interactive
$vM = New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Het maken van de virtuele machine duurt een paar minuten. Niet doorgaan met de resterende stappen totdat de virtuele machine wordt gemaakt en PowerShell retourneert de uitvoer.

## <a name="test-network-communication"></a>Test de netwerkcommunicatie

Als u wilt testen netwerkcommunicatie met de netwerk-Watcher, moet u eerst inschakelen in de regio die de virtuele machine die u wilt testen, is in een netwerk-watcher en gebruik vervolgens de netwerk-Watcher volgende hop mogelijkheid om communicatie te testen.

## <a name="enable-network-watcher"></a>Inschakelen van netwerk-watcher

Als u al een netwerk-watcher ingeschakeld in de regio VS-Oost, gebruikt u [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher) voor het ophalen van de netwerk-watcher. Het volgende voorbeeld wordt een bestaande netwerk-watcher met de naam opgehaald *NetworkWatcher_eastus* die zich in de *NetworkWatcherRG* resourcegroep:

```azurepowershell-interactive
$networkWatcher = Get-AzureRmNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Als u een netwerk-watcher ingeschakeld in de regio VS-Oost nog geen hebt, gebruikt u [nieuw AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) maken van een netwerk-watcher in de regio VS-Oost:

```azurepowershell-interactive
$networkWatcher = New-AzureRmNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Gebruik de volgende hop

Azure maakt automatisch routes naar standaard bestemmingen. U kunt aangepaste routes die de standaardroutes overschrijven maken. Soms aangepaste routes kunnen leiden tot communicatie mislukken. U kunt testen routering van een virtuele machine met de [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) opdracht om te bepalen van de volgende hop routering wanneer verkeer is dat is bestemd voor een specifiek adres.

Uitgaande communicatie van de virtuele machine op een van de IP-adressen voor www.bing.com testen:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Na enkele seconden, informeert u met de uitvoer die de **NextHopType** is **Internet**, en dat de **RouteTableId** is **Systeemroute**. Dit laat u weten of er een geldige route naar de bestemming is.

Uitgaande communicatie van de virtuele machine 172.31.0.100 testen:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

De uitvoer die wordt geretourneerd, informeert u die **geen** is de **NextHopType**, en dat de **RouteTableId** is ook **Systeemroute**. Dit laat u weten dat er is een geldige route naar de bestemming, maar er geen volgende hop is voor het routeren van het verkeer naar de bestemming.

## <a name="view-details-of-a-route"></a>Details weergeven van een route

Voor het analyseren van verdere routering, Controleer de effectieve routes voor de netwerkinterface met de [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) opdracht:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

Uitvoer met de volgende tekst wordt geretourneerd:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Zoals u in de vorige uitvoer is de route met ziet de **AaddressPrefix** van **0.0.0.0/0** alle verkeer dat niet is bestemd voor adressen binnen andere route-adresvoorvoegsels met een volgende hop van gerouteerd**Internet**. Zoals u in de uitvoer ziet, maar er is een standaardroute aan het voorvoegsel 172.16.0.0/12, waaronder de 172.31.0.100 adres, de **nextHopType** is **geen**. Azure maakt een standaardroute voor 172.16.0.0/12, maar pas er is een reden voor een volgend hoptype niet opgeven. Als u bijvoorbeeld het adresbereik 172.16.0.0/12 toegevoegd aan de adresruimte van het virtuele netwerk, Azure verandert de **nextHopType** naar **virtueel netwerk** voor de route. Vervolgens wordt een controle wilt weergeven **virtueel netwerk** als de **nextHopType**.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een virtuele machine gemaakt en gediagnosticeerd netwerkroutering van de virtuele machine. Hebt u geleerd dat Azure verschillende standaardroutes maakt en getest routering naar twee verschillende bestemmingen. Meer informatie over [routering in Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en hoe [maken van aangepaste routes](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Voor uitgaande verbindingen van de VM, u kunt ook bepalen de latentie en toegestane en geweigerde netwerkverkeer tussen de virtuele machine en een eindpunt met behulp van netwerk-Watcher [verbinding oplossen](network-watcher-connectivity-powershell.md) mogelijkheid. U kunt de communicatie tussen een virtuele machine en een eindpunt, zoals een IP-adres of -URL, gedurende een periode met de netwerk-Watcher verbinding monitor mogelijkheid bewaken. Voor meer informatie Zie [bewaken van een netwerkverbinding](connection-monitor.md).