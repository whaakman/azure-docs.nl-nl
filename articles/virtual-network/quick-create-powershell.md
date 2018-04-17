---
title: 'Een virtueel netwerk maken: snelstart - Azure PowerShell | Microsoft Docs'
description: In deze snelstart leert u hoe u een virtueel netwerk maakt met Azure Portal. Met een virtueel netwerk kunnen Azure-resources, zoals virtuele machines, privé communiceren met elkaar en met internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 3d4c8e130f96c1b89247fe0c092363c33032ec3d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Snelstart: een virtueel netwerk maken met PowerShell

Met een virtueel netwerk kunnen Azure-resources, zoals virtuele machines (VM), privé communiceren met elkaar en met internet. In deze snelstart leert u hoe u een virtueel netwerk maakt. Nadat u een virtueel netwerk hebt gemaakt, implementeert u twee virtuele machines in het virtuele netwerk. Vervolgens maakt u verbinding met één virtuele machine via internet en is er privécommunicatie tussen de twee virtuele machines mogelijk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze snelstart AzureRM PowerShell-module versie 5.4.1 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om te zien welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk kunt maken, moet u een resourcegroep maken die het virtuele netwerk bevat. Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). In het volgende voorbeeld wordt een standaard virtueel netwerk gemaakt met de naam *myVirtualNetwork* op de locatie *EastUS*:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Azure-resources worden geïmplementeerd in een subnet binnen een virtueel netwerk, daarom moet u een subnet maken. Maak een subnetconfiguratie met [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). 

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Schrijf de subnetconfiguratie naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), waarmee het subnet in het virtuele netwerk wordt gemaakt:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee virtuele machines in het virtuele netwerk:

### <a name="create-the-first-vm"></a>De eerste VM maken

Maak een VM met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Wanneer de volgende opdracht wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine. Met de optie `-AsJob` wordt de virtuele machine op de achtergrond gemaakt, zodat u kunt doorgaan met de volgende stap.

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

### <a name="create-the-second-vm"></a>De tweede VM maken 

Voer de volgende opdracht in:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Het maken van de virtuele machine duurt een paar minuten. Ga niet verder met de volgende stap totdat de vorige opdracht is uitgevoerd en de uitvoer is geretourneerd naar PowerShell.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine via internet

Gebruik [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van een virtuele machine op te halen. In het volgende voorbeeld wordt het openbare IP-adres van de VM *myVm1* opgehaald:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Vervang `<publicIpAddress>` in de volgende opdracht door het openbare IP-adres dat met de vorige opdracht is geretourneerd en voer vervolgens de volgende opdracht in: 

```
mstsc /v:<publicIpAddress>
```

Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload naar uw computer. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. Selecteer **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding.

## <a name="communicate-between-vms"></a>Communicatie tussen virtuele machines

Voer vanuit PowerShell op de VM *myVm1* `ping myvm2` in. Pingen mislukt, omdat ping het ICMP (Internet Control Message Protocol) gebruikt en ICMP standaard niet is toegestaan via de Windows-firewall.

Om *myVm2* toe te staan *myVm1* in een latere stap te pingen, voert u de volgende opdracht uit vanuit PowerShell, waarmee inkomend ICMP wordt toegestaan via de Windows-firewall:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Sluit de externe bureaubladverbinding met *myVm1*. 

Voer de stappen in [Verbinding maken met een virtuele machine via internet](#connect-to-a-vm-from-the-internet) opnieuw uit, maar maak nu verbinding met *myVm2*. 

Voer vanaf een opdrachtprompt op de virtuele machine *myVm2* `ping myvm1` in.

U ontvangt antwoorden van *myVm1* omdat u ICMP hebt toegestaan via de Windows-firewall op de VM *myVm1* in de vorige stap.

Sluit de externe bureaubladverbinding met *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een standaard virtueel netwerk en twee virtuele machines gemaakt. U hebt met één virtuele machine verbinding gemaakt via internet en is er privécommunicatie tussen de twee virtuele machines geweest. Zie [Virtueel netwerk beheren](manage-virtual-network.md) voor meer informatie over instellingen voor virtuele netwerken. 

Standaard staat Azure onbeperkte privécommunicatie tussen virtuele machines toe, maar staat deze alleen inkomende externe bureaubladverbindingen met Windows VM's via internet toe. Voor informatie over het toestaan of beperken van verschillende soorten netwerkcommunicatie naar en van virtuele machines gaat u verder met de zelfstudie [Netwerkverkeer filteren](tutorial-filter-network-traffic.md).
