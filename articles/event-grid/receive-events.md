---
title: Gebeurtenissen ontvangen uit Azure Event raster op een HTTP-eindpunt
description: Hierin wordt beschreven hoe een HTTP-eindpunt valideren vervolgens ontvangen en gebeurtenissen van Azure Event raster deserialiseren
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: article
ms.date: 04/26/2018
ms.author: babanisa
ms.openlocfilehash: db79629c5f806fe50d22200574c29052a485dd06
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="receive-events-to-an-http-endpoint"></a>Gebeurtenissen op een HTTP-eindpunt ontvangen

Dit artikel wordt beschreven hoe u [valideren van een HTTP-eindpunt](security-authentication.md#webhook-event-delivery) gebeurtenissen ontvangen van een abonnement op gebeurtenissen en ontvangen en deserialiseren van gebeurtenissen. Dit artikel wordt een Azure-functie voor demonstratiedoeleinden, maar dezelfde concepten die van toepassing ongeacht waar de toepassing wordt gehost.

> [!NOTE]
> Het is **sterk** aanbevolen dat u een [gebeurtenis raster Trigger](../azure-functions/functions-bindings-event-grid.md) wanneer de activering van een Azure-functie met gebeurtenis raster. Het gebruik van een algemene WebHook-trigger hier is demonstrative.

## <a name="prerequisites"></a>Vereisten

* U moet een functie-app met een [HTTP functie geactiveerd](../azure-functions/functions-create-generic-webhook-triggered-function.md)

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

Als u in .NET ontwikkelt [toevoegen van een afhankelijkheid](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) aan de functie voor de `Microsoft.Azure.EventGrid` [Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.EventGrid). SDK's voor andere talen beschikbaar zijn via de [SDK's publiceren](./sdk-overview.md#data-plane-sdks) verwijzing. Deze pakketten bevatten zoals de modellen voor systeemeigen gebeurtenistypen `EventGridEvent`, `StorageBlobCreatedEventData`, en `EventHubCaptureFileCreatedEventData`.

Klik op de koppeling 'Bestanden weergeven' in uw Azure-functie (de meeste rechterdeelvenster in de Azure functions-portal) en maken van een bestand met de naam project.json. Voeg de volgende inhoud naar de `project.json` -bestand en sla het:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "1.1.0-preview"
      }
    }
   }
}
```

![Toegevoegde NuGet-pakket](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>Validatie van het eindpunt

Het eerste wat u wilt doen is afgehandeld `Microsoft.EventGrid.SubscriptionValidationEvent` gebeurtenissen. Telkens wanneer iemand zich op een gebeurtenis abonneert, raster gebeurtenis een validatiegebeurtenis verzendt naar het eindpunt met een `validationCode` in de nettolading van gegevens. Het eindpunt is vereist voor de echo dit terug in de hoofdtekst van de reactie naar [aantonen dat u het eindpunt geldig is en in eigendom van door u](security-authentication.md#webhook-event-delivery). Als u een [gebeurtenis raster Trigger](../azure-functions/functions-bindings-event-grid.md) in plaats van een WebHook geactiveerd functie, de validatie van het eindpunt is afgehandeld. Als u een API-service van derden (zoals [Zapier](https://zapier.com) of [IFTTT](https://ifttt.com/)), mogelijk niet via programmacode echo code voor de validatie. U kunt handmatig het abonnement via een validatie-URL die wordt verzonden in de validatie-gebeurtenis abonnement voor deze services valideren. Kopieer de URL in de `validationUrl` eigenschap en verzendt een GET-aanvragen via een REST-client of de webbrowser.

Programmatisch echo van code voor de validatie, moet u de volgende code gebruiken:

```csharp
using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{

    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}

```

```javascript
var http = require('http');

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

### <a name="test-validation-response"></a>Validatie antwoord testen

Test de validatiefunctie antwoord door de voorbeeldgebeurtenis in het veld voor de functie plakken:

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

Wanneer u op uitvoeren klikt, de uitvoer moet 200 OK en `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` in de hoofdtekst:

![validatie-antwoord](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>Blob storage gebeurtenissen verwerken

Nu gaan we uitbreiden van de functie voor het afhandelen van `Microsoft.Storage.BlobCreated`:

```cs
using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");

            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}

```

```javascript
var http = require('http');

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

### <a name="test-blob-created-event-handling"></a>Test Blob gemaakt gebeurtenisverwerking

De nieuwe functionaliteit van de functie testen door de gegevens een [Blob storage gebeurtenis](./event-schema-blob-storage.md#example-event) in de testveld en wordt uitgevoerd:

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

U ziet de uitvoer van de blob-URL in het logboek voor de functie:

![Uitvoerlogboek](./media/receive-events/blob-event-response.png)

U kunt ook testen door het maken van een Blob storage-account of algemene doel V2 (GPv2) Storage-account, [toe te voegen en de gebeurtenis abonnement](../storage/blobs/storage-blob-event-quickstart.md), en het instellen van het eindpunt naar de URL van de functie:

![Functie-URL](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Aangepaste gebeurtenissen verwerken

Ten slotte kunt de functie zodra er meer uitbreiden zodat deze kan ook aangepaste gebeurtenissen verwerken. Toevoegen van een selectievakje voor de gebeurtenis `Contoso.Items.ItemReceived`. Uw laatste code moet eruitzien als:

```cs
using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

class ContosoItemReceivedEventData
{
    public string id { get; set; }
    public string message { get; set; }
    public string time { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    const string CustomTopicEvent = "Contoso.Items.ItemReceived";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }

        else if (string.Equals(eventGridEvent.EventType, CustomTopicEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<ContosoItemReceivedEventData>();
            log.Info($"Got ContosoItemReceived event data, item URI {eventData.id}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}

```

```javascript
var http = require('http');
var t = require('tcomb');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";
    var ContosoItemReceivedEventData = t.struct({
        id: t.Str,
        message: t.Str,
        time: t.Str,
    })

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
            var payload = new ContosoItemReceivedEventData(body.data);
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
            context.log(payload instanceof ContosoItemReceivedEventData);
        }
    }
    context.done();
};

```

### <a name="test-custom-event-handling"></a>Test aangepaste gebeurtenissen worden verwerkt

Ten slotte test uw EA functie aankan nu uw aangepaste gebeurtenistype:

```json
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Microsoft.EventGrid.CustomEventType",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "id": "831e1650-001e-001b-66ab-eeb76e069631",
            "message": "Contoso item Foo",
            "time": "2017-06-26T18:41:00.9584103Z"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
```

U kunt ook deze live door functionaliteit testen [een aangepaste gebeurtenis met CURL verzenden via de Portal](./custom-event-quickstart-portal.md) of door [boeken in een eigen onderwerp](./post-to-custom-topic.md) met behulp van een service of toepassing die naar een eindpunt zoals postenkan[Postman](https://www.getpostman.com/). Maak een aangepaste onderwerp en een gebeurtenisabonnement met het eindpunt instellen als de URL van de functie.

## <a name="next-steps"></a>Volgende stappen

* Verken de [Azure Event raster Management en SDK's publiceren](./sdk-overview.md)
* Meer informatie over hoe [posten naar een aangepaste onderwerp](./post-to-custom-topic.md)
* Probeer een van de diepgaande zelfstudies voor de gebeurtenis raster en functies zoals [formaat van afbeeldingen geüpload naar Blob storage](resize-images-on-storage-blob-upload-event.md)
