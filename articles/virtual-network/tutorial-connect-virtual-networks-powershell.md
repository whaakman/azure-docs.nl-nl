---
title: Virtuele netwerken verbinden met virtueel-netwerkpeering - PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u virtuele netwerken verbinden met virtueel-netwerkpeering met behulp van Azure PowerShell.
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
ms.openlocfilehash: 49a6c91587905a8f7086b46b275a5078197939eb
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649948"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Virtuele netwerken verbinden met virtueel-netwerkpeering met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt virtuele netwerken met elkaar verbinden met virtueel-netwerk peering. Wanneer virtuele netwerken als peers zijn gekoppeld, kunnen resources in beide virtuele netwerken met elkaar communiceren met dezelfde latentie en bandbreedte als wanneer de resources zich in hetzelfde virtuele netwerk zouden bevinden. In dit artikel leert u het volgende:

* Twee virtuele netwerken maken
* Twee virtuele netwerken koppelen met virtueel-netwerkpeering
* Een virtuele machine (VM) implementeren op elk van de virtuele netwerken
* Communiceren tussen VM's

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-virtual-networks"></a>Virtuele netwerken maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en alle andere resources in dit artikel hebt gemaakt. Maak een resourcegroep met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maak een virtueel netwerk met [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork1* met het adresvoorvoegsel *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Maak een subnetconfiguratie met [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Het volgende voorbeeld wordt een subnetconfiguratie met een adresvoorvoegsel van 10.0.0.0/24:

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Schrijf de subnetconfiguratie naar het virtuele netwerk met [Set AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), waarmee het subnet wordt gemaakt:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzVirtualNetwork
```

Een virtueel netwerk maken met een adresvoorvoegsel 10.1.0.0/16 en één subnet:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Peering van virtuele netwerken

Maakt een koppeling met [toevoegen AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering). Het volgende voorbeeld peers *myVirtualNetwork1* naar *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

In de uitvoer die wordt geretourneerd na de vorige opdracht is uitgevoerd, ziet u dat de **PeeringState** is *gestart*. De peering blijft in de *gestart* status totdat u de peering vanuit *myVirtualNetwork2* naar *myVirtualNetwork1*. Maken van een peering van *myVirtualNetwork2* naar *myVirtualNetwork1*.

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

In de uitvoer die wordt geretourneerd na de vorige opdracht is uitgevoerd, ziet u dat de **PeeringState** is *verbonden*. Azure heeft ook gewijzigd voor de status van de peering van het *myVirtualNetwork1 myVirtualNetwork2* peering aan *verbonden*. Controleer de status van de peering voor het *myVirtualNetwork1 myVirtualNetwork2* peering gewijzigd in *verbonden* met [Get-AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Resources in een virtueel netwerk kan niet communiceren met resources in het virtuele netwerk totdat de **PeeringState** voor de peerings in beide virtuele netwerken is *verbonden*.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak een VM in elk virtueel netwerk, zodat u er in een latere stap tussen kunt communiceren.

### <a name="create-the-first-vm"></a>De eerste VM maken

Maak een VM met [New-AzVM](/powershell/module/az.compute/new-azvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1* in de *myVirtualNetwork1* virtueel netwerk. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap. Wanneer u hierom wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u wilt aanmelden bij de virtuele machine met.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>De tweede VM maken

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Het maken van de virtuele machine duurt een paar minuten. Ga niet verder met latere stappen voordat Azure de virtuele machine maakt en wordt uitvoer geretourneerd naar PowerShell.

## <a name="communicate-between-vms"></a>Communiceren tussen VM's

U kunt verbinding maken met het openbare IP-adres van een virtuele machine via internet. Gebruik [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) om terug te keren van het openbare IP-adres van een virtuele machine. In het volgende voorbeeld wordt het openbare IP-adres van de VM *myVm1* opgehaald:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Gebruik de volgende opdracht om te maken van een extern-bureaubladsessie met de *myVm1* VM vanaf uw lokale computer. Vervang `<publicIpAddress>` door het IP-adres dat is geretourneerd met de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt, naar uw computer gedownload en geopend. Voer de gebruikersnaam en het wachtwoord (mogelijk moet u selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**om op te geven van de referenties die u hebt opgegeven tijdens het maken van de virtuele machine), en klik vervolgens op **OK** . Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

Op de *myVm1* VM, het Internet Control Message Protocol (ICMP) via de Windows firewall-, zodat u kunt deze virtuele machine uit pingen inschakelen *myVm2* in een latere stap, met behulp van PowerShell:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Hoewel ping wordt gebruikt voor communicatie tussen virtuele machines in dit artikel, wordt het ICMP via de Windows Firewall voor productie-implementaties toe te staan niet aanbevolen.

Maak verbinding met *myVm2* door op *myVm1* de volgende opdracht in te voeren vanaf een opdrachtprompt:

```
mstsc /v:10.1.0.4
```

Aangezien u ping hebt ingeschakeld op *myVm1*, u kunt deze nu pingen met IP-adres vanaf een opdrachtprompt op de *myVm2* VM:

```
ping 10.0.0.4
```

U ontvangt vier reacties. Verbreek de RDP-sessies met zowel *myVm1* als *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, gebruikt u [Remove-AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle resources die deze bevat te verwijderen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken in dezelfde Azure-regio, verbinden met virtueel-netwerkpeering. U kunt ook virtuele netwerken in verschillende [ondersteunde regio's](virtual-network-manage-peering.md#cross-region) en in [ verschillende Azure-abonnementen ](create-peering-different-subscriptions.md#powershell) 'peeren', en peering gebruiken om [netwerkontwerpen met een stertopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) te maken. Zie voor meer informatie over virtueel-netwerkpeering [Peering op virtueel netwerk](virtual-network-peering-overview.md) en [Virtueel-netwerkpeerings beheren](virtual-network-manage-peering.md).

U kunt [uw eigen computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via een VPN-verbinding, en communiceren met resources in een virtueel netwerk of in gekoppelde virtuele netwerken. Zie voor herbruikbare scripts om uit te voeren veel van de taken die worden besproken in het virtuele netwerk-artikelen, [script voorbeelden](powershell-samples.md).