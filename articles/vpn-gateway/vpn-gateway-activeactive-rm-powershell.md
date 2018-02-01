---
title: 'Actieve S2S VPN-verbindingen configureren voor VPN-Gateways: Azure Resource Manager: PowerShell | Microsoft Docs'
description: In dit artikel begeleidt u bij het configureren van actieve verbindingen met Azure VPN-Gateways met Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: yushwang
ms.openlocfilehash: 41cca764335f21bed60fe968288bc8b8274f3215
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Actieve S2S VPN-verbindingen met Azure VPN-Gateways configureren

Dit artikel begeleidt u bij de stappen voor het maken van de actieve cross-premises en VNet-naar-VNet-verbindingen met het Resource Manager-implementatiemodel en PowerShell.

## <a name="about-highly-available-cross-premises-connections"></a>Over maximaal beschikbare cross-premises verbindingen
Om te zorgen voor hoge beschikbaarheid voor cross-premises en VNet-naar-VNet-connectiviteit, moet u meerdere VPN-gateways implementeren en meerdere parallelle verbindingen tussen uw netwerken en Azure tot stand brengen. Zie [maximaal beschikbare Cross-Premises en VNet-naar-VNet-connectiviteit](vpn-gateway-highlyavailable.md) voor een overzicht van opties voor netwerkconnectiviteit en -topologie.

Dit artikel bevat de instructies voor het instellen van een actieve cross-premises VPN-verbinding en de actieve verbinding tussen twee virtuele netwerken.

