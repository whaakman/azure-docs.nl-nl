---
title: Een virtueel netwerk maken in Azure - PowerShell | Microsoft Docs
description: "Snel informatie over het maken van een virtueel netwerk met behulp van PowerShell. Een virtueel netwerk kan veel soorten Azure-bronnen te privé met elkaar communiceren."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 091e7e6cabf325cdd9d4289e7d22e71c583d91db
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>Maak een virtueel netwerk met behulp van PowerShell

In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, moet u twee virtuele machines implementeren in het virtuele netwerk en privé communiceren tussen deze twee.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in deze zelfstudie vereist de Azure PowerShell moduleversie 5.1.1 of hoger. Ga voor de geïnstalleerde versie uitvoeren ` Get-Module -ListAvailable AzureRM`. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*. Alle Azure-resources worden in een Azure-locatie (of regio) gemaakt.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam van standaard *myVirtualNetwork* in de *EastUS* locatie:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/24
```

Alle virtuele netwerken zijn een of meer adresvoorvoegsels aan hen toegewezen. De adresruimte is opgegeven in CIDR-notatie. Het 10.0.0.0/24 ruimte omvat 10.0.0.0-10.0.0.254. Virtuele netwerken hebben nul of meer subnetten binnen deze. Resources worden geïmplementeerd in een subnet in een virtueel netwerk. 

Maken van een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Alle subnetten hebben een adresvoorvoegsel die binnen het virtuele netwerk adresvoorvoegsel bestaat. In dit voorbeeld wordt een subnet is geconfigureerd met hetzelfde adresvoorvoegsel als voorvoegsel van het adres van het virtuele netwerk gemaakt:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Hoewel het subnetvoorvoegsel adres omvat 10.0.0.0-10.0.0.254, zijn alleen de adressen 10.0.0.4-10.0.0.254 beschikbaar, omdat Azure reserveert de eerste vier adressen (0-3) en het laatste adres in elk subnet. Omdat het subnet adresvoorvoegsel hetzelfde als het virtuele netwerk-adresvoorvoegsel is, kan slechts één subnet in dit virtuele netwerk bestaan.

De configuratie van subnetten met schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), die het subnet wordt gemaakt:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtueel netwerk kunt verschillende soorten Azure-bronnen te privé met elkaar communiceren. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine. Twee virtuele machines maken in het virtuele netwerk, zodat u kunt valideren en inzicht in de werking van communicatie tussen virtuele machines in een virtueel netwerk in een latere stap.

Maak een virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine. De locatie die in een virtuele machine wordt gemaakt, moet het virtuele netwerk bestaat in dezelfde locatie. De virtuele machine is niet in dezelfde resourcegroep bevinden als de virtuele machine is vereist als het in dit artikel. De `-AsJob` parameter kan de opdracht op de achtergrond uitgevoerd, zodat u kunt doorgaan met de volgende taak.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Uitvoer die vergelijkbaar is met de volgende voorbeelduitvoer wordt geretourneerd en Azure begint met het maken van de virtuele machine op de achtergrond.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

Azure DHCP wijst automatisch 10.0.0.4 aan de virtuele machine tijdens het maken, omdat dit het eerste beschikbare adres in de *standaard* subnet.

Maak een tweede virtuele machine. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```
De virtuele machine duurt een paar minuten maken. Nadat deze is gemaakt, Azure retourneert uitvoer over de gemaakte virtuele machine. Hoewel het niet in de uitvoer van de geretourneerde Azure toegewezen *10.0.0.5* naar de *myVm2* virtuele machine, omdat het adres van de volgende beschikbaar in het subnet.

## <a name="connect-to-a-virtual-machine"></a>Verbinding maken met een virtuele machine

Gebruik de [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) opdracht voor het retourneren van het openbare IP-adres van een virtuele machine. Een openbaar, Internet routeerbare IP-adres in Azure met elke virtuele machine standaard wordt toegewezen. Het openbare IP-adres is toegewezen aan de virtuele machine vanuit een [groep adressen toegewezen aan elke Azure-regio](https://www.microsoft.com/download/details.aspx?id=41653). Terwijl Azure welk openbare IP-adres is toegewezen aan een virtuele machine weet, is het besturingssysteem in een virtuele machine geen bewust te maken van een openbaar IP-adres toegewezen. Het volgende voorbeeld wordt het openbare IP-adres van de *myVm1* virtuele machine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name myVm1 -ResourceGroupName myResourceGroup | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVm1* virtuele machine van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt, naar de computer gedownload en geopend. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine en klik vervolgens op **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

## <a name="validate-communication"></a>Communicatie valideren

Probeert te pingen van een Windows-virtuele machine mislukt, omdat ping niet is toegestaan via de Windows firewall standaard. Om toe te staan ping naar *myVm1*, voer de volgende opdracht uit vanaf de opdrachtprompt:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Valideren van de communicatie met *myVm2*, voer de volgende opdracht vanaf een opdrachtprompt op de *myVm1* virtuele machine. Geef de referenties die u hebt gebruikt toen u de virtuele machine gemaakt en voltooi vervolgens de verbinding:

```
mstsc /v:myVm2
```

Verbinding met extern bureaublad is mislukt omdat beide virtuele machines privé IP-adressen toegewezen vanuit de *standaard* subnet en omdat de extern bureaublad openen via de Windows-firewall standaard is. U bent geen verbinding maken met *myVm2* door hostnaam omdat Azure automatisch DNS-naamomzetting voor alle hosts binnen een virtueel netwerk biedt. De opdracht ping vanaf een opdrachtprompt opdracht mijn *myVm1*, van *myVm2*.

```
ping myvm1
```

Ping is voltooid, omdat deze toegestaan via de Windows firewall op de *myVm1* virtuele machine in de vorige stap. Om te bevestigen uitgaande communicatie met Internet, voer de volgende opdracht:

```
ping bing.com
```

U ontvangt vier reacties van bing.com. Standaard kan een virtuele machine in een virtueel netwerk uitgaand naar het Internet communiceren.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) opdracht om te verwijderen van de resourcegroep en alle resources bevat. De extern bureaublad-sessiehost sluiten en voer vervolgens de volgende opdracht vanaf uw computer te verwijderen van de resourcegroep:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel als u een standaard virtuele netwerk met één subnet en twee virtuele machines geïmplementeerd. Voor meer informatie over hoe u een aangepaste virtueel netwerk maken met meerdere subnetten en beheertaken uitvoeren in virtuele Basisnetwerk, blijven de zelfstudie voor het maken van een aangepaste virtueel netwerk en van het beheer.


> [!div class="nextstepaction"]
> [Een aangepaste virtueel netwerk maken en deze te beheren](virtual-networks-create-vnet-arm-pportal.md#powershell)
