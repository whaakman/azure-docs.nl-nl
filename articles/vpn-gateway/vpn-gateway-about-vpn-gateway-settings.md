---
title: VPN-gateway-instellingen voor cross-premises Azure-verbindingen | Microsoft Docs
description: Meer informatie over VPN-Gateway-instellingen voor gateways voor virtuele Azure-netwerk.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 07aa6946b9c3994c5afc5c88837f23567b95d8a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a>Over configuratie-instellingen voor VPN-Gateway

Een VPN-gateway is een type van de virtuele netwerkgateway die versleuteld verkeer tussen uw virtuele netwerk en uw on-premises locatie via een openbare verbinding verzonden. U kunt ook een VPN-gateway gebruiken om verkeer tussen virtuele netwerken via de Azure-backbone te verzenden.

Er is een VPN-gatewayverbinding afhankelijk van de configuratie van meerdere resources, die elk configureerbare instellingen bevat. De secties in dit artikel worden de resources en de instellingen met betrekking tot een VPN-gateway voor een virtueel netwerk gemaakt in Resource Manager-implementatiemodel. U vindt beschrijvingen en diagrammen topologie voor elke oplossing verbinding in de [over VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.

## <a name="gwtype"></a>Gatewaytypen

Elk virtueel netwerk kan slechts één virtuele netwerkgateway van elk type hebben. Wanneer u een virtuele netwerkgateway maakt, moet u ervoor zorgen dat het Gatewaytype voor uw configuratie klopt.

De beschikbare waarden voor - GatewayType zijn:

* VPN
* ExpressRoute

Een VPN-gateway vereist de `-GatewayType` *Vpn*.

Voorbeeld:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-the-gateway-sku"></a>De gateway-SKU configureren

#### <a name="azure-portal"></a>Azure Portal

Als u de Azure-portal voor het maken van een virtuele netwerkgateway van Resource Manager gebruikt, selecteert u de gateway-SKU met behulp van de vervolgkeuzelijst. De opties die u krijgt correspondeert met het type van de Gateway en de VPN-type dat u selecteert.

#### <a name="powershell"></a>PowerShell

Het volgende PowerShell-voorbeeld worden de `-GatewaySku` als VpnGw1.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Wijzig de grootte van (wijzigen) een gateway-SKU

Als u wilt dat uw gateway-SKU bijwerken naar een krachtigere SKU, kunt u de `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet. Ook kunt u de gateway-SKU-grootte die met deze cmdlet downgraden.

De volgende PowerShell-voorbeeld ziet u een gateway-SKU voor VpnGw2 wordt gewijzigd.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Verbindingstypen

Elke configuratie vereist in het Resource Manager-implementatiemodel, het type van een specifieke virtuele-netwerkgateway verbinding. De beschikbare Resource Manager PowerShell-waarden voor `-ConnectionType` zijn:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

In het volgende PowerShell-voorbeeld, maken we een S2S-verbinding waarvoor het verbindingstype *IPsec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN-typen

Wanneer u de virtuele netwerkgateway voor de configuratie van een VPN-gateway maakt, moet u een VPN-type. Het VPN-type dat u kiest, is afhankelijk van de verbinding-topologie die u wilt maken. Bijvoorbeeld, vereist een P2S-verbinding een RouteBased VPN-type. Een VPN-type kan ook afhankelijk van de hardware die u gebruikt. S2S-configuraties vereisen een VPN-apparaat. Sommige VPN-apparaten ondersteunen alleen een bepaalde VPN-type.

Het VPN-type dat u selecteert, moet voldoen aan alle verbinding vereisten voor de oplossing die dat u wilt maken. Bijvoorbeeld, als u een S2S VPN-gatewayverbinding en een P2S-VPN-gatewayverbinding voor hetzelfde virtuele netwerk maken wilt, gebruikt u VPN-type *RouteBased* omdat P2S een RouteBased VPN-type vereist. U moet ook verifiëren of uw VPN-apparaat een RouteBased VPN-verbinding ondersteund. 

Wanneer een virtuele netwerkgateway is gemaakt, kunt u het VPN-type niet wijzigen. U moet de virtuele netwerkgateway verwijderen en een nieuwe maken. Er zijn twee VPN-typen:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Het volgende PowerShell-voorbeeld worden de `-VpnType` als *RouteBased*. Wanneer u een gateway maakt, moet u het juiste VPN-type voor uw configuratie kiezen.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Vereisten voor de gateway

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Gatewaysubnet

Voordat u een VPN-gateway maakt, moet u een gatewaysubnet maken. Het gatewaysubnet bevat de IP-adressen die gebruikmaken van de virtuele netwerkgateway virtuele machines en services. Wanneer u uw virtuele netwerkgateway maakt, worden gateway-VM's in het gatewaysubnet is geïmplementeerd en geconfigureerd met de vereiste instellingen voor VPN-gateway. U moet nooit iets anders (bijvoorbeeld extra VM's) om het gatewaysubnet te implementeren. Het gatewaysubnet moet de naam 'GatewaySubnet' goed te laten werken. Naamgeving van het gatewaysubnet 'GatewaySubnet', kunt weet dat deze het subnet voor het implementeren van de virtuele netwerkgateway virtuele machines en services met Azure.

Wanneer u het gatewaysubnet maakt, geeft u op hoeveel IP-adressen het subnet bevat. De IP-adressen in het gatewaysubnet worden toegewezen aan de gateway-VM's en gatewayservices. Sommige configuraties moeten meer IP-adressen dan andere. Bekijk de instructies voor de configuratie die u wilt maken en te controleren of het gatewaysubnet dat u wilt maken, voldoet aan deze vereisten. Bovendien wilt u Zorg ervoor dat het gatewaysubnet bevat voldoende IP-adressen zodat mogelijke toekomstige aanvullende configuraties. U kunt een gatewaysubnet slechts/29 maken, wordt aangeraden dat u een gatewaysubnet van/28 of groter maakt (/ 28, / 27, /26 enz.). Op die manier als u de functionaliteit in de toekomst toevoegt u hoeft te verwijderen van uw gateway en vervolgens verwijderen en opnieuw maken van het gatewaysubnet om toe te staan voor meer IP-adressen.

De volgende Resource Manager PowerShell-voorbeeld ziet een gatewaysubnet met de naam GatewaySubnet. U ziet dat de CIDR-notatie een/27, geeft dit biedt voldoende IP-adressen voor de meeste configuraties die momenteel aanwezig zijn.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Lokale netwerkgateways

Wanneer u de configuratie van een VPN-gateway maakt, wordt de lokale netwerkgateway vaak uw on-premises locatie vertegenwoordigt. In het klassieke implementatiemodel werd de lokale gateway een lokale site genoemd. 

U geeft de lokale netwerkgateway een naam, het openbare IP-adres van de on-premises VPN-apparaat en de adresvoorvoegsels die op de on-premises locatie bevinden zich opgeven. Azure kijkt naar de bestemmingsadressen voor netwerkverkeer, raadpleegt de configuratie die u voor uw lokale netwerkgateway hebt opgegeven en routeert pakketten dienovereenkomstig. U wordt ook lokale netwerkgateways voor VNet-naar-VNet-configuraties die gebruikmaken van een VPN-gatewayverbinding.

Het volgende PowerShell-voorbeeld wordt een nieuwe lokale netwerkgateway:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Soms moet u de lokale gateway-instellingen wijzigen. Bijvoorbeeld, wanneer u toevoegen of wijzigen van het adresbereik, of als het IP-adres van de VPN-apparaat is gewijzigd. U kunt deze instellingen in de klassieke portal op de pagina lokale netwerken wijzigen voor een klassiek VNet. Zie voor Resource Manager [lokale gateway netwerkinstellingen wijzigen met behulp van PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST-API's en PowerShell-cmdlets

Zie voor aanvullende technische bronnen en de syntaxis van de specifieke vereisten voor het met REST-API's, PowerShell-cmdlets of Azure CLI voor VPN-Gateway-configuraties, de volgende pagina's:

| **Klassiek** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Niet ondersteund | [Azure-CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beschikbare verbinding configuraties [over VPN-Gateway](vpn-gateway-about-vpngateways.md).