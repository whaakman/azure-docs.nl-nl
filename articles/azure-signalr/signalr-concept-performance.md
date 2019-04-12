---
title: Prestatiehandleiding voor voor Azure SignalR Service
description: Een overzicht van Azure SignalR Service-prestaties.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: zhshang
ms.openlocfilehash: 53139dd253c491ea6578fd0b9cbada4e7b331c7d
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502036"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Prestatiehandleiding voor voor Azure SignalR Service

Een van de belangrijkste voordelen voor het gebruik van Azure SignalR Service is het eenvoudig om SignalR-toepassingen te schalen. In een grootschalige scenario wordt de prestaties van een belangrijke factor. In deze handleiding stellen we de factoren die invloed hebben op de prestaties van de toepassing SignalR en onder verschillende use-casescenario's, wat is de normale prestaties? Uiteindelijk moet worden we ook de omgeving en de hulpprogramma's die worden gebruikt voor het genereren van prestatierapport geïntroduceerd.

## <a name="terms-definition"></a>De definitie van de voorwaarden

*ASRS*: Azure SignalR Service

*Inkomende*: het binnenkomende bericht naar Azure SignalR Service

*Uitgaande*: het uitgaande bericht van Azure SignalR Service

*Bandbreedte*: totale grootte van alle berichten in 1 seconde

*Standaardmodus*: ASRS wordt verwacht dat de app-server verbinding maken met het voordat alle clientverbindingen worden geaccepteerd. Dit is de standaardmodus werken wanneer een ASRS is gemaakt.

*Serverloze modus*: ASRS accepteert alleen verbindingen van clients. Er is geen serververbinding is toegestaan.

## <a name="overview"></a>Overzicht

ASRS definieert zeven standaard-laag voor verschillende prestatie-capaciteiten, en deze handleiding zal worden de volgende vragen beantwoorden:

-   Wat is de gebruikelijke ASRS prestaties voor elke laag?

-   ASRS voldoet aan de vereiste van bericht doorvoer, bijvoorbeeld 100.000 berichten per seconde verzenden?

-   Voor mijn specifieke scenario, welke laag geschikt voor mij is? Of hoe kan ik de juiste laag selecteren?

-   Wat voor soort app-server (VM-grootte) is geschikt voor mij en hoe vaak moeten ik implementeren?

Als u wilt deze vragen te beantwoorden, deze prestatiehandleiding eerst geeft een op hoog niveau uitleg over de factoren die invloed op de prestaties hebben en vervolgens ziet u de maximale binnenkomende en uitgaande berichten voor elke laag voor typische gebruiksvoorbeelden: **echo**, **uitzending**, **verzenden aan groep**, en **verzenden / verbinding** (peer-to-peer chatten).

Het is niet mogelijk voor dit document om alle scenario's (en verschillende use-case, verschillende berichtgrootte of bericht verzenden patroon enz.). Het biedt echter enkele evaluatiemethoden zodat gebruikers kunnen op ongeveer evalueren hun behoefte van de binnenkomende of uitgaande berichten, en vervolgens de juiste lagen vinden door te controleren van de tabel van de prestaties.

## <a name="performance-insight"></a>Queryprestaties

In deze sectie beschrijft de methoden van de evaluatie van prestaties en een lijst met alle factoren die invloed op de prestaties hebben. Uiteindelijk moet biedt het methoden om u te helpen bij het evalueren van de prestatie-eisen.

### <a name="methodology"></a>Methodologie

**Doorvoer** en **latentie** zijn twee typische aspecten van de prestaties controleren. Voor ASRS heeft verschillende SKU-laag verschillende doorvoer beperking van beleid. In dit document wordt **de maximaal toegestane doorvoer (binnenkomende en uitgaande bandbreedte)** zoals de maximale doorvoer behaalde wanneer 99% van de berichten latentie van minder dan 1 seconde.

De latentie is de tijdsduur van de verbinding bericht verzenden naar het antwoordbericht ontvangt van ASRS. Laten we **echo** elke clientverbinding wordt bijvoorbeeld een tijdstempel wordt toegevoegd in het bericht. App-server-hub verzendt het oorspronkelijke bericht terug naar de client. De vertraging doorgifte wordt dus eenvoudig berekend door elke clientverbinding. De tijdstempel is gekoppeld voor elk bericht dat in **uitzending**, **verzenden aan groep**, en **verzenden / verbinding**.