* [Deel 1 - maken en configureren van uw Azure VPN-gateway in de actieve-actieve modus](#aagateway)
* [Deel 2: actieve cross-premises verbindingen tot stand brengen](#aacrossprem)
* [Deel 3 - actief / actief-VNet-naar-VNet-verbindingen tot stand brengen](#aav2v)

Als u al een VPN-gateway hebt, kunt u:
* [Bijwerken van een bestaande VPN-gateway van de actieve stand-by-actief-actief of andersom](#aaupdate)

U kunt deze samen om samen te stellen een meer complexe, maximaal beschikbare netwerktopologie die voldoet aan de behoeften van uw combineren.

> [!IMPORTANT]
> De actieve-actieve modus maakt gebruik van de volgende SKU's: 
  * VpnGw1, VpnGw2, VpnGw3
  * HighPerformance (voor oude verouderde SKU's)
> 
> 

## <a name ="aagateway"></a>Deel 1 - maken en configureren van actieve VPN-gateways
De volgende stappen configureert uw Azure VPN-gateway in actief / actief-modi. De belangrijkste verschillen tussen de actieve en stand-by active-gateways:

* U moet twee IP-Gateway configuraties met twee openbare IP-adressen maken
* U moet de vlag EnableActiveActiveFeature instellen
* De gateway-SKU moet VpnGw1, VpnGw2, VpnGw3 of HighPerformance (verouderde SKU).

De overige eigenschappen zijn hetzelfde als de niet-actieve gateways. 

### <a name="before-you-begin"></a>Voordat u begint
* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* U moet de Azure Resource Manager PowerShell-cmdlets installeren. Zie [overzicht van Azure PowerShell](/powershell/azure/overview) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1: Maak en configureer VNet1
#### <a name="1-declare-your-variables"></a>1. De variabelen declareren
Voor deze oefening declareert u eerst de variabelen. In het volgende voorbeeld worden de variabelen gedeclareerd met de waarden voor deze oefening. Zorg dat u de waarden door uw eigen waarden vervangt wanneer u configureert voor productie. U kunt deze variabelen gebruiken als u de stappen wilt doorlopen om vertrouwd te raken met dit type configuratie. Wijzig de variabelen en kopieer en plak ze in uw PowerShell-console.

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
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
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Verbinding maken met uw abonnement en een nieuwe resourcegroep maken
Zorg ervoor dat u overschakelt naar de PowerShell-modus als u de Resource Manager-cmdlets wilt gebruiken. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 maken
In onderstaand voorbeeld wordt een virtueel netwerk gemaakt met de naam TestVNet1. Er worden ook drie subnetten gemaakt, GatewaySubnet, FrontEnd en BackEnd. Wanneer u de waarden vervangt, is het belangrijk dat u de juiste namen voor de gatewaysubnets gebruikt, in het bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Stap 2: de VPN-gateway voor TestVNet1 maken met de actieve-actieve modus
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Maken van de openbare IP-adressen en de gateway-IP-configuraties
Aanvraag twee openbare IP-adressen worden toegewezen aan de gateway die u voor uw VNet maakt. Ook definieert u het subnet en IP-configuraties die nodig zijn.

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. De VPN-gateway maken met een actief / actief-configuratie
Maak de gateway van het virtuele netwerk voor TestVNet1. Houd er rekening mee dat er twee GatewayIpConfig vermeldingen zijn en de EnableActiveActiveFeature-vlag is ingesteld. Het maken van een gateway kan even duren (45 minuten of langer).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. De gateway openbare IP-adressen en de BGP-Peer-IP-adres verkrijgen
Zodra de gateway is gemaakt, moet u het BGP-Peer-IP-adres op de Azure VPN-Gateway. Dit adres wordt gebruikt voor de Azure VPN-Gateway configureren als een BGP-Peer voor uw on-premises VPN-apparaten.

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

Gebruik de volgende cmdlets voor het weergeven van de twee openbare IP-adressen toegewezen voor uw VPN-gateway en de bijbehorende BGP-Peer-IP-adressen voor elk gatewayexemplaar:

```powershell
PS D:\> $gw1pip1.IpAddress
40.112.190.5

PS D:\> $gw1pip2.IpAddress
138.91.156.129

PS D:\> $vnet1gw.BgpSettingsText
{
  "Asn": 65010,
  "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
  "PeerWeight": 0
}
```

De volgorde van de openbare IP-adressen voor de gatewayexemplaren en de bijbehorende BGP-Peering-adressen zijn hetzelfde. In dit voorbeeld wordt de gateway-VM met openbare IP-adres van 40.112.190.5 10.12.255.4 gebruikt als het adres BGP-Peering en de gateway met 138.91.156.129 10.12.255.5 wordt gebruikt. Deze informatie is nodig bij het instellen van uw on premises VPN-apparaten verbinding maken met de gateway actief-actief. De gateway wordt weergegeven in het diagram hieronder met alle adressen:

![actieve-actieve gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Zodra de gateway is gemaakt, kunt u deze gateway tot stand brengen van actieve cross-premises of VNet-naar-VNet-verbinding. De volgende secties helpt bij de stappen voor het voltooien van de oefening.

## <a name ="aacrossprem"></a>Deel 2: een actieve cross-premises-verbinding tot stand brengen
Als u wilt een cross-premises-verbinding tot stand brengen, moet u een lokale netwerkgateway om weer te geven van uw on-premises VPN-apparaat en een verbinding met de Azure VPN-gateway verbinding met de lokale netwerkgateway maken. In dit voorbeeld is de Azure VPN-gateway actief / actief-actief. Als gevolg hiervan, zelfs als er slechts één on-premises VPN-apparaat (lokale netwerkgateway) en één verbindingsbron, beide exemplaren van Azure VPN-gateway tot stand brengen S2S VPN-tunnels met de on-premises-apparaat.

Voordat u doorgaat, Controleer of u hebt voltooid [Part 1](#aagateway) van deze oefening.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1 - maken en configureren van de lokale netwerkgateway
#### <a name="1-declare-your-variables"></a>1. De variabelen declareren
In deze oefening blijven maken van de configuratie in het diagram worden weergegeven. Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Een aantal dingen om aan te geven met betrekking tot het lokale netwerk gateway-parameters:

* De lokale netwerkgateway kan zich in dezelfde of een andere locatie en resourcegroep als de VPN-gateway. Dit voorbeeld ziet u ze in verschillende resourcegroepen, maar in dezelfde Azure-locatie.
* Als er slechts één on-premises VPN-apparaat zoals hierboven beschreven, wordt de verbinding actief / actief kunt werken met of zonder BGP-protocol. In dit voorbeeld maakt gebruik van BGP voor de verbinding tussen meerdere locaties.
* Als u BGP is ingeschakeld, is het voorvoegsel moet u opgeven voor de lokale netwerkgateway adres van de host van de BGP-Peer-IP-adres op uw VPN-apparaat. In dit geval is een /32 prefix '10.52.255.253/32'.
* U moet verschillende ASN van BGP tussen uw on-premises netwerken en Azure VNet gebruiken als een herinnering. Als ze hetzelfde zijn, moet u uw VNet ASN wijzigen als de ASN in uw on-premises VPN-apparaat al worden gebruikt als peer met andere BGP-neighbors.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. De lokale netwerkgateway voor Site5 maken
Controleer voordat u verder gaat of u nog bent verbonden met abonnement 1. De resourcegroep maken als deze nog niet is gemaakt.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet-gateway en de lokale netwerkgateway
#### <a name="1-get-the-two-gateways"></a>1. Ophalen van de twee gateways

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. De verbinding tussen TestVNet1 en Site5 maken
In deze stap maakt u de verbinding van TestVNet1 naar Site5_1 met 'EnableBGP' is ingesteld op $True.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN- en BGP-parameters voor uw on-premises VPN-apparaat
Het volgende voorbeeld bevat de parameters die u in de configuratiesectie BGP op uw on-premises VPN-apparaat voor deze oefening wordt invoert:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.253
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

De verbinding moet worden gemaakt na een paar minuten en de BGP-peeringsessie wordt gestart nadat de IPsec-verbinding tot stand is gebracht. In dit voorbeeld is tot nu toe geconfigureerd dat slechts één on-premises VPN-apparaat, wat resulteert in het diagram hieronder weergegeven:

![active-active-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Stap 3: verbinding maken met twee on-premises VPN-apparaten aan de actieve VPN-gateway
Als u twee VPN-apparaten op de dezelfde on-premises netwerk hebt, kunt u dubbele redundantie kunt bereiken door de Azure VPN-gateway naar het tweede apparaat met een VPN-verbinding te maken.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. De tweede lokale netwerkgateway voor Site5 maken
De gateway IP-adres, adresvoorvoegsel en adres voor BGP-peering voor de tweede lokale netwerkgateway mogen elkaar niet overlappen met de vorige lokale netwerkgateway voor de dezelfde on-premises netwerk.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"
```

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Verbinding maken met de VNet-gateway en de tweede lokale netwerkgateway
Maken van de verbinding van TestVNet1 naar Site5_2 met 'EnableBGP' is ingesteld op $True

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5
```

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN- en BGP-parameters voor uw tweede on-premises VPN-apparaat
Op deze manier hieronder een lijst met de parameters voert u in het tweede VPN-apparaat:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Wanneer de verbinding (tunnels) worden gemaakt, hebt u twee redundante VPN-apparaten en tunnels verbinding maken met uw on-premises netwerk en Azure:

![dual-redundancy-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name ="aav2v"></a>Deel 3: een actieve VNet-naar-VNet-verbinding tot stand brengen
Deze sectie maakt een actief / actief-VNet-naar-VNet-verbinding met BGP. 

De onderstaande instructies volgen op de hierboven beschreven stappen. U moet voltooien [Part 1](#aagateway) maken en configureren van TestVNet1 en de VPN-Gateway met BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1: TestVNet2 en de VPN-gateway maken
Het is belangrijk om ervoor te zorgen dat de IP-adresruimte van het nieuwe virtuele netwerk TestVNet2, niet met een van uw VNet-bereiken overlapt.

In dit voorbeeld wordt de virtuele netwerken tot hetzelfde abonnement behoren. U kunt een VNet-naar-VNet-verbindingen tussen de verschillende abonnementen; instellen Raadpleeg [een VNet-naar-VNet-verbinding configureren](vpn-gateway-vnet-vnet-rm-ps.md) voor meer informatie over meer informatie. Zorg ervoor dat u toevoegt het '-EnableBgp $True "bij het maken van de verbindingen om in te schakelen van BGP.

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren
Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
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
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
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

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. De actieve VPN-gateway voor TestVNet2 maken
Aanvraag twee openbare IP-adressen worden toegewezen aan de gateway die u voor uw VNet maakt. Ook definieert u het subnet en IP-configuraties die nodig zijn.

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

Maak de VPN-gateway met het AS-nummer en de vlag 'EnableActiveActiveFeature'. Houd er rekening mee dat u de standaard een ASN op uw Azure VPN-gateways moet overschrijven. De ASN's voor de verbonden VNets moet verschillen van BGP en transitroutering inschakelen.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
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

Na het voltooien van deze stappen de verbindingen worden tot stand brengen in een paar minuten en het BGP zijn sessie als peer nadat de VNet-naar-VNet-verbinding is voltooid met een dual redundantie:

![active-active-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Een bestaande VPN-gateway bijwerken

Deze sectie helpt u een bestaande Azure VPN-gateway gewijzigd van actief stand-by-actieve modus of vice versa.

### <a name="change-an-active-standby-gateway-to-an-active-active-gateway"></a>Een actieve stand-by-gateway naar een actieve gateway wijzigen

Het volgende voorbeeld wordt een gateway actief stand-by-converteert naar een actief / actief-gateway. Wanneer u een actieve stand-by-gateway naar actieve wijzigt, u een ander openbaar IP-adres maken en vervolgens een tweede IP-Gateway-configuratie toevoegen.

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

Vervang de volgende parameters gebruikt voor de voorbeelden met de instellingen die u nodig hebt voor uw eigen configuratie en vervolgens deze variabelen declareren.

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"
```

U kunt na de variabelen declareren, kopiëren en plakken in dit voorbeeld aan uw PowerShell-console.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Het openbare IP-adres maken, en voeg vervolgens de tweede gateway IP-configuratie

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. Actieve-actieve modus inschakelen en de gateway bijwerken

In deze stap maakt u actieve-actieve modus inschakelen en bijwerken van de gateway. In het voorbeeld wordt de VPN-gateway momenteel wordt gebruikt door een verouderde standaard SKU. Actieve biedt echter geen ondersteuning voor de standaard SKU. Als u de verouderde SKU die wordt ondersteund (in dit geval HighPerformance), moet u gewoon de ondersteunde oude SKU die u wilt gebruiken opgeven.

* U kunt een verouderde SKU niet wijzigen in een van de nieuwe SKU's met behulp van deze stap. U kunt alleen een verouderde SKU op een andere ondersteunde verouderde SKU grootte. Bijvoorbeeld, kan u wijzigen de SKU van standaard VpnGw1 (Hoewel VpnGw1 wordt ondersteund voor actieve) omdat standaard een verouderde SKU is en VpnGw1 een huidige SKU. Zie voor meer informatie over het wijzigen van de grootte en migreren SKU's [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gwsku).

* Als u het formaat van een huidige SKU, bijvoorbeeld VpnGw1 naar VpnGw3, wilt kunt u doen met behulp van deze stap, omdat de SKU's zich in dezelfde SKU-serie. Om dit te doen, gebruikt u de waarde:```-GatewaySku VpnGw3```

Wanneer u dit in uw omgeving, als u niet hoeft te vergroten of verkleinen van de gateway, hoeft u niet de GatewaySku - opgeven. U ziet in deze stap moet u het gateway-object instellen in PowerShell voor het activeren van de feitelijke update. Deze update kan 30 tot 45 minuten duren, zelfs als u niet vergroten of uw gateway verkleinen bent.

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

### <a name="change-an-active-active-gateway-to-an-active-standby-gateway"></a>Een actieve gateway naar een actieve stand-by-gateway wijzigen
#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

Vervang de volgende parameters gebruikt voor de voorbeelden met de instellingen die u nodig hebt voor uw eigen configuratie en vervolgens deze variabelen declareren.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"
```

Na de variabelen declareren, krijgen de naam van de IP-configuratie die u wilt verwijderen.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Verwijderen van de IP-configuratie van de gateway en de actieve-actieve modus uitschakelen

In dit voorbeeld gebruiken om te verwijderen van de IP-configuratie van de gateway en actieve-actieve modus uitschakelen. U ziet dat u het gateway-object in PowerShell voor het activeren van de werkelijke update moet instellen.

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

Deze update kan maximaal 30 tot 45 minuten duren.

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.