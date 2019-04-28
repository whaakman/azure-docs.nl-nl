---
title: Prestatiehandleiding voor voor Azure SignalR Service
description: Een overzicht van de prestaties van Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: zhshang
ms.openlocfilehash: f7cc05c8c2a299d809c4386d119fef58fa2548d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269438"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Prestatiehandleiding voor voor Azure SignalR Service

Een van de belangrijkste voordelen van het gebruik van Azure SignalR Service is het gemak van het SignalR-toepassingen schalen. In een grootschalige scenario zijn de prestaties een belangrijke factor. 

In deze handleiding stellen we de factoren die invloed hebben op prestaties van SignalR-toepassingen. We beschrijven normale prestaties in verschillende gebruiksscenario's. In de end moet stellen we de omgeving en de hulpprogramma's die u gebruiken kunt voor het genereren van een rapport over prestaties.

## <a name="term-definitions"></a>Term definities

*Inkomende*: Het binnenkomende bericht naar Azure SignalR Service.

*Uitgaande*: Het uitgaande bericht van Azure SignalR-Service.

*Bandbreedte*: De totale grootte van alle berichten in één seconde.

*Standaardmodus*: De modus met de standaard-werken als een Azure SignalR Service-exemplaar is gemaakt. Azure SignalR-Service wordt verwacht dat de app-server tot stand brengen van een verbinding met het voordat een clientverbindingen.

*Serverloze modus*: Een modus waarin Azure SignalR Service alleen clientverbindingen accepteert. Er is geen serververbinding is toegestaan.

## <a name="overview"></a>Overzicht

Azure SignalR Service definieert zeven standaard-laag voor verschillende prestatie-capaciteit. Deze handleiding vindt u antwoorden op de volgende vragen:

-   Wat is de standaard Azure SignalR Service-prestaties voor elke laag?

-   Azure SignalR Service voldoet aan de mijn vereisten voor doorvoer van bericht (bijvoorbeeld verzenden 100.000 berichten per seconde)?

-   Voor mijn specifieke scenario, welke laag geschikt voor mij is? Of hoe kan ik de juiste laag selecteren?

-   Wat voor soort app-server (VM-grootte) is geschikt voor mij? Hoe vaak moet ik implementeren?

Als u wilt deze vragen te beantwoorden, biedt deze handleiding eerst een op hoog niveau Beschrijving van de factoren die invloed hebben op prestaties. Vervolgens ziet u de maximale binnenkomende en uitgaande berichten voor elke laag voor typische gebruiksvoorbeelden: **echo**, **uitzending**, **verzenden naar groep**, en **verzenden naar verbinding** (peer-to-peer chatten).

Deze handleiding dekken niet alle scenario's (en verschillende use cases, berichtgrootten, berichten verzenden patronen, enzovoort). Maar het biedt enkele methoden om u te helpen:

- Evalueer uw geschatte vereiste voor de binnenkomende of uitgaande berichten.
- De juiste lagen vinden door te controleren van de tabel van de prestaties.

## <a name="performance-insight"></a>Queryprestaties

Deze sectie beschrijft de methoden van de evaluatie van prestaties, en vervolgens een lijst met alle factoren die invloed hebben op prestaties. Uiteindelijk moet biedt het methoden om te evalueren prestatie-eisen.

### <a name="methodology"></a>Methodologie

*Doorvoer* en *latentie* zijn twee typische aspecten van de prestaties controleren. Voor Azure SignalR Service heeft elke SKU-laag een eigen beleid beperking doorvoer. Het beleid wordt gedefinieerd *de maximaal toegestane doorvoer (binnenkomende en uitgaande bandbreedte)* zoals de maximale doorvoer behaalde wanneer 99 procent van de berichten latentie die is minder dan 1 seconde.

