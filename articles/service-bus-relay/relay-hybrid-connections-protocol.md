---
title: Azure Relay hybride verbindingen protocol handleiding | Microsoft Docs
description: Handleiding voor Azure Relay hybride verbindingen-protocol.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: 
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 43c40baa74b3f7c1f5c9d6626b25bcd45c2f9a10
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Protocol voor Azure Relay hybride verbindingen
Azure Relay is een van de belangrijkste mogelijkheden stijlen van het Azure Service Bus-platform. De nieuwe *hybride verbindingen* mogelijkheid relay is een op basis van HTTP- en WebSockets evolutie van beveiligde, open-protocol. Het eerste evenredig met de naam die hij vervangt *BizTalk Services* functie die is gebaseerd op een eigen protocol foundation. De integratie van hybride verbindingen in Azure App Services, blijven functioneren als-is.

Hybride verbindingen kunnen in twee richtingen, binaire gegevensstroom communicatie tussen twee netwerktoepassingen waarover een of beide partijen kunnen zich achter een NAT's en firewalls. In dit artikel beschrijft de clientzijde interactie met de relay hybride verbindingen voor het verbinden van clients in listener en functies van de afzender en hoe listeners nieuwe verbindingen accepteren.

## <a name="interaction-model"></a>Interactie model
De relay hybride verbindingen verbindt twee partijen door een rendezvous-punt in de Azure-cloud die zowel partijen kunnen detecteren en verbinding maken met vanaf hun eigen netwerk perspectief te bieden. Dat rendezvous-punt wordt 'Hybride verbinding' in dit document en andere documentatie genoemd in de API's en in de Azure portal. Het service-eindpunt voor hybride verbindingen aangeduid als 'service' voor de rest van dit artikel. Het model interactie leans op de naamgeving volgt de namen tot stand gebracht door veel andere netwerk-API's.

Er is een listener die eerst duidt gereedheid voor het afhandelen van binnenkomende verbindingen en vervolgens accepteert naarmate ze binnenkomen. Aan de andere kant zijn er is een client die verbinding maakt die verbinding voor de listener, die verbinding maakt kunnen worden geaccepteerd voor het tot stand brengen van een bidirectionele communicatiepad verwacht.
'Verbinding', 'Luisteren' en 'Accepteren' zijn dezelfde voorwaarden u in de meeste socket API's vindt.

Een communicatiemodel relayed heeft beide partijen maken van uitgaande verbindingen naar een service-eindpunt dat zorgt ervoor dat de 'listener' ook een 'client' in gewone gebruik en mogelijk ook andere terminologie overloads. De precieze terminologie die wij daarom voor hybride verbindingen gebruiken is als volgt:

De programma's aan beide zijden van een verbinding worden 'clients,' genoemd, omdat ze clients naar de service. De client die wordt gewacht op en aanvaardt verbindingen is de 'listener' of genoemd in de 'listener rol'. De client die een nieuwe verbinding naar een listener via de service start wordt de 'afzender' genoemd, of bevindt zich in de 'afzender rol'.

### <a name="listener-interactions"></a>Listener-interacties
De listener beschikt over vier interactie met de service; alle gegevens van de kabel worden verderop in dit artikel in de sectie Verwijzingen beschreven.

#### <a name="listen"></a>Luisteren
Om aan te geven van de gereedheid van de service die een listener is gereed om te accepteren, verbindingen, maakt het een uitgaande WebSocket-verbinding. De handshake van verbinding zijn voor de naam van een hybride verbinding geconfigureerd op de Relay-naamruimte en een beveiligingstoken dat rechts op de 'luisteren' verleent deze naam.
Wanneer de WebSocket is geaccepteerd door de service, de registratie is voltooid en het tot stand gebrachte web WebSocket is in stand gehouden als 'besturingskanaal' voor het inschakelen van alle opeenvolgende interacties. De service kunnen maximaal 25 gelijktijdige listeners op een hybride verbinding. Als er twee of meer actieve listeners, worden binnenkomende verbindingen verdeeld zijn over ze in willekeurige volgorde; evenredige verdeling kan niet worden gegarandeerd.

