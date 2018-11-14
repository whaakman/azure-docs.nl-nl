---
title: 'Interoperabiliteit in Azure back-end-connectiviteit-functies: gegevens vlak analyse | Microsoft Docs'
description: Dit artikel bevat de gegevens vlak analyse van de test-instellingen die u gebruiken kunt voor het analyseren van interoperabiliteit tussen ExpressRoute, een site-naar-site VPN- en virtueel netwerk in Azure-peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 8b9e5b2b073309f177fa0ce4bb2a2d08009a06ff
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614410"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-data-plane-analysis"></a>Interoperabiliteit in Azure back-end-connectiviteit-functies: gegevens vlak analyse

In dit artikel beschrijft de gegevens vlak analyse van de [instellingen testen][Setup]. U kunt ook bekijken de [setup Testconfiguratie] [ Configuration] en de [besturingselement vlak analysis] [ Control-Analysis] van de test-installatie.

Analyse van gegevens vlak onderzoekt het pad van de pakketten die van een lokale netwerk (LAN of een virtueel netwerk) naar een andere binnen een topologie passeren. Het gegevenspad tussen twee lokale netwerken is niet noodzakelijkerwijs symmetrisch. In dit artikel analyseren we daarom een pad doorsturen vanaf een lokaal netwerk naar een ander netwerk dat is gescheiden van de omgekeerde pad.

## <a name="data-path-from-the-hub-vnet"></a>Het gegevenspad van de hub VNet

### <a name="path-to-the-spoke-vnet"></a>Pad naar het knooppunt VNet

Peering op virtueel netwerk (VNet), emuleert netwerkfunctionaliteit de brug tussen de twee VNets die zijn gekoppeld. Traceroute uitvoer van een hub VNet met een virtuele machine in het knooppunt dat vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

De volgende afbeelding ziet u de verbinding met grafische weergave van de hub VNet en het knooppunt VNet vanuit het perspectief van Azure Network Watcher:


[![1]][1]

### <a name="path-to-the-branch-vnet"></a>Pad naar de vertakking VNet

Traceroute uitvoer van een hub VNet met een virtuele machine in de vertakking die vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

In deze traceroute is de eerste hop de VPN-gateway in de Azure VPN-Gateway van de hub VNet. De tweede hop is de VPN-gateway van de vertakking VNet. Het IP-adres van de VPN-gateway van de vertakking VNet wordt niet aangekondigd in de hub VNet. De derde hop is de virtuele machine op de vertakking VNet.

De volgende afbeelding ziet u de verbinding met grafische weergave van de hub VNet en de vertakking VNet vanuit het perspectief van Network Watcher:

[![2]][2]

Voor de verbinding ziet de volgende afbeelding u de rasterweergave in Network Watcher:

[![3]][3]

### <a name="path-to-on-premises-location-1"></a>Pad naar on-premises locatie 1

Traceroute uitvoer van een hub VNet met een virtuele machine in on-premises locatie 1 wordt hier weergegeven:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

In deze traceroute is de eerste hop het eindpunt voor Azure ExpressRoute-gateway-tunnel naar een Microsoft Enterprise Edge Router (MSEE). De tweede en derde hops zijn de router van de klant-edge (CE) en de on-premises locatie 1 LAN IP-adressen. Deze IP-adressen worden niet geadverteerd in de hub VNet. De vierde hop is de virtuele machine in de on-premises locatie 1.


### <a name="path-to-on-premises-location-2"></a>Pad naar on-premises locatie 2

Traceroute uitvoer van een hub VNet met een virtuele machine in on-premises locatie 2 wordt hier weergegeven:

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

In deze traceroute is de eerste hop het eindpunt van de ExpressRoute-gateway tunnel voor een MSEE. De tweede en derde hops zijn de CE-router en de on-premises locatie 2 LAN IP-adressen. Deze IP-adressen worden niet geadverteerd in de hub VNet. De vierde hop is de virtuele machine op de on-premises locatie 2.

### <a name="path-to-the-remote-vnet"></a>Pad naar het externe VNet

Traceroute uitvoer van een hub VNet met een virtuele machine in het externe VNet wordt hier weergegeven:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

In deze traceroute is de eerste hop het eindpunt van de ExpressRoute-gateway tunnel voor een MSEE. De tweede hop is van het externe VNet-gateway-IP. De tweede hop-IP-adresbereik is niet aangekondigd in de hub VNet. De derde hop is de virtuele machine op het externe VNet.

## <a name="data-path-from-the-spoke-vnet"></a>Gegevenspad van het knooppunt VNet

Het knooppunt VNet deelt de netwerk-weergave van de hub VNet. Het knooppunt VNet gebruikt de RAS-gateway-connectiviteit van de hub VNet via VNet-peering, alsof deze rechtstreeks verbonden met het knooppunt VNet.

### <a name="path-to-the-hub-vnet"></a>Pad naar de hub VNet

Traceroute uitvoer van het knooppunt VNet met een virtuele machine in de hub die vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet"></a>Pad naar de vertakking VNet

Traceroute uitvoer van het knooppunt VNet met een virtuele machine in de vertakking die vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

In deze traceroute is de eerste hop de VPN-gateway van de hub VNet. De tweede hop is de VPN-gateway van de vertakking VNet. Het IP-adres van de VPN-gateway van de vertakking VNet is niet binnen de/ster VNet aangekondigd. De derde hop is de virtuele machine op de vertakking VNet.

### <a name="path-to-on-premises-location-1"></a>Pad naar on-premises locatie 1

Traceroute uitvoer van het knooppunt VNet met een virtuele machine in on-premises locatie 1 wordt hier weergegeven:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

In deze traceroute is de eerste hop van de hub VNet ExpressRoute-gateway-tunneleindpunt voor een MSEE. De tweede en derde hops zijn de CE-router en de on-premises locatie 1 LAN IP-adressen. Deze IP-adressen worden niet geadverteerd in de hub/knooppunt VNet. De vierde hop is de virtuele machine in de on-premises locatie 1.

### <a name="path-to-on-premises-location-2"></a>Pad naar on-premises locatie 2

Traceroute uitvoer van het knooppunt VNet met een virtuele machine in on-premises locatie 2 wordt hier weergegeven:


    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

In deze traceroute is de eerste hop van de hub VNet ExpressRoute-gateway-tunneleindpunt voor een MSEE. De tweede en derde hops zijn de CE-router en de on-premises locatie 2 LAN IP-adressen. Deze IP-adressen worden niet in het hub/knooppunt VNets geadverteerd. De vierde hop is de virtuele machine in de on-premises locatie 2.

### <a name="path-to-the-remote-vnet"></a>Pad naar het externe VNet

Traceroute uitvoer van het knooppunt VNet met een virtuele machine in het externe VNet wordt hier weergegeven:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

In deze traceroute is de eerste hop van de hub VNet ExpressRoute-gateway-tunneleindpunt voor een MSEE. De tweede hop is van het externe VNet-gateway-IP. De tweede hop-IP-adresbereik niet wordt in de hub/knooppunt VNet geadverteerd. De derde hop is de virtuele machine op het externe VNet.

## <a name="data-path-from-the-branch-vnet"></a>Het gegevenspad van de VNet-vertakking

### <a name="path-to-the-hub-vnet"></a>Pad naar de hub VNet

Traceroute is de uitvoer van de VNet-vertakking naar een virtuele machine in de hub die vnet wordt hier weergegeven:

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

In deze traceroute is de eerste hop de VPN-gateway van de vertakking VNet. De tweede hop is de VPN-gateway van de hub VNet. Het IP-adres van de VPN-gateway van de hub VNet wordt niet aangekondigd in het externe VNet. De derde hop is de virtuele machine op de hub VNet.

