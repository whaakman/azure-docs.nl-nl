---
title: Overzicht van de API's van Azure Relay knooppunt | Microsoft Docs
description: Relay-knooppunt API-overzicht
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: bf0173f9c9802be689f7f3a893d381a251a2b16a
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43701133"
---
# <a name="relay-hybrid-connections-node-api-overview"></a>Relay-hybride verbindingen knooppunt API-overzicht

## <a name="overview"></a>Overzicht

De [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) knooppunt-pakket voor hybride Azure Relay-verbindingen is gebouwd op en breidt de ['ws'](https://www.npmjs.com/package/ws) NPM-pakket. Dit pakket opnieuw alle uitvoer van deze basispakket exporteert en voegt nieuwe uitvoer die mogelijk van integratie met de functie hybride verbindingen van Azure Relay-service. 

Bestaande toepassingen die `require('ws')` kunt dit pakket met `require('hyco-ws')` in plaats daarvan ook waarmee hybride scenario's waarin een toepassing kan luisteren voor WebSocket-verbindingen van 'binnen de firewall' lokaal en via hybride verbindingen, alles op tegelijkertijd.
  
## <a name="documentation"></a>Documentatie

De API's zijn [beschreven in het pakket belangrijkste ws](https://github.com/websockets/ws/blob/master/doc/ws.md). Dit artikel wordt beschreven hoe dit pakket verschilt van die basislijn. 

De belangrijkste verschillen tussen het basis-pakket en deze 'hyco ws' is dat deze wordt toegevoegd een nieuwe serverklasse, geëxporteerd `require('hyco-ws').RelayedServer`, en een aantal methoden.

### <a name="package-helper-methods"></a>Pakket Help-methoden

Er zijn verschillende nuttige methoden beschikbaar op het pakket exporteren die u kunt verwijzen naar als volgt:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

De Help-methoden zijn voor gebruik met dit pakket, maar kunnen ook worden gebruikt door een knooppunt-server voor het inschakelen van web- of apparaat clients listeners of afzenders te maken. De server maakt gebruik van deze methoden door deze URI's die eenvoudige insluittokens worden doorgegeven. Deze URI's kan ook worden gebruikt met algemene WebSocket-stacks die geen ondersteuning voor HTTP-headers van instelling voor de WebSocket-handshake. Autorisatietokens insluiten in de URI wordt voornamelijk voor deze bibliotheek-externe gebruiksscenario's ondersteund. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Hiermee maakt u een geldig Azure Relay Hybrid Connection listener URI voor de opgegeven naamruimte en het pad. Deze URI kan vervolgens worden gebruikt met de relay-versie van de klasse WebSocketServer.

- `namespaceName` (vereist): de domein-gekwalificeerde naam van de Azure Relay-naamruimte te gebruiken.
- `path` (vereist): de naam van een bestaande Azure Relay Hybrid-verbinding in die naamruimte.
- `token` (optioneel): een eerder uitgegeven Relay access token dat is ingesloten in de URI-listener (Zie het volgende voorbeeld).
- `id` (optioneel): een tracerings-id waarmee de end-to-end diagnostische tracering van aanvragen.

De `token` waarde is optioneel en mag alleen worden gebruikt wanneer het is niet mogelijk voor het verzenden van HTTP-headers, samen met de WebSocket-handshake, zoals het geval is met de W3C-WebSocket-stack.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Hiermee maakt u een geldig Azure Relay Hybrid Connection verzenden URI voor de opgegeven naamruimte en het pad. Deze URI kan worden gebruikt met een willekeurige client WebSocket.

- `namespaceName` (vereist): de domein-gekwalificeerde naam van de Azure Relay-naamruimte te gebruiken.
- `path` (vereist): de naam van een bestaande Azure Relay Hybrid-verbinding in die naamruimte.
- `token` (optioneel): een eerder uitgegeven Relay access token dat is ingesloten in de URI (Zie het volgende voorbeeld).
- `id` (optioneel): een tracerings-id waarmee de end-to-end diagnostische tracering van aanvragen.

De `token` waarde is optioneel en mag alleen worden gebruikt wanneer het is niet mogelijk voor het verzenden van HTTP-headers, samen met de WebSocket-handshake, zoals het geval is met de W3C-WebSocket-stack.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Hiermee maakt u een Azure Relay Shared Access Signature (SAS)-token voor het opgegeven doel-URI, SAS-regel en SAS-sleutel voor regel die geldig is voor het opgegeven aantal seconden of voor een uur van de huidige instant als het argument vervaldatum wordt weggelaten.

- `uri` (vereist): de URI waarvoor het token is om te worden uitgegeven. De URI is genormaliseerd voor het gebruik van het HTTP-schema en querygegevens als tekenreeks wordt verwijderd.
- `ruleName` (vereist) - regel SAS-naam voor een van beide de entiteit die wordt vertegenwoordigd door de opgegeven URI of voor de naamruimte die wordt vertegenwoordigd door het hostgedeelte van de URI.
- `key` (vereist) - geldige sleutel voor de SAS-regel. 
- `expirationSeconds` (optioneel): het aantal seconden totdat de gegenereerde token moet verlopen. Indien niet opgegeven, is de standaardwaarde is 1 uur (3600).

De uitgegeven tokens verleent de rechten voor in verband met de opgegeven SAS-regel voor de opgegeven duur.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Deze methode is functioneel equivalent met de `createRelayToken` eerder beschreven methode retourneert, maar het token correct is toegevoegd aan de URI van de invoer.

### <a name="class-wsrelayedserver"></a>Klasse ws. RelayedServer

De `hycows.RelayedServer` klasse is een alternatief voor de `ws.Server` klasse die luistert niet op het lokale netwerk, maar gemachtigden te hebben geluisterd naar de Azure Relay-service.

De twee klassen zijn voornamelijk contract die compatibel zijn, wat betekent dat een bestaande toepassing met de `ws.Server` klasse kan eenvoudig worden gewijzigd om de relayed versie te gebruiken. De belangrijkste verschillen zijn in de constructor en de beschikbare opties.

#### <a name="constructor"></a>Constructor  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

De `RelayedServer` constructor biedt ondersteuning voor een andere set argumenten gebruiken dan de `Server`, omdat het is niet een listener zelfstandige of kunnen worden ingesloten in een bestaand framework van de HTTP-listener. Er zijn ook minder opties beschikbaar omdat de WebSocket-management is grotendeels gedelegeerd naar de Relay-service.

Constructorargumenten:

- `server` (vereist): de volledig gekwalificeerde URI voor de naam van een hybride verbinding op waarnaar moet worden geluisterd, meestal geconstrueerd met de WebSocket.createRelayListenUri() Help-methode.
- `token` (vereist): dit argument een eerder uitgegeven tokens tekenreeks of een retouraanroepfunctie die kan worden aangeroepen om op te halen van een token tekenreeks bevat. De callback-optie is de voorkeur, vernieuwing van het token is ingeschakeld.

#### <a name="events"></a>Gebeurtenissen

`RelayedServer` exemplaren van verzenden drie gebeurtenissen waarmee u kunt inkomende aanvragen worden verwerkt, verbindingen tot stand brengen en opsporen van fouten. U moet zich abonneren op de `connect` gebeurtenis om berichten te verwerken. 

##### <a name="headers"></a>headers

```JavaScript 
function(headers)
```

De `headers` gebeurtenis wordt geactiveerd vlak voordat een binnenkomende verbinding geaccepteerd, waardoor het wijzigen van de headers verzenden naar de client. 

##### <a name="connection"></a>verbinding

```JavaScript
function(socket)
```

Verzonden wanneer een nieuwe WebSocket-verbinding is geaccepteerd. Het object is van het type `ws.WebSocket`, net als met de basis-pakket.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Als de onderliggende-server een fout verzendt, wordt deze hier doorgestuurd.  

#### <a name="helpers"></a>Hulpprogramma 's

Ter vereenvoudiging van een relayed server starten en Abonneer u onmiddellijk op binnenkomende verbindingen, het pakket wordt aangegeven dat een eenvoudige Help-functie, die ook wordt gebruikt in de voorbeelden als volgt:

##### <a name="createrelayedlistener"></a>createRelayedListener

```JavaScript
var WebSocket = require('hyco-ws');

var wss = WebSocket.createRelayedServer(
    {
        server: WebSocket.createRelayListenUri(ns, path),
        token: function() { return WebSocket.createRelayToken('http://' + ns, keyrule, key); }
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(JSON.parse(event.data));
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
});
``` 

##### <a name="createrelayedserver"></a>createRelayedServer

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

Deze methode roept de constructor voor het maken van een nieuw exemplaar van de RelayedServer en vervolgens de verstrekte callback aan de gebeurtenis 'verbinding' is geabonneerd.
 
##### <a name="relayedconnect"></a>relayedConnect

Spiegelen gewoon de `createRelayedServer` helper in de functie, `relayedConnect` een clientverbinding maakt en de gebeurtenis 'open' op de resulterende socket op is geabonneerd.

```JavaScript
var uri = WebSocket.createRelaySendUri(ns, path);
WebSocket.relayedConnect(
    uri,
    WebSocket.createRelayToken(uri, keyrule, key),
    function (socket) {
        ...
    }
);
```

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure Relay:
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Beschikbare Relay-API 's](relay-api-overview.md)
