---
title: Over het gebruik van Service Bus-wachtrijen in Node.js | Microsoft Docs
description: Informatie over het gebruik van Service Bus-wachtrijen in Azure vanuit een Node.js-app.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 09/10/2018
ms.author: aschhab
ms.openlocfilehash: 32b566056de76d4e73b88c7ce37e148b4ecc3fd7
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56587868"
---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a>Over het gebruik van Service Bus-wachtrijen met Node.js

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt beschreven hoe u Service Bus-wachtrijen gebruiken met behulp van Node.js. De voorbeelden zijn geschreven in JavaScript en gebruiken van de Node.js-Azure-module. De behandelde scenario's zijn **maken van wachtrijen**, **verzenden en ontvangen van berichten**, en **verwijderen van wachtrijen**. Zie voor meer informatie over wachtrijen de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]


## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
Maak een lege Node.js-toepassing. Zie voor instructies over het maken van een Node.js-toepassing [maken en implementeren van een Node.js-toepassing naar een Azure-Website][Create and deploy a Node.js application to an Azure Website], of [Node.js-Cloudservice] [ Node.js Cloud Service] met behulp van Windows PowerShell.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Voor het gebruik van Azure Service Bus, downloaden en gebruiken van de Node.js-Azure-pakket. Dit pakket bevat een set met bibliotheken die met de Service Bus REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te verkrijgen
1. Gebruik de **Windows PowerShell voor Node.js** opdrachtvenster om te navigeren naar de **c:\\knooppunt\\sbqueues\\WebRole1** map waarin u uw voorbeeld hebt gemaakt de toepassing.
2. Type **npm installeren azure** in het opdrachtvenster in die moet resulteren in de uitvoer is vergelijkbaar met het volgende:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```
3. U kunt de **ls**-opdracht handmatig uitvoeren om te controleren of een **node_modules**-map is gemaakt. In deze map vinden de **azure** , dit pakket de bibliotheken die u nodig hebt bevat voor toegang tot Service Bus-wachtrijen.

### <a name="import-the-module"></a>De module importeren
In Kladblok of een andere teksteditor en voeg het volgende toe aan het begin van de **server.js** -bestand van de toepassing:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Een Azure Service Bus-verbinding instellen
De Azure-module leest de omgevingsvariabele `AZURE_SERVICEBUS_CONNECTION_STRING` informatie vereist om verbinding met de Service Bus te verkrijgen. Als deze omgevingsvariabele is niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van `createServiceBusService`.

Voor een voorbeeld van het instellen van de omgevingsvariabelen de [Azure-portal] [ Azure portal] Zie voor een Azure-Website [Node.js-webtoepassing met Storage] [ Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Een wachtrij maken
De **ServiceBusService** object kunt u werken met Service Bus-wachtrijen. De volgende code maakt een **ServiceBusService** object. Voeg deze toe aan de bovenkant van de **server.js** bestand, na de instructie voor het importeren van de Azure-module:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Door het aanroepen van `createQueueIfNotExists` op de **ServiceBusService** object, de opgegeven wachtrij wordt geretourneerd (indien aanwezig) of een nieuwe wachtrij met de opgegeven naam is gemaakt. De volgende code gebruikt `createQueueIfNotExists` maken of verbinding maken met de wachtrij met de naam `myqueue`:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

De `createServiceBusService` methode biedt ook ondersteuning voor extra opties, zodat ze wachtrij standaardinstellingen, zoals time to live of Maximale wachtrijgrootte bericht negeren. Het volgende voorbeeld wordt de maximale wachtrijgrootte ingesteld op 5 GB en een tijd naar live (TTL) waarde van 1 minuut:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filters
Optionele filters gebruiken om bewerkingen kunnen worden toegepast op bewerkingen die worden uitgevoerd met behulp van **ServiceBusService**. Filteren van bewerkingen kunt opnemen logboekregistratie, automatisch opnieuw wordt geprobeerd, enzovoort. Filters zijn objecten die een methode implementeren met de handtekening:

```javascript
function handle (requestOptions, next)
```

Hierna de voorverwerking van de Aanvraagopties, de methode moet aanroepen `next`, een callback met de volgende handtekening te geven:

```javascript
function (returnObject, finalCallback, next)
```

In deze callback en na de verwerking de `returnObject` (het antwoord van de aanvraag naar de server), moet ofwel de callback aanroepen `next` als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon aanroepen `finalCallback`, die de service is beëindigd aanroep.

Twee filters die logica voor opnieuw proberen implementeren zijn opgenomen in de Azure SDK voor Node.js, `ExponentialRetryPolicyFilter` en `LinearRetryPolicyFilter`. De volgende code maakt een `ServiceBusService` object die gebruikmaakt van de `ExponentialRetryPolicyFilter`:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Een bericht verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `sendQueueMessage` methode voor het **ServiceBusService** object. Berichten verzonden naar (en ontvangen van) Service Bus-wachtrijen zijn **BrokeredMessage** objecten en hebben een aantal standaardeigenschappen (zoals **Label** en **TimeToLive**), een woordenlijst die wordt gebruikt voor het opslaan van aangepaste toepassingsspecifieke eigenschappen en een hoofdtekst met willekeurige toepassingsgegevens. Een toepassing kan de hoofdtekst van het bericht ingesteld door een reeks wordt doorgegeven als het bericht. Alle vereiste eigenschappen standaard worden gevuld met standaardwaarden.

Het volgende voorbeeld ziet u hoe u een testbericht verzenden naar de wachtrij met de naam `myqueue` met behulp van `sendQueueMessage`:

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij
Berichten worden ontvangen van een wachtrij met de `receiveQueueMessage` methode voor het **ServiceBusService** object. Standaard worden berichten uit de wachtrij verwijderd als ze worden gelezen; u kunt echter (peek) lezen en vergrendelen van het bericht zonder deze te verwijderen uit de wachtrij door in te stellen de optionele parameter `isPeekLock` naar **waar**.

Het standaardgedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking, is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Als de `isPeekLock` parameter is ingesteld op **waar**, wordt de ontvangst een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van `deleteMessage` methode en het geven van het bericht moet worden verwijderd als een parameter. De `deleteMessage` methode wordt het bericht als verbruikt gemarkeerd en wordt deze verwijderd uit de wachtrij.

Het volgende voorbeeld ziet u hoe u ontvangen en verwerken van berichten met `receiveQueueMessage`. Het voorbeeld eerst ontvangt en Hiermee verwijdert u een bericht en vervolgens ontvangt een bericht met `isPeekLock` ingesteld op **waar**, verwijdert het bericht met `deleteMessage`:

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de `unlockMessage` methode voor het **ServiceBusService** object. Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling is verlopen (bijvoorbeeld, als de toepassing vastloopt), Service Bus wordt het bericht automatisch ontgrendelen en maken beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `deleteMessage` methode wordt aangeroepen, wordt het bericht zal opnieuw worden bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal verwerken*, dat wil zeggen, wordt elk bericht ten minste één keer worden verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met de **MessageId**-eigenschap van het bericht dat gelijk blijft bij meerdere bezorgingspogingen.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over wachtrijen.

* [Wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions]
* [Azure SDK voor Node] [ Azure SDK for Node] -bibliotheek op GitHub
* [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
