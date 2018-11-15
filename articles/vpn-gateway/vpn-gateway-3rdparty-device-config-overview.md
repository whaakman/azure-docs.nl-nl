---
title: Partner van VPN-apparaatconfiguraties voor het verbinden met Azure VPN-gateways | Microsoft Docs
description: Dit artikel bevat een overzicht van de partner VPN-apparaatconfiguraties voor het verbinden met Azure VPN-gateways.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: ''
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 901de2ac3d80b3ee529d89a22afae7e823437b9b
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685205"
---
# <a name="overview-of-partner-vpn-device-configurations"></a>Overzicht van VPN-apparaatconfiguraties partner
Dit artikel bevat een overzicht van het configureren van on-premises VPN-apparaten voor het verbinden met Azure VPN-gateways. Een voorbeeld van een virtueel Azure-netwerk en VPN-gateway-installatie wordt gebruikt om u te laten zien u hoe u verbinding maken met verschillende on-premises VPN-apparaatconfiguraties met dezelfde parameters.

## <a name="device-requirements"></a>Vereisten voor apparaten
Azure VPN-gateways gebruiken standaard IPsec/IKE-protocol-pakketten voor site-naar-site (S2S) VPN-tunnels. Zie voor een lijst met IPsec/IKE-parameters en cryptografische algoritmen voor Azure VPN-gateways, [over VPN-apparaten](vpn-gateway-about-vpn-devices.md). U kunt ook opgeven de exacte algoritmen en belangrijkste sterke punten voor een specifieke verbinding zoals beschreven in [over cryptografische vereisten](vpn-gateway-about-compliance-crypto.md).

## <a name ="singletunnel"></a>Één VPN-tunnel
De eerste configuratie in het voorbeeld bestaat uit één S2S-VPN-tunnel tussen een Azure VPN-gateway en een on-premises VPN-apparaat. U kunt eventueel de [Border Gateway Protocol (BGP) via de VPN-tunnel](#bgp).

![Diagram van één S2S-VPN-tunnel](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Zie voor stapsgewijze instructies voor het instellen van een één VPN-tunnel [een site-naar-site-verbinding configureren](vpn-gateway-howto-site-to-site-resource-manager-portal.md). De volgende secties geven de verbindingsparameters voor de voorbeeldconfiguratie van en bieden een PowerShell-script om u aan de slag te helpen.

### <a name="connection-parameters"></a>Verbindingsparameters
Deze sectie vindt u de parameters voor de voorbeelden die worden beschreven in de vorige secties.

| **Parameter**                | **Waarde**                    |
| ---                          | ---                          |
| Virtueel netwerk-adresvoorvoegsels        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN-gateway-IP         | Azure VPN-Gateway IP         |
| On-premises adresvoorvoegsels | 10.51.0.0/16<br>10.52.0.0/16 |
| On-premises VPN-apparaat IP    | On-premises VPN-apparaat IP    |
| * Virtuele netwerk ASN van BGP                | 65010                        |
| * Azure BGP-peer-IP           | 10.12.255.30                 |
| * On-premises BGP-ASN         | 65050                        |
| * On-premises BGP-peer-IP     | 10.52.255.254                |

\* Optionele parameter voor BGP alleen.

### <a name="sample-powershell-script"></a>PowerShell-voorbeeldscript
Deze sectie bevat een voorbeeldscript dat u aan de slag. Zie voor gedetailleerde instructies [een S2S-VPN-verbinding maken met behulp van PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md).

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subscription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect to your subscription and create a new resource group

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create the S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name ="policybased"></a>(Optioneel) Aangepast IPsec/IKE-beleid met UsePolicyBasedTrafficSelectors gebruiken
Als uw VPN-apparaten bieden geen ondersteuning voor any-to-any verkeerkiezers, zoals route- of VTI configuraties, maakt u een aangepast IPsec/IKE-beleid met de [UsePolicyBasedTrafficSelectors](vpn-gateway-connect-multiple-policybased-rm-ps.md) optie.

> [!IMPORTANT]
> U moet een IPsec/IKE-beleid om in te schakelen de **UsePolicyBasedTrafficSelectors** optie op de verbinding.


Het voorbeeldscript maakt u een IPsec/IKE-beleid met de volgende algoritmen en parameters:
* IKEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA1, PFS24, SA-levensduur 7,200 seconden en 20,480,000 KB (20 GB)

Het script wordt de IPsec-/ IKE-beleid toegepast en kunnen de **UsePolicyBasedTrafficSelectors** optie op de verbinding.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>(Optioneel) BGP gebruiken voor S2S VPN-verbinding
Wanneer u de S2S VPN-verbinding maakt, kunt u eventueel gebruiken [BGP voor de VPN-gateway](vpn-gateway-bgp-resource-manager-ps.md). Deze benadering heeft twee verschillen:

* De on-premises adresvoorvoegsels kunnen een afzonderlijke host-adres zijn. IP-adres voor de on-premises BGP-peer wordt als volgt bepaald:

    ```powershell
    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
    ```

* Wanneer u de verbinding maakt, moet u instellen de **- EnableBGP** optie op $True:

    ```powershell
    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor stapsgewijze instructies voor het instellen van de actief / actief VPN-gateways [actief / actief VPN-gateways voor cross-premises en VNet-naar-VNet-verbindingen configureren](vpn-gateway-activeactive-rm-powershell.md).

