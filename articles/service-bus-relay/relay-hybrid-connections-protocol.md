---
title: Azure Relay hybride verbindingen protocol handleiding | Microsoft Docs
description: Handleiding voor Azure Relay hybride verbindingen-protocol.
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
ms.openlocfilehash: 306a21add76261dce99c954a2ba373e4b5047a75
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Protocol voor Azure Relay hybride verbindingen

Azure Relay is een van de belangrijkste mogelijkheden stijlen van het Azure Service Bus-platform. De nieuwe _hybride verbindingen_ mogelijkheid relay is een op basis van HTTP- en WebSockets evolutie van beveiligde, open-protocol. Het eerste evenredig met de naam die hij vervangt _BizTalk Services_ functie die is gebaseerd op een eigen protocol foundation. De integratie van hybride verbindingen in Azure App Services, blijven functioneren als-is.

Hybride verbindingen kunt u communicatie in twee richtingen, binaire gegevensstroom en eenvoudige datagram stroom tussen twee netwerktoepassingen. Een of beide partijen kunnen zich achter een NAT's en firewalls.

In dit artikel beschrijft de clientzijde interactie met de relay hybride verbindingen voor het verbinden van clients in listener en afzender rollen. Ook wordt beschreven hoe listeners aanvragen en nieuwe verbindingen accepteren.

## <a name="interaction-model"></a>Interactie model

De relay hybride verbindingen verbindt twee partijen door een rendezvous-punt in de Azure-cloud die partijen kunnen detecteren en verbinding maken met vanaf hun eigen netwerk perspectief te bieden. Het rendezvous-punt wordt 'Hybride verbinding' in dit document en andere documentatie genoemd in de API's en in de Azure portal. Het service-eindpunt voor hybride verbindingen aangeduid als 'service' voor de rest van dit artikel.

De service kunnen voor het doorgeven van Web-Socket-verbindingen en HTTP (S)-aanvragen en -antwoorden van.

Het model interactie leans op de naamgeving volgt de namen tot stand gebracht door veel andere netwerk-API's. Er is een listener die eerst duidt gereedheid voor het afhandelen van binnenkomende verbindingen en vervolgens accepteert naarmate ze binnenkomen. Een client verbinding maakt op de andere kant naar de listener, die verbinding kunnen worden geaccepteerd voor het tot stand brengen van een bidirectionele communicatiepad verwacht. 'Verbinding', 'Luisteren' en 'Accepteren' zijn dezelfde voorwaarden u in de meeste socket API's vindt.

Een communicatiemodel relayed heeft beide partijen maken van uitgaande verbindingen naar een service-eindpunt. Hierdoor wordt de 'listener' ook een 'client' gewone gebruikt, en mogelijk ook andere terminologie overloads. De precieze terminologie daarom gebruikt voor hybride verbindingen is als volgt:

De programma's aan beide zijden van een verbinding worden 'clients,' genoemd, omdat ze clients naar de service. De client die wordt gewacht op en aanvaardt verbindingen is de 'listener' of genoemd in de 'listener rol'. De client die een nieuwe verbinding naar een listener via de service start wordt de 'afzender' genoemd, of bevindt zich in de 'afzender rol'.

### <a name="listener-interactions"></a>Listener-interacties

De listener beschikt over vijf interactie met de service; alle gegevens van de kabel worden verderop in dit artikel in de sectie Verwijzingen beschreven.

De Listen accepteren en aanvraag berichten worden ontvangen van de service. Het vernieuwen en Ping-bewerkingen zijn verzonden door de listener.

#### <a name="listen-message"></a>Bericht luisteren

Om aan te geven van de gereedheid van de service die een listener is gereed om te accepteren, verbindingen, maakt het een uitgaande WebSocket-verbinding. De handshake van verbinding zijn voor de naam van een hybride verbinding geconfigureerd op de Relay-naamruimte en een beveiligingstoken dat rechts op de 'luisteren' verleent deze naam.

Wanneer de WebSocket is geaccepteerd door de service, de registratie is voltooid en de tot stand gebrachte WebSocket is in stand gehouden, als het 'besturingskanaal' voor het inschakelen van alle opeenvolgende interacties. De service kunnen maximaal 25 gelijktijdige listeners een hybride verbinding. Het quotum voor AppHooks is bepaald.

Voor hybride verbindingen, als er twee of meer actieve listeners worden binnenkomende verbindingen verdeeld zijn over ze in willekeurige volgorde; evenredige verdeling wordt geprobeerd met zo goed mogelijke poging.

#### <a name="accept-message"></a>Bericht accepteren

Wanneer een afzender een nieuwe verbinding op de service opent, wordt de service kiest en waarschuwt een van de actieve listeners op de hybride verbinding. Deze melding wordt verzonden naar de listener via het open besturingskanaal als een JSON-bericht. Het bericht bevat de URL van het WebSocket-eindpunt dat de listener verbinding maken moet met voor het accepteren van de verbinding.

