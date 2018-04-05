---
title: Maken van een virtuele Azure-netwerk - PowerShell | Microsoft Docs
description: Snel informatie over het maken van een virtueel netwerk met behulp van PowerShell. Een virtueel netwerk kunt Azure-resources, zoals virtuele machines, privé communiceren met elkaar en met het internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: fe171000f83c27f23972569b93e351340f4426ad
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>Maak een virtueel netwerk met behulp van PowerShell

Azure-resources, zoals virtuele machines (VM), kunt een virtueel netwerk privé communiceren met elkaar en met het internet. In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, kunt u twee virtuele machines in het virtuele netwerk implementeert. U vervolgens verbinding maken met één virtuele machine vanaf het internet en privé communicatie tussen de twee virtuele machines.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de AzureRM PowerShell moduleversie 5.4.1 of hoger. Ga voor de geïnstalleerde versie uitvoeren ` Get-Module -ListAvailable AzureRM`. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk maken kunt, moet u een resourcegroep om het virtuele netwerk maken. Maak een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam van standaard *myVirtualNetwork* in de *EastUS* locatie:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Azure-resources zijn geïmplementeerd op een subnet binnen een virtueel netwerk, dus moet u een subnet maken. Maken van een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). 

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

De configuratie van subnetten met schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), die wordt gemaakt met het subnet in het virtuele netwerk:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee virtuele machines in het virtuele netwerk:

### <a name="create-the-first-vm"></a>De eerste virtuele machine maken

Maak een VM met [nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Wanneer de volgende opdracht wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

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

### <a name="create-the-second-vm"></a>De tweede virtuele machine maken 

Voer de volgende opdracht in:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

De virtuele machine duurt een paar minuten maken. Ga niet verder met de volgende stap totdat de vorige opdracht wordt uitgevoerd en wordt de uitvoer geretourneerd naar PowerShell.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine vanaf het internet

Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Vervang `<publicIpAddress>` in de volgende opdracht met het openbare IP-adres van de vorige opdracht geretourneerd en voer vervolgens de volgende opdracht: 

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt en gedownload op uw computer. Open het gedownloade rdp-bestand. Als u wordt gevraagd, selecteert u **Connect**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine. U wilt selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven. Selecteer **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **doorgaan**, om door te gaan met de verbinding.

## <a name="communicate-privately-between-vms"></a>Privé communiceren tussen VM 's

Vanuit PowerShell op de *myVm1* virtuele machine, voer `ping myvm2`. Ping mislukt, omdat ping gebruikmaakt van de internet control message protocol (ICMP) en ICMP is niet toegestaan via de Windows firewall standaard.

Om toe te staan *myVm2* te pingen *myVm1* in een latere stap, voert u de volgende opdracht vanuit PowerShell waarmee ICMP inkomende via de Windows firewall:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Sluit de verbinding met extern bureaublad naar *myVm1*. 

Voer de stappen in [verbinding maken met een virtuele machine met het internet](#connect-to-a-vm-from-the-internet) opnieuw, maar verbinding maken met *myVm2*. 

Vanaf een opdrachtprompt op de *myVm2* virtuele machine, voer `ping myvm1`.

U ontvangt antwoorden van *myVm1*, omdat u ICMP toegestaan via de Windows firewall op de *myVm1* VM in de vorige stap.

Sluit de verbinding met extern bureaublad naar *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources die deze bevat:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel leert u gemaakt een standaard virtueel netwerk en twee virtuele machines. U verbonden met één virtuele machine vanaf het Internet en privé uitgewisseld tussen de virtuele machine en een andere virtuele machine. Zie voor meer informatie over virtuele-netwerkinstellingen, [beheren van een virtueel netwerk](manage-virtual-network.md). 

Standaard Azure onbeperkte persoonlijke communicatie tussen virtuele machines, maar kunt alleen binnenkomende verbindingen met extern bureaublad voor Windows-VM's van het Internet. Voor informatie over het toestaan of beperken van verschillende soorten netwerkcommunicatie naar en van virtuele machines, Ga naar de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Filteren van netwerkverkeer](tutorial-filter-network-traffic.md)
