---
title: Handleiding voor Azure Relay Hybrid Connections-protocol | Microsoft Docs
description: Voor Azure Relay Hybrid Connections-protocol.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: ''
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: clemensv
ms.openlocfilehash: 913e702cc72472e81937bfe3b0939695daadc011
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543508"
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay Hybrid Connections-protocol

Azure Relay is een van de belangrijkste mogelijkheden pijlers van de Azure Service Bus-platform. De nieuwe _hybride verbindingen_ mogelijkheden van Relay is een veilige, open-protocolversie ontwikkeling op basis van HTTP en WebSockets. Vervangt de vorige, genaamd _BizTalk Services_ functie die is gebouwd op een eigen protocol foundation. De integratie van hybride verbindingen in Azure App Services, blijven functioneren als-is.

Hybrid Connections kunt bidirectionele, binaire gegevensstroom communicatie en eenvoudige datagram stroom tussen twee netwerktoepassingen. Een van beide of beide partijen zich achter NAT's of firewalls kunnen bevinden.

Dit artikel beschrijft de client-side-interacties met de relay Hybrid Connections voor het verbinden van clients in listener- en verzender-rollen. Ook wordt beschreven hoe listeners aanvragen en nieuwe verbindingen accepteren.

## <a name="interaction-model"></a>Interactie van model

De relay Hybrid Connections verbindt twee partijen door een punt rendezvous in de Azure-cloud die partijen kunnen detecteren en verbinding maken met vanuit het perspectief van hun eigen netwerk te bieden. Het rendezvous-punt wordt 'Hybride verbinding' in deze en andere documentatie genoemd in de API's, en ook in de Azure-portal. Het service-eindpunt voor hybride verbindingen wordt aangeduid als de 'service' voor de rest van dit artikel.

De service kunnen voor het doorgeven van Web Socket-verbindingen en HTTP (S) aanvragen en antwoorden.

Het model interactie leans op de naamgeving tot stand gebracht door veel andere netwerken API's. Er is een listener die eerst geeft aan dat de gereedheid voor het afhandelen van binnenkomende verbindingen, en vervolgens accepteert wanneer ze binnenkomen. Aan de andere kant is een client verbinding maakt voor de listener, die verbinding kunnen worden geaccepteerd voor het tot stand brengen van een pad bidirectionele communicatie wordt verwacht. 'Connect', "Listen" en 'Accepteren' zijn dezelfde voorwaarden die u in de meeste socket API's vinden.

Een relayed communicatiemodel heeft beide partijen uitgaande verbindingen naar een service-eindpunt maken. Dit zorgt ervoor dat de 'listener' ook een 'client' in gewone gebruik en kan ook leiden tot andere overloads terminologie. De precieze terminologie daarom gebruikt voor hybride verbindingen is als volgt:

De programma's aan beide zijden van een verbinding zijn "-clients," genoemd, omdat ze clients naar de service. De client die wordt gewacht en verbindingen accepteert de 'listener' is of wordt gezegd in de 'listener rol'. De client die een nieuwe verbinding naar een listener via de service initieert de 'zender' wordt genoemd, of bevindt zich in de 'afzender rol'.

### <a name="listener-interactions"></a>Listener-interacties

De listener beschikt over vijf interacties met de service. alle gegevens van wire worden verderop in dit artikel in de sectie documentatie beschreven.

De berichten luisteren naar en accepteren aanvragen worden ontvangen van de service. Het vernieuwen en Ping bewerkingen zijn verzonden door de listener.

#### <a name="listen-message"></a>Bericht luisteren

Om aan te geven van de gereedheid van de service die een listener is gereed om te accepteren, verbindingen, maakt het een uitgaande WebSocket-verbinding. De handshake van verbinding is, wordt de naam van een hybride verbinding geconfigureerd op de Relay-naamruimte en een beveiligingstoken dat rechtstreeks op de 'Listen"biedt deze naam.

Wanneer de WebSocket wordt geaccepteerd door de service, de registratie is voltooid en het tot stand gebrachte WebSocket blijft actief als het 'besturingselement kanaal' voor het inschakelen van alle volgende interacties. De service kunnen maximaal 25 gelijktijdige listeners een hybride verbinding. Het quotum voor AppHooks wordt vastgesteld.

Voor hybride verbindingen, als er twee of meer actieve listeners worden binnenkomende verbindingen verdeeld over deze in willekeurige volgorde. geoorloofd distributie wordt geprobeerd met best-effort.

#### <a name="accept-message"></a>Bericht accepteren

Wanneer een afzender opent een nieuwe verbinding van de service, wordt de service kiest en waarschuwt een van de actieve listeners op de hybride verbinding. Deze melding wordt verzonden naar de listener via het open besturingskanaal als een JSON-bericht. Het bericht bevat de URL van de WebSocket-eindpunt waarmee de listener verbinding maken moet voor het accepteren van de verbinding.

