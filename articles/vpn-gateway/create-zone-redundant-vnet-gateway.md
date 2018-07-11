---
title: Een zone-redundante virtuele netwerkgateway maken in Azure-Beschikbaarheidszones - Preview | Microsoft Docs
description: VPN-Gateway en ExpressRoute-gateways in Beschikbaarheidszones - voorbeeld implementeren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/09/2018
ms.author: cherylmc
ms.openlocfilehash: fa349555a5effd41ca519cbd5a29005203d79543
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952552"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Een zone-redundante virtuele netwerkgateway maken in Azure-Beschikbaarheidszones - Preview

U kunt VPN en ExpressRoute-gateways in implementeren [Azure Availability Zones](../availability-zones/az-overview.md). Dit zorgt voor tolerantie, schaalbaarheid en hogere mate van beschikbaarheid naar virtuele netwerkgateways. Implementeren van gateways in Azure-Beschikbaarheidszones fysiek en logisch, scheidt gateways binnen een regio, terwijl u uw on-premises netwerkconnectiviteit naar Azure beveiligt tegen storingen van de zone-niveau.

Zones gekoppelde en zoneredundante gateways hebben fundamentele prestatieverbeteringen via reguliere virtuele netwerkgateways. Daarnaast is het sneller dan andere gateways maken het maken van een zone-redundante of zonegebonden virtuele netwerkgateway. Maak in plaats van 45 minuten duurt, tijden duurt het ongeveer 15 minuten voor een ExpressRoute-gateway en 19 minuten voor een VPN-gateway.

### <a name="zrgw"></a>Zone-redundante gateways

Voor het automatisch implementeren van uw virtuele netwerkgateways in meerdere beschikbaarheidszones, kunt u zone-redundante virtuele netwerkgateways. Met zone-redundante gateways, kunt u profiteren van de bedrijfstijd van 99,99% SLA bij algemene beschikbaarheid voor toegang tot uw essentiële, schaalbare services op Azure.

<br>
<br>