De URL kan en moet rechtstreeks worden gebruikt door de listener zonder extra werk.
De gecodeerde gegevens is alleen geldig voor een korte periode, hoofdzakelijk voor zolang de afzender is bereid om te wachten op de verbinding tot stand gebrachte end-to-end. Het maximum wordt ervan uitgegaan dat is 30 seconden. De URL kan alleen worden gebruikt voor een geslaagde verbindingspoging. Zodra de WebSocket-verbinding met de rendezvous-URL is gemaakt, wordt alle verdere activiteiten op dit WebSocket doorgegeven van en naar de afzender. Dit gebeurt zonder tussenkomst van de of interpretatie door de service.

### <a name="request-message"></a>Aanvraagbericht

Naast het WebSocket-verbindingen, kan de listener ook ontvangen HTTP-aanvraag frames van een afzender als deze mogelijkheid expliciet is ingeschakeld op de hybride verbinding.

-Listeners die aan hybride verbindingen met HTTP-ondersteuning koppelen moeten verwerken de `request` gebaar. Listener kunnen niet worden verwerkt `request` en daarom herhaalde time-outfouten veroorzaakt tijdens het verbinding kan worden gebeurd door de service in de toekomst.

HTTP-frame header metagegevens wordt omgezet in JSON voor eenvoudigere verwerking door de listener-framework ook omdat HTTP-header parseren bibliotheken zeldzame dan JSON parsers zijn. HTTP-metagegevens die is alleen relevant voor de relatie tussen de afzender en de Relay-HTTP-gateway, inclusief de autorisatie-informatie is niet doorgestuurd. HTTP-aanvraag instanties worden transparant als binaire WebSocket-frames overgedragen.

De listener kan reageren op HTTP-aanvragen via een gebaar van de equivalente antwoord.

De aanvraag/antwoord-stroom maakt standaard gebruik van het besturingskanaal, maar kan worden 'bijgewerkt' naar een afzonderlijke rendezvous WebSocket wanneer vereist. Afzonderlijke WebSocket-verbindingen doorvoer voor elke client conversatie verbeteren, maar ze belasten de listener met meer verbindingen die worden behandeld moeten, die niet willen kunnen voor lichte clients mag zijn.

