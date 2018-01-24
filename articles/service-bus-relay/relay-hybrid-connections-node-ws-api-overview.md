---
title: Overzicht van de API van Azure Relay knooppunt | Microsoft Docs
description: Relay-knooppunt API-overzicht
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 696e3f77a283cc31d3c8f6007a839480ae8eb984
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="relay-hybrid-connections-node-api-overview"></a>Relay-hybride verbindingen knooppunt API-overzicht

## <a name="overview"></a>Overzicht

De [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) knooppunt-pakket voor Azure Relay hybride verbindingen is gebaseerd op en breidt de ['ws'](https://www.npmjs.com/package/ws) NPM-pakket. Dit pakket opnieuw exporteert uitvoer van dat base pakket en voegt nieuwe uitvoer die integratie met de functie Azure Relay service hybride verbindingen inschakelen. 

Bestaande toepassingen die `require('ws')` kunt dit pakket met `require('hyco-ws')` in plaats daarvan ook waardoor hybride scenario's waarin een toepassing kunt luisteren voor WebSocket-verbindingen van 'binnen de firewall' lokaal en via hybride verbindingen, alles op hetzelfde moment.
  
## <a name="documentation"></a>Documentatie

De API's zijn [gedocumenteerd in het pakket belangrijkste ws](https://github.com/websockets/ws/blob/master/doc/ws.md). Dit artikel wordt beschreven hoe dit pakket verschilt van die basislijn. 

De belangrijkste verschillen tussen het basis-pakket en deze 'hyco ws' is dat het een nieuwe serverklasse, geëxporteerd toevoegt `require('hyco-ws').RelayedServer`, en een aantal methoden.

### <a name="package-helper-methods"></a>Pakket Help-methoden

Er zijn verschillende hulpprogrammamethoden beschikbaar op het pakket exporteren die u kunt verwijzen naar als volgt:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

De methoden voor gebruik met dit pakket zijn, maar kunnen ook worden gebruikt door de server van een knooppunt voor het inschakelen van web- of clients listeners of afzenders te maken. De server gebruikt deze methoden door het doorgeven van URI's waarvan de tijdelijke tokens insluiten. Deze URI's kan ook worden gebruikt met algemene WebSocket-stacks die instelling HTTP-headers niet de WebSocket-handshake ondersteunen. Autorisatie tokens insluiten in de URI wordt voornamelijk voor deze bibliotheek-externe gebruiksscenario's ondersteund. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Maakt een geldig Azure Relay hybride verbinding listener URI voor de opgegeven naamruimte en het pad. Deze URI kan vervolgens worden gebruikt met de relay-versie van de klasse WebSocketServer.

- `namespaceName`(vereist): het domein gekwalificeerde naam van de Azure-Relay-naamruimte moet worden gebruikt.
- `path`(vereist): de naam van een bestaande hybride verbinding van een Azure-Relay in waarmee de naamruimte.
- `token`(optioneel): een eerder uitgegeven relaytoegang token die is ingesloten in de URI-listener (Zie het volgende voorbeeld).
- `id`(optioneel): een tracerings-id end-to-end diagnostische tracering van aanvragen.

De `token` waarde is optioneel en mag alleen worden gebruikt wanneer het is niet mogelijk voor het verzenden van HTTP-headers samen met de WebSocket-handshake, zoals het geval is met de W3C-WebSocket-stack.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Hiermee maakt u een geldige Azure Relay hybride verbinding verzenden URI voor de opgegeven naamruimte en het pad. Deze URI kan worden gebruikt met een WebSocket-client.

- `namespaceName`(vereist): het domein gekwalificeerde naam van de Azure-Relay-naamruimte moet worden gebruikt.
- `path`(vereist): de naam van een bestaande hybride verbinding van een Azure-Relay in waarmee de naamruimte.
- `token`(optioneel): een eerder uitgegeven relaytoegang token die is ingesloten in de URI (Zie het volgende voorbeeld).
- `id`(optioneel): een tracerings-id end-to-end diagnostische tracering van aanvragen.

De `token` waarde is optioneel en mag alleen worden gebruikt wanneer het is niet mogelijk voor het verzenden van HTTP-headers samen met de WebSocket-handshake, zoals het geval is met de W3C-WebSocket-stack.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Maakt een Azure Relay Shared Access Signature (SAS)-token voor het opgegeven doel-URI, SAS-regel en SAS-sleutel voor regel die is geldig voor het opgegeven aantal seconden of voor een uur van de huidige instant als het argument voor de verloopdatum wordt weggelaten.

- `uri`(vereist): de URI waarvan het token is worden uitgegeven. De URI voor het gebruik van het HTTP-schema is genormaliseerd en querygegevens tekenreeks wordt verwijderd.
- `ruleName`(vereist) - regelnaam SAS voor beide de entiteit die wordt vertegenwoordigd door de opgegeven URI of voor de naamruimte die wordt vertegenwoordigd door het hostgedeelte van URI.
- `key`(vereist) - geldige sleutel voor de SAS-regel. 
- `expirationSeconds`(optioneel): het aantal seconden totdat het gegenereerde token moet verlopen. Als niet wordt opgegeven, is de standaardwaarde 1 uur (3600).

Het gepubliceerde token verleent de rechten die zijn gekoppeld aan de opgegeven SAS-regel voor de opgegeven duur.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Deze methode is functioneel equivalent met de `createRelayToken` methode beschreven eerder, maar retourneert het token correct toegevoegd aan de invoer-URI.

### <a name="class-wsrelayedserver"></a>Klasse ws. RelayedServer

De `hycows.RelayedServer` klasse vormt een alternatief voor de `ws.Server` klasse die luistert niet op het lokale netwerk, maar gemachtigden luisteren naar de Azure-Relay-service.

De twee klassen zijn voornamelijk contract die compatibel zijn, wat betekent dat een bestaande toepassing met behulp van de `ws.Server` klasse kan eenvoudig worden gewijzigd om de relayed versie te gebruiken. De belangrijkste verschillen zijn in de constructor en de beschikbare opties.

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

De `RelayedServer` constructor biedt ondersteuning voor een andere set argumenten dan de `Server`, omdat deze niet de listener voor een zelfstandige of kan worden ingesloten in een bestaande HTTP-listener-framework. Er zijn ook minder mogelijkheden beschikbaar omdat de WebSocket-management grotendeels aan de Relay-service overgedragen is.

Constructor-argumenten:

- `server`(vereist) - de volledig gekwalificeerde URI voor de naam van een hybride verbinding waarop wordt geluisterd, meestal wordt gemaakt met de WebSocket.createRelayListenUri() Help-methode.
- `token`Dit argument is (vereist) - bevat een eerder uitgegeven token tekenreeks of een callback-functie die kan worden aangeroepen voor een token tekenreeks. De callback-optie verdient de voorkeur, zoals kunnen de vernieuwing van het token.

#### <a name="events"></a>Gebeurtenissen

`RelayedServer`exemplaren verzenden drie gebeurtenissen waarmee u voor het verwerken van binnenkomende aanvragen, verbindingen tot stand brengen en foutcondities detecteren. U moet zich abonneren op de `connect` gebeurtenis om berichten te handelen. 

##### <a name="headers"></a>headers

```JavaScript 
function(headers)
```

De `headers` gebeurtenis wordt geactiveerd vlak voordat een binnenkomende verbinding is geaccepteerd, het inschakelen van de wijziging van de headers worden verzonden naar de client. 

##### <a name="connection"></a>verbinding

```JavaScript
function(socket)
```

Verzonden wanneer een nieuwe WebSocket-verbinding is geaccepteerd. Het object is van het type `ws.WebSocket`, net als met de basis-pakket.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Als de onderliggende server een fout opgetreden verzendt, is deze hier doorgestuurd.  

#### <a name="helpers"></a>Hulpprogramma 's

Om te beginnen een relayed server en onmiddellijk abonneren op binnenkomende verbindingen vereenvoudigen, wordt het pakket een eenvoudige Help-functie, die ook wordt gebruikt in de voorbeelden als volgt:

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

Deze methode wordt de constructor voor het maken van een nieuw exemplaar van de RelayedServer en klik vervolgens op de opgegeven retouraanroep toe aan de gebeurtenis 'verbinding' is geabonneerd.
 
##### <a name="relayedconnect"></a>relayedConnect

Spiegelen gewoon de `createRelayedServer` helper in de functie `relayedConnect` een clientverbinding maakt en de gebeurtenis 'open' op de resulterende socket is geabonneerd.

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
