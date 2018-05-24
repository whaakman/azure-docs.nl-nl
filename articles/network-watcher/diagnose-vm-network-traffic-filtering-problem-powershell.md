---
title: Diagnose uitvoeren voor een probleem met netwerkverkeersfilters op een virtuele machine - snelstart - Azure PowerShell | Microsoft Docs
description: In deze snelstart leert u hoe u een diagnose uitvoert voor een probleem met netwerkverkeersfilters op een virtuele machine met behulp van de functie IP-stroomverificatie in Azure Network Watcher.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose a virtual machine (VM) network traffic filter problem that prevents communication to and from a VM.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: d98a804961defc80bebe3e3a838dd229c23044bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-diagnose-a-virtual-machine-network-traffic-filter-problem---azure-powershell"></a>Snelstart: Diagnose uitvoeren voor een probleem met netwerkverkeersfilters op een virtuele machine - Azure PowerShell

In deze snelstart implementeert u een VM (virtuele machine) en controleert u vervolgens de communicatie naar een IP-adres en URL, en vanaf een IP-adres. U stelt de oorzaak van mislukte communicatie vast en leert hoe u dit probleem kunt oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze snelstart AzureRM PowerShell-module versie 5.4.1 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om na te gaan welke versie er is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-vm"></a>Een virtuele machine maken

Voordat u een VM kunt maken, maakt u eerst een resourcegroep die de VM bevat. Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

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

Als u de netwerkcommunicatie met Network Watcher wilt testen, moet u eerst een netwerk-watcher inschakelen in de regio met de VM die u wilt testen. Gebruik vervolgens de functie IP-stroomverificatie in Network Watcher om de communicatie te testen.

### <a name="enable-network-watcher"></a>Netwerk-watcher inschakelen

Als u al een netwerk-watcher hebt ingeschakeld in de regio VS Oost, gebruikt u [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher) om de netwerk-watcher op te halen. In het volgende voorbeeld wordt een bestaande netwerk-watcher opgehaald met de naam *NetworkWatcher_eastus* die zich in de resourcegroep *NetworkWatcherRG* bevindt:

```azurepowershell-interactive
$networkWatcher = Get-AzureRmNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Als u nog geen netwerk-watcher hebt ingeschakeld in de regio VS Oost, gebruikt u [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) om een netwerk-watcher te maken in de regio VS Oost:

```azurepowershell-interactive
$networkWatcher = New-AzureRmNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-ip-flow-verify"></a>IP-stroomverificatie gebruiken

Als u een VM maakt, wordt netwerkverkeer van en naar de VM standaard toegestaan en geweigerd in Azure. U kunt de standaardinstellingen in Azure later negeren en extra typen verkeer toestaan en weigeren. Gebruik de opdracht [Test-AzureRmNetworkWatcherIPFlow](/powershell/module/azurerm.network/test-azurermnetworkwatcheripflow) om te testen of verkeer naar verschillende bestemmingen en vanaf een bron-IP-adres is toegestaan of wordt geweigerd.

Uitgaande communicatie van de VM naar een van de IP-adressen testen voor www.bing.com:

```azurepowershell-interactive
Test-AzureRmNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Outbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 60000 `
  -RemoteIPAddress 13.107.21.200 `
  -RemotePort 80
```

Na enkele seconden blijkt uit het resultaat dat toegang is toegestaan op basis van een beveiligingsregel met de naam **AllowInternetOutbound**.

Uitgaande communicatie van de VM naar 172.31.0.100 testen:

```azurepowershell-interactive
Test-AzureRmNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Outbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 60000 `
  -RemoteIPAddress 172.31.0.100 `
  -RemotePort 80
```

Uit het resultaat blijkt dat toegang wordt geweigerd op basis van een beveiligingsregel met de naam **DefaultOutboundDenyAll**.

Binnenkomende communicatie van 172.31.0.100 naar de VM testen:

```azurepowershell-interactive
Test-AzureRmNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Inbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 80 `
  -RemoteIPAddress 172.31.0.100 `
  -RemotePort 60000
```

