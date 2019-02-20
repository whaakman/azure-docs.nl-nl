---
title: 'BGP op Azure VPN-Gateways configureren: Resource Manager: PowerShell | Microsoft Docs'
description: In dit artikel begeleidt u bij het configureren van BGP met Azure VPN-Gateways met behulp van Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: c65ea038fc39702affae93cb68b8cf644393c62e
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415609"
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a>Het configureren van BGP op Azure VPN-Gateways met behulp van PowerShell
In dit artikel leidt u door de stappen u BGP wilt inschakelen op een cross-premises Site-naar-Site (S2S) VPN-verbinding en een VNet-naar-VNet-verbinding met het Resource Manager-implementatiemodel en PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about-bgp"></a>Over BGP
BGP is het standaardprotocol voor routering dat doorgaans op internet wordt gebruikt voor het uitwisselen van routerings- en bereikbaarheidsgegevens tussen twee of meer netwerken. BGP kunt u de Azure VPN-Gateways en uw on-premises VPN-apparaten, aangeroepen BGP-peers of neighbors, voor het uitwisselen van 'routes' die worden beide gateways over de beschikbaarheid en bereikbaarheid voor deze voorvoegsels informeren zodat ze via de gateways of routers die betrokken zijn. Met BGP kan ook transitroutering tussen meerdere netwerken worden ingeschakeld door routes die een BGP-gateway leert van één BGP te propageren naar alle andere BGP-peers.

Zie [overzicht van BGP met Azure VPN-Gateways](vpn-gateway-bgp-overview.md) voor meer informatie over de voordelen van BGP en geeft inzicht in de technische vereisten en overwegingen van het gebruik van BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Aan de slag met BGP op Azure VPN-gateways

In dit artikel leidt u door de stappen voor het uitvoeren van de volgende taken:

