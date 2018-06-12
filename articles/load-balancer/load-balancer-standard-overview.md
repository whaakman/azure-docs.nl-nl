---
title: Overzicht van Azure Standard Load Balancer | Microsoft Docs
description: Overzicht van Azure Standard Load Balancer-functies
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2018
ms.author: kumud
ms.openlocfilehash: 20897137c617ddf9a33a8f4966bcd7e30ac7c60c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261930"
---
# <a name="azure-load-balancer-standard-overview"></a>Overzicht van Azure Load Balancer standaard

Azure Load Balancer kunt u hoge beschikbaarheid voor uw services maken en schalen van uw toepassingen. Load Balancer kan worden gebruikt voor scenario's voor zowel inkomende als uitgaande en lage latentie en hoge doorvoersnelheid biedt en schaalt miljoenen stromen voor alle TCP en UDP-toepassingen. 

In dit artikel is gericht op een standaard Load Balancer.  Raadpleeg voor een meer algemene overzicht voor de Azure Load Balancer [Load Balancer Overview](load-balancer-overview.md) ook.

## <a name="what-is-standard-load-balancer"></a>Wat is standaard Load Balancer?

Standaard Load Balancer is een nieuwe Load Balancer-product voor alle TCP en UDP-toepassingen met een uitgebreide en gedetailleerde functieset via Basic Load Balancer.  Hoewel er veel overeenkomsten, is het belangrijk om vertrouwd te raken met de verschillen zoals wordt beschreven in dit artikel.

U kunt standaard Load Balancer gebruiken als een openbare of interne Load Balancer. En een virtuele machine kunnen worden verbonden met één openbare en één interne Load Balancer-resource.

Functies van de bron van de Load Balancer worden altijd uitgedrukt als een frontend, een regel, een health test en de definitie van een back-end-adresgroep.  Een resource kan meerdere regels bevatten. U kunt virtuele machines in de back-endpool plaatsen door op te geven van de back-endpool van de virtuele machine NIC-resource.  Deze parameter is doorgegeven aan het netwerkprofiel en uitgevouwen wanneer ze met behulp van de virtuele-machineschaalsets.

Een belangrijk aspect is de omvang van het virtuele netwerk voor de resource.  Hoewel Basic Load Balancer binnen het bereik van een beschikbaarheidsset bestaat, een standaard Load Balancer is volledig geïntegreerd met het bereik van een virtueel netwerk en alle virtuele netwerkconcepten van toepassing.

Load Balancer resources zijn waarbinnen u hoe de multitenant-infrastructuur voor het scenario dat u wilt maken op Azure moet programma kunt express-objecten.  Er is geen directe relatie tussen resources van de Load Balancer en werkelijke infrastructuur; geen exemplaar maken van een Load Balancer niet maken, capaciteit altijd beschikbaar is en er zijn geen opstarten of schalen van vertragingen in overweging moet nemen. 

>[!NOTE]
> Azure biedt een reeks volledig beheerde oplossingen voor uw scenario's voor taakverdeling.  Als u voor TLS-beëindiging ('SSL-offload') of per toepassing HTTP/HTTPS-laag aanvraagverwerking zoeken wilt, bekijkt u [Application Gateway](../application-gateway/application-gateway-introduction.md).  Als u op zoek bent voor globale DNS taakverdeling, Bekijk [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Uw end-to-end-scenario's kunnen profiteren van deze oplossingen combineren indien nodig.

## <a name="why-use-standard-load-balancer"></a>Waarom standaard Load Balancer gebruiken?

Met behulp van Standard Load Balancer kunt u de schaal van uw toepassingen aanpassen en hoge beschikbaarheid realiseren voor kleinschalige implementaties tot grote en complexe architecturen met meerdere zones.

Controleer de onderstaande tabel voor een overzicht van de verschillen tussen standaard Load Balancer en Basic Load Balancer:

>[!NOTE]
> Nieuwe ontwerpen Overweeg het gebruik van standaard Load Balancer. 

| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| Grootte van de back-end | maximaal 1000 exemplaren | maximaal 100 exemplaren |
| Back-end-pool-eindpunten | Een virtuele machine in één virtueel netwerk, met inbegrip van de combinatie van virtuele machines, beschikbaarheidssets, virtuele-machineschaalset wordt ingesteld. | virtuele machines in een enkel beschikbaarheid instellen of de virtuele machine schaal instellen |
| Beschikbaarheidszones | Zone-redundante en zonal frontends voor inkomend en uitgaand, uitgaande stromen toewijzingen zone storingen, taakverdeling cross-zone | / |
| Diagnostiek | Azure Monitor multidimensionale metrische gegevens inclusief byte en pakket tellers, health test status, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (snat omzetten geslaagde en mislukte flows), actieve gegevens vlak metingen | Azure Log Analytics voor openbare Load Balancer, snat omzetten uitputting van de waarschuwing, status aantal back-end Pools |
| HA poorten | Interne Load Balancer | / |
| Standaard beveiligen | standaard gesloten voor openbare IP-adres en de Load Balancer-eindpunten en een netwerkbeveiligingsgroep worden gebruikt voor het expliciet geaccepteerde voor verkeer stromen | standaard open is, netwerkbeveiligingsgroep optioneel |
| Uitgaande verbindingen | Meerdere frontends met per regel opt-out. Een uitgaande scenario _moet_ expliciet voor de virtuele machine te kunnen gebruiken uitgaande verbinding worden gemaakt.  [VNet-Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) zonder uitgaande verbinding kan worden bereikt en kan niet worden meegeteld voor gegevens die worden verwerkt.  Openbare IP-adressen, inclusief Azure PaaS-services niet beschikbaar als de Service-eindpunten VNet, moeten worden bereikt via uitgaande verbinding en het aantal naar gegevens die worden verwerkt. Wanneer een interne Load Balancer een virtuele machine fungeert, zijn uitgaande verbindingen via standaard snat omzetten niet beschikbaar. Uitgaande snat omzetten programmering is transportprotocol bepaald op basis van het protocol van de regel voor binnenkomende taakverdeling. | Één front-end willekeurig worden geselecteerd als er meerdere frontends aanwezig zijn.  Wanneer een virtuele machine alleen interne Load Balancer fungeert, wordt standaard snat omzetten wordt gebruikt. |
| Meerdere frontends | Binnenkomend en uitgaand | Alleen binnenkomende gegevens |
| Beheerbewerkingen | De meeste bewerkingen < 30 seconden | 60-90 seconden typische |
| SLA | 99,99% voor gegevenspad met twee virtuele machines die in orde | Impliciete in VM SLA | 
| Prijzen | Kosten in rekening gebracht op basis van regels, verwerkt gegevens binnenkomende of uitgaande die zijn gekoppeld aan een resource  | Gratis |