#### <a name="accept"></a>Accepteren
Wanneer een afzender een nieuwe verbinding op de service opent, wordt de service kiest en waarschuwt een van de actieve listeners op de hybride verbinding. Deze melding wordt verzonden naar de listener via het open besturingskanaal als een JSON-bericht met de URL van het WebSocket-eindpunt dat de listener verbinding maken moet met voor het accepteren van de verbinding.

De URL kan en moet rechtstreeks worden gebruikt door de listener zonder extra werk.
De gecodeerde gegevens is alleen geldig voor een korte periode, hoofdzakelijk voor zolang de afzender is bereid om te wachten voor de verbinding tot stand gebrachte end-to-end worden, maar maximaal 30 seconden. De URL kan alleen worden gebruikt voor een geslaagde verbindingspoging. Zodra de WebSocket-verbinding met de rendezvous-URL is gemaakt, wordt alle verdere activiteiten op dit WebSocket doorgegeven van en naar de afzender, zonder tussenkomst van de of interpretatie door de service.

#### <a name="renew"></a>Verlengen
Het beveiligingstoken dat moet worden gebruikt voor het registreren van de listener en onderhouden van het besturingskanaal mogelijk verlopen terwijl de listener actief is. Het toegangstoken heeft geen invloed op actieve verbindingen, maar dit leidt tot het besturingskanaal worden verwijderd door de service op of kort na het tijdstip van de geldigheidsduur. De bewerking 'vernieuwen' is een JSON-bericht dat de listener verzenden kunt ter vervanging van het token dat is gekoppeld aan het besturingskanaal zodat het besturingskanaal gedurende langere perioden kan worden beheerd.

#### <a name="ping"></a>Ping
Als het besturingskanaal inactief gedurende een lange periode schakels op de manier blijft zoals load balancers of NAT's mogelijk verwijderen de TCP-verbinding. De bewerking 'pingen' voorkomt dat door een kleine hoeveelheid gegevens op het kanaal dat iedereen in de netwerkroute die de verbinding is bedoeld als actief is en deze fungeert ook als een 'live' test voor de listener herinnert verzenden. Als het pingen mislukt, wordt het besturingskanaal moet worden beschouwd als onbruikbaar en de listener moet opnieuw worden verbonden.

### <a name="sender-interaction"></a>Interactie van de afzender
De afzender heeft slechts één interactie met de service: deze verbinding maakt.

#### <a name="connect"></a>Verbinding maken
De bewerking "connect" Hiermee opent u een WebSocket op de service, de naam van de hybride verbinding en (optioneel, maar vereist standaard) bieden een beveiligingsbeleid voor token verleent machtigingen voor 'Verzenden' in de queryreeks. De service communiceert vervolgens met de listener op de manier die eerder zijn beschreven, en de listener maakt een rendezvous-verbinding die wordt samengevoegd met dit WebSocket. Nadat de WebSocket heeft geaccepteerd, worden alle verdere interacties op die WebSocket met een listener verbonden zijn.

### <a name="interaction-summary"></a>Interactie van de samenvatting
Het resultaat van dit model interactie is dat de afzender client buiten de handshake met een 'schone' WebSocket, die is verbonden met een listener en dat er geen verdere preambles of voorbereiding moet afkomstig is. Dit model kunt vrijwel alle bestaande implementatie van de client WebSocket om direct te profiteren van de hybride verbindingen-service door het opgeven van een URL juist samengesteld in hun laag WebSocket-client.

De verbinding rendezvous WebSocket die de listener via de interactie accepteren verkrijgt ook schoon is en kan worden verstrekt aan een bestaande implementatie van de server WebSocket met een minimale extra abstractie die wordt onderscheid tussen 'accepteren' bewerkingen op het lokale netwerk-listeners van hun framework en hybride verbindingen 'accepteren' externe bewerkingen gemaakt.

## <a name="protocol-reference"></a>Naslaginformatie over het protocol

Deze sectie beschrijft de details van het protocol interacties die eerder zijn beschreven.

Alle WebSocket-verbindingen worden uitgevoerd op poort 443 als een upgrade vanaf HTTPS 1.1, die meestal wordt gescheiden door een aantal WebSocket-framework of API. Hier de beschrijving wordt opgeslagen implementatie neutrale, zonder dat een specifiek framework voorstellen.

### <a name="listener-protocol"></a>Listener-protocol
De listener-protocol bestaat uit twee verbinding gebaren en drie berichtbewerkingen.

