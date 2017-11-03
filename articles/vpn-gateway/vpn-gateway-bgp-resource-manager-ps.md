---
title: 'Configureren van BGP op Azure VPN-Gateways: Resource Manager: PowerShell | Microsoft Docs'
description: In dit artikel begeleidt u bij het configureren van BGP met Azure VPN-Gateways met Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: b00a3fe7ba4b12c2e9c486188c292cd6fafb60a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a>Het configureren van BGP op Azure VPN-Gateways met behulp van PowerShell
Dit artikel begeleidt u bij de stappen voor het inschakelen van BGP op een cross-premises Site-naar-Site (S2S) VPN-verbinding en een VNet-naar-VNet-verbinding met het Resource Manager-implementatiemodel en PowerShell.

## <a name="about-bgp"></a>Over BGP
BGP is het standaardprotocol voor routering dat doorgaans op internet wordt gebruikt voor het uitwisselen van routerings- en bereikbaarheidsgegevens tussen twee of meer netwerken. BGP kunt u de Azure VPN-Gateways en uw on-premises VPN-apparaten, aangeroepen BGP-peers of neighbors, 'routes' exchange die wordt beide gateways over de beschikbaarheid en bereikbaarheid voor deze voorvoegsels informeren zodat ze via de gateways of routers die betrokken zijn. Met BGP kan ook transitroutering tussen meerdere netwerken worden ingeschakeld door routes die een BGP-gateway leert van één BGP te propageren naar alle andere BGP-peers.

Zie [overzicht van BGP met Azure VPN-Gateways](vpn-gateway-bgp-overview.md) meer discussie over de voordelen van BGP en inzicht in de technische vereisten en overwegingen van het gebruik van BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Aan de slag met BGP op Azure VPN-gateways

Dit artikel begeleidt u bij de stappen voor de volgende taken uitvoeren:

