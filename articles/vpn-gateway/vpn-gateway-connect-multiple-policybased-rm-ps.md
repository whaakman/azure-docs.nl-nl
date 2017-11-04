---
title: 'Verbinding maken met Azure VPN-gateways u meerdere on-premises op beleid gebaseerde VPN-apparaten: Azure Resource Manager: PowerShell | Microsoft Docs'
description: In dit artikel begeleidt u bij het configureren van Azure op route gebaseerde VPN-gateway niet meerdere op beleid gebaseerde VPN-apparaten met behulp van Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/27/2017
ms.author: yushwang
ms.openlocfilehash: db4d8837fb5c5d15364422e957e4914966215674
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>Verbinding maken met Azure VPN-gateways u meerdere on-premises op beleid gebaseerde VPN-apparaten met behulp van PowerShell

In dit artikel helpt u bij het configureren van een Azure op route gebaseerde VPN-gateway verbinding maken met meerdere on-premises op beleid gebaseerde VPN-apparaten gebruik te maken van aangepaste IPsec/IKE-beleidsregels voor S2S-VPN-verbindingen.

## <a name="about-policy-based-and-route-based-vpn-gateways"></a>Over op beleid gebaseerd en op route gebaseerde VPN-gateways

Beleid - *versus* op route gebaseerde VPN-apparaten in hoe de IPSec-verkeer selectoren zijn ingesteld op een verbinding verschillen:

* **Op basis van beleid** VPN-apparaten de combinaties van voorvoegsels van beide netwerken gebruiken om te definiëren hoe verkeer wordt versleuteld/ontsleuteld via IPsec-tunnels. Dit wordt gewoonlijk samengesteld op de firewallapparaten wordt uitgevoerd met het filteren van netwerkpakketten. IPSec-tunnel versleuteling en ontsleuteling worden toegevoegd aan het pakket voor het filteren en het verwerken van de engine.
* **Op route gebaseerde** VPN-apparaten gebruiken any-to-any (jokertekens) verkeer selectoren en IPsec-tunnels laten direct verkeer naar andere tabellen routering/doorsturen. Dit wordt gewoonlijk samengesteld op router platforms waarbij elke IPsec-tunnel is gemodelleerd als een netwerkinterface of VTI (virtuele tunnelinterface).

De volgende diagrammen Markeer twee modellen:

### <a name="policy-based-vpn-example"></a>Op beleid gebaseerde VPN-voorbeeld
![op basis van beleid](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Op route gebaseerde VPN-voorbeeld
![op route gebaseerd](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Azure-ondersteuning voor VPN op basis van beleid
Azure ondersteunt momenteel, beide modi van VPN-gateways: op route gebaseerde VPN-gateways en op beleid gebaseerde VPN-gateways. Ze zijn gebaseerd op verschillende interne platforms, ertoe leiden dat andere specificaties:

|                          | **Beleid gebaseerd VPN-Gateway** | **Op route gebaseerd VPN-Gateway**               |
| ---                      | ---                         | ---                                      |
| **Azure-Gateway SKU**    | Basic                       | Basic, Standard, HighPerformance, VpnGw1, VpnGw2, VpnGw3 |
| **IKE-versie**          | IKEv1                       | IKEv2                                    |
| **Max. S2S-verbindingen** | **1**                       | Basic-/ Standard: 10<br> HighPerformance: 30 |
|                          |                             |                                          |

Het aangepaste IPsec/IKE-beleid, kunt u nu Azure op route gebaseerde VPN-gateways voor het gebruik van selectoren verkeer op basis van het voorvoegsel met de optie configureren '**PolicyBasedTrafficSelectors**', verbinding maken met on-premises op beleid gebaseerde VPN-apparaten. Deze manier kunt u verbinding maken vanaf een virtuele Azure-netwerk en VPN-gateway naar meerdere on-premises VPN-/ firewall-apparaten op basis van beleid, de limiet van één verbinding verwijderen uit de huidige Azure op beleid gebaseerde VPN-gateways.

> [!IMPORTANT]
> 1. Schakel deze connectiviteit door uw on-premises op beleid gebaseerde VPN-apparaten moeten ondersteunen **IKEv2** verbinding maken met de Azure op route gebaseerde VPN-gateways. Raadpleeg de specificaties van uw VPN-apparaat.
> 2. De on-premises netwerken verbinding maken via de VPN-apparaten op basis van beleid met dit mechanisme, kunnen u alleen verbinding maken met Azure virtual network; **ze doorvoer kunnen niet naar andere on-premises netwerken of virtuele netwerken via dezelfde Azure VPN-gateway**.
> 3. De configuratieoptie maakt deel uit van het aangepaste beleid voor IPsec/IKE-verbinding. Als u het verkeer op basis van beleid selector-optie inschakelt, moet u het volledige beleid (IPsec/IKE-algoritmen voor versleuteling en integriteit, kracht en SA-levensduur).

Het volgende diagram laat zien waarom transitroutering via Azure VPN-gateway niet met de optie op basis van beleid werkt:

![doorvoer op basis van beleid](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Zoals u in het diagram, is de Azure VPN-gateway selectoren verkeer van het virtuele netwerk aan elk van de voorvoegsels van het lokale netwerk, maar niet de voorvoegsels voor cross-verbinding. Bijvoorbeeld: lokale site 2, 3, en site 4 kan elk communiceren met VNet1 respectievelijk, maar kan geen verbinding maken via de Azure VPN-gateway met elkaar. Het diagram toont de cross-connect verkeer selectoren die niet beschikbaar in de Azure VPN-gateway in deze configuratie.

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a>Op beleid gebaseerde verkeer selectoren configureren op een verbinding

De instructies in dit artikel Volg het hetzelfde voorbeeld zoals beschreven in [configureren IPsec/IKE-beleid voor S2S of VNet-naar-VNet-verbindingen](vpn-gateway-ipsecikepolicy-rm-powershell.md) een S2S VPN-verbinding tot stand brengen. Dit wordt weergegeven in het volgende diagram:

![s2s-beleid](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

De werkstroom voor deze verbindingen:
1. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway voor uw cross-premises-verbinding maken
2. Een IPsec/IKE-beleid maken
3. Het beleid van toepassing wanneer u een S2S of VNet-naar-VNet-verbinding maken en **inschakelen van het verkeer op basis van beleid selectoren** op de verbinding
4. Als de verbinding al gemaakt is, kunt u deze toepassing of het beleid bijwerken naar een bestaande verbinding

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a>Op beleid gebaseerde verkeer selectoren op een verbinding inschakelen

Zorg ervoor dat u hebt voltooid [deel 3 van het beleid configureren IPsec/IKE artikel](vpn-gateway-ipsecikepolicy-rm-powershell.md) voor deze sectie. Het volgende voorbeeld wordt de parameters en de stappen uit:

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>Stap 1: het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

#### <a name="1-declare-your-variables--connect-to-your-subscription"></a>1. De variabelen declareren & verbinding maken met uw abonnement
Voor deze oefening eerst we onze variabelen declareren. Zorg dat u de waarden door uw eigen waarden vervangt wanneer u configureert voor productie.

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
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
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```
Zorg ervoor dat u overschakelt naar de PowerShell-modus voor het gebruik van de Resource Manager-cmdlets. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken
Het volgende voorbeeld wordt het virtuele netwerk TestVNet1 met drie subnetten en de VPN-gateway. Bij het vervangen van waarden, is het belangrijk dat u altijd de naam het gatewaysubnet bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>Stap 2: een S2S VPN-verbinding maken met een IPsec/IKE-beleid

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

> [!IMPORTANT]
> U moet een IPsec/IKE-beleid maken om in te schakelen, de optie 'UsePolicyBasedTrafficSelectors' op de verbinding.

Het volgende voorbeeld wordt een IPsec/IKE-beleid met deze algoritmen en parameters:
* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA256, PFS24, SA levensduur 3600 seconden & 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-the-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. De S2S VPN-verbinding maken met op beleid gebaseerde verkeer selectoren en IPsec/IKE-beleid
Een S2S VPN-verbinding maken en toepassen van het IPsec/IKE-beleid in de vorige stap hebt gemaakt. Houd rekening met de extra parameter '-UsePolicyBasedTrafficSelectors $True ' waardoor selectoren verkeer op basis van beleid op de verbinding.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Nadat de stappen is voltooid, wordt de S2S VPN-verbinding gebruik van de IPsec/IKE-beleid dat is gedefinieerd en schakel selectoren verkeer op basis van beleid op de verbinding. U kunt dezelfde stappen om meer verbindingen toevoegen aan extra on-premises op beleid gebaseerde VPN-apparaten van dezelfde Azure VPN-gateway herhalen.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>Selectoren verkeer op basis van beleid voor een verbinding bijwerken
De laatste sectie laat zien hoe het verkeer op basis van beleid Selector-optie voor een bestaande S2S VPN-verbinding bijwerken.

### <a name="1-get-the-connection"></a>1. De verbinding ophalen
De verbindingsbron ophalen.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-the-policy-based-traffic-selectors-option"></a>2. Controleer de Selector-optie van verkeer op basis van beleid
De volgende regel geeft aan of de selectoren verkeer op basis van beleid worden gebruikt voor de verbinding:

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

Als de regel retourneert '**True**', vervolgens selectoren verkeer op basis van beleid zijn geconfigureerd op de verbinding; anders wordt '**False**. "

### <a name="3-update-the-policy-based-traffic-selectors-on-a-connection"></a>3. Bijwerken van de selectoren verkeer op basis van beleid voor een verbinding
Zodra u de verbindingsbron hebt verkregen, kunt u in- of uitschakelen van de optie.

#### <a name="disable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors uitschakelen
Het volgende voorbeeld wordt de verkeer op basis van beleid Selector-optie uitgeschakeld, maar het beleid voor IPsec/IKE ongewijzigd blijft:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors inschakelen
Het volgende voorbeeld wordt de verkeer op basis van beleid Selector-optie ingeschakeld, maar het beleid voor IPsec/IKE ongewijzigd blijft:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a>Volgende stappen
Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.

Bekijk ook [configureren IPsec/IKE-beleid voor S2S-VPN- of VNet-naar-VNet-verbindingen](vpn-gateway-ipsecikepolicy-rm-powershell.md) voor meer informatie over aangepaste IPsec/IKE-beleidsregels.
