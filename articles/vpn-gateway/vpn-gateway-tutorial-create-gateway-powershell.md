---
title: Een Azure VPN-gateway maken en beheren met behulp van PowerShell | Microsoft Docs
description: 'Zelfstudie: Een Azure VPN-gateway maken en beheren met behulp van Microsoft Azure PowerShell'
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 05/14/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 0f10384e7e21d65b3a16869a10f8294b9643c74c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210195"
---
# <a name="create-and-manage-vpn-gateway-with-the-azure-powershell-module"></a>Een Azure VPN-gateway maken en beheren met behulp van Microsoft Azure PowerShell

Azure VPN-gateways bieden veilige, cross-premises connectiviteit tussen de klanten-premises en Azure. Deze zelfstudie bevat informatie over implementatie-basiselementen voor Azure VPN-gateways, zoals het maken en beheren van een VPN-gateway. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een VPN-gateway maken
> * Het formaat van een VPN-gateway wijzigen
> * Een VPN-gateway opnieuw instellen

Het volgende diagram toont het virtuele netwerk en de VPN-gateway die zijn gemaakt als onderdeel van deze zelfstudie.

![VNet en VPN-gateway](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell en Azure PowerShell

[!INCLUDE [working with cloudshell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.3 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="common-network-parameter-values"></a>Algemene parameterwaarden van het netwerk

Wijzig de waarden hieronder op basis van uw omgeving en netwerkinstellingen.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Eerst moet u een resourcegroep maken. In het volgende voorbeeld wordt een resourcegroep met de naam *TestRG1* gemaakt in de regio *VS Oost*:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Azure VPN-gateway biedt cross-premises connectiviteit en P2S-VPN-server-functionaliteit voor uw virtuele netwerk. Voeg de VPN-gateway toe aan een bestaand virtueel netwerk of maak een nieuw virtueel netwerk en de gateway. In dit voorbeeld maakt u een nieuw virtueel netwerk met drie subnetten: Frontend, Backend en GatewaySubnet met [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) en [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubnet1 -AddressPrefix $GwPrefix1
$vnet   = New-AzureRmVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>Een openbaar IP-adres voor de VPN-gateway aanvragen

Azure VPN-gateways communiceren met uw on-premises VPN-apparaten via Internet om IKE (Internet Key Exchange)-onderhandeling uit te voeren en IPsec-tunnels tot stand te brengen. Maak een openbaar IP-adres en wijs dit toe aan uw VPN-gateway, zoals wordt weergegeven in het onderstaande voorbeeld met [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) en [New-AzureRmVirtualNetworkGatewayIpConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayipconfig):

> [!IMPORTANT]
> Op dit moment kunt u alleen een dynamisch openbaar IP-adres gebruiken voor de gateway. Statische IP-adressen worden niet ondersteund op Azure VPN-gateways.

```azurepowershell-interactive
$gwpip    = New-AzureRmPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-vpn-gateway"></a>Een VPN-gateway maken

Het maken van een VPN-gateway kan tot 45 minuten of langer duren. Nadat het maken van de gateway is voltooid, kunt u een verbinding tussen uw virtuele netwerk en een ander VNet maken. Of maak een verbinding tussen uw virtuele netwerk en een on-premises locatie. Maak een VPN-gateway met de cmdlet [New-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/New-AzureRmVirtualNetworkGateway).

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Sleutelparameterwaarden:
* GatewayType: Gebruik **Vpn** voor site-naar-site- en VNet-naar-VNet-verbindingen
* VpnType: Gebruik **RouteBased** om te communiceren met meer verschillende VPN-apparaten en meer routeringsfuncties
* GatewaySku: **VpnGw1** is de standaardinstelling; wijzig deze naar VpnGw2 of VpnGw3 als u hogere doorvoercapaciteit of meer verbindingen nodig hebt. Zie [Gateway-SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor meer informatie.

Nadat het maken van de gateway is voltooid, kunt u een verbinding maken tussen uw virtuele netwerk en een ander VNet of een verbinding maken tussen uw virtuele netwerk en een on-premises locatie. U kunt ook een P2S-verbinding configureren voor uw VNet vanaf een clientcomputer.

## <a name="resize-vpn-gateway"></a>Het formaat van VPN-gateway wijzigen

U kunt de VPN-gateway-SKU wijzigen nadat de gateway is gemaakt. Verschillende gateway-SKU's ondersteunen verschillende specificaties zoals doorvoercapaciteit, aantal verbindingen, enzovoort. Het volgende voorbeeld gebruikt [Resize-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Resize-AzureRmVirtualNetworkGateway) om het formaat van uw gateway van VpnGw1 naar VpnGw2 te wijzigen. Zie [Gateway-SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor meer informatie.

```azurepowershell-interactive
$gw = Get-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzureRmVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Vergroten of verkleinen van een VPN-gateway duurt ook ongeveer 30 tot 45 minuten, hoewel deze bewerking bestaande verbindingen en configuraties **niet** onderbreekt of verwijdert.

## <a name="reset-vpn-gateway"></a>VPN-gateway opnieuw instellen

Als onderdeel van de stappen voor probleemoplossing, kunt u uw Azure VPN-gateway opnieuw instellen om de VPN-gateway te dwingen de IPsec-/IKE-tunnelconfiguraties opnieuw op te starten. Gebruik [Reset-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Reset-AzureRmVirtualNetworkGateway) voor het opnieuw instellen van uw gateway.

```azurepowershell-interactive
$gw = Get-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Zie [Reset a VPN gateway](vpn-gateway-resetgw-classic.md) (Een VPN-gateway opnieuw instellen) voor meer informatie.

## <a name="get-the-gateway-public-ip-address"></a>Het openbare IP-adres van de gateway ophalen

Als u de naam van het openbare IP-adres weet, gebruikt u [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/Reset-AzureRmPublicIpAddress) om het openbare IP-adres weer te geven dat is toegewezen aan de gateway.

```azurepowershell-interactive
$myGwIp = Get-AzureRmPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="delete-vpn-gateway"></a>VPN-gateway verwijderen

Een volledige configuratie van cross-premises en VNet-naar-VNet-connectiviteit vereist meerdere resourceypes naast de VPN-gateway. Verwijder de verbindingen die zijn gekoppeld aan de VPN-gateway voordat u de gateway zelf verwijdert. Zodra de gateway is verwijderd, kunt u vervolgens de openbare IP-adressen voor de gateway verwijderen. Zie [Delete a VPN gateway](vpn-gateway-delete-vnet-gateway-powershell.md) (Een VPN-gateway verwijderen) voor gedetailleerde stappen.

Als de gateway deel uitmaakt van een protype of bewijs van concept-implementatie, kunt u de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken voor het verwijderen van de resourcegroep, de VPN-gateway en alle bijbehorende resources.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u informatie gehad over basistaken voor het maken en beheren van een virtuele machine, zoals:

> [!div class="checklist"]
> * Een VPN-gateway maken
> * Het formaat van een VPN-gateway wijzigen
> * Een VPN-gateway opnieuw instellen

Ga naar de volgende zelfstudies voor meer informatie over S2S-, VNet-naar-VNet- en P2S-verbindingen.

> [!div class="nextstepaction"]
> * [S2S-verbindingen maken](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [VNet-naar-VNet-verbindingen maken](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S-verbindingen maken](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