Meerdere VM's om te simuleren duizenden gelijktijdige clientverbindingen, gemaakt in een virtueel particulier netwerk in Azure. Alle VM's verbinding maken met de dezelfde ASRS-instantie.

In de standaardmodus ASRS, worden ook appserver-VM's geïmplementeerd in hetzelfde virtuele particuliere netwerk als client VM's.

Alle client VM's en app-server die virtuele machines worden geïmplementeerd in hetzelfde netwerk van dezelfde regio bevinden om te voorkomen dat meerdere regio latentie.

### <a name="performance-factors"></a>Prestatiefactoren

In theorie wordt ASRS capaciteit beperkt door de rekenbronnen die: CPU, geheugen en netwerk. Bijvoorbeeld, meer verbindingen met ASRS, meer geheugen ASRS verbruikt. Voor grotere berichtenverkeer, bijvoorbeeld, elk bericht groter is dan 2048 bytes, ASRS te besteden aan meer CPU-cycli voor het verwerken van ook is vereist. Ondertussen Azure netwerkbandbreedte ook standaard een limiet ingesteld voor maximum verkeer.

Het transporttype [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [server-verzonden-gebeurtenis](https://en.wikipedia.org/wiki/Server-sent_events), of [Long-Polling](https://en.wikipedia.org/wiki/Push_technology), is een andere factor is van invloed op prestaties. WebSocket is een protocol bidirectionele en full-duplex-communicatie via één TCP-verbinding. Server-verzonden-gebeurtenis is echter Unidirectioneel protocol push-bericht van server naar de client. Long-Polling vereist dat de clients te peilen regelmatig gegevens van de server via HTTP-aanvraag. Voor de dezelfde API onder dezelfde voorwaarde WebSocket is de beste prestaties, server-verzonden-gebeurtenis is trager en Long-Polling het langzaamst is. ASRS raadt WebSocket standaard.

De kosten voor het doorsturen van bericht beperkt bovendien ook de prestaties. ASRS speelt een rol als een router bericht het bericht van een set van clients of servers met andere clients of servers stuurt. Ander scenario of API vereist verschillende routeringsbeleid. Voor **echo**, de client verzendt een bericht naar zichzelf en de bestemming van de routering is ook zelf. Dit patroon heeft de laagste routering kosten. Maar voor **uitzending**, **verzenden aan groep**, **verzenden / verbinding**, ASRS nodig heeft om te controleren of de doel-verbindingen via de interne gedistribueerde gegevensstructuur die verbruikt meer CPU, geheugen en zelfs de netwerkbandbreedte. Als gevolg hiervan is trager dan **echo**.

In de standaardmodus kan de app-server ook een knelpunt voor bepaalde scenario's, omdat Azure SignalR SDK moet het aanroepen van de Hub, behoudt de live-verbinding met elke client via de heartbeat signalen die tijd.

In de modus voor serverloze verzendt de client bericht door HTTP post, die niet zo efficiënt als WebSocket.

Een andere factor is een protocol: JSON en [MessagePack](https://msgpack.org/index.html). MessagePack is kleiner dan de grootte en geleverde sneller dan de JSON. Intuïtief, MessagePack profiteren prestaties, maar ASRS prestaties is niet met protocollen gevoelig omdat het de berichtnettolading van het niet tijdens het doorsturen van berichten van clients op servers of vice versa decoderen is.

Kortom, wordt in de volgende factoren gevolgen hebben voor de inkomende en uitgaande capaciteit:

-   SKU-laag (CPU/geheugen)

-   Aantal verbindingen

-   Berichtgrootte

-   snelheid van berichten verzenden

-   transporttype (WebSocket/server-verzonden-gebeurtenis/Long-Polling)

-   Use-casescenario (routering kosten)

-   App-server en service-verbindingen (in de servermodus)


### <a name="find-a-proper-sku"></a>Een juiste SKU zoeken

Het evalueren van de inkomende/uitgaande capaciteit of over het vinden van welke laag is geschikt voor een specifieke use case?

We ervan uitgaan dat de app-server is krachtig genoeg en is niet het prestatieknelpunt. Vervolgens controleren we de maximale bandbreedte voor binnenkomende en uitgaande voor elke laag.

#### <a name="quick-evaluation"></a>Een snelle evaluatie

Vereenvoudig de evaluatie eerst laten we veronderstelling dat sommige standaardinstellingen: WebSocket wordt gebruikt, grootte van het bericht is 2048 bytes, bericht verzenden elke 1 seconde en het is in de modus.

Voor elke laag heeft een eigen maximale bandbreedte voor binnenkomende en uitgaande bandbreedte. Goede gebruikerservaring kan niet worden gegarandeerd wanneer het binnenkomend of uitgaand de limiet overschrijdt.

**Echo** geeft de maximale bandbreedte voor binnenkomende omdat deze de laagste routering kosten heeft. **Uitzenden** definieert het maximum aantal uitgaande message-bandbreedte.

Voer **niet** groter zijn dan de geselecteerde waarden in de volgende twee tabellen.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen                       | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| **Binnenkomende bandbreedte (bytes/s)** | **2 MIN.**    | **4 MIN.**    | **10M**   | **20 MILJOEN**    | **40 MIN.**    | **100 MILJOEN**   | **200 MILJOEN**    |
| Uitgaande bandbreedte (bytes/s) | 2 MIN.    | 4 MIN.    | 10M   | 20 MILJOEN    | 40 MIN.    | 100 MILJOEN   | 200 MILJOEN    |


|     Uitzenden             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Binnenkomende bandbreedte (bytes/s)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K     |
| **Uitgaande bandbreedte (bytes/s)** | **4 MIN.**    | **8 MIN.**    | **20 MILJOEN**    | **40 MIN.**    | **80M**    | **200 MILJOEN**    | **400 MIN.**   |

De binnenkomende bandbreedte en de uitgaande bandbreedte formules:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

*inboundConnections*: het aantal verbindingen bericht verzenden

*outboundConnections*: het aantal verbindingen ontvangen van bericht

*messageSize*: de grootte van een enkel bericht (gemiddelde waarde). Voor kleine bericht waarvan de grootte minder dan 1024 bytes is, heeft deze vergelijkbaar invloed op de prestaties als 1024-byte-bericht.

*sendInterval*: de tijd van het verzenden van één bericht, meestal is 1 seconde per bericht, wat betekent dat elke seconde van één bericht verzenden. Kleinere sendInterval betekent meer bericht verzonden in bepaalde periode. 0,5 seconde per bericht betekent bijvoorbeeld dat twee berichten verzonden per seconde.

*Verbindingen* de ASRS toegezegde maximum drempelwaarde voor elke laag is. Als het nummer van de verbinding is verhoogd, wordt het ten koste gaan van bandbreedtebeperking van verbinding.

*Binnenkomende bandbreedte* en *uitgaande bandbreedte* zijn de totale berichtgrootte per seconde. Hier heb "houdt in megabytes voor het gemak.

#### <a name="evaluation-for-complex-use-cases"></a>Evaluatie voor complexe use-cases

##### <a name="bigger-message-size-or-different-sending-rate"></a>Grootte van het bericht groter of andere verzendsnelheid

De echte use-case is gecompliceerder. Kan het bericht groter is dan 2048 bytes verzenden of verzendsnelheid bericht is niet één bericht per seconde. We gaan de uitzending van unit100 als voorbeeld om te zoeken over het evalueren van de prestaties.

De volgende tabel ziet u een echte geval van **uitzending**, maar de grootte van het bericht, aantal verbindingen en bericht verzenden tarief verschillen van wat wordt ervan uitgegaan in de vorige sectie dat. De vraag is hoe we kunt deduceren een van deze items (grootte van het bericht, aantal verbindingen of snelheid voor het verzenden van berichten) als we weten dat alleen 2 hiervan.

| Uitzenden  | Berichtgrootte (bytes) | Inkomende (bericht/s) | Verbindingen | Verzenden intervallen (tweede) |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 K                 | 1                        | 100.000     | 5                       |
| 2 | 256 K                | 1                        | 8,000       | 5                       |

De volgende formule is eenvoudig om te worden afgeleid op basis van de vorige bestaande formule:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Voor unit100, we weten dat de maximale bandbreedte voor uitgaand is 400M uit de vorige tabel en klik vervolgens voor de berichtgrootte van 20-K, moet het maximale aantal uitgaande verbindingen 400M \* 5 / 20 K = 100.000, die overeenkomt met de werkelijke waarde.

##### <a name="mixed-use-cases"></a>Gemengde use-cases

Normaal gesproken combineert de vier eenvoudige gebruiksvoorbeelden samen de echte use-case: **echo**, **uitzending**, **verzenden naar groep**, of **verzenden naar verbinding**. De methode die wordt gebruikt voor het evalueren van de capaciteit is het verdelen van de verschillende use cases in vier eenvoudige use-cases, **berekenen van de maximumgrootte van een bericht binnenkomende en uitgaande bandbreedte** met behulp van de bovenstaande formules afzonderlijk, en ze de totale som maximale bandbreedte voor binnenkomend en uitgaand. Kies een van de juiste laag uit de tabellen Maximumbandbreedte binnenkomend en uitgaand.

In de tussentijd zorgen, voor het verzenden van berichten naar honderden of duizenden kleine groepen of duizenden clients bericht verzenden naar elkaar worden verbonden, de kosten voor routering worden dominante. Deze gevolgen moet rekening worden gehouden. Meer informatie worden in de volgende secties voor 'CASESTUDY' beschreven.

Voor de use-case van het bericht te verzenden naar clients, zorg ervoor dat de app-server is **niet** het knelpunt. Sectie voor 'CASESTUDY' geeft de richtlijn over het aantal appservers u nodig hebt en het aantal serververbindingen moeten worden geconfigureerd.

## <a name="case-study"></a>Casestudy

De volgende secties doorlopen vier standaard use cases voor WebSocket-transport: **echo**, **uitzending**, **verzenden aan groep**, en **verzenden naar verbinding** . Voor elk scenario, geeft de huidige ASRS binnenkomende en uitgaande capaciteit in de tussentijd zorgen wordt uitgelegd wat de belangrijkste factoren op de prestaties.

Modus, App-server, via Azure SignalR Service SDK standaard, maakt in vijf serververbindingen met ASRS. In de prestaties testen resultaat hieronder, server-verbindingen worden verhoogd tot 15 (of meer voor uitzendingen en verzenden van het bericht aan grote groep).

Verschillende use cases hebben verschillende vereisten op servers met Apps. **Uitzenden** moet klein aantal servers met Apps. **Echo** of **verzenden / verbinding** moet veel appservers.

Gebruik in alle gevallen, de standaardgrootte van het bericht is 2048 bytes en bericht verzenden interval is 1 seconde.

## <a name="default-mode"></a>Standaardmodus

Clients, servers met web apps en ASRS betrokken zijn bij deze modus. Elke client staat voor een enkele verbinding.

### <a name="echo"></a>Echo

Ten eerste is de web-apps verbinding maken met ASRS staat. Ten tweede veel clients verbinding maken met web-app, die de clients naar ASRS met het toegangstoken en een eindpunt omleiden. Clients stellen vervolgens de WebSocket-verbindingen met ASRS.

Nadat alle clients verbindingen tot stand brengen, start ze-bericht met een tijdstempel op de specifieke Hub elke seconde verzenden. De Hub kan het bericht terug naar de oorspronkelijke client. Elke client berekent de latentie wanneer deze weer de echo-bericht ontvangt.

De stappen 5\~8 (rood gemarkeerde verkeer) zijn in een lus, die worden uitgevoerd voor een standaardduur (5 minuten) en ophalen van de statistieken van alle bericht latentie.
De prestatiehandleiding voor geeft de maximale nummer van de client-verbinding.

![Echo](./media/signalr-concept-performance/echo.png)

**Echo**van gedrag bepaalt dat de maximale bandbreedte voor binnenkomende gelijk aan het maximum aantal uitgaande bandbreedte is. Zie de volgende tabel.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen                       | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Inkomend/uitgaand (bericht/s) | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Inkomend/uitgaand bandbreedte (bytes/s) | 2 MIN.    | 4 MIN.    | 10M   | 20 MILJOEN    | 40 MIN.    | 100 MILJOEN   | 200 MILJOEN    |

In dit geval gebruik roept elke client de hub die is gedefinieerd in de app-server. De hub worden alleen de methode die is gedefinieerd in de oorspronkelijke client aanroepen. Deze hub is het meest lichte gewogen hub voor **echo**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Zelfs voor dit eenvoudige hub, de druk netwerkverkeer op de app-server is ook belangrijk als de **echo** binnenkomend bericht toeneemt. Daarom is veel appservers voor grote SKU-laag vereist. De volgende tabel bevat het aantal van de app-server voor elke laag.


|    Echo          | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> De client verbinding number, berichtgrootte, bericht verzenden snelheid, SKU-laag en de toepassingsserver CPU/geheugen hebben invloed op de algehele prestaties van **echo**.

### <a name="broadcast"></a>Uitzenden

Voor **uitzending**, wanneer de web-app ontvangt het bericht, zendt op alle clients. De meer clients om uit te zenden, het meer berichtverkeer naar alle clients. Zie het volgende diagram.

![Uitzenden](./media/signalr-concept-performance/broadcast.png)

Het kenmerk van broadcast is dat er een klein aantal clients broadcasting, wat betekent dat de bandbreedte van het binnenkomende bericht is klein dat, maar de uitgaande bandbreedte enorm is. De bandbreedte voor uitgaand multicastverkeer voor message wordt verhoogd als de clientverbinding of broadcastsnelheid wordt verhoogd.

De maximale-clientverbindingen, aantal inkomende/uitgaande berichten, en bandbreedte worden in de volgende tabel samengevat.

|     Uitzenden             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Inkomende (bericht/s)  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Uitgaand (bericht/s) | 2,000 | 4,000 | 10.000 | 20,000 | 40,000 | 100.000 | 200.000 |
| Binnenkomende bandbreedte (bytes/s)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K      |
| Uitgaande bandbreedte (bytes/s) | 4 MIN.    | 8 MIN.    | 20 MILJOEN    | 40 MIN.    | 80M    | 200 MILJOEN    | 400 MIN.    |

De clients broadcasting geposte berichten zijn niet meer dan 4, dus vereist minder appservers vergeleken met **echo** omdat het binnenkomende bericht bedrag klein is. Twee appservers zijn voldoende voor overweging SLA en de prestaties. Maar de verbindingen van de standaard-server moeten worden verhoogd om te voorkomen dat niet in balans met name voor Unit50 en Unit100.

|   Uitzenden      | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
>
> De verbindingen van de standaard-server van 5 naar 40 op elke appserver om te voorkomen dat mogelijk is dat niet in balans server-verbindingen met ASRS verhogen.
>
> Het nummer van de client verbinding, de grootte van het bericht, de snelheid voor het verzenden van berichten en het SKU-laag invloed hebben op de algehele prestaties voor **broadcast**

### <a name="send-to-group"></a>Verzenden aan groep

**Verzenden aan groep** is soortgelijk patroon voor verkeer, behalve dat na het tot stand brengen van verbindingen met ASRS WebSocket-clients, ze groepen toevoegen moeten voordat u ze kunnen bericht verzenden naar een specifieke groep. De verkeersstroom wordt aangegeven door het volgende diagram.

![Verzenden aan groep](./media/signalr-concept-performance/sendtogroup.png)

Een lid en het aantal van de groep zijn afhankelijk van twee factoren dat dit invloed heeft op de prestaties. Ter vereenvoudiging van de analyse, definiëren we twee soorten groepen: kleine groep en big-groep.

- `small group`: 10 verbindingen in elke groep. Het groepsnummer is gelijk aan (max verbinding aantal) / 10. Bijvoorbeeld, voor 1 eenheid, als er verbinding tellingen van 1000, nu zijn 1000 / 10 = 100 groepen.

- `Big group`: Groepsnummer is altijd 10. Het aantal groepen lid is gelijk aan (max verbinding aantal) / 10. Voor 1 eenheid, als er verbinding tellingen van 1000, klikt u vervolgens elke groep heeft bijvoorbeeld 1000 / 10 = 100 leden.

**Verzenden aan groep** zorgt voor routering kosten in ASRS omdat er te vinden van de doel-verbindingen via de structuur van een gedistribueerde gegevens. Als de verzendende verbindingen verhogen, neemt ook de kosten toe.

#### <a name="small-group"></a>Kleine groep

De kosten voor routering is van belang voor het verzenden van berichten naar veel kleine groepen. De implementatie ASRS treffers op dit moment routering kostenlimiet op unit50. Toevoegen van meer CPU en geheugen niet helpen bij, zodat unit100 verbeteren kan niet meer standaard. Als u meer binnenkomende bandbreedte, neem dan contact op met de klantondersteuning voor aanpassing.

|   Verzenden naar een kleine groep     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50 | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000 | 100.000
| Aantal voor leden van groep        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Aantal groepen               | 100   | 200   | 500    | 1000  | 2,000  | 5.000  | 10.000 
| Inkomende (bericht/s)  | 200   | 400   | 1000  | 2,500  | 4,000  | 7,000  | 7,000   |
| Binnenkomende bandbreedte (bytes/s)  | 400 K  | 800 K  | 2 MIN.     | 5 MIN.     | 8 MIN.     | 14 MIN.    | 14 MIN.     |
| Uitgaand (bericht/s) | 2,000 | 4,000 | 10.000 | 25,000 | 40,000 | 70,000 | 70,000  |
| Uitgaande bandbreedte (bytes/s) | 4 MIN.    | 8 MIN.    | 20 MILJOEN    | 50 MILJOEN     | 80M    | 140 MIN.   | 140 MIN.    |

Er zijn veel clientverbindingen aanroepen van de hub, app-server-nummer is daarom ook essentieel voor prestaties. Het aantal van de voorgestelde app-server wordt vermeld in de volgende tabel.

|  Verzenden naar een kleine groep   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> Het nummer van de client verbinding, grootte van het bericht, snelheid voor het verzenden van berichten, routering kosten, SKU-laag en de toepassingsserver CPU/geheugen invloed hebben op de algehele prestaties van **verzenden aan kleine groep**.

#### <a name="big-group"></a>Grote groep

Voor **verzenden aan grote groep**, de uitgaande bandbreedte wordt het knelpunt voordat de limiet te maken met de routering kosten. De volgende tabel bevat de maximale uitgaande bandbreedte, die bijna hetzelfde is als **uitzending**.

|    Verzenden naar grote groep      | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000
| Aantal voor leden van groep        | 100   | 200   | 500    | 1000  | 2,000  | 5.000   | 10.000 
| Aantal groepen               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Inkomende (bericht/s)  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Binnenkomende bandbreedte (bytes/s)  | 80 K   | 40 K   | 40 K    | 20 K    | 40 K    | 40 K     | 40 K     |
| Uitgaand (bericht/s) | 2,000 | 4,000 | 10.000 | 20,000 | 40,000 | 100.000 | 200.000 |
| Uitgaande bandbreedte (bytes/s) | 8 MIN.    | 8 MIN.    | 20 MILJOEN    | 40 MIN.    | 80M    | 200 MILJOEN    | 400 MIN.    |

De verzendende aantal verbindingen is niet meer dan 40, de belasting van de app-server is klein, het nummer van de voorgestelde web-app is dus ook kleine.

|  Verzenden naar grote groep  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
>
> De verbindingen van de standaard-server van 5 naar 40 op elke appserver om te voorkomen dat mogelijk is dat niet in balans server-verbindingen met ASRS verhogen.
> 
> Het nummer van de client verbinding, grootte van het bericht, snelheid voor het verzenden van berichten, routering kosten en SKU-laag invloed hebben op de algehele prestaties van **verzenden aan grote groep**.

### <a name="send-to-connection"></a>Verzenden naar verbinding

In dit geval gebruiken wanneer clients tot stand brengen van verbindingen met ASRS, roept elke client een speciale hub om hun eigen verbindings-ID. De benchmark prestaties is verantwoordelijk voor het verzamelen van alle id's die verbinding, ze in willekeurige volgorde en deze opnieuw toewijzen aan alle clients als het doel van een verzenden. De clients blijven bericht verzenden naar de doelverbinding totdat de prestatietest is voltooid.

![Naar de client zenden](./media/signalr-concept-performance/sendtoclient.png)

Voor de routering kosten **verzenden / verbinding** is vergelijkbaar als **verzenden aan kleine groep**.

Als het aantal verbindingen toeneemt, wordt de algehele prestaties wordt beperkt door Routering kosten. Eenheid 50 heeft de limiet bereikt. Als gevolg hiervan kan niet eenheid 100 verder verbeteren.

De volgende tabel bevat een samenvattende statistieken na veel afgerond van het uitvoeren **verzenden / verbinding** benchmark

|   Verzenden naar verbinding   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50          | Unit100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Verbindingen                        | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000          | 100.000         |
| Inkomende / uitgaande (bericht/s) | 1000 | 2,000 | 5.000 | 8,000  | 9,000  | 20,000 | 20,000 |
| Inkomende / uitgaande bandbreedte (bytes/s) | 2 MIN.    | 4 MIN.    | 10M   | 16M    | 18M    | 40 MIN.       | 40 MIN.       |

Deze use case vereist hoge belasting van app-serverzijde. Zie de voorgestelde appserver tellen in de volgende tabel.

|  Verzenden naar verbinding  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> Het nummer van de client verbinding, grootte van het bericht, snelheid voor het verzenden van berichten, routering kosten, SKU-laag en de toepassingsserver CPU/geheugen invloed hebben op de algehele prestaties van **verzenden / verbinding**.

### <a name="aspnet-signalr-echobroadcastsend-to-connection"></a>ASP.NET SignalR echo/broadcast/verzenden / verbinding-

ASRS biedt dezelfde prestaties capaciteit voor ASP.NET SignalR. In deze sectie geeft het aantal van de voorgestelde web-app voor ASP.NET SignalR **echo**, **uitzending**, en **verzenden aan kleine groep**.

De prestatietest maakt gebruik van Azure-Web-App van [Standard S3 van Service-Plan](https://azure.microsoft.com/pricing/details/app-service/windows/) voor ASP.NET SignalR.

- `echo`

|   Echo           | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

- `broadcast`

|  Uitzenden       | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

- `Send-to-small-group`

|  Verzenden naar een kleine groep     | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

## <a name="serverless-mode"></a>Serverloze modus

Clients en ASRS betrokken zijn bij deze modus. Elke client staat voor een enkele verbinding. De client verzendt berichten via REST-API naar een andere client of broadcast-berichten op alle.

Verzenden van hoge dichtheid berichten via REST-API is niet zo efficiënt als WebSocket, omdat die nodig is voor het bouwen van een nieuwe HTTP-verbinding telkens - extra kosten in de modus zonder server zijn verbonden.

### <a name="broadcast-through-rest-api"></a>Verzonden via de REST-API
Alle clients tot stand brengen van verbindingen met ASRS WebSocket. Sommige clients start broadcasting via REST-API. Het bericht verzenden (inkomend) zijn allemaal via HTTP Post, die niet efficiënt vergeleken met WebSocket.

|   Verzonden via de REST-API     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Inkomende (bericht/s)  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Uitgaand (bericht/s) | 2,000 | 4,000 | 10.000 | 20,000 | 40,000 | 100.000 | 200.000 |
| Binnenkomende bandbreedte (bytes/s)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K      |
| Uitgaande bandbreedte (bytes/s) | 4 MIN.    | 8 MIN.    | 20 MILJOEN    | 40 MIN.    | 80M    | 200 MILJOEN    | 400 MIN.    |

### <a name="send-to-user-through-rest-api"></a>Verzenden naar de gebruiker via REST-API
Voordat ze verbinding maken met ASRS, de benchmark gebruikersnamen toegewezen aan alle clients. Nadat de clients tot stand WebSocket-verbindingen met ASRS gebracht, start ze berichten verzenden naar anderen via HTTP Post.

|   Verzenden naar de gebruiker via REST-API | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Inkomende (bericht/s)  | 300   | 600   | 900    | 1,300  | 2,000  | 10.000  | 18,000  |
| Uitgaand (bericht/s) | 300   | 600   | 900    | 1,300  | 2,000  | 10.000  | 18,000 |
| Binnenkomende bandbreedte (bytes/s)  | 600 K  | 1.2 MIN.  | 1.8 MIN.   | 2.6 MIN.   | 4 MIN.     | 10M     | 36M    |
| Uitgaande bandbreedte (bytes/s) | 600 K  | 1.2 MIN.  | 1.8 MIN.   | 2.6 MIN.   | 4 MIN.     | 10M     | 36M    |

## <a name="performance-test-environments"></a>Prestaties en testomgevingen

De prestatietest voor alle hierboven vermelde van use cases zijn uitgevoerd in Azure-omgeving. De meeste 50 client VM's, en 20 appserver worden VM's gebruikt.

Client VM-grootte: StandardDS2V2 (2 vCPU, 7G memory)

App-server VM-grootte: StandardF4sV2 (4 vCPU, geheugen van 8G)

Azure SignalR-SDK-server-verbindingen: 15

## <a name="performance-tools"></a>Hulpprogramma's voor prestaties

https://github.com/Azure/azure-signalr-bench/tree/master/SignalRServiceBenchmarkPlugin

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een overzicht van SignalR-Service-prestaties in typische use-casescenario's.

Lees de volgende handleiding voor meer informatie over de inhoud van het SignalR-Service en schaalbaarheid voor SignalR-Service.

* [Azure SignalR Service Internals](signalr-concept-internals.md)
* [Azure SignalR Service Scaling](signalr-howto-scale-multi-instances.md)