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
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: 4f46e796ff1ab85c0061c70ff9a725a6945a4f5d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-load-balancer-overview"></a>Overzicht van Azure Load Balancer

Azure Load Balancer kunt u hoge beschikbaarheid voor uw services maken en schalen van uw toepassingen. Load Balancer ondersteunt zowel inkomende als uitgaande scenario's, en biedt lage latentie en hoge doorvoer en schaalt miljoenen stromen voor alle TCP en UDP-toepassingen.   

Load Balancer wordt nieuwe inkomende stromen die binnenkomen op de load balancer-frontend naar exemplaren van de back-end van toepassingen volgens de regels en statuscontroles distribueren.  

Een openbare Load Balancer kunt daarnaast ook opgeven voor uitgaande verbindingen voor virtuele machines binnen het virtuele netwerk doordat hun persoonlijke IP-adressen naar openbare IP-adressen.

Azure Load Balancer is beschikbaar in twee verschillende SKU's: Basic en Standard.  Er zijn verschillen in schaal, functies en prijzen.  Elk scenario mogelijk met Basic Load Balancer kan ook worden gemaakt met Standard Load Balancer, hoewel de aanpak mogelijk enigszins anders.  Als u meer informatie over de Load Balancer, is het belangrijk om vertrouwd te raken met de basisprincipes en SKU-specifieke verschillen.

## <a name="why-use-load-balancer"></a>Waarom Load Balancer gebruiken? 

Azure Load Balancer kan worden gebruikt om:

* Binnenkomend internetverkeer saldo aan virtuele machines worden geladen. Deze configuratie wordt ook wel een [openbare Load Balancer](#publicloadbalancer).
* Load balance verkeer tussen virtuele machines binnen een virtueel netwerk. U kunt ook een Load Balancer-frontend uit een on-premises netwerk in een hybride scenario bereiken. Beide scenario's bij het gebruik van een configuratie die wordt ook wel een [interne Load Balancer](#internalloadbalancer).
* Doorsturen netwerkverkeer via de poort op een specifieke poort op een specifieke virtuele machines met binnenkomende NAT-regels.
* Geef [uitgaande verbinding](load-balancer-outbound-connections.md) voor virtuele machines binnen het virtuele netwerk met behulp van een openbare Load Balancer.


>[!NOTE]
> Azure biedt een reeks volledig beheerde oplossingen voor uw scenario's voor taakverdeling.  Als u TLS-beëindiging ('SSL-offload') of HTTP/HTTPS application layer verwerking zoekt, Raadpleeg [Application Gateway](../application-gateway/application-gateway-introduction.md).  Als u op zoek bent voor globale DNS taakverdeling, Bekijk [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Uw end-to-end-scenario's kunnen profiteren van deze oplossingen combineren indien nodig.

## <a name="what-is-load-balancer"></a>Wat is load balancer?

Een Load Balancer-bron kan bestaan als een openbare Load Balancer of een interne Load Balancer. Functies van de bron van de Load Balancer worden uitgedrukt als een frontend, een regel, een health test en de definitie van een back-end-adresgroep.  Virtuele machines worden in de back-endpool geplaatst door te geven van de back-endpool van de virtuele machine.

Load Balancer resources zijn waarbinnen u hoe de multitenant-infrastructuur voor het scenario dat u wilt maken op Azure moet programma kunt express-objecten.  Er is geen directe relatie tussen resources van de Load Balancer en werkelijke infrastructuur; geen exemplaar maken van een Load Balancer niet maken en capaciteit altijd beschikbaar is. 

## <a name="fundamental-load-balancer-features"></a>Fundamentele voorzieningen van de Load Balancer

Load Balancer biedt de volgende fundamentele mogelijkheden voor TCP en UDP-toepassingen:

* **Taakverdeling**

    Azure Load Balancer kunt u een taakverdelingsregel voor het distribueren van verkeer dat binnenkomt bij een frontend voor exemplaren van back-end-pool maken.  Wordt gebruikt op basis van het hash-algoritme voor distributie van inkomende stromen en herschrijft de headers van stromen voor exemplaren van back-end-pool dienovereenkomstig. Een server is beschikbaar voor het ontvangen van nieuwe stromen wanneer de test status geeft aan dat een goede back-end-eindpunt.
    
    Standaard wordt een 5-tuple hash bestaat uit de bron-IP-adres, bronpoort, IP-doeladres, doelpoort en IP-protocolnummer gebruikt stromen om toe te wijzen beschikbare servers.  U kunt de affiniteit met een specifiek IP-adres maken door naar een 2 of 3 tuple hash voor een bepaalde regel uitschakelen.  Alle pakketten van dezelfde stroom van het pakket binnenkomen op hetzelfde exemplaar achter de frontend taakverdeling.  De client initieert wanneer een nieuwe stroom van de dezelfde bron-IP, de poort is gewijzigd. De resulterende 5-tuple kan ertoe leiden dat het verkeer naar een andere back-end-eindpunt als gevolg hiervan.

    Zie voor meer informatie [Load balancer-distributie modus](load-balancer-distribution-mode.md). De volgende afbeelding toont de distributie op basis van een hash:

    ![Hash-distributiepunt](./media/load-balancer-overview/load-balancer-distribution.png)

    *Afbeelding - Hash-based distribution*

* **Poort doorsturen**

    Azure Load Balancer kunt u het maken van een binnenkomende NAT-regel voor doorsturen netwerkverkeer via de poort van een specifieke poort van een specifieke frontend-IP-adres op een specifieke poort van een specifieke back-end-exemplaar binnen het virtuele netwerk. Dit is ook doen door de dezelfde hash-distributiepunt als taakverdeling.  Algemene scenario's voor deze mogelijkheid zijn Remote Desktop Protocol (RDP) of Secure Shell (SSH)-sessies met afzonderlijke virtuele machine-exemplaren binnen het virtuele netwerk.  U kunt meerdere interne eindpunten toewijzen aan de andere poorten op de dezelfde frontend-IP-adres. U kunt deze gebruiken voor het extern beheren van uw virtuele machines via Internet zonder de noodzaak van een extra korte inleiding in.

* **Toepassing agnostisch en transparante**

    Load Balancer communiceert rechtstreeks niet met TCP of UDP of de toepassingslaag en eventuele TCP of UDP-toepassingsscenario kan worden ondersteund.  Bijvoorbeeld, terwijl de Load Balancer TLS zelf niet beëindigt, kunt u bouwen en TLS-toepassingen met behulp van de Load Balancer uitbreiden en de TLS-verbinding op de virtuele machine zelf beëindigen. Load Balancer niet beëindigt een stroom en protocol handshakes zijn altijd rechtstreeks tussen de client en het exemplaar van de groep back-end-hash is geselecteerd. Bijvoorbeeld, is een TCP-handshake altijd tussen de client en de geselecteerde back-end virtuele machine.  En een reactie op een aanvraag naar een frontend heeft een indeling die is gegenereerd op basis van de back-end virtuele machine.  De Load Balancer uitgaande netwerkervaring voor prestaties is alleen beperkt door de virtuele machine SKU die u kiest en stromen blijft actief gedurende een langere tijd als de time-out voor inactiviteit is nooit bereikt.

* **Automatische herconfiguratie**

    Azure Load Balancer opnieuw onmiddellijk als u exemplaren omhoog of omlaag schalen. Het toevoegen of verwijderen van virtuele machines van de back-endpool opnieuw geconfigureerd de load balancer zonder aanvullende bewerkingen op de Load Balancer-resource.

* **Statuscontroles**

    Azure Load Balancer gebruikmaakt van statuscontroles definieert u om te bepalen van de status van exemplaren in de back-endpool. Wanneer een test niet reageert, stopt de load balancer nieuwe verbindingen verzenden naar de slechte exemplaren. Bestaande verbindingen zijn niet van invloed op en gaat door totdat de toepassing de stroom een niet-actieve time-out optreedt beëindigt, of de virtuele machine wordt afgesloten.

    Drie soorten tests worden ondersteund:

    - **Aangepaste HTTP-test:** kunt u het maken van uw eigen aangepaste regels om te bepalen van de status van een exemplaar van de groep back-end. De load balancer wordt regelmatig probe voor het eindpunt (elke 15 seconden, standaard). Het exemplaar wordt beschouwd als in orde als deze met een HTTP 200 binnen de time-outperiode (standaard 31 seconden reageert). Elke andere status dan HTTP 200 zorgt ervoor dat deze test mislukt.  Dit is ook nuttig voor het implementeren van uw eigen logica voor het verwijderen van exemplaren van de load balancer worden gedraaid. U kunt bijvoorbeeld configureren dat het exemplaar om de status van een niet-200 retourneren als het exemplaar hoger dan 90% CPU is.   Deze test overschrijft de standaard guest agent-test.

    - **Aangepaste TCP-test:** deze test is afhankelijk van geslaagde TCP-sessie tot stand brengen van een poort gedefinieerde test.  Als de opgegeven listener op de virtuele machine bestaat, wordt deze test slaagt. Als de verbinding is geweigerd, mislukt de test. Deze test overschrijft de standaard guest agent-test.

    - **Test van de Guest-agent (op Platform als een Service alleen virtuele Machines):** de gastagent op de virtuele machine kan ook gebruikmaken van de load balancer. De gastagent luistert en reageert met een HTTP 200 OK antwoord wanneer het exemplaar in de status gereed. Als de agent niet reageert met een HTTP 200 OK, wordt de load balancer markeert de instantie als niet-reagerende en stopt verkeer kunnen verzenden naar dit exemplaar. De load balancer blijft probeert te krijgen tot het exemplaar. Als u de guest-agent reageert met een HTTP 200, wordt de load balancer verkeer verzenden naar dit exemplaar opnieuw.  Guest agent tests een laatste toevlucht zijn en mag niet worden gebruikt als HTTP- of TCP aangepaste test configuraties mogelijk zijn. 
    
* **Uitgaande verbindingen (bron NAT)**

    Alle uitgaande stroomt van privé IP-adressen binnen het virtuele netwerk naar openbare IP-adressen op Internet kunnen worden omgezet in een frontend-IP-adres van de Load Balancer. Wanneer een openbaar front-end is gekoppeld aan een back-end virtuele machine in een regel voor taakverdeling, programma Azure uitgaande verbindingen moeten worden automatisch omgezet in het openbaar front-end-IP-adres. Dit wordt ook bron NAT (snat omzetten) genoemd. Snat omzetten biedt belangrijke voordelen:

    + Hiermee kunt easy upgrade en herstel na noodgevallen van services, aangezien de frontend kan dynamisch worden toegewezen aan een ander exemplaar van de service.
    + Dit maakt het eenvoudiger toegangsbeheer toegangsbeheerlijst (ACL). ACL's die zijn uitgedrukt in termen van frontend-IP-adressen niet wijzigen als services schaal omhoog, omlaag of ophalen geïmplementeerd.

    Raadpleeg [uitgaande verbindingen](load-balancer-outbound-connections.md) artikel voor een gedetailleerde discussie over deze mogelijkheid.

Standaard Load Balancer heeft extra SKU-specifieke mogelijkheden buiten deze basisprincipes.  Bekijk de rest van dit artikel voor meer informatie.

## <a name="skus"></a> Vergelijking van de Load Balancer-SKU

Azure Load Balancer ondersteunt twee verschillende SKU's: Basic en Standard.  Er zijn verschillen in scenario schaal, functies en prijzen.  Elk scenario mogelijk met Basic Load Balancer kan ook worden gemaakt met standaard Load Balancer.  De API's voor beide SKU's zijn in feite vergelijkbare en aangeroepen via de specificatie van een SKU.  De API voor de ondersteuning van SKU's voor Load Balancer en openbare IP-adres is beschikbaar vanaf de 2017-08-01-API.  Beide SKU's hebben de dezelfde algemene API en de structuur.

Echter, afhankelijk van die SKU is gekozen, afwijken het volledige scenario configuratiedetails enigszins. De Load Balancer-documentatie illustreert wanneer een artikel van toepassing op een specifieke SKU alleen. Bekijk de volgende tabel hieronder om te vergelijken en te weten over de verschillen.  Bekijk [standaard Load Balancer Overview](load-balancer-standard-overview.md) voor meer informatie.

>[!NOTE]
> Nieuwe ontwerpen moeten standaard Load Balancer gebruiken. 

Zelfstandige virtuele machines, beschikbaarheidssets en virtuele-machineschaalsets kunnen alleen worden verbonden met een SKU, niet beide. Bij gebruik met openbare IP-adressen is, zowel de Load Balancer en de openbare IP-adres SKU moet overeenkomen. Load Balancer en openbare IP-SKU's zijn niet veranderlijke.

_Het is een best practice om op te geven de SKU's expliciet, zelfs als deze nog niet verplicht._  Op dit moment zijn vereiste wijzigingen tot een minimum wordt beperkt. Als een SKU niet opgegeven is, wordt dit geïnterpreteerd als de bedoeling basis-SKU gebruiken in de 2017-08-01-API-versie.

>[!IMPORTANT]
>Standaard Load Balancer is een nieuw product van de Load Balancer en grotendeels een hoofdverzameling van Basic Load Balancer.  Er zijn belangrijk en opzettelijk verschillen tussen beide producten.  Een end-to-end-scenario met Basic Load Balancer mogelijk kan worden gemaakt met Standard Load Balancer.  Als u al met Basic Load Balancer hebt gebruikt, moet u dat vertrouwd raken met standaard Load Balancer te begrijpen grote wijzigingen in gedrag tussen standaard en Basic en de invloed ervan. Deze sectie zorgvuldig te controleren.

| | [Standaard SKU](load-balancer-standard-overview.md) | Basis-SKU |
| --- | --- | --- |
| Grootte van de back-end | maximaal 1000 exemplaren | maximaal 100 exemplaren|
| Back-end-pool-eindpunten | Een virtuele machine in één virtueel netwerk, met inbegrip van de combinatie van virtuele machines, beschikbaarheidssets, virtuele-machineschaalset wordt ingesteld. | virtuele machines in een enkel beschikbaarheid instellen of de virtuele machine schaal instellen |
| Beschikbaarheidszones | Zone-redundante en zonal frontends voor inkomend en uitgaand, uitgaande stromen toewijzingen zone storingen, taakverdeling cross-zone | / |
| Diagnostiek | Azure Monitor multidimensionale metrische gegevens inclusief byte en pakket tellers, health test status, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (snat omzetten geslaagde en mislukte flows), actieve gegevens vlak metingen | Azure Log analytics voor openbare Load Balancer, snat omzetten uitputting van de waarschuwing, status aantal back-end Pools |
| HA poorten | Interne Load Balancer | / |
| Standaard beveiligen | openbare IP-adres en de Load Balancer-eindpunten standaard altijd gesloten, netwerkbeveiligingsgroep gebruikt voor het expliciet geaccepteerde | Standaardwaarden openen, optionele netwerkbeveiligingsgroep |
| Uitgaande verbindingen | Meerdere frontends per regel opt-out. Koppeling van een virtuele machine met een uitgaand adres _moet_ expliciet worden gemaakt.  Dit omvat connectiviteit met andere Azure-PaaS-services of [VNet Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) moet worden gebruikt. Uitgaande verbindingen via standaard snat omzetten zijn niet beschikbaar wanneer een interne Load Balancer een virtuele machine fungeert. | Één frontend. Standaard-snat omzetten wordt gebruikt wanneer een virtuele machine alleen interne Load Balancer fungeert |
| Meerdere frontends | Binnenkomend en uitgaand | Alleen binnenkomende gegevens |
| Bewerkingen | De meeste bewerkingen < 30 seconden | 60-90 seconden typische |
| SLA | 99,99% voor gegevenspad met twee virtuele machines die in orde | Impliciete in VM SLA | 
| Prijzen | Kosten in rekening gebracht op basis van regels, verwerkt gegevens binnenkomende of uitgaande die zijn gekoppeld aan een resource  | Gratis |

Bekijk [Servicelimieten voor de Load Balancer](https://aka.ms/lblimits).  Standaard Load Balancer ook Raadpleeg voor meer gedetailleerde [overzicht](load-balancer-standard-overview.md), [prijzen](https://aka.ms/lbpricing), en [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Concepten

### <a name = "publicloadbalancer"></a>Openbare Load Balancer

Openbare Load Balancer wijst het openbare IP-adres en poortnummer van inkomend verkeer naar de privé IP-adres en poortnummer van de virtuele machine en omgekeerd voor het verkeer van het antwoord van de virtuele machine. Regels voor taakverdeling kunnen u bepaalde soorten verkeer tussen meerdere virtuele machines of services distribueren. Bijvoorbeeld, kunt u het laden van de aanvraag webverkeer spreiden tussen meerdere webservers.

De volgende afbeelding ziet een eindpunt met gelijke taakverdeling voor internetverkeer die wordt gedeeld door drie virtuele machines voor de openbare en persoonlijke TCP-poort 80. Deze drie virtuele machines zijn in een set met gelijke taakverdeling.

![openbare load balancer-voorbeeld](./media/load-balancer-overview/IC727496.png)

**Afbeelding 1: Voor de Load Balancer-webverkeer met behulp van een openbare Load Balancer**

Wanneer Internet-clients webpagina-aanvragen naar het openbare IP-adres van een web-app op TCP-poort 80 verzenden, distribueert de Azure Load Balancer de aanvragen tussen de drie virtuele machines in de set met gelijke taakverdeling. Zie voor meer informatie over de algoritmen voor load balancer, het [overzichtspagina van load balancer](load-balancer-overview.md#load-balancer-features).

Azure Load Balancer distribueert standaard netwerkverkeer wordt evenredig verdeeld over meerdere exemplaren van de virtuele machine. U kunt ook de affiniteit van de sessie configureren. Zie voor meer informatie [load balancer-distributie modus](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interne Load Balancer

Interne Load Balancer wordt alleen stuurt het verkeer naar bronnen die zich in een virtueel netwerk of die een VPN-verbinding gebruikt voor toegang tot Azure-infrastructuur. In dit opzicht verschilt de interne Load Balancer van een openbare Load Balancer. Azure-infrastructuur beperkt toegang tot de taakverdeling frontend-IP-adressen van een virtueel netwerk. Frontend IP-adressen en virtuele netwerken worden nooit rechtstreeks blootgesteld aan een internet-eindpunt. Interne line-of-business-toepassingen worden uitgevoerd in Azure en zijn toegankelijk vanuit Azure of via lokale bronnen.

Interne Load Balancer biedt de volgende soorten taakverdeling:

* Binnen een virtueel netwerk: taakverdeling van VM's in het virtuele netwerk naar een set van virtuele machines die zich in hetzelfde virtuele netwerk bevinden.
* Voor een virtueel netwerk met cross-premises: taakverdeling van lokale computers aan een set van virtuele machines die zich in hetzelfde virtuele netwerk bevinden. 
* Voor toepassingen met meerdere lagen: Load balancing voor toepassingen met meerdere lagen internetgerichte waarin de back-end-lagen niet verbonden met internet zijn. De back-end-lagen vereisen verkeer taakverdeling van de internetgerichte servicetier (Zie afbeelding 2).
* Voor line-of-business-toepassingen: taakverdeling voor line-of-business-toepassingen die worden gehost in Azure zonder extra load balancer-hardware of software. Dit scenario omvat het on-premises servers die in de set van computers waarvan het verkeer verdeeld wordt.

![interne load balancer-voorbeeld](./media/load-balancer-overview/IC744147.png)

**Afbeelding 2 - toepassingen met meerdere lagen met zowel openbare als interne load balancers voor taakverdeling**

## <a name="pricing"></a>Prijzen
Standaard Load Balancer is een bedrag product op basis van het aantal load balancer regels die zijn geconfigureerd en van alle binnenkomende en uitgaande gegevens verwerkt. Standaard Load Balancer prijsgegevens, gaat u naar de [Load Balancer prijzen](https://azure.microsoft.com/pricing/details/load-balancer/) pagina.

Basic Load Balancer wordt gratis aangeboden.

## <a name="sla"></a>SLA

Voor informatie over de standaard Load Balancer SLA, gaat u naar de [Load Balancer SLA](https://aka.ms/lbsla) pagina. 

## <a name="next-steps"></a>Volgende stappen

- Bekijk [standaard Load Balancer in meer detail](load-balancer-standard-overview.md)
- Meer informatie over het gebruik van [standaard Load Balancer en beschikbaarheid Zones](load-balancer-standard-availability-zones.md)
- Meer informatie over het gebruik van [Load Balancer voor uitgaande verbindingen](load-balancer-outbound-connections.md)
- Meer informatie over [Load Balancer HA-poorten](load-balancer-ha-ports-overview.md)
- Meer informatie over het gebruik van [Load Balancer met meerdere Frontends](load-balancer-multivip-overview.md)
- Meer informatie over [VNet Service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md)
- Meer informatie over het maken van een [basic openbare load balancer](load-balancer-get-started-internet-portal.md)

