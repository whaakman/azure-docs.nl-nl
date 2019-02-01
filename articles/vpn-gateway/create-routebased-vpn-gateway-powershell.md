---
title: 'Maak een route gebaseerde Azure VPN-gateway: PowerShell | Microsoft Docs'
description: Maak snel een op route gebaseerde VPN-Gateway met behulp van PowerShell
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: 9460f184e3da6769048b30ca743169c5a6044bd0
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55505526"
---
# <a name="create-a-route-based-vpn-gateway-using-powershell"></a>Een op route gebaseerde VPN-gateway maken met behulp van PowerShell

Dit artikel helpt u snel een op route gebaseerde Azure VPN-gateway maken met behulp van PowerShell. Een VPN-gateway wordt gebruikt bij het maken van een VPN-verbinding met uw on-premises netwerk. U kunt ook een VPN-gateway gebruiken om VNets te verbinden.

De stappen in dit artikel maakt een VNet, een subnet, een gatewaysubnet en een op route gebaseerde VPN-gateway (virtuele netwerkgateway). Als het maken van de gateway is voltooid, kunt u vervolgens verbindingen maken. Deze stappen hebt u een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.3.0 of later van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name TestRG1 -Location EastUS
```

## <a name="vnet"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam **VNet1** in de **EastUS** locatie:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName TestRG1 `
  -Location EastUS `
  -Name VNet1 `
  -AddressPrefix 10.1.0.0/16
```

Maak een subnet configureren met de [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet.

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Frontend `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Stel de subnetconfiguratie voor het virtuele netwerk met de [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) cmdlet.


```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="gwsubnet"></a>Een gatewaysubnet toevoegen

Het gatewaysubnet bevat de gereserveerde IP-adressen die de virtuele-netwerkgatewayservices gebruik. Gebruik de volgende voorbeelden om toe te voegen een gateway-subnet:

Stel een variabele in voor uw VNet.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name VNet1
```

Maakt de gateway-subnet via de [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/Add-AzureRmVirtualNetworkSubnetConfig) cmdlet.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $vnet
```

Stel de subnetconfiguratie voor het virtuele netwerk met de [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) cmdlet.

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="PublicIP"></a>Vraag een openbaar IP-adres

Een VPN-gateway moet een dynamisch toegewezen openbare IP-adres hebben. Wanneer u een verbinding met een VPN-gateway maakt, is dit het IP-adres dat u opgeeft. Gebruik het volgende voorbeeld om aan te vragen van een openbaar IP-adres:

```azurepowershell-interactive
$gwpip= New-AzureRmPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>De configuratie van gateway IP-adres maken

De gatewayconfiguratie bepaalt welk subnet en openbaar IP-adres moeten worden gebruikt. Gebruik het volgende voorbeeld om de gatewayconfiguratie te maken:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```
## <a name="CreateGateway"></a>De VPN-gateway maken

Het maken van een VPN-gateway kan tot 45 minuten of langer duren. Als de gateway is voltooid, kunt u een verbinding maken tussen uw virtuele netwerk en een ander VNet. Of maak een verbinding tussen uw virtuele netwerk en een on-premises locatie. Maak een VPN-gateway met de cmdlet [New-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/New-AzureRmVirtualNetworkGateway).

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="viewgw"></a>De VPN-gateway weergeven

U ziet dat de VPN-gateway met behulp van de [Get-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGateway) cmdlet.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkGateway -Name Vnet1GW -ResourceGroup TestRG1
```

De uitvoer ziet er vergelijkbaar met het volgende voorbeeld:

```
Name                   : VNet1GW
ResourceGroupName      : TestRG1
Location               : eastus
Id                     : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provide
                         rs/Microsoft.Network/virtualNetworkGateways/VNet1GW
Etag                   : W/"0952d-9da8-4d7d-a8ed-28c8ca0413"
ResourceGuid           : dc6ce1de-2c4494-9d0b-20b03ac595
ProvisioningState      : Succeeded
Tags                   :
IpConfigurations       : [
                           {
                             "PrivateIpAllocationMethod": "Dynamic",
                             "Subnet": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet"
                             },
                             "PublicIpAddress": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP"
                             },
                             "Name": "default",
                             "Etag": "W/\"0952d-9da8-4d7d-a8ed-28c8ca0413\"",
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                         RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/de
                         fault"
                           }
                         ]
GatewayType            : Vpn
VpnType                : RouteBased
EnableBgp              : False
ActiveActive           : False
GatewayDefaultSite     : null
Sku                    : {
                           "Capacity": 2,
                           "Name": "VpnGw1",
                           "Tier": "VpnGw1"
                         }
VpnClientConfiguration : null
BgpSettings            : {
     
```

## <a name="viewgwpip"></a>Het openbare IP-adres weergeven

Als u het openbare IP-adres voor uw VPN-gateway, gebruikt u de [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/Get-AzureRmPublicIpAddress) cmdlet.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1
```

In het voorbeeldantwoord is de waarde van de IP-adres het openbare IP-adres.

```
Name                     : VNet1GWIP
ResourceGroupName        : TestRG1
Location                 : eastus
Id                       : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provi
                           ders/Microsoft.Network/publicIPAddresses/VNet1GWIP
Etag                     : W/"5001666a-bc2a-484b-bcf5-ad488dabd8ca"
ResourceGuid             : 3c7c481e-9828-4dae-abdc-f95b383
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Dynamic
IpAddress                : 13.90.153.3
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                           RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/
                           default"
                           }
DnsSettings              : null
Zones                    : {}
Sku                      : {
                             "Name": "Basic"
                           }
IpTags                   : {}
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de gemaakte resources niet meer nodig hebt, gebruikt u de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) om de resourcegroep te verwijderen. Hiermee verwijdert u de resourcegroep en alle resources die deze bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name TestRG1
```

## <a name="next-steps"></a>Volgende stappen

Zodra de gateway heeft gemaakt, kunt u een verbinding maken tussen uw virtuele netwerk en een ander VNet. Of maak een verbinding tussen uw virtuele netwerk en een on-premises locatie.

> [!div class="nextstepaction"]
> [Maak een site-naar-site-verbinding](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Een punt-naar-site-verbinding maken](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Maak een verbinding met een ander VNet](vpn-gateway-vnet-vnet-rm-ps.md)
