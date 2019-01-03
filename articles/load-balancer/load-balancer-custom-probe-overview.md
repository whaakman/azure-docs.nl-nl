---
title: Azure Load Balancer statuscontroles om te schalen en hoge beschikbaarheid bieden voor uw service gebruiken
titlesuffix: Azure Load Balancer
description: Informatie over het gebruik van statuscontroles instanties achter Load Balancer bewaken
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2018
ms.author: kumud
ms.openlocfilehash: 51d781f331bcbc08642dc32c21baa150e9e5eee6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53538682"
---
# <a name="load-balancer-health-probes"></a>Load Balancer-tests voor status

Azure Load Balancer biedt statuscontroles voor gebruik met regels voor taakverdeling.  Status configuratie- en test testrespons te bepalen welke instanties van de groep back-end ontvangt nieuwe stromen. U kunt statuscontroles gebruiken voor het detecteren van het uitvallen van een toepassing op een back-end-instantie. U kunt ook een aangepast antwoord op een statustest genereren en de statustest voor datatransportbesturing gebruiken voor het beheren van belasting of geplande uitvaltijd. Wanneer een statustest mislukt, stopt Load Balancer nieuwe stromen te verzenden naar het desbetreffende exemplaar niet in orde.

Statuscontroles ondersteuning voor meerdere protocollen. De beschikbaarheid van een specifiek type statustest voor de ondersteuning van een specifieke protocol is afhankelijk van Load Balancer-SKU.  Bovendien het gedrag van de service is afhankelijk van Load Balancer-SKU.

| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| [Test-typen](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Test omlaag gedrag](#probedown) | Alle tests, gaan alle TCP-stromen. | Alle tests omlaag beëindigen voor alle TCP-stromen. | 

> [!IMPORTANT]
> Load Balancer statuscontroles zijn afkomstig uit het IP-adres 168.63.129.16 en niet voor tests markeert u uw exemplaar moeten worden geblokkeerd.  Beoordeling [bron-IP-adres-test](#probesource) voor meer informatie.

## <a name="types"></a>Test-typen

De statustest kan worden geconfigureerd voor listeners met behulp van de volgende drie protocollen:

- [TCP-listeners](#tcpprobe)
- [HTTP-eindpunten](#httpprobe)
- [HTTPS-eindpunten](#httpsprobe)

De beschikbare typen statuscontroles variëren, afhankelijk van de Load Balancer-SKU geselecteerd:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standaard SKU |    &#9989; |   &#9989; |   &#9989; |
| Basis-SKU |   &#9989; |   &#9989; | &#10060; |

Ongeacht welke test dat u kiest, ziet de statuscontroles een willekeurige poort op een back-end-instantie, met inbegrip van de poort waarop de service wordt geleverd.

### <a name="tcpprobe"></a> TCP-test

TCP-tests opnieuw verbinding maken met het uitvoeren van een drie richtingen open TCP-handshake met de gedefinieerde poort.  TCP-tests beëindigen een verbinding met een TCP-handshake van vier richtingen sluiten.

De minimale testinterval is 5 seconden en het minimale aantal antwoorden niet in orde is 2.  De totale duur van alle intervallen mag niet meer dan 120 seconden.

Een TCP-test mislukt wanneer:
* De TCP listener op het exemplaar reageert helemaal niet tijdens de time-outperiode.  Een test is gemarkeerd omlaag op basis van het aantal mislukte test-aanvragen die zijn geconfigureerd om te gaan onbeantwoorde voordat u markeert u de test.
* De test ontvangt een TCP opnieuw instellen van het exemplaar.

#### <a name="resource-manager-template"></a>Resource Manager-sjabloon

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> HTTP / HTTPS-test

> [!NOTE]
> HTTPS-test is alleen beschikbaar voor [Standard Load Balancer](load-balancer-standard-overview.md).

HTTP en HTTPS-tests op de TCP-test maken en uitgeven van een HTTP GET met het opgegeven pad. Beide van deze tests ondersteuning voor relatieve paden voor de HTTP GET. HTTPS-tests zijn hetzelfde als HTTP-tests met de toevoeging van een Transport Layer Security (TLS, voorheen bekend als SSL) wrapper. De statustest is gemarkeerd als het exemplaar met een HTTP-statuscode 200 binnen de time-outperiode reageert.  De statustest probeert om te controleren op de testpoort geconfigureerd de gezondheid van elke 15 seconden standaard. De minimale testinterval is 5 seconden. De totale duur van alle intervallen mag niet meer dan 120 seconden.

HTTP / HTTPS-tests kunnen ook handig als u wilt statustest express.  Implementeer uw eigen logica voor de exemplaren uit de load balancer-rotatie verwijderen als de testpoort ook de listener voor de service zelf is. Bijvoorbeeld, kan u wilt verwijderen van een exemplaar als deze hoger dan 90% CPU is en een niet - 200 HTTP-status retourneren. 

Als u gebruik van Cloudservices en web-functies die gebruikmaken van w3wp.exe hebben, bereikt u ook automatische bewaking van uw website. De status van een niet-200 terug fouten in de websitecode van uw naar de load balancer-test.

Een HTTP / HTTPS-test mislukt wanneer:
* Statustest-eindpunt retourneert een HTTP-responscode dan 200 (bijvoorbeeld, 403, 404 of 500). Hiermee worden onmiddellijk naar beneden de statustest gemarkeerd. 
* Statustest-eindpunt reageert helemaal niet tijdens de 31 seconde time-outperiode. Meerdere aanvragen van de WebTest gaat onbeantwoorde voordat de test wordt gemarkeerd als niet wordt uitgevoerd en pas de som van alle time-outintervallen is bereikt.
* De verbinding via een TCP-opnieuw instellen van statustest-eindpunt wordt gesloten.

#### <a name="resource-manager-templates"></a>Resource Manager-sjablonen

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Test van Gast-agent (alleen klassiek)

Cloudservicerollen (werkrollen en webrollen) gebruiken een gastagent voor het bewaken van test standaard.  De test voor een gast-agent is een laatste toevlucht-configuratie.  Gebruik altijd een statustest expliciet met een TCP- of HTTP-test. De test voor een gast-agent is niet zo effectief expliciet gedefinieerde tests voor de meeste scenario's van toepassing.

De test voor een gast-agent is een controle van de gastagent binnen de virtuele machine. Vervolgens wordt geluisterd en reageert met een HTTP 200 OK antwoord alleen wanneer het exemplaar in de status gereed heeft is. (Overige statussen zijn bezet, Recycling of stoppen.)

Zie voor meer informatie, [het servicedefinitiebestand (csdef) configureren voor statuscontroles](https://msdn.microsoft.com/library/azure/ee758710.aspx) of [aan de slag met het maken van een openbare load balancer voor cloudservices](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Als de guest-agent niet reageert met een HTTP 200 OK, markeert de load balancer het exemplaar als reageert niet. Wordt geblokkeerd stromen te verzenden naar dat exemplaar. De load balancer blijft om te controleren of het exemplaar. 

Als de guest-agent met een HTTP-200 reageert, stuurt de load balancer nieuwe stromen opnieuw naar dat exemplaar.

Wanneer u een Webrol, de websitecode meestal wordt uitgevoerd in w3wp.exe, die niet wordt bewaakt door de Azure-infrastructuur of Gast-agent. Fouten in w3wp.exe (bijvoorbeeld 500 HTTP-antwoorden) worden niet gerapporteerd aan de gastagent. Als gevolg daarvan kan neemt de load balancer dat exemplaar uitgeschakeld.

<a name="health"></a>
## <a name="probehealth"></a>Status

Statuscontroles TCP, HTTP en HTTPS worden beschouwd als in orde en markeren van de rolinstantie als in orde wanneer:

* De statustest is één keer geslaagd nadat de virtuele machine wordt opgestart.
* Het opgegeven aantal tests die zijn vereist voor het markeren van de rolinstantie als in orde is bereikt.

> [!NOTE]
> Als de statustest fluctueert, wordt de load balancer meer wacht voordat de rolinstantie wordt geplaatst in de status in orde. Deze extra wachttijd beschermt de gebruiker en de infrastructuur en is een opzettelijke beleid.

## <a name="probe-count-and-timeout"></a>Aantal en de time-test

Afhankelijk van testgedrag:

* Het aantal geslaagde tests waarmee een instantie moet worden gemarkeerd als maximaal.
* Het aantal mislukte tests die ertoe leiden een exemplaar dat moet worden gemarkeerd als offline.

De opgegeven waarden voor time-outs en interval vaststellen of een exemplaar is gemarkeerd als omhoog of omlaag.

## <a name="probedown"></a>Test omlaag gedrag

### <a name="tcp-connections"></a>TCP-verbindingen

Nieuwe TCP-verbindingen slaagt resterende goede back-end-exemplaren.

Als de statustest van de instantie van een back-end is mislukt, blijven tot stand gebrachte TCP-verbindingen voor dit exemplaar van de back-end.

Als alle tests voor alle exemplaren in een back-endpool mislukken, worden er geen nieuwe stromen worden verzonden naar de back-endpool. Standard Load Balancer toestaat tot stand gebrachte TCP stromen om door te gaan.  Basic Load Balancer wordt beëindigd als alle TCP-stromen naar de back-endpool.
 
Load Balancer is een pass through-service (niet beëindigen voor TCP-verbindingen) en de stroom is altijd tussen de client en de Gast-OS en toepassing van de virtuele machine. Een groep met alle tests omlaag zorgt ervoor dat een frontend mogelijk niet reageert op TCP-verbinding open pogingen (SYN) als er is geen exemplaar in orde back-end de stroom ontvangen en erop kunt reageren met een SYN-ACK.

### <a name="udp-datagrams"></a>UDP-datagrammen

UDP-datagrammen zullen worden geleverd op in orde back-end-exemplaren.

UDP is zonder verbinding en er is geen flow-status voor UDP bijgehouden. Als de statustest van een back-end-instantie is mislukt, mogelijk bestaande UDP-stromen verplaatsen naar een andere goede instantie in de back-endpool.

Als alle tests voor alle exemplaren in een back-endpool mislukken, worden bestaande UDP-stromen worden beëindigd voor Basic en Standard Load Balancers.

<a name="source"></a>
## <a name="probesource"></a>Bron-IP-adres-test

Load Balancer maakt gebruik van een gedistribueerde testinterval service voor de interne statusmodel. De testinterval service zich bevindt op elke host waar VM's en kunnen worden geprogrammeerd op aanvraag voor het genereren van statuscontroles per configuratie van de klant. Het health test-verkeer is rechtstreeks tussen de testinterval-service die wordt gegenereerd met de statustest en de virtuele machine van de klant. Alle tests van de Load Balancer zijn afkomstig uit het IP-adres 168.63.129.16 als bron gebruikt.  U kunt IP-adresruimte in een VNet dat is niet RFC1918 ruimte gebruiken.  Met behulp van een wereldwijd gereserveerde, eigendom Microsoft IP-adres vermindert de kans dat een IP-adresconflict met de IP-adresruimte die u binnen het VNet gebruiken.  Dit IP-adres is hetzelfde als in alle regio's en wordt niet gewijzigd en is niet een beveiligingsrisico, omdat alleen het onderdeel van de interne Azure-platform kunt een pakket van dit IP-adres van bron. 

De tag AzureLoadBalancer service identificeert deze bron-IP-adres in uw [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) en health test verkeer standaard is toegestaan.

Naast het Load Balancer-tests gebruiken de volgende bewerkingen voor dit IP-adres:

- Kan de VM-Agent om te communiceren met het platform om aan te geven is in een status 'Gereed'
- Communicatie met de virtuele DNS-server voor gefilterde naamomzetting voor klanten die geen aangepaste DNS-servers definieert mogelijk maakt.  Dit filter zorgt ervoor dat klanten alleen de hostnamen van de implementatie kunnen omzetten.
- Kan de virtuele machine om te verkrijgen van een dynamisch IP-adres van de DHCP-service in Azure.

## <a name="design"></a> Ontwerprichtlijnen

Statuscontroles worden gebruikt om uw service robuuste maken en toe te staan om te schalen. Een onjuiste configuratie of een slecht ontwerppatroon kan invloed hebben op de beschikbaarheid en schaalbaarheid van uw service. Raadpleeg dit hele document en wat de impact op uw scenario is wanneer deze test-antwoord is gemarkeerd als omlaag of gemarkeerd en hoe dit invloed heeft op de beschikbaarheid van uw toepassingsscenario.

Bij het ontwerpen van het statusmodel voor uw toepassing, moet u een poort op een back-end-instantie die overeenkomt met de status van deze instantie probe __en__ de toepassingsservice die u opgeeft.  Poort van de toepassing en de testpoort hoeven niet hetzelfde te zijn.  In sommige scenario's kan het wenselijk zijn voor de testpoort anders dan de poort die op in uw toepassing service kunt zijn.  

Soms kan het nuttig zijn voor uw toepassing voor het genereren van een health test-antwoord om niet alleen de toepassingsstatus van uw te detecteren, maar ook rechtstreeks aan de Load Balancer signaal of uw exemplaar moet ontvangen of ontvangt geen nieuwe stromen.  U kunt het antwoord van de test om toe te staan van uw toepassing tegendruk en te beperken levering van nieuwe stromen met een exemplaar maken van het mislukken van de statustest voorbereiden voor het onderhoud van uw toepassing en initiëren Verwerkingsstop voor uw scenario wilt bewerken.  Bij het gebruik van Standard Load Balancer, een [probe omlaag](#probedown) signaal wordt TCP-stromen om door te gaan tot niet-actieve time-out of verbindingsfout sluiting altijd toestaan. 

Voor UDP-taakverdeling, moet u een aangepaste test statussignaal genereren vanuit de back-end-exemplaar en een TCP, HTTP of HTTPS-statustest die gericht is op de bijbehorende listener gebruiken in overeenstemming met de status van uw toepassing UDP.

Bij het gebruik van [HA-poorten-taakverdelingsregels](load-balancer-ha-ports-overview.md) met [standaardversie van Load Balancer](load-balancer-standard-overview.md), alle poorten worden gelijkmatig verdeeld en één health test antwoord moet geven de status van de volledige-exemplaar.

Kan niet worden omgezet of proxy een statustest via het exemplaar dat de statustest naar een andere instantie in uw VNet ontvangt deze configuratie kan leiden tot een opeenstapeling van storingen in uw scenario.  Houd rekening met het volgende scenario: een set van externe apparaten is geïmplementeerd in de back-endadresgroep van een Load Balancer-resource voor schaal en redundantie van de apparaten en de statustest is geconfigureerd voor een testpoort die het apparaat van de derde partij proxy's of wordt omgezet in andere virtuele machines achter het apparaat.  Als u de dezelfde poort die u gebruikt om te zetten of proxy om aanvragen aan de andere virtuele machines achter de toestel-test, markeert test reacties van één virtuele machine achter het toestel het apparaat zelf dead. Deze configuratie kan leiden tot een trapsgewijze fout van de gehele toepassingsscenario als gevolg van een exemplaar één back-end achter het apparaat.  De trigger is een fout met onregelmatige test die zorgt ervoor dat de Load Balancer te markeren in de oorspronkelijke bestemming (het exemplaar van het apparaat) en uw gehele toepassingsscenario op zijn beurt kunt uitschakelen. Test in plaats daarvan de status van het apparaat zelf. De selectie van de test om te bepalen van de statussignaal is een belangrijk aandachtspunt voor scenario's voor netwerken virtuele netwerkapparaten (NVA) en u moet de leverancier van uw toepassing raadplegen voor de juiste statussignaal voor dergelijke scenario's is.

Als u niet de [bron-IP](#probesource) van de test in uw firewall-beleid, de statustest mislukt omdat deze kan niet worden bereikt uw exemplaar.  Load Balancer wordt op zijn beurt markeren in uw exemplaar omdat de status test is mislukt.  Deze onjuiste configuratie kan leiden tot uw load balanced toepassingsscenario mislukken.

Voor de Load Balancer-statustest markeert u uw exemplaar u **moet** toestaan dit IP-adres in elke Azure [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) en lokale firewall-beleid.  Elke netwerkbeveiligingsgroep omvat standaard de [servicetag](../virtual-network/security-overview.md#service-tags) AzureLoadBalancer naar health test verkeer toestaan.

Als u wilt testen van een test-fout health- of markeren in een afzonderlijk exemplaar, kunt u een [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) expliciet blokkeren de statustest (doelpoort of [bron-IP](#probesource)) en simuleren de een test is mislukt.

Stel uw VNet niet met de IP-adresbereik dat 168.63.129.16 bevat het eigendom van Microsoft.  Deze configuraties worden conflicteren met het IP-adres van de statustest en kunnen leiden tot uw scenario mislukken.

Als u meerdere interfaces op de virtuele machine hebt, moet u zorgen dat u de test op de interface die u hebt ontvangen op reageren.  U moet mogelijk Bronnetwerk adres vertalen dit adres in de virtuele machine op basis van de per-interface.

Schakel geen [TCP tijdstempels](https://tools.ietf.org/html/rfc1323).  Inschakelen van TCP tijdstempels zorgt ervoor dat statuscontroles mislukt vanwege een TCP-pakketten door van de VM-Gast OS TCP-stack, wat tot de Load Balancer markeren omlaag het respectieve eindpunt leidt wordt verwijderd.  TCP-tijdstempels worden regelmatig standaard ingeschakeld voor beveiliging beveiligde VM-installatiekopieën en moet worden uitgeschakeld.

## <a name="monitoring"></a>Bewaking

Openbare en interne [Standard Load Balancer](load-balancer-standard-overview.md) weergeven per eindpunt en de back-end-exemplaar de integriteitsstatus test van als multi-dimensionale metrische gegevens via Azure Monitor. Deze metrische gegevens kunnen worden gebruikt door andere Azure-services of toepassingen van derden arty. 

Basic openbare Load Balancer wordt aangegeven dat de integriteitsstatus test van samengevat per back-endpool via Log Analytics.  Log Analytics zijn niet beschikbaar voor interne Basic Load Balancers.  U kunt [melden analytics](load-balancer-monitor-log.md) om te controleren op de status van openbare load balancer-test en count-test. Logboekregistratie kan worden gebruikt met Power BI of Azure Operational Insights voor statistische gegevens over de integriteitsstatus van de load balancer.

## <a name="limitations"></a>Beperkingen

- HTTPS-tests bieden geen ondersteuning voor wederzijdse verificatie met een clientcertificaat.
- Statuscontroles mislukken wanneer TCP tijdstempels zijn ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md)
- [Maken van een openbare load balancer in Resource Manager met behulp van PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [REST-API voor statuscontroles](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Nieuwe mogelijkheden voor health test met aanvragen [van de Load Balancer Uservoice](https://aka.ms/lbuservoice)