Bekijk [Servicelimieten voor de Load Balancer](https://aka.ms/lblimits), evenals [prijzen](https://aka.ms/lbpricing), en [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Back-endpool

Standaard Load Balancer back-endpools uitgebreid tot alle bronnen van de virtuele machine in een virtueel netwerk.  Naam mag maximaal 1000 back-end-exemplaren.  Een back-end-exemplaar is een IP-configuratie, een eigenschap van een NIC-resource is.

De back-endpool kan zelfstandige virtuele machines, beschikbaarheidssets of virtuele-machineschaalsets bevatten.  U kunt ook de bronnen in de back-endpool overlopen. U kunt maximaal 150 bronnen in de back-endpool per resource van de Load Balancer combineren.

Wanneer u overweegt het ontwerp van uw back endpool, u kunt ontwerpen voor het laagste aantal resources in de afzonderlijke back-end voor de duur van beheerbewerkingen verder te optimaliseren.  Er is geen verschil in prestaties vlak of schaal.

## <a name="az"></a>Beschikbaarheid Zones

Standaard Load Balancer biedt ondersteuning voor extra mogelijkheden in regio's waar beschikbaarheid Zones beschikbaar zijn.  Deze functies zijn incrementele aan alle standaard Load Balancer biedt.  Beschikbaarheid Zones configuraties zijn beschikbaar voor openbare en interne standaard Load Balancer.

Niet-zonal frontends worden zone-redundante standaard wanneer geïmplementeerd in een regio met beschikbaarheid Zones.   Een zone-redundante frontend blijft zone mislukt en wordt geleverd door speciale infrastructuur in alle zones tegelijk. 

U kunt bovendien een frontend naar een specifieke zone garanderen. Een zonal frontend gedrag deelt met de desbetreffende zone en alleen door een speciale infrastructuur in een enkele zone wordt behandeld.

Cross-zone taakverdeling is beschikbaar voor de back-endpool en de bron van een virtuele machine in een vnet kan deel uitmaken van een back-endpool.

Bekijk [gedetailleerde discussie over beschikbaarheid Zones gerelateerde mogelijkheden](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a> Diagnostische gegevens

Standaard Load Balancer biedt multidimensionale metrische gegevens via de Azure-Monitor.  Deze metrische gegevens kunnen worden gefilterd, gegroepeerd en uitgelicht voor een opgegeven dimensie.  Ze bieden de huidige en historische inzicht in prestaties en status van uw service.  Resourcestatus wordt ook ondersteund.  Hier volgt een kort overzicht van ondersteunde diagnostische gegevens:

| Gegevens | Beschrijving |
| --- | --- |
| VIP-beschikbaarheid | Load Balancer standaard oefent continu het gegevenspad van binnen een regio aan de Load Balancer-front-helemaal tot aan de stack SDN die ondersteuning biedt voor uw virtuele machine. Als in orde exemplaren blijft, volgt de meting hetzelfde pad als uw toepassing taakverdeling verkeer. Het gegevenspad die wordt gebruikt door uw klanten ook wordt gevalideerd. De meting is onzichtbaar voor uw toepassing en niet van invloed op een andere bewerkingen.|
| Beschikbaarheid van de DIP | Load Balancer standaard maakt gebruik van een gedistribueerde health-service die u de status van het toepassingseindpunt van uw volgens de configuratie-instellingen bewaakt te zoeken. Met deze metriek zorgt voor een statistische functie of groep per eindpunt gefilterd-weergave van elk eindpunt afzonderlijke exemplaar in de Load Balancer.  U kunt zien hoe de Load Balancer de status van uw toepassing, zoals aangegeven door uw configuratie van health test bekijkt.
| SYN-pakketten | Load Balancer standaard niet beëindigen TCP-verbindingen of interactie met TCP of UDP pakketstromen. Stromen en hun handshakes zijn altijd tussen de bron en de VM-instantie. Als u wilt uw TCP-protocol scenario's op te lossen, kunt u het gebruik van SYN pakketten items om te begrijpen hoeveel TCP-verbinding is geprobeerd. De metriek rapporteert het aantal TCP SYN-pakketten die zijn ontvangen.|
| Verbindingen met snat omzetten | Load Balancer standaard rapporteert het aantal uitgaande stromen die op de front-openbare IP-adres zijn masqueraded. Snat omzetten poorten zijn een onuitputtelijk resource. Met deze metriek kunt geven een indicatie van hoe zwaar uw toepassing is vertrouwen op snat omzetten voor uitgaande oorsprong stromen.  Tellers voor geslaagde en mislukte uitgaande snat omzetten stromen worden gerapporteerd, en kunnen worden gebruikt om problemen op te begrijpen van de status van uw uitgaande stromen.|
| Byte-prestatiemeteritems | Load Balancer standaard rapporten de gegevens per front-end verwerkt.|
| Pakket-prestatiemeteritems | Load Balancer standaard rapporteert de pakketten dat per front-end verwerkt.|

Bekijk [gedetailleerde bespreking van de standaard Load Balancer diagnostische](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>HA poorten

Standaard Load Balancer biedt ondersteuning voor een nieuw type regel.  

U kunt de load-balancingregels om de schaal van uw toepassing en maximaal betrouwbaar configureren. Bij het gebruik van een HA-poorten voor de load balancer regel standaard Load Balancer biedt per stroom voor de load balancer op elke tijdelijke poort van een interne standaard Load Balancer frontend-IP-adres.  De functie is handig voor andere scenario's waarin het niet handig of ongewenste als afzonderlijke poorten wilt opgeven.

Een taakverdelingsregel voor HA-poorten kunt u actief-passief of actief / actief n + 1 scenario's maken voor virtuele netwerkapparaten en elke toepassing, die grote gegevensreeksen van poorten voor inkomend verkeer vereist.  Een health test kan worden gebruikt om te bepalen welke back-ends moet worden ontvangen van nieuwe stromen.  Een Netwerkbeveiligingsgroep kunt u een scenario voor het bereik van poort worden geëmuleerd.

>[!IMPORTANT]
> Als u van plan bent te gebruiken van een virtueel netwerkapparaat, neem contact op met uw leverancier voor hulp bij het of hun product is getest met HA poorten en volg hun specifieke richtlijnen voor implementatie. 

Bekijk [gedetailleerde bespreking van de HA-poorten](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Standaard beveiligen

Standaard Load Balancer is volledig vrijgegeven aan het virtuele netwerk.  Het virtuele netwerk is een particulier netwerk gesloten.  Omdat standaard Load Balancers en de standaard openbare IP-adressen zo dat dit virtuele netwerk ontworpen zijn toegankelijk van buiten het virtuele netwerk, standaard deze resources nu gesloten, tenzij u ze opent. Dit betekent Netwerkbeveiligingsgroepen (nsg's) worden nu gebruikt om expliciet toestaan en geaccepteerde verkeer toegestaan.  U kunt uw gehele virtuele Datacenter maken en via het NSG bepalen wat en wanneer deze beschikbaar moet zijn.  Als u niet beschikt over een NSG op een subnet of NIC van de bron van de virtuele machine kan verkeer niet tot deze bron wordt toegestaan.

Zie voor meer informatie over nsg's en hoe u deze wilt toepassen voor uw scenario, [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).

### <a name="outbound"></a> Uitgaande verbindingen

Load Balancer ondersteunt scenario's voor binnenkomend en uitgaand.  Standaard Load Balancer is aanzienlijk anders voor Basic Load Balancer met betrekking tot uitgaande verbindingen.

Bron Network Address Translation (snat omzetten) wordt gebruikt voor interne, persoonlijke IP-adressen in het virtuele netwerk worden toegewezen aan het openbare IP-adressen op de Load Balancer frontends.

Standaard Load Balancer introduceert een nieuw algoritme voor een [robuuste, schaalbare en voorspelbare snat omzetten algoritme](load-balancer-outbound-connections.md#snat) en kunnen nieuwe mogelijkheden, verwijdert dubbelzinnigheid en dwingt expliciete configuraties in plaats daarvan effecten zijde. Deze wijzigingen zijn nodig zodat voor nieuwe functies te geven. 

Dit zijn de belangrijkste basisprincipes te onthouden als u werkt met standaard Load Balancer:

- de voltooiing van een regel bepaalt de Load Balancer-resource.  alle programmering van Azure is afgeleid van de configuratie ervan.
- Wanneer meerdere frontends beschikbaar zijn, alle frontends worden gebruikt en elke frontend vermenigvuldigt het aantal beschikbare poorten met snat omzetten
- u kunt kiezen en bepalen of u niet voor een bepaalde frontend wenst moet worden gebruikt voor uitgaande verbindingen.
- uitgaande scenario's zijn expliciete en uitgaande verbinding bestaat niet totdat dit is opgegeven.
- regels voor taakverdeling afleiden hoe snat omzetten is geprogrammeerd. Regels voor taakverdeling zijn specifieke-protocol. Snat omzetten is protocolspecifiek en de configuratie moet navenant in plaats van een neveneffect maken.

#### <a name="multiple-frontends"></a>Meerdere frontends
Als u meer snat omzetten poorten wilt omdat u had verwacht of u bent al ondervindt hoge eisen voor uitgaande verbindingen, kunt u ook incrementele snat omzetten poort inventaris toevoegen door extra frontends, regels en back-endpools op de virtuele machine configureren bronnen.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Welke frontend wordt gebruikt voor het besturingselement uitgaand
Als u beperken uitgaande verbindingen wilt via alleen afkomstig zijn van een specifieke frontend-IP-adres, kunt u eventueel uitgaande snat omzetten op de regel die staat voor de uitgaande toewijzing uitschakelen.

#### <a name="control-outbound-connectivity"></a>Besturingselement uitgaande verbinding
Standaard Load Balancer bestaat in de context van het virtuele netwerk.  Een virtueel netwerk is een geïsoleerd, particuliere netwerk.  Tenzij een koppeling met een openbaar IP-adres bestaat, wordt de openbare-connectiviteit niet toegestaan.  U kunt bereiken [VNet Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) omdat ze van en lokaal naar uw virtuele netwerk.  Als u uitgaande verbinding met een bestemming buiten het virtuele netwerk tot stand brengen wilt, hebt u twee opties:
- een standaard SKU openbare IP-adres toewijzen als een instantieniveau openbare IP-adres aan de bron van de virtuele machine of
- de bron van de virtuele machine in de back-endpool van een openbare standaard Load Balancer plaatsen.

Beide kunt uitgaande verbinding hebben met het virtuele netwerk naar buiten het virtuele netwerk. 

Als u _alleen_ hebben een interne standaard Load Balancer die zijn gekoppeld aan de back-endpool waarin de bron van de virtuele machine zich bevindt, de virtuele machine kan alleen resources van een virtueel netwerk bereiken en [VNet-Service Eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md).  U kunt de stappen beschreven in de vorige alinea uitgaande verbinding te maken.

Uitgaande verbinding van de bron van een virtuele machine niet is gekoppeld aan de standaard SKU's blijft als voordat.

Bekijk [gedetailleerde bespreking van uitgaande verbindingen](load-balancer-outbound-connections.md).

### <a name="multife"></a>Meerdere frontends
Load Balancer biedt ondersteuning voor meerdere regels met meerdere frontends.  Standaard Load Balancer wordt dit vergroot tot uitgaande scenario's.  Uitgaande scenario's zijn in feite de inverse van een inkomende regel voor taakverdeling.  De binnenkomende taakverdelingsregel ook maakt een koppelen voor uitgaande verbindingen. Alle frontends die zijn gekoppeld aan de bron van een virtuele machine via een taakverdelingsregel maakt gebruik van standaard Load Balancer.  Bovendien wordt een parameter voor de taakverdeling regel en kunt u een regel voor taakverdeling voor de doeleinden van uitgaande verbinding waarmee de selectie van specifieke frontends inclusief geen onderdrukken.

Ter vergelijking: Basic Load Balancer wordt een enkele front willekeurig geselecteerd en er is geen mogelijkheid om te bepalen welke is geselecteerd.

Bekijk [gedetailleerde bespreking van uitgaande verbindingen](load-balancer-outbound-connections.md).

### <a name="operations"></a> Beheerbewerkingen

Standaard resources van de Load Balancer bestaat op een geheel nieuwe infrastructuurplatform.  Hierdoor kunnen aanzienlijk sneller beheerbewerkingen voor standaard SKU's en voltooiingstijden zijn meestal minder dan 30 seconden per standaard SKU-resource.  Houd er rekening mee dat als back-endpools in omvang toeneemt, de duur vereist voor back-end van toepassingen ook verhogen verandert.

U kunt standaard Load Balancer resources wijzigen en een standaard openbare IP-adres van één virtuele machine verplaatsen naar een andere veel sneller.

## <a name="migration-between-skus"></a>Migratie tussen SKU 's

Er zijn geen veranderlijke SKU's. Volg de stappen in deze sectie voor het verplaatsen van één resource SKU naar een andere.

>[!IMPORTANT]
>Bekijk dit document in zijn geheel te begrijpen van de verschillen tussen SKU's en uw scenario hebt zorgvuldig worden onderzocht.  Wellicht moet u extra wijzigingen aanbrengen in uw scenario uitlijnen.

### <a name="migrate-from-basic-to-standard-sku"></a>Migreren van Basic naar standaard SKU

1. Maak een nieuwe standaard resource (Load Balancer en openbare IP-adressen, indien nodig). Maak uw regels en definities-test.

2. Maken van nieuwe of bestaande NSG op NIC of subnet bijwerken naar geaccepteerde taakverdelingsverkeer, test, evenals andere verkeer die u wilt toestaan.

3. De basis-SKU-resources (Load Balancer en openbare IP-adressen, indien van toepassing) verwijderen van alle VM-instanties. Zorg ervoor dat alle VM-instanties van een beschikbaarheidsset ook verwijderd.

4. Alle exemplaren van de VM koppelen aan de nieuwe standaard SKU-resources.

### <a name="migrate-from-standard-to-basic-sku"></a>Migreren van standaard naar de basis-SKU

1. Maak een nieuwe Basic resource (Load Balancer en openbare IP-adressen, indien nodig). Maak uw regels en definities-test. 

2. De standaard SKU-resources (Load Balancer en openbare IP-adressen, indien van toepassing) verwijderen van alle VM-exemplaren. Zorg ervoor dat alle VM-instanties van een beschikbaarheidsset ook verwijderd.

3. Alle exemplaren van de VM koppelen aan de nieuwe basis-SKU-resources.

>[!IMPORTANT]
>
>Zijn er beperkingen met betrekking tot gebruik van de basis en standaard SKU's.
>
>HA zijn poorten en diagnostische gegevens van de standaard SKU alleen beschikbaar in de standaard SKU. U kunt geen migreren van de standaard SKU naar de basis-SKU of deze functies ook behouden.
>
>Zowel basis en standaard SKU hebben een aantal verschillen zoals wordt beschreven in dit artikel.  Zorg ervoor dat u begrijpt en bereid ze.
>
>Overeenkomende SKU's moet worden gebruikt voor Load Balancer en openbare IP-bronnen. Een combinatie van basis-SKU-bronnen en bronnen van de standaard SKU is niet mogelijk. Het is evenmin mogelijk om zelfstandige virtuele machines, virtuele machines in een resource van een beschikbaarheidsset of resources uit schaalset met virtuele machines op beide SKU's tegelijk in te stellen.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Load Balancer standaard is momenteel beschikbaar in alle openbare cloud-regio's.

## <a name="sla"></a>SLA

Standaard Load Balancers zijn beschikbaar met een SLA van 99,99%.  Controleer de [standaard Load Balancer SLA](https://aka.ms/lbsla) voor meer informatie.

## <a name="pricing"></a>Prijzen

Standaard Load Balancer is een bedrag product op basis van het aantal load balancer regels die zijn geconfigureerd en van alle binnenkomende en uitgaande gegevens verwerkt. Standaard Load Balancer prijsgegevens, gaat u naar de [Load Balancer prijzen](https://aka.ms/lbpricing) pagina.

## <a name="limitations"></a>Beperkingen

- Er zijn geen veranderlijke SKU's. U kunt de SKU van een bestaande resource niet wijzigen.
- De bron van een zelfstandige virtuele machine beschikbaarheidsset resource of bron voor virtuele machine scale set kan verwijzen naar een SKU, niet beide.
- Een Load Balancer-regel kan niet twee virtuele netwerken omvatten.  Frontends en hun bijbehorende back-end-exemplaren moeten zich bevinden in hetzelfde virtuele netwerk.  
- Load Balancer frontends zijn niet toegankelijk via het algemene virtueel netwerk peering.
- [Abonnement bewerkingen verplaatsen](../azure-resource-manager/resource-group-move-resources.md) worden niet ondersteund voor standaard SKU LB en PIP resources.
- Web-werkrollen zonder een VNet en andere Microsoft-platform-services kunnen worden geopend als alleen een als interne standaard Load Balancer wordt gebruikt als gevolg van een neveneffect van hoe het pre-VNet-services en andere platform functie services. U moet geen gebruik van dit aangezien de betreffende service zelf of het onderliggende platform kan zonder kennisgeving worden gewijzigd. U moet altijd wordt ervan uitgegaan dat u wilt maken [uitgaande verbinding](load-balancer-outbound-connections.md) expliciet indien gewenst bij gebruik van een interne standaard Load Balancer alleen.
- Load Balancer is een TCP- of UDP-product voor taakverdeling en het doorsturen van de poort voor deze specifieke IP-protocollen.  Taakverdelingsregels en binnenkomende NAT-regels worden ondersteund voor TCP en UDP en wordt niet ondersteund voor andere IP-protocollen met inbegrip van ICMP. Load Balancer niet beëindigt, reageren of anders communiceren met de nettolading van een UDP of TCP-stroom. Het is niet een proxy. Geslaagde validatie van de verbinding met een front-plaats in-band moet nemen met hetzelfde protocol gebruikt in een load balancing of inkomende NAT-regel (TCP of UDP) _en_ ten minste één van uw virtuele machines moet een antwoord genereren voor een client een reactie van een front-zien.  Geen een in-band-antwoord ontvangen van de Load Balancer-front-Hiermee geeft u dat geen virtuele machines kunnen reageren.  Het is niet mogelijk om te communiceren met een Load Balancer-front-zonder een virtuele machine kunnen reageren.  Dit geldt ook voor uitgaande verbindingen waar [poort maskerade snat omzetten](load-balancer-outbound-connections.md#snat) wordt alleen ondersteund voor TCP en UDP; andere IP-protocollen inclusief ICMP ook mislukken.  Een instantieniveau openbare IP-adres toewijzen te verhelpen.
- In tegenstelling tot openbare Load Balancers die bieden [uitgaande verbindingen](load-balancer-outbound-connections.md) tijdens het veranderen van privé IP-adressen binnen het virtuele netwerk naar openbare IP-adressen, interne Load Balancers komen niet vertalen uitgaande afkomstig is verbindingen met de front-end van een interne Load Balancer als beide zijn in de privé IP-adresruimte.  Zo voorkomt u mogelijkheden voor bronuitputting snat omzetten in unieke interne IP-adresruimte waar de vertaling niet vereist is.  Een neveneffect is dat als een uitgaande stroom van een virtuele machine in de groep back-end probeert een stroom aan front-end van de interne Load Balancer in welke groep database _en_ is toegewezen naar zichzelf, beide zijden van de stroom komen niet overeen en de stroom mislukt .  Als de stroom niet terug naar dezelfde virtuele machine in de back-end-pool die de stroom voor de front-end gemaakt toegewezen is, slaagt de stroom.   Wanneer de stroom wordt toegewezen aan zichzelf de uitgaande stroom lijkt te zijn afkomstig uit de virtuele machine op de front- en de bijbehorende binnenkomende stroom lijkt te zijn afkomstig uit de virtuele machine naar zichzelf. Uit oogpunt van het Gastbesturingssysteem, niet overeenkomen met de binnenkomende en uitgaande delen van dezelfde stroom in de virtuele machine. De TCP-stack herkent deze helften van dezelfde stroom niet als deel van dezelfde stroom als de bron- en doelserver komen niet overeen.  Wanneer de stroom wordt toegewezen aan met eventuele andere virtuele machine in de back-end-pool, komt overeen met de helften van stroom en de virtuele machine met succes kan reageren op de stroom.  Het symptoom voor dit scenario is een onregelmatige verbinding time-outs. Er zijn enkele algemene oplossingen voor het op betrouwbare wijze bereiken van dit scenario (die afkomstig zijn stroomt van een back-end-pool naar de back-end voor groepen van respectieve interne Load Balancer front-end-) zoals beide invoegen van een derde partij proxy achter de interne Load Balancer of [met behulp van de stijlregels DSR](load-balancer-multivip-overview.md).  Terwijl u een openbare Load Balancer gebruiken kunt om te beperken, het resulterende scenario is de kans op [snat omzetten uitputting](load-balancer-outbound-connections.md#snat) en tenzij u zorgvuldig worden beheerd, moeten worden vermeden.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [standaard Load Balancer en beschikbaarheid Zones](load-balancer-standard-availability-zones.md)
- Meer informatie over [beschikbaarheid Zones](../availability-zones/az-overview.md).
- Meer informatie over [standaard Load Balancer Diagnostics](load-balancer-standard-diagnostics.md).
- Meer informatie over [ondersteund multidimensionale metrische gegevens](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) voor diagnostische gegevens in [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
- Meer informatie over het gebruik van [Load Balancer voor uitgaande verbindingen](load-balancer-outbound-connections.md)
- Meer informatie over [standaard Load Balancer met taakverdelingsregels HA poorten](load-balancer-ha-ports-overview.md)
- Meer informatie over het gebruik van [Load Balancer met meerdere Frontends](load-balancer-multivip-overview.md)
- Meer informatie over [virtuele netwerken](../virtual-network/virtual-networks-overview.md).
- Meer informatie over [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md).
- Meer informatie over [VNet Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md)
- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden](../networking/networking-overview.md) in Azure.
- Meer informatie over [Load Balancer](load-balancer-overview.md).
