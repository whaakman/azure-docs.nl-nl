---
title: 'Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - besturingselement vlak analyse: Azure back-end-connectiviteit biedt interoperabiliteit | Microsoft Docs'
description: Deze pagina bevat het besturingselement vlak analyse van de test-instellingen gemaakt voor het analyseren van de interoperabiliteit van functies voor ExpressRoute, Site-naar-site VPN- en VNet-Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: ee887da18b5666e61bc25365791b2e7dffb925e0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947218"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---control-plane-analysis"></a>Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - besturingselement vlak analyse

In dit artikel, we gaan via het besturingselement vlak analyse van de test-installatie. Als u bekijken van de Test-installatie wilt, raadpleegt u de [Test Setup][Setup]. De details van de configuratie van instellingen, Zie [Setup Testconfiguratie][Configuration].

Besturingselement vlak analysis onderzoekt in feite routes die worden uitgewisseld tussen netwerken in een topologie. Besturingselement vlak analysis help hoe verschillende weergave de topologie van het netwerk.

##<a name="hub-and-spoke-vnet-perspective"></a>Hub- en -Spokenetwerktopologie VNet perspectief

Het volgende diagram illustreert het netwerk van de Hub VNet en het perspectief van knooppunt VNet (gemarkeerd in het blauw). Het diagram toont ook de Autonomous System Number (ASN) van een ander netwerk en routes die worden uitgewisseld tussen verschillende netwerken. 

[![1]][1]

U ziet dat het VNet ASN ExpressRoute-gateway van de ASN van Microsoft Enterprise-Randrouters (msee's verschilt). ExpressRoute-gateway maakt gebruik van een persoonlijke ASN (65515) en msee's gebruiken openbare ASN (12076) wereldwijd. Als u ExpressRoute-peering, omdat MSEE de peer configureert, kunt u 12076 gebruikt als de peer-ASN. Aan de Azure brengt MSEE eBGP-peering met ExpressRoute GW. De dubbele eBGP-peering die de MSEE wordt tot stand voor elk ExpressRoute-peering gebracht is transparant op het niveau van het vlak van het besturingselement. Daarom wanneer een routetabel ExpressRoute wordt weergegeven, ziet u het VNet ExpressRoute GW ASN worden gebruikt voor het VNet-voorvoegsels. Een voorbeeld van ExpressRoute route tabel schermopname is hieronder weergegeven: 

[![5]][5]

In Azure is alleen het ASN aanzienlijke vanuit het oogpunt van de peering. De ASN van zowel ExpressRoute-gateway en VPN-GW is standaard 65515.

##<a name="on-premises-location-1-and-remote-vnet-perspective-via-expressroute-1"></a>On-Premises locatie-1 en externe VNet perspectief via ExpressRoute-1

On-Premises locatie-1 en externe VNet zijn beide verbonden met de Hub VNet via ExpressRoute 1 en daarom delen ze hetzelfde perspectief van de topologie op, zoals wordt weergegeven in het onderstaande diagram.

[![2]][2]

##<a name="on-premises-location-1-and-branch-vnet-perspective-via-site-to-site-vpn"></a>On-Premises locatie-1 en vertakking VNet perspectief via Site-naar-Site-VPN

On-Premises locatie-1 en vertakking VNet zijn beide verbonden met de Hub VNet-VPN-GW via Site-naar-Site VPN-verbindingen en hebben dus ze hetzelfde perspectief van de topologie op, zoals wordt weergegeven in het onderstaande diagram.

[![3]][3]

##<a name="on-premises-location-2-perspective"></a>On-Premises locatie-2-perspectief

On-Premises locatie-2 is verbonden met de Hub VNet via persoonlijke peering van ExpressRoute-2. 

[![4]][4]

## <a name="further-reading"></a>Lees hier meer over

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>In combinatie met behulp van ExpressRoute en Site-naar-Site VPN-verbinding

####  <a name="site-to-site-vpn-over-expressroute"></a>Site-naar-Site-VPN via ExpressRoute

Site-naar-Site VPN kan worden geconfigureerd via ExpressRoute-Microsoft-peering voor het uitwisselen van gegevens tussen uw on-premises netwerk en uw Azure-VNets privé met vertrouwen te behandelen, anti opnieuw afspelen, echtheid en integriteit. Zie voor meer informatie over het configureren van Site-naar-Site IPSec VPN-in-tunnelmodus via ExpressRoute-Microsoft-peering [Site-naar-site-VPN via het ExpressRoute-Microsoft-peering][S2S-Over-ExR]. 

De belangrijkste beperking van het configureren van S2S VPN via Microsoft-peering is de doorvoer. Doorvoer via de IPSec-tunnel wordt beperkt door de VPN-GW-capaciteit. De VPN-GW-doorvoer is dat kleiner vergeleken met de doorvoer van ExpressRoute. In dergelijke scenario's wilt met behulp van de IPSec-tunnel voor hoge beveiligd verkeer en privépeering voor alle andere verkeer helpen de ExpressRoute-bandbreedtegebruik optimaliseren.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-naar-Site-VPN als een beveiligd failoverpad voor ExpressRoute
ExpressRoute wordt aangeboden als paar redundante circuit om hoge beschikbaarheid te garanderen. U kunt geo-redundante ExpressRoute-connectiviteit configureren in verschillende Azure-regio's. Ook zoals in de instellingen van de test wordt uitgevoerd binnen een bepaald Azure-regio, kunt als u wilt dat een failoverpad voor ExpressRoute-connectiviteit u doen met behulp van Site-naar-Site VPN. Wanneer dezelfde voorvoegsels worden geadverteerd via ExpressRoute- en S2S VPN-, Azure geeft de voorkeur aan ExpressRoute via S2S VPN-verbinding. Om te voorkomen dat asymmetrische routering tussen ExpressRoute als S2S-VPN, on-premises netwerkconfiguratie moet ook hyperlink voorkeur ExpressRoute via een S2S-VPN-verbinding.

Zie voor meer informatie over het configureren van ExpressRoute en Site-naar-Site VPN-verbindingen [ExpressRoute en Site-naar-Site co-existentie][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Back-end-connectiviteit voor knooppunt VNets en filialen uitbreiden

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Knooppunt-VNet-connectiviteit met behulp van VNet-peering

Hub en spoke-Vnet-architectuur wordt veel gebruikt. De hub is een virtueel netwerk (VNet) in Azure die als een centraal punt van connectiviteit tussen uw knooppunt VNets en naar uw on-premises netwerk fungeert. De spaken zijn VNets die zijn gekoppeld aan de hub en kunnen worden gebruikt om workloads te isoleren. Verkeer stroomt tussen het on-premises datacenter en de hub via een ExpressRoute of VPN-verbinding. Zie voor meer informatie over de architectuur [Hub en Spoke-architectuur][Hub-n-Spoke]

VNet-peering binnen een regio kunt knooppunt VNets hub VNet-gateways (VPN- en ExpressRoute-gateways) gebruiken om te communiceren met externe netwerken.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>VNet-connectiviteit met behulp van Site-naar-Site VPN vertakking

Als u wilt dat de vertakking vnet's (in verschillende regio's) en on-premises netwerken met elkaar via een hub vnet communiceren, is de systeemeigen Azure-oplossing site-naar-site VPN-connectiviteit via VPN. Een alternatief is het gebruik van een NVA voor routering in de hub.

Zie voor het configureren van VPN-gateways, [configureren van VPN-Gateway][VPN]. Zie voor het implementeren van maximaal beschikbare NVA [NVA maximaal beschikbaar implementeren][Deploy-NVA].

## <a name="next-steps"></a>Volgende stappen

Zie voor analyse van gegevens vlak van de test-instellingen en voor Azure-netwerk functies controleweergaven [Gegevensvlak Analysis][Data-Analysis].

Zie voor meer informatie over het aantal ExpressRoute-circuits die u kunt verbinding maken met een ExpressRoute-Gateway of het aantal ExpressRoute-Gateways u verbinding met een ExpressRoute-circuit maken kunt, of voor meer informatie over andere schaallimieten van ExpressRoute, [Veelgestelde vragen over ExpressRoute][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "hub en Spoke VNet perspectief van de topologie"
[2]: ./media/backend-interoperability/Loc1ExRView.png "locatie-1 en het externe VNet perspectief via ExpressRoute 1 van de topologie"
[3]: ./media/backend-interoperability/Loc1VPNView.png "locatie-1 en het perspectief van de vertakking VNet via S2S VPN van de topologie"
[4]: ./media/backend-interoperability/Loc2View.png "locatie 2 perspectief van de topologie"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "ExpressRoute 1 RouteTable"

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