* [Deel 1 - Enable BGP op uw Azure VPN-gateway](#enablebgp)
* [Deel 2: een cross-premises verbinding maken met BGP](#crossprembgp)
* [Deel 3 – een VNet-naar-VNet verbinding maken met BGP](#v2vbgp)

Elk deel van de instructies vormt een basisbouwsteen voor het inschakelen van BGP in uw netwerkverbinding. Als u alle drie delen hebt voltooid, wordt de topologie build zoals weergegeven in het volgende diagram:

![BGP-topologieën](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

U kunt onderdelen samen om samen te stellen een meer complexe, Multihop, doorvoer-netwerk dat voldoet aan uw behoeften kunt combineren.

## <a name ="enablebgp"></a>Deel 1: BGP configureren op de Azure VPN-Gateway
De configuratiestappen stelt u de BGP-parameters van de Azure VPN-gateway, zoals wordt weergegeven in het volgende diagram:

![BGP-Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Voordat u begint
* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Installeer de Azure Resource Manager PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het installeren van de PowerShell-cmdlets. 

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1: Maak en configureer VNet1
#### <a name="1-declare-your-variables"></a>1. De variabelen declareren
Voor deze oefening eerst we onze variabelen declareren. Het volgende voorbeeld declareert de variabelen met de waarden voor deze oefening. Zorg dat u de waarden door uw eigen waarden vervangt wanneer u configureert voor productie. U kunt deze variabelen gebruiken als u de stappen wilt doorlopen om vertrouwd te raken met dit type configuratie. Wijzig de variabelen en kopieer en plak ze in uw PowerShell-console.

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Verbinding maken met uw abonnement en een nieuwe resourcegroep maken
Zorg ervoor dat u overschakelt naar de PowerShell-modus voor het gebruik van de Resource Manager-cmdlets. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 maken
Het volgende voorbeeld maakt een virtueel netwerk met de naam TestVNet1 en drie subnetten, één naam GatewaySubnet, een opgeroepen FrontEnd en één opgeroepen back-end. Wanneer u de waarden vervangt, is het belangrijk dat u de juiste namen voor de gatewaysubnets gebruikt, in het bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Stap 2: de VPN-Gateway voor TestVNet1 maken met de BGP-parameters
#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. De configuraties van IP-adres en subnet maken
Vraag om een openbaar IP-adres toe te wijzen aan de gateway die u voor uw VNet gaat maken. Ook definieert u de vereiste subnet en IP-configuraties.

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. De VPN-gateway maken met het AS-nummer
Maak de gateway van het virtuele netwerk voor TestVNet1. BGP is vereist voor een op Route gebaseerde VPN-gateway en de toevoeging parameter, - Asn het ASN (AS-nummer) instellen voor TestVNet1. Als de ASN-parameter niet is ingesteld, wordt de ASN van 65515 toegewezen. Het maken van een gateway kan even duren (30 minuten of langer).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Het Azure BGP-Peer-IP-adres verkrijgen
Zodra de gateway is gemaakt, moet u het BGP-Peer-IP-adres op de Azure VPN-Gateway. Dit adres wordt gebruikt voor de Azure VPN-Gateway configureren als een BGP-Peer voor uw on-premises VPN-apparaten.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

De laatste opdracht toont de bijbehorende BGP-configuraties op de Azure VPN-Gateway; bijvoorbeeld:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Zodra de gateway is gemaakt, kunt u deze gateway om cross-premises-verbinding of een VNet-naar-VNet-verbinding waarvoor BGP. De volgende secties helpt bij de stappen voor het voltooien van de oefening.

## <a name ="crossprembbgp"></a>Deel 2: een cross-premises verbinding maken met BGP

Als u wilt een cross-premises-verbinding tot stand brengen, moet u een lokale netwerkgateway om weer te geven van uw on-premises VPN-apparaat en een verbinding met de VPN-gateway verbinding met de lokale netwerkgateway maken. Hoewel er artikelen die u bij deze stappen helpt, wordt in dit artikel om op te geven van de BGP-configuratieparameters vereiste aanvullende eigenschappen bevat.

![BGP voor Cross-Premises](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Controleer of u hebt uitgevoerd voordat u doorgaat, [Part 1](#enablebgp) van deze oefening.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1 - maken en configureren van de lokale netwerkgateway

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

In deze oefening blijft het bouwen van de configuratie in het diagram worden weergegeven. Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Een aantal dingen om aan te geven met betrekking tot het lokale netwerk gateway-parameters:

* De lokale netwerkgateway kan zich in dezelfde of een andere locatie en resourcegroep als de VPN-gateway. Dit voorbeeld ziet u ze in verschillende resourcegroepen op verschillende locaties.
* Het minimum-voorvoegsel moet u opgeven voor de lokale netwerkgateway is het adres van de host van de BGP-Peer-IP-adres op uw VPN-apparaat. In dit geval is een /32 voorvoegsel "10.52.255.254/32".
* U moet verschillende ASN van BGP tussen uw on-premises netwerken en Azure VNet gebruiken als een herinnering. Als ze hetzelfde zijn, moet u uw VNet ASN wijzigen als de ASN in uw on-premises VPN-apparaat al worden gebruikt als peer met andere BGP-neighbors.

Controleer voordat u verdergaat of u nog bent verbonden met Abonnement 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. De lokale netwerkgateway voor Site5 maken

Zorg ervoor dat de resourcegroep maken als deze niet gemaakt is, voordat u de lokale netwerkgateway maakt. U ziet de twee extra parameters voor de lokale netwerkgateway: Asn en BgpPeerAddress.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet-gateway en de lokale netwerkgateway

#### <a name="1-get-the-two-gateways"></a>1. Ophalen van de twee gateways

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. De verbinding tussen TestVNet1 en Site5 maken

In deze stap maakt u de verbinding van TestVNet1 naar Site5. U moet opgeven '-EnableBGP $True ' BGP inschakelen voor deze verbinding. Zoals eerder besproken, is het mogelijk om BGP- en niet-BGP-verbindingen voor dezelfde Azure VPN-Gateway. Tenzij BGP in de eigenschap connection is ingeschakeld, wordt Azure niet BGP inschakelen voor deze verbinding Hoewel BGP-parameters zijn al geconfigureerd op beide gateways.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

Het volgende voorbeeld bevat de parameters die u in de configuratiesectie BGP op uw on-premises VPN-apparaat voor deze oefening:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

De verbinding is hersteld na een paar minuten en de BGP-peeringsessie begint eenmaal IPsec-verbinding tot stand is gebracht.

## <a name ="v2vbgp"></a>Deel 3 – een VNet-naar-VNet verbinding maken met BGP

In deze sectie voegt een VNet-naar-VNet-verbinding met BGP, zoals wordt weergegeven in het volgende diagram:

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

De volgende instructies blijven uit de vorige stap. U moet voltooien [deel I](#enablebgp) maken en configureren van TestVNet1 en de VPN-Gateway met BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1: TestVNet2 en de VPN-gateway maken

Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk TestVNet2, niet met een van uw VNet-bereiken overlapt.

In dit voorbeeld wordt de virtuele netwerken tot hetzelfde abonnement behoren. U kunt de VNet-naar-VNet-verbindingen tussen de verschillende abonnementen instellen. Zie voor meer informatie [een VNet-naar-VNet-verbinding configureren](vpn-gateway-vnet-vnet-rm-ps.md). Zorg ervoor dat u toevoegt het '-EnableBgp $True "bij het maken van de verbindingen om in te schakelen van BGP.

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. TestVNet2 in de nieuwe resourcegroep maken

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. De VPN-gateway maken voor TestVNet2 met BGP-parameters

Aanvragen van een openbare IP-adres worden toegewezen aan de gateway maakt voor uw VNet en definieer het vereiste subnet en IP-configuraties.

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

Maak de VPN-gateway met het AS-nummer. U moet de standaard een ASN overschrijven op uw Azure VPN-gateways. De ASN's voor de verbonden VNets moet verschillen van BGP en transitroutering inschakelen.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Stap 2: verbinding maken met de TestVNet1 en TestVNet2 gateways

In dit voorbeeld zijn beide gateways in hetzelfde abonnement. U kunt deze stap voltooien in dezelfde PowerShell-sessie.

#### <a name="1-get-both-gateways"></a>1. Beide gateways ophalen

Zorg dat u zich aanmeldt bij en verbinding maakt met Abonnement 1.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Beide verbindingen maken

In deze stap maakt u de verbinding van TestVNet1 naar TestVNet2 en de verbinding van TestVNet2 naar TestVNet1.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Zorg ervoor dat het inschakelen van BGP voor beide verbindingen.
> 
> 

Nadat u deze stappen uitvoert, wordt de verbinding na een paar minuten. De BGP-peeringsessie is nadat de VNet-naar-VNet-verbinding is voltooid.

Als u alle drie de gedeelten van deze oefening hebt voltooid, kunt u de volgende netwerktopologie hebt ingesteld:

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Volgende stappen

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.