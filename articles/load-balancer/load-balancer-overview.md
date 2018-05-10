---
title: Overzicht van Azure Load Balancer | Microsoft Docs
description: Overzicht van Azure Load Balancer-functies, architectuur en implementatie. Informatie over de werking van de load balancer en van maken in de cloud.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 8a3eedb5a3d96eedd1a64d85afdb58f8961df272
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-azure-load-balancer"></a>Wat is Azure Load Balancer?

U kunt met Azure Load Balancer schalen van uw toepassingen en hoge beschikbaarheid voor uw services maken. Load Balancer biedt ondersteuning voor binnenkomend en uitgaand scenario's, lage latentie en hoge doorvoer en schaalt miljoenen stromen voor alle TCP en UDP-toepassingen.  

Load Balancer distribueert nieuwe inkomende stromen die op de load balancer front-exemplaren van de back-end-pool, volgens de regels en statuscontroles binnenkomen. 

Bovendien, kan een openbare load balancer voorzien uitgaande verbindingen voor virtuele machines (VM's) binnen het virtuele netwerk doordat hun persoonlijke IP-adressen naar openbare IP-adressen.

Azure Load Balancer is beschikbaar in twee SKU's: Basic en Standard. Er zijn verschillen in schaal, functies en prijzen. Elk scenario dat mogelijk is met Basic Load Balancer kan ook worden gemaakt met Standard Load Balancer, hoewel de benaderingen mogelijk enigszins anders. Als u meer informatie over de Load Balancer, is het belangrijk om vertrouwd te raken met de basisprincipes en SKU-specifieke verschillen.

## <a name="why-use-load-balancer"></a>Waarom Load Balancer gebruiken? 

U kunt Azure Load Balancer gebruiken:

* Taakverdeling binnenkomende internetverkeer naar uw virtuele machines. Deze configuratie wordt ook wel een [openbare load balancer](#publicloadbalancer).
* Verkeer toepassen van taakverdeling over VM's binnen een virtueel netwerk. U kunt ook een load balancer-front-end van een on-premises netwerk in een hybride scenario bereiken. Beide scenario's gebruiken een configuratie die wordt ook wel een [interne load balancer](#internalloadbalancer).
* Doorsturen netwerkverkeer via de poort op een specifieke poort op specifieke virtuele machines met regels voor binnenkomende netwerk adres NAT (Netwerkadresomzetting).
* Geef [uitgaande verbinding](load-balancer-outbound-connections.md) voor virtuele machines binnen het virtuele netwerk met behulp van een openbare load balancer.


>[!NOTE]
> Azure biedt een suite met volledig beheerde load balancing oplossingen voor uw scenario's. Als u zoekt naar beëindiging van Transport Layer Security (TLS)-protocol ('SSL-offload') of per-HTTP/HTTPS-aanvraag, toepassingslaag verwerken, bekijkt u [Application Gateway](../application-gateway/application-gateway-introduction.md). Als u op zoek bent voor globale DNS taakverdeling, Bekijk [Traffic Manager](../traffic-manager/traffic-manager-overview.md). Uw end-to-end-scenario's kunnen profiteren van deze oplossingen combineren indien nodig.

## <a name="what-are-load-balancer-resources"></a>Wat zijn de load balancer-bronnen?

Een load balancer-bron kan bestaan als een openbare load balancer of een interne load balancer. Functies van de load balancer-resource worden uitgedrukt als een front-end, een regel, een health test en de definitie van een back-end-pool. U plaatsen virtuele machines in de back-end-pool door op te geven van de back-end-pool van de virtuele machine.

Load balancer resources zijn waarbinnen u hoe de multitenant-infrastructuur voor het scenario dat u wilt maken op Azure moet programma kunt express-objecten. Er is geen directe relatie tussen de load balancer resources en de werkelijke infrastructuur. Geen exemplaar maken van een load balancer niet maken en capaciteit altijd beschikbaar is. 

## <a name="fundamental-load-balancer-features"></a>Fundamentele voorzieningen van de Load Balancer

Load Balancer biedt de volgende fundamentele mogelijkheden voor TCP en UDP-toepassingen:

* **Taakverdeling**

    Met Azure Load Balancer kunt u een regel voor load balancing te verdelen van verkeer dat op front-exemplaren van de back-end-pool binnenkomt maken. Load Balancer gebruikmaakt van een op basis van het hash-algoritme voor distributie van inkomende stromen en herschrijft de headers van stromen naar back-end-pool exemplaren dienovereenkomstig. Een server is beschikbaar voor het nieuwe stromen ontvangen bij een health test een goede back-end-eindpunt geeft.
    
    Load Balancer gebruikt standaard een 5-tuple hash bestaat uit de bron-IP-adres, bronpoort, IP-doeladres, doelpoort en IP-protocolnummer stromen om toe te wijzen beschikbare servers. U kunt de affiniteit met een specifiek IP-adres maken door naar een 2 of 3 tuple hash voor een bepaalde regel uitschakelen. Alle pakketten van dezelfde stroom van het pakket binnenkomen op hetzelfde exemplaar achter Netwerktaakverdeling front-end. De client initieert wanneer een nieuwe stroom van de dezelfde bron-IP, de poort is gewijzigd. Als gevolg hiervan hebben de 5-tuple kan leiden tot het verkeer naar een andere back-end-eindpunt.

    Zie voor meer informatie [Load balancer-distributie modus](load-balancer-distribution-mode.md). De volgende afbeelding geeft de hash-distributiepunt:

    ![Hash-distributiepunt](./media/load-balancer-overview/load-balancer-distribution.png)

    *Afbeelding: Hash-based distribution*

* **Poort doorsturen**

    Met Load Balancer, kunt u een binnenkomende NAT-regel voor doorsturen netwerkverkeer via de poort van een specifieke poort van een specifieke front-end-IP-adres op een specifieke poort van een specifieke back-end-exemplaar binnen het virtuele netwerk. Dit is ook doen door de dezelfde hash-distributiepunt als taakverdeling. Algemene scenario's voor deze mogelijkheid zijn Remote Desktop Protocol (RDP) of Secure Shell (SSH)-sessies met afzonderlijke VM-exemplaren binnen het virtuele netwerk van Azure. U kunt meerdere interne eindpunten toewijzen aan de verschillende poorten op de dezelfde front-end-IP-adres. U kunt deze gebruiken voor het extern beheren van uw virtuele machines via internet zonder de noodzaak van een extra korte inleiding in.

* **Toepassing agnostisch en transparante**

    Load Balancer communiceert rechtstreeks niet met TCP of UDP of de toepassingslaag en eventuele TCP of UDP-toepassingsscenario kan worden ondersteund.  Load Balancer niet beëindigen of afkomstig zijn van stromen, communiceren met de nettolading van de stroom biedt geen application layer gateway-functie en protocol handshakes optreden altijd rechtstreeks tussen de client en de back-endpool-instantie.  Een reactie op een binnenkomende stroom is altijd een reactie van een virtuele machine.  Als de stroom is bereikt op de virtuele machine, wordt het oorspronkelijke IP-bronadres ook behouden.  Een aantal voorbeelden ter illustratie van verdere transparantie:
    - Elk eindpunt wordt alleen beantwoord door een virtuele machine.  Bijvoorbeeld, een TCP-handshake wordt altijd optreedt tussen de client en de geselecteerde back-end-VM.  Een reactie van een aanvraag naar een front-end heeft een indeling die is gegenereerd door de back-end-VM. Wanneer u verbinding met een front-is valideert, valideert u de end-to-end-connectiviteit met ten minste één back-end virtuele machine.
    - Nettoladingen van toepassing zijn transparant voor de Load Balancer en eventuele UDP of TCP-toepassing kan worden ondersteund. Voor werkbelastingen waarvoor per verwerking van HTTP-aanvraag of manipuleren van application layer nettoladingen (bijvoorbeeld bij het parseren van HTTP-URL's), moet u een laag 7 load balancer zoals [Application Gateway](https://azure.microsoft.com/en-us/services/application-gateway).
    - Omdat de Load Balancer is agnostisch ten opzichte van de nettolading van de TCP- en TLS-offload ('SSL') is niet opgegeven, kunt u complete versleutelde scenario's met Load Balancer bouwen en grote scale-out voor TLS toepassingen krijgen door de TLS-verbinding op de virtuele machine zelf wordt beëindigd.  De TLS-sessie voor het versleutelen van capaciteit is bijvoorbeeld alleen beperkt door het type en aantal virtuele machines die u aan de groep back-end toevoegt.  Als u 'SSL-offloading', application layer behandeling of wenst te delegeren Certificaatbeheer naar Azure vereist, moet u Azure laag 7 load balancer [Application Gateway](https://azure.microsoft.com/en-us/services/application-gateway) in plaats daarvan.
        

* **Automatische herconfiguratie**

    Load Balancer opnieuw onmiddellijk als u exemplaren omhoog of omlaag schalen. Opnieuw geconfigureerd de load balancer zonder aanvullende bewerkingen op de load balancer-bron toevoegen of verwijderen van virtuele machines van de groep back-end.

* **Statuscontroles**

     Om te bepalen van de status van exemplaren in de back-end-pool, Load Balancer gebruikmaakt van statuscontroles die u definieert. Wanneer een test niet reageert, stopt de load balancer nieuwe verbindingen verzenden naar de slechte exemplaren. Bestaande verbindingen worden niet beïnvloed en ze blijven totdat de toepassing de stroom een niet-actieve time-out optreedt beëindigt, of de virtuele machine wordt afgesloten.

    Drie soorten tests worden ondersteund:

    - **Aangepaste HTTP-test**: U kunt deze test gebruiken voor het maken van uw eigen aangepaste regels om te bepalen van de status van een back-end-pool-exemplaar. De load balancer-tests regelmatig uw eindpunt (elke 15 seconden, standaard). Het exemplaar wordt beschouwd als in orde als deze met een HTTP 200 binnen de time-outperiode (standaard 31 seconden reageert). Elke andere status dan HTTP 200 zorgt ervoor dat deze test mislukt. Deze test is ook nuttig voor het implementeren van uw eigen logica voor het verwijderen van exemplaren van de load balancer worden gedraaid. U kunt bijvoorbeeld configureren dat het exemplaar om de status van een niet-200 retourneren als het exemplaar groter dan 90 procent CPU is.  Deze test overschrijft de standaard guest agent-test.

    - **Aangepaste test TCP**: deze test is afhankelijk van het tot stand brengen van een geslaagde TCP-sessie op een gedefinieerde testpoort. Als de opgegeven listener op de virtuele machine bestaat, wordt deze test slaagt. Als de verbinding is geweigerd, wordt de test mislukt. Deze test overschrijft de standaard guest agent-test.

    - **Test van de Guest-agent (op platform als een service [PaaS] alleen virtuele machines)**: de guest-agent in de virtuele machine kan ook gebruikmaken van de load balancer. De gastagent luistert en reageert met een HTTP 200 OK antwoord wanneer het exemplaar in de status gereed. Als de agent niet reageert met een HTTP 200 OK, wordt de load balancer markeert de instantie als niet-reagerende en stopt verkeer kunnen verzenden naar dit exemplaar. De load balancer blijft probeert te krijgen tot het exemplaar. Als u de guest-agent reageert met een HTTP 200, verzendt de load balancer verkeer opnieuw naar dit exemplaar. Guest agent tests een laatste toevlucht zijn en niet aanbevolen als HTTP- of TCP aangepaste test configuraties mogelijk zijn. 
    
* **Uitgaande verbindingen (snat omzetten)**

    Alle uitgaande stroomt van privé IP-adressen binnen het virtuele netwerk naar openbare IP-adressen op internet kunnen worden omgezet in een front-end-IP-adres van de load balancer. Wanneer een openbaar front-end is gekoppeld aan een back-end virtuele machine in een load-balancingregel, geeft Azure uitgaande verbindingen via worden automatisch omgezet naar het openbare IP-adres front-programma's.

    * Eenvoudig upgraden en herstel na noodgevallen van services, niet inschakelen omdat de front-end kan dynamisch worden toegewezen aan een ander exemplaar van de service.
    * Eenvoudiger toegangsbeheerlijst (ACL) toegangsbeheer op. ACL's die zijn uitgedrukt in termen van de front-end-IP-adressen niet wijzigen als services schaal omhoog of omlaag of ophalen opnieuw geïmplementeerd.  Uitgaande verbindingen via een kleiner aantal IP-adressen vertalen dan machines de last van whitelisting kunnen verminderen.

    Zie voor meer informatie [uitgaande verbindingen](load-balancer-outbound-connections.md).

Standaard Load Balancer heeft extra SKU-specifieke mogelijkheden buiten deze basisprincipes. Bekijk de rest van dit artikel voor meer informatie.

## <a name="skus"></a> Vergelijking van de Load Balancer-SKU

Load Balancer ondersteunt zowel Basic en Standard-SKU's, elk scenario schaal, functies, qua en prijzen. Elk scenario dat mogelijk is met Basic Load Balancer kan ook worden gemaakt met standaard Load Balancer. De API's voor beide SKU's zijn in feite vergelijkbare en aangeroepen via de specificatie van een SKU. De API voor de ondersteuning van SKU's voor Load Balancer en het openbare IP-adres is beschikbaar vanaf de 2017-08-01-API. Beide SKU's hebben de dezelfde algemene API en de structuur.

Echter, afhankelijk van welke SKU die u kiest, de configuratie van het volledige scenario mogelijk enigszins anders. Load Balancer documentatie illustreert wanneer een artikel alleen voor een specifieke SKU geldt. Zie de volgende tabel om te vergelijken en te weten over de verschillen. Zie voor meer informatie [standaard Load Balancer overzicht](load-balancer-standard-overview.md).

>[!NOTE]
> Als u een nieuwere ontwerp scenario gebruikt, kunt u overwegen standaard Load Balancer. 

Zelfstandige virtuele machines, beschikbaarheidssets en virtuele-machineschaalsets kunnen worden verbonden met slechts één SKU, niet beide. Wanneer u ze met openbare IP-adressen gebruikt, zowel Load Balancer en het openbare IP-adres SKU moet overeenkomen. Load Balancer en openbare IP-SKU's zijn niet veranderlijke.

_Het is een best practice om op te geven de SKU's expliciet, zelfs als deze nog niet verplicht._  Op dit moment zijn vereiste wijzigingen tot een minimum wordt beperkt. Als een SKU niet opgegeven is, wordt dit wordt geïnterpreteerd als een voornemen de 2017-08-01-API-versie van de basis-SKU te gebruiken.

>[!IMPORTANT]
>Standaard Load Balancer is een nieuw product van de Load Balancer en grotendeels een hoofdverzameling van Basic Load Balancer. Er zijn belangrijk en opzettelijk verschillen tussen de twee producten. Een end-to-end-scenario dat mogelijk is met Basic Load Balancer kan ook worden gemaakt met Standard Load Balancer. Als u bent al met Basic Load Balancer gebruikt, moet u dat vertrouwd raken met standaard Load Balancer te begrijpen van de meest recente wijzigingen in gedrag tussen standaard en Basic en de invloed ervan. Deze sectie zorgvuldig te controleren.

| | [Standaard SKU](load-balancer-standard-overview.md) | Basis-SKU |
| --- | --- | --- |
| Grootte van de back-end | Maximaal 1000 exemplaren. | Maximaal 100 exemplaren. |
| Back-end-pool-eindpunten | Hiermee stelt u een virtuele machine in één virtueel netwerk, met inbegrip van een combinatie van virtuele machines, beschikbaarheidssets en virtuele-machineschaalset. | Virtuele machines in een enkel beschikbaarheid ingesteld of virtuele-machineschaalset ingesteld. |
| Azure Beschikbaarheidszones | Zone-redundante en zonal front-ends voor inkomend en uitgaand, uitgaande stroomtoewijzingen zone storingen, taakverdeling cross-zone. | / |
| Diagnostiek | Azure Monitor multidimensionale metrische gegevens inclusief byte en pakket tellers, health test status, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (snat omzetten geslaagde en mislukte flows), actieve gegevens vlak metingen. | Azure Log Analytics voor openbare load balancer alleen, snat omzetten uitputting van de waarschuwing, back-end-pool health aantal. |
| HA poorten | Interne load balancer. | / |
| Standaard beveiligen | Standaard gesloten voor openbare IP-adres en de load balancer-eindpunten. Voor verkeer, moet een netwerkbeveiligingsgroep aan expliciet geaccepteerde entiteiten worden gebruikt. | Standaard opent, netwerkbeveiligingsgroep is optioneel. |
| Uitgaande verbindingen | Meerdere voorzijde eindigt met per regel opt-out. Een uitgaande scenario _moet_ expliciet voor de virtuele machine te kunnen gebruiken uitgaande verbinding worden gemaakt. [Virtueel netwerk service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) zonder uitgaande verbinding kan worden bereikt en niet meetellen voor gegevens die worden verwerkt. Alle openbare IP-adressen, inclusief Azure PaaS-services die niet beschikbaar omdat de service-eindpunten voor virtueel netwerk zijn, moeten worden bereikt via uitgaande verbinding en telt als een verwerkte gegevens. Wanneer u alleen een interne load balancer fungeert een virtuele machine, zijn uitgaande verbindingen via standaard snat omzetten niet beschikbaar. Uitgaande snat omzetten programmeren is transportprotocol specifieke, op basis van het protocol van de binnenkomende regel voor load balancing. | Één front-end, willekeurig worden geselecteerd als er meerdere front-ends aanwezig zijn. Wanneer een virtuele machine alleen een interne load balancer fungeert, wordt de standaard snat omzetten gebruikt. |
| Meerdere front-ends | Inkomend en uitgaand. | Alleen voor binnenkomend verkeer. |
| Beheerbewerkingen | De meeste bewerkingen < 30 seconden. | 60-90 seconden typische. |
| SLA | 99,99 procent voor een gegevenspad met twee orde virtuele machines. | Impliciete in de VM SLA. | 
| Prijzen | Kosten zijn gebaseerd op het aantal regels en gegevens die worden verwerkt voor binnenkomende of uitgaande die zijn gekoppeld aan de resource.  | Zonder kosten. |

Zie voor meer informatie [Servicelimieten voor de Load Balancer](https://aka.ms/lblimits). Zie voor meer informatie standaard Load Balancer [overzicht](load-balancer-standard-overview.md), [prijzen](https://aka.ms/lbpricing), en [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Concepten

### <a name = "publicloadbalancer"></a>Openbare load balancer

Een openbare load balancer wijst het openbare IP-adres en poort aantal binnenkomend verkeer naar het particuliere IP-adres en poort nummer van de virtuele machine en omgekeerd voor het verkeer van het antwoord van de virtuele machine. Door het toepassen van taakverdeling regels, kunt u specifieke typen verkeer verdelen over meerdere virtuele machines of services. Bijvoorbeeld, kunt u het laden van de aanvraag webverkeer spreiden tussen meerdere webservers.

De volgende afbeelding ziet een eindpunt met gelijke taakverdeling voor internetverkeer die wordt gedeeld door drie virtuele machines voor de openbare en persoonlijke TCP-poort 80. Deze drie virtuele machines zijn in een set met gelijke taakverdeling.

![openbare load balancer-voorbeeld](./media/load-balancer-overview/IC727496.png)

*Afbeelding: Taakverdeling webverkeer laden met behulp van een openbare load balancer*

Wanneer u internet-clients verzenden webpagina-aanvragen naar het openbare IP-adres van een web-app op TCP-poort 80, verdeelt Azure Load Balancer de aanvragen over de drie virtuele machines in de set met gelijke taakverdeling. Zie voor meer informatie over de algoritmen voor load balancer, het [overzichtspagina van load balancer](load-balancer-overview.md#load-balancer-features).

Azure Load Balancer distribueert standaard netwerkverkeer wordt evenredig verdeeld over meerdere VM-exemplaren. U kunt ook de affiniteit van de sessie configureren. Zie voor meer informatie [load balancer-distributie modus](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interne load balancer

Een interne load balancer wordt doorgestuurd verkeer alleen naar bronnen die zich in een virtueel netwerk of die een VPN-verbinding gebruikt voor toegang tot Azure-infrastructuur. In dit opzicht verschilt een interne load balancer van een openbare load balancer. Azure-infrastructuur beperkt toegang tot de taakverdeling front-end-IP-adressen van een virtueel netwerk. Front-end-IP-adressen en virtuele netwerken worden nooit rechtstreeks blootgesteld aan een internet-eindpunt. Interne line-of-business-toepassingen worden uitgevoerd in Azure en zijn toegankelijk vanuit Azure of via lokale bronnen.

Een interne load balancer biedt de volgende soorten taakverdeling:

* **Binnen een virtueel netwerk**: taakverdeling van VM's in het virtuele netwerk naar een set van virtuele machines die zich in hetzelfde virtuele netwerk bevinden.
* **Voor een virtueel netwerk met cross-premises**: taakverdeling van lokale computers aan een set van virtuele machines die zich in hetzelfde virtuele netwerk bevinden. 
* **Voor toepassingen met meerdere lagen**: Load balancing voor toepassingen met meerdere lagen internetgerichte waarin de back-end-lagen niet verbonden met internet zijn. De back-end-lagen vereisen verkeer taakverdeling van de internetgerichte servicetier (Zie de volgende afbeelding).
* **Voor line-of-business-toepassingen**: taakverdeling voor line-of-business-toepassingen die worden gehost in Azure zonder extra load balancer-hardware of software. Dit scenario omvat het on-premises servers die in de set van computers waarvan het verkeer verdeeld wordt.

![interne load balancer-voorbeeld](./media/load-balancer-overview/IC744147.png)

*Afbeelding: Taakverdeling toepassingen met meerdere lagen met behulp van beide openbare en interne load balancers*

## <a name="pricing"></a>Prijzen
Standaard gebruik van de Load Balancer wordt in rekening gebracht op basis van het aantal geconfigureerde taakverdeling regels en de hoeveelheid verwerkte binnenkomende en uitgaande gegevens. Voor een standaard Load Balancer prijsgegevens, gaat u naar de [Load Balancer prijzen](https://azure.microsoft.com/pricing/details/load-balancer/) pagina.

Basic Load Balancer wordt gratis aangeboden.

## <a name="sla"></a>SLA

Voor informatie over de standaard Load Balancer SLA, gaat u naar de [Load Balancer SLA](https://aka.ms/lbsla) pagina. 

## <a name="limitations"></a>Beperkingen

- Load Balancer is een TCP- of UDP-product voor taakverdeling en het doorsturen van de poort voor deze specifieke IP-protocollen.  Taakverdelingsregels en binnenkomende NAT-regels worden ondersteund voor TCP en UDP en wordt niet ondersteund voor andere IP-protocollen met inbegrip van ICMP. Load Balancer niet beëindigt, reageren of anders communiceren met de nettolading van een UDP of TCP-stroom. Het is niet een proxy. Geslaagde validatie van de verbinding met een front-plaats in-band moet nemen met hetzelfde protocol gebruikt in een load balancing of inkomende NAT-regel (TCP of UDP) _en_ ten minste één van uw virtuele machines moet een antwoord genereren voor een client een reactie van een front-zien.  Geen een in-band-antwoord ontvangen van de Load Balancer-front-Hiermee geeft u dat geen virtuele machines kunnen reageren.  Het is niet mogelijk om te communiceren met een Load Balancer-front-zonder een virtuele machine kunnen reageren.  Dit geldt ook voor uitgaande verbindingen waar [poort maskerade snat omzetten](load-balancer-outbound-connections.md#snat) wordt alleen ondersteund voor TCP en UDP; andere IP-protocollen inclusief ICMP ook mislukken.  Een instantieniveau openbare IP-adres toewijzen te verhelpen.
- In tegenstelling tot openbare Load Balancers die bieden [uitgaande verbindingen](load-balancer-outbound-connections.md) tijdens het veranderen van privé IP-adressen binnen het virtuele netwerk naar openbare IP-adressen, interne Load Balancers komen niet vertalen uitgaande afkomstig is verbindingen met de front-end van een interne Load Balancer als beide zijn in de privé IP-adresruimte.  Zo voorkomt u mogelijkheden voor bronuitputting snat omzetten in unieke interne IP-adresruimte waar de vertaling niet vereist is.  Een neveneffect is dat als een uitgaande stroom van een virtuele machine in de groep back-end probeert een stroom aan front-end van de interne Load Balancer in welke groep database _en_ is toegewezen naar zichzelf, beide zijden van de stroom komen niet overeen en de stroom mislukt .  Als de stroom niet terug naar dezelfde virtuele machine in de back-end-pool die de stroom voor de front-end gemaakt toegewezen is, slaagt de stroom.   Wanneer de stroom wordt toegewezen aan zichzelf de uitgaande stroom lijkt te zijn afkomstig uit de virtuele machine op de front- en de bijbehorende binnenkomende stroom lijkt te zijn afkomstig uit de virtuele machine naar zichzelf. Uit oogpunt van het Gastbesturingssysteem, niet overeenkomen met de binnenkomende en uitgaande delen van dezelfde stroom in de virtuele machine. De TCP-stack herkent deze helften van dezelfde stroom niet als deel van dezelfde stroom als de bron- en doelserver komen niet overeen.  Wanneer de stroom wordt toegewezen aan met eventuele andere virtuele machine in de back-end-pool, komt overeen met de helften van stroom en de virtuele machine met succes kan reageren op de stroom.  Het symptoom voor dit scenario is een onregelmatige verbinding time-outs. Er zijn enkele algemene oplossingen voor het op betrouwbare wijze bereiken van dit scenario (die afkomstig zijn stroomt van een back-end-pool naar de back-end voor groepen van respectieve interne Load Balancer front-end-) zoals beide invoegen van een derde partij proxy achter de interne Load Balancer of [met behulp van de stijlregels DSR](load-balancer-multivip-overview.md).  Terwijl u een openbare Load Balancer gebruiken kunt om te beperken, het resulterende scenario is de kans op [snat omzetten uitputting](load-balancer-outbound-connections.md#snat) en tenzij u zorgvuldig worden beheerd, moeten worden vermeden.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Azure Load Balancer. Maak een om te beginnen met het gebruik van een load balancer, virtuele machines maken met een aangepaste IIS-uitbreiding geïnstalleerd en toepassen van taakverdeling op de web-app tussen de virtuele machines. Voor meer informatie Zie de [een Basic Load Balancer maken](quickstart-create-basic-load-balancer-portal.md) Quick Start.