#### <a name="listener-control-channel-connection"></a>Listener-kanaal controleverbinding
Het besturingskanaal is geopend met een WebSocket-verbinding te maken:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

De `namespace-address` is de volledig gekwalificeerde domeinnaam van de Azure-Relay-naamruimte die als host fungeert voor de hybride verbinding, doorgaans van het formulier `{myname}.servicebus.windows.net`.

De parameteropties voor query-tekenreeks zijn als volgt.

| Parameter | Vereist | Beschrijving |
| --- | --- | --- |
| `sb-hc-action` |Ja |Voor de listener-rol de parameter moet **sb-CH-action = luisteren** |
| `{path}` |Ja |Het naamruimtepad van de URL-codering van de vooraf geconfigureerde hybride verbinding met het registreren van deze listener op. Deze expressie wordt toegevoegd aan de vaste `$hc/` padgedeelte te hebben. |
| `sb-hc-token` |Ja\* |De listener moet een geldige, door de URL-codering Service Bus Shared Access Token opgeven voor de naamruimte of hybride verbinding die verleent de **luisteren** rechts. |
| `sb-hc-id` |Nee |Deze client opgegeven optionele ID kunt diagnostische tracering end-to-end. |

Als de WebSocket-verbinding is mislukt vanwege de hybride verbinding pad niet wordt geregistreerd, of een ongeldige of ontbrekende token of een andere fout, wordt de fout feedback opgegeven met het normale HTTP 1.1-status feedback model. De beschrijving van status bevat een fout tracerings-id die kan worden doorgegeven aan Azure ondersteuningspersoneel:

| Code | Fout | Beschrijving |
| --- | --- | --- |
| 404 |Niet gevonden |Het pad voor hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling. |
| 401 |Niet geautoriseerd |Het beveiligingstoken is ontbreken of onjuist gevormd of ongeldig. |
| 403 |Verboden |Het beveiligingstoken is niet geldig voor dit pad voor deze actie. |
| 500 |Interne fout |Er is een fout in de service. |

Als de WebSocket-verbinding opzettelijk door de service afgesloten wordt nadat deze is in eerste instantie hebt ingesteld, de reden voor dit wel doet wordt gecommuniceerd met behulp van een juiste WebSocket-protocol foutcode samen met een beschijvend foutbericht die ook een tracerings-ID. De service wordt niet afgesloten het besturingskanaal zonder dat er een fout opgetreden. Geldige afsluiting is de client worden beheerd.

| WS Status | Beschrijving |
| --- | --- |
| 1001 |Het pad voor hybride verbinding is verwijderd of uitgeschakeld. |
| 1008 |Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |
| 1011 |Er is een fout in de service. |

### <a name="accept-handshake"></a>Handshake accepteren
De melding 'accepteren' is verzonden door de service aan de listener via de eerder vastgestelde besturingskanaal als een JSON-bericht in een WebSocket frame. Er is geen antwoord op dit bericht.

Het bericht bevat een JSON-object die met de naam 'accepteren', waarmee de volgende eigenschappen zijn gedefinieerd op dit moment:

* **adres** : de URL-reeks moet worden gebruikt voor het tot stand brengen van de WebSocket met de service een binnenkomende verbinding accepteren.
* **id** – de unieke id voor deze verbinding. Als de ID is opgegeven door de afzender-client, is dit de afzender opgegeven waarde, anders is een waarde van het systeem gegenereerd.
* **connectHeaders** – alle HTTP-headers die door de afzender, waaronder ook Sec-WebSocket-Protocol en de seconde-WebSocket-extensies headers met de Relay-eindpunt worden verstrekt.

#### <a name="accept-message"></a>Bericht accepteren

