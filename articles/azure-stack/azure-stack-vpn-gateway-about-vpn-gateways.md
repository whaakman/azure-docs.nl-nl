---
title: Over VPN-gateway voor Azure Stack | Microsoft Docs
description: Meer informatie over en configureren van VPN-gateways die u met Azure Stack gebruikt.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/02/2018
ms.author: brenduns
ms.openlocfilehash: ed405a0f8892b278d81f2494ad90f776ad5faa41
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857088"
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Over VPN-gateway voor Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Voordat u netwerkverkeer tussen uw Azure-netwerk en uw on-premises site verzendt kunt, moet u een virtuele netwerkgateway maken voor het virtuele netwerk.

Een VPN-gateway is een soort gateway voor virtuele netwerken die versleuteld verkeer verzendt via een openbare verbinding. U kunt VPN-gateways gebruiken om verkeer te verzenden veilig tussen een virtueel netwerk in Azure Stack en een virtueel netwerk in Azure. U kunt ook verkeer veilig tussen een virtueel netwerk en een ander netwerk die is verbonden met een VPN-apparaat verzenden.

Wanneer u een virtuele netwerkgateway maakt, geeft u het gatewaytype aan dat u wilt maken. Azure Stack biedt ondersteuning voor één type virtuele netwerkgateway, de **Vpn** type.

Elk virtueel netwerk kan twee virtuele netwerkgateways hebben, maar slechts één van elk type. Afhankelijk van de instellingen die u kiest, kunt u meerdere verbindingen maken voor één VPN-gateway. Een voorbeeld is een configuratie met meerdere Site-verbinding.

Voordat u maken en configureren van VPN-Gateways voor Azure Stack, controleert u de [overwegingen voor Azure Stack-netwerken](/articles/azure-stack/user/azure-stack-network-differences.md) voor meer informatie over hoe configuraties voor Azure Stack verschillen van Azure.

>[!NOTE]
>In Azure, moet de doorvoer van de bandbreedte voor VPN-gateway-SKU die u kiest, worden verdeeld over alle verbindingen die zijn verbonden met de gateway. Maar in Azure Stack, de waarde voor de bandbreedte voor de VPN-gateway-SKU wordt toegepast op elke verbinding-resource die is verbonden met de gateway.
>
> Bijvoorbeeld:
> * De Basic VPN-Gateway-SKU aankan in Azure, ongeveer 100 Mbps van geaggregeerde doorvoer. Als u twee verbindingen met deze VPN-Gateway maken en één verbinding 50 Mbps bandbreedte wordt gebruikt, klikt u vervolgens 50 Mbps is beschikbaar voor de andere verbinding.
> * In Azure Stack, *elke* verbinding met de Basic VPN-Gateway-SKU wordt het toegewezen 100 Mbps aan doorvoer.

## <a name="configuring-a-vpn-gateway"></a>Een VPN-gateway configureren

Een VPN-gatewayverbinding is afhankelijk van verschillende resources die zijn geconfigureerd met specifieke instellingen. De meeste van deze resources kunnen afzonderlijk worden geconfigureerd, maar in sommige gevallen deze in een bepaalde volgorde moeten worden geconfigureerd.

### <a name="settings"></a>Instellingen

De instellingen die u voor elke resource hebt gekozen, zijn essentieel voor het maken van verbinding is geslaagd.

Zie voor meer informatie over afzonderlijke resources en -instellingen voor een VPN-gateway [over VPN gateway-instellingen voor Azure Stack](azure-stack-vpn-gateway-settings.md). In dit artikel krijgt u inzicht in:

* Gatewaytypen, VPN-typen en verbindingstypen.
* Gatewaysubnetten, lokale netwerkgateways en andere resource-instellingen die u wilt gebruiken.

### <a name="deployment-tools"></a>Implementatiehulpmiddelen

U kunt maken en configureren van resources met behulp van een configuratiehulpprogramma, zoals de Azure-portal. U kunt later overschakelen naar een ander hulpprogramma, zoals PowerShell voor het configureren van extra resources of wijzig indien nodig bestaande bronnen. Op dit moment is het niet mogelijk om elke resource en resource-instelling in Azure Portal te configureren. De instructies in de artikelen voor elke verbindingstopologie geven aan of een specifiek confihuratiehulpprogramma nodig is.

## <a name="connection-topology-diagrams"></a>De verbindingstopologie

Het is belangrijk te weten dat er verschillende configuraties beschikbaar zijn voor VPN-gatewayverbindingen. Bepalen welke configuratie het beste past bij uw behoeften. In de volgende secties vindt u informatie en topologiediagrammen over de volgende VPN-gateway-verbindingen:

* Beschikbaar implementatiemodel
* Beschikbare configuratiehulpprogramma's
* Rechtstreekse koppelingen naar een artikel, indien beschikbaar

De diagrammen en beschrijvingen in de volgende secties kunt u een verbinding selecteren om te voldoen aan uw vereisten. De diagrammen tonen de belangrijkste basistopologieën, maar het is mogelijk te maken van complexere configuraties met de diagrammen als richtlijn.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Site-naar-site en multi-site (IPsec-/IKE VPN-tunnel)

### <a name="site-to-site"></a>Site-naar-site

Een site-naar-site-VPN-gatewayverbinding (S2S) is een verbinding via een VPN-tunnel met IPsec/IKE (IKEv1 of IKEv2). Dit type verbinding vereist een VPN-apparaat die zich on-premises en een openbaar IP-adres is toegewezen. Dit apparaat kan zich niet achter een NAT bevinden. S2S-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

![Voorbeeld van configuratie van een site-naar-site VPN-verbinding](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Multi-site

Dit type verbinding is een variatie op de site-naar-site-verbinding. U maakt meer dan één VPN-verbinding vanaf uw virtuele netwerkgateway, meestal met verschillende on-premises sites. Wanneer u met meerdere verbindingen werkt, moet u een RouteBased VPN-type (ook wel een gateway voor dynamische bij het werken met klassieke VNets.) Omdat elk virtueel netwerk maar één VPN-gateway kan hebben, delen alle verbindingen via de gateway de beschikbare bandbreedte. Dit wordt vaak een multi-site-verbinding genoemd.

![Voorbeeld van een verbinding tussen meerdere locaties met Azure VPN Gateway](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)

## <a name="gateway-skus"></a>Gateway-SKU's

Wanneer u een virtuele netwerkgateway voor Azure Stack maken, geeft u de gateway-SKU die u wilt gebruiken. De volgende VPN-gateway-SKU's worden ondersteund:

* Basic
* Standard
* HighPerformance

Wanneer u een hogere gateway-SKU, zoals Standard via Basic of HighPerformance via Standard- of Basic selecteert, worden meer CPU en bandbreedte van het netwerk worden toegewezen aan de gateway. Als gevolg hiervan kan de gateway hogere netwerkdoorvoer aan het virtuele netwerk ondersteunen.

Azure Stack biedt geen ondersteuning voor de UltraPerformance-gateway-SKU, die wordt gebruikt met Express Route.

Overweeg het volgende wanneer u een SKU selecteert:

* Azure Stack biedt geen ondersteuning voor op beleid gebaseerde gateways.
* Border Gateway Protocol (BGP) wordt niet ondersteund op de basis-SKU.
* ExpressRoute-VPN-Gateway worden gecombineerd configuraties worden niet ondersteund in Azure Stack.
* Actief-actief S2S VPN-gatewayverbindingen kunnen alleen worden geconfigureerd op de HighPerformance SKU.

## <a name="estimated-aggregate-throughput-by-sku"></a>Geschatte geaggregeerde doorvoer per SKU

In de volgende tabel ziet u de gatewaytypen en de geschatte geaggregeerde doorvoer per gateway-SKU.

|   | Doorvoer VPN-Gateway *(1)* | VPN-Gateway max. IPsec-tunnels *(2)* |
|-------|-------|-------|
|**Basis-SKU** ***(3)***    | 100 Mbps  | 10    |
|**Standaard-SKU**       | 100 Mbps  | 10    |
|**High Performance SKU** | 200 Mbps    | 5 |

**Opmerkingen bij de tabel:**

*Houd er rekening mee (1)* -VPN-doorvoer is niet een gegarandeerde doorvoer voor cross-premises verbindingen via Internet. Het is de meting van de maximaal mogelijke doorvoer.  
*Houd er rekening mee (2)* -Max tunnels wordt het totale aantal per Azure Stack-implementatie voor alle abonnementen.  
*Houd er rekening mee (3)* -BGP-routering wordt niet ondersteund voor de basis-SKU.

## <a name="next-steps"></a>Volgende stappen

[VPN-gatewayconfiguratie-instellingen voor Azure Stack](azure-stack-vpn-gateway-settings.md)
