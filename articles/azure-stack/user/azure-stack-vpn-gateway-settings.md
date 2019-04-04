---
title: VPN-gateway-instellingen voor Azure Stack | Microsoft Docs
description: Meer informatie over instellingen voor VPN-gateways die u met Azure Stack gebruikt.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 1ff5aeddbf05011f7c7d105e6c48552bca81580c
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483280"
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>VPN-gatewayconfiguratie-instellingen voor Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Een VPN-gateway is een type virtuele netwerkgateway waarmee versleuteld verkeer tussen het virtuele netwerk in Azure Stack en een externe VPN-gateway wordt verzonden. De externe VPN-gateway kan zijn in Azure en een apparaat in uw datacenter of een apparaat op een andere site. Als er verbinding met het netwerk tussen de twee eindpunten, kunt u een veilige Site-naar-Site (S2S) VPN-verbinding tussen de twee netwerken kunt maken.

Een VPN-gatewayverbinding is afhankelijk van de configuratie van meerdere resources, die elk configureerbare instellingen bevat. Dit artikel beschrijft de resources en de instellingen die betrekking hebben op een VPN-gateway voor een virtueel netwerk die u in het Resource Manager-implementatiemodel maakt. U vindt beschrijvingen en topologiediagrammen voor elke oplossing voor verbinding [over VPN-Gateway voor Azure Stack](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>Instellingen voor VPN gateway

### <a name="gateway-types"></a>Gatewaytypen

Elk virtueel netwerk van Azure Stack biedt ondersteuning voor een enkele virtuele netwerkgateway, moet van het type **Vpn**.  Deze ondersteuning verschilt van Azure, die ondersteuning biedt voor andere typen.  

Wanneer u een virtuele netwerkgateway maakt, moet u ervoor zorgen dat de gateway van het type voor de configuratie juist is. Een VPN-gateway vereist de `-GatewayType Vpn`vlag; bijvoorbeeld:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Gateway-SKU's

Wanneer u een virtuele netwerkgateway maakt, moet u de gateway-SKU die u wilt gebruiken. Selecteer de SKU's die aan uw vereisten voldoen op basis van de typen werkbelasting, doorvoer, functies en SLA's.

Azure Stack biedt de VPN-gateway SKU's die in de volgende tabel worden weergegeven.

|   | VPN-gateway-doorvoer |Maximum aantal IPsec-tunnels VPN-gateway |
|-------|-------|-------|
|**Basis-SKU**  | 100 Mbps  | 20    |
|**Standaard-SKU**           | 100 Mbps  | 20    |
|**High Performance SKU** | 200 Mbps    | 10    |

### <a name="resizing-gateway-skus"></a>Gateway-SKU's vergroten of verkleinen

Azure Stack biedt geen ondersteuning voor een grootte van SKU's tussen de ondersteunde verouderde SKU's.

Azure Stack biedt ook geen ondersteuning voor een grootte van een ondersteunde verouderde SKU (Basic, Standard en HighPerformance) naar een nieuwere SKU wordt ondersteund door Azure (VpnGw1, VpnGw2 en VpnGw3.)

### <a name="configure-the-gateway-sku"></a>Configureren van de gateway-SKU

#### <a name="azure-stack-portal"></a>Azure Stack-portal

Als u de Azure Stack-portal gebruiken om u te maken van een virtuele netwerkgateway van Resource Manager, kunt u de gateway-SKU selecteren met behulp van de vervolgkeuzelijst. De opties komen overeen met de gateway van het type en de VPN-type dat u selecteert.

#### <a name="powershell"></a>PowerShell

Hiermee geeft u op de volgende PowerShell-voorbeeld de **- GatewaySku** als `VpnGw1`:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```

### <a name="connection-types"></a>Verbindingstypen

Elke configuratie vereist in het Resource Manager-implementatiemodel, een specifieke virtuele-gateway-verbindingstype. De beschikbare Resource Manager PowerShell-waarden voor **- ConnectionType** zijn:

* IPsec

   In het volgende PowerShell-voorbeeld wordt een S2S-verbinding gemaakt die het verbindingstype IPsec vereist:

   ```powershell
   New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
   -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```

### <a name="vpn-types"></a>VPN-typen

Wanneer u de virtuele netwerkgateway voor de configuratie van een VPN-gateway maakt, moet u een VPN-type opgeven. De VPN-type dat u kiest, is afhankelijk van de verbindingstopologie die u wilt maken. Een VPN-type kan ook afhankelijk van de hardware die u gebruikt. S2S-configuraties vereisen een VPN-apparaat. Sommige VPN-apparaten ondersteunen alleen een bepaalde VPN-type.

> [!IMPORTANT]  
> Azure Stack ondersteunt momenteel alleen het Route gebaseerd VPN-type. Als uw apparaat biedt alleen ondersteuning voor beleid op basis van VPN-verbindingen, worden niet klikt u vervolgens verbindingen met die apparaten vanuit Azure Stack ondersteund.  
>
> Bovendien biedt Azure Stack geen ondersteuning met behulp van beleid gebaseerde Verkeerkiezers voor Route gebaseerde Gateways op dit moment, omdat aangepaste configuraties voor IPSec/IKE-beleid worden niet ondersteund.

* **PolicyBased**: Op beleid gebaseerde VPN-verbindingen versleutelen pakketten en sturen via IPsec-tunnels op basis van de IPsec-beleidsregels die zijn geconfigureerd met de combinaties van adresvoorvoegsels tussen uw on-premises netwerk en het Azure Stack-VNet. Het beleid of de verkeersselector, is doorgaans een toegangslijst in de configuratie van de VPN-apparaat.

  >[!NOTE]
  >**PolicyBased** wordt ondersteund in Azure, maar niet in Azure Stack.

* **RouteBased**: Op route gebaseerd VPN-verbindingen routes die zijn geconfigureerd in de doorsturen via IP of routeringstabel om direct pakketten naar de bijbehorende tunnelinterfaces te gebruiken. De tunnelinterfaces versleutelen of ontsleutelen de pakketten vervolgens naar en vanuit de tunnels. Het beleid of de verkeersselector, voor **RouteBased** VPN-verbindingen zijn geconfigureerd als alles-naar-alles (of jokertekens gebruiken.) Standaard kunnen niet ze worden gewijzigd. De waarde voor een **RouteBased** VPN-type is **RouteBased**.

Hiermee geeft u op de volgende PowerShell-voorbeeld de **- VpnType** als **RouteBased**. Wanneer u een gateway maakt, moet u ervoor zorgen dat de **- VpnType** juist is voor uw configuratie.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```

### <a name="gateway-requirements"></a>Gatewayvereisten

De volgende tabel bevat de vereisten voor VPN-gateways.

| |Op beleid gebaseerde Basic VPN-Gateway | Basic op route gebaseerd VPN-Gateway | Standaard op route gebaseerd VPN-Gateway | RouteBased High Performance VPN-Gateway|
|--|--|--|--|--|
| **Site-naar-Site-connectiviteit (S2S-connectiviteit)** | Niet ondersteund | RouteBased VPN-configuratie | RouteBased VPN-configuratie | RouteBased VPN-configuratie |
| **Verificatiemethode**  | Niet ondersteund | Vooraf gedeelde sleutel voor S2S-connectiviteit  | Vooraf gedeelde sleutel voor S2S-connectiviteit  | Vooraf gedeelde sleutel voor S2S-connectiviteit  |   
| **Maximumaantal S2S-verbindingen**  | Niet ondersteund | 20 | 20| 10|
|**Ondersteuning voor actieve routering (BGP)** | Niet ondersteund | Niet ondersteund | Ondersteund | Ondersteund |

### <a name="gateway-subnet"></a>Gatewaysubnet

Voordat u een VPN-gateway maakt, moet u een gatewaysubnet maken. Het gatewaysubnet bevat de IP-adressen die gebruikmaken van de virtuele netwerkgateway virtuele machines en services. Wanneer u uw virtuele netwerkgateway maakt, zijn de gateway-VM's in het gatewaysubnet geïmplementeerd en geconfigureerd met de vereiste instellingen voor VPN-gateway. Implementeer iets anders (bijvoorbeeld extra VM's) niet in het gatewaysubnet.