Latentie is de tijdsduur van de verbinding voor het ontvangen van het antwoordbericht van Azure SignalR-Service te verzenden. Laten we **echo** als voorbeeld. Elke clientverbinding wordt een tijdstempel in het bericht toegevoegd. Het oorspronkelijke bericht verzendt van de appserver-hub terug naar de client. De vertraging doorgifte wordt dus eenvoudig berekend door elke clientverbinding. De tijdstempel is gekoppeld voor elk bericht dat in **uitzending**, **verzenden naar groep**, en **verzenden naar verbinding**.

Meerdere VM's om te simuleren duizenden gelijktijdige clientverbindingen, gemaakt in een virtueel particulier netwerk in Azure. Alle VM's verbinding maken met de dezelfde Azure SignalR Service-exemplaar.

In de standaardmodus van Azure SignalR Service, appserver-VM's worden geïmplementeerd in hetzelfde virtuele particuliere netwerk als client VM's. Alle client-VM's en app-server virtuele machines zijn geïmplementeerd in hetzelfde netwerk van dezelfde regio bevinden om te voorkomen dat de regio-overschrijdende latentie.

### <a name="performance-factors"></a>Prestatiefactoren

In theorie wordt Azure SignalR Service capaciteit beperkt door de rekenbronnen die: CPU, geheugen en netwerk. Meer verbindingen met Azure SignalR Service bijvoorbeeld ervoor zorgen dat de service meer geheugen gebruiken. Voor grotere berichtenverkeer (bijvoorbeeld elk bericht groter is dan 2048 bytes), Azure SignalR-Service moet meer CPU-cycli voor het verwerken van verkeer. Ondertussen Azure netwerkbandbreedte ook standaard een limiet ingesteld voor maximum verkeer.

Het transporttype is een andere factor die van invloed op prestaties. Er zijn drie typen [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [Server-verzonden-gebeurtenis](https://en.wikipedia.org/wiki/Server-sent_events), en [Long-Polling](https://en.wikipedia.org/wiki/Push_technology). 

WebSocket is een in twee richtingen en full-duplex communicatieprotocol via één TCP-verbinding. Server-verzonden-gebeurtenis is een Unidirectioneel protocol voor pushberichten uit naar de client. Long-Polling vereist dat de clients te peilen regelmatig gegevens van de server via een HTTP-aanvraag. Voor de dezelfde API dezelfde voorwaarden, WebSocket is de beste prestaties, Server-verzonden-gebeurtenis is trager en Long-Polling het langzaamst is. Azure SignalR Service raadt WebSocket standaard.

De kosten voor het doorsturen van bericht beperkt ook prestaties. Azure SignalR Service speelt een rol als een router bericht het bericht van een set van clients of servers met andere clients of servers stuurt. Een ander scenario of API vereist een andere routeringsbeleid. 

Voor **echo**, de client verzendt een bericht naar zichzelf en de bestemming van de routering is ook zelf. Dit patroon heeft de laagste routering kosten. Maar voor **uitzending**, **verzenden naar groep**, en **verzenden naar verbinding**, Azure SignalR Service nodig heeft om te controleren of de doel-verbindingen via de interne gedistribueerde gegevens structuur. Deze extra verwerking maakt gebruik van meer CPU, geheugen en netwerkbandbreedte. Als gevolg hiervan is trager.

In de standaardmodus kan de app-server ook een knelpunt voor bepaalde scenario's. De SDK van Azure SignalR heeft om aan te roepen van de hub, terwijl er wordt een live-verbinding met elke client via de heartbeat signalen bijgehouden.

De client verzendt een bericht in de modus voor serverloze, door HTTP post, die niet zo efficiënt als WebSocket.

Een andere factor is een protocol: JSON en [MessagePack](https://msgpack.org/index.html). MessagePack is kleiner dan de grootte en geleverde sneller dan de JSON. MessagePack kan niet door de prestaties verbeteren. De prestaties van Azure SignalR Service is niet gevoelig zijn voor protocollen, omdat deze niet worden gedecodeerd de berichtnettolading van het tijdens het doorsturen van berichten van clients op servers of vice versa.

Kortom, de volgende factoren invloed hebben op de inkomende en uitgaande capaciteit:

-   SKU-laag (CPU/geheugen)

-   Aantal verbindingen

-   Berichtgrootte

-   snelheid van berichten verzenden

-   Type transport (WebSocket, Server-verzonden-gebeurtenis of Long-Polling)

-   Gebruiksscenario's (routering kosten)

-   App-server en service-verbindingen (in de servermodus)


### <a name="finding-a-proper-sku"></a>Zoeken naar een juiste SKU

Hoe kunt u de capaciteit voor binnenkomend en uitgaand evalueren of vinden welke laag is geschikt voor een specifieke use case?

Wordt ervan uitgegaan dat de app-server is een krachtig genoeg is niet van het prestatieknelpunt. Controleer de maximale bandbreedte voor binnenkomende en uitgaande voor elke laag.

#### <a name="quick-evaluation"></a>Een snelle evaluatie

Vereenvoudig de evaluatie eerst laten we veronderstelling dat sommige standaardinstellingen: 

- Het transporttype is WebSocket.
- De grootte van het bericht is 2048 bytes.
- Elke seconde wordt een bericht verzonden.
- Azure SignalR-Service is in de standaardmodus.

Voor elke laag heeft een eigen maximale bandbreedte voor binnenkomende en uitgaande bandbreedte. Nadat de verbinding binnenkomend of uitgaand de limiet overschrijdt kan niet worden gegarandeerd door een goede gebruikerservaring.

**Echo** geeft de maximale bandbreedte voor binnenkomende omdat deze de laagste routering kosten heeft. **Uitzenden** definieert het maximum aantal uitgaande message-bandbreedte.

Voer *niet* groter zijn dan de geselecteerde waarden in de volgende twee tabellen.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen                       | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| **Binnenkomende bandbreedte** | **2 MBps**    | **4 MBps**    | **10 MBps**   | **20 MBps**    | **40 MBps**    | **100 MBps**   | **200 MBps**    |
| Uitgaande bandbreedte | 2 Mbps   | 4 Mbps   | 10 Mbps  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |


|     Uitzenden             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Binnenkomende bandbreedte  | 4 k   | 4 k   | 4 k    | 4 k    | 4 k    | 4 k     | 4 k    |
| **Uitgaande bandbreedte** | **4 MBps**    | **8 MBps**    | **20 MBps**    | **40 MBps**    | **80 MBps**    | **200 MBps**    | **400 MBps**   |

*Binnenkomende bandbreedte* en *uitgaande bandbreedte* zijn de totale berichtgrootte per seconde.  Hier volgen de formules:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

- *inboundConnections*: Het aantal verbindingen die het bericht te verzenden.

- *outboundConnections*: Het aantal verbindingen die het bericht te ontvangen.

- *messageSize*: De grootte van een enkel bericht (gemiddelde waarde). Een kleine bericht dat kleiner is dan 1024 bytes heeft invloed op de prestaties die vergelijkbaar is met een bericht 1024 bytes.

- *sendInterval*: De tijd van het verzenden van één bericht. Het is doorgaans 1 seconde per bericht, wat betekent dat elke seconde van één bericht verzenden. Een kleinere waarde betekent meer bericht verzonden in een bepaalde periode. 0,5 seconden per bericht betekent bijvoorbeeld dat twee berichten verzonden per seconde.

- *Verbindingen*: De vastgelegde maximale drempelwaarde voor Azure SignalR Service voor elke laag. Als het nummer van de verbinding is verhoogd, wordt het ten koste gaan van bandbreedtebeperking van verbinding.

#### <a name="evaluation-for-complex-use-cases"></a>Evaluatie voor complexe use-cases

##### <a name="bigger-message-size-or-different-sending-rate"></a>Grootte van het bericht groter of andere verzendsnelheid

De echte use-case is gecompliceerder. Er kan een bericht groter is dan 2048 bytes verzonden, of de verzendsnelheid bericht is niet één bericht per seconde. We gaan de uitzending van Unit100 als voorbeeld om te zoeken over het evalueren van de prestaties.

De volgende tabel ziet u een echte use case van **uitzending**. Maar de grootte van het bericht, aantal verbindingen en bericht verzenden tarief verschillen van wat wordt ervan uitgegaan in de vorige sectie dat. De vraag is hoe we kunt deduceren een van deze items (grootte van het bericht, aantal verbindingen of snelheid voor het verzenden van berichten) als we weten slechts twee dat.

| Uitzenden  | Berichtgrootte | Binnenkomende berichten per seconde | Verbindingen | Intervallen verzenden |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 KB                | 1                        | 100.000     | 5 per seconde                      |
| 2 | 256 kB               | 1                        | 8,000       | 5 per seconde                      |

De volgende formule is eenvoudig te afleiden op basis van de vorige formule:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Voor Unit100 is de maximale bandbreedte voor uitgaand 400 MB van de vorige tabel. Het maximum aantal uitgaande verbindingen moet voor de grootte van een bericht van 20-KB, 400 MB \* 5 / 20 KB = 100.000, die overeenkomt met de werkelijke waarde.

##### <a name="mixed-use-cases"></a>Gemengde use-cases

Normaal gesproken combineert de vier eenvoudige gebruiksvoorbeelden samen de echte use-case: **echo**, **uitzending**, **verzenden naar groep**, en **verzenden naar verbinding**. De methode die u gebruiken om te evalueren van de capaciteit is:

1. De gemengde gebruiksvoorbeelden onderverdelen in vier eenvoudige gebruiksvoorbeelden.
1. De maximumgrootte van een bericht binnenkomende en uitgaande bandbreedte berekenen met behulp van de vorige formules afzonderlijk.
1. Som van de Bandbreedteberekeningen voor het ophalen van de totale bandbreedte van de maximale binnenkomend en uitgaand. 

Kies een van de juiste laag uit de tabellen Maximumbandbreedte binnenkomend en uitgaand.

> [!NOTE]
> Voor het verzenden van een bericht naar honderden of duizenden kleine groepen of van duizenden clients een bericht verzenden naar elkaar worden verbonden, worden de kosten voor routering dominante. Deze gevolgen rekening nemen.

Zorg ervoor dat de app-server voor het geval van het gebruik van een bericht te verzenden naar clients, *niet* het knelpunt. De volgende 'CASESTUDY'-sectie vindt u richtlijnen over het aantal appservers u nodig hebt en het aantal serververbindingen dient u te configureren.

## <a name="case-study"></a>Casestudy

De volgende secties doorlopen vier standaard use cases voor WebSocket-transport: **echo**, **uitzending**, **verzenden naar groep**, en **verzenden naar verbinding**. Voor elk scenario vindt de sectie u de huidige capaciteit voor binnenkomend en uitgaand voor Azure SignalR Service. Hierin wordt ook uitgelegd over de belangrijkste factoren die invloed hebben op prestaties.

In de standaardmodus maakt de appserver vijf server-verbindingen met Azure SignalR Service. De app-server maakt standaard gebruik van de Azure SignalR Service-SDK. In de volgende Prestatietestresultaten serververbindingen verhoogd tot 15 (of meer voor uitzenden en het verzenden van een bericht naar een grote groep).

Verschillende use cases hebben verschillende vereisten voor app-servers. **Uitzenden** moet klein aantal servers met Apps. **Echo** of **verzenden naar verbinding** moet veel appservers.

Gebruik in alle gevallen, de standaardgrootte van het bericht is 2048 bytes en het interval voor het verzenden van bericht is 1 seconde.

### <a name="default-mode"></a>Standaardmodus

Clients, servers met web apps en Azure SignalR Service betrokken zijn bij de standaardmodus. Elke client staat voor een enkele verbinding.

#### <a name="echo"></a>Echo

Een web-app maakt eerst verbinding met Azure SignalR Service. Ten tweede veel clients verbinding maken met de web-app, die de clients omgeleid naar Azure SignalR Service met het toegangstoken en een eindpunt. Vervolgens dat de clients WebSocket-verbindingen met Azure SignalR Service tot stand brengen.

Nadat alle clients verbindingen tot stand brengen, start ze verzenden van een bericht met een tijdstempel op de specifieke hub per seconde. De hub kan het bericht terug naar de oorspronkelijke client. Elke client berekent de latentie wanneer deze weer de echo-bericht ontvangt.

In het volgende diagram wordt 5 tot en met 8 (rood gemarkeerde verkeer) zijn in een lus. De lus wordt uitgevoerd voor een standaardduur (5 minuten) en de statistieken van alle bericht latentie opgehaald.

![Verkeer voor de echo-use-case](./media/signalr-concept-performance/echo.png)

Het gedrag van **echo** bepaalt dat de maximale bandbreedte voor inkomend gelijk aan het maximum aantal uitgaande bandbreedte is. Zie de volgende tabel voor meer informatie.

|       Echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen                       | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Inkomende/uitgaande berichten per seconde | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Inkomende/uitgaande bandbreedte | 2 Mbps   | 4 Mbps   | 10 Mbps  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |

In dit geval gebruik roept elke client de hub die is gedefinieerd in de app-server. De hub worden alleen de methode die is gedefinieerd in de oorspronkelijke client aanroepen. Deze hub is de meest eenvoudige hub voor **echo**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Zelfs voor dit eenvoudige hub, de druk verkeer op de appserver is belangrijk als de **echo** binnenkomend bericht belasting toeneemt. Deze druk verkeer vereist veel appservers voor grote SKU-laag. De volgende tabel bevat het aantal van de app-server voor elke laag.


|    Echo          | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> De client verbinding number, berichtgrootte, bericht verzenden snelheid, de SKU-laag en de CPU/geheugen van de appserver invloed hebben op de algehele prestaties van **echo**.

#### <a name="broadcast"></a>Uitzenden

Voor **uitzending**, wanneer de web-app het bericht ontvangt verzendt een broadcast naar alle clients. Er zijn meer clients om uit te zenden, het meer berichtverkeer er is op alle clients. Zie het volgende diagram.

![Verkeer voor de uitzending use-case](./media/signalr-concept-performance/broadcast.png)

Een klein aantal clients worden uitgezonden. De bandbreedte multicastverkeer voor message klein is, maar de uitgaande bandbreedte is enorm. De bandbreedte voor uitgaand multicastverkeer voor message wordt verhoogd als de clientverbinding of broadcastsnelheid wordt verhoogd.

De volgende tabel geeft een overzicht van de maximale-clientverbindingen, aantal inkomende/uitgaande berichten, en bandbreedte.

|     Uitzenden             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Binnenkomende berichten per seconde  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Uitgaande berichten per seconde | 2,000 | 4,000 | 10.000 | 20,000 | 40,000 | 100.000 | 200.000 |
| Binnenkomende bandbreedte  | 4 k   | 4 k   | 4 k    | 4 k    | 4 k    | 4 k     | 4 k     |
| Uitgaande bandbreedte | 4 Mbps   | 8 MBps   | 20 MBps   | 40 MBps   | 80 MBps   | 200 MBps   | 400 MBps   |

De clients broadcasting geposte berichten zijn niet meer dan vier. Ze nodig hebben minder appservers vergeleken met **echo** omdat het bedrag multicastverkeer voor message klein is. Twee appservers zijn voldoende voor zowel SLA- en Prestatieoverwegingen. Maar u moet de standaard-server-verbindingen om te voorkomen dat imbalance, met name voor Unit50 en Unit100 verhogen.

|   Uitzenden      | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Verhoog de verbindingen van de standaard-server van 5 tot en met 40 op elke appserver om te voorkomen dat mogelijk is dat niet in balans server-verbindingen met Azure SignalR Service.
>
> Het nummer van de client verbinding, de grootte van het bericht, de snelheid voor het verzenden van berichten en het SKU-laag van invloed zijn op de algehele prestaties voor **uitzending**.

#### <a name="send-to-group"></a>Verzenden aan groep

De **verzenden naar groep** use-case heeft een vergelijkbaar patroon verkeer naar **uitzending**. Het verschil is dat nadat clients tot stand brengen van verbindingen met Azure SignalR Service WebSocket, ze moeten deelnemen aan groepen voordat ze een bericht naar een specifieke groep kunnen verzenden. Het volgende diagram illustreert de werkstroom verkeer.

![Verkeer voor de aanvraag verzenden aan groep gebruiken](./media/signalr-concept-performance/sendtogroup.png)

Een lid en het aantal van de groep zijn afhankelijk van twee factoren die invloed hebben op prestaties. Ter vereenvoudiging van de analyse, definiëren we twee soorten groepen:

- **Kleine groep**: Elke groep heeft 10 verbindingen. Het groepsnummer is gelijk aan (max verbinding aantal) / 10. Bijvoorbeeld, voor Unit1, als er 1000 verbinding tellingen, nu zijn 1000 / 10 = 100 groepen.

- **Grote groep**: Het groepsnummer is altijd 10. Het aantal groepen lid is gelijk aan (max verbinding aantal) / 10. Voor Unit1, als er 1000 verbinding tellingen, klikt u vervolgens elke groep heeft bijvoorbeeld 1000 / 10 = 100 leden.

**Verzenden naar groep** zorgt voor een routering kosten voor Azure SignalR Service omdat het vinden van de doel-verbindingen via de structuur van een gedistribueerde gegevens heeft. Als de verzendende verbindingen verhoogt, verhoogt de kosten.

##### <a name="small-group"></a>Kleine groep

De kosten voor routering is van belang voor het verzenden van berichten naar veel kleine groepen. Op dit moment de Azure SignalR Service-implementatie komt binnen via de routering kostenlimiet op Unit50. Toevoegen van meer CPU en geheugen niet helpen, zodat Unit100 verbeteren kan niet meer standaard. Als u meer binnenkomende bandbreedte nodig hebt, moet u contact opnemen met klantondersteuning.

|   Verzenden naar een kleine groep     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50 | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000 | 100.000
| Aantal voor leden van groep        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Aantal groepen               | 100   | 200   | 500    | 1000  | 2,000  | 5.000  | 10.000 
| Binnenkomende berichten per seconde  | 200   | 400   | 1000  | 2,500  | 4,000  | 7,000  | 7,000   |
| Binnenkomende bandbreedte  | 400 k  | 800 kBps  | 2 Mbps     | 5 Mbps     | 8 MBps     | 14 MBps    | 14 MBps     |
| Uitgaande berichten per seconde | 2,000 | 4,000 | 10.000 | 25,000 | 40,000 | 70,000 | 70,000  |
| Uitgaande bandbreedte | 4 Mbps    | 8 MBps    | 20 MBps    | 50 MBps     | 80 MBps    | 140 MBps   | 140 MBps    |

Veel clientverbindingen zijn aanroepen van de hub, zodat het nummer van de app is ook belangrijk voor prestaties. De volgende tabel bevat de aantallen van de voorgestelde Apps-server.

|  Verzenden naar een kleine groep   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> De client verbinding number, berichtgrootte, bericht verzenden snelheid, routering kosten, SKU-laag en CPU/geheugen van de appserver invloed hebben op de algehele prestaties van **verzenden naar een kleine groep**.

##### <a name="big-group"></a>Grote groep

Voor **verzenden naar grote groep**, de uitgaande bandbreedte wordt het knelpunt voordat de limiet te maken met de routering kosten. De volgende tabel bevat de maximale uitgaande bandbreedte, die bijna hetzelfde is als die voor **uitzending**.

|    Verzenden naar grote groep      | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000
| Aantal voor leden van groep        | 100   | 200   | 500    | 1000  | 2,000  | 5.000   | 10.000 
| Aantal groepen               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Binnenkomende berichten per seconde  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Binnenkomende bandbreedte  | 80 kBps   | 40 kBps   | 40 kBps    | 20 kBps    | 40 kBps    | 40 kBps     | 40 kBps     |
| Uitgaande berichten per seconde | 2,000 | 4,000 | 10.000 | 20,000 | 40,000 | 100.000 | 200.000 |
| Uitgaande bandbreedte | 8 MBps    | 8 MBps    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

De verzendende aantal verbindingen is niet meer dan 40. De werkbelasting van de app-server is klein, zodat het aanbevolen aantal web-apps klein is.

|  Verzenden naar grote groep  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Verhoog de verbindingen van de standaard-server van 5 tot en met 40 op elke appserver om te voorkomen dat mogelijk is dat niet in balans server-verbindingen met Azure SignalR Service.
> 
> Het nummer van de client verbinding, grootte van het bericht, snelheid voor het verzenden van berichten, routering kosten en SKU-laag van invloed zijn op de algehele prestaties van **verzenden naar grote groep**.

#### <a name="send-to-connection"></a>Verzenden naar verbinding

In de **verzenden naar verbinding** use-case, wanneer clients tot stand brengen van verbindingen met Azure SignalR Service, elke client roept een speciale hub om hun eigen verbindings-ID. De benchmark prestaties verzamelt alle id's die verbinding, plaatst ze en ze opnieuw toegewezen aan alle clients als het doel van een verzenden. De clients blijven het bericht verzenden naar de doelverbinding, totdat de prestatietest is voltooid.

![Verkeer voor de aanvraag verzenden naar de client gebruiken](./media/signalr-concept-performance/sendtoclient.png)

Voor de routering kosten **verzenden naar verbinding** is vergelijkbaar met de kosten voor **verzenden naar een kleine groep**.

Als het aantal verbindingen toeneemt, beperkt de kosten voor routering algehele prestaties. Unit50 heeft de limiet bereikt. Als gevolg hiervan kan niet Unit100 verder verbeteren.

De volgende tabel bevat een overzicht van statistische na veel afgerond van het uitvoeren de **verzenden naar verbinding** benchmark.

|   Verzenden naar verbinding   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50          | Unit100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Verbindingen                        | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000          | 100.000         |
| Inkomende/uitgaande berichten per seconde | 1000 | 2,000 | 5.000 | 8,000  | 9,000  | 20,000 | 20,000 |
| Inkomende/uitgaande bandbreedte | 2 Mbps    | 4 Mbps    | 10 Mbps   | 16 MBps    | 18 MBps    | 40 MBps       | 40 MBps       |

Deze use case vereist hoge belasting op de server app. Zie de voorgestelde appserver tellen in de volgende tabel.

|  Verzenden naar verbinding  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> De client verbinding number, berichtgrootte, bericht verzenden snelheid, routering kosten, SKU-laag en CPU/geheugen voor de appserver invloed hebben op de algehele prestaties van **verzenden naar verbinding**.

#### <a name="aspnet-signalr-echo-broadcast-and-send-to-small-group"></a>ASP.NET SignalR echo, uitzenden, maken en te verzenden naar een kleine groep

Azure SignalR Service biedt dezelfde prestaties capaciteit voor ASP.NET SignalR. 

De prestatietest maakt gebruik van Azure Web Apps van [Standard S3 van Service-Plan](https://azure.microsoft.com/pricing/details/app-service/windows/) voor ASP.NET SignalR.

De volgende tabel geeft het aantal van de voorgestelde web-app voor ASP.NET SignalR **echo**.

|   Echo           | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

De volgende tabel geeft het aantal van de voorgestelde web-app voor ASP.NET SignalR **uitzending**.

|  Uitzenden       | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

De volgende tabel geeft het aantal van de voorgestelde web-app voor ASP.NET SignalR **verzenden naar een kleine groep**.

|  Verzenden naar een kleine groep     | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Verbindingen      | 1000 | 2,000 | 5.000 | 10.000 | 20,000 | 50,000 | 100.000 |
| Aantal App-server | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

### <a name="serverless-mode"></a>Serverloze modus

Clients en Azure SignalR Service zijn in de modus voor serverloze betrokken. Elke client staat voor een enkele verbinding. De client verzendt berichten via de REST-API naar een andere client of broadcast-berichten op alle.

Verzenden van hoge dichtheid berichten via de REST-API is niet zo efficiënt als het gebruik van WebSocket. Moet u een nieuwe telkens van de HTTP-verbinding kunt maken en die zijn extra kosten in de modus zonder server.

#### <a name="broadcast-through-rest-api"></a>Verzonden via de REST-API
Alle clients tot stand brengen van verbindingen met Azure SignalR Service WebSocket. Sommige clients start broadcasting via de REST-API. Het bericht verzenden (inkomend) is allemaal via HTTP Post, die niet efficiënt vergeleken met WebSocket.

|   Verzonden via de REST-API     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Binnenkomende berichten per seconde  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Uitgaande berichten per seconde | 2,000 | 4,000 | 10.000 | 20,000 | 40,000 | 100.000 | 200.000 |
| Binnenkomende bandbreedte  | 4 k    | 4 k    | 4 k     | 4 k     | 4 k     | 4 k      | 4 k      |
| Uitgaande bandbreedte | 4 Mbps    | 8 MBps    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

#### <a name="send-to-user-through-rest-api"></a>Verzenden naar de gebruiker via REST-API
Voordat ze verbinding maken met Azure SignalR Service, de benchmark gebruikersnamen toegewezen aan alle clients. Nadat de clients tot stand brengen van de WebSocket-verbindingen met Azure SignalR Service, start ze berichten verzenden naar anderen via HTTP Post.

|   Verzenden naar de gebruiker via REST-API | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Verbindingen               | 1000 | 2,000 | 5.000  | 10.000 | 20,000 | 50,000  | 100.000 |
| Binnenkomende berichten per seconde  | 300   | 600   | 900    | 1,300  | 2,000  | 10.000  | 18,000  |
| Uitgaande berichten per seconde | 300   | 600   | 900    | 1,300  | 2,000  | 10.000  | 18,000 |
| Binnenkomende bandbreedte  | 600 kBps  | 1.2 MBps  | 1.8 MBps   | 2.6 MBps   | 4 Mbps     | 10 Mbps     | 36 MBps    |
| Uitgaande bandbreedte | 600 kBps  | 1.2 MBps  | 1.8 MBps   | 2.6 MBps   | 4 Mbps     | 10 Mbps     | 36 MBps    |

## <a name="performance-test-environments"></a>Prestaties en testomgevingen

Gebruik voor alle eerder vermelde gevallen, we de prestaties te testen in een Azure-omgeving die wordt uitgevoerd. We hebben gebruikt, maximaal 50 client VM's en 20 appserver-VM's. Hier volgen enkele details:

- Client VM-grootte: StandardDS2V2 (2 vCPU, 7G memory)

- App-server VM-grootte: StandardF4sV2 (4 vCPU, geheugen van 8G)

- Azure SignalR-SDK-server-verbindingen: 15

## <a name="performance-tools"></a>Hulpprogramma's voor prestaties

U kunt de prestaties van hulpprogramma's voor Azure SignalR Service vinden op [GitHub](https://github.com/Azure/azure-signalr-bench/tree/master/SignalRServiceBenchmarkPlugin).

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een overzicht van Azure SignalR Service-prestaties in typische gebruiksscenario's.

Als u meer informatie over leest de inhoud van de service en schalen, u de volgende handleidingen:

* [Azure SignalR Service-inhoud](signalr-concept-internals.md)
* [Azure SignalR Service schalen](signalr-howto-scale-multi-instances.md)
