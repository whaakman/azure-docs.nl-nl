---
title: 'Interoperabiliteit in Azure back-end-connectiviteit-functies: Besturingselement vlak analyse | Microsoft Docs'
description: Dit artikel bevat de besturingselement vlak analyse van de test-instellingen die u gebruiken kunt voor het analyseren van interoperabiliteit tussen ExpressRoute, een site-naar-site VPN- en virtueel netwerk in Azure-peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 77a405e2f020ff764348370fc001388610ad75b6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155524"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-control-plane-analysis"></a>Interoperabiliteit in Azure back-end-connectiviteit-functies: Besturingselement vlak analyse

Dit artikel wordt beschreven voor het besturingselement vlak analyse van de [instellingen testen][Setup]. U kunt ook bekijken de [setup Testconfiguratie] [ Configuration] en de [gegevens vlak analysis] [ Data-Analysis] van de test-installatie.

Besturingselement vlak analysis onderzoekt in feite routes die worden uitgewisseld tussen netwerken in een topologie. Besturingselement vlak analyse kan u helpen begrijpen hoe verschillende netwerken weergeven van de topologie.

## <a name="hub-and-spoke-vnet-perspective"></a>Hub en spoke-VNet-perspectief

De volgende afbeelding ziet u het netwerk vanuit het perspectief van een hub-netwerk (VNet) en een knooppunt VNet (gemarkeerd in het blauw). De afbeelding toont ook de autonoom systeemnummer (ASN) van de verschillende netwerken en routes die worden uitgewisseld tussen verschillende netwerken: 

[![1]][1]

De ASN van Azure ExpressRoute-gateway van het VNet wijkt af van de ASN van Microsoft Enterprise-Randrouters (msee's). Een ExpressRoute-gateway maakt gebruik van een persoonlijke ASN (een waarde van **65515**) en de msee's gebruiken openbare ASN (een waarde van **12076**) wereldwijd. Bij het configureren van ExpressRoute-peering, omdat de MSEE de peer is, gebruikt u **12076** als de peer-ASN. Aan de Azure brengt MSEE eBGP-peering met de ExpressRoute-gateway. De dubbele eBGP-peering die de MSEE wordt tot stand voor elk ExpressRoute-peering gebracht is transparant op het niveau van het besturingselement vlak. Daarom wanneer u een routetabel ExpressRoute bekijkt, ziet u het VNet ExpressRoute-gateway ASN voor van de VNet-voorvoegsels. 

De volgende afbeelding ziet u de routetabel van een voorbeeld van ExpressRoute: 

[![5]][5]

In Azure is het ASN aanzienlijke alleen vanuit het oogpunt van de peering. De ASN van zowel de ExpressRoute-gateway en de VPN-gateway in de Azure VPN-Gateway is standaard **65515**.

## <a name="on-premises-location-1-and-the-remote-vnet-perspective-via-expressroute-1"></a>On-premises locatie 1 en het externe VNet perspectief via ExpressRoute 1

Zowel on-premises locatie 1 als het externe VNet zijn verbonden met de hub VNet via ExpressRoute 1. Ze delen hetzelfde perspectief van de topologie op, zoals wordt weergegeven in het volgende diagram:

[![2]][2]

## <a name="on-premises-location-1-and-the-branch-vnet-perspective-via-a-site-to-site-vpn"></a>On-premises locatie 1 en de vertakking perspectief VNet via een site-naar-site-VPN

Zowel on-premises locatie 1 en de vertakking VNet zijn verbonden met een hub-VNet-VPN-gateway via een site-naar-site VPN-verbinding. Ze delen hetzelfde perspectief van de topologie op, zoals wordt weergegeven in het volgende diagram:

[![3]][3]

## <a name="on-premises-location-2-perspective"></a>On-premises locatie 2 perspectief

On-premises locatie 2 is verbonden met een hub VNet via persoonlijke peering van ExpressRoute 2: 

[![4]][4]

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>ExpressRoute- en site-naar-site VPN-connectiviteit in combinatie

###  <a name="site-to-site-vpn-over-expressroute"></a>Site-naar-site-VPN via ExpressRoute

U kunt een site-naar-site-VPN configureren met behulp van ExpressRoute Microsoft-peering voor het uitwisselen van gegevens tussen uw on-premises netwerk en uw Azure-VNets privé. Met deze configuratie kunt u gegevens uitwisselen met de vertrouwelijkheid, echtheid en integriteit. De gegevensuitwisseling is ook anti opnieuw afspelen. Zie voor meer informatie over het configureren van een site-naar-site IPsec VPN in tunnelmodus met behulp van ExpressRoute Microsoft-peering [Site-naar-site-VPN via het ExpressRoute-Microsoft-peering][S2S-Over-ExR]. 

De belangrijkste beperking van het configureren van een site-naar-site-VPN dat gebruikmaakt van Microsoft-peering is doorvoer. Doorvoer via de IPsec-tunnel wordt beperkt door de capaciteit van de VPN-gateway. De VPN-gateway-doorvoer is lager dan de doorvoer van ExpressRoute. In dit scenario, met behulp van de IPsec-tunnel voor goed beveiligd verkeer en het gebruik van persoonlijke peering voor al het andere verkeer kunt de ExpressRoute-bandbreedtegebruik optimaliseren.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-naar-site-VPN als een beveiligd failoverpad voor ExpressRoute

ExpressRoute fungeert als een paar redundante circuit om hoge beschikbaarheid te garanderen. U kunt geo-redundante ExpressRoute-connectiviteit configureren in verschillende Azure-regio's. Ook, zoals u in onze test setup, binnen een Azure-regio, kunt u een site-naar-site VPN een failoverpad voor de ExpressRoute-verbinding maken. Wanneer dezelfde voorvoegsels worden geadverteerd via ExpressRoute en een site-naar-site-VPN, prioriteert Azure ExpressRoute. Om te voorkomen dat asymmetrische routering tussen ExpressRoute en de site-naar-site VPN-verbinding, on-premises netwerkconfiguratie moet ook hyperlink met behulp van ExpressRoute-connectiviteit voordat deze site-naar-site VPN-verbinding gebruikt.

Zie voor meer informatie over het naast elkaar bestaande verbindingen configureren voor ExpressRoute en een site-naar-site-VPN [ExpressRoute en site-naar-site co-existentie][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Back-end-connectiviteit voor knooppunt VNets en filialen uitbreiden

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>Knooppunt-VNet-connectiviteit met behulp van VNet-peering

Hub en spoke-VNet-architectuur wordt veel gebruikt. De hub is een VNet in Azure die als een centraal punt van connectiviteit tussen uw knooppunt VNets en naar uw on-premises netwerk fungeert. De spaken zijn VNets die zijn gekoppeld aan de hub, en die u kunt gebruiken om workloads te isoleren. Verkeer stroomt tussen het on-premises datacenter en de hub via een ExpressRoute of VPN-verbinding. Zie voor meer informatie over de architectuur, [een ster-netwerktopologie implementeren in Azure][Hub-n-Spoke].

In de VNet-peering binnen een regio, knooppunt VNets hub VNet-gateways (VPN- en ExpressRoute-gateways) gebruiken om te communiceren met externe netwerken.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>VNet-connectiviteit vertakking met behulp van site-naar-site-VPN

U kunt de vertakking VNets die zich in verschillende regio's en on-premises netwerken met elkaar communiceren via een hub VNet. De systeemeigen Azure-oplossing voor deze configuratie is de site-naar-site VPN-verbinding met behulp van een VPN. Een alternatief is het gebruik van een virtueel netwerkapparaat (NVA) voor de routering in de hub.

Zie voor meer informatie, [wat is VPN-Gateway?] [ VPN] en [implementeren van een maximaal beschikbare NVA][Deploy-NVA].

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [gegevens vlak analysis] [ Data-Analysis] van de installatie van de test en de Azure monitoring-weergaven van de functie-netwerk.

Zie de [Veelgestelde vragen over ExpressRoute] [ ExR-FAQ] aan:
-   Meer informatie over het aantal ExpressRoute-circuits die u verbinding met een ExpressRoute-gateway maken kunt.
-   Meer informatie over het aantal ExpressRoute-gateways die u verbinding met een ExpressRoute-circuit maken kunt.
-   Meer informatie over andere schaallimieten van ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "hub en spoke-VNet-perspectief van de topologie"
[2]: ./media/backend-interoperability/Loc1ExRView.png "locatie 1 en het externe VNet perspectief van de topologie via ExpressRoute 1"
[3]: ./media/backend-interoperability/Loc1VPNView.png "locatie 1 en de vertakking VNet perspectief van de topologie via een site-naar-site-VPN"
[4]: ./media/backend-interoperability/Loc2View.png "locatie 2 perspectief van de topologie"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "routetabel ExpressRoute 1"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


