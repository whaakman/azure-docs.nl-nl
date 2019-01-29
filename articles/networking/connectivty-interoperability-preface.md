---
title: 'Interoperabiliteit in Azure back-end-connectiviteit-functies: Instellingen testen | Microsoft Docs'
description: Dit artikel beschrijft een test-instellingen die u gebruiken kunt voor het analyseren van interoperabiliteit tussen ExpressRoute, een site-naar-site VPN- en virtueel netwerk in Azure-peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: abcf22dfebee695de54c36952c8f93dfaae31d57
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178763"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-setup"></a>Interoperabiliteit in Azure back-end-connectiviteit-functies: Test-installatie

Dit artikel wordt een test-instellingen die u gebruiken kunt om te analyseren hoe Azure netwerkservices samenwerken op het besturingselement vlak niveau en gegevens vlak niveau beschreven. Laten we kijken kort de Azure VPN-onderdelen:

-   **Azure ExpressRoute**: Gebruik van persoonlijke peering in Azure ExpressRoute particuliere IP-adresruimten in uw on-premises netwerk rechtstreeks verbinding te maken met uw Azure Virtual Network-implementaties. Met behulp van ExpressRoute kunt u een hogere bandbreedte en een particuliere verbinding. Veel ExpressRoute eco partners bieden ExpressRoute-connectiviteit met SLA's. Zie voor meer informatie over ExpressRoute en voor informatie over het configureren van ExpressRoute [Inleiding tot ExpressRoute][ExpressRoute].
-   **Site-naar-site VPN**: U kunt Azure VPN-Gateway als een site-naar-site VPN-netwerk veilig verbinding maken met een on-premises netwerk naar Azure via internet of met behulp van ExpressRoute. Zie voor informatie over het configureren van een site-naar-site-VPN verbinding maken met Azure, [VPN-Gateway configureren][VPN].
-   **VNet-peering**: Gebruik de peering op virtueel netwerk (VNet) om verbinding tussen VNets in Azure-netwerk te maken. Zie voor meer informatie over VNet-peering, de [zelfstudie over VNet-peering][VNet].

## <a name="test-setup"></a>Test-installatie

De volgende afbeelding ziet u de installatie testen:

[![1]][1]

De spil van de installatie van de test is de hub VNet in Azure-regio 1. De hub VNet is verbonden met andere netwerken in de volgende manieren:

-   De hub VNet is verbonden met het knooppunt VNet via VNet-peering. Externe toegang tot beide gateways van heeft het knooppunt VNet in de hub VNet.
-   De hub VNet is verbonden met de vertakking VNet met behulp van site-naar-site VPN. De verbinding maakt gebruik van eBGP voor het uitwisselen van routes.
-   De hub VNet is verbonden met de on-premises locatie 1-netwerk met behulp van ExpressRoute-privépeering als pad voor de primaire. Site-naar-site VPN-verbinding wordt gebruikt als de back-uppad. In de rest van dit artikel verwijzen we naar dit ExpressRoute-circuit als ExpressRoute 1. ExpressRoute-circuits kennen standaard redundante connectiviteit voor hoge beschikbaarheid. ExpressRoute-1, is de secundaire klant edge (CE)-router Hiermee die gezichten van de secundaire Microsoft Enterprise Edge Router (MSEE) uitgeschakeld. De uitgeschakelde CE router Hiermee Hiermee geeft u een rode lijn boven de dubbele lijn pijl in de afbeelding hierboven.
-   De hub VNet is verbonden met de on-premises locatie 2-netwerk met behulp van een ander ExpressRoute-privépeering. In de rest van dit artikel verwijzen we naar dit tweede ExpressRoute-circuit als ExpressRoute 2.
-   ExpressRoute-1 ook verbindt zowel de hub VNet als de locatie 1 on-premises netwerk met een externe VNet in Azure-regio 2.

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

Meer informatie over [configuratiedetails] [ Configuration] voor de test-topologie.

Meer informatie over [besturingselement vlak analysis] [ Control-Analysis] van de test-instellingen en de weergaven van andere VNets of VLAN's in de topologie.

Meer informatie over de [gegevens vlak analysis] [ Data-Analysis] van de installatie van de test en de Azure monitoring-weergaven van de functie-netwerk.

Zie de [Veelgestelde vragen over ExpressRoute] [ ExR-FAQ] aan:
-   Meer informatie over het aantal ExpressRoute-circuits die u verbinding met een ExpressRoute-gateway maken kunt.
-   Meer informatie over het aantal ExpressRoute-gateways die u verbinding met een ExpressRoute-circuit maken kunt.
-   Meer informatie over andere schaallimieten van ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "diagram van de test-topologie"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: connectivty-interoperability-configuration.md
[Control-Analysis]: connectivty-interoperability-control-plane.md
[Data-Analysis]: connectivty-interoperability-data-plane.md
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha


