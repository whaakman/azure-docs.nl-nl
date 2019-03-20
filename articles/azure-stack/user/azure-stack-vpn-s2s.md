---
title: Azure Stack-site-naar-site VPN-verbindingen configureren | Microsoft Docs
description: Meer informatie over IPsec/IKE-beleid voor site-naar-site VPN- of VNet-naar-VNet-verbindingen in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2019
ms.author: sethm
ms.lastreviewed: 01/19/2019
ms.openlocfilehash: 07e598d6fd4ed2937d86f31593a220c0c28ba328
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58074655"
---
# <a name="configure-ipsecike-policy-for-site-to-site-vpn-connections"></a>IPsec/IKE-beleid voor site-naar-site VPN-verbindingen configureren

In dit artikel worden de stappen beschreven voor het configureren van een IPsec/IKE-beleid voor site-naar-site (S2S) VPN-verbindingen in Azure Stack.

>[!NOTE]
> U moet beschikken over Azure Stack-build **1809** of later om deze functie te gebruiken.  Als u een build vóór 1809 momenteel worden uitgevoerd, werkt u uw Azure Stack-systeem naar de meest recente build voordat u deze functie te gebruiken of volg de stappen in dit artikel.

## <a name="ipsec-and-ike-policy-parameters-for-vpn-gateways"></a>Parameters voor IPsec en IKE-beleid voor VPN-gateways