Aanvraag en -antwoord instanties zijn in het besturingskanaal beperkt tot maximaal 64 kB groot. HTTP-header metagegevens is beperkt tot een totaal van 32 kB. Als de aanvraag of antwoord groter is dan deze drempelwaarde, de listener moet upgraden naar een rendezvous-WebSocket met een gebaar van verwerking van de [accepteren](#accept-message).

Voor aanvragen besluit de service of om aanvragen te versturen via het besturingskanaal. Dit omvat, maar kan niet worden beperkt tot gevallen wanneer een aanvraag meer dan 64 kB (headers plus hoofdtekst) rechtstreekse, of als de aanvraag is verzonden met ['chunked' transfer-encoding](https://tools.ietf.org/html/rfc7230#section-4.1) en de service heeft reden worden verwacht voor de aanvraag meer dan 64 kB of lezen van de aanvraag is niet onmiddellijk. Als de service kiest voor het afleveren van de aanvraag via rendezvous, worden alleen het rendezvous-adres aan de listener doorgegeven.
De listener vervolgens de ontmoeting WebSocket moet maken en de service onmiddellijk de volledige aanvraag, met inbegrip van instellingen via de ontmoeting WebSocket te bezorgen. Het antwoord moet ook de ontmoeting WebSocket gebruiken.

Voor aanvragen die over het besturingskanaal binnenkomen, besluit de listener of via het besturingskanaal of via rendezvous reageren. De service moet bestaan uit een rendezvous-adres met elke aanvraag via het besturingskanaal doorgestuurd. Dit adres is alleen geldig voor het upgraden van de huidige aanvraag.

Als de listener ervoor kiest om bij te werken, verbinding maakt en onmiddellijk levert het antwoord via de tot stand gebrachte rendezvous-socket.

Eenmaal de ontmoeting die websocket tot stand is gebracht, de listener moet onderhouden voor verdere verwerking van aanvragen en antwoorden van dezelfde client. De service blijft de WebSocket voor zolang het HTTPS-verbinding met de afzender socket zich blijft voordoen en routeert alle volgende aanvragen van die afzender via de WebSocket handhaven. Als de listener ervoor kiest de ontmoeting WebSocket verwijderen uit de zijde, wordt de verbinding met de afzender, ongeacht of een volgende aanvraag al mogelijk uitgevoerd ook verwijderen door de service.

#### <a name="renew-operation"></a>Bewerking vernieuwen

Het beveiligingstoken dat moet worden gebruikt voor het registreren van de listener en onderhouden van het besturingskanaal mogelijk verlopen terwijl de listener actief is. Het toegangstoken heeft geen invloed op actieve verbindingen, maar dit leidt tot het besturingskanaal worden verwijderd door de service op of kort na het tijdstip van de geldigheidsduur. De bewerking 'vernieuwen' is een JSON-bericht dat de listener verzenden kunt ter vervanging van het token dat is gekoppeld aan het besturingskanaal zodat het besturingskanaal gedurende langere perioden kan worden beheerd.

#### <a name="ping-operation"></a>Ping-bewerking

Als het besturingskanaal inactief gedurende een lange periode schakels op de manier blijft zoals load balancers of NAT's mogelijk verwijderen de TCP-verbinding. De bewerking 'pingen' voorkomt dat door een kleine hoeveelheid gegevens op het kanaal dat iedereen in de netwerkroute die de verbinding is bedoeld als actief is en deze fungeert ook als een 'live' test voor de listener herinnert verzenden. Als het pingen mislukt, wordt het besturingskanaal moet worden beschouwd als onbruikbaar en de listener moet opnieuw worden verbonden.

### <a name="sender-interaction"></a>Interactie van de afzender

De afzender heeft twee interactie met de service: deze verbinding maakt met een Socket Web of deze verzendt aanvragen via HTTPS. Aanvragen kunnen niet worden verzonden via een Socket Web uit de rol van de afzender.

#### <a name="connect-operation"></a>Verbindingsbewerking

De bewerking "connect" Hiermee opent u een WebSocket op de service, de naam van de hybride verbinding en (optioneel, maar vereist standaard) bieden een beveiligingsbeleid voor token verleent machtigingen voor 'Verzenden' in de queryreeks. De service communiceert vervolgens met de listener op de manier die eerder zijn beschreven, en de listener maakt een rendezvous-verbinding die wordt samengevoegd met dit WebSocket. Nadat de WebSocket heeft geaccepteerd, worden alle verdere interacties op die WebSocket met een listener verbonden zijn.

#### <a name="request-operation"></a>Bewerking aanvragen

Voor hybride verbindingen waarvoor de functie is ingeschakeld, kan de afzender grotendeels onbeperkte HTTP-aanvragen verzenden naar listeners.

Met uitzondering van een toegangstoken Relay die een ingesloten in de querytekenreeks of in een HTTP-header van de aanvraag is is de Relay volledig transparant voor alle HTTP-bewerkingen op het adres van de Relay en alle achtervoegsels van het pad van het adres Relay, waardoor de listener volledig beheer van en d-to-end-autorisatie en zelfs uitbreiding van HTTP-functies zoals [CORS](https://www.w3.org/TR/cors/).

Afzender autorisatie met de Relay-eindpunt is standaard ingeschakeld, maar is optioneel. De eigenaar van de hybride verbinding kunt kiezen om anonieme afzenders. De service wordt onderschept, controleren en autorisatie-informatie als volgt van strook /:

1. Als de queryreeks bevat een `sb-hc-token` expressie de expressie wordt altijd worden verwijderd uit de queryreeks. Dit wordt geëvalueerd als de Relay-autorisatie is ingeschakeld.
2. Als de aanvraagheaders bevatten een `ServiceBusAuthorization` koptekst, de header expressie wordt altijd worden verwijderd uit de verzameling header.
   Dit wordt geëvalueerd als de Relay-autorisatie is ingeschakeld.
3. Als de Relay-autorisatie is ingeschakeld, en als de aanvraagheaders bevatten een `Authorization` header en geen van de voorafgaande expressies aanwezig is, de koptekst wordt geëvalueerd en verwijderd. Anders wordt de `Authorization`is altijd doorgegeven als-is.

Als er geen actieve listener, wordt de service een 502 'Ongeldige Gateway' foutcode geretourneerd. Als de service niet wordt weergegeven om de aanvraag te verwerken, wordt de service een 504 'Gateway timeout gegenereerd' geretourneerd na 60 seconden.

### <a name="interaction-summary"></a>Interactie van de samenvatting

Het resultaat van dit model interactie is dat de afzender client buiten de handshake met een 'schone' WebSocket, die is verbonden met een listener en dat er geen verdere preambles of voorbereiding moet afkomstig is. Dit model kunt vrijwel alle bestaande implementatie van de client WebSocket om direct te profiteren van de hybride verbindingen-service door het opgeven van een correct samengestelde URL in de laag WebSocket-client.

De verbinding rendezvous WebSocket die de listener via de interactie accepteren verkrijgt ook schoon is en kan worden verstrekt aan een bestaande implementatie van de server WebSocket met een minimale extra abstractie die wordt onderscheid tussen 'accepteren' bewerkingen op het lokale netwerk-listeners van hun framework en hybride verbindingen 'accepteren' externe bewerkingen gemaakt.

Het HTTP-aanvragen/reacties model biedt de afzender een grotendeels onbeperkte HTTP-protocol oppervlak met een laag optionele autorisatie. De listener haalt een vooraf geparseerde HTTP-aanvraag-headersectie die kan worden weer ingeschakeld in een downstream HTTP-aanvraag of verwerkt als de uitvoering is, met binaire frames instanties van HTTP. Antwoorden gebruiken dezelfde indeling. Interactie met minder dan 64 KB van aanvraag en -antwoord instantie kunnen worden verwerkt via één Web Socket die wordt gedeeld voor alle afzenders. Grotere aanvragen en antwoorden kunnen worden verwerkt met behulp van het rendezvous-model.

## <a name="protocol-reference"></a>Naslaginformatie over het protocol

Deze sectie beschrijft de details van het protocol interacties die eerder zijn beschreven.

Alle WebSocket-verbindingen worden uitgevoerd op poort 443 als een upgrade vanaf HTTPS 1.1, die meestal wordt gescheiden door een aantal WebSocket-framework of API. Hier de beschrijving wordt opgeslagen implementatie neutrale, zonder dat een specifiek framework voorstellen.

### <a name="listener-protocol"></a>Listener-protocol

De listener-protocol bestaat uit twee verbinding gebaren en drie berichtbewerkingen.

#### <a name="listener-control-channel-connection"></a>Listener-kanaal controleverbinding

Het besturingskanaal is geopend met een WebSocket-verbinding te maken:

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

De `namespace-address` is de volledig gekwalificeerde domeinnaam van de Azure-Relay-naamruimte die als host fungeert voor de hybride verbinding, doorgaans van het formulier `{myname}.servicebus.windows.net`.

De parameteropties voor query-tekenreeks zijn als volgt.

| Parameter        | Vereist | Beschrijving
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | Ja      | Voor de listener-rol, de parameter moet **sb-CH-action = luisteren**
| `{path}`         | Ja      | Het naamruimtepad van de URL-codering van de vooraf geconfigureerde hybride verbinding met het registreren van deze listener op. Deze expressie wordt toegevoegd aan de vaste `$hc/` padgedeelte te hebben.
| `sb-hc-token`    | Ja\*    | De listener moet een geldige, door de URL-codering Service Bus Shared Access Token opgeven voor de naamruimte of hybride verbinding die verleent de **luisteren** rechts.
| `sb-hc-id`       | Nee       | Deze client opgegeven optionele ID kunt diagnostische tracering end-to-end.

Als de WebSocket-verbinding is mislukt vanwege de hybride verbinding pad niet wordt geregistreerd, of een ongeldige of ontbrekende token of een andere fout, wordt de fout feedback opgegeven met het normale HTTP 1.1-status feedback model. De beschrijving van status bevat een fout tracerings-id die kan worden doorgegeven aan Azure ondersteuningspersoneel:

| Code | Fout          | Beschrijving
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Niet gevonden      | Het pad voor hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling.
| 401  | Niet geautoriseerd   | Het beveiligingstoken is ontbreken of onjuist gevormd of ongeldig.
| 403  | Verboden      | Het beveiligingstoken is niet geldig voor dit pad voor deze actie.
| 500  | Interne fout | Er is een fout in de service.

Als de WebSocket-verbinding opzettelijk door de service afgesloten wordt nadat deze is in eerste instantie hebt ingesteld, de reden voor dit wel doet wordt gecommuniceerd met behulp van een juiste WebSocket-protocol foutcode samen met een beschijvend foutbericht die ook een tracerings-ID. De service wordt niet afgesloten het besturingskanaal zonder dat er een fout opgetreden. Geldige afsluiting is de client worden beheerd.

| WS-Status | Beschrijving
| --------- | -------------------------------------------------------------------------------
| 1001      | Het pad voor hybride verbinding is verwijderd of uitgeschakeld.
| 1008      | Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden.
| 1011      | Er is een fout in de service.

#### <a name="accept-handshake"></a>Handshake accepteren

De melding 'accepteren' is verzonden door de service aan de listener via de eerder vastgestelde besturingskanaal als een JSON-bericht in een WebSocket frame. Er is geen antwoord op dit bericht.

Het bericht bevat een JSON-object die met de naam 'accepteren', waarmee de volgende eigenschappen zijn gedefinieerd op dit moment:

* **adres** : de URL-reeks moet worden gebruikt voor het tot stand brengen van de WebSocket met de service een binnenkomende verbinding accepteren.
* **id** – de unieke id voor deze verbinding. Als de ID is opgegeven door de afzender-client, is dit de afzender opgegeven waarde, anders is een waarde van het systeem gegenereerd.
* **connectHeaders** – alle HTTP-headers die door de afzender, waaronder ook Sec-WebSocket-Protocol en de seconde-WebSocket-extensies headers met de Relay-eindpunt worden verstrekt.

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

De adres-URL in het JSON-bericht wordt gebruikt door de listener tot stand brengen van de WebSocket voor het goedkeuren of weigeren van de afzender-socket.

##### <a name="accepting-the-socket"></a>Acceptatie van de socket

Als u wilt accepteren, stelt u de listener voor een WebSocket-verbinding naar het opgegeven adres.

Als het bericht 'accepteren' zijn voor een `Sec-WebSocket-Protocol` koptekst wordt verwacht dat de listener alleen de WebSocket accepteert als het protocol ondersteunt. Bovendien wordt de koptekst als de WebSocket tot stand is gebracht.

Hetzelfde geldt voor de `Sec-WebSocket-Extensions` header. Als het framework een extensie ondersteunt, moet deze de header ingesteld op een antwoord voor de serverzijde van de vereiste `Sec-WebSocket-Extensions` handshake voor de extensie.

De URL moet worden gebruikt als-is voor het tot stand brengen van de socket accepteren, maar bevat de volgende parameters:

| Parameter      | Vereist | Beschrijving
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Ja      | Voor het accepteren van een socket, moet de parameter worden `sb-hc-action=accept`
| `{path}`       | Ja      | (Zie het volgende lid)
| `sb-hc-id`     | Nee       | Zie vorige beschrijving van **id**.

`{path}` is het naamruimtepad van de URL-codering van de vooraf geconfigureerde hybride verbinding waarop deze listener registreren. Deze expressie wordt toegevoegd aan de vaste `$hc/` padgedeelte te hebben.

De `path` expressie kan worden uitgebreid met een achtervoegsel en een query-tekenreeksexpressie die volgt op de geregistreerde naam na een bijbehorende slash.
Hiermee kan de client afzender dispatch-argumenten doorgegeven aan de overnemende listener wanneer het is niet mogelijk om op te nemen van HTTP-headers. De verwachting is dat de listener-framework het vaste padgedeelte te hebben en de geregistreerde naam van het pad analyseert en de overige mogelijk zonder query-tekenreeksargumenten voorafgegaan maakt door `sb-`, beschikbaar voor de toepassing om te beslissen of u de verbinding accepteren.

Zie de volgende sectie voor "Afzender Protocol" voor meer informatie.

Als er een fout is, kan de service als volgt beantwoorden:

| Code | Fout          | Beschrijving
| ---- | -------------- | -----------------------------------
| 403  | Verboden      | De URL is niet geldig.
| 500  | Interne fout | Er is een fout in de service

 De verbinding tot stand is gebracht, de server afgesloten nadat de WebSocket wanneer de afzender WebSocket wordt afgesloten omlaag of met de volgende statussen:

| WS-Status | Beschrijving                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | De afzender-client de verbinding wordt afgesloten.                                    |
| 1001      | Het pad voor hybride verbinding is verwijderd of uitgeschakeld.                        |
| 1008      | Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |
| 1011      | Er is een fout in de service.                                            |

##### <a name="rejecting-the-socket"></a>De socket weigeren

 De socket weigeren nadat u hebt de `accept` bericht vereist een vergelijkbare handshake, zodat de statuscode en de beschrijving van status van communicatie van de reden voor de afwijzing kan stromen terug naar de afzender.

 Het protocol ontwerpkeuze hier is het gebruik van een WebSocket-handshake (die is ontworpen om te eindigen op een gedefinieerde foutstatus) zodat listener clientimplementaties afhankelijk zijn van een WebSocket-client blijven en hoeft niet kan te gebruiken van een extra, bare HTTP-client.

 Voor het weigeren van de socket, de client heeft de adres-URI van de `accept` bericht en voegt u twee parameters voor query-tekenreeks, op als volgt:

| Param                   | Vereist | Beschrijving                              |
| ----------------------- | -------- | ---------------------------------------- |
| SB-CH-statusCode        | Ja      | Numerieke HTTP-statuscode.                |
| SB-CH-statusDescription | Ja      | Menselijke leesbare reden voor de afwijzing. |

De resulterende URI wordt vervolgens gebruikt om een WebSocket-verbinding te maken.

Wanneer correct is voltooid, wordt deze handshake opzettelijk mislukt met een HTTP-foutcode 410, omdat er geen WebSocket is ingesteld. Als er iets mis gaat, wordt de fout in de volgende codes beschreven:

| Code | Fout          | Beschrijving                          |
| ---- | -------------- | ------------------------------------ |
| 403  | Verboden      | De URL is niet geldig.                |
| 500  | Interne fout | Er is een fout in de service. |

#### <a name="request-message"></a>Aanvraagbericht

De `request` bericht wordt verzonden door de service aan de listener via het besturingskanaal. Hetzelfde bericht worden ook verzonden via de ontmoeting WebSocket eenmaal tot stand gebracht.

De `request` bestaat uit twee delen: een header en binaire hoofdtekst frames.
Als er geen instantie is, wordt de instantie frames worden weggelaten. De indicator voor het of hoofdtekst aanwezig is, is de Booleaanse waarde `body` eigenschap in het aanvraagbericht.

Voor een aanvraag met een aanvraagtekst de structuur ziet er als volgt:

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

De listener ontvangen van de aanvraagtekst verdelen over meerdere binaire frames moet verwerken (Zie [WebSocket fragmenten](https://tools.ietf.org/html/rfc6455#section-5.4)).
De aanvraag wordt beëindigd wanneer een binaire kader met de vlag is ingesteld FIN is ontvangen.

Er is slechts één tekstframe voor een aanvraag zonder de hoofdtekst.

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

* **adres** -URI-tekenreeks. Dit is het rendezvous-adres moet worden gebruikt voor deze aanvraag. Als de binnenkomende aanvraag groter dan 64 kB is, de rest van dit bericht leeg en start de client een rendezvous-handshake gelijk is aan de `accept` bewerking die hieronder worden beschreven. De service wordt vervolgens de volledige geplaatst `request` op de bestaande Web-socket. Als het antwoord worden gehouden met meer dan 64 kB, moet de listener ook een rendezvous-handshake tot stand te brengen en zet vervolgens het antwoord via de bestaande Web-socket.
* **id** -tekenreeks. De unieke id voor deze aanvraag.
* **requestHeaders** – dit object bevat alle HTTP-headers die naar het eindpunt is verstrekt door de afzender, met uitzondering van de autorisatie-informatie zoals uitgelegd [hierboven](#request-operation), en -koppen die strikt betrekking hebben op de de verbinding met de gateway. In het bijzonder alle koppen gedefinieerd of gereserveerd in [RFC7230](https://tools.ietf.org/html/rfc7230), met uitzondering van `Via`, zijn verwijderd en niet doorgestuurd:

  * `Connection` (RFC7230, punt 6.1)
  * `Content-Length`  (RFC7230 punt 3.3.2)
  * `Host`  (RFC7230 punt 5.4)
  * `TE`  (RFC7230 paragraaf 4.3)
  * `Trailer`  (RFC7230 sectie 4.4)
  * `Transfer-Encoding`  (RFC7230 sectie 3.3.1)
  * `Upgrade` (RFC7230 sectie 6.7)
  * `Close`  (RFC7230 sectie 8.1)

* **requestTarget** -tekenreeks. Deze eigenschap bevat de ['Doel van de aanvraag' (RFC7230, punt 5.3)](https://tools.ietf.org/html/rfc7230#section-5.3) van de aanvraag. Dit omvat het query-tekenreeksdeel dat wordt verwijderd van alle `sb-hc-` voorafgegaan parameters.
* **methode** -tekenreeks. Dit is de methode van de aanvraag [RFC7231, sectie 4](https://tools.ietf.org/html/rfc7231#section-4). De `CONNECT` methode mogen niet worden gebruikt.
* **hoofdtekst** – Boolean-waarde. Hiermee wordt aangegeven of een meer meer binaire hoofdtekst frame wordt gevolgd.

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

De ontvanger moet reageren. Herhaalde fouten te reageren op aanvragen terwijl de verbinding kan leiden tot de listener ophalen op zwarte lijst geplaatst.

Antwoorden kunnen worden verzonden in willekeurige volgorde, maar elke aanvraag moet worden gereageerd binnen 60 seconden of de levering als wordt gemeld. De deadline 60 seconden wordt geteld totdat de `response` frame is ontvangen door de service. Een actieve antwoord met meerdere binaire frames kan niet inactief gedurende meer dan 60 seconden of het proces is beëindigd.

Als de aanvraag wordt ontvangen via het besturingskanaal, het antwoord ofwel in het besturingskanaal moet worden verzonden vanaf waar de aanvraag is ontvangen of het moet worden verzonden via een rendezvous-kanaal.

Het antwoord is een JSON-object met de naam "antwoord". De regels voor het verwerken van de inhoud van de hoofdtekst zijn precies hetzelfde als met de `request` bericht en op basis van de `body` eigenschap.

* **requestId** -tekenreeks. VEREIST. De `id` eigenschapwaarde van de `request` bericht wordt beantwoord.
* **statusCode** : aantal. VEREIST. een numerieke HTTP-statuscode die het resultaat van de melding aangeeft. Alle statuscodes van [RFC7231, sectie 6](https://tools.ietf.org/html/rfc7231#section-6) zijn toegestaan, met uitzondering van [502 'Ongeldige Gateway'](https://tools.ietf.org/html/rfc7231#section-6.6.3) en [504 'Gateway timeout gegenereerd'](https://tools.ietf.org/html/rfc7231#section-6.6.5).
* **statusDescription** -tekenreeks. OPTIONEEL. HTTP-statuscode reden per [RFC7230, punt 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2)
* **responseHeaders** – HTTP-headers worden ingesteld in een externe HTTP-antwoord.
  Net als bij de `request`, RFC7230 gedefinieerde headers moeten niet worden gebruikt.
* **hoofdtekst** – Boolean-waarde. Hiermee wordt aangegeven of de hoofdtekst van de binaire frames follow(s).

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

##### <a name="responding-via-rendezvous"></a>Reageert via rendezvous

Voor antwoorden die groter zijn dan 64 kB, moet het antwoord worden geleverd via een rendezvous-socket. Ook als de aanvraag is groter dan 64 kB en de `request` bevat alleen het adresveld een socket rendezvous moet worden ingesteld voor de `request`. Zodra een socket rendezvous actief is, moeten antwoorden voor de desbetreffende client en de volgende aanvragen van de respectieve client worden geleverd via de socket rendezvous terwijl deze zich blijft voordoen.

De `address` -URL in de `request` moeten worden gebruikt als-is voor het tot stand brengen van de socket rendezvous, maar bevat de volgende parameters:

| Parameter      | Vereist | Beschrijving
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Ja      | Voor het accepteren van een socket, moet de parameter worden `sb-hc-action=request`

Als er een fout is, kan de service als volgt beantwoorden:

| Code | Fout           | Beschrijving
| ---- | --------------- | -----------------------------------
| 400  | Ongeldige aanvraag | Onbekende action of de URL is niet geldig.
| 403  | Verboden       | De URL is verlopen.
| 500  | Interne fout  | Er is een fout in de service

 De verbinding tot stand is gebracht, de server afgesloten nadat de WebSocket wanneer de client HTTP-socket wordt afgesloten, of met de volgende statussen:

| WS-Status | Beschrijving                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | De afzender-client de verbinding wordt afgesloten.                                    |
| 1001      | Het pad voor hybride verbinding is verwijderd of uitgeschakeld.                        |
| 1008      | Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |
| 1011      | Er is een fout in de service.                                            |


#### <a name="listener-token-renewal"></a>Vernieuwing van het token listener

Wanneer de listener-token bijna verlopen is, kan deze door een frame tekstbericht verzenden naar de service via het tot stand gebrachte besturingskanaal vervangen. Het bericht bevat een JSON-object aangeroepen `renewToken`, definieert een eigenschap van de volgende op dit moment:

* **token** – een geldige, door de URL-codering Service Bus gedeeld toegangstoken voor de naamruimte of hybride verbinding die verleent de **luisteren** rechts.

```json
{
  "renewToken": {
    "token":
      "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
  }
}
```

Als de validatie van het token is mislukt, de toegang is geweigerd en de cloudservice wordt gesloten voor het besturingskanaal WebSocket met een fout. Anders is geen antwoord ontvangen.

| WS-Status | Beschrijving                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1008      | Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |

### <a name="web-socket-connect-protocol"></a>Web-Socket protocol verbinding maken

De afzender-protocol is in feite gelijk is aan de manier waarop die een listener tot stand is gebracht.
Het doel is de maximale transparantie voor de WebSocket-end-to-end. Verbinding maken met het adres is hetzelfde als voor de listener, maar de "action" verschilt en het token moet een andere machtiging:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

De _naamruimte adres_ is de volledig gekwalificeerde domeinnaam van de Azure-Relay-naamruimte die als host fungeert voor de hybride verbinding, doorgaans van het formulier `{myname}.servicebus.windows.net`.

De aanvraag kan willekeurige extra HTTP-headers, met inbegrip van de toepassing gedefinieerde waarden bevatten. Alle opgegeven headers overgebracht naar de listener en kunt u vinden op de `connectHeader` object van het **accepteren** bericht besturingselement.

De parameteropties voor query-tekenreeks zijn als volgt:

| Param          | Vereist? | Beschrijving
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | Ja       | Voor de rol van de afzender, de parameter moet `sb-hc-action=connect`.
| `{path}`       | Ja       | (Zie het volgende lid)
| `sb-hc-token`  | Ja\*     | De listener moet een geldige, door de URL-codering Service Bus Shared Access Token opgeven voor de naamruimte of hybride verbinding die verleent de **verzenden** rechts.
| `sb-hc-id`     | Nee        | Een optionele ID die end-to-end-diagnostische tracering en beschikbaar wordt gesteld aan de listener tijdens de handshake accepteren.

 De `{path}` is het naamruimtepad van de URL-codering van de vooraf geconfigureerde hybride verbinding waarop deze listener registreren. De `path` expressie kan worden uitgebreid met een achtervoegsel en een tekenreeksexpressie query verder communiceren. Als de hybride verbinding is geregistreerd onder het pad `hyco`, wordt de `path` expressie kan worden `hyco/suffix?param=value&...` gevolgd door de queryreeksparameters die hier zijn gedefinieerd. Een volledige expressie kan vervolgens als volgt zijn:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

De `path` expressie wordt doorgegeven aan de listener in de adres-URI die zijn opgenomen in het bericht 'accepteren' besturingselement.

Als de WebSocket-verbinding is mislukt vanwege de hybride verbinding pad niet wordt geregistreerd, een ongeldige of ontbrekende token of een andere fout, wordt de fout feedback opgegeven met het normale HTTP 1.1-status feedback model. De beschrijving van status bevat een fout tracerings-ID die kan worden gecommuniceerd naar Azure ondersteuningspersoneel:

| Code | Fout          | Beschrijving
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Niet gevonden      | Het pad voor hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling.
| 401  | Niet geautoriseerd   | Het beveiligingstoken is ontbreken of onjuist gevormd of ongeldig.
| 403  | Verboden      | Het beveiligingstoken is niet geldig voor dit pad en voor deze actie.
| 500  | Interne fout | Er is een fout in de service.

Als de WebSocket-verbinding opzettelijk door de service afgesloten wordt als deze in eerste instantie is ingesteld om de reden voor dit wel doet wordt gecommuniceerd met behulp van een juiste WebSocket-protocol foutcode samen met een beschijvend foutbericht die ook een tracerings-ID.

| WS-Status | Beschrijving
| --------- | ------------------------------------------------------------------------------- 
| 1000      | De listener is afgesloten van de socket.
| 1001      | Het pad voor hybride verbinding is verwijderd of uitgeschakeld.
| 1008      | Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden.
| 1011      | Er is een fout in de service.

### <a name="http-request-protocol"></a>HTTP-aanvraagprotocol

De aanvraag HTTP-protocol kunt willekeurige HTTP-aanvragen, behalve protocol upgrades.
HTTP-aanvragen zijn op de entiteit reguliere runtime-adres, zonder de $hc infix die wordt gebruikt voor hybride verbindingen WebSocket-clients gericht.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

De _naamruimte adres_ is de volledig gekwalificeerde domeinnaam van de Azure-Relay-naamruimte die als host fungeert voor de hybride verbinding, doorgaans van het formulier `{myname}.servicebus.windows.net`.

De aanvraag kan willekeurige extra HTTP-headers, met inbegrip van de toepassing gedefinieerde waarden bevatten. Alle headers, met uitzondering van die rechtstreeks zijn gedefinieerd in RFC7230 opgegeven (Zie [aanvraagbericht](#Request message)) naar de listener vloeien en kunt u vinden op de `requestHeader` object van de **aanvraag** bericht.

De parameteropties voor query-tekenreeks zijn als volgt:

| Param          | Vereist? | Beschrijving
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | Ja\*     | De listener moet een geldige, door de URL-codering Service Bus Shared Access Token opgeven voor de naamruimte of hybride verbinding die verleent de **verzenden** rechts.

Het token kan ook worden uitgevoerd in ofwel de `ServiceBusAuthorization` of `Authorization` HTTP-header. Het token kan worden weggelaten als de hybride verbinding is geconfigureerd voor het toestaan van anonieme aanvragen.

Omdat de service effectief als een proxy, fungeert zelfs als niet als een HTTP-proxy van de waarde true ofwel wordt toegevoegd een `Via` header of de bestaande annotates `Via` header die compatibel zijn met [RFC7230, sectie 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1).
De service wordt toegevoegd voor de hostnaam van de Relay-naamruimte voor `Via`.

| Code | Bericht  | Beschrijving                    |
| ---- | -------- | ------------------------------ |
| 200  | OK       | De aanvraag is verwerkt door ten minste één listener.  |
| 202  | Geaccepteerd | De aanvraag is goedgekeurd door ten minste één listener. |

Als er een fout is, kunt de service als volgt reageren. Hiermee wordt aangegeven of het antwoord afkomstig is van de service of van de listener kan worden geïdentificeerd door middel van de aanwezigheid van de `Via` header. Als de header aanwezig is, is het antwoord van de listener.

| Code | Fout           | Beschrijving
| ---- | --------------- |--------- |
| 404  | Niet gevonden       | Het pad voor hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling.
| 401  | Niet geautoriseerd    | Het beveiligingstoken is ontbreken of onjuist gevormd of ongeldig.
| 403  | Verboden       | Het beveiligingstoken is niet geldig voor dit pad en voor deze actie.
| 500  | Interne fout  | Er is een fout in de service.
| 503  | Ongeldige gateway     | De aanvraag kan niet worden doorgestuurd naar een listener.
| 504  | Time-out van gateway | De aanvraag is gerouteerd naar een listener, maar de listener erkent ontvangst in de tijd die nodig zijn.

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)
