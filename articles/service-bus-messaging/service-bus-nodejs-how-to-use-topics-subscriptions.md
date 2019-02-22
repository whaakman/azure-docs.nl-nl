---
title: Over het gebruik van Azure Service Bus-onderwerpen en abonnementen met behulp van Node.js | Microsoft Docs
description: Meer informatie over het gebruik van Service Bus-onderwerpen en abonnementen in Azure vanuit een Node.js-app.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 10/16/2018
ms.author: aschhab
ms.openlocfilehash: c2a6acc1ce48a8671af20df6daa89498a81fdb4c
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56587154"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs"></a>Gebruik Service Bus-onderwerpen en abonnementen met behulp van Node.js

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Deze handleiding wordt beschreven hoe u Service Bus-onderwerpen en abonnementen van Node.js-toepassingen. De behandelde scenario's zijn onder andere:

- Het maken van onderwerpen en abonnementen 
- Het maken van abonnementsfilters 
- Berichten verzenden naar een onderwerp 
- Ontvangen van berichten van een abonnement
- Verwijderen van onderwerpen en abonnementen 

Zie voor meer informatie over onderwerpen en abonnementen, [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
Maak een lege Node.js-toepassing. Zie voor instructies over het maken van een Node.js-toepassing [Een Node.js-toepassing naar een Azure-website maken en implementeren], [Node.js-Cloudservice] [ Node.js Cloud Service] met behulp van Windows PowerShell of de website met WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Download het Node.js-Azure-pakket voor het gebruik van Service Bus. Dit pakket bevat een set met bibliotheken die met de Service Bus REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te verkrijgen
1. Open een opdrachtregelinterface zoals **PowerShell** (Windows), **Terminal** (Mac), of **Bash** (Unix).
2. Navigeer naar de map waarin u de voorbeeldtoepassing hebt gemaakt.
3. Type **npm installeren azure** in het opdrachtvenster, moet die resulteren in de volgende uitvoer:

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
3. U kunt handmatig uitvoeren de **ls** opdracht uit om te controleren of een **knooppunt\_modules** map is gemaakt. In deze map, vinden de **azure** , dit pakket de bibliotheken die u nodig hebt bevat voor toegang tot Service Bus-onderwerpen.

### <a name="import-the-module"></a>De module importeren
In Kladblok of een andere teksteditor en voeg het volgende toe aan het begin van de **server.js** -bestand van de toepassing:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Instellen van een Service Bus-verbinding
De Azure-module leest de omgevingsvariabele `AZURE_SERVICEBUS_CONNECTION_STRING` voor de verbindingsreeks die u uit de vorige stap hebt verkregen, ' de referenties ophalen." Als deze omgevingsvariabele is niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van `createServiceBusService`.

Zie voor een voorbeeld van de omgevingsvariabelen instellen voor een Azure-Cloudservice, [omgevingsvariabelen instellen](../container-instances/container-instances-environment-variables.md#azure-cli-example).



## <a name="create-a-topic"></a>Een onderwerp maken
De **ServiceBusService** object kunt u werken met onderwerpen. De volgende code maakt een **ServiceBusService** object. Voeg deze toe aan de bovenkant van de **server.js** bestand, na de instructie voor het importeren van de azure-module:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Als u aanroepen `createTopicIfNotExists` op de **ServiceBusService** object, de opgegeven onderwerp wordt geretourneerd (indien aanwezig) of een nieuw onderwerp met de opgegeven naam is gemaakt. De volgende code gebruikt `createTopicIfNotExists` maken of verbinding maken met het onderwerp met de naam `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

De `createTopicIfNotExists` methode biedt ook ondersteuning voor extra opties waarmee u kunt de standaardinstellingen van onderwerp zoals time to live van bericht of grootte van het maximum aantal onderwerp negeren. 

Het volgende voorbeeld wordt de grootte van het maximum aantal onderwerp naar 5 GB met een time to live van één minuut van:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filters
Optionele filters gebruiken om bewerkingen kunnen worden toegepast op bewerkingen die worden uitgevoerd met behulp van **ServiceBusService**. Filteren van bewerkingen kunt opnemen logboekregistratie, automatisch opnieuw wordt geprobeerd, enzovoort. Filters zijn objecten die een methode implementeren met de handtekening:

```javascript
function handle (requestOptions, next)
```

Na het uitvoeren van voorverwerking van de Aanvraagopties, de methode roept `next`, en geeft een retouraanroep met de handtekening van de volgende:

```javascript
function (returnObject, finalCallback, next)
```

In deze callback en na de verwerking de `returnObject` (het antwoord van de aanvraag naar de server), de callback moet de volgende aanroepen (indien aanwezig) als u wilt doorgaan met het verwerken van andere filters of aanroepen `finalCallback` aan het einde van het service-aanroepen.

Twee filters die logica implementeren voor nieuwe pogingen zijn opgenomen in de Azure SDK voor Node.js: **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende code maakt een **ServiceBusService** object die gebruikmaakt van de **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Maken van abonnementen
Ook onderwerpabonnementen worden gemaakt met de **ServiceBusService** object. Abonnementen zijn met de naam en een optioneel filter dat u Hiermee beperkt u de verzameling berichten die worden geleverd aan de virtuele wachtrij van het abonnement kunnen hebben.

> [!NOTE]
> Abonnementen zijn permanent totdat een van beide ze of het onderwerp zijn gekoppeld, worden verwijderd. Als uw toepassing bevat de logica voor het maken van een abonnement, moet er eerst gecontroleerd of het abonnement met behulp van bestaat de `getSubscription` methode.
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
De **MatchAll** filter is het standaardfilter dat wordt gebruikt wanneer een abonnement wordt gemaakt. Bij gebruik van het **MatchAll**-filter worden alle berichten die naar het onderwerp worden gepubliceerd, in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld maakt u een abonnement met de naam AllMessages en gebruikmaakt van de **MatchAll** filter.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
U kunt ook filters waarmee u aan het bereik dat berichten naar een onderwerp verzonden binnen een bepaald onderwerpabonnement weergegeven maken.

Het meest flexibele type filter dat wordt ondersteund door abonnementen is de **SqlFilter**, die wordt geïmplementeerd met een subset van SQL92 wordt geïmplementeerd. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Raadpleeg voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter de [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxis.

Filters kunnen worden toegevoegd aan een abonnement met behulp van de `createRule` -methode van de **ServiceBusService** object. Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [!NOTE]
> Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft alle andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de `deleteRule` -methode van de **ServiceBusService** object.
>
>

Het volgende voorbeeld wordt een abonnement genaamd `HighMessages` met een **SqlFilter** dat alleen berichten selecteert die een aangepaste `messagenumber` eigenschap groter dan 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `LowMessages` met een **SqlFilter** dat alleen berichten selecteert die een `messagenumber` eigenschap minder dan of gelijk aan 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Wanneer nu een bericht is verzonden naar `MyTopic`, wordt bezorgd bij ontvangers die zijn geabonneerd op de `AllMessages` onderwerpabonnement en selectief bezorgd bij ontvangers die zijn geabonneerd op de `HighMessages` en `LowMessages` onderwerpabonnementen (al naar gelang de inhoud van het bericht).

## <a name="how-to-send-messages-to-a-topic"></a>Over het verzenden van berichten naar een onderwerp
Voor het verzenden van een bericht naar een Service Bus-onderwerp, uw toepassing moet gebruikmaken van de `sendTopicMessage` -methode van de **ServiceBusService** object.
Berichten verzonden naar Service Bus-onderwerpen zijn **BrokeredMessage** objecten.
**BrokeredMessage** objecten hebben een aantal standaardeigenschappen (zoals `Label` en `TimeToLive`), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren en een hoofdtekst met tekenreeksen. Een toepassing de hoofdtekst van het bericht worden ingesteld door door te geven van een string-waarde voor de `sendTopicMessage` en alle vereiste standaardeigenschappen zijn gevuld met standaardwaarden.

Het volgende voorbeeld ziet u hoe u voor het verzenden van vijf testberichten naar `MyTopic`. De `messagenumber` eigenschapswaarde van elk bericht varieert op de herhaling van de lus (deze eigenschap bepaalt welke abonnementen ontvangen):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten op een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement
Berichten worden ontvangen van een abonnement met de `receiveSubscriptionMessage` methode voor het **ServiceBusService** object. Standaard worden berichten verwijderd uit het abonnement zoals ze worden gelezen. U kunt echter de optionele parameter instellen `isPeekLock` naar **waar** (peek) lezen en vergrendelen van het bericht zonder deze te verwijderen uit het abonnement.

Het standaardgedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking, is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing niet verwerken van een bericht tolereren kan wanneer er een fout. Voor meer informatie over dit probleem, u hebt een scenario waarin de consument problemen met de aanvraag ontvangen en vervolgens vastloopt voordat deze wordt verwerkt. Omdat Service Bus kan het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens ontbreekt wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint het het bericht dat voor het vastlopen is verbruikt.

Als de `isPeekLock` parameter is ingesteld op **waar**, wordt de ontvangst een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Wanneer Service Bus een aanvraag ontvangt, deze vindt het volgende bericht te gebruiken, wordt vergrendeld om te voorkomen dat andere consumenten het ontvangen en geeft dit terug aan de toepassing.
Nadat de toepassing het bericht verwerkt (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van **deleteMessage** methode, en wordt het bericht te verwijderen als parameter doorgegeven. De **deleteMessage** methode wordt het bericht gemarkeerd als verbruikt en wordt deze verwijderd uit het abonnement.

Het volgende voorbeeld laat zien hoe berichten kunnen worden ontvangen en verwerkt met behulp `receiveSubscriptionMessage`. Het voorbeeld eerst ontvangt en Hiermee verwijdert u een bericht van het abonnement 'LowMessages' en vervolgens een bericht ontvangt van de 'HighMessages' abonnement met behulp `isPeekLock` is ingesteld op true. Deze vervolgens verwijdert het bericht met `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        })
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de `unlockMessage` methode voor het **ServiceBusService** object. Deze methode zorgt ervoor dat Service Bus het bericht in het abonnement ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen. In dit geval door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is een time-out gekoppeld aan een bericht dat in het abonnement is vergrendeld. Als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld, als de toepassing vastloopt), klikt u vervolgens ontgrendelt Service Bus het bericht automatisch en maakt ze beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `deleteMessage` methode is aangeroepen, wordt het bericht is opnieuw naar de toepassing bezorgd wanneer deze opnieuw wordt opgestart. Dit gedrag wordt vaak genoemd *tenminste één keer worden verwerkt*. Dat wil zeggen dat elk bericht ten minste één keer wordt verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als het scenario niet kan dubbele verwerking tolereren, moet u logica toevoegen aan uw toepassing voor het afhandelen van dubbele berichtbezorging. U kunt de **MessageId** eigenschap van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
Onderwerpen en abonnementen persistent zijn en moeten expliciet worden verwijderd via de [Azure-portal] [ Azure portal] of via een programma.
Het volgende voorbeeld ziet u hoe u wilt verwijderen van het onderwerp met de naam `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. Het volgende voorbeeld ziet u hoe u een abonnement met de naam verwijdert `HighMessages` uit de `MyTopic` onderwerp:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Zie [wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions].
* API-naslaginformatie voor [SqlFilter][SqlFilter].
* Ga naar de [Azure SDK voor Node] [ Azure SDK for Node] -bibliotheek op GitHub.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Een Node.js-toepassing naar een Azure-website maken en implementeren]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

