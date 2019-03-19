---
title: Een zone-redundante virtuele netwerkgateway maken in Azure-Beschikbaarheidszones | Microsoft Docs
description: VPN-Gateway en ExpressRoute-gateways in Beschikbaarheidszones implementeren
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: cherylmc
ms.openlocfilehash: 00c50f95324e18f738740a9bec572ca676ab9165
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58010606"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Een zone-redundante virtuele netwerkgateway maken in Azure-Beschikbaarheidszones

U kunt VPN en ExpressRoute-gateways in Azure-Beschikbaarheidszones kunt implementeren. Dit zorgt voor tolerantie, schaalbaarheid en hogere mate van beschikbaarheid naar virtuele netwerkgateways. Implementeren van gateways in Azure-Beschikbaarheidszones fysiek en logisch, scheidt gateways binnen een regio, terwijl u uw on-premises netwerkconnectiviteit naar Azure beveiligt tegen storingen van de zone-niveau. Zie voor meer informatie, [over zone-redundante virtuele netwerkgateways](about-zone-redundant-vnet-gateways.md) en [over Azure Availability Zones](../availability-zones/az-overview.md).

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt een van beide PowerShell lokaal is geïnstalleerd op uw computer of de Azure Cloud Shell gebruiken. Als u ervoor kiest om te installeren en de PowerShell lokaal gebruikt, is deze functie vereist de meest recente versie van de PowerShell-module.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Het gebruik van PowerShell lokaal

Als u PowerShell lokaal worden gebruikt op uw computer, in plaats van de Cloud Shell gebruikt, moet u PowerShell-module 1.0.0 of hoger. Om te controleren of de versie van PowerShell die u hebt geïnstalleerd, gebruikt u de volgende opdracht:

```azurepowershell
Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
```

Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="variables"></a>1. De variabelen declareren

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

## <a name="configure"></a>2. Het virtuele netwerk maken

Maak een resourcegroep.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Maak een virtueel netwerk.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>3. Voeg het gatewaysubnet toe

Het gatewaysubnet bevat de gereserveerde IP-adressen die de virtuele-netwerkgatewayservices gebruik. Gebruik de volgende voorbeelden toevoegen en instellen van een gateway-subnet:

Voeg het gatewaysubnet toe.

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Stel de configuratie van de gateway-subnet voor het virtuele netwerk.

```azurepowershell-interactive
$getvnet | Set-AzVirtualNetwork
```
## <a name="publicip"></a>4. Een openbaar IP-adres aanvragen
 
In deze stap kiest u de instructies die betrekking hebben op de gateway die u wilt maken. De selectie van zones voor het implementeren van de gateways, is afhankelijk van de zones die zijn opgegeven voor het openbare IP-adres.

### <a name="ipzoneredundant"></a>Voor de zone-redundante gateways

Vraag een openbaar IP-adres met een **Standard** PublicIpaddress SKU en geeft niet een zone. In dit geval is de standaard openbare IP-adres hebt gemaakt een zone-redundante openbare IP-adres.   

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Voor zonegebonden gateways

Vraag een openbaar IP-adres met een **Standard** PublicIpaddress SKU. Geef de zone (1, 2 of 3). Alle gatewayexemplaren wordt geïmplementeerd in deze zone.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Voor regionale gateways

Vraag een openbaar IP-adres met een **Basic** PublicIpaddress SKU. In dit geval wordt de gateway wordt geïmplementeerd als een regionale gateway en hoeft niet elke zone-redundantie ingebouwd in de gateway. De gateway-instanties worden gemaakt in alle zones.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>5. Maak IP-configuratie

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>6. De gateway maken

De virtuele netwerkgateway maakt.

### <a name="for-expressroute"></a>For ExpressRoute

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Voor VPN-Gateway

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Wat verandert er wanneer ik deze nieuwe SKU's implementeren?

U kunt uw gateways met zone-redundantie implementeren vanuit het perspectief. Dit betekent dat alle exemplaren van de gateways worden geïmplementeerd in Azure-Beschikbaarheidszones en elke Beschikbaarheidszone een ander fout- en -domein is. Dit maakt uw gateways betrouwbare, beschikbare en flexibele op fouten van de zone.

### <a name="can-i-use-the-azure-portal"></a>Kan ik de Azure-portal gebruiken?

Ja, kunt u de Azure-portal naar de nieuwe SKU's implementeren. Echter, ziet u deze nieuwe SKU's alleen in de Azure-regio's waarvoor Azure-Beschikbaarheidszones.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Welke regio's zijn beschikbaar voor de nieuwe SKU's worden gebruikt?

Zie [Beschikbaarheidszones](../availability-zones/az-overview.md#regions-that-support-availability-zones) voor de meest recente lijst met beschikbare regio's.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kan ik wijzigen/migreren/upgrade mijn bestaande virtuele netwerkgateways met zone-redundante of zonegebonden-gateways?

Migreren van uw bestaande virtuele netwerkgateways met zone-redundante of zonegebonden gateways wordt momenteel niet ondersteund. U kunt echter uw bestaande gateway verwijderen en opnieuw maken van een zone-redundante of zonegebonden-gateway.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kan ik een VPN- en Expressroute-gateways in hetzelfde virtuele netwerk implementeren?

Meerdere VPN- en Expressroute-gateways in hetzelfde virtuele netwerk wordt ondersteund. U moet echter een/27 reserveren IP-adresbereik voor het gatewaysubnet.
