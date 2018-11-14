---
title: 'Interoperabiliteit in Azure back-end-connectiviteit-functies: informatie over de configuratie | Microsoft Docs'
description: Dit artikel bevat informatie over de configuratie voor de installatie van de test die u gebruiken kunt voor het analyseren van interoperabiliteit tussen ExpressRoute, een site-naar-site VPN- en virtueel netwerk in Azure-peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: a460a8f4c652182a7916cba5aef0520834432909
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614808"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-configuration-details"></a>Interoperabiliteit in Azure back-end-connectiviteit-functies: informatie over de configuratie testen

In dit artikel beschrijft de configuratiedetails van de [instellingen testen][Setup]. De test-installatie helpt u analyseren hoe Azure netwerkservices samenwerken op het besturingselement vlak niveau en gegevens vlak niveau.

## <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>Knooppunt-VNet-connectiviteit met behulp van VNet-peering

De volgende afbeelding ziet u de details van Azure Virtual Network peering van een knooppunt virtueel netwerk (VNet). Zie voor meer informatie over het instellen van een peering tussen twee VNets, [beheren VNet-peering][VNet-Config]. Als u wilt dat het knooppunt VNet-naar-gebruik van de gateways die zijn verbonden met de hub VNet, selecteer **externe gateways gebruiken**.

[![1]][1]

De volgende afbeelding ziet u de details van VNet-peering van de hub VNet. Als u wilt dat het knooppunt VNet-naar de hub VNet-gateways gebruiken, selecteert u **externe gateways gebruiken**.

[![2]][2]

## <a name="branch-vnet-connectivity-by-using-a-site-to-site-vpn"></a>VNet-connectiviteit vertakking met behulp van een site-naar-site-VPN

Site-naar-site VPN-verbinding tussen de hub en de vertakking VNets instellen met behulp van VPN-gateways in Azure VPN-Gateway. Standaard VPN-gateways en Azure ExpressRoute-gateways gebruiken een persoonlijke autonomous system number (ASN)-waarde van **65515**. U kunt de ASN-waarde in de VPN-Gateway wijzigen. In de test-instellingen, de ASN-waarde van de vertakking VNet-VPN-gateway is gewijzigd in **65516** ter ondersteuning van eBGP routering tussen de hub en de vertakking VNets.


[![3]][3]


## <a name="on-premises-location-1-connectivity-by-using-expressroute-and-a-site-to-site-vpn"></a>On-premises locatie 1 connectiviteit met behulp van ExpressRoute en een site-naar-site-VPN

### <a name="expressroute-1-configuration-details"></a>Informatie over de configuratie van de ExpressRoute-1

De volgende afbeelding ziet u de configuratie van de Azure-regio 1 ExpressRoute-circuit naar on-premises locatie 1 klant (CE) randrouters:

[![4]][4]

De volgende afbeelding ziet u de configuratie van de verbinding tussen de 1 ExpressRoute-circuit en de hub VNet:

[![5]][5]

De volgende lijst bevat de configuratie van de primaire CE router voor ExpressRoute-connectiviteit voor persoonlijke peering. (Cisco ASR1000 routers worden gebruikt als CE routers in de test-installatie.) Wanneer de site-naar-site VPN en ExpressRoute-circuits zijn geconfigureerd in gelijktijdig naar een on-premises netwerk verbinden met Azure, Azure bepaalt de volgorde van het ExpressRoute-circuit standaard. Om te voorkomen asymmetrische routering, de on-premises netwerk ook prioriteit moet krijgen ExpressRoute-connectiviteit via site-naar-site VPN-verbinding. De volgende configuratie prioriteitsaanduiding maakt met behulp van de BGP **lokale voorkeur** kenmerk:

    interface TenGigabitEthernet0/0/0.300
     description Customer 30 private peering to Azure
     encapsulation dot1Q 30 second-dot1q 300
     ip vrf forwarding 30
     ip address 192.168.30.17 255.255.255.252
    !
    interface TenGigabitEthernet1/0/0.30
     description Customer 30 to south bound LAN switch
     encapsulation dot1Q 30
     ip vrf forwarding 30
     ip address 192.168.30.0 255.255.255.254
     ip ospf network point-to-point
    !
    router ospf 30 vrf 30
     router-id 10.2.30.253
     redistribute bgp 65021 subnets route-map BGP2OSPF
     network 192.168.30.0 0.0.0.1 area 0.0.0.0
    default-information originate always
     default-metric 10
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 192.168.30.18 remote-as 12076
      neighbor 192.168.30.18 activate
      neighbor 192.168.30.18 next-hop-self
      neighbor 192.168.30.18 soft-reconfiguration inbound
      neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
      neighbor 192.168.30.18 prefix-list Cust30_to_Private out
     exit-address-family
    !
    route-map prefer-ER-over-VPN permit 10
     set local-preference 200
    !
    ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
    !

### <a name="site-to-site-vpn-configuration-details"></a>Informatie over de configuratie van de site-naar-site VPN

De volgende lijst bevat de configuratie van de primaire CE router voor site-naar-site VPN-verbinding:

    crypto ikev2 proposal Cust30-azure-proposal
     encryption aes-cbc-256 aes-cbc-128 3des
     integrity sha1
     group 2
    !
    crypto ikev2 policy Cust30-azure-policy
     match address local 66.198.12.106
     proposal Cust30-azure-proposal
    !
    crypto ikev2 keyring Cust30-azure-keyring
     peer azure
      address 52.168.162.84
      pre-shared-key local IamSecure123
      pre-shared-key remote IamSecure123
    !
    crypto ikev2 profile Cust30-azure-profile
     match identity remote address 52.168.162.84 255.255.255.255
     identity local address 66.198.12.106
     authentication local pre-share
     authentication remote pre-share
     keyring local Cust30-azure-keyring
    !
    crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
     mode tunnel
    !
    crypto ipsec profile Cust30-azure-ipsec-profile
     set transform-set Cust30-azure-ipsec-proposal-set
     set ikev2-profile Cust30-azure-profile
    !
    interface Loopback30
     ip address 66.198.12.106 255.255.255.255
    !
    interface Tunnel30
     ip vrf forwarding 30
     ip address 10.2.30.125 255.255.255.255
     tunnel source Loopback30
     tunnel mode ipsec ipv4
     tunnel destination 52.168.162.84
     tunnel protection ipsec profile Cust30-azure-ipsec-profile
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 10.10.30.254 remote-as 65515
      neighbor 10.10.30.254 ebgp-multihop 5
      neighbor 10.10.30.254 update-source Tunnel30
      neighbor 10.10.30.254 activate
      neighbor 10.10.30.254 soft-reconfiguration inbound
     exit-address-family
    !
    ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30

## <a name="on-premises-location-2-connectivity-by-using-expressroute"></a>On-premises locatie 2-connectiviteit met behulp van ExpressRoute

On-premises locatie 2 verbindt een tweede ExpressRoute-circuit in dichter bij de on-premises locatie 2, met de hub VNet. De volgende afbeelding ziet u de tweede ExpressRoute-configuratie:

[![6]][6]

De volgende afbeelding ziet u de configuratie van de verbinding tussen het tweede ExpressRoute-circuit en de hub VNet:

[![7]][7]

ExpressRoute-1 maakt de hub VNet, zowel on-premises locatie 1 verbinding met een externe VNet in een andere Azure-regio:

[![8]][8]

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

U kunt de vertakking VNets die zich in verschillende regio's en on-premises netwerken met elkaar communiceren via een hub VNet. De systeemeigen Azure-oplossing voor dit cofiguration is site-naar-site VPN-verbinding via een VPN-verbinding. Een alternatief is het gebruik van een virtueel netwerkapparaat (NVA) voor de routering in de hub.

Zie voor meer informatie, [wat is VPN-Gateway?] [ VPN] en [implementeren van een maximaal beschikbare NVA][Deploy-NVA].

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [besturingselement vlak analysis] [ Control-Analysis] van de test-instellingen en de weergaven van andere VNets of VLAN's in de topologie.

Meer informatie over [gegevens vlak analysis] [ Data-Analysis] van de installatie van de test en de Azure monitoring-weergaven van de functie-netwerk.

Zie de [Veelgestelde vragen over ExpressRoute] [ ExR-FAQ] aan:
-   Meer informatie over het aantal ExpressRoute-circuits die u verbinding met een ExpressRoute-gateway maken kunt.
-   Meer informatie over het aantal ExpressRoute-gateways die u verbinding met een ExpressRoute-circuit maken kunt.
-   Meer informatie over andere schaallimieten van ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "knooppunt van het VNet-VNet-peering"
[2]: ./media/backend-interoperability/HubVNet-peering.png "hub VNet van VNet-peering"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "VPN-Gateway-configuratie van een vertakking VNet"
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute-1-configuratie"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "Verbindingsconfiguratie van ExpressRoute 1 naar een hub VNet ExR gateway"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute 2-configuratie"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "Verbindingsconfiguratie van ExpressRoute 2 met een hub VNet ExR gateway"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "Verbindingsconfiguratie van ExpressRoute 2 met een externe VNet ExR-gateway"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
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


