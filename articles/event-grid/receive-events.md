---
title: Gebeurtenissen ontvangen van Azure Event Grid naar een HTTP-eindpunt
description: Beschrijft hoe u een HTTP-eindpunt, valideren en vervolgens ontvangen en deserialisatie van gebeurtenissen uit Azure Event Grid
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: babanisa
ms.openlocfilehash: bb82ef542db09b3b7f864c4901107e1c5c0827f6
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464767"
---
# <a name="receive-events-to-an-http-endpoint"></a>Gebeurtenissen op een HTTP-eindpunt ontvangen

Dit artikel wordt beschreven hoe u [valideren van een HTTP-eindpunt](security-authentication.md#webhook-event-delivery) gebeurtenissen ontvangen van een gebeurtenisabonnement en ontvangen en deserialisatie van gebeurtenissen. In dit artikel wordt een Azure-functie voor demonstratiedoeleinden te gebruiken, maar dezelfde concepten worden toegepast, ongeacht waar de toepassing wordt gehost.

> [!NOTE]
> Het is **sterk** aanbevolen dat u een [Trigger Gebeurtenisraster](../azure-functions/functions-bindings-event-grid.md) bij het activeren van een Azure-functie met Event Grid. Het gebruik van een algemene WebHook-trigger hier is ter demonstratie.

## <a name="prerequisites"></a>Vereisten

U moet een functie-app met een door HTTP geactiveerde functie.

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

Als u in .NET ontwikkelt, [een afhankelijkheid toevoegen](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) naar de functie voor de `Microsoft.Azure.EventGrid` [Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.EventGrid). De voorbeelden in dit artikel moet versie 1.4.0 of hoger.

SDK's voor andere talen zijn beschikbaar via de [SDK's publiceren](./sdk-overview.md#data-plane-sdks) verwijzing. Deze pakketten hebben, zoals de modellen voor systeemeigen gebeurtenistypen `EventGridEvent`, `StorageBlobCreatedEventData`, en `EventHubCaptureFileCreatedEventData`.

Klik op de koppeling 'Bestanden weergeven' in uw Azure-functie (de meeste rechterdeelvenster in de Azure functions-portal) en maken van een bestand met de naam van project.json. Voeg de volgende inhoud in de `project.json` bestand en sla het bestand:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "2.0.0"
      }
    }
   }
}
```

![Toegevoegde NuGet-pakket](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>De eindpuntvalidatie

Het eerste wat u wilt doen is verwerkt `Microsoft.EventGrid.SubscriptionValidationEvent` gebeurtenissen. Telkens wanneer iemand zich op een gebeurtenis abonneert, Event Grid een validatiegebeurtenis verzonden naar het eindpunt met een `validationCode` in de nettolading van de gegevens. Het eindpunt is vereist voor het echo dit terug in de hoofdtekst van het antwoord op [het eindpunt geldig is en u eigendom bewijzen](security-authentication.md#webhook-event-delivery). Als u een [Trigger Gebeurtenisraster](../azure-functions/functions-bindings-event-grid.md) in plaats van een WebHook geactiveerde functie, wordt de eindpuntvalidatie voor u afgehandeld. Als u een API-service van derden (zoals [Zapier](https://zapier.com) of [IFTTT](https://ifttt.com/)), mogelijk niet via een programma echo de code voor validatie. U kunt handmatig het abonnement met behulp van een validatie-URL die wordt verzonden in de gebeurtenis van de validatie van abonnement voor deze services valideren. Kopieer de URL in de `validationUrl` eigenschap en verzendt een GET-aanvragen via een REST-client of uw webbrowser.

Handmatige validatie is in preview. Als u de functie wilt gebruiken, moet u de [Event Grid-extensie](/cli/azure/azure-cli-extensions-list) voor de [Azure CLI](/cli/azure/install-azure-cli) installeren. U kunt deze installeren met `az extension add --name eventgrid`. Als u de REST-API, zorg ervoor dat u `api-version=2018-05-01-preview`.

In C#, de `DeserializeEventGridEvents()` functie gedeserialiseerd de Event Grid-gebeurtenissen. Deze gegevens van de gebeurtenis in het juiste type, zoals StorageBlobCreatedEventData gedeserialiseerd. Gebruik de `Microsoft.Azure.EventGrid.EventTypes` klasse om ondersteunde gebeurtenistypen en namen te verkrijgen.

Programmatisch echo code voor de validatie, gebruik de volgende code. Vindt u voorbeelden op [Event Grid-consumentvoorbeelden](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridConsumer).

```csharp
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }
    }
    context.done();
};
```

### <a name="test-validation-response"></a>Validatie-antwoord testen

De validatie van antwoord-functie testen door de voorbeeldgebeurtenis in het testveld voor de functie te plakken:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Wanneer u klikt op uitvoeren, de uitvoer moet 200 OK en `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` in de hoofdtekst:

![validatie-antwoord](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>Verwerken van gebeurtenissen van Blob storage

Nu gaan we de functie voor het afhandelen van uitbreiden `Microsoft.Storage.BlobCreated`:

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }
    }
    context.done();
};

```

### <a name="test-blob-created-event-handling"></a>Gebeurtenisverwerking voor test-Blob gemaakt

De nieuwe functionaliteit van de functie testen door een [Blob storage-gebeurtenis](./event-schema-blob-storage.md#example-event) in het testveld en uitvoeren:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

U ziet de uitvoer van de blob-URL in het logboek van de functie:

![Uitvoerlogboek](./media/receive-events/blob-event-response.png)

U kunt ook testen door het maken van een Blob storage-account of General Purpose V2 (GPv2) Storage-account, [toe te voegen en dit gebeurtenisabonnement](../storage/blobs/storage-blob-event-quickstart.md), en het instellen van het eindpunt voor de functie-URL:

![Functie-URL](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Aangepaste gebeurtenissen verwerken

Ten slotte kunt de functie zodra er meer uitbreiden zodat deze kan ook aangepaste gebeurtenissen verwerken. 

In C#, de SDK biedt ondersteuning voor een naam van het type gebeurtenis toe te wijzen aan het gegevenstype van de gebeurtenis. Gebruik de `AddOrUpdateCustomEventMapping()` functie voor het toewijzen van het aangepaste gebeurtenis.

Toevoegen van een controle voor uw gebeurtenis `Contoso.Items.ItemReceived`. Uw laatste code moet uitzien:

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

class ContosoItemReceivedEventData
{
    [JsonProperty(PropertyName = "itemSku")]
    public string ItemSku { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();
    eventGridSubscriber.AddOrUpdateCustomEventMapping("Contoso.Items.ItemReceived", typeof(ContosoItemReceivedEventData));
    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
        else if (eventGridEvent.Data is ContosoItemReceivedEventData)
        {
            var eventData = (ContosoItemReceivedEventData)eventGridEvent.Data;
            log.Info($"Got ContosoItemReceived event data, item SKU {eventData.ItemSku}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }

        else if (body.data && body.eventType == customEventType) {
            var payload = body.data;
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
        }
    }
    context.done();
};
```

### <a name="test-custom-event-handling"></a>Test aangepaste gebeurtenis verwerken

Ten slotte testen dat door uw functie nu uw type aangepaste gebeurtenis kan worden verwerkt:

```json
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Contoso.Items.ItemReceived",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "itemSku": "Standard"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
```

U kunt ook deze live door functionaliteit testen [verzenden van een aangepaste gebeurtenis met CURL vanuit de Portal](./custom-event-quickstart-portal.md) of door [plaatsen op een aangepast onderwerp](./post-to-custom-topic.md) met behulp van een service of toepassing die u kunt naar een eindpunt, zoals posten[Postman](https://www.getpostman.com/). Een aangepast onderwerp en een gebeurtenisabonnement maken met het eindpunt is ingesteld als de functie-URL.

## <a name="next-steps"></a>Volgende stappen

* Verken de [Azure Event Grid Mangement en SDK's publiceren](./sdk-overview.md)
* Meer informatie over het [plaatsen op een aangepast onderwerp](./post-to-custom-topic.md)
* Probeer een van de uitgebreide zelfstudies voor Event Grid en functies zoals [afbeeldingen aanpast geüpload naar Blob-opslag](resize-images-on-storage-blob-upload-event.md)
