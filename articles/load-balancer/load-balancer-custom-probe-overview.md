---
title: Load Balancer-tests gebruiken voor het beveiligen van uw service | Microsoft Docs
description: Informatie over het gebruik van statuscontroles instanties achter Load Balancer bewaken
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2018
ms.author: kumud
ms.openlocfilehash: 58bc0c0669992b8b3884e24c39862f47412b9110
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584652"
---
# <a name="load-balancer-health-probes"></a>Load Balancer-tests voor status

Azure Load Balancer maakt gebruik van de gezondheid van tests om te bepalen welke instanties van de groep back-end ontvangt nieuwe stromen. U kunt statuscontroles gebruiken voor het detecteren van het uitvallen van een toepassing op een back-end-instantie. U kunt ook een aangepast antwoord op een statustest genereren en gebruiken de statustest voor datatransportbesturing en signaal naar Load Balancer of u wilt doorgaan met het verzenden van nieuwe stromen of stoppen met het nieuwe stromen te verzenden naar een back-end-exemplaar. Dit kan worden gebruikt voor het beheren van de belasting of geplande uitvaltijd. Wanneer een statustest mislukt, stopt Load Balancer nieuwe stromen te verzenden naar het desbetreffende exemplaar niet in orde.

De typen statuscontroles beschikbaar en de manier waarop de gezondheid van tests zich gedragen is afhankelijk van welke SKU Load Balancer u gebruikt. Bijvoorbeeld: het gedrag van nieuwe en bestaande stromen is afhankelijk van of een stroom is TCP of UDP als alsmede welke Load Balancer-SKU die u gebruikt.

| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| [Test-typen](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Test omlaag gedrag](#probedown) | Alle tests, gaan alle TCP-stromen. | Alle tests omlaag beëindigen voor alle TCP-stromen. | 

> [!IMPORTANT]
> Load Balancer statuscontroles zijn afkomstig uit het IP-adres 168.63.129.16 en moeten niet worden geblokkeerd voor tests uitvoeren voor uw exemplaar markeren.  Beoordeling [bron-IP-adres-test](#probesource) voor meer informatie.

## <a name="types"></a>Test-typen

Statuscontroles kunnen bekijken van een willekeurige poort op een back-end-instantie, met inbegrip van de poort waarop de service wordt geleverd. Het health test-protocol kan worden geconfigureerd voor drie verschillende typen statuscontroles:

- [TCP-listeners](#tcpprobe)
- [HTTP-eindpunten](#httpprobe)
- [HTTPS-eindpunten](#httpsprobe)

De beschikbare typen statuscontroles variëren, afhankelijk van de Load Balancer-SKU geselecteerd:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standaard SKU |    &#9989; |   &#9989; |   &#9989; |
| Basis-SKU |   &#9989; |   &#9989; | &#10060; |

Voor het UDP-taakverdeling, genereert u een aangepaste test van het statussignaal voor het back-end-exemplaar met behulp van een TCP-, HTTP of HTTPS-statustest.

Bij het gebruik van [HA-poorten in load balancer-regels](load-balancer-ha-ports-overview.md) met [standaardversie van Load Balancer](load-balancer-standard-overview.md), alle poorten worden gelijkmatig verdeeld en één health test antwoord moet geven de status van de volledige-exemplaar.  

U moet geen NAT- of proxy een statustest via het exemplaar waarop de statustest naar een andere instantie in uw VNet ontvangt, omdat dit tot een opeenstapeling van storingen in uw scenario leiden kan.

Als u wilt testen van een test-fout health- of markeren in een afzonderlijk exemplaar, kunt u een beveiligingsgroep op expliciete blokkeren de statustest (doel of [bron](#probesource)).

### <a name="tcpprobe"></a> TCP-test

TCP-tests opnieuw verbinding maken met het uitvoeren van een drie richtingen open TCP-handshake met de gedefinieerde poort.  Dit wordt gevolgd door een vier-manier sluiten TCP-handshake.

De minimale testinterval is 5 seconden en het minimale aantal antwoorden niet in orde is 2.  De totale duur mag niet meer dan 120 seconden.

Een TCP-test mislukt wanneer:
* De TCP listener op het exemplaar reageert helemaal niet tijdens de time-outperiode.  Een test is gemarkeerd omlaag op basis van het aantal mislukte test-aanvragen die zijn geconfigureerd om te gaan onbeantwoorde voordat u markeert de test omlaag.
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

HTTP en HTTPS-tests een TCP-verbinding tot stand brengen en uitgeven van een HTTP GET met het opgegeven pad. Beide van deze tests ondersteuning voor relatieve paden voor de HTTP GET. HTTPS-tests zijn hetzelfde als HTTP-tests met de toevoeging van een Transport Layer Security (TLS, voorheen bekend als SSL) wrapper. De statustest is gemarkeerd als het exemplaar met een HTTP-statuscode 200 binnen de time-outperiode reageert.  Deze status tests probeert om te controleren op de testpoort geconfigureerd de gezondheid van elke 15 seconden standaard. De minimale testinterval is 5 seconden. De totale duur mag niet meer dan 120 seconden. 

HTTP / HTTPS-tests kunnen ook handig als u wilt om uw eigen logica als u wilt verwijderen van exemplaren van de load balancer-rotatie te implementeren. Bijvoorbeeld, kan u wilt verwijderen van een exemplaar als deze hoger dan 90% CPU is en een niet - 200 HTTP-status retourneren. 

Als u gebruik van Cloudservices en web-functies die gebruikmaken van w3wp.exe hebben, bereikt u ook automatische bewaking van uw website. De status van een niet-200 terug fouten in de websitecode van uw naar de load balancer-test.  De HTTP-test heeft voorrang op de standaard guest agent-test. 

Een HTTP / HTTPS-test mislukt wanneer:
* Statustest-eindpunt retourneert een HTTP-responscode dan 200 (bijvoorbeeld, 403, 404 of 500). Hiermee wordt de statustest omlaag onmiddellijk gemarkeerd. 
* Statustest-eindpunt reageert helemaal niet tijdens de time-outperiode voor 31 seconde. Afhankelijk van de time-outwaarde die is ingesteld, meerdere aanvragen van de WebTest onbeantwoorde mogelijk gaan voordat de test wordt gemarkeerd als niet wordt uitgevoerd (dat wil zeggen, voordat u SuccessFailCount tests worden verzonden).
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

Cloudservicerollen (werkrollen en webrollen) gebruiken een gastagent voor het bewaken van test standaard.   U moet rekening houden met deze optie uiterste.  U moet altijd een statustest expliciet met een TCP- of HTTP-test definiëren. De test voor een gast-agent is niet zo effectief expliciet gedefinieerde tests voor de meeste scenario's van toepassing.  

De test voor een gast-agent is een controle van de gastagent binnen de virtuele machine. Vervolgens wordt geluisterd en reageert met een HTTP 200 OK antwoord alleen wanneer het exemplaar in de status gereed heeft is. (Overige statussen zijn bezet, Recycling of stoppen.)

Zie voor meer informatie, [het servicedefinitiebestand (csdef) configureren voor statuscontroles](https://msdn.microsoft.com/library/azure/ee758710.aspx) of [aan de slag met het maken van een openbare load balancer voor cloudservices](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Als de guest-agent niet reageert met een HTTP 200 OK, markeert de load balancer het exemplaar als reageert niet. Wordt geblokkeerd stromen te verzenden naar dat exemplaar. De load balancer blijft om te controleren of het exemplaar. 

Als de guest-agent met een HTTP-200 reageert, stuurt de load balancer nieuwe stromen opnieuw naar dat exemplaar.

Wanneer u een Webrol, de websitecode meestal wordt uitgevoerd in w3wp.exe, die niet wordt bewaakt door de Azure-infrastructuur of Gast-agent. Fouten in w3wp.exe (bijvoorbeeld 500 HTTP-antwoorden) worden niet gerapporteerd aan de gastagent. Als gevolg daarvan kan neemt de load balancer dat exemplaar uitgeschakeld.

## <a name="probehealth"></a>Status

Statuscontroles TCP, HTTP en HTTPS worden beschouwd als in orde en markeren van de rolinstantie als in orde wanneer:

* De health-test is geslaagd eerst die de virtuele machine wordt opgestart.
* Het nummer voor SuccessFailCount (zoals eerder beschreven) definieert de waarde van geslaagde tests die nodig zijn voor het markeren van de rolinstantie als in orde. Als een rolinstantie is verwijderd, kan het aantal geslaagde, opeenvolgende tests moet gelijk zijn aan of groter zijn dan de waarde van SuccessFailCount markeren van de rolinstantie als het uitvoeren.

> [!NOTE]
> Als de status van een rolinstantie fluctueert, wordt de load balancer meer wacht voordat de rolinstantie wordt geplaatst in de status in orde. Deze extra wachttijd beschermt de gebruiker en de infrastructuur en is een opzettelijke beleid.

## <a name="probe-count-and-timeout"></a>Aantal en de time-test

Afhankelijk van testgedrag:

* Het aantal geslaagde tests waarmee een instantie moet worden gemarkeerd als maximaal.
* Het aantal mislukte tests die ertoe leiden een exemplaar dat moet worden gemarkeerd als offline.

De waarden voor time-outs en frequentie in SuccessFailCount te bepalen of een exemplaar is bevestigd dat moet worden uitgevoerd of niet wordt uitgevoerd. In de Azure-portal, is de time-out ingesteld op tweemaal de waarde van de frequentie.

Een regel voor taakverdeling is een eenmalige statustest gedefinieerd de respectieve back-endpool.

## <a name="probedown"></a>Test omlaag gedrag

### <a name="tcp-connections"></a>TCP-verbindingen

Nieuwe TCP-verbindingen slaagt met back-end-instantie die in orde is en heeft een gast-OS en de toepassing kan instemmen met een nieuwe stroom.

Als de statustest van de instantie van een back-end is mislukt, blijven tot stand gebrachte TCP-verbindingen voor dit exemplaar van de back-end.

Als alle tests voor alle exemplaren in een back-endpool mislukken, worden er geen nieuwe stromen worden verzonden naar de back-endpool. Standard Load Balancer toestaat tot stand gebrachte TCP stromen om door te gaan.  Basic Load Balancer wordt beëindigd als alle TCP-stromen naar de back-endpool.
 
Aangezien de stroom altijd tussen de client en de VM Gast-OS is, zal een groep met alle tests in een frontend mogelijk niet reageert op TCP-verbindingspogingen openen als er geen exemplaar in orde back-end is voor het ontvangen van de stroom.

### <a name="udp-datagrams"></a>UDP-datagrammen

UDP-datagrammen zullen worden geleverd op in orde back-end-exemplaren.

UDP is zonder verbinding en er is geen flow-status voor UDP bijgehouden. Als de statustest van een back-end-instantie is mislukt, mogelijk bestaande UDP-stromen verplaatsen naar een andere goede instantie in de back-endpool.

Als alle tests voor alle exemplaren in een back-endpool mislukken, worden bestaande UDP-stromen worden beëindigd voor Basic en Standard Load Balancers.

## <a name="probesource"></a>Bron-IP-adres-test

Load Balancer maakt gebruik van een gedistribueerde testinterval service voor de interne statusmodel. Elke host waarop de virtuele machines zich bevinden, kan worden geprogrammeerd voor het genereren van statuscontroles per configuratie van de klant. Het health test-verkeer is rechtstreeks tussen het onderdeel van de infrastructuur die wordt gegenereerd met de statustest en de virtuele machine van de klant. Alle tests van de Load Balancer zijn afkomstig uit het IP-adres 168.63.129.16 als bron gebruikt.  Wanneer u uw eigen IP-adressen naar Azure Virtual Network meebrengen, wordt dit IP-adres van de gezondheid van test bron gegarandeerd uniek zijn omdat deze wereldwijd is bestemd voor Microsoft.  Dit adres is hetzelfde als in alle regio's en verandert niet. Deze moet niet worden beschouwd als een veiligheidsrisico inhouden omdat alleen het interne Azure-platform kunt een pakket van dit IP-adres van bron. 

Naast het Load Balancer-tests gebruiken de volgende bewerkingen voor dit IP-adres:

- Kan de VM-Agent om te communiceren met het platform om aan te geven is in een status 'Gereed'
- Communicatie met de virtuele DNS-server voor gefilterde naamomzetting voor klanten die geen aangepaste DNS-servers definieert mogelijk maakt.  Dit filter zorgt ervoor dat klanten alleen de hostnamen van de implementatie kunnen omzetten.

Voor Load Balancer statustest uw exemplaar omhoog, markeert u **moet** toestaan dit IP-adres in elke Azure [beveiligingsgroepen](../virtual-network/security-overview.md) en lokale firewall-beleid.

Als u niet toestaan dat dit IP-adres in uw firewall-beleid, mislukt de statustest omdat deze kan niet worden bereikt uw exemplaar.  Load Balancer wordt op zijn beurt markeren in uw exemplaar omdat de status test is mislukt.  Hierdoor kan de service met taakverdeling mislukken. 

U moet uw VNet ook niet configureren met de IP-adresbereik dat 168.63.129.16 bevat het eigendom van Microsoft.  Dit wordt in conflict zijn met het IP-adres van de statustest.

Als u meerdere interfaces op de virtuele machine hebt, moet u zorgen dat u de test op de interface die u hebt ontvangen op reageren.  Dit is mogelijk een unieke bron NAT'ing dit adres in de virtuele machine op basis van de per-interface.

## <a name="monitoring"></a>Bewaking

Openbare en interne [Standard Load Balancer](load-balancer-standard-overview.md) weergeven per eindpunt en de back-end-exemplaar de integriteitsstatus test van als multi-dimensionale metrische gegevens via Azure Monitor. Dit kan vervolgens worden gebruikt door andere Azure-services of 3rd party toepassingen. 

Basic openbare Load Balancer wordt aangegeven dat de integriteitsstatus test van samengevat per back-endpool via Log Analytics.  Dit is niet beschikbaar voor interne Basic Load Balancers.  U kunt [melden analytics](load-balancer-monitor-log.md) om te controleren op de status van openbare load balancer-test en count-test. Logboekregistratie kan worden gebruikt met Power BI of Azure Operational Insights voor statistische gegevens over de integriteitsstatus van de load balancer.

## <a name="limitations"></a>Beperkingen

-  HTTPS-tests bieden geen ondersteuning voor wederzijdse verificatie met een clientcertificaat.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md)
- [Maken van een openbare load balancer in Resource Manager met behulp van PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [REST-API voor statuscontroles](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Nieuwe mogelijkheden voor health test met aanvragen [van de Load Balancer Uservoice](https://aka.ms/lbuservoice)