![zone-redunant gateways afbeelding](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Zonegebonden gateways

Voor het implementeren van gateways in een specifieke zone, moet u zonegebonden gateways gebruiken. Wanneer u een zonegebonden gateway implementeert, worden beide exemplaren van de gateway in dezelfde Beschikbaarheidszone geïmplementeerd.

<br>
<br>

![afbeelding van zonegebonden gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Gateway-SKU's

Zone-redundante en zonegebonden gateways moeten de nieuwe gateway-SKU's gebruiken. Nadat u [zelf registreren in de Preview-versie](#enroll), ziet u de nieuwe virtuele netwerkgateway-SKU's in alle van de AZ Azure-regio's. Deze SKU's zijn vergelijkbaar met de bijbehorende SKU's voor ExpressRoute en VPN-Gateway, behalve dat ze specifiek voor de zone-redundante en zonegebonden gateways zijn.

De nieuwe gateway SKU's zijn:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>Openbare IP-SKU 's

Zone-redundante gateways en zonegebonden gateways beide zijn afhankelijk van de Azure openbare IP-adresresource *Standard* SKU. De configuratie van de Azure openbare IP-adresresource wordt bepaald of de gateway die u implementeert zone-redundante of zonegebonden. Als u een openbare IP-resource met maakt een *Basic* SKU, de gateway heeft geen eventuele zoneredundantie en de gateway-resources worden regionale.

### <a name="pipzrg"></a>Zone-redundante gateways

Wanneer u een openbaar IP-adres met maakt de **Standard** openbare IP-SKU zonder een zone op te geven, het gedrag verschilt, afhankelijk van of de gateway een VPN-gateway of een ExpressRoute-gateway is. 

* Voor een VPN-gateway, wordt de twee gateway-instanties worden geïmplementeerd in elke 2 buiten deze drie zones om zone-redundantie te verlenen. 
* Voor een ExpressRoute-gateway, omdat er meer dan twee instanties, kunt de gateway overbruggen alle drie zones.

### <a name="pipzg"></a>Zonegebonden gateways

Wanneer u een openbaar IP-adres met maakt de **Standard** openbare IP-SKU en geef de Zone (1, 2 of 3), alle gatewayexemplaren wordt geïmplementeerd in dezelfde regio.

### <a name="piprg"></a>Regionale gateways

Wanneer u een openbaar IP-adres met maakt de **Basic** openbare IP-SKU, de gateway wordt geïmplementeerd als een regionale gateway en hoeft niet elke zone-redundantie ingebouwd in de gateway.

## <a name="before-you-begin"></a>Voordat u begint

U kunt een van beide PowerShell lokaal is geïnstalleerd op uw computer of de Azure Cloud Shell gebruiken. Als u ervoor kiest om te installeren en de PowerShell lokaal gebruikt, is deze functie vereist de meest recente versie van de PowerShell-module.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Het gebruik van PowerShell lokaal

Als u PowerShell lokaal worden gebruikt op uw computer, in plaats van de Cloud Shell gebruikt, moet u PowerShell-module 6.1.1 installeren of hoger. Om te controleren of de versie van PowerShell die u hebt geïnstalleerd, gebruikt u de volgende opdracht:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Schrijf u in de Preview-versie

Voordat u een zone-redundante of zonegebonden gateway configureren kunt, moet u eerst zelf zich inschrijven uw abonnement in de Preview-versie. Zodra uw abonnement actief is, wordt u om te zien van de nieuwe gateway-SKU's in alle regio's Azure AZ gestart. 

Zorg ervoor dat u zich hebt aangemeld bij uw Azure-account en het abonnement dat u voor deze Preview-versie aan lijst met geaccepteerde wilt gebruiken. Gebruik het volgende voorbeeld om in te schrijven:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

Gebruik de volgende opdracht uit om te controleren of de functie 'AllowVMSSVirtualNetworkGateway' met uw abonnement is geregistreerd:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

Het resultaat ziet eruit als in dit voorbeeld:

![ingericht](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. De variabelen declareren

De waarden voor de voorbeeldenstappen worden hieronder vermeld. Bovendien enkele van de voorbeelden gedeclareerde variabelen in de stappen gebruiken. Als u deze stappen in uw eigen omgeving gebruikt, zorg er dan voor dat deze waarden vervangen door uw eigen. Wanneer u locatie opgeeft, moet u controleren of de regio die u opgeeft wordt ondersteund. Zie voor meer informatie de [Veelgestelde vragen over](#faq).

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

Het gatewaysubnet bevat de gereserveerde IP-adressen die de virtuele-netwerkgatewayservices gebruik. Gebruik de volgende voorbeelden toevoegen en instellen van een gateway-subnet:

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
 
In deze stap kiest u de instructies die betrekking hebben op de gateway die u wilt maken. De selectie van zones voor het implementeren van de gateways, is afhankelijk van de zones die zijn opgegeven voor het openbare IP-adres.

### <a name="ipzoneredundant"></a>Voor de zone-redundante gateways

Vraag een openbaar IP-adres met een **Standard** PublicIpaddress SKU en geeft niet een zone. In dit geval is de standaard openbare IP-adres hebt gemaakt een zone-redundante openbare IP-adres.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Voor zonegebonden gateways

Vraag een openbaar IP-adres met een **Standard** PublicIpaddress SKU. Geef de zone (1, 2 of 3). Alle gatewayexemplaren wordt geïmplementeerd in deze zone.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Voor regionale gateways

Vraag een openbaar IP-adres met een **Basic** PublicIpaddress SKU. In dit geval wordt de gateway wordt geïmplementeerd als een regionale gateway en hoeft niet elke zone-redundantie ingebouwd in de gateway. De gateway-instanties worden gemaakt in alle zones.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>6. Maak IP-configuratie

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>7. De gateway maken

De virtuele netwerkgateway maakt.

### <a name="for-expressroute"></a>Voor ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Voor VPN-Gateway

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Hoe u om feedback te geven

We zouden waarderen uw feedback. Stuur een e-mail naar aznetworkgateways@microsoft.com problemen melden of feedback (positief of negatief) bieden voor de zone-redundante en zonegebonden VPN en Expressroute-gateways. De naam van uw bedrijf in '['] opnemen in de onderwerpregel. Ook uw abonnements-ID als u een probleem wilt melden.

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-do-i-sign-up-for-the-preview"></a>Hoe registreer ik me voor de Preview?

U kunt [zelf inschrijven](#enroll) met behulp van de PowerShell-opdrachten in dit artikel.

### <a name="what-will-change-when-i-enroll"></a>Wat verandert er wanneer ik registreren?

De Preview-versie van uw perspectief, kunt u uw gateways met zone-redundantie implementeren. Dit betekent dat alle exemplaren van de gateways worden geïmplementeerd in Azure-Beschikbaarheidszones en elke Beschikbaarheidszone een ander fout- en -domein is. Dit maakt uw gateways betrouwbare, beschikbare en flexibele op fouten van de zone.

### <a name="can-i-use-the-azure-portal"></a>Kan ik de Azure-portal gebruiken?

Ja, u kunt de Azure-portal gebruiken voor de Preview-versie. Echter, u toch wilt inschrijven met behulp van PowerShell of kunt u zich niet aan de portal te gebruiken tijdens de Preview.

### <a name="what-regions-are-available-for-the-preview"></a>Welke regio's zijn beschikbaar voor de Preview-versie?

Zone-redundante en zonegebonden gateways zijn beschikbaar in openbare productie/Azure-regio's.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>Wordt ik gefactureerd voor uw deelname aan deze Preview?

U wordt niet in rekening gebracht voor uw gateways tijdens de Preview. Er is echter geen SLA die is gekoppeld met uw implementatie. Er zijn we zeer geïnteresseerd in uw feedback horen.

> [!NOTE]
> Voor ExpressRoute-gateways, de gateway is niet in rekening gebracht/in rekening gebracht. Echter, het circuit zelf (niet de gateway) wordt gefactureerd.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>Welke regio's zijn beschikbaar voor me om te proberen dit op?

De openbare preview-versie is beschikbaar in VS-midden en Frankrijk-centraal (Azure-regio's waarvoor Beschikbaarheidszones algemeen beschikbaar). Vanaf nu maken we de Zone-redundante Gateways beschikbaar wilt voor u in andere openbare Azure-regio.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kan ik mijn bestaande virtuele netwerkgateways met zone-redundante of zonegebonden gateways wijzigen?

Migreren van uw bestaande virtuele netwerkgateways met zone-redundante of zonegebonden gateways wordt momenteel niet ondersteund. U kunt echter uw bestaande gateway verwijderen en opnieuw maken van een zone-redundante of zonegebonden-gateway.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kan ik een VPN- en Expressroute-gateways in hetzelfde virtuele netwerk implementeren?

Meerdere VPN- en Expressroute-gateways in hetzelfde virtuele netwerk wordt ondersteund tijdens de openbare Preview. Echter rekening houden met de volgende vereisten en beperkingen:

* Reserveer een /27 IP-adresbereik voor het gatewaysubnet.
* Zone-redundante/zonegebonden Express Route-gateways kunnen alleen naast elkaar bestaan met zone-redundante/zonegebonden VPN-gateways.
* De zone-redundante/zonegebonden Express Route-gateway implementeren voordat u de zone-redundante/zonegebonden VPN-gateway implementeert.
* Een zone-redundante/zonegebonden Express Route-gateway kan worden verbonden aan maximaal 4 circuits.

## <a name="next-steps"></a>Volgende stappen

We zouden waarderen uw feedback. Stuur een e-mail naar aznetworkgateways@microsoft.com problemen melden of feedback (positief of negatief) bieden voor de zone-redundante en zonegebonden VPN en Expressroute-gateways. De naam van uw bedrijf in '['] opnemen in de onderwerpregel. Ook uw abonnements-ID als u een probleem wilt melden.