* [Deel 1: BGP op uw Azure VPN-gateway inschakelen](#enablebgp)
* Deel 2: een cross-premises verbinding maken met BGP
* [Deel 3: een VNet-naar-VNet verbinding maken met BGP](#v2vbgp)

Elk onderdeel van de instructies vormt een eenvoudige bouwsteen voor het inschakelen van BGP in uw netwerkverbinding. Als u alle drie delen hebt voltooid, wordt de topologie bouwen zoals wordt weergegeven in het volgende diagram:

![BGP-topologie](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

U kunt combineren onderdelen samen om te maken van een meer complexe, Multihop, tussenliggende-netwerk dat voldoet aan uw behoeften.

## <a name ="enablebgp"></a>Deel 1: BGP configureren op de Azure VPN-Gateway
De configuratiestappen uit om de BGP-parameters van de Azure VPN-gateway te stellen zoals wordt weergegeven in het volgende diagram:

![BGP Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Voordat u begint
* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Installeer de Azure Resource Manager PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het installeren van de PowerShell-cmdlets. 

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1: maken en configureren van VNet1
#### <a name="1-declare-your-variables"></a>1. De variabelen declareren
We beginnen door op te geven van de variabelen voor deze oefening. Het volgende voorbeeld worden de variabelen met de waarden voor deze oefening gedeclareerd. Zorg dat u de waarden door uw eigen waarden vervangt wanneer u configureert voor productie. U kunt deze variabelen gebruiken als u de stappen wilt doorlopen om vertrouwd te raken met dit type configuratie. Wijzig de variabelen en kopieer en plak ze in uw PowerShell-console.

```powershell
$Sub1 = "Replace_With_Your_Subscription_Name"
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
De Resource Manager-cmdlets wilt gebruiken, zorg ervoor dat u overschakelt naar de PowerShell-modus. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 maken
Het volgende voorbeeld wordt een virtueel netwerk met de naam TestVNet1 en drie subnetten, één met de naam GatewaySubnet, één met de naam FrontEnd en één met de naam back-end. Wanneer u de waarden vervangt, is het belangrijk dat u de juiste namen voor de gatewaysubnets gebruikt, in het bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Stap 2: de VPN-Gateway voor TestVNet1 te maken met de BGP-parameters
#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. De configuraties van IP-adres en subnet maken
Vraag om een openbaar IP-adres toe te wijzen aan de gateway die u voor uw VNet gaat maken. U kunt ook de vereiste subnet en IP-configuraties definiëren.

```powershell
$gwpip1 = New-AzPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. De VPN-gateway maken met het AS-nummer
Maak de gateway van het virtuele netwerk voor TestVNet1. BGP is vereist voor een Route gebaseerde VPN-gateway en ook de toevoeging-parameter, - Asn, de ASN (AS-nummer) instellen voor TestVNet1. Als de ASN-parameter niet is ingesteld, wordt de ASN van 65515 toegewezen. Het maken van een gateway kan even duren (30 minuten of langer).

```powershell
New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Haal het Azure BGP-Peer-IP-adres
Nadat de gateway is gemaakt, moet u het BGP-Peer-IP-adres op de Azure VPN-Gateway. Dit adres is nodig om de Azure VPN-Gateway configureren als een BGP-Peer voor uw on-premises VPN-apparaten.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

De laatste opdracht ziet u de bijbehorende BGP-configuraties op de Azure VPN-Gateway; bijvoorbeeld:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Nadat de gateway is gemaakt, kunt u deze gateway tot stand brengen van cross-premises-verbinding of VNet-naar-VNet-verbinding waarvoor BGP is. De volgende secties helpen bij de stappen voor het voltooien van de oefening.

## <a name ="crossprembbgp"></a>Deel 2: een cross-premises verbinding maken met BGP

Als u wilt een cross-premises-verbinding tot stand brengen, moet u een lokale netwerkgateway voor uw on-premises VPN-apparaat en een verbinding om de VPN-gateway verbinding met de lokale netwerkgateway te maken. Hoewel er artikelen waarin wordt uitgelegd dat u deze stappen, bevat dit artikel de aanvullende eigenschappen die nodig zijn om op te geven van de BGP-configuratieparameters.

![BGP voor Cross-Premises](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Controleer of u hebt uitgevoerd voordat u doorgaat, [deel 1](#enablebgp) van deze oefening.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1: maken en configureren van de lokale netwerkgateway

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

In deze oefening blijft het bouwen van de configuratie weergegeven in het diagram. Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Een aantal aandachtspunten met betrekking tot het lokale netwerk gateway parameters:

* De lokale netwerkgateway kan zich in dezelfde of een andere locatie en resourcegroep als de VPN-gateway. In dit voorbeeld laat zien dat ze in verschillende resourcegroepen bevinden in verschillende locaties.
* Het voorvoegsel dat u nodig hebt om aan te geven voor de lokale netwerkgateway is het hostadres van uw BGP-Peer-IP-adres op uw VPN-apparaat. In dit geval is het een/32 adverteren voorvoegsel van "10.52.255.254/32".
* Als een herinnering, moet u andere BGP-ASN's tussen uw on-premises netwerken en Azure VNet. Als ze hetzelfde zijn, moet u uw VNet-ASN wijzigen als het ASN in uw on-premises VPN-apparaat al worden gebruikt om te koppelen aan andere BGP-neighbors.

Controleer voordat u verdergaat of u nog bent verbonden met Abonnement 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. De lokale netwerkgateway maken voor Site5

Zorg ervoor dat de resourcegroep te maken als deze niet gemaakt is voordat u de lokale netwerkgateway maken. U ziet de twee extra parameters voor de lokale netwerkgateway: ASN en BgpPeerAddress.

```powershell
New-AzResourceGroup -Name $RG5 -Location $Location5

New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet-gateway en de lokale netwerkgateway

#### <a name="1-get-the-two-gateways"></a>1. Ophalen van de twee gateways

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. De verbinding tussen TestVNet1 en Site5 maken

In deze stap maakt u de verbinding van TestVNet1 naar Site5. U moet opgeven '-EnableBGP $True "u BGP wilt inschakelen voor deze verbinding. Zoals eerder besproken, is het mogelijk om zowel BGP en niet-BGP-verbindingen voor dezelfde Azure VPN-Gateway. Tenzij BGP is ingeschakeld in de eigenschap connection, wordt Azure niet BGP inschakelen voor deze verbinding zelfs als BGP-parameters al zijn geconfigureerd voor beide gateways.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

Het volgende voorbeeld worden de parameters die u in de sectie van de BGP-configuratie op uw on-premises VPN-apparaat voor deze oefening invoert:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

De verbinding tot stand is gebracht na een paar minuten en de BGP-peeringsessie wordt gestart nadat de verbinding tot stand is gebracht IPsec.

## <a name ="v2vbgp"></a>Deel 3: een VNet-naar-VNet verbinding maken met BGP

In deze sectie voegt een VNet-naar-VNet-verbinding waarvoor BGP is, zoals wordt weergegeven in het volgende diagram:

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

De volgende instructies volgen op de vorige stappen. U moet voltooien [deel I](#enablebgp) maken en configureren van TestVNet1 en de VPN-Gateway met BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1: TestVNet2 en de VPN-gateway maken

Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk, TestVNet2, niet met een van uw VNet-bereiken overlapt.

In dit voorbeeld wordt de virtuele netwerken tot hetzelfde abonnement behoren. U kunt instellen als er een VNet-naar-VNet-verbindingen tussen verschillende abonnementen. Zie voor meer informatie, [een VNet-naar-VNet-verbinding configureren](vpn-gateway-vnet-vnet-rm-ps.md). Zorg ervoor dat u de '-EnableBgp $True "bij het maken van de verbindingen BGP wilt inschakelen.

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
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. De VPN-gateway maken voor TestVNet2 met BGP-parameters

Vraag een openbaar IP-adres worden toegewezen aan de gateway die u maakt voor uw VNet en definieer de vereiste subnet en IP-configuraties.

```powershell
$gwpip2    = New-AzPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

Maak de VPN-gateway met het AS-nummer. U moet de standaard een ASN overschrijven op uw Azure VPN-gateways. De ASN's voor de verbonden VNets moeten een uniek BGP en transitroutering inschakelen.

```powershell
New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Stap 2: TestVNet1 en TestVNet2 gateways verbinden

In dit voorbeeld zijn beide gateways tot hetzelfde abonnement. U kunt deze stap voltooien in dezelfde PowerShell-sessie.

#### <a name="1-get-both-gateways"></a>1. Beide gateways ophalen

Zorg dat u zich aanmeldt bij en verbinding maakt met Abonnement 1.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Beide verbindingen maken

In deze stap maakt u de verbinding van TestVNet1 naar TestVNet2 en de verbinding van TestVNet2 naar TestVNet1.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Zorg ervoor dat BGP inschakelen voor beide verbindingen.
> 
> 

Na het voltooien van deze stappen wordt de verbinding na een paar minuten. De BGP-peeringsessie is beschikbaar zodra de VNet-naar-VNet-verbinding is voltooid.

Als u alle drie de gedeelten van deze oefening hebt voltooid, kunt u de volgende topologie van netwerk hebt gemaakt:

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Volgende stappen

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.