```json
{                                                           
    "accept" : {
        "address" : "wss://168.61.148.205:443/$hc/{path}?..."    
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

#### <a name="accepting-the-socket"></a>Acceptatie van de socket
Als u wilt accepteren, stelt u de listener voor een WebSocket-verbinding naar het opgegeven adres.

Als het bericht 'accepteren' zijn voor een `Sec-WebSocket-Protocol` koptekst wordt verwacht dat de listener alleen de WebSocket accepteert als het protocol ondersteunt. Bovendien wordt de koptekst als de WebSocket tot stand is gebracht.

Hetzelfde geldt voor de `Sec-WebSocket-Extensions` header. Als het framework een extensie ondersteunt, moet deze de header ingesteld op een antwoord voor de serverzijde van de vereiste `Sec-WebSocket-Extensions` handshake voor de extensie.

De URL moet worden gebruikt als-is voor het tot stand brengen van de socket accepteren, maar bevat de volgende parameters:

| Parameter | Vereist | Beschrijving |
| --- | --- | --- |
| `sb-hc-action` |Ja |Voor het accepteren van een socket, moet de parameter worden`sb-hc-action=accept` |
| `{path}` |Ja |(Zie het volgende lid) |
| `sb-hc-id` |Nee |Zie vorige beschrijving van **id**. |

`{path}`is het naamruimtepad van de URL-codering van de vooraf geconfigureerde hybride verbinding waarop deze listener registreren. Deze expressie wordt toegevoegd aan de vaste `$hc/` padgedeelte te hebben. 

De `path` expressie kan worden uitgebreid met een achtervoegsel en een query-tekenreeksexpressie die volgt op de geregistreerde naam na een bijbehorende slash. Hiermee kan de client afzender dispatch-argumenten doorgegeven aan de overnemende listener wanneer het is niet mogelijk om op te nemen van HTTP-headers. De verwachting is dat de listener-framework het vaste padgedeelte te hebben en de geregistreerde naam van het pad analyseert en de overige mogelijk zonder query-tekenreeksargumenten voorafgegaan maakt door `sb-`, beschikbaar voor de toepassing om te beslissen of u de verbinding accepteren.

Zie de volgende sectie voor "Afzender Protocol" voor meer informatie.

Als er een fout is, kan de service als volgt beantwoorden:

| Code | Fout | Beschrijving |
| --- | --- | --- |
| 403 |Verboden |De URL is niet geldig. |
| 500 |Interne fout |Er is een fout in de service |

De verbinding tot stand is gebracht, de server afgesloten nadat de WebSocket wanneer de afzender WebSocket wordt afgesloten omlaag of met de volgende statussen:

| WS Status | Beschrijving |
| --- | --- |
| 1001 |De afzender-client de verbinding wordt afgesloten. |
| 1001 |Het pad voor hybride verbinding is verwijderd of uitgeschakeld. |
| 1008 |Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |
| 1011 |Er is een fout in de service. |

#### <a name="rejecting-the-socket"></a>De socket weigeren
De socket weigeren na inspectie van het bericht 'accepteren' vereist een vergelijkbare handshake, zodat de statuscode en de beschrijving van status van de reden voor de afwijzing communicatie terug naar de afzender stromen kunnen.

Het protocol ontwerpkeuze hier is het gebruik van een WebSocket-handshake (die is ontworpen om te eindigen op een gedefinieerde foutstatus) zodat listener clientimplementaties afhankelijk zijn van een WebSocket-client blijven en hoeft niet kan te gebruiken van een extra, bare HTTP-client.

Voor het weigeren van de socket de client neemt de URI-adres van het bericht 'accepteren' en voegt van twee queryreeksparameters, als volgt:

| Param | Vereist | Beschrijving |
| --- | --- | --- |
| statusCode |Ja |Numerieke HTTP-statuscode. |
| StatusDescription |Ja |Menselijke leesbare reden voor de afwijzing. |

De resulterende URI wordt vervolgens gebruikt om een WebSocket-verbinding te maken.

Wanneer correct is voltooid, wordt deze handshake opzettelijk mislukt met een HTTP-foutcode 410, omdat er geen WebSocket is ingesteld. Als er iets mis gaat, wordt de fout in de volgende codes beschreven:

| Code | Fout | Beschrijving |
| --- | --- | --- |
| 403 |Verboden |De URL is niet geldig. |
| 500 |Interne fout |Er is een fout in de service. |

### <a name="listener-token-renewal"></a>Vernieuwing van het token listener
Wanneer de listener-token bijna verlopen is, kan deze door een frame tekstbericht verzenden naar de service via het tot stand gebrachte besturingskanaal vervangen. Het bericht bevat een JSON-object aangeroepen `renewToken`, definieert een eigenschap van de volgende op dit moment:

* **token** – een geldige, door de URL-codering Service Bus gedeeld toegangstoken voor de naamruimte of hybride verbinding die verleent de **luisteren** rechts.

#### <a name="renewtoken-message"></a>renewToken bericht

```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Als de validatie van het token is mislukt, de toegang is geweigerd en de cloudservice wordt gesloten voor het besturingskanaal WebSocket met een fout. Anders is geen antwoord ontvangen.

