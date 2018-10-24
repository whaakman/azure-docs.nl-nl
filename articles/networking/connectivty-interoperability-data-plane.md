---
title: 'Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - gegevens vlak analyse: Azure back-end-connectiviteit biedt interoperabiliteit | Microsoft Docs'
description: Deze pagina bevat de gegevens vlak analyse van de test-instelling die is gemaakt voor het analyseren van de interoperabiliteit van functies voor ExpressRoute, Site-naar-site VPN- en VNet-Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: c9f3824b1e0f44338696ba3c2e434d60eee3af8b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947213"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---data-plane-analysis"></a>Interoperabiliteit van ExpressRoute, Site-naar-site VPN- en VNet-Peering - analyse van gegevens vlak

In dit artikel, we gaan door de gegevens vlak analyse van de installatie testen. De Test-instellingen, Zie de [Test Setup][Setup]. De details van de configuratie van instellingen, Zie [Setup Testconfiguratie][Configuration]. Besturingselement vlak analyse van de test-instellingen, Zie [besturingselement vlak Analysis][Control-Analysis].

Analyse van gegevens vlak onderzoekt het pad dat wordt gebruikt door pakketten doorlopen van een lokale netwerk (LAN/VNet) naar een andere binnen een topologie. Het gegevenspad tussen twee lokale netwerken zijn mogelijk niet per se symmetrisch. Daarom in dit artikel gaan we doorsturen pad van een lokaal netwerk naar een andere los van de omgekeerde pad te analyseren.

##<a name="data-path-from-hub-vnet"></a>Het gegevenspad van de Hub VNet

###<a name="path-to-spoke-vnet"></a>Pad naar het knooppunt VNet

VNet-peering emuleert netwerkbrug functionaliteit tussen de twee VNets die zijn gekoppeld. Traceroute uitvoer van een Hub VNet met een virtuele machine in het knooppunt VNet vindt u hieronder:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

Het volgende scherm clip is de verbinding grafische weergave van de Hub VNet en het knooppunt VNet gepresenteerd door Azure Network Watcher:


[![1]][1]

###<a name="path-to-branch-vnet"></a>Pad naar de vertakking VNet

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

In de bovenstaande traceroute is de eerste hop de GW VPN van de Hub VNet. De tweede hop is de GW VPN van de vertakking VNet, waarvan het IP-adres niet binnen de Hub VNet wordt aangekondigd. De derde hop is de virtuele machine op de vertakking VNet.

Het volgende scherm clip is de verbinding grafische weergave van de Hub VNet en de vertakking VNet gepresenteerd door Azure Network Watcher:

[![2]][2]

Volgende scherm clip is voor de verbinding, de rasterweergave gepresenteerd door Azure Network Watcher:

[![3]][3]

###<a name="path-to-on-premises-location-1"></a>Pad naar On-Premises locatie-1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

In de bovenstaande traceroute is de eerste hop het eindpunt van de tunnel ExpressRoute GW voor MSEE. De tweede en de derde hop respectievelijk CE-router en on-premises locatie 1 LAN IP-adressen, deze IP-adressen zijn niet worden verzonden in de Hub VNet. De vierde hop is de virtuele machine op de on-premises locatie-1.


###<a name="path-to-on-premises-location-2"></a>Pad naar On-Premises locatie-2

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

In de bovenstaande traceroute is de eerste hop het eindpunt van de tunnel ExpressRoute GW voor MSEE. De tweede en de derde hop respectievelijk CE-router en on-premises locatie 2 LAN IP-adressen, deze IP-adressen zijn niet worden verzonden in de Hub VNet. De vierde hop is de virtuele machine op de on-premises locatie-2.

###<a name="path-to-remote-vnet"></a>Pad naar externe VNet

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

In de bovenstaande traceroute is de eerste hop het eindpunt van de tunnel ExpressRoute GW voor MSEE. De tweede hop is van het externe VNet GW IP. De tweede hop-IP-adresbereik wordt niet binnen de Hub VNet aangekondigd. De derde hop is de virtuele machine op het externe VNet.

##<a name="data-path-from-spoke-vnet"></a>Het gegevenspad van knooppunt VNet