### <a name="path-to-the-spoke-vnet"></a>Pad naar het knooppunt VNet

Traceroute is de uitvoer van de VNet-vertakking naar een virtuele machine in het knooppunt dat vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

In deze traceroute is de eerste hop de VPN-gateway van de vertakking VNet. De tweede hop is de VPN-gateway van de hub VNet. Het IP-adres van de VPN-gateway van de hub VNet wordt niet aangekondigd in het externe VNet. De derde hop is de virtuele machine op het knooppunt VNet.

### <a name="path-to-on-premises-location-1"></a>Pad naar on-premises locatie 1

Traceroute uitvoer van de vertakking VNet met een virtuele machine in on-premises locatie 1 wordt hier weergegeven:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

In deze traceroute is de eerste hop de VPN-gateway van de vertakking VNet. De tweede hop is de VPN-gateway van de hub VNet. Het IP-adres van de VPN-gateway van de hub VNet wordt niet aangekondigd in het externe VNet. De derde hop is het VPN-tunnel beëindiging punt op de primaire CE-router. De vierde hop is een interne IP-adres van de on-premises locatie 1. Dit LAN-IP-adres is niet buiten de router CE aangekondigd. De vijfde hop is de bestemming virtuele machine in de on-premises locatie 1.

### <a name="path-to-on-premises-location-2-and-the-remote-vnet"></a>Pad naar het on-premises locatie 2 en het externe VNet

Zoals in het besturingselement vlak-analyse besproken, heeft de vertakking VNet geen zichtbaarheid in on-premises locatie 2 of met het externe VNet per configuratie van het netwerk. De volgende ping-resultaat is bevestigen: 

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

## <a name="data-path-from-on-premises-location-1"></a>Gegevenspad van on-premises locatie 1

### <a name="path-to-the-hub-vnet"></a>Pad naar de hub VNet

Traceroute van on-premises locatie 1 worden uitgevoerd naar een virtuele machine in de hub die vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

In deze traceroute zijn de eerste twee hops onderdeel van de on-premises netwerk. De derde hop is de primaire MSEE-interface waarmee de router CE gezichten. De vierde hop is de ExpressRoute-gateway van de hub VNet. Het IP-adresbereik van het ExpressRoute-gateway van de hub VNet wordt niet geadverteerd naar de on-premises netwerk. De vijfde hop is de bestemming VM.

Network Watcher biedt alleen een Azure-georiënteerde weergave. Voor het perspectief van een on-premises gebruiken we Azure Network Performance Monitor. Network Performance Monitor biedt agents die u op servers in netwerken buiten Azure voor gegevensanalyse van het pad installeren kunt.

De volgende afbeelding toont de Topologieweergave van de on-premises locatie 1 VM-connectiviteit met de virtuele machine op de hub VNet via ExpressRoute:

[![4]][4]

Zoals eerder besproken, gebruikt de instellingen voor een site-naar-site-VPN als back-connectiviteit voor ExpressRoute tussen de on-premises locatie-1 en de hub VNet. Als u wilt testen van het pad van de back-upgegevens, laten we u een ExpressRoute-koppeling communicatiefout tussen de on-premises locatie 1 primaire CE-router en de bijbehorende MSEE veroorzaken. Als u wilt een ExpressRoute-koppeling-fout veroorzaken, sluit u de CE-interface die de MSEE gezichten:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

De volgende afbeelding toont de Topologieweergave van de on-premises locatie 1 VM-connectiviteit met de virtuele machine op de hub VNet via site-naar-site VPN-verbinding bij het ExpressRoute-connectiviteit is niet beschikbaar:

[![5]][5]

### <a name="path-to-the-spoke-vnet"></a>Pad naar het knooppunt VNet

Traceroute van on-premises locatie 1 worden uitgevoerd naar een virtuele machine in het knooppunt dat vnet wordt hier weergegeven:

We brengen de primaire ExpressRoute-verbinding met het doen van de gegevensanalyse-pad naar het knooppunt VNet:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Breng de primaire 1 ExpressRoute-connectiviteit voor de rest van de gegevensanalyse-pad.

### <a name="path-to-the-branch-vnet"></a>Pad naar de vertakking VNet

Traceroute van on-premises locatie 1 worden uitgevoerd naar een virtuele machine in de vertakking die vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

### <a name="path-to-on-premises-location-2"></a>Pad naar on-premises locatie 2

Zoals besproken de [besturingselement vlak analysis][Control-Analysis], de on-premises locatie 1 heeft geen zichtbaarheid in on-premises locatie 2 per configuratie van het netwerk. De volgende ping-resultaat is bevestigen: 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

### <a name="path-to-the-remote-vnet"></a>Pad naar het externe VNet

Traceroute uitvoer van on-premises locatie 1 naar een virtuele machine in het externe VNet wordt hier weergegeven:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

## <a name="data-path-from-on-premises-location-2"></a>Gegevenspad van on-premises locatie 2

### <a name="path-to-the-hub-vnet"></a>Pad naar de hub VNet

Traceroute van on-premises locatie 2 worden uitgevoerd naar een virtuele machine in de hub die vnet wordt hier weergegeven:

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-spoke-vnet"></a>Pad naar het knooppunt VNet

Traceroute van on-premises locatie 2 worden uitgevoerd naar een virtuele machine in het knooppunt dat vnet wordt hier weergegeven:

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet-on-premises-location-1-and-the-remote-vnet"></a>Pad naar de vertakking VNet, on-premises locatie 1, en het externe VNet

Zoals besproken de [besturingselement vlak analysis][Control-Analysis], de on-premises locatie 1 heeft geen zichtbaarheid op de vertakking VNet, on-premises locatie 1 of met het externe VNet per configuratie van het netwerk. 

## <a name="data-path-from-the-remote-vnet"></a>Het gegevenspad van de externe VNet

### <a name="path-to-the-hub-vnet"></a>Pad naar de hub VNet

Traceroute is de uitvoer van het externe VNet naar een virtuele machine in de hub die vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-spoke-vnet"></a>Pad naar het knooppunt VNet

Traceroute is de uitvoer van het externe VNet naar een virtuele machine in het knooppunt dat vnet wordt hier weergegeven:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet-and-on-premises-location-2"></a>Pad naar de vertakking VNet en on-premises locatie 2

Zoals besproken de [besturingselement vlak analysis][Control-Analysis], het externe VNet heeft geen zichtbaarheid op de vertakking VNet of op on-premises locatie 2 per configuratie van het netwerk. 

### <a name="path-to-on-premises-location-1"></a>Pad naar on-premises locatie 1

Traceroute uitvoer van het externe VNet met een virtuele machine in on-premises locatie 1 wordt hier weergegeven:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


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

Zie de [Veelgestelde vragen over ExpressRoute] [ ExR-FAQ] aan:
-   Meer informatie over het aantal ExpressRoute-circuits die u verbinding met een ExpressRoute-gateway maken kunt.
-   Meer informatie over het aantal ExpressRoute-gateways die u verbinding met een ExpressRoute-circuit maken kunt.
-   Meer informatie over andere schaallimieten van ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "network Watcher-weergave van de verbinding van een hub VNet met een knooppunt VNet"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "network Watcher-weergave van de verbinding van een hub VNet met een vertakking VNet"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "rasterweergave network Watcher van de verbinding van een hub VNet aan een vertakking VNet"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "network Performance Monitor weergave van de verbinding van de locatie 1 virtuele machine naar de hub VNet via ExpressRoute 1"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "weergave van de verbinding van de locatie 1 virtuele machine naar de hub VNet via een site-naar-site-VPN network Performance Monitor"

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