De URL kan en rechtstreeks door de listener zonder extra werk moet worden gebruikt.
De gecodeerde gegevens is alleen geldig voor een korte tijd, in feite voor zolang als de afzender is bereid na afloop van de verbinding tot stand gebrachte end-to-end. Het maximum wordt ervan uitgegaan dat is 30 seconden. De URL kan alleen worden gebruikt voor een geslaagde verbindingspoging. Zodra de WebSocket-verbinding met de URL rendezvous tot stand is gebracht, wordt alle verdere activiteiten op deze WebSocket doorgegeven van en naar de afzender. Dit gebeurt zonder tussenkomst van de of interpretatie door de service.

### <a name="request-message"></a>Aanvraagbericht

Naast de WebSocket-verbindingen, kunt de listener ontvangen ook HTTP-aanvraag frames van een afzender, als deze mogelijkheid expliciet is ingeschakeld op de hybride verbinding.

Listeners waarmee hybride verbindingen worden gekoppeld met HTTP-ondersteuning moeten omgaan met de `request` gebaar. Een listener die niet verwerken `request` en daarom zorgt ervoor dat herhaalde time-outfouten optreden tijdens het verbinding kan worden op de blokkeringslijst door de service in de toekomst.

HTTP-frame-header metagegevens is vertaald naar JSON voor het verwerken van eenvoudiger door het framework listener ook omdat HTTP-header parseren bibliotheken sommige dan parsers voor JSON zijn. HTTP-metagegevens die alleen relevant is voor de relatie tussen de afzender en de Relay-HTTP-gateway, inclusief de autorisatie-informatie wordt niet doorgestuurd. HTTP-aanvraagtekst worden transparant overgebracht als binaire WebSocket frames.

De listener kan reageren op HTTP-aanvragen met een gebaar van de equivalente antwoord.

De aanvraag/antwoord-stroom maakt standaard gebruik van het besturingskanaal, maar kan worden 'bijgewerkt' naar een afzonderlijke rendezvous WebSocket wanneer vereist. Afzonderlijke WebSocket verbindingen doorvoer voor elke client-conversatie verbeteren, maar ze belasten de listener met meer verbindingen die worden verwerkt moeten, die mogelijk niet willen kunnen voor lichtgewicht clients.

