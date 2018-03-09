---
title: Virtuele netwerken te verbinden met het virtuele netwerk peering - PowerShell | Microsoft Docs
description: Ontdek hoe u virtuele netwerken te verbinden met het virtuele netwerk peering.
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
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c7b3fa2b566ab02e7fb4a03055db83f1545895e8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Virtuele netwerken te verbinden met het virtuele netwerk peering met behulp van PowerShell

U kunt virtuele netwerken met elkaar verbinden met het virtuele netwerk peering. Als u virtuele netwerken brengen, zijn resources in beide virtuele netwerken met elkaar communiceren met de dezelfde latentie en bandbreedte, alsof de bronnen zich in hetzelfde virtuele netwerk. In dit artikel bevat informatie over het maken en peering van twee virtuele netwerken. Procedures voor:

> [!div class="checklist"]
> * Twee virtuele netwerken maken
> * Een peering tussen virtuele netwerken maken
> * Test-peering

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, wordt in dit artikel Azure PowerShell moduleversie 3,6 of hoger vereist. Voer ` Get-Module -ListAvailable AzureRM` de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

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

Het adresvoorvoegsel voor de *myVirtualNetwork2* virtueel netwerk niet overlapt met het adresvoorvoegsel van de *myVirtualNetwork1* virtueel netwerk. U kunt geen virtuele netwerken met overlappende adresvoorvoegsels peer.

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

Peerings tussen twee virtuele netwerken zijn, maar niet transitief zijn. Ja, bijvoorbeeld, als u wilt ook peer *myVirtualNetwork2* naar *myVirtualNetwork3*, moet u een extra peering tussen virtuele netwerken maken *myVirtualNetwork2* en *myVirtualNetwork3*. Hoewel *myVirtualNetwork1* is gekoppeld *myVirtualNetwork2*, resources binnen *myVirtualNetwork1* kan alleen toegang tot bronnen in  *myVirtualNetwork3* als *myVirtualNetwork1* is ook gekoppeld *myVirtualNetwork3*. 

Voordat de peering productie virtuele netwerken, het raadzaam dat u zorgvuldig vertrouwd raken met de [peering overzicht](virtual-network-peering-overview.md), [beheren peering](virtual-network-manage-peering.md), en [limieten voor virtueel netwerk ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Hoewel dit artikel ziet u een peering tussen twee virtuele netwerken in hetzelfde abonnement en locatie, kunt u ook virtuele netwerken in peer [verschillende regio's](#register) en [verschillende Azure-abonnementen](create-peering-different-subscriptions.md#powershell). U kunt ook maken [hub en spoke-netwerk ontwerpen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering.

## <a name="test-peering"></a>Test-peering

Een virtuele machine implementeren in elk subnet om te testen netwerkcommunicatie tussen virtuele machines in verschillende virtuele netwerken via een peering, en vervolgens communiceren tussen de virtuele machines. 

### <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtuele machine in elk virtueel netwerk maken, zodat u kunt de communicatie tussen deze in een later stadium valideren.

Maak een virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1* in de *myVirtualNetwork1* virtueel netwerk. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap. Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u wilt aanmelden bij de virtuele machine met.

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

10.0.0.4 Azure automatisch toegewezen als de persoonlijke IP-adres van de virtuele machine, omdat het eerste beschikbare IP-adres in 10.0.0.4 *Subnet1* van *myVirtualNetwork1*. 

Maken van een virtuele machine in de *myVirtualNetwork2* virtueel netwerk.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

De virtuele machine duurt een paar minuten maken. Hoewel het niet in de uitvoer van de geretourneerde Azure toegewezen 10.1.0.4 als het privé IP-adres van de virtuele machine omdat 10.1.0.4 is het eerste beschikbare IP-adres in *Subnet1* van *myVirtualNetwork2*. 

Ga niet verder met latere stappen totdat Azure maakt van de virtuele machine en wordt de uitvoer geretourneerd naar PowerShell.

### <a name="test-virtual-machine-communication"></a>Communicatie van de virtuele machine testen

U kunt verbinding maken met het openbare IP-adres van een virtuele machine vanaf het Internet. Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVm1* virtuele machine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVm1* virtuele machine van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt, naar de computer gedownload en geopend. Geef de gebruikersnaam en wachtwoord (moet u de selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven), en klik vervolgens op  **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

Vanaf een opdrachtprompt ping via de Windows firewall inschakelen zodat u deze virtuele machine kunt pingen *myVm2* in een later stadium.

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Hoewel ping in dit artikel wordt gebruikt voor het testen, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

Verbinding maken met de *myVm2* virtuele machine, voer de volgende opdracht vanaf een opdrachtprompt op de *myVm1* virtuele machine:

```
mstsc /v:10.1.0.4
```

Aangezien u ping ingeschakeld op *myVm1*, kunt u nu een ping het IP-adres vanaf een opdrachtprompt op de *myVm2* virtuele machine:

```
ping 10.0.0.4
```

U ontvangt vier antwoorden. Als u op de naam van de virtuele machine pingen (*myVm1*), in plaats van het IP-adres, pingen mislukt, omdat *myVm1* is een onbekende host-naam. Azure standaard-naamomzetting werkt tussen virtuele machines in hetzelfde virtuele netwerk, maar niet tussen virtuele machines in verschillende virtuele netwerken. Voor het omzetten van namen in virtuele netwerken, moet u [uw eigen DNS-server implementeren](virtual-networks-name-resolution-for-vms-and-role-instances.md) of gebruik [persoonlijke Azure DNS-domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

De RDP-sessies op beide *myVm1* en *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

**<a name="register"></a>Registreren voor de peering preview globale virtueel netwerk**

Peering van virtuele netwerken in dezelfde regio's is algemeen beschikbaar. Virtuele netwerken in verschillende regio's is momenteel in preview-peering. Zie [virtuele netwerk updates](https://azure.microsoft.com/updates/?product=virtual-network) voor beschikbare regio's. Virtuele netwerken to-peer tussen regio's, moet u eerst registreren voor de preview, door de volgende stappen (binnen het abonnement elk virtueel netwerk dat u wilt peer wordt):

1. Registreer het abonnement dat elke virtuele netwerk dat u wilt peer in voor de preview hiertoe de volgende opdrachten:

    ```powershell-interactive
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
2. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```powershell-interactive    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Als u probeert te peer virtuele netwerken in verschillende regio's voordat de **RegistrationState** uitvoer wordt weergegeven nadat het invoeren van de vorige opdracht is **geregistreerde** voor beide abonnementen peering mislukt .

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken te verbinden met het virtuele netwerk peering. U kunt [uw eigen computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via een VPN, en communiceren met resources in een virtueel netwerk of in virtuele netwerken peer is ingesteld.

Scriptvoorbeelden van het voor herbruikbare scripts om uit te voeren veel van de taken besproken in het virtuele netwerk artikelen blijven.

> [!div class="nextstepaction"]
> [Voorbeelden van virtueel netwerk script](../networking/powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