De standaard IPsec en IKE-protocol ondersteunt een breed scala aan cryptografische algoritmen in verschillende combinaties. Als u wilt zien welke parameters worden ondersteund in Azure Stack, Zie [IPsec/IKE-parameters](azure-stack-vpn-gateway-settings.md#ipsecike-parameters), die kan helpen voldoen aan uw vereisten voor naleving of beveiliging.

In dit artikel bevat instructies over het maken en een IPsec/IKE-beleid configureren en toepassen op een nieuwe of bestaande verbinding.

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende belangrijke overwegingen bij het gebruik van deze beleidsregels:

- Het IPsec/IKE-beleid werkt alleen op de *Standard* en *HighPerformance* (op een route gebaseerd) gateway-SKU's.

- U kunt alleen opgeven **één** beleidscombinatie voor een bepaalde verbinding.

- U moet alle algoritmen en parameters opgeven voor zowel IKE (Main Mode) en IPsec (Quick Mode). Gedeeltelijke beleidsspecificatie is niet toegestaan.

- Neem contact op met uw VPN-leverancier apparaatspecificaties om te controleren of dat het beleid wordt ondersteund op uw on-premises VPN-apparaten. Site-naar-site-verbindingen kunnen niet worden ingesteld als het beleid niet compatibel zijn.

## <a name="part-1---workflow-to-create-and-set-ipsecike-policy"></a>Deel 1: werkstroom voor het maken en IPsec/IKE-beleid instellen

In deze sectie geeft een overzicht van de werkstroom die is vereist voor het maken en bijwerken van het IPsec/IKE-beleid op een site-naar-site VPN-verbinding:

1. Maak een virtueel netwerk en een VPN-gateway.

2. Maak een lokale netwerkgateway voor cross-premises-verbinding.

3. Maak een IPsec/IKE-beleid met de geselecteerde algoritmen en parameters.

4. Een IPSec-verbinding maken met het IPsec/IKE-beleid.

5. Toevoegen/bijwerken/verwijderen een IPsec/IKE-beleid voor een bestaande verbinding.

De instructies in de help van dit artikel instellen en configureren van IPsec/IKE-beleid, zoals wordt weergegeven in de volgende afbeelding:

![Instellen en IPsec/IKE-beleid configureren](media/azure-stack-vpn-s2s/site-to-site.png)

## <a name="part-2---supported-cryptographic-algorithms-and-key-strengths"></a>Deel 2 - ondersteunde cryptografische algoritmen en belangrijkste sterke punten

De volgende tabel bevat de ondersteunde cryptografische algoritmen en sleutelsterkten die door Azure Stack-klanten:

| IPsec/IKEv2                                          | Opties                                                                  |
|------------------------------------------------------|--------------------------------------------------------------------------|
| IKEv2-versleuteling                                     | AES256, AES192, AES128, DES3, DES                                        |
| IKEv2-integriteit                                      | SHA384, SHA256, SHA1, MD5                                                |
| DH-groep                                             | ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None         |
| IPsec-versleuteling                                     | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, geen |
| IPsec-integriteit                                      | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                       |
| PFS-groep                                            | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, geen                         |
| QM SA-levensduur                                       | (Optioneel: standaardwaarden worden gebruikt als u niets opgeeft)<br />                         Seconden (geheel getal; min. 300/standaard 27000 seconden)<br />                         KB (geheel getal; min. 1024/standaard 102400000 kB) |
| Verkeersselector                                     | Beleid gebaseerde Verkeerkiezers worden niet ondersteund in Azure Stack.         |

- De configuratie van uw on-premises VPN-apparaat moet overeenkomen met of de volgende algoritmen en parameters bevatten die u in het Azure IPsec/IKE-beleid opgeeft:

  - IKE-versleutelingsalgoritme (Main Mode / fase 1)
  - IKE-integriteitsalgoritme (Main Mode / fase 1)
  - DH-groep (hoofdmodus / fase 1)
  - IPsec-versleutelingsalgoritme (Quick Mode / fase 2)
  - IPsec-integriteitsalgoritme (Quick Mode / fase 2)
  - PFS-groep (Quick Mode / fase 2)
  - De SA-levensduren zijn alleen lokale specificaties en hoeven niet overeen te komen.

- Als GCMAES als voor IPsec-versleutelingsalgoritme wordt gebruikt, moet u de hetzelfde GCMAES-algoritme en de sleutellengte voor IPSec-integriteit; selecteren bijvoorbeeld, GCMAES128 gebruikt voor beide.

- In de voorgaande tabel:

  - IKEv2 komt overeen met Main Mode of fase 1
  - IPsec komt overeen met de snelle modus of fase 2
  - DH-groep Hiermee geeft u de Diffie-Hellmen-groep die wordt gebruikt in Main Mode of in fase 1
  - PFS-groep opgegeven de Diffie-Hellmen-groep die wordt gebruikt in snelle modus of in fase 2

- IKEv2 Main Mode SA-levensduur is vastgesteld op 28.800 seconden op de Azure Stack-VPN-gateways.

De volgende tabel bevat de bijbehorende Diffie-Hellman-groepen die door het aangepaste beleid worden ondersteund:

| Diffie-Hellman-groep | DHGroup   | PFSGroup      | Sleutellengte    |
|----------------------|-----------|---------------|---------------|
| 1                    | DHGroup1  | PFS1          | 768-bits MODP  |
| 2                    | DHGroup2  | PFS2          | 1024-bits MODP |
| 14                   | DHGroup14<br/>DHGroup2048 | PFS2048       | 2048-bits MODP |
| 19                   | ECP256    | ECP256        | 256-bits ECP   |
| 20                   | ECP384    | ECP384        | 384-bits ECP   |
| 24                   | DHGroup24 | PFS24         | 2048-bits MODP |

Zie voor meer informatie, [RFC3526](https://tools.ietf.org/html/rfc3526) en [RFC5114](https://tools.ietf.org/html/rfc5114).

## <a name="part-3---create-a-new-site-to-site-vpn-connection-with-ipsecike-policy"></a>Deel 3: een nieuwe site-naar-site VPN-verbinding maken met IPsec/IKE-beleid

In deze sectie doorloopt u de stappen voor het maken van een site-naar-site VPN-verbinding met een IPsec/IKE-beleid. De volgende stappen maakt de verbinding, zoals wordt weergegeven in de volgende afbeelding:

![site-naar-site-policy](media/azure-stack-vpn-s2s/site-to-site.png)

Zie voor meer Stapsgewijze instructies gedetailleerde voor het maken van een site-naar-site VPN-verbinding, [maken van een site-naar-site VPN-verbinding](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).

### <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u de volgende vereisten hebt:

- Een Azure-abonnement. Als u nog een Azure-abonnement hebt, kunt u uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), zich ook aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

- De Azure Resource Manager PowerShell-cmdlets. Zie [PowerShell voor Azure Stack installeren](../azure-stack-powershell-install.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>Stap 1: het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

#### <a name="1-declare-variables"></a>1. Variabelen declareren

Voor deze oefening starten door op te geven van de volgende variabelen. Zorg ervoor dat u de plaatsaanduidingen vervangt door uw eigen waarden wanneer u configureert voor productie:

```powershell
$Sub1 = "\<YourSubscriptionName\>"
$RG1 = "TestPolicyRG1"
$Location1 = "East US 2"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPconf1 = "gw1ipconf1"
$Connection16 = "VNet1toSite6"
$LNGName6 = "Site6"
$LNGPrefix61 = "10.61.0.0/16"
$LNGPrefix62 = "10.62.0.0/16"
$LNGIP6 = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Verbinding maken met uw abonnement en een nieuwe resourcegroep maken

Zorg ervoor dat u overschakelt naar de PowerShell-modus als u de Resource Manager-cmdlets wilt gebruiken. Zie voor meer informatie, [verbinding maken met Azure Stack met PowerShell als een gebruiker](azure-stack-powershell-configure-user.md).

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Het virtuele netwerk, de VPN-gateway en de lokale netwerkgateway maken

Het volgende voorbeeld wordt het virtuele netwerk, **TestVNet1**met drie subnetten, en de VPN-gateway. Bij het vervangen van waarden, is het belangrijk dat u altijd de naam het gatewaysubnet specifiek **GatewaySubnet**. Als u een andere naam kiest, mislukt het maken van de gateway.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1

$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
-VirtualNetwork $vnet1

$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1
-Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
-Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn
-VpnType RouteBased -GatewaySku VpnGw1

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1
-Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix
$LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-site-to-site-vpn-connection-with-an-ipsecike-policy"></a>Stap 2: een site-naar-site VPN-verbinding maken met een IPsec/IKE-beleid

#### <a name="1-create-an-ipsecike-policy"></a>1. Een IPsec/IKE-beleid maken

Met dit voorbeeldscript wordt een IPsec/IKE-beleid met de volgende algoritmen en parameters gemaakt:

- IKEv2: AES128, SHA1, DHGroup14
- IPsec: AES256, SHA256, none, SA-levensduur 14400 seconden en 102400000KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup none -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

Als u GCMAES voor IPsec gebruikt, moet u de hetzelfde GCMAES-algoritme en de lengte van de sleutel voor IPsec-codering en integriteit.

#### <a name="2-create-the-site-to-site-vpn-connection-with-the-ipsecike-policy"></a>2. De site-naar-site VPN-verbinding maken met het IPsec/IKE-beleid

Een site-naar-site VPN-verbinding maken en toepassen van het IPsec/IKE-beleid dat u eerder hebt gemaakt.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'Azs123'
```

> [!IMPORTANT]
> Zodra een IPsec/IKE-beleid op een verbinding is opgegeven, wordt de Azure VPN-gateway alleen verzenden of het voorstel IPsec/IKE met opgegeven cryptografische algoritmen en belangrijkste sterke punten op die bepaalde verbinding accepteren. Zorg ervoor dat uw on-premises VPN-apparaat voor de verbinding wordt gebruikt of accepteert de exacte beleidscombinatie, anders die wordt niet de site-naar-site VPN-tunnel tot stand worden gebracht.

## <a name="part-4---update-ipsecike-policy-for-a-connection"></a>Deel 4 - Update IPsec/IKE-beleid voor een verbinding

De vorige sectie hebt u geleerd hoe u voor het beheren van IPsec/IKE-beleid voor een bestaande site-naar-site-verbinding. De volgende sectie leert u hoe via de volgende bewerkingen uit op een verbinding:

1. Het IPsec/IKE-beleid van een verbinding weergeven
2. Toevoegen of bijwerken van het IPsec/IKE-beleid op een verbinding
3. Het IPsec/IKE-beleid van een verbinding verwijderen

> [!NOTE]
> IPsec/IKE-beleid wordt ondersteund op *Standard* en *HighPerformance* op route gebaseerde VPN-gateways alleen. Werkt niet op de *Basic* gateway-SKU.

### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Het IPsec/IKE-beleid van een verbinding weergeven

Het volgende voorbeeld ziet hoe u aan het IPsec/IKE-beleid dat is geconfigureerd op een verbinding. De scripts blijven ook van de vorige oefeningen:

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

De laatste opdracht geeft een lijst van de huidige IPsec/IKE-beleid dat is geconfigureerd op de verbinding, indien van toepassing. Het volgende voorbeeld wordt een voorbeeld van uitvoer voor de verbinding:

```shell
SALifeTimeSeconds : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption : AES256
IpsecIntegrity : SHA256
IkeEncryption : AES128
IkeIntegrity : SHA1
DhGroup : DHGroup14
PfsGroup : None
```

Als er geen IPsec/IKE-beleid geconfigureerd, de opdracht `$connection6.policy` een lege geretourneerde opgehaald. Dit betekent niet dat IPsec/IKE niet is geconfigureerd op de verbinding. Dit betekent dat er geen aangepast IPsec/IKE-beleid. Het standaardbeleid onderhandeld tussen uw on-premises VPN-apparaat en de Azure VPN-gateway maakt gebruik van de huidige verbinding.

### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Toevoegen of bijwerken van een IPsec/IKE-beleid voor een verbinding

De stappen om een nieuw beleid toevoegen of bijwerken van een bestaand beleid op een verbinding zijn hetzelfde: een nieuw beleid maken en vervolgens het nieuwe beleid toepassen op de verbinding.

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1

$newpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

$connection6.SharedKey = "AzS123"

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

De verbinding opnieuw om te controleren of het beleid is bijgewerkt, kunt u krijgen:

```powershell
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

U ziet de uitvoer van de laatste regel, zoals wordt weergegeven in het volgende voorbeeld:

```shell
SALifeTimeSeconds : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption : AES256
IpsecIntegrity : SHA256
IkeEncryption : AES128
IkeIntegrity : SHA1
DhGroup : DHGroup14
PfsGroup : None
```

### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Een IPsec/IKE-beleid van een verbinding verwijderen

Wanneer u het aangepaste beleid van een verbinding verwijdert, wordt de Azure VPN-gateway teruggezet naar de [standaard IPsec/IKE voorstel](azure-stack-vpn-gateway-settings.md#ipsecike-parameters), en start een nieuwe onderhandeling met uw on-premises VPN-apparaat.

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1
$connection6.SharedKey = “AzS123”
$currentpolicy = $connection6.IpsecPolicies\[0\]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

U kunt hetzelfde script gebruiken om te controleren als het beleid is verwijderd uit de verbinding.

## <a name="next-steps"></a>Volgende stappen

- [VPN-gatewayconfiguratie-instellingen voor Azure Stack](azure-stack-vpn-gateway-settings.md)