| WS Status | Beschrijving |
| --- | --- |
| 1008 |Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |

## <a name="sender-protocol"></a>Afzender-protocol
De afzender-protocol is in feite gelijk is aan de manier waarop die een listener tot stand is gebracht.
Het doel is de maximale transparantie voor de WebSocket-end-to-end. Verbinding maken met het adres is hetzelfde als voor de listener, maar de "action" verschilt en het token moet een andere machtiging:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

De *naamruimte adres* is de volledig gekwalificeerde domeinnaam van de Azure-Relay-naamruimte die als host fungeert voor de hybride verbinding, doorgaans van het formulier `{myname}.servicebus.windows.net`.

De aanvraag kan willekeurige extra HTTP-headers, met inbegrip van de toepassing gedefinieerde waarden bevatten. Alle opgegeven headers overgebracht naar de listener en kunt u vinden op de `connectHeader` object van het **accepteren** bericht besturingselement.

De parameteropties voor query-tekenreeks zijn als volgt:

| Param | Vereist? | Beschrijving |
| --- | --- | --- |
| `sb-hc-action` |Ja |Voor de rol van de afzender, de parameter moet `action=connect`. |
| `{path}` |Ja |(Zie het volgende lid) |
| `sb-hc-token` |Ja\* |De listener moet een geldige, door de URL-codering Service Bus Shared Access Token opgeven voor de naamruimte of hybride verbinding die verleent de **verzenden** rechts. |
| `sb-hc-id` |Nee |Een optionele ID die end-to-end-diagnostische tracering en beschikbaar wordt gesteld aan de listener tijdens de handshake accepteren. |

De `{path}` is het naamruimtepad van de URL-codering van de vooraf geconfigureerde hybride verbinding waarop deze listener registreren. De `path` expressie kan worden uitgebreid met een achtervoegsel en een tekenreeksexpressie query verder communiceren. Als de hybride verbinding is geregistreerd onder het pad `hyco`, wordt de `path` expressie kan worden `hyco/suffix?param=value&...` gevolgd door de queryreeksparameters die hier zijn gedefinieerd. Een volledige expressie kan vervolgens als volgt zijn:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

De `path` expressie wordt doorgegeven aan de listener in de adres-URI die zijn opgenomen in het bericht 'accepteren' besturingselement.

Als de WebSocket-verbinding is mislukt vanwege de hybride verbinding pad niet wordt geregistreerd, een ongeldige of ontbrekende token of een andere fout, wordt de fout feedback opgegeven met het normale HTTP 1.1-status feedback model. De beschrijving van status bevat een fout tracerings-ID die kan worden gecommuniceerd naar Azure ondersteuningspersoneel:

| Code | Fout | Beschrijving |
| --- | --- | --- |
| 404 |Niet gevonden |Het pad voor hybride verbinding is ongeldig of de basis-URL heeft onjuiste indeling. |
| 401 |Niet geautoriseerd |Het beveiligingstoken is ontbreken of onjuist gevormd of ongeldig. |
| 403 |Verboden |Het beveiligingstoken is niet geldig voor dit pad en voor deze actie. |
| 500 |Interne fout |Er is een fout in de service. |

Als de WebSocket-verbinding opzettelijk door de service afgesloten wordt als deze in eerste instantie is ingesteld om de reden voor dit wel doet wordt gecommuniceerd met behulp van een juiste WebSocket-protocol foutcode samen met een beschijvend foutbericht die ook een tracerings-ID.

| WS Status | Beschrijving |
| --- | --- |
| 1000 |De listener is afgesloten van de socket. |
| 1001 |Het pad voor hybride verbinding is verwijderd of uitgeschakeld. |
| 1008 |Het beveiligingstoken is verlopen, dus het verificatiebeleid wordt geschonden. |
| 1011 |Er is een fout in de service. |

## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

