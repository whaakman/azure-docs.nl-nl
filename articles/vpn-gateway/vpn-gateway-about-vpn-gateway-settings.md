---
title: Instellingen voor VPN-gateway voor cross-premises Azure-verbindingen | Microsoft Docs
description: Meer informatie over VPN-Gateway-instellingen voor virtuele netwerkgateways van Azure.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: cherylmc
ms.openlocfilehash: 76323ab00a3562cae10520b18008d030e40043fc
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57864667"
---
# <a name="about-vpn-gateway-configuration-settings"></a>Over VPN-Gateway-instellingen

Een VPN-gateway is een type virtuele netwerkgateway waarmee versleuteld verkeer tussen uw virtuele netwerk en uw on-premises locatie via een openbare verbinding verzendt. U kunt ook een VPN-gateway gebruiken voor het verzenden van verkeer tussen virtuele netwerken via de Azure-backbone.

Een VPN-gatewayverbinding is afhankelijk van de configuratie van meerdere resources, die elk configureerbare instellingen bevat. De secties in dit artikel worden de resources en de instellingen die betrekking op een VPN-gateway voor een virtueel netwerk hebt gemaakt in Resource Manager-implementatiemodel hebben. U vindt beschrijvingen en topologiediagrammen voor elke oplossing verbinding in de [over VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.

De waarden in dit artikel zijn van toepassing VPN-gateways (virtuele netwerkgateways die - GatewayType Vpn gebruikt). In dit artikel omvat niet alle soorten gateways of zone-redundante gateways.

* Zie voor waarden die betrekking hebben op - GatewayType 'ExpressRoute' [virtuele netwerkgateways voor ExpressRoute](../expressroute/expressroute-about-virtual-network-gateways.md).

* Zie voor de zone-redundante gateways [over zone-redundante gateways](about-zone-redundant-vnet-gateways.md).

* Zie voor virtuele WAN [over virtuele WAN](../virtual-wan/virtual-wan-about.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gwtype"></a>Gatewaytypen

Elk virtueel netwerk kan slechts één virtuele netwerkgateway van elk type hebben. Wanneer u een virtuele netwerkgateway maakt, moet u ervoor zorgen dat de gateway van het type voor de configuratie juist is.

De beschikbare waarden voor - GatewayType zijn:

* VPN
* ExpressRoute

Een VPN-gateway vereist de `-GatewayType` *Vpn*.

Voorbeeld:

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-a-gateway-sku"></a>Configureren van een gateway-SKU

#### <a name="azure-portal"></a>Azure Portal

Als u de Azure portal gebruiken voor het maken van een virtuele netwerkgateway van Resource Manager, kunt u de gateway-SKU selecteren met behulp van de vervolgkeuzelijst. De opties die u krijgt overeenkomen met de Gateway van het type en de VPN-type dat u selecteert.

#### <a name="powershell"></a>PowerShell

Hiermee geeft u op de volgende PowerShell-voorbeeld de `-GatewaySku` als VpnGw1. Wanneer u PowerShell gebruikt om een gateway te maken, hebt u eerst de IP-configuratie maken en vervolgens een variabele gebruiken om te verwijzen naar deze. In dit voorbeeld is de configuratievariabele $gwipconfig.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'US East' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWPIP --resource-group TestRG1 --vnet VNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait
```

###  <a name="resizechange"></a>Het formaat wijzigen van een SKU of

Als u een VPN-gateway hebt en u wilt een andere gateway-SKU gebruiken, wordt uw opties zijn ofwel de grootte van uw gateway-SKU wijzigen of te wijzigen op een andere SKU. Wanneer u naar een andere gateway-SKU overschakelt, kunt u de bestaande gateway volledig te verwijderen en een nieuwe build. Een gateway kan bouwen tot 45 minuten duren. Ter vergelijking, wanneer u het formaat van een gateway-SKU, is er veel downtime omdat u niet hoeft te verwijderen en opnieuw opbouwen van de gateway. Als u de optie voor het formaat van uw gateway-SKU, in plaats van deze wijzigen hebt, wilt u dat doen. Er zijn echter regels met betrekking tot het vergroten of verkleinen:

1. U kunt wisselen tussen VpnGw1-, VpnGw2- en VpnGw3-SKU's.
2. Als u met de oude gateway-SKU's werkt, kunt u wisselen tussen Basic-, Standard- en HighPerformance-SKU's.
3. U kunt**niet** wisselen van Basic-/Standard-/HighPerformance-SKU's naar de nieuwe VpnGw2-/VpnGw1-/VpnGw3-SKU's. In plaats daarvan moet u [wijzigen](#change) naar de nieuwe SKU's.

#### <a name="resizegwsku"></a>Grootte van een gateway wijzigen

[!INCLUDE [Resize a SKU](../../includes/vpn-gateway-gwsku-resize-include.md)]

####  <a name="change"></a>Wijzigen van een oude (verouderde) SKU naar een nieuwe SKU

[!INCLUDE [Change a SKU](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="connectiontype"></a>Verbindingstypen

Elke configuratie vereist in het Resource Manager-implementatiemodel, een specifieke virtuele-gateway-verbindingstype. De beschikbare Resource Manager PowerShell-waarden voor `-ConnectionType` zijn:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

In de volgende PowerShell-voorbeeld maken we een S2S-verbinding waarvoor het verbindingstype *IPsec*.

```azurepowershell-interactive
New-AzVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN-typen

Wanneer u de virtuele netwerkgateway voor de configuratie van een VPN-gateway maakt, moet u een VPN-type opgeven. De VPN-type dat u kiest, is afhankelijk van de verbindingstopologie die u wilt maken. Een P2S-verbinding vereist, bijvoorbeeld een RouteBased VPN-type. Een VPN-type kan ook afhankelijk van de hardware die u gebruikt. S2S-configuraties vereisen een VPN-apparaat. Sommige VPN-apparaten ondersteunen alleen een bepaalde VPN-type.

De VPN-type dat u selecteert moet voldoen aan alle de verbinding voor de oplossing die dat u wilt maken. Bijvoorbeeld, als u maken van een S2S-VPN-gatewayverbinding en een P2S-VPN-gatewayverbinding voor hetzelfde virtuele netwerk wilt, gebruikt u VPN-type *RouteBased* omdat P2S is vereist voor een RouteBased VPN-type. U moet ook controleren of uw VPN-apparaat een RouteBased VPN-verbinding ondersteund. 

Nadat een virtuele netwerkgateway is gemaakt, kunt u het VPN-type niet wijzigen. U moet de virtuele netwerkgateway verwijderen en een nieuwe maken. Er zijn twee VPN-typen:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Hiermee geeft u op de volgende PowerShell-voorbeeld de `-VpnType` als *RouteBased*. Wanneer u een gateway maakt, moet u het juiste VPN-type voor uw configuratie kiezen.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Vereisten voor de gateway

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Gatewaysubnet

Voordat u een VPN-gateway maakt, moet u een gatewaysubnet maken. Het gatewaysubnet bevat de IP-adressen die gebruikmaken van de virtuele netwerkgateway virtuele machines en services. Wanneer u uw virtuele netwerkgateway maakt, zijn de gateway-VM's in het gatewaysubnet geïmplementeerd en geconfigureerd met de vereiste instellingen voor VPN-gateway. Implementeer nooit iets anders (bijvoorbeeld extra VM's) in het gatewaysubnet. Het gatewaysubnet moet de naam 'GatewaySubnet' goed te laten werken. Naamgeving van het gatewaysubnet 'GatewaySubnet', kunt Azure weet dat dit de virtuele netwerkgateway virtuele machines en services implementeren om het subnet is.

>[!NOTE]
>[!INCLUDE [vpn-gateway-gwudr-warning.md](../../includes/vpn-gateway-gwudr-warning.md)]
>

Wanneer u het gatewaysubnet maakt, geeft u op hoeveel IP-adressen het subnet bevat. De IP-adressen in het gatewaysubnet worden toegewezen aan de gateway-VM's en de gateway-services. Sommige configuraties vereisen meer IP-adressen dan andere. Bekijk de instructies voor de configuratie die u wilt maken en te controleren of het gatewaysubnet dat u wilt maken, voldoet aan deze vereisten. Bovendien kunt u om ervoor te zorgen dat uw gatewaysubnet voldoende IP-adressen voor mogelijke toekomstige extra configuraties bevat. U kunt maken met een gatewaysubnet zo klein is als/29, wordt aangeraden dat u een gatewaysubnet van/28 of groter maken (/ 28, 27, / 26 etc.). Op die manier als u functionaliteit in de toekomst worden toevoegen u geen uw gateway verwijderen en vervolgens verwijderen en opnieuw maken van het gatewaysubnet om toe te staan voor meer IP-adressen.

De volgende Resource Manager PowerShell-voorbeeld ziet u een gateway-subnet met de naam GatewaySubnet. U ziet dat de CIDR-notatie een/27, geeft dit biedt voldoende IP-adressen voor de meeste configuraties die momenteel aanwezig zijn.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Lokale netwerkgateways

 Een lokale netwerkgateway is anders dan een virtuele netwerkgateway. Bij het maken van de configuratie van een VPN-gateway, wordt in de lokale netwerkgateway doorgaans uw on-premises locatie vertegenwoordigt. In het klassieke implementatiemodel werd de lokale gateway een lokale site genoemd.

U geeft de lokale netwerkgateway een naam, het openbare IP-adres van de on-premises VPN-apparaat en de adresvoorvoegsels die zich op de on-premises locatie opgeven. Azure kijkt naar de bestemmingsadressen voor netwerkverkeer, raadpleegt de configuratie die u voor uw lokale netwerkgateway hebt opgegeven en routeert pakketten dienovereenkomstig. U geeft ook de lokale netwerkgateways voor VNet-naar-VNet-configuraties die gebruikmaken van een VPN-gatewayverbinding.

De volgende PowerShell-voorbeeld maakt u een nieuwe lokale netwerkgateway:

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Soms moet u de instellingen van uw lokale netwerkgateway wijzigen. Bijvoorbeeld, wanneer u toevoegen of wijzigen van het adresbereik, of als het IP-adres van de VPN-apparaat is gewijzigd. Zie [lokale instellingen netwerkgateway wijzigen met behulp van PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST-API's, PowerShell-cmdlets en CLI

Zie voor aanvullende technische bronnen en de syntaxis van de specifieke vereisten bij het gebruik van REST-API's, PowerShell-cmdlets of Azure CLI voor VPN-Gateway-configuraties, de volgende pagina's:

| **Klassiek** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azurerm.network/#networking) |[PowerShell](/powershell/module/az.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Niet ondersteund | [Azure-CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beschikbare verbindingsconfiguraties [over VPN-Gateway](vpn-gateway-about-vpngateways.md).
