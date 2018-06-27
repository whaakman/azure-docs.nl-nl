---
title: Maak een zone-redundante virtuele netwerkgateway in Azure beschikbaarheid Zones - Preview | Microsoft Docs
description: VPN-Gateway en ExpressRoute-gateways in beschikbaarheid Zones - Preview implementeren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/25/2018
ms.author: cherylmc
ms.openlocfilehash: a08c0f772965ddb2b40ac1ced1f26ade4cba3197
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37017029"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Maak een zone-redundante virtuele netwerkgateway in Azure beschikbaarheid Zones - Preview

U kunt VPN- en ExpressRoute-gateways in implementeren [Azure beschikbaarheid Zones](../availability-zones/az-overview.md). Hiermee wordt de tolerantie, schaalbaarheid en hogere beschikbaarheid voor virtuele netwerkgateways. Gateways in Azure beschikbaarheid Zones implementeren fysiek en logisch scheidt gateways binnen een regio bij het beveiligen van uw lokale netwerkverbinding naar Azure van zoneniveau fouten.

Zonal en zone-redundante gateways hebben fundamentele prestatieverbeteringen via reguliere virtuele netwerkgateways. Maken van een zone-redundante of zonal virtuele netwerkgateway is bovendien sneller dan andere gateways te maken. Maak in plaats van 45 minuten duurt, tijden nemen ongeveer 15 minuten voor een ExpressRoute-gateway en 19 minuten voor een VPN-gateway.

### <a name="zrgw"></a>Zone-redundante gateways

Om automatisch te implementeren uw virtuele netwerkgateways over beschikbaarheid zones, kunt u zone-redundante virtuele netwerkgateways. Met zone-redundante gateways, kunt u profiteren van de SLA van 99,99% beschikbaarheid op NH voor toegang tot uw bedrijfskritieke, schaalbare services in Azure.

<br>
<br>