>[!IMPORTANT]
>Voor een goede werking moet het gatewaysubnet de naam **GatewaySubnet** krijgen. Deze naam wordt gebruikt voor het identificeren van het subnet voor het implementeren van de virtuele netwerkgateway virtuele machines en services van Azure Stack.

Wanneer u het gatewaysubnet maakt, geeft u op hoeveel IP-adressen het subnet bevat. De IP-adressen in het gatewaysubnet worden toegewezen aan de gateway-VM's en de gateway-services. Sommige configuraties vereisen meer IP-adressen dan andere. Bekijk de instructies voor de configuratie die u wilt maken en te controleren of het gatewaysubnet dat u wilt maken, voldoet aan deze vereisten.

Bovendien moet u ervoor zorgen dat uw gatewaysubnet voldoende IP-adressen voor het afhandelen van aanvullende toekomstige configuraties heeft. Hoewel kunt u een gatewaysubnet zo klein is als/29, raden wij aan dat u maakt een gatewaysubnet van/28 of groter (/ 28, / 27, / 26, enzovoort.) Op die manier als u functionaliteit in de toekomst toevoegen niet hebt u uw gateway verbreken en vervolgens verwijderen en opnieuw maken van het gatewaysubnet om toe te staan voor meer IP-adressen.

De volgende Resource Manager PowerShell-voorbeeld ziet u een gateway-subnet met de naam **GatewaySubnet**. U ziet dat de CIDR-notatie een/27, geeft dit biedt voldoende IP-adressen voor de meeste configuraties die momenteel aanwezig zijn.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Als u met gatewaysubnetten werkt, vermijd dan om een netwerkbeveiligingsgroep (NSG) te koppelen aan het gatewaysubnet. Koppelen van een netwerkbeveiligingsgroep naar dit subnet kan leiden tot uw VPN-gateway niet meer werkt zoals verwacht. Zie voor meer informatie over netwerkbeveiligingsgroepen [wat is er een netwerkbeveiligingsgroep?](../../virtual-network/virtual-networks-nsg.md).

### <a name="local-network-gateways"></a>Lokale netwerkgateways

Bij het maken van de configuratie van een VPN-gateway in Azure, wordt in de lokale netwerkgateway vaak uw on-premises locatie vertegenwoordigt. In Azure Stack staat voor een externe VPN-apparaat bevindt zich buiten Azure Stack. Dit wordt mogelijk een VPN-apparaat in uw datacenter (of een extern datacenter), of een VPN-Gateway in Azure.

U geeft de lokale netwerkgateway een naam, het openbare IP-adres van de VPN-apparaat en de adresvoorvoegsels die zich op de on-premises locatie opgeven. Azure kijkt naar de bestemmingsadressen voor netwerkverkeer, raadpleegt de configuratie die u voor uw lokale netwerkgateway hebt opgegeven en routeert pakketten dienovereenkomstig.

Het volgende PowerShell-voorbeeld maakt u een nieuwe lokale netwerkgateway:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Soms moet u de gateway-instellingen van het lokale netwerk; wijzigen bijvoorbeeld, wanneer u toevoegen of wijzigen van het adresbereik, of als het IP-adres van de VPN-apparaat is gewijzigd. Zie [lokale instellingen netwerkgateway wijzigen met behulp van PowerShell](../../vpn-gateway/vpn-gateway-modify-local-network-gateway.md).

## <a name="ipsecike-parameters"></a>IPsec/IKE-parameters

Bij het instellen van een VPN-verbinding in Azure Stack, moet u de verbinding aan beide uiteinden configureren. Als u een VPN-verbinding tussen Azure Stack en een hardwareapparaat zoals een switch of router die als een VPN-gateway fungeert configureert, dat apparaat mogelijk vragen u om aanvullende instellingen.

In tegenstelling tot Azure, die ondersteuning biedt voor meerdere aanbiedingen als zowel een begin- en een eindpunt, Azure Stack biedt ondersteuning voor slechts één aanbieding.

### <a name="ike-phase-1-main-mode-parameters"></a>Parameters voor IKE Phase 1 (Main Mode)

| Eigenschap              | Value|
|-|-|
| IKE-versie           | IKEv2 |
|Diffie-Hellman-groep   | Groep 2 (1024 bits) |
| Verificatiemethode | Vooraf gedeelde sleutel |
|Versleutelings- en hash-algoritmen | AES256, SHA256 |
|SA-levensduur (tijd)     | 28.800 seconden|

### <a name="ike-phase-2-quick-mode-parameters"></a>Parameters voor IKE Phase 2 (Quick Mode)

| Eigenschap| Value|
|-|-|
|IKE-versie |IKEv2 |
|Versleuteling en hash-algoritmen (codering)     | GCMAES256|
|Versleuteling en hash-algoritmen (verificatie) | GCMAES256|
|SA-levensduur (tijd)  | 27.000 seconden  |
|SA-levensduur (kB) | 33,553,408     |
|Perfect Forward Secrecy (PFS) |Geen<sup>Zie Opmerking 1</sup> |
|Dead Peer Detection | Ondersteund|  

* *Opmerking 1:*  Voorafgaand aan versie 1807, Azure Stack maakt gebruik van een waarde van PFS2048 voor de Perfect Forward Secrecy (PFS).

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met behulp van ExpressRoute](../azure-stack-connect-expressroute.md)