Aanvraag en respons instanties zijn in het besturingskanaal beperkt tot maximaal 64 kB groot. De metagegevens van de HTTP-header is beperkt tot een totaal van 32 kB. Als de aanvraag of het antwoord is groter dan deze drempelwaarde, de listener moet upgraden naar een rendezvous WebSocket met behulp van een beweging die gelijk is aan de verwerking van de [accepteren](#accept-message).

Voor aanvragen besluit de service of voor het routeren van aanvragen via het besturingskanaal. Dit omvat, maar kan niet worden beperkt tot gevallen wanneer een aanvraag groter is dan 64 kB (headers en hoofdtekst) aanschaffen, of als de aanvraag wordt verzonden met ["chunked' transfer-encoding](https://tools.ietf.org/html/rfc7230#section-4.1) en de service heeft een reden die u kunt verwachten bij de aanvraag voor groter zijn dan 64 kB of lezen van de aanvraag is niet onmiddellijk. Als de service kiest voor het leveren van de aanvraag via rendezvous, geeft deze alleen het rendezvous-adres aan de listener.
De listener vervolgens de WebSocket-ontmoeting moet maken en de service direct biedt de volledige aanvraag instanties inclusief via de ontmoeting WebSocket. Het antwoord moet ook gebruiken voor de ontmoeting WebSocket.

Voor aanvragen die via het besturingskanaal binnenkomen, besluit de listener of om te reageren via het besturingskanaal of via rendezvous. De service omvat een rendezvous-adres met elke aanvraag gerouteerd via het besturingskanaal. Dit adres is alleen geldig voor het upgraden van de huidige aanvraag.

Als de listener ervoor kiest om bij te werken, verbinding maakt en direct biedt het antwoord via de tot stand gebrachte rendezvous-socket.

Zodra de ontmoeting die websocket tot stand is gebracht, de listener moet onderhouden voor verdere verwerking van aanvragen en antwoorden van dezelfde client. De service onderhoudt de WebSocket voor, zolang de HTTPS-socketverbinding met de afzender zich blijft voordoen en alle volgende aanvragen van de afzender wordt gerouteerd via het WebSocket wordt beheerd. Als de listener voor de ontmoeting WebSocket niet verwijderen uit de zijde, wordt de verbinding met de afzender, ongeacht of een volgende aanvraag al uitgevoerd wordt mogelijk ook verwijderen door de service.

#### <a name="renew-operation"></a>Bewerking

Terwijl de listener actief is het mogelijk dat het beveiligingstoken dat moet worden gebruikt voor het registreren van de listener en onderhouden van het besturingskanaal verloopt. De vervaldatum van token heeft geen invloed op actieve verbindingen, maar dit leidt tot het besturingskanaal worden verwijderd door de service op of kort na het moment van de vervaldatum. De bewerking 'vernieuwen' is een JSON-bericht dat de listener verzenden kunt naar het token dat is gekoppeld aan het besturingskanaal vervangen, zodat het besturingskanaal gedurende langere perioden kan worden beheerd.

#### <a name="ping-operation"></a>Ping-bewerking

Als het besturingskanaal niet-actieve gedurende een lange periode schakels op de manier blijft zoals load kunnen balancers of NAT-apparaten verwijderen de TCP-verbinding. De bewerking 'pingen' voorkomt dat door het verzenden van een kleine hoeveelheid gegevens op het kanaal dat u eraan herinnert dat iedereen op de netwerkroute die de verbinding is bedoeld om te worden actief en omgeving doet ook dienst als een 'live' test voor de listener. Als het pingen mislukt, wordt het besturingskanaal moet worden overwogen onbruikbaar en de listener opnieuw verbinding te maken.

### <a name="sender-interaction"></a>Tussenkomst van de afzender

De afzender heeft twee interacties met de service: een Web Socket verbinding wordt gemaakt of deze verzendt aanvragen via HTTPS. Aanvragen kunnen niet worden verzonden via een Web Socket van de rol van de afzender.

#### <a name="connect-operation"></a>Verbindingsbewerking

De bewerking 'connect' opent een WebSocket op de service, die de naam van de hybride verbinding en (optioneel, maar vereist standaard) een security token verleent machtigingen voor "Verzenden" in de querytekenreeks. De service communiceert vervolgens met de listener op de manier die eerder zijn beschreven, en de listener maakt een rendezvous-verbinding die wordt samengevoegd met dit WebSocket. Nadat de WebSocket heeft geaccepteerd, wordt alle verdere interactie op die WebSocket met de listener van een verbonden zijn.

#### <a name="request-operation"></a>Aanvraagbewerking

Voor hybride verbindingen waarvoor de functie is ingeschakeld, kan de afzender grotendeels onbeperkte HTTP-aanvragen verzenden naar de listeners.

Met uitzondering van een Relay-toegangstoken dat is dat een ingesloten in de querytekenreeks of in een HTTP-header van de aanvraag, is de Relay volledig transparant voor alle HTTP-bewerkingen op de Relay-adres en alle achtervoegsels van het pad van het adres Relay, waardoor de listener volledig beheer van en d-to-end-autorisatie en zelfs HTTP-extensie-functies, zoals [CORS](https://www.w3.org/TR/cors/).

Afzender autorisatie met de Relay-eindpunt is standaard ingeschakeld, maar is optioneel. Anonieme afzenders kan de eigenaar van de hybride verbinding kiezen. De service wordt onderschept, controleren en autorisatie-informatie als volgt van strook /:

1. Als de query-tekenreeks bevat een `sb-hc-token` -expressie, maar de expressie wordt altijd worden verwijderd uit de query-tekenreeks. Wordt het apparaat geëvalueerd als Relay autorisatie is ingeschakeld.
2. Als de aanvraagheaders bevatten een `ServiceBusAuthorization` koptekst, de header expressie altijd worden verwijderd uit de kop-verzameling.
   Wordt het apparaat geëvalueerd als Relay autorisatie is ingeschakeld.
3. Alleen als de Relay-autorisatie is ingeschakeld, en als de aanvraagheaders bevatten een `Authorization` koptekst en geen van de voorafgaande expressies aanwezig is, wordt de koptekst van worden geëvalueerd en verwijderd. Anders wordt de `Authorization`altijd wordt doorgegeven als-is.

Als er geen actieve listener, wordt de service een 502 'Ongeldige Gateway'-foutcode geretourneerd. Als de service niet wordt weergegeven voor het afhandelen van de aanvraag, wordt de service een 504 "time-out van Gateway' geretourneerd na 60 seconden.

### <a name="interaction-summary"></a>Tussenkomst van de samenvatting

Het resultaat van dit model interactie is dat de afzender-client wordt geleverd buiten de handshake met een 'schone' WebSocket, die is verbonden met een listener en die geen verdere preambles of voorbereiding nodig. Dit model kunt vrijwel alle bestaande WebSocket-clientimplementatie om gemakkelijk te profiteren van de Hybrid Connections-service door het opgeven van een correct opgemaakte URL in de laag van de WebSocket-client.

De verbinding rendezvous WebSocket die de listener wordt opgehaald via de interactie accepteren ook schoon is en kan worden overgedragen aan de implementatie van een bestaande WebSocket-server met een minimale extra abstractie die wordt onderscheid tussen bewerkingen op 'accept gemaakt' van hun framework lokale netwerk listeners en hybride verbindingen van externe accepteren' ' bewerkingen.

Het HTTP-aanvraag/antwoord-model biedt de afzender een grotendeels onbeperkte HTTP-protocol-gebied met een optionele autorisatie-laag. De listener van een vooraf geparseerde HTTP-aanvraag sectie header die kan worden uitgeschakeld in een downstream HTTP-aanvraag of verwerkt als de uitvoering is, met binaire frames HTTP-instanties opgehaald. Antwoorden op de dezelfde indeling gebruiken. Interactie met minder dan 64 KB aan de hoofdtekst van de aanvraag en antwoord kunnen worden verwerkt via een enkele Web Socket die is gedeeld voor alle afzenders te maken. Grotere aanvragen en antwoorden kunnen worden verwerkt met behulp van het rendezvous-model.

## <a name="protocol-reference"></a>Naslaginformatie over het protocol

Deze sectie beschrijft de details van het protocol interacties die eerder zijn beschreven.

Alle WebSocket-verbindingen worden gemaakt op poort 443 als een upgrade van HTTPS 1.1, waarvan wordt vaak door sommige WebSocket-framework of API geabstraheerd. Hier de beschrijving wordt opgeslagen implementatie neutraal, zonder dat een specifiek framework voorstellen.

### <a name="listener-protocol"></a>Listener-protocol

Het protocol listener bestaat uit twee verbindingen gebaren en drie berichtbewerkingen.

#### <a name="listener-control-channel-connection"></a>Listener-kanaalverbinding

Het besturingskanaal is geopend met het maken van een WebSocket-verbinding met:

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

De `namespace-address` is de volledig gekwalificeerde domeinnaam van de Azure Relay-naamruimte die als host fungeert voor de hybride verbinding, meestal van het formulier `{myname}.servicebus.windows.net`.

De opties voor queryreeksen parameter zijn als volgt.

| Parameter        | Vereist | Beschrijving
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | Ja      | Voor de listener-rol, de parameter moet **sb-hc-action = luisteren**
| `{path}`         | Ja      | Het naamruimtepad van de URL-gecodeerde van de vooraf geconfigureerde hybride verbinding met het registreren van deze listener op. Deze expressie wordt toegevoegd aan de vaste `$hc/` padgedeelte.
| `sb-hc-token`    | Ja\*    | De listener van een geldige, door de URL-gecodeerde Service Bus gedeeld toegangstoken moet opgeven voor de naamruimte of een hybride verbinding biedt de **luisteren** rechts.
| `sb-hc-id`       | Nee       | Deze client wordt geleverd optionele ID kunt diagnostische tracering van end-to-end.

Als de WebSocket-verbinding is mislukt vanwege de hybride verbinding pad niet wordt geregistreerd, of een ongeldige of ontbrekende token of een andere fout, wordt de fout feedback volgen met behulp van de reguliere gesegmenteerde HTTP 1.1-status feedback model. De statusbeschrijving van de bevat een fout tracerings-id die kan worden doorgegeven aan Azure-ondersteuningspersoneel:

| Code | Fout          | Beschrijving
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Niet gevonden      | Het pad van de hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling.
| 401  | Niet geautoriseerd   | Het beveiligingstoken is een ontbrekend of onjuist gevormd of ongeldig.
| 403  | Verboden      | Het beveiligingstoken is niet geldig voor dit pad voor deze actie.
| 500  | Interne fout | Er is iets misgegaan in de service.

Als de WebSocket-verbinding opzettelijk door de service afgesloten wordt nadat deze is in eerste instantie hebt ingesteld, de reden voor doet, wordt doorgegeven met behulp van een juiste WebSocket-protocol foutcode samen met een beschrijvend foutbericht dat ook een tracerings-ID bevat. De service wordt niet afgesloten het besturingskanaal zonder dat er een fout optreedt. Een schone afsluiten is beheerd-client.

| WS-Status | Beschrijving
| --------- | -------------------------------------------------------------------------------
| 1001      | Het pad van de hybride verbinding is verwijderd of uitgeschakeld.
| 1008      | Het beveiligingstoken is verlopen, dus het autorisatiebeleid wordt geschonden.
| 1011      | Er is iets misgegaan in de service.

#### <a name="accept-handshake"></a>Handshake accepteren

De melding 'accepteren' worden verzonden door de service voor de listener via het eerder tot stand gebrachte besturingskanaal als een JSON-bericht in een frame WebSocket. Er is geen antwoord op dit bericht.

Het bericht bevat een JSON-object die met de naam 'accepteren', waarin de volgende eigenschappen op dit moment zijn gedefinieerd:

* **adres** : de URL-tekenreeks moet worden gebruikt voor het tot stand brengen van de WebSocket naar de service een binnenkomende verbinding accepteren.
* **id** : de unieke id voor deze verbinding. Als de ID is opgegeven door de client van de afzender, is dit de afzender opgegeven waarde, anders is de waarde van een systeem gegenereerd.
* **connectHeaders** – alle HTTP-headers die naar de Relay-eindpunt zijn opgegeven door de afzender, waaronder ook de seconde-WebSocket-Protocol en de seconde-WebSocket-extensies-headers.

```json
{
    "accept" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?..."
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",
        "connectHeaders" : {
            "Host" : "...",
            "Sec-WebSocket-Protocol" : "...",
            "Sec-WebSocket-Extensions" : "..."
        }
     }
}
```

De adres-URL die is opgegeven in het JSON-bericht wordt gebruikt door de listener tot stand brengen van de WebSocket voor het accepteren of weigeren van de afzender-socket.

##### <a name="accepting-the-socket"></a>Acceptatie van de socket

Als u wilt accepteren, de listener van een verbinding tot stand WebSocket naar het opgegeven adres.

Als het bericht 'accepteren' voert een `Sec-WebSocket-Protocol` header, is het waarschijnlijk dat de listener alleen de WebSocket accepteert als het protocol ondersteunt. Bovendien wordt de header ingesteld als de WebSocket tot stand is gebracht.

Dit geldt ook voor de `Sec-WebSocket-Extensions` header. Als het framework een extensie ondersteunt, moet deze de header ingesteld op de server-side-antwoord van de vereiste `Sec-WebSocket-Extensions` handshake voor de extensie.

De URL moet worden gebruikt als-is voor het tot stand brengen van de socket accepteren, maar bevat de volgende parameters:

| Parameter      | Vereist | Beschrijving
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Ja      | Voor het accepteren van een socket, moet de parameter worden `sb-hc-action=accept`
| `{path}`       | Ja      | (Zie het volgende lid)
| `sb-hc-id`     | Nee       | Zie de vorige beschrijving van **id**.

`{path}` is het naamruimtepad van het URL-gecodeerde van de vooraf geconfigureerde hybride verbinding waarop deze listener registreren. Deze expressie wordt toegevoegd aan de vaste `$hc/` padgedeelte.

De `path` expressie kan worden uitgebreid met een achtervoegsel en een query-tekenreeksexpressie die volgt op de naam van de geregistreerde na een bijbehorende slash.
Hierdoor kan de client afzender verzending van de argumenten doorgeven aan de listener van accepteren wanneer het is niet mogelijk om op te nemen van HTTP-headers. De verwachting is dat de listener-framework het vaste padgedeelte en de geregistreerde naam van het pad parseert en de resterende mogelijk zonder een query tekenreeksargumenten voorafgegaan maakt door `sb-`, beschikbaar voor de toepassing om te beslissen of u de verbinding accepteren.

Zie de volgende sectie van de 'Afzender Protocol' voor meer informatie.

Als er een fout is, kunt de service als volgt beantwoorden:

| Code | Fout          | Beschrijving
| ---- | -------------- | -----------------------------------
| 403  | Verboden      | De URL is ongeldig.
| 500  | Interne fout | Er is iets misgegaan in de service

 Nadat de verbinding is tot stand is gebracht, de server wordt afgesloten WebSocket wanneer de afzender WebSocket wordt afgesloten omlaag of met de volgende status:

| WS-Status | Beschrijving                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | De afzender-client de verbinding wordt afgesloten.                                    |
| 1001      | Het pad van de hybride verbinding is verwijderd of uitgeschakeld.                        |
| 1008      | Het beveiligingstoken is verlopen, dus het autorisatiebeleid wordt geschonden. |
| 1011      | Er is iets misgegaan in de service.                                            |

##### <a name="rejecting-the-socket"></a>De socket weigeren

 Verwijderen van de socket na het inspecteren van de `accept` bericht vereist een vergelijkbare handshake, zodat de statuscode en de statusbeschrijving communicatie van de reden voor de afwijzing kan stromen terug naar de afzender.

 Dit protocol ontwerp is gekozen hier is het gebruik van een WebSocket-handshake (die is ontworpen om te eindigen met een gedefinieerde foutstatus) zodat listener-clientimplementaties kunt blijven afhankelijk zijn van een client WebSocket en niet hoeft te gebruiken van een extra, bare HTTP-client.

 Voor het weigeren van de socket, de client wordt verricht de URI-adres van de `accept` bericht- en twee queryreeksparameters als volgt, toegevoegd:

| Param                   | Vereist | Beschrijving                              |
| ----------------------- | -------- | ---------------------------------------- |
| SB-hc-statusCode        | Ja      | Numerieke HTTP-statuscode.                |
| SB-hc-statusbeschrijving | Ja      | Mens leesbaar reden voor de afwijzing. |

De resulterende URI wordt vervolgens gebruikt om een WebSocket-verbinding te maken.

Als het goed is voltooid, wordt deze handshake opzettelijk mislukt met een HTTP-foutcode 410, omdat er geen WebSocket tot stand is gebracht. Als er iets misgaat, wordt de fout in de volgende codes beschreven:

| Code | Fout          | Beschrijving                          |
| ---- | -------------- | ------------------------------------ |
| 403  | Verboden      | De URL is ongeldig.                |
| 500  | Interne fout | Er is iets misgegaan in de service. |

#### <a name="request-message"></a>Aanvraagbericht

De `request` bericht wordt verzonden door de service voor de listener via het besturingskanaal. Hetzelfde bericht worden ook verzonden via de ontmoeting WebSocket eenmaal tot stand gebracht.

De `request` bestaat uit twee delen: een koptekst en binaire hoofdtekst frame (s).
Als er geen instantie is, wordt de hoofdtekst van de frames worden weggelaten. De indicator voor het of er een instantie aanwezig is, is de Booleaanse waarde `body` eigenschap in de aanvraag.

Voor een aanvraag met een aanvraagtekst, de structuur ziet er als volgt:

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : true,
        ...
    }
}
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame -FIN
FEFEFEFEFEFEFEFEFEFEF...
----------------------------------
```

De listener moet omgaan met het ontvangen van de hoofdtekst van de aanvraag verdeeld over meerdere frames van de binaire (Zie [WebSocket-fragmenten](https://tools.ietf.org/html/rfc6455#section-5.4)).
De aanvraag wordt beëindigd wanneer een binaire kader met de vlag is ingesteld FIN is ontvangen.

Er is slechts één tekstframe voor een aanvraag zonder een tekst.

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : false,
        ...
    }
}
----------------------------------
```

De JSON-inhoud voor `request` is als volgt:

* **adres** -URI-tekenreeks. Dit is het rendezvous-adres moet worden gebruikt voor deze aanvraag. Als de inkomende aanvraag groter dan 64 kB is, wordt de rest van dit bericht is leeg en de client moet een gelijk aan het rendezvous-handshake opnieuw de `accept` bewerking die hieronder worden beschreven. De service wordt vervolgens de volledige samengesteld `request` op de tot stand gebrachte Web socket. Als het antwoord kan worden verwacht groter zijn dan 64 kB, moet de listener ook tot stand brengen van een handshake rendezvous en zet vervolgens het antwoord via de tot stand gebrachte Web socket.
* **id** -tekenreeks. De unieke id voor deze aanvraag.
* **requestHeaders** – dit object bevat alle HTTP-headers die naar het eindpunt is opgegeven door de afzender, met uitzondering van de autorisatie-informatie, zoals wordt beschreven [hierboven](#request-operation), en -koppen die uitsluitend betrekking hebben op de verbinding met de gateway. Specifiek, alle headers gedefinieerd of gereserveerd in [RFC7230](https://tools.ietf.org/html/rfc7230), met uitzondering van `Via`, worden verwijderd en niet doorgestuurd:

  * `Connection` (RFC7230, sectie 6.1)
  * `Content-Length`  (RFC7230, punt 3.3.2)
  * `Host`  (RFC7230, sectie 5.4)
  * `TE`  (RFC7230, sectie 4.3)
  * `Trailer`  (RFC7230, sectie 4.4)
  * `Transfer-Encoding`  (RFC7230, sectie 3.3.1)
  * `Upgrade` (RFC7230, sectie 6.7)
  * `Close`  (RFC7230, sectie 8.1)

* **requestTarget** -tekenreeks. Deze eigenschap bevat de ['Doel van de aanvraag' (RFC7230, sectie 5.3)](https://tools.ietf.org/html/rfc7230#section-5.3) van de aanvraag. Dit omvat het gedeelte van de tekenreeks query, die wordt verwijderd van alle `sb-hc-` voorafgegaan parameters.
* **methode** -tekenreeks. Dit is de methode van de aanvraag [RFC7231, sectie 4](https://tools.ietf.org/html/rfc7231#section-4). De `CONNECT` methode mogen niet worden gebruikt.
* **hoofdtekst** – Boolean-waarde. Geeft aan of volgt op een of meer binaire hoofdtekst frame.

``` JSON
{
    "request" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?...",
        "id" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "requestTarget" : "/abc/def?myarg=value&otherarg=...",
        "method" : "GET",
        "requestHeaders" : {
            "Host" : "...",
            "Content-Type" : "...",
            "User-Agent" : "..."
        },
        "body" : true
     }
}
```

##### <a name="responding-to-requests"></a>Reageren op aanvragen

De ontvanger moet reageren. Herhaalde fouten te reageren op aanvragen behoud van de verbinding kan leiden tot de listener ophalen op de blokkeringslijst.

Antwoorden kunnen in willekeurige volgorde worden verzonden, maar elke aanvraag moet worden gereageerd op binnen 60 seconden of de levering worden gerapporteerd als dat is mislukt. De deadline 60 seconden wordt geteld totdat de `response` frame is ontvangen door de service. Een continue-antwoord met meerdere frames van de binaire gedurende meer dan 60 seconden kan niet worden niet-actieve of deze wordt beëindigd.

Als de aanvraag wordt ontvangen via het besturingskanaal, het antwoord ofwel in het besturingskanaal moet worden verzonden vanuit waar de aanvraag is ontvangen, of deze moet worden verzonden via een rendezvous-kanaal.

Het antwoord is een JSON-object met de naam 'response'. De regels voor het verwerken van de inhoud van de hoofdtekst zijn precies hetzelfde als met de `request` bericht- en op basis van de `body` eigenschap.

* **requestId** -tekenreeks. VEREIST. De `id` eigenschapswaarde van de `request` bericht wordt beantwoord.
* **statusCode** : nummer. VEREIST. een numerieke HTTP-statuscode die het resultaat van het bericht aangeeft. Alle statuscodes van [RFC7231, sectie 6](https://tools.ietf.org/html/rfc7231#section-6) zijn toegestaan, met uitzondering van [502 'Ongeldige Gateway'](https://tools.ietf.org/html/rfc7231#section-6.6.3) en [504 "time-out van Gateway'](https://tools.ietf.org/html/rfc7231#section-6.6.5).
* **Statusbeschrijving** -tekenreeks. OPTIONEEL. HTTP-statuscode reden per [RFC7230, sectie 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2)
* **responseHeaders** – HTTP-headers zijn ingesteld in een externe HTTP-antwoord.
  Net als bij de `request`, RFC7230 gedefinieerde headers mogen niet worden gebruikt.
* **hoofdtekst** – Boolean-waarde. Geeft aan of de hoofdtekst van de binaire frames follow(s).

``` text
----- Web Socket text frame ----
{
    "response" : {
        "requestId" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "statusCode" : "200",
        "responseHeaders" : {
            "Content-Type" : "application/json",
            "Content-Encoding" : "gzip"
        }
         "body" : true
     }
}
----- Web Socket binary frame -FIN
{ "hey" : "mydata" }
----------------------------------
```

##### <a name="responding-via-rendezvous"></a>Reageren via rendezvous

Voor antwoorden die groter zijn dan 64 kB, moet het antwoord via een socket rendezvous worden geleverd. Ook als de aanvraag is groter dan 64 kB, en de `request` bevat alleen het adresveld, een socket rendezvous moet worden gemaakt met het verkrijgen van de `request`. Zodra een socket rendezvous eenmaal is ingesteld, moeten antwoorden op de desbetreffende client en de volgende aanvragen van die respectieve client worden geleverd via de socket rendezvous terwijl het probleem blijft bestaan.

De `address` in de URL van de `request` moeten worden gebruikt als-is voor het tot stand brengen van de rendezvous-socket, maar bevat de volgende parameters:

| Parameter      | Vereist | Beschrijving
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Ja      | Voor het accepteren van een socket, moet de parameter worden `sb-hc-action=request`

Als er een fout is, kunt de service als volgt beantwoorden:

| Code | Fout           | Beschrijving
| ---- | --------------- | -----------------------------------
| 400  | De aanvraag is ongeldig | Niet-herkende actie of de URL is ongeldig.
| 403  | Verboden       | De URL is verlopen.
| 500  | Interne fout  | Er is iets misgegaan in de service

 Nadat de verbinding is tot stand is gebracht, de server wordt afgesloten WebSocket wanneer HTTP-socket van de client wordt afgesloten, of met de volgende status:

| WS-Status | Beschrijving                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | De afzender-client de verbinding wordt afgesloten.                                    |
| 1001      | Het pad van de hybride verbinding is verwijderd of uitgeschakeld.                        |
| 1008      | Het beveiligingstoken is verlopen, dus het autorisatiebeleid wordt geschonden. |
| 1011      | Er is iets misgegaan in de service.                                            |


#### <a name="listener-token-renewal"></a>Listener token vernieuwen

Wanneer de listener-token bijna verlopen is, kan het deze vervangen door een frame tekstbericht verzenden naar de service via het tot stand gebrachte besturingskanaal. Het bericht bevat een JSON-object met de naam `renewToken`, die op dit moment wordt de volgende eigenschap gedefinieerd:

* **token** – een geldige, door de URL-gecodeerde gedeelde toegang van Service Bus-token voor de naamruimte of een hybride verbinding biedt de **luisteren** rechts.

```json
{
  "renewToken": {
    "token":
      "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
  }
}
```

Als de validatie van het token is mislukt, de toegang is geweigerd en de cloudservice wordt gesloten voor het besturingskanaal WebSocket met een fout. Anders wordt is er geen antwoord ontvangen.

| WS-Status | Beschrijving                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1008      | Het beveiligingstoken is verlopen, dus het autorisatiebeleid wordt geschonden. |

### <a name="web-socket-connect-protocol"></a>Web Socket protocol verbinding maken

De afzender-protocol is in feite gelijk is aan de manier waarop die een listener tot stand is gebracht.
Het doel is de maximale transparantie voor de end-to-end-WebSocket. Het adres verbinding maken met is dezelfde als die voor de listener, maar verschilt van de "action" en het token moet een andere machtiging:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

De _naamruimte-address_ is de volledig gekwalificeerde domeinnaam van de Azure Relay-naamruimte die als host fungeert voor de hybride verbinding, meestal van het formulier `{myname}.servicebus.windows.net`.

De aanvraag kan willekeurige extra HTTP-headers, met inbegrip van de toepassing gedefinieerde waarden bevatten. Alle opgegeven headers overgebracht naar de listener en kunt u vinden op de `connectHeader` object van de **accepteren** controlebericht.

De opties voor queryreeksen parameter zijn als volgt:

| Param          | Vereist? | Beschrijving
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | Ja       | Voor de rol van de afzender, de parameter moet `sb-hc-action=connect`.
| `{path}`       | Ja       | (Zie het volgende lid)
| `sb-hc-token`  | Ja\*     | De listener van een geldige, door de URL-gecodeerde Service Bus gedeeld toegangstoken moet opgeven voor de naamruimte of een hybride verbinding biedt de **verzenden** rechts.
| `sb-hc-id`     | Nee        | Een optionele ID waarmee de end-to-end diagnostische tracering geactiveerd en wordt beschikbaar gesteld aan de listener tijdens de handshake accepteren.

 De `{path}` is het naamruimtepad van de URL-gecodeerde van de vooraf geconfigureerde hybride verbinding waarop deze listener registreren. De `path` expressie met een achtervoegsel en een query-tekenreeks-expressie om te communiceren verder kan worden uitgebreid. Als de hybride verbinding is geregistreerd onder het pad `hyco`, wordt de `path` expressie mag `hyco/suffix?param=value&...` gevolgd door de queryreeksparameters die hier zijn gedefinieerd. Een volledige expressie kan vervolgens als volgt zijn:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

De `path` expressie wordt doorgegeven aan de listener in de adres-URI die is opgenomen in het controlebericht 'accepteren'.

Als de WebSocket-verbinding is mislukt vanwege de hybride verbinding pad niet wordt geregistreerd, een ongeldige of ontbrekende token of een andere fout, wordt de fout feedback volgen met behulp van de reguliere gesegmenteerde HTTP 1.1-status feedback model. De statusbeschrijving van de bevat een fout tracerings-ID die kan worden doorgegeven aan Azure-ondersteuningspersoneel:

| Code | Fout          | Beschrijving
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Niet gevonden      | Het pad van de hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling.
| 401  | Niet geautoriseerd   | Het beveiligingstoken is een ontbrekend of onjuist gevormd of ongeldig.
| 403  | Verboden      | Het beveiligingstoken is niet geldig voor dit pad en voor deze actie.
| 500  | Interne fout | Er is iets misgegaan in de service.

Als de WebSocket-verbinding opzettelijk door de service afgesloten wordt nadat deze is in eerste instantie ingesteld, de reden voor doet, wordt doorgegeven met behulp van een juiste WebSocket-protocol foutcode samen met een beschrijvend foutbericht dat ook een tracerings-ID bevat .

| WS-Status | Beschrijving
| --------- | ------------------------------------------------------------------------------- 
| 1000      | De listener is afgesloten van de socket.
| 1001      | Het pad van de hybride verbinding is verwijderd of uitgeschakeld.
| 1008      | Het beveiligingstoken is verlopen, dus het autorisatiebeleid wordt geschonden.
| 1011      | Er is iets misgegaan in de service.

### <a name="http-request-protocol"></a>HTTP-aanvraagprotocol

HTTP-aanvraag voor dit protocol kunnen willekeurige HTTP-aanvragen, met uitzondering van protocol upgrades.
HTTP-aanvragen zijn waarnaar wordt verwezen op van de entiteit reguliere runtime-adres, zonder de $hc infix die wordt gebruikt voor hybride verbindingen van clients voor WebSocket.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

De _naamruimte-address_ is de volledig gekwalificeerde domeinnaam van de Azure Relay-naamruimte die als host fungeert voor de hybride verbinding, meestal van het formulier `{myname}.servicebus.windows.net`.

De aanvraag kan willekeurige extra HTTP-headers, met inbegrip van de toepassing gedefinieerde waarden bevatten. Alle headers, met uitzondering van die rechtstreeks zijn gedefinieerd in RFC7230 geleverd (Zie [aanvraagbericht](#Request message)) overgebracht naar de listener en kunt u vinden op de `requestHeader` object van de **aanvraag** bericht.

De opties voor queryreeksen parameter zijn als volgt:

| Param          | Vereist? | Beschrijving
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | Ja\*     | De listener van een geldige, door de URL-gecodeerde Service Bus gedeeld toegangstoken moet opgeven voor de naamruimte of een hybride verbinding biedt de **verzenden** rechts.

Het token kan ook worden uitgevoerd in ofwel de `ServiceBusAuthorization` of `Authorization` HTTP-header. Het token kan worden weggelaten als de hybride verbinding is geconfigureerd voor het toestaan van anonieme aanvragen.

Omdat de service daadwerkelijk als een proxy, fungeert zelfs als niet als een ' True ' HTTP-proxy, ofwel wordt toegevoegd een `Via` header of de bestaande annotates `Via` header die compatibel zijn met [RFC7230, sectie 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1).
De service wordt de hostnaam van de Relay-naamruimte aan `Via`.

| Code | Bericht  | Beschrijving                    |
| ---- | -------- | ------------------------------ |
| 200  | OK       | De aanvraag is verwerkt door ten minste één listener.  |
| 202  | Geaccepteerd | De aanvraag is goedgekeurd door ten minste één listener. |

Als er een fout is, kunt de service als volgt beantwoorden. Of het antwoord afkomstig is van de service of van de listener kan worden geïdentificeerd door de aanwezigheid van de `Via` header. Als de header aanwezig is, is het antwoord van de listener.

| Code | Fout           | Beschrijving
| ---- | --------------- |--------- |
| 404  | Niet gevonden       | Het pad van de hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling.
| 401  | Niet geautoriseerd    | Het beveiligingstoken is een ontbrekend of onjuist gevormd of ongeldig.
| 403  | Verboden       | Het beveiligingstoken is niet geldig voor dit pad en voor deze actie.
| 500  | Interne fout  | Er is iets misgegaan in de service.
| 503  | Ongeldige gateway     | De aanvraag kan niet worden doorgestuurd naar een listener.
| 504  | Time-out voor gateway | De aanvraag is doorgestuurd naar een listener, maar de listener erkent ontvangst in de tijd die nodig is.

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)