Intrekken dat de netwerk-weergave van de Hub VNet wordt gedeeld door het knooppunt VNet. Het knooppunt VNet gebruikt de RAS-gateway-connectiviteit van de hub VNet via VNet-peering, alsof ze rechtstreeks zijn verbonden met het knooppunt VNet.

###<a name="path-to-hub-vnet"></a>Pad naar de Hub VNet

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

###<a name="path-to-branch-vnet"></a>Pad naar de vertakking VNet

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

In de bovenstaande traceroute is de eerste hop de GW VPN van de Hub VNet. De tweede hop is de GW VPN van de vertakking VNet, waarvan het IP-adres niet binnen het VNet/ster wordt aangekondigd. De derde hop is de virtuele machine op de vertakking VNet.

###<a name="path-to-on-premises-location-1"></a>Pad naar On-Premises locatie-1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

In de bovenstaande traceroute is de eerste hop van de Hub VNet ExpressRoute GW-tunnel-eindpunt voor MSEE. Deze IP-adressen, dat is de tweede en de derde hop respectievelijk CE-router en on-premises locatie 1 LAN IP-adressen zijn, worden niet geadverteerd binnen het VNet/ster. De vierde hop is de virtuele machine op de on-premises locatie-1.

###<a name="path-to-on-premises-location-2"></a>Pad naar On-Premises locatie-2

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

In de bovenstaande traceroute is de eerste hop van de Hub VNet ExpressRoute GW-tunnel-eindpunt voor MSEE. De tweede en de derde hop respectievelijk CE-router en on-premises locatie 2 LAN IP-adressen, deze IP-adressen zijn niet worden verzonden in de Hub/Spoke-VNets. De vierde hop is de virtuele machine op de on-premises locatie-2.

###<a name="path-to-remote-vnet"></a>Pad naar externe VNet

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

In de bovenstaande traceroute is de eerste hop van de Hub VNet ExpressRoute GW-tunnel-eindpunt voor MSEE. De tweede hop is van het externe VNet GW IP. De tweede hop-IP-adresbereik wordt niet binnen het VNet/ster aangekondigd. De derde hop is de virtuele machine op het externe VNet.

##<a name="data-path-from-branch-vnet"></a>Het gegevenspad van Branch VNet

###<a name="path-to-hub-vnet"></a>Pad naar de Hub VNet

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

In de bovenstaande traceroute is de eerste hop de GW VPN van de VNet-vertakking. De tweede hop is de GW VPN van de Hub VNet, waarvan het IP-adres is niet binnen het externe VNet worden verzonden. De derde hop is de virtuele machine op de Hub VNet.

###<a name="path-to-spoke-vnet"></a>Pad naar het knooppunt VNet

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

In de bovenstaande traceroute is de eerste hop de GW VPN van de VNet-vertakking. De tweede hop is de GW VPN van de Hub VNet, waarvan het IP-adres is niet binnen het externe VNet aangekondigd, en de derde hop is de virtuele machine op het knooppunt VNet.

###<a name="path-to-on-premises-location-1"></a>Pad naar On-Premises locatie-1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

In de bovenstaande traceroute is de eerste hop de GW VPN van de VNet-vertakking. De tweede hop is de GW VPN van de Hub VNet, waarvan het IP-adres is niet binnen het externe VNet worden verzonden. De derde hop is het VPN-tunnel beëindiging punt op de primaire CE-router. De vierde hop is een interne IP-adres van de on-premises locatie-1 LAN IP-adres dat niet wordt aangekondigd buiten CE-router. De vijfde hop is de bestemming virtuele machine op de on-premises locatie-1.

###<a name="path-to-on-premises-location-2-and-remote-vnet"></a>Pad naar On-Premises locatie 2 en externe VNet

Zoals voorafgaande in het besturingselement vlak-analyse besproken, heeft de vertakking VNet geen zichtbaarheid naar on-premises locatie-2 of naar externe VNet per configuratie van het netwerk. De volgende ping-resultaten controleert u of het feit. 

    C:\Users\rb>ping 10.1.31.10

    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

    C:\Users\rb>ping 10.17.30.4

    Pinging 10.17.30.4 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.17.30.4:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

##<a name="data-path-from-on-premises-location-1"></a>Gegevenspad van On-Premises locatie-1

###<a name="path-to-hub-vnet"></a>Pad naar de Hub VNet

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

In de bovenstaande traceroute zijn de eerste twee hops onderdeel van het On-Premises netwerk. De derde hop is de primaire MSEE-interface naar de router CE gericht. De vierde hop is ExpressRoute G/W van de hub VNet, met IP-adresbereik niet wordt geadverteerd voor het On-Premises netwerk. De vijfde hop is de bestemming VM.

De Azure Network Watcher biedt alleen Azure-georiënteerde weergave. Daarom hebben we voor on-premises georiënteerde weergave Azure (Netwerkprestatiemeter) gebruikt. NPM biedt agents die kunnen worden geïnstalleerd servers in het netwerk buiten Azure en pad naar de data-analyse.

Het volgende scherm clip is de Topologieweergave van de on-premises locatie-1 VM-connectiviteit met de virtuele machine op de hub VNet via ExpressRoute.

[![4]][4]

Zoals eerder vermeld, de installatie van de test maakt gebruik van Site-naar-Site-VPN als back-connectiviteit voor ExpressRoute tussen on-premises locatie-1 en de Hub VNet. Als u wilt testen back gegevenspad, laten we u een ExpressRoute-koppeling communicatiefout tussen on-premises locatie-1 primaire CE-router en de bijbehorende MSEE veroorzaken door het afsluiten van de CE-interface gericht de MSEE.

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

Het volgende scherm clip is de Topologieweergave van de on-premises locatie-1 VM-connectiviteit met de virtuele machine op de hub VNet via Site-naar-Site VPN-verbinding als de ExpressRoute-verbinding niet actief is.

[![5]][5]

###<a name="path-to-spoke-vnet"></a>Pad naar het knooppunt VNet

We brengen de primaire ExpressRoute-connectiviteit de analyse van het gegevenspad voor knooppunt VNet.

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Laat het ons doen om de primaire ExpressRoute-1-connectiviteit voor de rest van het gegevenspad-analyse.

###<a name="path-to-branch-vnet"></a>Pad naar de vertakking VNet

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

###<a name="path-to-on-premises-location-2"></a>Pad naar On-Premises locatie-2

Zoals voorafgaande in het besturingselement vlak-analyse besproken, heeft het on-premises locatie-1 geen zichtbaarheid van on-premises locatie-2 per configuratie van het netwerk. De volgende ping-resultaten controleert u of het feit. 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

###<a name="path-to-remote-vnet"></a>Pad naar externe VNet

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

##<a name="data-path-from-on-premises-location-2"></a>Gegevenspad van On-Premises locatie-2

###<a name="path-to-hub-vnet"></a>Pad naar de Hub VNet

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Pad naar het knooppunt VNet

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

###<a name="path-to-branch-vnet-on-premises-location-1-and-remote-vnet"></a>Pad naar het VNet, On-Premises locatie-1 en externe VNet vertakking

Zoals we besproken voorafgaande in het besturingselement vlak-analyse, het on-premises locatie-1 heeft geen zichtbaarheid van branch VNet, on-premises locatie-1, en externe VNet per configuratie van het netwerk. 

##<a name="data-path-from-remote-vnet"></a>Het gegevenspad van externe VNet

###<a name="path-to-hub-vnet"></a>Pad naar de Hub VNet

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Pad naar het knooppunt VNet

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-branch-vnet-and-on-premises-location-2"></a>Pad naar de vertakking VNet en On-Premises locatie-2

Zoals voorafgaande in het besturingselement vlak-analyse besproken, heeft het externe VNet geen zichtbaarheid vertakking VNet, en on-premises locatie-2 per configuratie van het netwerk. 


### <a name="path-to-on-premises-location-1"></a>Pad naar On-Premises locatie-1

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="further-reading"></a>Lees hier meer over

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>In combinatie met behulp van ExpressRoute en Site-naar-Site VPN-verbinding

####<a name="site-to-site-vpn-over-expressroute"></a>Site-naar-Site-VPN via ExpressRoute

Site-naar-Site VPN kan worden geconfigureerd via ExpressRoute-Microsoft-peering voor het uitwisselen van gegevens tussen uw on-premises netwerk en uw Azure-VNets privé met vertrouwen te behandelen, anti opnieuw afspelen, echtheid en integriteit. Zie voor meer informatie over het configureren van Site-naar-Site IPSec VPN-in-tunnelmodus via ExpressRoute-Microsoft-peering [Site-naar-site-VPN via het ExpressRoute-Microsoft-peering][S2S-Over-ExR]. 

De belangrijkste beperking van het configureren van S2S VPN via Microsoft-peering is de doorvoer. Doorvoer via de IPSec-tunnel wordt beperkt door de VPN-GW-capaciteit. De VPN-GW-doorvoer is dat kleiner vergeleken met de doorvoer van ExpressRoute. In dergelijke scenario's wilt met behulp van de IPSec-tunnel voor hoge beveiligd verkeer en privépeering voor alle andere verkeer helpen de ExpressRoute-bandbreedtegebruik optimaliseren.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>Site-naar-Site-VPN als een beveiligd failoverpad voor ExpressRoute
ExpressRoute wordt aangeboden als paar redundante circuit om hoge beschikbaarheid te garanderen. U kunt geo-redundante ExpressRoute-connectiviteit configureren in verschillende Azure-regio's. Ook zoals in de instellingen van de test wordt uitgevoerd binnen een bepaald Azure-regio, kunt als u wilt dat een failoverpad voor ExpressRoute-connectiviteit u doen met behulp van Site-naar-Site VPN. Wanneer dezelfde voorvoegsels worden geadverteerd via ExpressRoute- en S2S VPN-, Azure geeft de voorkeur aan ExpressRoute via S2S VPN-verbinding. Om te voorkomen dat asymmetrische routering tussen ExpressRoute als S2S-VPN, on-premises netwerkconfiguratie moet ook hyperlink voorkeur ExpressRoute via een S2S-VPN-verbinding.

Zie voor meer informatie over het configureren van ExpressRoute en Site-naar-Site VPN-verbindingen [ExpressRoute en Site-naar-Site co-existentie][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Back-end-connectiviteit met het knooppunt VNets en de vertakking locaties uitbreiden

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Knooppunt-VNet-connectiviteit met behulp van VNet-peering

Hub en spoke-Vnet-architectuur wordt veel gebruikt. De hub is een virtueel netwerk (VNet) in Azure die als een centraal punt van connectiviteit tussen uw knooppunt VNets en naar uw on-premises netwerk fungeert. De spaken zijn VNets die zijn gekoppeld aan de hub en kunnen worden gebruikt om workloads te isoleren. Verkeer stroomt tussen het on-premises datacenter en de hub via een ExpressRoute of VPN-verbinding. Zie voor meer informatie over de architectuur [Hub en Spoke-architectuur][Hub-n-Spoke]

VNet-peering binnen een regio kunt knooppunt VNets hub VNet-gateways (VPN- en ExpressRoute-gateways) gebruiken om te communiceren met externe netwerken.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>VNet-connectiviteit met behulp van Site-naar-Site VPN vertakking

Als u wilt dat de vertakking vnet's (in verschillende regio's) en on-premises netwerken met elkaar via een hub vnet communiceren, is de systeemeigen Azure-oplossing site-naar-site VPN-connectiviteit via VPN. Een alternatief is het gebruik van een NVA voor routering in de hub.

Zie voor het configureren van VPN-gateways, [configureren van VPN-Gateway][VPN]. Zie voor het implementeren van maximaal beschikbare NVA [NVA maximaal beschikbaar implementeren][Deploy-NVA].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het aantal ExpressRoute-circuits die u kunt verbinding maken met een ExpressRoute-Gateway of het aantal ExpressRoute-Gateways u verbinding met een ExpressRoute-circuit maken kunt, of voor meer informatie over andere schaallimieten van ExpressRoute, [Veelgestelde vragen over ExpressRoute][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "network Watcher-weergave van de verbinding van de Hub VNet naar VNet knooppunt"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "network Watcher-weergave van de verbinding van de Hub VNet naar VNet vertakking"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "rasterweergave network Watcher van de verbinding van de Hub VNet naar VNet vertakking"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "network Performance Monitor weergave van de verbinding van locatie-1-virtuele machine met de Hub VNet via ExpressRoute 1"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "weergave van de verbinding van locatie-1-virtuele machine met de Hub VNet via S2S VPN network Performance Monitor"

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




