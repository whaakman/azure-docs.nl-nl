---
title: VPN-gateway-instellingen voor Azure-Stack | Microsoft Docs
description: Meer informatie over instellingen voor VPN-gateways die u met Azure-Stack gebruiken.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2018
ms.author: brenduns
ms.openlocfilehash: 1eba5df93b461eb22ab8341b4498682957c9298a
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>VPN-gateway configuratie-instellingen voor Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een VPN-gateway is een type van de virtuele netwerkgateway die versleuteld verkeer tussen uw virtuele netwerk in Azure-Stack en een externe VPN-gateway verzendt. De externe VPN-gateway kan zich in Azure, een apparaat in uw datacenter of een apparaat op een andere site.  Als er een netwerkverbinding tussen de twee eindpunten is, kunt u een beveiligde Site-naar-Site (S2S) VPN-verbinding tussen de twee netwerken maken.

Er is een VPN-gatewayverbinding afhankelijk van de configuratie van meerdere resources, die elk configureerbare instellingen bevat. De secties in dit artikel worden de resources en de instellingen met betrekking tot een VPN-gateway voor een virtueel netwerk gemaakt in Resource Manager-implementatiemodel. U vindt beschrijvingen en diagrammen topologie voor elke verbinding oplossing in [over VPN-Gateway voor Azure-Stack](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>Instellingen voor VPN-gateway

### <a name="gateway-types"></a>Gatewaytypen
Elke virtuele Azure-Stack-netwerk ondersteunt een enkele virtuele netwerkgateway, moet van het type **Vpn**.  Deze ondersteuning verschilt van Azure die ondersteuning biedt voor extra typen.  

Wanneer u een virtuele netwerkgateway maakt, moet u ervoor zorgen dat het Gatewaytype voor uw configuratie klopt. Een VPN-gateway vereist de `-GatewayType Vpn`.

Voorbeeld:
```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Gateway-SKU's
Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Selecteer de SKU's die aan uw vereisten voldoen op basis van de typen werkbelasting, doorvoer, functies en SLA's.

>[!NOTE]
> Voor klassieke virtuele netwerken moeten de oude SKU's nog altijd worden gebruikt. Zie [Werken met virtuele-netwerkgateway-SKU's (oud)](/azure/vpn-gateway/vpn-gateway-about-skus-legacy) voor meer informatie over de oude gateway-SKU's.

Azure-Stack biedt de volgende VPN-gateway SKU's:

|   | Doorvoer VPN-Gateway |Max. IPsec-tunnels VPN-Gateway |
|-------|-------|-------|
|**Basis-SKU**  | 100 Mbps  | 10    |
|**Standaard SKU**           | 100 Mbps  | 10    |
|**Hoge prestaties SKU** | 200 Mbps    | 30    |

### <a name="resizing-gateway-skus"></a>Gateway-SKU's vergroten of verkleinen
Azure-Stack biedt geen ondersteuning voor een formaat van SKU's tussen de ondersteunde oude SKU's.

Op deze manier biedt Azure Stack geen ondersteuning voor een Wijzig het formaat van een ondersteunde verouderde SKU's (Basic, Standard en HighPerformance), de nieuwere SKU's ondersteund door Azure (VpnGw1, VpnGw2 en VpnGw3).

### <a name="configure-the-gateway-sku"></a>De gateway-SKU configureren
#### <a name="azure-stack-portal"></a>Azure Stack-portal
Als u de Stack van Azure-portal voor het maken van een virtuele netwerkgateway van Resource Manager gebruikt, selecteert u de gateway-SKU met behulp van de vervolgkeuzelijst. De opties die u krijgt correspondeert met het type van de Gateway en de VPN-type dat u selecteert.

#### <a name="powershell"></a>PowerShell
De volgende PowerShell-voorbeeld geeft de GatewaySku - als VpnGw1.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```
### <a name="connection-types"></a>Verbindingstypen
Elke configuratie vereist in het Resource Manager-implementatiemodel, het type van een specifieke virtuele-netwerkgateway verbinding. De beschikbare Resource Manager PowerShell-waarden voor - ConnectionType zijn:

- IPsec

In het volgende PowerShell-voorbeeld wordt een S2S-verbinding gemaakt die het verbindingstype IPsec vereist.  

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>VPN-typen
Wanneer u de virtuele netwerkgateway voor de configuratie van een VPN-gateway maakt, moet u een VPN-type. Het VPN-type dat u kiest, is afhankelijk van de verbinding-topologie die u wilt maken.  Een VPN-type kan ook afhankelijk van de hardware die u gebruikt. S2S-configuraties vereisen een VPN-apparaat. Sommige VPN-apparaten ondersteunen alleen een bepaalde VPN-type.

> [!IMPORTANT]  
> Op dit moment ondersteunt Azure Stack alleen het Route gebaseerd VPN-type. Als uw apparaat alleen beleid gebaseerd VPN-verbindingen ondersteunt, worden klikt u vervolgens verbindingen met die apparaten uit de Stack Azure niet ondersteund.

- **PolicyBased**: *(ondersteund door Azure, maar niet door de Azure-Stack)* op beleid gebaseerde VPN-verbindingen versleutelen pakketten en sturen via IPsec-tunnels op basis van de IPsec-beleidsregels die zijn geconfigureerd met de combinaties van adresvoorvoegsels tussen uw on-premises netwerk en de Stack Azure VNet. Het beleid (of de verkeersselector) wordt gewoonlijk gedefinieerd als een toegangslijst in de configuratie van het VPN-apparaat.

- **RouteBased**: op route gebaseerd VPN-verbindingen gebruiken 'routes' in de IP-doorstuurtabel of routeringstabel om pakketten direct naar de bijbehorende tunnelinterfaces. De tunnelinterfaces versleutelen of ontsleutelen de pakketten vervolgens naar en vanuit de tunnels. Het beleid (of de verkeersselector) voor op route gebaseerd VPN-verbindingen zijn geconfigureerd als alles-naar-alles (of jokertekens). De waarde voor een RouteBased VPN-type is RouteBased.

De volgende PowerShell-voorbeeld geeft de VpnType - als RouteBased. Wanneer u een gateway maakt, moet u het juiste VPN-type voor uw configuratie kiezen.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```
### <a name="gateway-requirements"></a>Gatewayvereisten
De volgende tabel bevat de vereisten voor VPN-gateways.

| |PolicyBased Basic VPN-Gateway | Basic op route gebaseerd VPN-Gateway | Standaard op route gebaseerd VPN-Gateway | RouteBased High Performance VPN-Gateway|
|--|--|--|--|--|
| **Site-naar-Site-connectiviteit (S2S-verbinding)** | Niet ondersteund | RouteBased VPN-configuratie | RouteBased VPN-configuratie | RouteBased VPN-configuratie |
| **Verificatiemethode**  | Niet ondersteund | Vooraf gedeelde sleutel voor S2S-connectiviteit  | Vooraf gedeelde sleutel voor S2S-connectiviteit  | Vooraf gedeelde sleutel voor S2S-connectiviteit  |   
| **Maximumaantal S2S-verbindingen**  | Niet ondersteund | 10 | 10| 30|
|**Ondersteuning voor actieve routering (BGP)** | Niet ondersteund | Niet ondersteund | Ondersteund | Ondersteund |

### <a name="gateway-subnet"></a>Gatewaysubnet
Voordat u een VPN-gateway maakt, moet u een gatewaysubnet maken. Het gatewaysubnet bevat de IP-adressen die gebruikmaken van de virtuele netwerkgateway virtuele machines en services. Wanneer u uw virtuele netwerkgateway maakt, worden gateway-VM's in het gatewaysubnet is geïmplementeerd en geconfigureerd met de vereiste instellingen voor VPN-gateway. Implementeert geen iets anders (bijvoorbeeld extra VM's) voor het gatewaysubnet. Het gatewaysubnet moet de naam 'GatewaySubnet' goed te laten werken. Naamgeving van het gatewaysubnet kunt 'GatewaySubnet' Azure-Stack het subnet voor het implementeren van de virtuele netwerkgateway virtuele machines en services te identificeren.

Wanneer u het gatewaysubnet maakt, geeft u op hoeveel IP-adressen het subnet bevat. De IP-adressen in het gatewaysubnet worden toegewezen aan de gateway-VM's en gatewayservices. Sommige configuraties vereisen meer IP-adressen dan andere. Bekijk de instructies voor de configuratie die u wilt maken en te controleren of het gatewaysubnet dat u wilt maken, voldoet aan deze vereisten. Bovendien wilt u Zorg ervoor dat het gatewaysubnet bevat voldoende IP-adressen zodat mogelijke toekomstige aanvullende configuraties. U kunt een gatewaysubnet slechts/29 maken, wordt aangeraden dat u een gatewaysubnet van/28 of groter maakt (/ 28, / 27, /26 enz.). Op die manier als u de functionaliteit in de toekomst toevoegt u beschikt niet over uw gateway verwijderen en vervolgens verwijderen en opnieuw maken van het gatewaysubnet om toe te staan voor meer IP-adressen.

De volgende Resource Manager PowerShell-voorbeeld ziet een gatewaysubnet met de naam GatewaySubnet. U ziet dat de CIDR-notatie een/27, geeft dit biedt voldoende IP-adressen voor de meeste configuraties die momenteel aanwezig zijn.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Als u met gatewaysubnetten werkt, vermijd dan om een netwerkbeveiligingsgroep (NSG) te koppelen aan het gatewaysubnet. Een koppeling van een netwerkbeveiligingsgroep naar dit subnet zorgt er mogelijk voor dat uw VPN-gateway niet meer werkt zoals verwacht. Zie voor meer informatie over netwerkbeveiligingsgroepen [wat is er een netwerkbeveiligingsgroep?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Lokale netwerkgateways
Wanneer u de configuratie van een VPN-gateway in Azure, vertegenwoordigt de lokale netwerkgateway vaak uw on-premises locatie. Deze geeft externe VPN-apparaat bevindt zich buiten de Azure-Stack in Azure-Stack.  Dit kan een VPN-apparaat in uw datacenter, een externe datacenter of een VPN-Gateway in Azure zijn.

U geeft de lokale netwerkgateway een naam, het openbare IP-adres van de VPN-apparaat en de adresvoorvoegsels die op de on-premises locatie opgeven. Azure kijkt naar de bestemmingsadressen voor netwerkverkeer, raadpleegt de configuratie die u voor uw lokale netwerkgateway hebt opgegeven en routeert pakketten dienovereenkomstig.

Het volgende PowerShell-voorbeeld wordt een nieuwe lokale netwerkgateway:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```
Soms moet u de lokale gateway-instellingen wijzigen. Bijvoorbeeld, wanneer u toevoegen of wijzigen van het adresbereik, of als het IP-adres van de VPN-apparaat is gewijzigd. Zie [lokale gateway netwerkinstellingen wijzigen met behulp van PowerShell](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>IPsec/IKE-parameters
Wanneer u een VPN-verbinding in Azure-Stack instelt, moet u de verbinding aan beide uiteinden configureren.  Als u een VPN-verbinding tussen Azure-Stack en een apparaat zoals een switch of router die als een VPN-Gateway fungeert, configureert dat apparaat mogelijk vragen u om aanvullende instellingen.

In tegenstelling tot Azure, die ondersteuning biedt voor meerdere aanbiedingen als zowel een begin- en een eindpunt, ondersteunt Azure Stack slechts één aanbieding.

###  <a name="ike-phase-1-main-mode-parameters"></a>Parameters voor IKE Phase 1 (Main Mode)
| Eigenschap              | Waarde|
|-|-|
| IKE-versie           | IKEv2 |
|Diffie-Hellman-groep   | Groep 2 (1024 bits) |
| Verificatiemethode | Vooraf gedeelde sleutel |
|Versleutelings- en hash-algoritmen | AES256, SHA256 |
|SA-levensduur (tijd)     | 28.800 seconden|

### <a name="ike-phase-2-quick-mode-parameters"></a>Parameters voor IKE Phase 2 (Quick Mode)
| Eigenschap| Waarde|
|-|-|
|IKE-versie |IKEv2 |
|Versleuteling en hash-algoritmen (versleuteling)     | GCMAES256|
|Versleuteling en hash-algoritmen (verificatie) | GCMAES256|
|SA-levensduur (tijd)  | 14.400 seconden |
|SA-levensduur (bytes) | 819,200       |
|Perfect Forward Secrecy (PFS) |PFS2048 |
|Dead Peer Detection | Ondersteund|  
