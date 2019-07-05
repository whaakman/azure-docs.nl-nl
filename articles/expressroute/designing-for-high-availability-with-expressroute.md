---
title: Ontwerpen voor hoge beschikbaarheid met Azure ExpressRoute | Microsoft Docs
description: Deze pagina bevat architectuur aanbevelingen voor hoge beschikbaarheid tijdens het gebruik van Azure ExpressRoute.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: rambala
ms.openlocfilehash: 4984b30daf6170873cad9472bfed2d879af57efe
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466649"
---
# <a name="designing-for-high-availability-with-expressroute"></a>Ontwerpen voor hoge beschikbaarheid met ExpressRoute

ExpressRoute is ontworpen voor hoge beschikbaarheid voor de provider over geavanceerde privénetwerkverbinding met Microsoft-resources. Met andere woorden, is er geen single point of failure wordt in het pad ExpressRoute binnen Microsoft-netwerk. Als u wilt de beschikbaarheid te maximaliseren, moeten de klant en de serviceprovider-segment van uw ExpressRoute-circuit ook worden ontworpen voor hoge beschikbaarheid. In dit artikel, eerst laten we kijken aandachtspunten voor netwerken architectuur voor het bouwen van robuuste netwerkverbinding met behulp van een ExpressRoute en vervolgens laten we bekijken de nauwkeuriger functies die u helpen bij het verbeteren van de hoge beschikbaarheid van uw ExpressRoute-circuit.


## <a name="architecture-considerations"></a>Overwegingen voor architectuur

De volgende afbeelding ziet u de aanbevolen manier om verbinding maken met behulp van een ExpressRoute-circuit voor het optimaliseren van de beschikbaarheid van een ExpressRoute-circuit.

 [![1]][1]

Voor hoge beschikbaarheid is het essentieel voor het onderhouden van de redundantie van de ExpressRoute-circuit in de end-to-end-netwerk. Met andere woorden, u moet onderhouden redundantie in uw on-premises netwerk en redundantie al dan niet mogen binnendringen in uw serviceprovider-netwerk. Onderhoud van redundantie ten minste houdt in één storingspunt netwerkfouten vermijden. Verbeter de hoge beschikbaarheid met redundante voeding en koeling voor het netwerk die apparaten wordt verder.

### <a name="first-mile-physical-layer-design-considerations"></a>Eerste mijl fysieke laag overwegingen bij het ontwerpen

 Als u zowel de primaire en secundaire verbindingen van een ExpressRoute-circuits op de dezelfde klant Premises apparatuur (CPE) wordt beëindigd, bent u in gevaar brengt de hoge beschikbaarheid binnen uw on-premises netwerk. Als u zowel de primaire en secundaire verbindingen via dezelfde poort van een CPE (een door de twee verbindingen onder verschillende subinterfaces wordt beëindigd of door het samenvoegen van de twee verbindingen in het partner network) configureert, bent u ook de partner forceren voor hoge beschikbaarheid in hun-netwerksegment inbreuk. Deze inbreuk wordt weergegeven in de volgende afbeelding.

[![2]][2]

Anderzijds, als u de primaire en secundaire verbindingen van een ExpressRoute-circuits in verschillende geografische locaties beëindigen, kan klikt u vervolgens u worden inbreuk op de netwerkprestaties van de verbinding. Als verkeer actief gelijkmatig verdeeld zijn over de primaire en de secundaire verbindingen die worden beëindigd op verschillende geografische locaties, zou potentiële aanzienlijk verschil in de netwerklatentie tussen de twee paden leiden tot suboptimale netwerk prestaties. 

Zie voor overwegingen bij het ontwerpen van geografisch redundante, [ontwerpen voor herstel na noodgeval met ExpressRoute][DR].

### <a name="active-active-connections"></a>Actief-actief-verbindingen

Microsoft-netwerk is geconfigureerd voor gebruik van de primaire en secundaire verbindingen van ExpressRoute-circuits in de modus actief-actief. U kunt echter de redundante verbindingen van een ExpressRoute-circuit te werken in de modus actief-passief forceren via uw routeadvertenties. Advertentie-meer specifieke routes en BGP-padtoevoeging zijn de veelgebruikte methoden gebruikt voor het maken van één pad beter dan de andere.

Het is raadzaam om te hoge beschikbaarheid verbeteren, werken beide de verbindingen van een ExpressRoute-circuit in de modus actief-actief. Als u de verbindingen die werken in de modus actief-actief laat, wordt Microsoft-netwerk taakverdeling het verkeer tussen de verbindingen op basis van per-stroom.

De primaire en secundaire verbindingen van een ExpressRoute-circuit in de modus actief-passief face het risico van beide mislukt na een storing in het pad van de actieve verbindingen uitgevoerd. De meest voorkomende oorzaken van fouten op overschakelen via zijn gebrek aan actieve beheer van de passieve verbindingen en passieve verbindingen verouderde routes te adverteren.

U kunt ook de primaire en secundaire verbindingen van een ExpressRoute-circuit uitgevoerd in de modus actief-actief, resulteert in alleen ongeveer een halve stromen mislukken en het ophalen van omgeleid, een ExpressRoute-verbindingsfout te volgen. Modus actief-actief wordt dus aanzienlijk te verbeteren van de gemiddelde tijd om te herstellen (MTTR).

### <a name="nat-for-microsoft-peering"></a>NAT voor Microsoft-peering 

Microsoft-peering is ontworpen voor de communicatie tussen openbare eindpunten. Dus meestal zijn on-premises persoonlijke eindpunten netwerk adres omgezet (gestaan) met openbare IP-adres van de klant of partnernetwerk voordat ze met elkaar via Microsoft-peering communiceren. Ervan uitgaande dat u de primaire en secundaire verbindingen in de modus actief-actief, waar en hoe u NAT heeft een invloed op hoe snel u na een storing in een van de ExpressRoute-verbindingen herstellen. Twee verschillende NAT-opties worden weergegeven in de volgende afbeelding:

[![3]][3]

In de optie 1, NAT toegepast na de splitsing van het verkeer tussen de primaire en secundaire verbindingen van ExpressRoute. Onafhankelijke NAT-groepen worden gebruikt tussen de primaire en secundaire apparaten om te voldoen aan de vereisten voor stateful NAT-apparaat, zodat het retourverkeer op hetzelfde edge-apparaat waarmee de stroom egressed zou aankomen.

In de optie 2, wordt een algemene NAT-pool gebruikt voordat het splitsen van het verkeer tussen de primaire en secundaire verbindingen van ExpressRoute. Het is belangrijk om het verschil is dat de algemene NAT-pool voordat het verkeer splitsen betekent niet dat introductie van single point of failure waardoor inbreuk op hoge beschikbaarheid.

Mogelijkheid voor het bereiken van de bijbehorende NAT-adresgroep is met de optie 1, na een mislukte ExpressRoute-verbinding verbroken. Daarom alle de verbroken stromen moeten ook worden opnieuw tot stand gebracht door TCP of toepassingslaag de time-out voor de bijbehorende venster te volgen. Als een van de NAT-groepen worden gebruikt voor het front-end die een van de on-premises servers en als de bijbehorende verbinding mislukt, de on-premises servers naar Azure kunnen niet worden bereikt totdat de verbinding is opgelost.

Terwijl met de optie 2, de NAT bereikbaar is, zelfs nadat een primaire of secundaire verbindingsfout is. De netwerklaag zelf kan daarom worden de pakketten en help sneller herstel de fout te volgen omgeleid. 

> [!NOTE]
> Als u NAT-optie 1 (onafhankelijke NAT-groepen voor de primaire en secundaire ExpressRoute-verbindingen) en een poort van een IP-adres van een van de NAT-pool naar een on-premises server wijzen, de server zich niet bereikbaar is via de ExpressRoute-circuit wanneer de bijbehorende verbinding is mislukt.
> 

## <a name="fine-tuning-features-for-private-peering"></a>Functies voor persoonlijke peering aan te passen

In deze sectie laat het ons revisie optioneel (afhankelijk van uw Azure-implementatie en hoe gevoelig u bent aan MTTR) functies te verbeteren van hoge beschikbaarheid van uw ExpressRoute-circuit. We gaan Lees met name zone-bewuste implementatie van virtuele netwerkgateways voor ExpressRoute en in twee richtingen doorsturen van detectie (BFD).

### <a name="availability-zone-aware-expressroute-virtual-network-gateways"></a>Binnen een Beschikbaarheidszone op de hoogte ExpressRoute virtuele netwerkgateways

Een beschikbaarheidszone in een Azure-regio is een combinatie van een foutdomein en een updatedomein. Als u ervoor voor de zone-redundante Azure IaaS-implementatie kiezen, wilt u mogelijk ook configureren zone-redundante virtuele netwerkgateways die ExpressRoute-privépeering beëindigen. Zie voor meer informatie [over zone-redundante virtuele netwerkgateways in Azure-Beschikbaarheidszones][zone redundant vgw]. To configure zone-redundant virtual network gateway, see [Create a zone-redundant virtual network gateway in Azure Availability Zones][conf zone redundant vgw].

### <a name="improving-failure-detection-time"></a>Verbetering van de fout detectietijd

ExpressRoute ondersteunt BFD via persoonlijke peering. BFD vermindert detectietijd van de fout via het netwerk laag 2 tussen Microsoft Enterprise Edge (msee's) en de BGP-neighbors aan de on-premises van ongeveer drie minuten (standaardinstelling) tot minder dan een seconde. Snelle fout detectietijd helpt hastening foutherstel. Zie voor meer informatie [BFD configureren via ExpressRoute][BFD].

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt besproken hoe u kunt ontwerpen voor hoge beschikbaarheid van een ExpressRoute-circuit-connectiviteit. Een ExpressRoute-circuit peering punt is vastgemaakt aan een geografische locatie en daarom kan worden beïnvloed door een onherstelbare fout die van invloed is op de gehele locatie. 

Zie voor overwegingen bij het ontwerp aan het bouwen van geografisch redundante netwerkverbinding met de Microsoft-backbone dat bestand is tegen kritieke fouten optreden die van invloed op een hele regio, [ontwerpen voor herstel na noodgeval met persoonlijke peeringExpressRoute][DR].

<!--Image References-->
[1]: ./media/designing-for-high-availability-with-expressroute/exr-reco.png "aanbevolen manier om te verbinden met behulp van ExpressRoute"
[2]: ./media/designing-for-high-availability-with-expressroute/suboptimal-lastmile-connectivity.png "Suboptimal laatste mile-connectiviteit"
[3]: ./media/designing-for-high-availability-with-expressroute/nat-options.png "NAT-opties"


<!--Link References-->
[zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/about-zone-redundant-vnet-gateways
[conf zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/create-zone-redundant-vnet-gateway
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[BFD]: https://docs.microsoft.com/azure/expressroute/expressroute-bfd
[DR]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering




