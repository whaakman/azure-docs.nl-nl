---
title: 'Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - configuratiedetails: Azure back-end-connectiviteit biedt interoperabiliteit | Microsoft Docs'
description: Deze pagina bevat de configuratiegegevens van de test-installatie die wordt gebruikt voor het analyseren van interoperabiliteit van functies voor ExpressRoute, Site-naar-site VPN- en VNet-Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: d94900b764331c6fff0e0384e6edbebc88ac938b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947212"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-configuration-details"></a>Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - informatie over de configuratie van de Test

In dit artikel, we gaan via de configuratiedetails van de installatie testen. De Test-instellingen, Zie de [Test Setup][Setup]. 

##<a name="spoke-vnet-connectivity-using-vnet-peering"></a>Knooppunt-VNet-connectiviteit met behulp van VNet-peering

De volgende schermafbeelding voor Azure portal ziet de details van VNet-peering van het knooppunt VNet. Zie voor stapsgewijze instructies voor het configureren van VNet-peering tussen twee virtuele netwerken [VNet-Peering beheren][VNet-Config]. Als u wilt dat het de gateways die zijn verbonden met de Hub VNet gebruiken knooppunt VNet, moet u controleren *externe gateways gebruiken*.

[![1]][1]

De volgende schermafbeelding voor Azure portal ziet de details van VNet-peering van de hub VNet. Als u wilt dat de Hub VNet waarmee u het knooppunt VNet de gateways gebruiken, moet u controleren *externe gateways gebruiken*.

[![2]][2]

##<a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>VNet-connectiviteit met behulp van Site-naar-Site VPN vertakking

Site-naar-Site VPN-verbinding tussen de Hub en de vertakking VNets is geconfigureerd met behulp van VPN-gateways. VPN en ExpressRoute-gateways zijn standaard geconfigureerd met de waarde van de persoonlijke ASN van 65515. VPN-gateway kunt u de ASN-waarde te wijzigen. In de test-instellingen, zoals wordt weergegeven in de volgende schermafbeelding Azure portal, de ASN-waarde van de vertakking VNet-VPN-gateway gewijzigd in 65516 eBGP routering tussen de Hub en de vertakking vnet's inschakelen.


[![3]][3]


##<a name="location-1-on-premises-connectivity-using-expressroute-and-site-to-site-vpn"></a>Locatie-1 on-premises-connectiviteit met ExpressRoute en Site-naar-Site-VPN

###<a name="expressroute1-configuration-details"></a>Informatie over de configuratie van ExpressRoute1

De volgende portal schermafbeelding ziet u de configuratie van de Azure-regio 1 ExpressRoute-circuit naar de locatie-1 on-premises CE Routers.

[![4]][4]

De volgende schermafbeelding van de portal ziet u de verbindingsconfiguratie tussen ExpressRoute1 circuit en de Hub VNet.

[![5]][5]

De volgende configuratie is de vermelding van de primaire CE-router (Cisco ASR1000 routers worden gebruikt als routers in de instellingen van de test CE) configureren met betrekking tot de ExpressRoute-connectiviteit voor persoonlijke peering. Wanneer zowel Site-naar-Site VPN en ExpressRoute-circuit worden geconfigureerd in gelijktijdig naar een on-premises netwerk verbinden met Azure; Azure geeft de voorkeur aan ExpressRoute-circuit standaard. Om te voorkomen asymmetrische routering, on-premises netwerk moet ook de voorkeur geeft aan ExpressRoute via Site-naar-Site VPN-verbinding voor de routes die worden ontvangen van zowel via ExpressRoute als Site-naar-Site VPN. Dit wordt bereikt in de volgende configuratie met behulp van BGP lokale voorkeur kenmerk. 

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

###<a name="site-to-site-vpn-configuration-details"></a>Informatie over de configuratie van de site-naar-Site VPN

Hier volgt de lijst met de primaire CE routerconfiguratie met betrekking tot Site-naar-Site VPN-verbinding:

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

##<a name="location-2-on-premises-connectivity-using-expressroute"></a>Locatie 2 on-premises connectiviteit met behulp van ExpressRoute

Een tweede ExpressRoute-circuit, in de nabijheid van dichter naar locatie 2 on-premises, verbindt de locatie 2 on-premises naar de hub VNet. De volgende schermafbeelding van de portal ziet u de tweede ExpressRoute-configuratie.

[![6]][6]

De volgende schermafbeelding van de portal ziet u de verbindingsconfiguratie tussen het tweede ExpressRoute-circuit en de Hub VNet.

[![7]][7]

De ExpressRoute1 maakt de Hub Vnet en de locatie-1 on-premises verbinding met een externe Vnet in een andere Azure-regio.

[![8]][8]

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

Zie voor het besturingselement vlak analyse van de test-setup en geeft inzicht in de weergaven van andere VNet/VLAN van de topologie, [Bedieningsvlak Analysis][Control-Analysis].

Zie voor analyse van gegevens vlak van de test-instellingen en voor Azure-netwerk functies controleweergaven [Gegevensvlak Analysis][Data-Analysis].

Zie voor meer informatie over het aantal ExpressRoute-circuits die u kunt verbinding maken met een ExpressRoute-Gateway of het aantal ExpressRoute-Gateways u verbinding met een ExpressRoute-circuit maken kunt, of voor meer informatie over andere schaallimieten van ExpressRoute, [Veelgestelde vragen over ExpressRoute][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "knooppunt van het VNet-VNet-peering"
[2]: ./media/backend-interoperability/HubVNet-peering.png "hub VNet van VNet-peering"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "VPN GW-configuratie van de vertakking VNet"
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute1 configuratie"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "Verbindingsconfiguratie van ExpressRoute1 naar de Hub VNet ExR GW"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute2 configuratie"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "Verbindingsconfiguratie van ExpressRoute2 naar de Hub VNet ExR GW"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "Verbindingsconfiguratie van ExpressRoute2 naar externe VNet ExR GW"

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




