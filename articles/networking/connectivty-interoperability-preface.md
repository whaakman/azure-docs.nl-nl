---
title: 'Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - instellingen testen: Azure back-end-connectiviteit biedt interoperabiliteit | Microsoft Docs'
description: Deze pagina vindt u een test-instelling die wordt gebruikt voor het analyseren van interoperabiliteit van functies voor ExpressRoute, Site-naar-site VPN- en VNet-Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: e859a0a3ac35a9d9f2dab579b7609192e599f90f
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947216"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-setup"></a>Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-peering - instellingen
In dit artikel gaan we een test-instellingen die u gebruiken kunt om te analyseren hoe de verschillende functies tussen werken met elkaar zowel op het niveau van controle- en gegevenslaag identificeren. Vóór het bespreken van de test-installatie, laten we kort bekijken wat deze verschillende Azure-netwerkfuncties betekenen.

Met behulp van ExpressRoute: ExpressRoute privépeering u rechtstreeks verbinding maken privé IP-adresruimten van uw on-premises netwerk naar uw Azure-VNet-implementaties.  Met behulp van ExpressRoute kunt u bereiken meer bandbreedte en particuliere verbinding. Er zijn veel ExpressRoute eco partners, die voorzien van ExpressRoute-connectiviteit met SLA. Zie voor meer informatie over ExpressRoute en hoe dit moet worden geconfigureerd, [Inleiding tot ExpressRoute][ExpressRoute]

Site-naar-Site-VPN: Als u wilt verbinden een on-premises netwerk veilig naar Azure via Internet of via ExpressRoute, Site-naar-Site (S2S) VPN-optie is beschikbaar. Zie voor meer informatie over het configureren van S2S VPN voor verbinding met Azure, [VPN-Gateway configureren][VPN]

VNet-peering: VNet-peering is beschikbaar voor het maken van verbinding tussen virtuele netwerken (VNets). Zie voor meer informatie over VNet-peering, [zelfstudie over VNet-Peering][VNet].

##<a name="test-setup"></a>Test-installatie

Het volgende diagram ziet u de installatie testen.

[![1]][1]

De center-gedeelte van de installatie van de test is de Hub Vnet in de Azure-regio 1. De Hub Vnet is verbonden met verschillende netwerken als volgt:

1.  Op het knooppunt Vnet via Vnet-peering. Het knooppunt Vnet heeft externe toegang tot beide gateways in de Hub Vnet.
2.  Vertakking vnet via Site-naar-Site VPN. De verbinding maakt gebruik van eBGP voor het uitwisselen van routes.
3.  Locatie-1 on-premises netwerk via ExpressRoute-privépeering als het primaire pad en de Site-naar-Site VPN-verbinding als pad voor de back-up. In de rest van dit document gaan we noemen dit ExpressRoute-circuit ExpressRoute1. Standaard bieden ExpressRoute-circuits redundante connectiviteit voor hoge beschikbaarheid. Van de secundaire CE-router Hiermee naar de secundaire MSEE gericht is op de ExpressRoute1 uitgeschakeld. Dit wordt aangegeven met een rode-regel boven de pijl dubbele lijn in het bovenstaande diagram.
4.  Op locatie 2 on-premises netwerk via een andere ExpressRoute-privépeering. In de rest van dit document gaan we noemen dit tweede ExpressRoute-circuit ExpressRoute2.
5.  ExpressRoute1 de Hub Vnet en de locatie-1 on-premises wordt ook verbinding maakt met een externe Vnet in Azure-regio 2.

## <a name="further-reading"></a>Lees hier meer over

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>In combinatie met behulp van ExpressRoute en Site-naar-Site VPN-verbinding

#### <a name="site-to-site-vpn-over-expressroute"></a>Site-naar-Site-VPN via ExpressRoute 

Site-naar-Site VPN kan worden geconfigureerd via ExpressRoute-Microsoft-peering voor het uitwisselen van gegevens tussen uw on-premises netwerk en uw Azure-VNets privé met vertrouwen te behandelen, anti opnieuw afspelen, echtheid en integriteit. Zie voor meer informatie over het configureren van Site-naar-Site IPSec VPN-in-tunnelmodus via ExpressRoute-Microsoft-peering [Site-naar-site-VPN via het ExpressRoute-Microsoft-peering][S2S-Over-ExR]. 

De belangrijkste beperking van het configureren van S2S VPN via Microsoft-peering is de doorvoer. Doorvoer via de IPSec-tunnel wordt beperkt door de VPN-GW-capaciteit. De VPN-GW-doorvoer is dat kleiner vergeleken met de doorvoer van ExpressRoute. In dergelijke scenario's wilt met behulp van de IPSec-tunnel voor hoge beveiligd verkeer en privépeering voor alle andere verkeer helpen de ExpressRoute-bandbreedtegebruik optimaliseren.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-naar-Site-VPN als een beveiligd failoverpad voor ExpressRoute
ExpressRoute wordt aangeboden als paar redundante circuit om hoge beschikbaarheid te garanderen. U kunt geo-redundante ExpressRoute-connectiviteit configureren in verschillende Azure-regio's. Ook zoals in de instellingen van de test wordt uitgevoerd binnen een bepaald Azure-regio, kunt als u wilt dat een failoverpad voor ExpressRoute-connectiviteit u doen met behulp van Site-naar-Site VPN. Wanneer dezelfde voorvoegsels worden geadverteerd via ExpressRoute- en S2S VPN-, Azure geeft de voorkeur aan ExpressRoute via S2S VPN-verbinding. Om te voorkomen dat asymmetrische routering tussen ExpressRoute als S2S-VPN, on-premises netwerkconfiguratie moet ook hyperlink voorkeur ExpressRoute via een S2S-VPN-verbinding.

Zie voor meer informatie over het configureren van ExpressRoute en Site-naar-Site VPN-verbindingen [ExpressRoute en Site-naar-Site co-existentie][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Back-end-connectiviteit voor knooppunt VNets en filialen uitbreiden

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Knooppunt-VNet-connectiviteit met behulp van VNet-peering

Hub en spoke-Vnet-architectuur wordt veel gebruikt. De hub is een virtueel netwerk (VNet) in Azure die als een centraal punt van connectiviteit tussen uw knooppunt VNets en naar uw on-premises netwerk fungeert. De spaken zijn VNets die zijn gekoppeld aan de hub en kunnen worden gebruikt om workloads te isoleren. Verkeer stroomt tussen het on-premises datacenter en de hub via een ExpressRoute of VPN-verbinding. Zie voor meer informatie over de architectuur, [Hub en Spoke-architectuur][Hub-n-Spoke]

VNet-peering binnen een regio kunt knooppunt VNets hub VNet-gateways (VPN- en ExpressRoute-gateways) gebruiken om te communiceren met externe netwerken.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>VNet-connectiviteit met behulp van Site-naar-Site VPN vertakking

Als u wilt dat de vertakking vnet's (in verschillende regio's) en on-premises netwerken met elkaar via een hub vnet communiceren, is de systeemeigen Azure-oplossing site-naar-site VPN-connectiviteit via VPN. Een alternatief is het gebruik van een NVA voor routering in de hub.

Zie voor het configureren van VPN-gateways, [configureren van VPN-Gateway][VPN]. Zie voor het implementeren van maximaal beschikbare NVA [NVA maximaal beschikbaar implementeren][Deploy-NVA].

## <a name="next-steps"></a>Volgende stappen

Zie voor de configuratiedetails van de test-topologie, [configuratiedetails][Configuration].

Zie voor het besturingselement vlak analyse van de test-setup en geeft inzicht in de weergaven van andere VNet/VLAN van de topologie, [Bedieningsvlak Analysis][Control-Analysis].

Zie voor analyse van gegevens vlak van de test-instellingen en voor Azure-netwerk functies controleweergaven [Gegevensvlak Analysis][Data-Analysis].

Zie voor meer informatie over het aantal ExpressRoute-circuits die u kunt verbinding maken met een ExpressRoute-Gateway of het aantal ExpressRoute-Gateways u verbinding met een ExpressRoute-circuit maken kunt, of voor meer informatie over andere schaallimieten van ExpressRoute, [Veelgestelde vragen over ExpressRoute][ExR-FAQ]



<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "de Test-topologie"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha




