---
title: Wat is Azure Standard Load Balancer?
titlesuffix: Azure Load Balancer
description: Overzicht van Azure Standard Load Balancer-functies
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/11/2019
ms.author: kumud
ms.openlocfilehash: 2eb2fbb1d184bf58923748278d4989a271adf434
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352577"
---
# <a name="azure-standard-load-balancer-overview"></a>Overzicht van Azure Standard Load Balancer

Azure Load Balancer kunt u uw toepassingen schalen en hoge beschikbaarheid voor uw services te maken. Load Balancer kan worden gebruikt voor zowel inkomende als uitgaande scenario's en lage latentie, hoge doorvoer, en kan worden geschaald tot miljoenen stromen voor alle TCP en UDP-toepassingen. 

In dit artikel is gericht op Standard Load Balancer.  Voor een meer algemeen overzicht voor Azure Load Balancer, raadpleegt u [overzicht van Load Balancer](load-balancer-overview.md) ook.

## <a name="what-is-standard-load-balancer"></a>Wat is een Standard Load Balancer?

Standard Load Balancer is een nieuw product in de Load Balancer voor alle TCP en UDP-toepassingen met een uitgebreide en gedetailleerdere functieset ten opzichte van Basic Load Balancer.  Hoewel er veel overeenkomsten, is het belangrijk om vertrouwd te raken met de verschillen in dit artikel wordt beschreven.

U kunt Standard Load Balancer gebruiken als een openbare of interne Load Balancer. En een virtuele machine kunnen worden verbonden met een openbare en een interne Load Balancer-resource.

Functies van de Load Balancer-resource worden altijd uitgedrukt als een frontend, een regel, een statustest en de definitie van een back-end-pool.  Een resource kan meerdere regels bevatten. U kunt virtuele machines in de back-endpool plaatsen door de back-endpool van de virtuele machine NIC resource op te geven.  Deze parameter is doorgegeven via het netwerkprofiel en bij het gebruik van virtuele-machineschaalsets uitgevouwen.

Een belangrijk aspect is het bereik van het virtuele netwerk voor de resource.  Hoewel Basic Load Balancer binnen het bereik van een beschikbaarheidsset bestaat, een Standard Load Balancer is volledig geïntegreerd met het bereik van een virtueel netwerk en alle virtuele netwerkconcepten zijn van toepassing.

Resources voor Load Balancer zijn waarin u hoe Azure de infrastructuur met meerdere tenants voor het scenario dat u wilt maken moet programma kunt express-objecten.  Er is geen directe relatie tussen resources voor Load Balancer en de werkelijke infrastructuur; het maken van een Load Balancer-exemplaar niet maken, capaciteit is altijd beschikbaar en er zijn geen lid is of schalen van vertragingen te houden. 

>[!NOTE]
> Azure biedt een reeks volledig beheerde oplossingen voor uw scenario's voor taakverdeling.  Als u voor beëindiging van TLS ('SSL-offload') of per toepassing HTTP/HTTPS-laag aanvraagverwerking zoeken wilt, raadpleegt u [Application Gateway](../application-gateway/application-gateway-introduction.md).  Raadpleeg [Traffic Manager](../traffic-manager/traffic-manager-overview.md) als u op zoek bent naar wereldwijde DNS-taakverdeling.  Uw end-to-end scenario's kunnen profiteren van deze oplossingen worden gecombineerd als nodig is.

## <a name="why-use-standard-load-balancer"></a>Waarom Standard Load Balancer gebruiken?

Met behulp van Standard Load Balancer kunt u de schaal van uw toepassingen aanpassen en hoge beschikbaarheid realiseren voor kleinschalige implementaties tot grote en complexe architecturen met meerdere zones.

Bekijk de onderstaande tabel voor een overzicht van de verschillen tussen Standard Load Balancer en Basic Load Balancer:

>[!NOTE]
> Nieuwe ontwerpen moeten Standard Load Balancer kunnen faciliteren. 

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Beoordeling [Servicelimieten voor Load Balancer](https://aka.ms/lblimits), evenals [prijzen](https://aka.ms/lbpricing), en [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Back-endpool

Standard Load Balancer-back-end-pools uitbreiden naar de bron van elke virtuele machine in een virtueel netwerk.  Mag maximaal 1000 back-end-exemplaren.  Een back-end-exemplaar is een IP-configuratie een eigenschap van een NIC-bron is.

De back-endpool kan bevatten zelfstandige virtuele machines, beschikbaarheidssets of virtuele-machineschaalsets.  U kunt ook de resources in de back-endpool combineren. U kunt maximaal 150 resources in de back-endpool per Load Balancer-resource combineren.

Wanneer u overweegt over het ontwerpen van uw back-endgroep, u kunt ontwerpen voor het minste aantal resources in de afzonderlijke back-end nog verder verbeteren van de duur van beheerbewerkingen.  Er is geen verschil in gegevens vlak prestaties of schaal.

### <a name="probes"></a>Statuscontroles
  
Standard Load Balancer voegt ondersteuning toe voor [HTTPS statuscontroles](load-balancer-custom-probe-overview.md#httpprobe) (HTTP-test met Transport Layer Security (TLS)-wrapper) voor het bewaken van uw toepassingen HTTPS nauwkeurig.  

Bovendien, als de volledige back-endpool [tests omlaag](load-balancer-custom-probe-overview.md#probedown), Standard Load Balancer kunt u alle tot stand gebrachte TCP-verbindingen om door te gaan. (Basic Load Balancer wordt beëindigd als alle TCP-verbindingen op alle exemplaren).

Beoordeling [Load Balancer statuscontroles](load-balancer-custom-probe-overview.md) voor meer informatie.

### <a name="az"></a>Beschikbaarheidszones

Standard Load Balancer biedt ondersteuning voor aanvullende mogelijkheden in regio's waar Beschikbaarheidszones beschikbaar zijn.  Deze functies zijn alle Standard Load Balancer biedt.  Beschikbaarheidszones configuraties zijn beschikbaar voor openbare en interne Standard Load Balancer.

Niet-zonegebonden front-ends worden zone-redundante door wanneer ze worden geïmplementeerd in een regio met Beschikbaarheidszones.   Een zone-redundante front blijft zone fout en wordt aangeboden door toegewezen infrastructuur in alle zones tegelijkertijd. 

Bovendien kunt u een frontend naar een specifieke zone garanderen. Een zonegebonden front-gedrag deelt met de bijbehorende zone en wordt aangeboden door toegewezen infrastructuur in een enkele zone.

Zoneoverschrijdende load balancing is beschikbaar voor de back-endpool en de bron van elke virtuele machine in een vnet kan deel uitmaken van een back-endpool.

Beoordeling [gedetailleerde bespreking van de Beschikbaarheidszones gerelateerde mogelijkheden](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a> Diagnostische gegevens

Standard Load Balancer biedt multi-dimensionale metrische gegevens via Azure Monitor.  Deze metrische gegevens kunnen worden gefilterd, gegroepeerd en uitgesplitst voor een opgegeven dimensie.  Beschikt over de huidige en historische informatie over prestaties en status van uw service.  Resource Health wordt ook ondersteund.  Hieronder volgt een kort overzicht van ondersteunde diagnostische gegevens:

| Gegevens | Description |
| --- | --- |
| VIP-beschikbaarheid | Standard Load Balancer oefeningen continu het gegevenspad van binnen een regio aan de Load Balancer front-end helemaal tot aan de SDN-stack die ondersteuning biedt voor uw virtuele machine. Als in orde exemplaren blijven, volgt de meting hetzelfde pad als verkeer met netwerktaakverdeling van uw toepassing. Het gegevenspad die wordt gebruikt door uw klanten wordt ook gevalideerd. De meting is zichtbaar voor uw toepassing en niet van invloed op andere bewerkingen.|
| Beschikbaarheid van DIP | Standard Load Balancer maakt gebruik van een gedistribueerde health service de status van het toepassingseindpunt van uw op basis van uw configuratie-instellingen controleert te scannen. Met deze metriek bevat een statistische functie of groep per eindpunt gefilterd-weergave van alle afzonderlijke gevallen-eindpunten in de Load Balancer.  U kunt zien hoe de Load Balancer de status van uw toepassing, zoals aangegeven door de configuratie van de health test bekijkt.
| SYN-pakketten | Standard Load Balancer niet TCP-verbindingen wordt beëindigd of ermee TCP of UDP-pakketten stromen. Stromen en hun aantal-handshakes zijn altijd tussen de bron en het VM-exemplaar. Als u wilt uw TCP-protocol-scenario's op te lossen, kunt u het gebruik van SYN pakketten items om te begrijpen hoeveel TCP-verbinding pogingen worden uitgevoerd. De metrische gegevens rapporteert het aantal TCP SYN-pakketten die zijn ontvangen.|
| SNAT-verbindingen | Standard Load Balancer rapporteert het aantal uitgaande stromen die zijn masqueraded naar het front-end-openbare IP-adres. SNAT poorten vormen een onuitputtelijk resource. Met deze metriek krijgt een indicatie van de mate waarin uw toepassing wordt afhankelijk zijn van SNAT voor uitgaande stromen.  Tellers voor geslaagde en mislukte uitgaande SNAT-stromen worden gerapporteerd en kunnen worden gebruikt om problemen op te begrijpen van de status van uw uitgaande stromen.|
| Byte-prestatiemeteritems | Standard Load Balancer rapporten de gegevens verwerkte per front-end.|
| Pakket-prestatiemeteritems | Standard Load Balancer rapporteert de pakketten dat per front-end wordt verwerkt.|

Beoordeling [gedetailleerde beschrijving van de diagnostische gegevens van Standard Load Balancer](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>Maximaal beschikbare poorten

Standard Load Balancer biedt ondersteuning voor een nieuw type regel.  

U kunt regels voor taakverdeling om de schaal van uw toepassing en worden zeer betrouwbare configureren. Wanneer u een regel voor taakverdeling HA-poorten, biedt standaardversie van Load Balancer per stroom taakverdeling op elke kortstondige poort van een interne standaardversie van Load Balancer van front-end-IP-adres.  De functie is handig voor andere scenario's waarin het niet praktisch of ongewenste als afzonderlijke poorten wilt opgeven.

Een regel voor taakverdeling HA-poorten kunt u actief-passief of actief-actief n + 1-scenario's voor Network Virtual Appliances en andere toepassingen waarvoor grote bereik van poorten voor inkomend verkeer te maken.  Een statustest kan worden gebruikt om te bepalen welke back-ends die nieuwe stromen moet worden ontvangen.  Een Netwerkbeveiligingsgroep kunt u een scenario voor het bereik van poort worden geëmuleerd.

>[!IMPORTANT]
> Als u van plan bent te gebruiken een virtueel netwerkapparaat, neem contact op met de leverancier voor informatie over of hun product is getest met HA-poorten en volgt u de specifieke hulp voor implementatie. 

Beoordeling [gedetailleerde beschrijving van de HA-poorten in](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Standaard beveiligen

Standard Load Balancer is volledig onboarding uitgevoerd voor het virtuele netwerk.  Het virtuele netwerk is een particulier netwerk gesloten.  Omdat Standard Load Balancers en de standaard openbare IP-adressen zijn ontworpen om toe te staan van dit virtuele netwerk toegankelijk van buiten het virtuele netwerk, standaard deze resources nu gesloten, tenzij u ze opent. Dit betekent dat Netwerkbeveiligingsgroepen (nsg's) zijn nu gebruikt voor het expliciet toestaan en geaccepteerde verkeer toegestaan.  U kunt uw volledige virtuele Datacenter maken en bepalen via NSG wat en wanneer deze beschikbaar moet zijn.  Als u een NSG op een subnet of NIC van de bron van uw virtuele machine hebt, wordt verkeer niet toegestaan voor het bereiken van deze resource.

Zie voor meer informatie over nsg's en hoe u deze wilt toepassen voor uw scenario, [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).

### <a name="outbound"></a> Uitgaande verbindingen

Load Balancer biedt ondersteuning voor scenario's voor binnenkomend en uitgaand.  Standard Load Balancer is heel anders dan Basic Load Balancer met betrekking tot uitgaande verbindingen.

Bron Network Address Translation (SNAT) wordt gebruikt om interne, persoonlijke IP-adressen in het virtuele netwerk worden toegewezen aan het openbare IP-adressen op de Load Balancer-front-ends.

Standard Load Balancer introduceert een nieuwe algoritme voor een [robuuste, schaalbare en voorspelbare SNAT algoritme](load-balancer-outbound-connections.md#snat) en nieuwe mogelijkheden kunnen Hiermee verwijdert u dubbelzinnigheid en krachten expliciete configuraties in plaats daarvan effecten zijde. Deze wijzigingen zijn die nodig zijn om toe te staan voor de nieuwe functies te geven. 

Dit zijn de belangrijkste basisprincipes te onthouden bij het werken met de standaardversie van Load Balancer:

- de voltooiing van een regel bepaalt de Load Balancer-resource.  Alle programma's van Azure is afgeleid van de configuratie ervan.
- Wanneer er meerdere front-ends beschikbaar zijn, alle front-ends worden gebruikt en elke front-end wordt het aantal beschikbare poorten van SNAT vermenigvuldigd
- u kunt kiezen en bepalen of u niet dat voor een bepaalde front-end wenst moet worden gebruikt voor uitgaande verbindingen.
- uitgaande scenario's zijn expliciete en uitgaande connectiviteit bestaat niet totdat deze is opgegeven.
- regels voor taakverdeling afleiden hoe SNAT is geprogrammeerd. Regels voor taakverdeling zijn specifieke-protocol. SNAT is protocolspecifiek en configuratie moet dit aangeven in plaats van een neveneffect maken.

#### <a name="multiple-frontends"></a>Meerdere frontends
Als u meer SNAT poorten wilt omdat u verwacht of zijn al ondervindt hoge eisen voor uitgaande verbindingen, kunt u ook incrementele SNAT poort inventaris toevoegen door extra front-ends, regels en back-endpools naar dezelfde virtuele machine configureren resources.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Welke front-end wordt gebruikt voor het besturingselement uitgaande
Als u beperken uitgaande verbindingen wilt naar alleen afkomstig zijn van een specifieke frontend-IP-adres, kunt u eventueel uitgaande SNAT op de regel die de toewijzing van het uitgaande uitschakelen.

#### <a name="control-outbound-connectivity"></a>Uitgaande connectiviteit van besturingselement
Standard Load Balancer bestaat in de context van het virtuele netwerk.  Een virtueel netwerk is een geïsoleerde, particuliere netwerk.  Tenzij een koppeling met een openbaar IP-adres bestaat, worden openbare verbinding is niet toegestaan.  U kunt bereiken [VNet-Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) omdat ze van en lokaal naar uw virtuele netwerk zijn.  Als u wilt om uitgaande verbinding naar een bestemming buiten het virtuele netwerk te maken, hebt u twee opties:
- Standaard-SKU openbaar IP-adres toewijzen als Instance-Level Public IP-adres aan de bron van de virtuele machine of
- Plaats de bron van de virtuele machine in de back-endadresgroep van een openbare Standard Load Balancer.

Beide kunnen uitgaande connectiviteit van het virtuele netwerk naar buiten het virtuele netwerk. 

Als u _alleen_ hebben een interne Standard Load Balancer die zijn gekoppeld aan de back-endpool waarin de resource van uw virtuele machine zich bevindt, uw virtuele machine kan alleen virtuele-netwerkbronnen bereiken en [VNet-Service Eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md).  U kunt de stappen die worden beschreven in de vorige alinea uitgaande connectiviteit maken.

Uitgaande connectiviteit van de bron van een virtuele machine die niet zijn gekoppeld aan de standaard-SKU's blijft voordat.

Beoordeling [gedetailleerde discussie over uitgaande verbindingen](load-balancer-outbound-connections.md).

### <a name="multife"></a>Meerdere frontends
Load Balancer biedt ondersteuning voor meerdere regels met meerdere front-ends.  Standard Load Balancer breidt deze uitgaande scenario's.  Uitgaande scenario's zijn in feite de omgekeerde waarde van een binnenkomende regel voor taakverdeling.  De binnenkomende regel voor taakverdeling maakt ook een koppelen voor uitgaande verbindingen. Standaardversie van Load Balancer maakt gebruik van alle front-ends die zijn gekoppeld aan de bron van een virtuele machine via een regel voor taakverdeling.  Bovendien een parameter voor de regel voor taakverdeling en kunt u een regel voor taakverdeling voor de toepassing van de uitgaande connectiviteit, waardoor de selectie van specifieke front-ends met inbegrip van geen onderdrukken.

Ter vergelijking: Basic Load Balancer wordt een enkele front willekeurig geselecteerd en er is geen mogelijkheid om te bepalen welke is geselecteerd.

Beoordeling [gedetailleerde discussie over uitgaande verbindingen](load-balancer-outbound-connections.md).

### <a name="operations"></a> Beheerbewerkingen

Standard Load Balancer resources aanwezig zijn op een geheel nieuwe infrastructuurplatform.  Hierdoor kunnen sneller beheerbewerkingen voor standaard-SKU's en voltooiing tijden zijn meestal minder dan 30 seconden per standaard SKU-resource.  Als back-endpools in omvang toeneemt, de duur die nodig is voor back-end pool verandert ook toename.

U kunt Standard Load Balancer resources wijzigen en een Standard openbaar IP-adres van een virtuele machine verplaatsen naar een andere veel sneller.

## <a name="migration-between-skus"></a>Migratie tussen SKU 's

Er zijn geen veranderlijke SKU's. Volg de stappen in deze sectie om te verplaatsen van één resource SKU naar een andere.

>[!IMPORTANT]
>Raadpleeg dit document in zijn geheel toe om te weten wat de verschillen tussen SKU's en uw scenario hebt zorgvuldig worden onderzocht.  Mogelijk moet u aanvullende wijzigingen aanbrengen in uw scenario uitlijnen.

### <a name="migrate-from-basic-to-standard-sku"></a>Migreren van Basic naar Standard-SKU

1. Maak een nieuwe Standard-resource (Load Balancer en openbare IP-adressen, indien nodig). Opnieuw maken van uw regels en test definities.  Als u een test TCP 443/tcp eerder, u kunt dit protocol test wijzigen naar een HTTPS-test en voegt u een pad.

2. Maak een nieuwe of bestaande NSG op de NIC of subnet update naar verkeer met gelijke lijst met toegestane adressen, test, evenals andere verkeer dat u wilt toestaan.

3. De basis-SKU-resources (Load Balancer en openbare IP-adressen, zoals van toepassing) verwijderen uit alle VM-exemplaren. Zorg ervoor dat ook verwijderen van alle VM-exemplaren van een beschikbaarheidsset.

4. Koppel alle VM-exemplaren aan de nieuwe standaard-SKU-resources.

### <a name="migrate-from-standard-to-basic-sku"></a>Migreren van Standard naar de basis-SKU

1. Maak een nieuwe Basic-resource (Load Balancer en openbare IP-adressen, indien nodig). Opnieuw maken van uw regels en test definities.  Hiermee wijzigt u een HTTPS-test in een test TCP 443/tcp. 

2. De standaard-SKU-resources (Load Balancer en openbare IP-adressen, zoals van toepassing) verwijderen uit alle VM-exemplaren. Zorg ervoor dat ook verwijderen van alle VM-exemplaren van een beschikbaarheidsset.

3. Koppel alle VM-exemplaren aan de nieuwe basis-SKU-resources.

>[!IMPORTANT]
>
>Er zijn beperkingen met betrekking tot gebruik van de Basic en Standard-SKU's.
>
>HA zijn-poorten en diagnostische gegevens van de standaard-SKU alleen beschikbaar in de standaard-SKU. U kunt geen migreren van de standaard-SKU naar de basis-SKU en deze functies ook behouden.
>
>Basis en standaard-SKU hebt een aantal verschillen, zoals wordt beschreven in dit artikel.  Zorg ervoor dat u begrijpt en voorbereiden voor hen.
>
>Overeenkomende SKU's mogen worden gebruikt voor Load Balancer en openbare IP-resources. U kunt geen een combinatie van standaard-SKU-middelen en basis-SKU. Het is evenmin mogelijk om zelfstandige virtuele machines, virtuele machines in een resource van een beschikbaarheidsset of resources uit schaalset met virtuele machines op beide SKU's tegelijk in te stellen.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Standard Load Balancer is momenteel beschikbaar in alle openbare cloud-regio's.

## <a name="sla"></a>SLA

Standard Load Balancers zijn beschikbaar met een SLA van 99,99%.  Controleer de [Standard Load Balancer SLA](https://aka.ms/lbsla) voor meer informatie.

## <a name="pricing"></a>Prijzen

Gebruik van Standard Load Balancer wordt in rekening gebracht.

- Aantal geconfigureerde load-balancing en uitgaande regels regels (binnenkomende NAT-regels niet meegeteld in het totale aantal regels)
- Hoeveelheid gegevens verwerkte regel voor binnenkomende en uitgaande ongeacht. 

Ga naar de pagina [Prijs van Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/) voor informatie over de prijs van Standard Load Balancer.

## <a name="limitations"></a>Beperkingen

- Er zijn geen veranderlijke SKU's. U kunt de SKU van een bestaande resource niet wijzigen.
- De bron van een zelfstandige virtuele machine resource beschikbaarheidsset of VM scale set resource kan verwijzen naar een SKU, niet beide.
- Een Load Balancer-regel kan niet twee virtuele netwerken omvatten.  Front-ends en hun bijbehorende back-end-exemplaren moeten zich bevinden in hetzelfde virtuele netwerk.  
- Load Balancer-front-ends zijn niet toegankelijk voor wereldwijde vnet-peering.
- [Bewerkingen in het abonnement verplaatsen](../azure-resource-manager/resource-group-move-resources.md) worden niet ondersteund voor Standard SKU LB en PIP-resources.
- Webwerkrollen zonder dat u een VNet en andere Microsoft-platform-services kunnen worden geopend als alleen een interne standaardversie van Load Balancer wordt gebruikt vanwege een neveneffect van hoe het pre-VNet-services en andere platform functie services. U moet niet afhankelijk zijn van dit als de desbetreffende service zelf of het onderliggende platform kan zonder kennisgeving worden gewijzigd. U moet altijd wordt ervan uitgegaan dat u wilt maken [uitgaande connectiviteit](load-balancer-outbound-connections.md) expliciet indien gewenst bij het gebruik van een interne Standard Load Balancer alleen.
- Load Balancer is een TCP- of UDP-product voor taakverdeling en Port Forwarding voor deze specifieke IP-protocollen.  Taakverdelingsregels en inkomende NAT-regels worden ondersteund voor TCP en UDP en worden niet ondersteund voor andere IP-protocollen, met inbegrip van ICMP. Load Balancer beëindigt de nettolading van een UDP- of TCP-stroom niet, reageert er niet op of communiceert er op geen enkele andere ander manier mee. Het is geen proxy. Succesvolle validatie van de verbinding met een front-plaats in-band moet uitvoeren met hetzelfde protocol gebruikt in een load balancing of inkomende NAT-regel (TCP of UDP) _en_ moet ten minste één van uw virtuele machines een antwoord genereren voor een client om te zien van een reactie van een front-end.  Niet ontvangen van een in-band-antwoord van de Load Balancer-front-geeft aan dat er waren geen virtuele machines kunnen reageren.  Het is niet mogelijk om te communiceren met een Load Balancer front-end zonder een virtuele machine kunnen reageren.  Dit geldt ook voor uitgaande verbindingen waar [poortmaskering SNAT](load-balancer-outbound-connections.md#snat) alleen wordt ondersteund voor TCP en UDP; andere IP-protocollen, waaronder ICMP, werken ook niet.  Wijs een openbaar IP-adres op instantieniveau toe om dit op te lossen.
- In tegenstelling tot openbare Load Balancers waardoor [uitgaande verbindingen](load-balancer-outbound-connections.md) bij het overstappen van privé-IP-adressen binnen het virtuele netwerk naar openbare IP-adressen, interne Load Balancers niet doen vertalen uitgaande afkomstig is verbindingen met de front-end van een interne Load Balancer als beide zijn in privé-IP-adresruimte.  Hiermee voorkomt u mogelijkheden voor uitputting van SNAT in unieke interne IP-adresruimte waarbij vertaling niet vereist is.  Een neveneffect is dat als een uitgaande stroom vanuit een virtuele machine in de back-end-adrespool probeert een webfront-end van de interne Load Balancer in die groep zich bevindt flow _en_ is toegewezen naar zichzelf, beide zijden van de stroom komen niet overeen en de stroom mislukt .  Als de stroom niet terug naar dezelfde virtuele machine in de back-end-pool die de stroom aan de front-end hebt gemaakt toegewezen is, wordt de stroom slaagt.   Wanneer de stroom wordt toegewezen aan zichzelf de uitgaande stroom lijkt te zijn afkomstig uit de virtuele machine aan de front-end en de bijbehorende inkomende stroom lijkt te zijn afkomstig uit de virtuele machine naar zichzelf. Vanuit het oogpunt van het gastbesturingssysteem gezien, komen de inkomende en uitgaande delen van dezelfde flow in de virtuele machine niet overeen. De TCP-stack herkent deze niet als twee helften van dezelfde stroom, omdat de bron en de bestemming niet overeenkomen.  Wanneer de stroom wordt toegewezen aan andere VM's in de back-endpool, komt overeen met de halve van de stroom en de stroom is de virtuele machine kan reageren.  Het symptoom voor dit scenario is onregelmatige verbindingstime-outs. Er zijn enkele algemene oplossingen voor het bereiken van dit scenario op betrouwbare wijze (die afkomstig zijn van stromen van een back-end-pool naar de back-end-pools respectieve interne Load Balancer front-) waaronder beide invoegen van een derde partij proxy achter de interne Load Balancer of [met behulp van de stijlregels DSR](load-balancer-multivip-overview.md).  Als oplossing hiervoor zou u een openbare Load Balancer kunnen gebruiken, maar het scenario dat daar het resultaat van is, is gevoelig voor [SNAT-uitputting](load-balancer-outbound-connections.md#snat) en moet worden vermeden, tenzij dit nauwkeurig wordt beheerd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [Standard Load Balancer en Beschikbaarheidszones](load-balancer-standard-availability-zones.md).
- Meer informatie over [statuscontroles](load-balancer-custom-probe-overview.md).
- Meer informatie over [Beschikbaarheidszones](../availability-zones/az-overview.md).
- Meer informatie over [standaardversie van Load Balancer Diagnostics](load-balancer-standard-diagnostics.md).
- Meer informatie over [ondersteunde multi-dimensionale metrische gegevens](../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) voor diagnostische gegevens in [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
- Meer informatie over het gebruik van [Load Balancer voor uitgaande verbindingen](load-balancer-outbound-connections.md).
- Meer informatie over [regels voor uitgaand verkeer](load-balancer-outbound-rules-overview.md).
- Meer informatie over [TCP opnieuw ingesteld op niet-actieve](load-balancer-tcp-reset.md).
- Meer informatie over [standaardversie van Load Balancer met regels voor taakverdeling HA-poorten in](load-balancer-ha-ports-overview.md).
- Meer informatie over het gebruik van [Load Balancer met meerdere front-ends](load-balancer-multivip-overview.md).
- Meer informatie over [virtuele netwerken](../virtual-network/virtual-networks-overview.md).
- Meer informatie over [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).
- Meer informatie over [VNet-Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md).
- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden die](../networking/networking-overview.md) in Azure.
- Meer informatie over [Load Balancer](load-balancer-overview.md).
