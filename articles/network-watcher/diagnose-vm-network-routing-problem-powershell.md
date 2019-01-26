---
title: Een virtuele machine netwerk routeringsprobleem vaststellen - Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u een VM-netwerk routering probleem met behulp van de volgende hop-mogelijkheden van Azure Network Watcher op te sporen.
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
ms.openlocfilehash: 81bbf2b69e0e492ea75e8cbbe980d7e83a86eae7
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54912847"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Een virtuele machine netwerk routeringsprobleem vaststellen - Azure PowerShell

In dit artikel, kunt u een virtuele machine (VM) implementeren, en controleert op communicatie naar een IP-adres en de URL. U stelt de oorzaak van mislukte communicatie vast en leert hoe u dit probleem kunt oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de AzureRM PowerShell-moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om na te gaan welke versie er is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-vm"></a>Een virtuele machine maken

Voordat u een VM kunt maken, maakt u eerst een resourcegroep die de VM bevat. Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Maak de VM met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

```azurepowershell-interactive
$vM = New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Het maken van de virtuele machine duurt een paar minuten. Ga niet door met de resterende stappen totdat de VM is gemaakt en uitvoer wordt geretourneerd met PowerShell.

## <a name="test-network-communication"></a>Netwerkcommunicatie testen

Als u wilt testen netwerkcommunicatie met Network Watcher, moet u eerst een netwerk-watcher in de regio voor de virtuele machine die u wilt testen inschakelen en vervolgens de volgende hop-mogelijkheden van Network Watcher gebruiken om communicatie te testen.

## <a name="enable-network-watcher"></a>Netwerk-watcher inschakelen

Als u al een netwerk-watcher hebt ingeschakeld in de regio US - oost, gebruikt u [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher) om de netwerk-watcher op te halen. In het volgende voorbeeld wordt een bestaande netwerk-watcher opgehaald met de naam *NetworkWatcher_eastus* die zich in de resourcegroep *NetworkWatcherRG* bevindt:

```azurepowershell-interactive
$networkWatcher = Get-AzureRmNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Als u nog geen netwerk-watcher hebt ingeschakeld in de regio US - oost, gebruikt u [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) om een netwerk-watcher te maken in de regio US - oost:

```azurepowershell-interactive
$networkWatcher = New-AzureRmNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Volgende hop gebruiken

Azure maakt automatisch routes naar standaardbestemmingen. U kunt uw eigen, aangepaste routes maken om die standaardroutes te overschrijven. Soms hebben aangepaste routes tot gevolg dat de communicatie mislukt. Als u wilt testen van de routering van een virtuele machine, gebruikt u de [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) opdracht om te bepalen van de volgende hop routering wanneer verkeer bestemd voor een specifiek adres is.

Uitgaande communicatie van de VM naar een van de IP-adressen testen voor www.bing.com:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Na enkele seconden, de uitvoer wordt gemeld dat de **NextHopType** is **Internet**, en dat de **RouteTableId** is **Systeemroute**. Dit resultaat laat u weten dat er een geldige route naar de bestemming is.

Uitgaande communicatie van de VM naar 172.31.0.100 testen:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

De uitvoer die wordt geretourneerd, informeert u die **geen** is de **NextHopType**, en dat de **RouteTableId** is ook **Systeemroute**. Hieruit kunt u afleiden dat er wel een geldige systeemroute is naar de bestemming, maar dat er geen volgende hop is voor het routeren van het verkeer naar de bestemming.

## <a name="view-details-of-a-route"></a>Details van een route weergeven

Voor het analyseren van verdere routering, Controleer de effectieve routes voor de netwerkinterface met de [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) opdracht:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

Uitvoer met de volgende tekst is geretourneerd:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Zoals u in de vorige uitvoer, de route met ziet de **AddressPrefix** van **0.0.0.0/0** al het verkeer dat niet is bestemd voor adressen binnen de adresvoorvoegsels van andere route met de volgende hop van **Internet**. Zoals u ook in de uitvoer ziet, maar er is een standaardroute voor het voorvoegsel 172.16.0.0/12, waaronder de 172.31.0.100-adres, de **nextHopType** is **geen**. Azure maakt een standaardroute naar 172.16.0.0/12, maar stelt geen volgend hoptype in, tenzij daar een reden voor is. Als u bijvoorbeeld het adresbereik 172.16.0.0/12 toegevoegd aan de adresruimte van het virtuele netwerk, Azure verandert de **nextHopType** naar **virtueel netwerk** voor de route. Vervolgens ziet u een selectievakje **virtueel netwerk** als de **nextHopType**.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een virtuele machine gemaakt en gediagnosticeerd de routering van de virtuele machine. U hebt geleerd dat Azure verschillende standaardroutes maakt en u hebt de routering naar twee verschillende bestemmingen getest. Lees hier meer over [routering in Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en hoe u [aangepaste routes maakt](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Voor uitgaande verbindingen van de virtuele machine, u kunt ook bepalen de latentie en toegestane en geweigerde netwerkverkeer tussen de virtuele machine en een eindpunt met behulp van Network Watcher [probleemoplossing voor verbindingen](network-watcher-connectivity-powershell.md) mogelijkheid. U kunt de communicatie tussen een virtuele machine en een eindpunt, zoals een IP-adres of de URL, na verloop van tijd met behulp van de mogelijkheden van Network Watcher connection monitor bewaken. Voor meer informatie Zie [bewaken van een netwerkverbinding](connection-monitor.md).