Uit het resultaat blijkt dat toegang wordt geweigerd vanwege een beveiligingsregel met de naam **DefaultInboundDenyAll**. Nu u weet op basis van welke beveiligingsregels verkeer van en naar een VM is toegestaan of wordt geweigerd, kunt u bepalen hoe u de problemen wilt oplossen.

## <a name="view-details-of-a-security-rule"></a>Details van een beveiligingsregel weergeven

Als u wilt vaststellen waarom communicatie is toegestaan of wordt geblokkeerd op basis van de regels in [Netwerkcommunicatie testen](#test-network-communication), controleert u de effectieve beveiligingsregels voor de netwerkinterface met [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup):

```azurepowershell-interactive
Get-AzureRmEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup
```

De geretourneerde uitvoer bevat de volgende tekst voor de regel **AllowInternetOutbound** op basis waarvan uitgaand verkeer naar www.bing.com is toegestaan in [IP-stroomverificatie gebruiken](#use-ip-flow-verify):

```powershell
{
  "Name":
"defaultSecurityRules/AllowInternetOutBound",
  "Protocol": "All",
  "SourcePortRange": [
    "0-65535"
  ],
  "DestinationPortRange": [
    "0-65535"
  ],
  "SourceAddressPrefix": [
    "0.0.0.0/0"
  ],
  "DestinationAddressPrefix": [
    "Internet"
  ],
  "ExpandedSourceAddressPrefix": [],
  "ExpandedDestinationAddressPrefix": [
    "1.0.0.0/8",
    "2.0.0.0/7",
    "4.0.0.0/6",
    "8.0.0.0/7",
    "11.0.0.0/8",
    "12.0.0.0/6",
    ...
    ],
    "Access": "Allow",
    "Priority": 65001,
    "Direction": "Outbound"
  },
```

In de uitvoer ziet u dat **DestinationAddressPrefix** **Internet** is. Het is niet duidelijk hoe 13.107.21.200, het adres dat u hebt getest in [IP-stroomverificatie gebruiken](#use-ip-flow-verify), is gerelateerd aan **Internet**. Er worden verschillende adresvoorvoegsels weergegeven onder **ExpandedDestinationAddressPrefix**. Een van de voorvoegsels in de lijst is **12.0.0.0/6**, dat het bereik met IP-adressen 12.0.0.1-15.255.255.254 bevat. Aangezien 13.107.21.200 zich binnen dit adresbereik bevindt, is uitgaand verkeer toegestaan op basis van de regel **AllowInternetOutBound**. Daarnaast worden in de met `Get-AzureRmEffectiveNetworkSecurityGroup` geretourneerde uitvoer geen regels met hogere **prioriteit** (lagere waarde) weergegeven op basis waarvan deze regel wordt genegeerd. Als u uitgaande communicatie naar 13.107.21.200 wilt weigeren, kunt u een beveiligingsregel met een hogere prioriteit toevoegen op basis waarvan uitgaand verkeer naar het IP-adres wordt geweigerd op poort 80.

Toen u de opdracht `Test-AzureRmNetworkWatcherIPFlow` uitvoerde om uitgaande communicatie naar 172.131.0.100 te testen in [IP-stroomverificatie gebruiken](#use-ip-flow-verify), bleek uit de uitvoergegevens dat communicatie wordt geweigerd op basis van de regel **DefaultOutboundDenyAll**. De regel **DefaultOutboundDenyAll** komt overeen met de regel **DenyAllOutBound** die wordt weergegeven in de volgende uitvoer via de opdracht `Get-AzureRmEffectiveNetworkSecurityGroup`:

```powershell
{
"Name": "defaultSecurityRules/DenyAllOutBound",
"Protocol": "All",
"SourcePortRange": [
  "0-65535"
],
"DestinationPortRange": [
  "0-65535"
],
"SourceAddressPrefix": [
  "0.0.0.0/0"
],
"DestinationAddressPrefix": [
  "0.0.0.0/0"
],
"ExpandedSourceAddressPrefix": [],
"ExpandedDestinationAddressPrefix": [],
"Access": "Deny",
"Priority": 65500,
"Direction": "Outbound"
}
```

In de regel wordt **0.0.0.0/0** vermeld als **DestinationAddressPrefix**. Op basis van de regel wordt uitgaande communicatie naar 172.131.0.100 geweigerd, omdat het adres zich niet in het **DestinationAddressPrefix** bevindt van een van de overige regels voor uitgaand verkeer met de opdracht `Get-AzureRmEffectiveNetworkSecurityGroup`. Als u de uitgaande communicatie wilt toestaan, kunt u een beveiligingsregel toevoegen met een hogere prioriteit, op basis waarvan uitgaand verkeer naar poort 80 is toegestaan op 172.131.0.100.

Toen u de opdracht `Test-AzureRmNetworkWatcherIPFlow` uitvoerde om binnenkomende communicatie van 172.131.0.100 te testen in [IP-stroomverificatie gebruiken](#use-ip-flow-verify), bleek uit de uitvoer dat communicatie wordt geweigerd op basis van de regel **DefaultOutboundDenyAll**. De regel **DefaultInboundDenyAll** is gelijk aan de regel **DenyAllInBound** die wordt weergegeven in de volgende uitvoer via de opdracht `Get-AzureRmEffectiveNetworkSecurityGroup`:

```powershell
{
"Name": "defaultSecurityRules/DenyAllInBound",
"Protocol": "All",
"SourcePortRange": [
  "0-65535"
],
"DestinationPortRange": [
  "0-65535"
],
"SourceAddressPrefix": [
  "0.0.0.0/0"
],
"DestinationAddressPrefix": [
  "0.0.0.0/0"
],
"ExpandedSourceAddressPrefix": [],
"ExpandedDestinationAddressPrefix": [],
"Access": "Deny",
"Priority": 65500,
"Direction": "Inbound"
},
```

De regel **DenyAllInBound** is toegepast omdat, zoals wordt weergegeven in de uitvoer, de uitvoer via de opdracht `Get-AzureRmEffectiveNetworkSecurityGroup` geen andere regel met hoge prioriteit bevat, op basis waarvan binnenkomend verkeer via poort 80 van 172.131.0.100 naar de VM is toegestaan. Als u de binnenkomende communicatie wilt toestaan, kunt u een beveiligingsregel met een hogere prioriteit toevoegen, op basis waarvan binnenkomend verkeer van 172.131.0.100 via poort 80 is toegestaan.

Met de controles in deze snelstart is de Azure-configuratie getest. Als de controles onverwachte resultaten retourneren en u nog steeds netwerkproblemen ondervindt, controleert u of er geen firewall is geplaatst tussen de VM en het eindpunt waarmee u communiceert, en of het besturingssysteem op de VM geen firewall heeft waardoor communicatie is toegestaan of wordt geweigerd.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een VM gemaakt en diagnose uitgevoerd voor netwerkfilters voor binnenkomend en uitgaand verkeer. U hebt geleerd dat verkeer van en naar een VM kan zijn toegestaan of worden geweigerd op basis van regels voor netwerkbeveiligingsgroepen. Meer informatie over [beveiligingsregels](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en het [maken van beveiligingsregels](../virtual-network/manage-network-security-group.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-security-rule).

Zelfs als de juiste netwerkverkeersfilters zijn toegepast, kan communicatie naar een VM mislukken, vanwege de configuratie van de routering. Zie [Diagnose uitvoeren voor problemen met VM-routering](diagnose-vm-network-routing-problem-powershell.md) voor informatie over het vaststellen van routeringsproblemen met VM-netwerken. Of zie [Problemen met de verbinding oplossen ](network-watcher-connectivity-powershell.md) om met één hulpprogramma de problemen vast te stellen met routering van uitgaand verkeer, latentie en verkeersfilters.