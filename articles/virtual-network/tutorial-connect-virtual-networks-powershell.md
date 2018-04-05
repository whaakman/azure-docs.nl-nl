---
title: Virtuele netwerken te verbinden met het virtuele netwerk peering - PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u virtuele netwerken te verbinden met het virtuele netwerk peering, met behulp van Azure PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 445baa36f33cbe02b68bdb37406f842932089183
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Virtuele netwerken te verbinden met het virtuele netwerk peering met behulp van PowerShell

U kunt virtuele netwerken met elkaar verbinden met het virtuele netwerk peering. Als u virtuele netwerken brengen, zijn resources in beide virtuele netwerken met elkaar communiceren met de dezelfde latentie en bandbreedte, alsof de bronnen zich in hetzelfde virtuele netwerk. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Twee virtuele netwerken maken
> * Twee virtuele netwerken te verbinden met een virtueel netwerk peering
> * Een virtuele machine (VM) in elk virtueel netwerk implementeren
> * Communicatie tussen virtuele machines

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell moduleversie 5.4.1 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="create-virtual-networks"></a>Virtuele netwerken maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en andere resources die zijn gemaakt in dit artikel. Maak een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork1* met het adresvoorvoegsel *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Maken van een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld wordt een subnet is geconfigureerd met een adresvoorvoegsel 10.0.0.0/24:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

De configuratie van subnetten met schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), die het subnet wordt gemaakt:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

Een virtueel netwerk maken met een adresvoorvoegsel 10.1.0.0/16 en één subnet:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Peer virtuele netwerken

Maken van een peering met [toevoegen AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering). Het volgende voorbeeld peers *myVirtualNetwork1* naar *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

In de uitvoer geretourneerd nadat de vorige opdracht wordt uitgevoerd, ziet u dat de **PeeringState** is *gestarte*. De peering blijft in de *gestarte* status totdat u de peering van *myVirtualNetwork2* naar *myVirtualNetwork1*. Maken van een peering van *myVirtualNetwork2* naar *myVirtualNetwork1*. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

In de uitvoer geretourneerd nadat de vorige opdracht wordt uitgevoerd, ziet u dat de **PeeringState** is *verbonden*. Azure ook gewijzigd de peering status van de *myVirtualNetwork1 myVirtualNetwork2* peering naar *verbonden*. Controleer of de status van de peering voor het *myVirtualNetwork1 myVirtualNetwork2* peering gewijzigd in *verbonden* met [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Resources in een virtueel netwerk kan niet communiceren met resources in het virtuele netwerk tot de **PeeringState** voor de peerings in beide virtuele netwerken is *verbonden*. 

## <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtuele machine in elk virtueel netwerk maken, zodat u tussen deze twee een latere stap kan communiceren.

### <a name="create-the-first-vm"></a>De eerste virtuele machine maken

Maak een VM met [nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1* in de *myVirtualNetwork1* virtueel netwerk. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap. Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u wilt aanmelden bij de virtuele machine met.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>De tweede virtuele machine maken

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

De virtuele machine duurt een paar minuten maken. Ga niet verder met latere stappen totdat Azure maakt van de virtuele machine en wordt de uitvoer geretourneerd naar PowerShell.

## <a name="communicate-between-vms"></a>Communicatie tussen virtuele machines

U kunt verbinding maken met het openbare IP-adres van een virtuele machine vanaf het internet. Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVm1* VM van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt, naar de computer gedownload en geopend. Geef de gebruikersnaam en wachtwoord (moet u de selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven), en klik vervolgens op **OK** . Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

Op de *myVm1* VM, het Internet Control Message Protocol (ICMP) via de Windows firewall zodat u kunt deze VM van pingen inschakelen *myVm2* in een latere stap, met behulp van PowerShell:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Hoewel ping wordt gebruikt voor communicatie tussen VM's in dit artikel, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

Verbinding maken met de *myVm2* virtuele machine, voer de volgende opdracht vanaf een opdrachtprompt op de *myVm1* VM:

```
mstsc /v:10.1.0.4
```

Aangezien u ping ingeschakeld op *myVm1*, kunt u nu een ping het IP-adres vanaf een opdrachtprompt op de *myVm2* VM:

```
ping 10.0.0.4
```

U ontvangt vier antwoorden. De RDP-sessies op beide *myVm1* en *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken in dezelfde Azure-regio, verbinden met het virtuele netwerk peering. U kunt ook virtuele netwerken in verschillende peer [ondersteunde regio's](virtual-network-manage-peering.md#cross-region) en in [verschillende Azure-abonnementen](create-peering-different-subscriptions.md#powershell), evenals maken [hub en spoke-netwerk ontwerpen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering. Zie voor meer informatie over het virtuele netwerk peering, [peering Virtual network-overzicht](virtual-network-peering-overview.md) en [beheren van virtueel netwerk peerings](virtual-network-manage-peering.md).

U kunt [uw eigen computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via een VPN, en communiceren met resources in een virtueel netwerk of in virtuele netwerken peer is ingesteld. Zie voor herbruikbare scripts om uit te voeren veel van de taken besproken in het virtuele netwerk artikelen [voorbeelden script](powershell-samples.md).