![afbeelding van zone redunant gateways](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Zonal gateways

Als u wilt implementeren gateways in een specifieke zone, moet u zonal gateways gebruiken. Wanneer u een zonal gateway implementeert, worden beide exemplaren van de gateway worden geïmplementeerd in dezelfde regio bevindt beschikbaarheid.

<br>
<br>

![afbeelding van zonal gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Gateway-SKU's

Zone-redundante en zonal gateways moeten de nieuwe gateway-SKU's gebruiken. Eenmaal u [zelf registreren in het voorbeeld](#enroll), ziet u de nieuwe virtuele netwerkgateway SKU's in alle van de AZ Azure-regio's. Deze SKU's zijn vergelijkbaar met de bijbehorende SKU's voor ExpressRoute- en VPN-Gateway, behalve dat ze specifiek voor de zone-redundante en zonal gateways zijn.

De nieuwe gateway-SKU's zijn:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>Openbare IP-SKU 's

Zone-redundante gateways en zonal gateways afhankelijk van de Azure openbare IP-resource *standaard* SKU. De configuratie van de Azure openbare IP-resource wordt bepaald of de gateway die u implementeert, zone-redundante of zonal. Als u een openbaar IP-resource met een *Basic* SKU, de gateway heeft niet de zoneredundantie van een en de gateway-bronnen worden regionale.

### <a name="pipzrg"></a>Zone-redundante gateways

Wanneer u maakt een openbare IP-adres met de **standaard** openbare IP-SKU zonder op te geven van een zone voor het gedrag is afhankelijk van of de gateway een VPN-gateway of een ExpressRoute-gateway is. 

* Voor een VPN-gateway, worden de twee gatewayexemplaren geïmplementeerd in elke 2 buiten deze drie zones om zone redundantie te verlenen. 
* Voor een ExpressRoute-gateway, omdat er meer dan twee instanties, kan de gateway overspannen alle drie zones.

### <a name="pipzg"></a>Zonal gateways

Wanneer u maakt een openbare IP-adres met de **standaard** openbare IP-SKU en geeft u de Zone (1, 2 of 3), alle gatewayexemplaren wordt geïmplementeerd in dezelfde regio bevindt.

### <a name="piprg"></a>Regionale gateways

Wanneer u maakt een openbare IP-adres met de **Basic** openbare IP-SKU de gateway wordt geïmplementeerd als een regionaal gateway en hoeft niet elke zone-redundantie ingebouwd in de gateway.

## <a name="before-you-begin"></a>Voordat u begint

U kunt beide PowerShell lokaal worden geïnstalleerd op uw computer of de Azure-Cloud-Shell gebruiken. Als u wilt installeren en gebruiken van de PowerShell lokaal hiervoor de nieuwste versie van de PowerShell-module.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Gebruik PowerShell lokaal

Als u werkt PowerShell lokaal op uw computer in plaats van met Cloud-Shell, moet u PowerShell-module 6.1.1 installeren of hoger. Als u wilt controleren de versie van PowerShell die u hebt geïnstalleerd, moet u de volgende opdracht gebruiken:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Registreren in de Preview-versie

Voordat u een zone-redundante of zonal gateway configureren kunt, moet u de uw abonnement in de Preview eerst zelf inschrijven. Wanneer uw abonnement is ingericht, wordt u om te zien van de nieuwe gateway-SKU's in alle regio's Azure AZ gestart. 

Zorg ervoor dat u bent aangemeld bij uw Azure-account en het abonnement dat u goedgekeurde IP-adressen voor deze Preview wilt gebruiken. Gebruik het volgende voorbeeld om in te schrijven:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

Gebruik de volgende opdracht om te controleren of de functie 'AllowVMSSVirtualNetworkGateway' is geregistreerd bij uw abonnement:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

Het resultaat ziet er ongeveer als in dit voorbeeld:

![ingericht](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. De variabelen declareren

De waarden voor de voorbeeld-stappen worden hieronder vermeld. Daarnaast gebruiken enkele voorbeelden gedeclareerde variabelen binnen de stappen. Als u deze stappen in uw eigen omgeving gebruikt, zorg er dan voor dat deze waarden te vervangen door uw eigen. Als u opgeeft, moet u controleren of de regio die u opgeeft, wordt ondersteund. Zie voor meer informatie de [Veelgestelde vragen over](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>3. Het virtuele netwerk maken

Maak een resourcegroep.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Maak een virtueel netwerk.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzureRmVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>4. Voeg het gatewaysubnet toe

Het gatewaysubnet bevat de gereserveerde IP-adressen die gebruikmaken van de services van de gateway virtuele netwerk. Gebruik de volgende voorbeelden toe te voegen en een gatewaysubnet instellen:

Voeg het gatewaysubnet toe.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Stel de configuratie van de gateway-subnet voor het virtuele netwerk.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>5. Een openbaar IP-adres aanvragen
 
Kies de instructies die betrekking hebben op de gateway die u wilt maken in deze stap. De selectie van zones voor het implementeren van de gateways, is afhankelijk van de zones die zijn opgegeven voor het openbare IP-adres.

### <a name="ipzoneredundant"></a>Voor de zone-redundante gateways

Een openbaar IP-adres met een **standaard** PublicIpaddress SKU en geeft niet een zone. In dit geval worden de standaard openbare IP-adres gemaakt een zone-redundante openbare IP-adres.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Standard
```

### <a name="ipzonalgw"></a>Voor zonal gateways

Een openbaar IP-adres met een **standaard** PublicIpaddress SKU. Geef de zone (1, 2 of 3). Alle gatewayexemplaren wordt geïmplementeerd in deze zone.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Voor regionale gateways

Een openbaar IP-adres met een **Basic** PublicIpaddress SKU. In dit geval de gateway wordt geïmplementeerd als een regionaal gateway en hoeft niet elke zone-redundantie ingebouwd in de gateway. De gatewayexemplaren worden gemaakt in alle zones.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>6. De IP-configuratie maken

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>7. De gateway maken

Maak de virtuele netwerkgateway.

>[!NOTE]
>Op dit moment kunt opgeven u de gateway-SKU niet. De SKU standaard automatisch ErGw1AZ voor ExpressRoute- en VpnGw1AZ voor VPN-Gateway.
>

### <a name="for-expressroute"></a>Voor ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Voor VPN-Gateway

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Hoe u om feedback te geven

We zouden stellen uw feedback. Een e-mail sturen naar aznetworkgateways@microsoft.com Meld eventuele problemen of feedback geven (positief of negatief) voor de zone-redundante en zonal VPN- en Express Route-gateways. De naam van uw bedrijf in '['] opnemen in de onderwerpregel. Ook uw abonnements-ID als u een probleem wilt melden.

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-do-i-sign-up-for-the-preview"></a>Hoe meld ik me voor de Preview?

U kunt [zelf inschrijven](#enroll) met de PowerShell-opdrachten in dit artikel.

### <a name="what-will-change-when-i-enroll"></a>Wat er wordt gewijzigd wanneer ik Inschrijf?

Vanuit het perspectief wordt kunt tijdens de Preview, u uw gateways met zone redundantie implementeren. Dit betekent dat alle exemplaren van de gateways worden geïmplementeerd via Azure beschikbaarheid Zones en elke Zone beschikbaarheid een ander probleem- en update-domein is. Hierdoor kan uw gateways betrouwbare, beschikbare en robuuste op fouten van de zone.

### <a name="what-regions-are-available-for-the-preview"></a>Welke regio's zijn beschikbaar voor de Preview?

Zone-redundante en zonal gateways zijn beschikbaar in productie-Azure openbare gebieden.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>Ik krijgt voor deelname in deze Preview?

U wordt niet gefactureerd voor uw gateways tijdens de Preview. Er is echter geen SLA die is gekoppeld aan uw implementatie. We zijn horen uw feedback zeer geïnteresseerd.

> [!NOTE]
> Voor ExpressRoute-gateways is de gateway niet in rekening gebracht/in rekening gebracht. Echter, het circuit zelf (niet de gateway) worden gefactureerd.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>Welke regio's zijn beschikbaar voor mij om te proberen dit op?

De openbare preview is beschikbaar in VS-midden en Frankrijk centrale regio's (Azure-regio's waarvoor beschikbaarheid Zones algemeen beschikbaar). Voortaan kunt zullen we de Zone-redundante Gateways u beschikbaar te maken in andere openbare Azure-gebieden.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kan ik mijn bestaande virtuele netwerkgateways naar zone-redundante of zonal gateways wijzigen?

Migreren van uw bestaande virtuele netwerkgateways naar zone-redundante of zonal gateways is momenteel niet ondersteund. U kunt echter uw bestaande gateway verwijdert en opnieuw maken van een zone-redundante of zonal-gateway.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kan ik zowel VPN- en Express Route-gateways in hetzelfde virtuele netwerk implementeren?

Naast elkaar bestaan van zowel de VPN- en Express Route-gateways in hetzelfde virtuele netwerk wordt ondersteund tijdens de openbare Preview. Maar ook op de hoogte van de volgende vereisten en beperkingen:

* Reserve a /27 IP-adresbereik voor het gatewaysubnet.
* Zone-redundante/zonal Express Route-gateways kunnen alleen bestaan naast elkaar met zone-redundante/zonal VPN-gateways.
* De zone-redundante/zonal Express Route-gateway implementeren voordat u de zone-redundante/zonal VPN-gateway implementeert.
* Een zone-redundante/zonal Express Route-gateway kunt verbonden zijn met maximaal 4 circuits.

## <a name="next-steps"></a>Volgende stappen

We zouden stellen uw feedback. Een e-mail sturen naar aznetworkgateways@microsoft.com Meld eventuele problemen of feedback geven (positief of negatief) voor de zone-redundante en zonal VPN- en Express Route-gateways. De naam van uw bedrijf in '['] opnemen in de onderwerpregel. Ook uw abonnements-ID als u een probleem wilt melden.