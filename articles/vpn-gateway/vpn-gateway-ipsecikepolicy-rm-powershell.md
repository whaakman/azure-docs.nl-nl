---
title: 'IPsec/IKE-beleid voor S2S VPN- of VNet-naar-VNet-verbindingen te configureren: Azure Resource Manager: PowerShell | Microsoft Docs'
description: IPsec/IKE-beleid voor S2S- of VNet-naar-VNet-verbindingen configureren met Azure VPN-Gateways met behulp van Azure Resource Manager en PowerShell.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: yushwang
ms.openlocfilehash: d04d62d66b4ba22437e6d854566f8bbf5536a6fc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121074"
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>IPsec/IKE-beleid voor S2S-VPN- of VNet-naar-VNet-verbindingen configureren

In dit artikel begeleidt u bij de stappen voor het configureren van IPsec/IKE-beleid voor Site-naar-Site VPN- of VNet-naar-VNet-verbindingen met behulp van de Resource Manager-implementatiemodel en PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about"></a>Informatie over IPsec en IKE-beleid-parameters voor Azure VPN-gateways
IPsec en IKE-protocol standaard ondersteunt een breed scala aan cryptografische algoritmen in verschillende combinaties. Raadpleeg [over cryptografische vereisten en Azure VPN-gateways](vpn-gateway-about-compliance-crypto.md) om te zien hoe dit kan helpen ervoor te zorgen dat cross-premises en VNet-naar-VNet-connectiviteit naleving of beveiliging aan uw vereisten voldoen.

Dit artikel bevat instructies voor het maken en een IPsec/IKE-beleid configureren en toepassen op een nieuwe of bestaande verbinding:

* [Deel 1: werkstroom voor het maken en IPsec/IKE-beleid instellen](#workflow)
* [Deel 2 - ondersteunde cryptografische algoritmen en belangrijkste sterke punten](#params)
* [Deel 3: een nieuwe S2S VPN-verbinding maken met IPsec/IKE-beleid](#crossprem)
* [Deel 4: een nieuwe VNet-naar-VNet-verbinding maken met IPsec/IKE-beleid](#vnet2vnet)
* [Deel 5 - beheren (maken, toevoegen, verwijderen) IPsec/IKE-beleid voor een verbinding](#managepolicy)

> [!IMPORTANT]
> 1. Houd er rekening mee dat IPsec/IKE-beleid werkt alleen op de volgende gateway-SKU's:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (route-based)
>    * ***Standard*** en ***HighPerformance*** (op een route gebaseerd)
> 2. U kunt maar ***één*** beleidscombinatie opgeven voor een bepaalde verbinding.
> 3. U moet alle algoritmen en parameters opgeven voor zowel IKE (Main Mode) en IPsec (Quick Mode). Gedeeltelijke beleidsspecificatie is niet toegestaan.
> 4. Neem contact op met uw VPN-leverancier apparaatspecificaties om te controleren of dat het beleid wordt ondersteund op uw on-premises VPN-apparaten. S2S- of VNet-naar-VNet-verbindingen kunnen niet tot stand brengen als het beleid niet compatibel zijn.

## <a name ="workflow"></a>Deel 1: werkstroom voor het maken en IPsec/IKE-beleid instellen
In deze sectie geeft een overzicht van de werkstroom voor het maken en bijwerken van IPsec/IKE-beleid op een S2S VPN- of VNet-naar-VNet-verbinding:
1. Een virtueel netwerk en een VPN-gateway maken
2. Een lokale netwerkgateway voor cross-premises-verbinding of een ander virtueel netwerk en een gateway voor VNet-naar-VNet-verbinding maken
3. Een IPsec/IKE-beleid maken met geselecteerde algoritmen en parameters
4. Maak een verbinding (IPSec- of VNet2VNet) met het IPsec/IKE-beleid
5. Toevoegen/bijwerken/verwijderen een IPsec/IKE-beleid voor een bestaande verbinding

De instructies in dit artikel helpt u bij het instellen en configureren van IPsec/IKE-beleid, zoals wordt weergegeven in het diagram:

![ipsec-ike-policy](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>Deel 2 - ondersteunde cryptografische algoritmen en belangrijkste sterke punten

De volgende tabel bevat de ondersteunde cryptografische algoritmen en sleutelsterkten die door de klant:

| **IPsec/IKEv2**  | **Opties**    |
| ---  | --- 
| IKEv2-versleuteling | AES256, AES192, AES128, DES3, DES  
| IKEv2-integriteit  | SHA384, SHA256, SHA1, MD5  |
| DH-groep         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None |
| IPsec-versleuteling | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, geen    |
| IPsec-integriteit  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS-groep        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, geen 
| QM SA-levensduur   | (**Optioneel**: standaardwaarden worden gebruikt als u niets opgeeft)<br>Seconden (geheel getal; **min. 300** /standaard 27000 seconden)<br>KB (geheel getal; **min. 1024**/standaard 102400000 KB)   |
| Verkeersselector | UsePolicyBasedTrafficSelectors ** ($True/$False; **Optioneel**, standaard $False als u niets opgeeft)    |
|  |  |

> [!IMPORTANT]
> 1. **De configuratie van uw on-premises VPN-apparaat moet overeenkomen met of bevatten de volgende algoritmen en parameters die u op het Azure IPsec/IKE-beleid opgeeft:**
>    * IKE-versleutelingsalgoritme (Main Mode / fase 1)
>    * IKE-integriteitsalgoritme (Main Mode / fase 1)
>    * DH-groep (hoofdmodus / fase 1)
>    * IPsec-versleutelingsalgoritme (Quick Mode / fase 2)
>    * IPsec-integriteitsalgoritme (Quick Mode / fase 2)
>    * PFS-groep (Quick Mode / fase 2)
>    * Kiezer-verkeer (als UsePolicyBasedTrafficSelectors wordt gebruikt)
>    * De SA-levensduren zijn alleen lokale specificaties en hoeven niet overeen te komen.
>
> 2. **Als GCMAES als voor IPsec-versleutelingsalgoritme wordt gebruikt, moet u de hetzelfde GCMAES-algoritme en de lengte van de sleutel selecteren voor IPSec-integriteit; bijvoorbeeld, met behulp van GCMAES128 voor zowel**
> 3. In de bovenstaande tabel:
>    * IKEv2 komt overeen met Main Mode of fase 1
>    * IPsec komt overeen met de snelle modus of fase 2
>    * DH-groep Hiermee geeft u de Diffie-Hellmen-groep die wordt gebruikt in Main Mode of in fase 1
>    * PFS-groep opgegeven de Diffie-Hellmen-groep die wordt gebruikt in snelle modus of in fase 2
> 4. SA-levensduur voor IKEv2 Main Mode staat vastgesteld op 28.800 seconden op de Azure VPN-gateways
> 5. 'UsePolicyBasedTrafficSelectors' instellen op $True op een verbinding, wordt de Azure VPN-gateway verbinding maken met op beleid gebaseerde VPN-firewall on-premises configureren. Als u PolicyBasedTrafficSelectors inschakelt, moet u om te controleren of dat uw VPN-apparaat heeft de overeenkomende verkeerkiezers gedefinieerd voor alle combinaties van uw on-premises netwerk (lokale netwerkgateway) voorvoegsels naar/van de voorvoegsels van de Azure-netwerk, in plaats van any-to-any. Als uw lokale netwerkvoorvoegsels bijvoorbeeld 10.1.0.0/16 en 10.2.0.0/16 zijn, en de voorvoegsels van uw virtuele netwerk 192.168.0.0/16 en 172.16.0.0/16, moet u de volgende verkeersselectoren opgeven:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

Zie voor meer informatie over op beleid gebaseerde verkeerkiezers [verbinding maken met meerdere on-premises op beleid gebaseerde VPN-apparaten](vpn-gateway-connect-multiple-policybased-rm-ps.md).

De volgende tabel bevat de bijbehorende Diffie-Hellman-groepen die door het aangepaste beleid worden ondersteund:

| **Diffie-Hellman-groep**  | **DHGroup**              | **PFSGroup** | **Sleutellengte** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768-bits MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024-bits MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048-bits MODP  |
| 19                        | ECP256                   | ECP256       | 256-bits ECP    |
| 20                        | ECP384                   | ECP284       | 384-bits ECP    |
| 24                        | DHGroup24                | PFS24        | 2048-bits MODP  |

Raadpleeg [RFC3526](https://tools.ietf.org/html/rfc3526) en [RFC5114](https://tools.ietf.org/html/rfc5114) voor meer informatie.

## <a name ="crossprem"></a>Deel 3: een nieuwe S2S VPN-verbinding maken met IPsec/IKE-beleid

In dit gedeelte leidt u door de stappen voor het maken van een S2S-VPN-verbinding met een IPsec/IKE-beleid. De volgende stappen maakt de verbinding, zoals wordt weergegeven in het diagram:

![s2s-beleid](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Zie [maken van een S2S-VPN-verbinding](vpn-gateway-create-site-to-site-rm-powershell.md) voor meer Stapsgewijze instructies gedetailleerde voor het maken van een S2S-VPN-verbinding.

### <a name="before"></a>Voordat u begint

* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Installeer de Azure Resource Manager PowerShell-cmdlets. Zie [overzicht van Azure PowerShell](/powershell/azure/overview) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="createvnet1"></a>Stap 1: het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

We beginnen door op te geven van de variabelen voor deze oefening. Zorg dat u de waarden door uw eigen waarden vervangt wanneer u configureert voor productie.

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Verbinding maken met uw abonnement en een nieuwe resourcegroep maken

Zorg ervoor dat u overschakelt naar de PowerShell-modus als u de Resource Manager-cmdlets wilt gebruiken. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

Het volgende voorbeeld maakt u het virtuele netwerk, TestVNet1 met drie subnetten gemaakt, en de VPN-gateway. Wanneer u de waarden vervangt, is het belangrijk dat u de juiste namen voor de gatewaysubnets gebruikt, in het bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1

New-AzLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="s2sconnection"></a>Stap 2: een S2S-VPN-verbinding maken met een IPsec/IKE-beleid

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

Het volgende voorbeeldscript maakt een IPsec/IKE-beleid met de volgende algoritmen en parameters:

* IKEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA256, PFS None, SA Lifetime 14400 seconds & 102400000KB

```powershell
$ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

Als u GCMAES voor IPsec gebruikt, moet u de hetzelfde GCMAES-algoritme en de lengte van de sleutel voor IPsec-codering en integriteit. Bijvoorbeeld, de bijbehorende parameters worden '-IpsecEncryption GCMAES256 - IpsecIntegrity GCMAES256 "bij het gebruik van GCMAES256.

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a>2. De S2S VPN-verbinding maken met het IPsec/IKE-beleid

Een S2S-VPN-verbinding maken en toepassen van het eerder gemaakte IPsec/IKE-beleid.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

U kunt eventueel toevoegen '-UsePolicyBasedTrafficSelectors $True ' aan de cmdlet van de verbinding maken om in te schakelen van Azure VPN-gateway verbinding maken met on-premises, op beleid gebaseerde VPN-apparaten, zoals hierboven beschreven.

> [!IMPORTANT]
> Zodra een IPsec/IKE-beleid op een verbinding is opgegeven, wordt de Azure VPN-gateway alleen verzenden of het voorstel IPsec/IKE met opgegeven cryptografische algoritmen en belangrijkste sterke punten op die bepaalde verbinding accepteren. Zorg ervoor dat uw on-premises VPN-apparaat voor de verbinding wordt gebruikt of accepteert de exacte beleidscombinatie anders de S2S VPN-tunnel niet tot stand brengen.


## <a name ="vnet2vnet"></a>Deel 4: een nieuwe VNet-naar-VNet-verbinding maken met IPsec/IKE-beleid

De stappen voor het maken van een VNet-naar-VNet-verbinding met een IPsec/IKE-beleid zijn vergelijkbaar met dat van een S2S-VPN-verbinding. De volgende voorbeeldscripts Maak de verbinding, zoals wordt weergegeven in het diagram:

![V2V-beleid](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Zie [maken van een VNet-naar-VNet-verbinding](vpn-gateway-vnet-vnet-rm-ps.md) voor meer stappen gedetailleerde voor het maken van een VNet-naar-VNet-verbinding. U moet voltooien [deel 3](#crossprem) maken en configureren van TestVNet1 en de VPN-Gateway.

### <a name="createvnet2"></a>Stap 1: de tweede virtuele netwerk en VPN-gateway maken

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

Zorg ervoor dat u de waarden vervangt door de waarden die u voor uw configuratie wilt gebruiken.

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a>2. Het tweede virtuele netwerk en VPN-gateway in de nieuwe resourcegroep maken

```powershell
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a>Stap 2: een VNet-toVNet verbinding maken met het IPsec/IKE-beleid

Net als bij de S2S VPN-verbinding een IPsec/IKE-beleid maken en toepassen op beleid naar de nieuwe verbinding.

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

Het volgende voorbeeldscript maakt een ander beleid voor IPsec/IKE met de volgende algoritmen en parameters:
* IKEv2: AES128, SHA1, DHGroup14
* IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 14400 seconds & 102400000KB

```powershell
$ipsecpolicy2 = New-AzIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a>2. VNet-naar-VNet-verbindingen maken met het IPsec/IKE-beleid

Een VNet-naar-VNet-verbinding maken en toepassen van het IPsec/IKE-beleid dat u hebt gemaakt. In dit voorbeeld zijn beide gateways tot hetzelfde abonnement. Het is dus mogelijk te maken en configureren van beide verbindingen met de dezelfde IPsec/IKE-beleid in dezelfde PowerShell-sessie.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Zodra een IPsec/IKE-beleid op een verbinding is opgegeven, wordt de Azure VPN-gateway alleen verzenden of het voorstel IPsec/IKE met opgegeven cryptografische algoritmen en belangrijkste sterke punten op die bepaalde verbinding accepteren. Zorg ervoor dat de IPsec-beleid voor beide verbindingen hetzelfde zijn, anders wordt de VNet-naar-VNet-verbinding niet tot stand brengen.

De verbinding tot stand is gebracht in een paar minuten en hebt u de volgende netwerktopologie zoals wordt weergegeven in het begin na het voltooien van deze stappen:

![ipsec-ike-policy](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>Deel 5 - Update IPsec/IKE-beleid voor een verbinding

De laatste sectie laat zien hoe u voor het beheren van IPsec/IKE-beleid voor een bestaande S2S of VNet-naar-VNet-verbinding. De oefening hieronder leidt u door de volgende bewerkingen uit op een verbinding:

1. Het IPsec/IKE-beleid van een verbinding weergeven
2. Toevoegen of bijwerken van het IPsec/IKE-beleid op een verbinding
3. Het IPsec/IKE-beleid van een verbinding verwijderen

De stappen die van toepassing op zowel S2S- en VNet-naar-VNet-verbindingen.

> [!IMPORTANT]
> IPsec/IKE-beleid wordt ondersteund op *Standard* en *HighPerformance* op route gebaseerde VPN-gateways alleen. Het werkt niet op de Basic gateway-SKU of de op beleid gebaseerde VPN-gateway.

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Het IPsec/IKE-beleid van een verbinding weergeven

Het volgende voorbeeld ziet hoe u aan het IPsec/IKE-beleid dat is geconfigureerd op een verbinding. De scripts blijven ook van de bovenstaande oefeningen.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

De laatste opdracht geeft een lijst van het huidige IPsec/IKE-beleid dat is geconfigureerd op de verbinding, indien deze aanwezig is. Hier volgt een voorbeeld van uitvoer voor de verbinding:

```powershell
SALifeTimeSeconds   : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

Als er geen IPsec/IKE-beleid geconfigureerd, de opdracht (PS > $connection6.policy) een lege geretourneerde opgehaald. Dit betekent niet dat IPsec/IKE is niet geconfigureerd op de verbinding, maar er is geen aangepast IPsec/IKE-beleid. Het standaardbeleid onderhandeld tussen uw on-premises VPN-apparaat en de Azure VPN-gateway maakt gebruik van de huidige verbinding.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Toevoegen of bijwerken van een IPsec/IKE-beleid voor een verbinding

De stappen om een nieuw beleid toevoegen of bijwerken van een bestaand beleid op een verbinding zijn hetzelfde: een nieuw beleid maken en vervolgens het nieuwe beleid toepassen op de verbinding.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

Toevoegen als u wilt inschakelen 'UsePolicyBasedTrafficSelectors' bij het verbinden met een on-premises op beleid gebaseerde VPN-apparaat, de '-UsePolicyBaseTrafficSelectors "parameter aan de cmdlet, of stel deze in op $False de optie uitschakelen:

```powershell
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

U kunt de verbinding opnieuw om te controleren of het beleid is bijgewerkt.

```powershell
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

U ziet de uitvoer van de laatste regel, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
SALifeTimeSeconds   : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Een IPsec/IKE-beleid van een verbinding verwijderen

Wanneer u het aangepaste beleid van een verbinding verwijdert, wordt de Azure VPN-gateway teruggezet naar de [standaardlijst met IPsec/IKE-voorstellen](vpn-gateway-about-vpn-devices.md) en start een opnieuw nieuwe onderhandeling met uw on-premises VPN-apparaat.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

U kunt hetzelfde script gebruiken om te controleren als het beleid is verwijderd uit de verbinding.

## <a name="next-steps"></a>Volgende stappen

Zie [verbinding maken met meerdere on-premises op beleid gebaseerde VPN-apparaten](vpn-gateway-connect-multiple-policybased-rm-ps.md) voor meer informatie over op beleid gebaseerde verkeerkiezers.

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.
