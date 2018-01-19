---
title: 'IPsec/IKE-beleid voor S2S-VPN- of VNet-naar-VNet-verbindingen te configureren: Azure Resource Manager: PowerShell | Microsoft Docs'
description: In dit artikel begeleidt u bij het configureren van beleid voor IPsec/IKE voor S2S of VNet-naar-VNet-verbindingen met Azure VPN-Gateways met Azure Resource Manager en PowerShell.
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
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: 323c008f7da833d627b35621a24cc29db1283847
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>Beleid voor IPsec/IKE voor S2S-VPN- of VNet-naar-VNet-verbindingen configureren

Dit artikel begeleidt u bij de stappen voor het configureren van beleid voor IPsec/IKE voor Site-naar-Site VPN- of VNet-naar-VNet-verbindingen met het Resource Manager-implementatiemodel en PowerShell.

## <a name="about"></a>Informatie over IPsec en IKE beleidsparameters voor Azure VPN-gateways
IPsec en IKE-protocol standaard ondersteunt een groot aantal cryptografische algoritmen in verschillende combinaties. Raadpleeg [over cryptografische vereisten en Azure VPN-gateways](vpn-gateway-about-compliance-crypto.md) om te zien hoe dit kan helpen waarborgen cross-premises en VNet-naar-VNet-connectiviteit voldoen aan uw vereisten voor naleving of beveiliging.

Dit artikel bevat instructies voor het maken en een IPsec/IKE-beleid configureren en toepassen op een nieuwe of bestaande verbinding:

* [Deel 1: werkstroom voor het maken en IPsec/IKE-beleid instellen](#workflow)
* [Deel 2 - ondersteund cryptografische algoritmen en de belangrijkste voordelen](#params)
* [Deel 3: een nieuwe S2S VPN-verbinding maken met IPsec/IKE-beleid](#crossprem)
* [Deel 4: een nieuwe VNet-naar-VNet-verbinding maken met IPsec/IKE-beleid](#vnet2vnet)
* [Deel 5 - beheren (maken, toevoegen, verwijderen) IPsec/IKE-beleid voor een verbinding](#managepolicy)

> [!IMPORTANT]
> 1. Houd er rekening mee dat IPsec/IKE-beleid alleen op de volgende gateway-SKU's werkt:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (op route gebaseerd)
>    * ***Standaard*** en ***HighPerformance*** (op route gebaseerd)
> 2. U kunt maar ***één*** beleidscombinatie opgeven voor een bepaalde verbinding.
> 3. U moet alle algoritmen en parameters opgeven voor zowel IKE (hoofdmodus) en IPsec (snelle modus). Gedeeltelijke beleidsspecificatie is niet toegestaan.
> 4. Neem contact op met de VPN-leverancier apparaatspecificaties om te controleren of dat het beleid wordt ondersteund op uw on-premises VPN-apparaten. S2S of VNet-naar-VNet-verbindingen kunnen niet tot stand brengen als het beleid niet compatibel zijn.

## <a name ="workflow"></a>Deel 1: werkstroom voor het maken en IPsec/IKE-beleid instellen
Deze sectie geeft een overzicht van de werkstroom voor het maken en IPsec/IKE-beleid op een S2S VPN- of VNet-naar-VNet-verbinding bijwerken:
1. Een virtueel netwerk en een VPN-gateway maken
2. Maak een lokale netwerkgateway voor cross-premises-verbinding of een ander virtueel netwerk en de gateway voor VNet-naar-VNet-verbinding
3. Een beleid voor IPsec/IKE maken met geselecteerde algoritmen en parameters
4. Maak een verbinding (IPSec- of VNet2VNet) met het beleid voor IPsec/IKE
5. Toevoegen/bijwerken/verwijderen uit een IPsec/IKE-beleid voor een bestaande verbinding

De instructies in dit artikel helpt u bij het instellen en configureren van IPsec/IKE-beleid, zoals wordt weergegeven in het diagram:

![ipsec-ike-policy](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>Deel 2 - ondersteund cryptografische algoritmen en kracht van

De volgende tabel bevat de ondersteunde cryptografische algoritmen en kracht kunnen worden geconfigureerd door de klanten:

| **IPsec/IKEv2**  | **Opties**    |
| ---  | --- 
| IKEv2-versleuteling | AES256, AES192, AES128, DES3, DES  
| IKEv2-integriteit  | SHA384, SHA256, SHA1, MD5  |
| DH-groep         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None |
| IPsec-versleuteling | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, geen    |
| IPsec-integriteit  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS-groep        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, geen 
| QM SA-levensduur   | (**Optioneel**: standaardwaarden worden gebruikt als dat niet het opgegeven)<br>Seconden (geheel getal; **min. 300** /standaard 27000 seconden)<br>KB (geheel getal; **min. 1024**/standaard 102400000 KB)   |
| Verkeersselector | UsePolicyBasedTrafficSelectors ** ($True/$False; **Optioneel**, standaard $False als u niets opgeeft)    |
|  |  |

> [!IMPORTANT]
> 1. **De configuratie van uw lokale VPN-apparaat moet overeenkomen met of bevatten de volgende algoritmen en parameters die u op het Azure IPsec/IKE-beleid opgeeft:**
>    * IKE-versleutelingsalgoritme (hoofdmodus / fase 1)
>    * IKE-hash-algoritme (hoofdmodus / fase 1)
>    * DH-groep (hoofdmodus / fase 1)
>    * IPsec-versleutelingsalgoritme (snelle modus / fase 2)
>    * IPSec-hash-algoritme (snelle modus / fase 2)
>    * PFS-groep (in de snelle modus / fase 2)
>    * Selector-verkeer (als UsePolicyBasedTrafficSelectors wordt gebruikt)
>    * De SA-levensduren zijn alleen lokale specificaties en hoeven niet overeen te komen.
>
> 2. **Als GCMAES als voor IPsec-versleutelingsalgoritme wordt gebruikt, moet u de dezelfde GCMAES algoritme en de lengte van de sleutel selecteren voor IPSec-integriteit; bijvoorbeeld, met behulp van GCMAES128 voor beide**
> 3. In de bovenstaande tabel:
>    * IKEv2 komt overeen met de hoofdmodus of fase 1
>    * IPsec komt overeen met de snelle modus of fase 2
>    * DH-groep bepaalt welke Diffie-Hellmen-groep in de hoofdmodus of fase 1 gebruikt
>    * PFS-groep opgegeven de Diffie-Hellmen-groep in de snelle modus of fase 2 gebruikt
> 4. SA-levensduur voor IKEv2 Main Mode staat vastgesteld op 28.800 seconden op de Azure VPN-gateways
> 5. 'UsePolicyBasedTrafficSelectors' op $True instellen op een verbinding, wordt de Azure VPN-gateway verbinding maken met op beleid gebaseerde VPN-firewall on-premises configureren. Als u PolicyBasedTrafficSelectors inschakelt, moet u ervoor zorgen dat uw VPN-apparaat heeft de overeenkomende verkeer selectoren gedefinieerd met behulp van alle combinaties van uw on-premises voorvoegsels (lokale netwerkgateway) van de voorvoegsels virtuele Azure-netwerk in plaats van het netwerk any-to-any. Als uw lokale netwerkvoorvoegsels bijvoorbeeld 10.1.0.0/16 en 10.2.0.0/16 zijn, en de voorvoegsels van uw virtuele netwerk 192.168.0.0/16 en 172.16.0.0/16, moet u de volgende verkeersselectoren opgeven:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

Zie voor meer informatie over op beleid gebaseerde verkeer selectoren [verbinding maken met meerdere on-premises op beleid gebaseerde VPN-apparaten](vpn-gateway-connect-multiple-policybased-rm-ps.md).

De volgende tabel worden de bijbehorende Diffie-Hellman-groepen wordt ondersteund door het aangepaste beleid:

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

In deze sectie leidt u door de stappen voor het maken van een S2S VPN-verbinding met een IPsec/IKE-beleid. De volgende stappen maakt de verbinding zoals weergegeven in het diagram:

![s2s-beleid](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Zie [een S2S VPN-verbinding](vpn-gateway-create-site-to-site-rm-powershell.md) voor stapsgewijze instructies gedetailleerde voor het maken van een S2S VPN-verbinding.

### <a name="before"></a>Voordat u begint

* Controleer of u een Azure-abonnement hebt. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).
* Installeer de Azure Resource Manager PowerShell-cmdlets. Zie [overzicht van Azure PowerShell](/powershell/azure/overview) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="createvnet1"></a>Stap 1: het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

#### <a name="1-declare-your-variables"></a>1. De variabelen declareren

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Verbinding maken met uw abonnement en een nieuwe resourcegroep maken

Zorg ervoor dat u overschakelt naar de PowerShell-modus als u de Resource Manager-cmdlets wilt gebruiken. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

Het volgende voorbeeld maakt het virtuele netwerk, TestVNet1 met drie subnetten en de VPN-gateway. Wanneer u de waarden vervangt, is het belangrijk dat u de juiste namen voor de gatewaysubnets gebruikt, in het bijzonder GatewaySubnet. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="s2sconnection"></a>Stap 2: een S2S VPN-verbinding maken met een IPsec/IKE-beleid

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

Het volgende voorbeeldscript wordt een beleid voor IPsec/IKE gemaakt met de volgende algoritmen en parameters:

* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA256, PFS None, SA Lifetime 14400 seconds & 102400000KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

Als u GCMAES voor IPSec-authenticatie gebruikt, moet u de dezelfde GCMAES algoritme en de lengte van de sleutel voor de IPsec-codering en integriteit. Bijvoorbeeld, de overeenkomstige parameters worden '-IpsecEncryption GCMAES256 - IpsecIntegrity GCMAES256 ' wanneer u GCMAES256.

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a>2. De S2S VPN-verbinding maken met het beleid voor IPsec/IKE

Maak een S2S VPN-verbinding en het eerder gemaakte IPsec/IKE-beleid toepassen.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

U kunt optioneel toevoegen '-UsePolicyBasedTrafficSelectors $True ' aan de cmdlet van de verbinding maken inschakelen Azure VPN-gateway verbinding maken met op beleid gebaseerde VPN-apparaten on-premises, zoals hierboven is beschreven.

> [!IMPORTANT]
> Als een beleid voor IPsec/IKE op een verbinding is opgegeven, wordt de Azure VPN-gateway alleen verzenden of de voorstel IPsec/IKE met opgegeven cryptografische algoritmen en de belangrijkste sterkte op die bepaalde verbinding accepteren. Controleer of uw on-premises VPN-apparaat voor de verbinding wordt gebruikt of accepteert de combinatie exacte beleid anders de S2S VPN-tunnel niet tot stand brengen.


## <a name ="vnet2vnet"></a>Deel 4: een nieuwe VNet-naar-VNet-verbinding maken met IPsec/IKE-beleid

De stappen voor het maken van een VNet-naar-VNet-verbinding met een beleid voor IPsec/IKE zijn vergelijkbaar met die van een S2S VPN-verbinding. De volgende voorbeeldscripts maken de verbinding, zoals weergegeven in het diagram:

![V2V-beleid](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Zie [Maak een VNet-naar-VNet-verbinding](vpn-gateway-vnet-vnet-rm-ps.md) voor stappen gedetailleerde voor het maken van een VNet-naar-VNet-verbinding. U moet voltooien [deel 3](#crossprem) naar TestVNet1 en de VPN-Gateway maken en configureren.

### <a name="createvnet2"></a>Stap 1: het tweede virtuele netwerk en de VPN-gateway maken

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

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a>2. Het tweede virtuele netwerk en de VPN-gateway in de nieuwe resourcegroep maken

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a>Stap 2: een VNet-toVNet verbinding maken met het beleid voor IPsec/IKE

Vergelijkbaar met de S2S VPN-verbinding, een IPsec/IKE-beleid maken en toepassen op beleid naar de nieuwe verbinding.

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

Het volgende voorbeeldscript wordt een ander beleid voor IPsec/IKE gemaakt met de volgende algoritmen en parameters:
* IKEv2: AES128, SHA1, DHGroup14
* IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 14400 seconds & 102400000KB

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a>2. VNet-naar-VNet-verbindingen maken met het beleid voor IPsec/IKE

Een VNet-naar-VNet-verbinding maken en toepassen van de IPsec/IKE-beleid dat u hebt gemaakt. In dit voorbeeld zijn beide gateways in hetzelfde abonnement. Het is daarom mogelijk te maken en configureren van beide verbindingen met hetzelfde IPsec/IKE-beleid in dezelfde PowerShell-sessie.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Als een beleid voor IPsec/IKE op een verbinding is opgegeven, wordt de Azure VPN-gateway alleen verzenden of de voorstel IPsec/IKE met opgegeven cryptografische algoritmen en de belangrijkste sterkte op die bepaalde verbinding accepteren. Zorg ervoor dat de IPSec-beleid voor beide verbindingen zijn hetzelfde, anders de VNet-naar-VNet-verbinding niet tot stand brengen.

De verbinding is gemaakt in een paar minuten en hebt u de volgende netwerktopologie zoals weergegeven in het begin na het voltooien van deze stappen:

![ipsec-ike-policy](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>Deel 5 - Update IPsec/IKE-beleid voor een verbinding

De laatste sectie leest u hoe voor het beheren van IPsec/IKE-beleid voor een bestaande S2S of VNet-naar-VNet-verbinding. De oefening hieronder wordt u begeleid bij de volgende bewerkingen uit op een verbinding:

1. Het beleid voor IPsec/IKE van een verbinding weergeven
2. Toevoegen of bijwerken van het beleid voor IPsec/IKE naar een verbinding
3. Het beleid voor IPsec/IKE verwijderen uit een verbinding

Dezelfde stappen van toepassing op S2S- en VNet-naar-VNet-verbindingen.

> [!IMPORTANT]
> IPsec/IKE-beleid wordt ondersteund op *standaard* en *HighPerformance* op route gebaseerde VPN-gateways alleen. Deze werkt niet op de standaard gateway-SKU of de op beleid gebaseerde VPN-gateway.

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Het beleid voor IPsec/IKE van een verbinding weergeven

Het volgende voorbeeld laat zien hoe om de IPsec/IKE-beleid dat is geconfigureerd op een verbinding te krijgen. De scripts blijven ook uit de bovenstaande oefeningen.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

De laatste opdracht geeft het huidige IPsec/IKE-beleid dat is geconfigureerd op de verbinding indien deze aanwezig is. Hier volgt een voorbeeld van uitvoer voor de verbinding:

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

Als er geen beleid voor IPsec/IKE geconfigureerd, de opdracht (PS > $connection6.policy) een leeg retourtype opgehaald. Dit betekent niet dat IPsec/IKE is niet geconfigureerd op de verbinding, maar er is geen aangepaste IPsec/IKE-beleid. De huidige verbinding maakt gebruik van het standaardbeleid onderhandeld tussen uw on-premises VPN-apparaat en de Azure VPN-gateway.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Toevoegen of bijwerken van een beleid voor IPsec/IKE voor een verbinding

De stappen voor het toevoegen van een nieuw beleid of een bestaand beleid op een verbinding bijwerken zijn hetzelfde: een nieuw beleid maken en vervolgens het nieuwe beleid toepassen op de verbinding.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

Toevoegen om te schakelen "UsePolicyBasedTrafficSelectors" bij het verbinden met een on-premises op beleid gebaseerde VPN-apparaat, de '-UsePolicyBaseTrafficSelectors ' parameter aan de cmdlet, of stel deze in op $False de optie uitschakelen:

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

U kunt de verbinding opnieuw om te controleren als het beleid wordt bijgewerkt.

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
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

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Een beleid voor IPsec/IKE verwijderen uit een verbinding

Wanneer u het aangepaste beleid van een verbinding verwijdert, wordt de Azure VPN-gateway teruggedraaid naar de [standaardlijst met IPsec/IKE voorstellen](vpn-gateway-about-vpn-devices.md) en start een opnieuw nieuwe onderhandeling met uw on-premises VPN-apparaat.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

U kunt hetzelfde script gebruiken om te controleren als het beleid van de verbinding is verwijderd.

## <a name="next-steps"></a>Volgende stappen

Zie [verbinding maken met meerdere on-premises op beleid gebaseerde VPN-apparaten](vpn-gateway-connect-multiple-policybased-rm-ps.md) voor meer informatie over op beleid gebaseerde verkeer selectoren.

Wanneer de verbinding is voltooid, kunt u virtuele machines aan uw virtuele netwerken toevoegen. Zie [Een virtuele machine maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de stappen.