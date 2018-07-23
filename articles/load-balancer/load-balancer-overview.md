---
title: Overzicht van Azure Load Balancer | Microsoft Docs
description: Overzicht van Azure Load Balancer-functies, -architectuur en -implementatie. Informatie over de werking van de load balancer en gebruikmaken van deze in de cloud.
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
ms.date: 07/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 4714a2792b30bfe5cb8a25f0b8912969858f5c34
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187084"
---
# <a name="what-is-azure-load-balancer"></a>Wat is Azure Load Balancer?

U kunt uw toepassingen schalen en hoge beschikbaarheid voor uw services te maken met Azure Load Balancer. Load Balancer biedt ondersteuning voor scenario's voor binnenkomend en uitgaand, lage latentie en hoge doorvoer en kan worden geschaald tot miljoenen stromen voor alle TCP en UDP-toepassingen.  

Load Balancer verdeelt het nieuwe binnenkomende stromen die op de load balancer-frontend back-end-pool-exemplaren op basis van regels en tests binnenkomen. 

Een openbare load balancer kunt uitgaande verbindingen voor virtuele machines (VM's) Bovendien bieden in uw virtuele netwerk door het vertalen van hun persoonlijke IP-adressen voor openbare IP-adressen.

Azure Load Balancer is beschikbaar in twee SKU's: Basic en Standard. Er zijn verschillen in schaal, functies en prijzen. Elk scenario dat er mogelijk is met de Basic Load Balancer kan ook worden gemaakt met de standaardversie van Load Balancer, hoewel de benaderingen mogelijk enigszins verschillen. Als u meer informatie over Load Balancer, is het belangrijk om vertrouwd te raken met de basisprincipes en de SKU-specifieke verschillen.

## <a name="why-use-load-balancer"></a>Waarom zou ik Load Balancer gebruiken? 

U kunt Azure Load Balancer te gebruiken:

* Taakverdeling inkomend internetverkeer naar uw virtuele machines. Deze configuratie wordt ook wel een [openbare load balancer](#publicloadbalancer).
* Verkeer verdelen over VM's binnen een virtueel netwerk. U kunt ook een load balancer-front-end van een on-premises netwerk in een hybride scenario bereiken. Beide scenario's gebruiken een configuratie die bekend als staat een [interne load balancer](#internalloadbalancer).
* Voorwaarts verkeer van poort naar een specifieke poort op specifieke virtuele machines met regels voor binnenkomende network address translation (NAT).
* Geef [uitgaande connectiviteit](load-balancer-outbound-connections.md) voor virtuele machines in uw virtuele netwerk met behulp van een openbare load balancer.


>[!NOTE]
> Azure biedt een reeks volledig beheerde oplossingen voor taakverdeling voor uw scenario's. Als u zoekt naar beëindiging van Transport Layer Security (TLS)-protocol ('SSL-offload') of per-HTTP/HTTPS-aanvraag, toepassingslaag verwerken, raadpleegt u [Application Gateway](../application-gateway/application-gateway-introduction.md). Als u voor globale DNS-taakverdeling, Controleer [Traffic Manager](../traffic-manager/traffic-manager-overview.md). Uw end-to-end scenario's kunnen profiteren van deze oplossingen worden gecombineerd als nodig is.

## <a name="what-are-load-balancer-resources"></a>Wat zijn de resources voor load balancer?

Een load balancer-resource kan bestaan als een openbare load balancer of een interne load balancer. De load balancer-resource-functies worden uitgedrukt als een front-end, een regel, een statustest en de definitie van een back-end-pool. U plaatsen virtuele machines in de back-endpool door de back-endpool van de virtuele machine op te geven.

Resources voor load balancer zijn waarin u hoe Azure de infrastructuur met meerdere tenants voor het scenario dat u wilt maken moet programma kunt express-objecten. Er is geen directe relatie tussen resources voor load balancer en de werkelijke infrastructuur. Het maken van een load balancer maakt een exemplaar niet en capaciteit is altijd beschikbaar. 

## <a name="fundamental-load-balancer-features"></a>Fundamentele voorzieningen van de Load Balancer

Load Balancer biedt de volgende fundamentele mogelijkheden voor TCP en UDP-toepassingen:

* **Taakverdeling**

    U kunt een taakverdelingsregel voor het distribueren van verkeer dat op frontend naar instanties van back-end-pool binnenkomt maken met Azure Load Balancer. Load Balancer gebruikt een op basis van een hash-algoritme voor distributie van inkomende stromen en herschrijft de headers van stromen naar back-end-groep exemplaren dienovereenkomstig. Een server is beschikbaar voor nieuwe stromen ontvangen wanneer een statustest geeft aan een goede back-endeindpunt dat.
    
    Load Balancer gebruikt standaard een 5-tuple hash die bestaat uit de bron-IP-adres, bronpoort, doel-IP-adres, doelpoort en IP-protocolnummer om toe te wijzen stromen servers beschikbaar zijn. U kunt de affiniteit met een specifieke bron-IP-adres maken door in een 2 - of 3-tuple hash voor een bepaalde regel. Alle pakketten van dezelfde pakket stroom binnenkomen op hetzelfde exemplaar van achter de load balancing front-end. De client initieert wanneer een nieuwe stroom vanuit de dezelfde bron-IP, de poort is gewijzigd. Als gevolg hiervan hebben de 5-tuple kan leiden tot het verkeer naar een andere back-endeindpunt.

    Zie voor meer informatie, [distributiemodus voor Load balancer](load-balancer-distribution-mode.md). De volgende afbeelding wordt weergegeven de hash-distributiepunt:

    ![Hash-gebaseerde distributiepunten](./media/load-balancer-overview/load-balancer-distribution.png)

    *Afbeelding: Hash-gebaseerde distributiepunten*

* **Doorsturen via poort**

    Met Load Balancer, kunt u een binnenkomende NAT-regel maken voor poort voorwaarts verkeer van een specifieke poort van een specifieke frontend-IP-adres op een specifieke poort van een specifieke back-end-exemplaar binnen het virtuele netwerk. Dit gebeurt ook door de dezelfde hash-distributiepunt als de taakverdeling. Algemene scenario's voor deze mogelijkheid zijn Remote Desktop Protocol (RDP) of Secure Shell (SSH)-sessies op afzonderlijke VM-exemplaren binnen het Virtueelnetwerk van Azure. U kunt meerdere interne eindpunten toewijzen aan de verschillende poorten op de dezelfde frontend-IP-adres. U kunt deze gebruiken voor het extern beheren van uw VM's via internet zonder de noodzaak voor een aanvullende jump-box.

* **Toepassing agnostische en transparante**

    Load Balancer niet rechtstreeks communiceren met TCP of UDP of niveau van de toepassing en eventuele TCP of UDP-toepassingsscenario kan worden ondersteund.  Load Balancer niet beëindigen of afkomstig zijn van stromen, communiceren met de nettolading van de stroom biedt geen functie application layer-gateway en protocol aantal handshakes altijd plaatsvinden rechtstreeks tussen de client en het exemplaar van de groep back-end.  Een reactie op een inkomende stroom is altijd een reactie van een virtuele machine.  Als de stroom is bereikt op de virtuele machine, wordt de oorspronkelijke bron-IP-adres ook behouden.  Een aantal voorbeelden ter illustratie van meer transparantie:
    - Elk eindpunt wordt alleen beantwoord door een virtuele machine.  Bijvoorbeeld, een TCP-handshake wordt altijd optreedt tussen de client en de geselecteerde back-end virtuele machine.  Een reactie op een aanvraag naar een front-end is een antwoord dat is gegenereerd door back-end virtuele machine. Wanneer u verbinding met een front-end is valideert, valideert u de end-to-end-connectiviteit met ten minste één back-end virtuele machine.
    - Toepassing nettoladingen zijn transparant voor de Load Balancer en alle UDP of TCP-toepassing kan worden ondersteund. Voor workloads waarvoor per HTTP-aanvraag verwerken of manipulatie van application layer nettoladingen (bijvoorbeeld bij het parseren van de HTTP-URL's), moet u een layer 7 load balancer, zoals [Application Gateway](https://azure.microsoft.com/services/application-gateway).
    - Omdat de Load Balancer is agnostisch ten opzichte van de TCP-nettolading en TLS-offload ('SSL') niet is opgegeven, kunt u end-to-end versleutelde scenario's met behulp van de Load Balancer maken en grote scale-out voor TLS-toepassingen krijgen door de TLS-verbinding op de virtuele machine zelf wordt beëindigd.  De toetsen capaciteit van de TLS-sessie wordt bijvoorbeeld alleen beperkt door het type en aantal VM's die u aan de back-endpool toevoegt.  Als u "SSL-offloading", application layer behandeling of wilt overdragen van het beheren van certificaten in Azure nodig hebt, moet u de Azure load balancer uit laag 7 [Application Gateway](https://azure.microsoft.com/services/application-gateway) in plaats daarvan.
        

* **Automatische herconfiguratie**

    Load Balancer herconfigureert zichzelf direct wanneer u omhoog of omlaag schalen van exemplaren. Toevoegen of verwijderen van virtuele machines uit de back-endpool herconfigureert de load balancer zonder aanvullende bewerkingen op de load balancer-resource.

* **Statuscontroles**

     Load Balancer gebruikt om te bepalen van de status van exemplaren in de back-endpool, statuscontroles die u definieert. Wanneer een test niet reageert, stopt de load balancer nieuwe verbindingen te verzenden naar de exemplaren. Bestaande verbindingen worden niet beïnvloed en blijven totdat de toepassing wordt de stroom beëindigd er treedt een time-out voor inactiviteit op, of de virtuele machine wordt afgesloten.

    Drie typen van de tests worden ondersteund:

    - **Aangepaste HTTP-test**: U kunt deze test gebruiken om te maken van uw eigen aangepaste logica om te bepalen van de status van een exemplaar van de groep back-end. De load balancer controleert regelmatig uw eindpunt (elke 15 seconden, standaard). Het exemplaar wordt beschouwd als in orde zijn als met een HTTP-200 binnen de time-outperiode (standaard 31 seconden reageert). Elke status dan HTTP 200 zorgt ervoor dat deze test mislukt. Deze test is ook handig voor het implementeren van uw eigen logica voor de exemplaren uit de load balancer-rotatie verwijderen. U kunt bijvoorbeeld configureren dat de instantie de status van een niet-200 retourneren als het exemplaar groter is dan 90 procent CPU.  Deze test heeft voorrang op de standaard guest agent-test.

    - **Aangepaste test TCP**: deze test is afhankelijk van het tot stand brengen van een geslaagde TCP-sessie met een gedefinieerde testpoort. Als de opgegeven listener op de virtuele machine bestaat, wordt deze test slaagt. Als de verbinding is geweigerd, mislukt de test. Deze test heeft voorrang op de standaard guest agent-test.

    - **Test voor Guest agent**: de load balancer kan ook gebruikmaken van de gastagent binnen de virtuele machine. De gastagent luistert en reageert met een HTTP 200 OK antwoord alleen wanneer het exemplaar in de status gereed heeft is. Als de agent niet reageert met een HTTP 200 OK, wordt de load balancer markeert het exemplaar als niet-reagerende en verkeer verzenden naar dat exemplaar stopt. De load balancer blijft probeert te krijgen tot het exemplaar. Als de guest-agent met een HTTP-200 reageert, stuurt de load balancer verkeer opnieuw naar dat exemplaar. Gast-agent tests zijn een _laatste redmiddel en wordt niet aanbevolen_ wanneer HTTP of TCP-test met aangepaste configuraties mogelijk zijn. 
    
* **Uitgaande verbindingen (SNAT)**

    Alle uitgaande stromen van privé IP-adressen in uw virtuele netwerk naar openbare IP-adressen op internet kunnen worden omgezet in een frontend-IP-adres van de load balancer. Wanneer de front-end van een openbare is gekoppeld aan een back-end virtuele machine in een regel voor taakverdeling, Azure-programma's uitgaande verbindingen moeten worden automatisch omgezet in het openbaar front-end-IP-adres.

    * Eenvoudig upgraden en herstel na noodgevallen van services, niet inschakelen omdat de front-end kan dynamisch worden toegewezen aan een ander exemplaar van de service.
    * Gemakkelijker besturingselement toegangsbeheerlijst (ACL) toegangsbeheer op. ACL's uitgedrukt in termen van front-end IP-adressen veranderen niet als services schaal omhoog of omlaag of ophalen opnieuw geïmplementeerd.  Vertalen van de uitgaande verbindingen met een kleiner aantal IP-adressen dan machines de last van opname in de whitelist verminderen kunnen.

    Zie voor meer informatie, [uitgaande verbindingen](load-balancer-outbound-connections.md).

Standard Load Balancer heeft extra SKU-specifieke mogelijkheden buiten deze basisprincipes. Bekijk de rest van dit artikel voor meer informatie.

## <a name="skus"></a> Vergelijking van de Load Balancer-SKU

Load Balancer biedt ondersteuning voor basis en standaard-SKU's, elk waarbij in scenario gewenste schaal, functies en prijzen. Elk scenario dat er mogelijk is met de Basic Load Balancer kan ook worden gemaakt met de standaardversie van Load Balancer. De API's voor beide SKU's zijn in feite vergelijkbare en aangeroepen via de specificatie van een SKU. De API voor de ondersteuning voor SKU's voor de Load Balancer en het openbare IP-adres is beschikbaar vanaf de 2017-08-01-API. Beide SKU's hebben de dezelfde algemene API en de structuur.

Echter, afhankelijk van welke SKU u kiest, de configuratie van de volledige scenario mogelijk enigszins anders. Documentatie voor Load Balancer worden aangeroepen wanneer een artikel alleen voor een specifieke SKU geldt. Als u wilt vergelijken en te weten wat de verschillen, Zie de volgende tabel. Zie voor meer informatie, [overzicht van Standard Load Balancer](load-balancer-standard-overview.md).

>[!NOTE]
> Nieuwe ontwerpen vaststellen Standard Load Balancer. 

Zelfstandige virtuele machines, beschikbaarheidssets en virtuele-machineschaalsets kunnen worden verbonden met slechts één SKU, niet beide. Als u ze met openbare IP-adressen gebruikt, zowel Load Balancer en het openbare IP-adres SKU moet overeenkomen. Load Balancer en openbare IP-SKU's zijn niet veranderlijke.

_Het is een aanbevolen procedure om op te geven van de SKU's expliciet, zelfs als deze nog niet verplicht._  Op dit moment zijn vereiste wijzigingen tot een minimum wordt beperkt. Als een SKU niet opgegeven is, wordt dit geïnterpreteerd als een voornemen gebruik van de 2017-08-01-API-versie van de basis-SKU.

>[!IMPORTANT]
>Standard Load Balancer is een nieuw product in de Load Balancer en grotendeels een hoofdverzameling van de basisversie van Load Balancer. Er zijn belangrijke en doelbewuste verschillen tussen de twee producten. Een end-to-end-scenario dat er mogelijk is met de Basic Load Balancer kan ook worden gemaakt met de standaardversie van Load Balancer. Als u bent al voor Basic Load Balancer gebruikt, zorg ervoor dat u met de standaardversie van Load Balancer om de meest recente wijzigingen in werking tussen Standard en Basic en hun invloed te begrijpen. In deze sectie zorgvuldig te controleren.

| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| Grootte van de back-end | maximaal 1000 instanties | maximaal 100 exemplaren |
| Back-end-pool eindpunten | elke virtuele machine in één virtueel netwerk, met inbegrip van de combinatie van virtuele machines, beschikbaarheidssets, virtuele-machineschaalsets. | virtuele machines in een beschikbaarheid van één set of virtuele machine schaal |
| Beschikbaarheidszones | zone-redundante en zonegebonden front-ends voor inkomend en uitgaand, uitgaande stromen toewijzingen zone storingen, taakverdeling tussen zones | / |
| Diagnostiek | Azure Monitor, multi-dimensionale metrische gegevens, waaronder bytes en pakket-tellers, health probe status, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (SNAT geslaagde en mislukte stromen), actieve gegevens vlak metingen | Azure Log Analytics voor een openbare Load Balancer, SNAT uitputting van waarschuwing, back-end-pool health tellen |
| Maximaal beschikbare poorten | een interne Load Balancer | / |
| Standaard beveiligen | standaard gesloten voor openbare IP-adres en de Load Balancer-eindpunten en een netwerkbeveiligingsgroep moet worden gebruikt om expliciet whitelist voor verkeer stromen | open, standaardnetwerkbeveiligingsgroep optioneel |
| [Uitgaande verbindingen](load-balancer-outbound-connections.md) | Meerdere front-ends met per regel opt-out voor taakverdeling. Een uitgaande scenario _moet_ expliciet worden gemaakt voor de virtuele machine om te kunnen gebruiken uitgaande connectiviteit.  [VNet-Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) kan worden bereikt zonder de uitgaande connectiviteit en tellen niet mee met gegevens die worden verwerkt.  Alle openbare IP-adressen, met inbegrip van Azure PaaS-services die niet beschikbaar als VNet-Service-eindpunten, moeten worden bereikt via de uitgaande connectiviteit en het aantal voor gegevens die worden verwerkt. Bij een interne Load Balancer een virtuele machine levert is, zijn uitgaande verbindingen via standaard SNAT niet beschikbaar. Uitgaande SNAT programmeren is transportprotocol bepaald op basis van het protocol van de inkomende regel voor taakverdeling. | Één front-end willekeurig worden geselecteerd als er meerdere front-ends aanwezig zijn.  Wanneer u alleen een interne Load Balancer is een virtuele machine fungeert, wordt standaard SNAT wordt gebruikt. |
| [Meerdere frontends](load-balancer-multivip-overview.md) | Inkomende en [uitgaande](load-balancer-outbound-connections.md) | Alleen binnenkomende gegevens |
| [Statustest omlaag gedrag](load-balancer-custom-probe-overview.md) | TCP-verbindingen op exemplaar test omlaag overleven __en__ op alle tests omlaag | TCP-verbindingen blijven op exemplaar test omlaag. Alle TCP-verbindingen wordt beëindigd op alle tests omlaag |
| Beheerbewerkingen | De meeste bewerkingen < 30 seconden | 60-90 seconden typische |
| SLA | 99,99% voor het gegevenspad met twee in orde virtuele machines | Impliciete in VM SLA | 
| Prijzen | Kosten in rekening gebracht op basis van het aantal regels, gegevens verwerkte binnenkomende of uitgaande die zijn gekoppeld aan de resource  | Er zijn geen kosten in rekening gebracht |

Zie voor meer informatie, [Servicelimieten voor Load Balancer](https://aka.ms/lblimits). Zie voor meer informatie Standard Load Balancer, [overzicht](load-balancer-standard-overview.md), [prijzen](https://aka.ms/lbpricing), en [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Concepten

### <a name = "publicloadbalancer"></a>Openbare load balancer

Een openbare load balancer wijst het openbare IP-adres en poort nummer van het binnenkomende verkeer naar het privé IP-adres en poort nummer van de virtuele machine, en vice versa voor de antwoordverkeer van de virtuele machine. Door het toepassen van regels voor taakverdeling, kunt u specifieke soorten verkeer verdelen over meerdere virtuele machines of services. U kunt bijvoorbeeld de belasting van de aanvraag webverkeer verdeeld over meerdere webservers.

De volgende afbeelding ziet u een load balancing-eindpunt voor webverkeer te genereren die wordt gedeeld door drie virtuele machines voor de openbare en persoonlijke TCP-poort 80. Deze drie VM's zijn in een load balancer-set.

![Voorbeeld van de openbare load balancer](./media/load-balancer-overview/IC727496.png)

*Afbeelding: Laden Netwerktaakverdeling webverkeer te genereren met behulp van een openbare load balancer*

Als internetclients webpagina-aanvragen naar het openbare IP-adres van een web-app op TCP-poort 80 verzendt, wordt de aanvragen in Azure Load Balancer verspreid over de drie virtuele machines in de load balancer-set. Zie voor meer informatie over load balancer-algoritmen, de [load balancer-functies](load-balancer-overview.md##fundamental-load-balancer-features) sectie van dit artikel.

Standaard verdeelt Azure Load Balancer netwerkverkeer wordt evenredig verdeeld over meerdere VM-exemplaren. U kunt ook de sessie-affiniteit configureren. Zie voor meer informatie, [distributiemodus voor load balancer](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interne load balancer

Een interne load balancer zorgt ervoor dat verkeer alleen naar resources die zich binnen een virtueel netwerk of een VPN-verbinding gebruiken voor toegang tot Azure-infrastructuur. In dit opzicht is een interne load balancer wijkt af van een openbare load balancer. Azure-infrastructuur wordt beperkt tot de netwerktaakverdeling frontend-IP-adressen van een virtueel netwerk. frontend-IP-adressen en virtuele netwerken worden nooit rechtstreeks blootgesteld aan een internet-eindpunt. Interne line-of-business-toepassingen uitvoeren in Azure en worden gebruikt in Azure of van on-premises bronnen.

Een interne load balancer kunt de volgende typen taakverdeling:

* **Binnen een virtueel netwerk**: de taakverdeling van virtuele machines in het virtuele netwerk naar een set van virtuele machines die zich in hetzelfde virtuele netwerk bevinden.
* **Voor een virtueel netwerk met cross-premises**: de taakverdeling van on-premises computers voor een set van virtuele machines die zich in hetzelfde virtuele netwerk bevinden. 
* **Voor toepassingen met meerdere lagen**: taakverdeling voor toepassingen met meerdere lagen internetgerichte waarin de back-end-lagen niet verbonden met internet zijn. De back-end-lagen vereisen verkeer load-balancing laag van de internetgerichte (Zie de volgende afbeelding).
* **Voor line-of-business-toepassingen**: taakverdeling voor line-of-business-toepassingen die worden gehost in Azure zonder extra load balancer-hardware of software. Dit geldt ook voor on-premises servers die zich in de set computers waarvan het verkeer verdeeld wordt.

![Voorbeeld van de interne load balancer](./media/load-balancer-overview/IC744147.png)

*Afbeelding: De taakverdeling van toepassingen met meerdere lagen met behulp van zowel openbare en interne load balancers*

## <a name="pricing"></a>Prijzen
Standard wordt Load Balancer verrekend op basis van het aantal geconfigureerde regels voor taakverdeling en de hoeveelheid verwerkte binnenkomende en uitgaande gegevens. Voor Standard Load Balancer prijsinformatie, gaat u naar de [Load Balancer prijzen](https://azure.microsoft.com/pricing/details/load-balancer/) pagina.

Basic Load Balancer wordt gratis aangeboden.

## <a name="sla"></a>SLA

Voor informatie over de Standard Load Balancer SLA, gaat u naar de [Load Balancer SLA](https://aka.ms/lbsla) pagina. 

## <a name="limitations"></a>Beperkingen

- Load Balancer is een TCP of UDP-product voor taakverdeling en doorsturen via poort voor deze specifieke IP-protocollen.  Taakverdelingsregels en inkomende NAT-regels worden ondersteund voor TCP en UDP en wordt niet ondersteund voor andere IP-protocollen met inbegrip van ICMP. Load Balancer niet beëindigt, reageren of anders communiceren met de nettolading van een UDP of TCP-stroom. Het is niet een proxy. Succesvolle validatie van connectiviteit naar een frontend plaats in-band moet uitvoeren met hetzelfde protocol gebruikt in een load balancing of inkomende NAT-regel (TCP of UDP) _en_ moet ten minste één van uw virtuele machines een antwoord genereren voor een client Zie een reactie van een frontend.  Niet ontvangen van een in-band-antwoord van de Load Balancer-frontend geeft aan dat er waren geen virtuele machines kunnen reageren.  Het is niet mogelijk om te communiceren met een Load Balancer-frontend zonder een virtuele machine kunnen reageren.  Dit geldt ook voor uitgaande verbindingen waar [poort maskerade SNAT](load-balancer-outbound-connections.md#snat) wordt alleen ondersteund voor TCP en UDP-; geen andere IP-protocollen met inbegrip van ICMP ook mislukken.  Toewijzen van een openbaar IP-adres op exemplaarniveau te beperken.
- In tegenstelling tot openbare Load Balancers waardoor [uitgaande verbindingen](load-balancer-outbound-connections.md) bij het overstappen van privé-IP-adressen binnen het virtuele netwerk naar openbare IP-adressen, interne Load Balancers niet doen vertalen uitgaande afkomstig is verbindingen met de front-end van een interne Load Balancer als beide zijn in privé-IP-adresruimte.  Hiermee voorkomt u mogelijkheden voor uitputting van SNAT in unieke interne IP-adresruimte waarbij vertaling niet vereist is.  Een neveneffect is dat als een uitgaande stroom vanuit een virtuele machine in de back-endpool probeert een stroom aan de front-end van de interne Load Balancer in die groep zich bevindt _en_ is toegewezen naar zichzelf, beide zijden van de stroom komen niet overeen en de stroom mislukt.  Als de stroom niet terug naar dezelfde virtuele machine in de back-endpool waarop de stroom aan de front-end hebt gemaakt toegewezen is, wordt de stroom slaagt.   Wanneer de stroom wordt toegewezen aan zichzelf de uitgaande stroom lijkt te zijn afkomstig uit de virtuele machine aan de front-end en de bijbehorende inkomende stroom lijkt te zijn afkomstig uit de virtuele machine naar zichzelf. Uit oogpunt van het Gastbesturingssysteem, niet overeenkomen met de binnenkomende en uitgaande delen van dezelfde stroom in de virtuele machine. Deze helften van dezelfde stroom wordt niet herkend als deel van dezelfde stroom als de bron- en doelserver komen niet overeen voor de TCP-stack.  Wanneer de stroom wordt toegewezen aan met een andere virtuele machine in de back-endpool, komt overeen met de halve van de stroom en de stroom is de virtuele machine kan reageren.  Het symptoom voor dit scenario is onregelmatige verbindingstime-outs. Er zijn enkele algemene oplossingen voor het bereiken van dit scenario (die afkomstig zijn van stromen van een back-endpool naar de back-end-pools respectieve interne Load Balancer front-end) op betrouwbare wijze waaronder beide invoegen van een derde partij proxy achter de interne Load Balancer of [met behulp van de stijlregels DSR](load-balancer-multivip-overview.md).  Terwijl u een openbare Load Balancer gebruiken kunt om te beperken, het resulterende scenario is de kans op [SNAT bronuitputting](load-balancer-outbound-connections.md#snat) en moeten worden vermeden, tenzij zorgvuldig worden beheerd.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Azure Load Balancer. Om te beginnen met het gebruik van een load balancer, maakt u er een, virtuele machines maken met een aangepaste IIS-extensie geïnstalleerd, en taken verdelen tussen de virtuele machines van de web-app. Voor meer informatie Zie de [maken van een Basic Load Balancer](quickstart-create-basic-load-balancer-portal.md) Quick Start.
