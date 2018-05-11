---
title: Gebeurtenis raster trigger voor Azure Functions
description: Begrijpen hoe gebeurtenis raster gebeurtenissen in de Azure Functions verwerken.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/26/2018
ms.author: tdykstra
ms.openlocfilehash: f12cdf2fc8a1aa3b7e8bc3c5eeb338601a8f2ffe
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="event-grid-trigger-for-azure-functions"></a>Gebeurtenis raster trigger voor Azure Functions

In dit artikel wordt uitgelegd hoe u voor het afhandelen van [gebeurtenis raster](../event-grid/overview.md) gebeurtenissen in de Azure Functions.

Gebeurtenis raster is een Azure-service waarmee HTTP-aanvragen op de hoogte van gebeurtenissen die worden verzonden *uitgevers*. Een uitgever is de service of de resource die afkomstig is van de gebeurtenis. Een Azure blob storage-account is bijvoorbeeld een uitgever en [een blob uploaden of de verwijdering is een gebeurtenis](../storage/blobs/storage-blob-event-overview.md). Sommige [Azure-services hebben een ingebouwde ondersteuning voor het publiceren van gebeurtenissen naar de gebeurtenis raster](../event-grid/overview.md#event-sources). 

Gebeurtenis *handlers* ontvangen en verwerken van gebeurtenissen. Azure Functions is een van de verschillende [Azure-services die hebben een ingebouwde ondersteuning voor het verwerken van gebeurtenissen van de gebeurtenis raster](../event-grid/overview.md#event-handlers). In dit artikel leert u hoe u een trigger gebeurtenis raster een functie aanroepen wanneer een gebeurtenis wordt ontvangen van de gebeurtenis raster.

Als u liever, kunt u een HTTP-trigger voor het afhandelen van gebeurtenis raster gebeurtenissen; Zie [een HTTP-trigger gebruiken als een gebeurtenis raster trigger](#use-an-http-trigger-as-an-event-grid-trigger) verderop in dit artikel.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pakketten

De gebeurtenis raster trigger is opgegeven de [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet-pakket. De broncode voor het pakket bevindt zich in de [azure-functies-eventgrid-extensie](https://github.com/Azure/azure-functions-eventgrid-extension) GitHub-opslagplaats.

<!--
If you want to bind to the `Microsoft.Azure.EventGrid.Models.EventGridEvent` type instead of `JObject`, install the [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) package.
-->

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke voor een trigger gebeurtenis raster:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

Zie voor een voorbeeld van de trigger HTTP [het gebruik van HTTP-trigger](#use-an-http-trigger-as-an-event-grid-trigger) verderop in dit artikel.

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt gebonden aan `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

<!--
The following example shows a [C# function](functions-dotnet-class-library.md) that binds to `EventGridEvent`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
            public static void EventGridTest([EventGridTrigger] Microsoft.Azure.EventGrid.Models.EventGridEvent eventGridEvent, TraceWriter log)
        {
            log.Info("C# Event Grid function processed a request.");
            log.Info($"Subject: {eventGridEvent.Subject}");
            log.Info($"Time: {eventGridEvent.EventTime}");
            log.Info($"Data: {eventGridEvent.Data.ToString()}");
        }
    }
}
```
-->

Zie voor meer informatie [pakketten](#packages), [kenmerken](#attributes), [configuratie](#configuration), en [gebruik](#usage).

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld ziet u een trigger-binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Hier volgt C# script-code die wordt gebonden aan `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

<!--
Here's C# script code that binds to `EventGridEvent`:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.Azure.WebJobs.Extensions.EventGrid"
#r "Microsoft.Azure.EventGrid"

using Microsoft.Azure.WebJobs.Extensions.EventGrid;
Using Microsoft.Azure.EventGrid.Models;

public static void Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info("C# Event Grid function processed a request.");
    log.Info($"Subject: {eventGridEvent.Subject}");
    log.Info($"Time: {eventGridEvent.EventTime}");
    log.Info($"Data: {eventGridEvent.Data.ToString()}");
}
```
-->

Zie voor meer informatie [pakketten](#packages), [kenmerken](#attributes), [configuratie](#configuration), en [gebruik](#usage).

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u een trigger-binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```
     
## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs) kenmerk.

Hier volgt een `EventGridTrigger` kenmerk in een handtekening voor methode:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, TraceWriter log)
{
    ...
}
 ```

Zie voor een compleet voorbeeld [C#-voorbeeld](#c-example).

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand. Er zijn geen constructorparameters of eigenschappen instellen in de `EventGridTrigger` kenmerk.

|de eigenschap Function.JSON |Beschrijving|
|---------|---------|----------------------|
| **type** | Vereist - moet worden ingesteld op `eventGridTrigger`. |
| **direction** | Vereist - moet worden ingesteld op `in`. |
| **Naam** | Vereist: de naam van de variabele in functiecode gebruikt voor de parameter die gegevens van de gebeurtenis ontvangt. |

## <a name="usage"></a>Gebruik

Voor C# en F # functies, kunt u de volgende parametertypen voor de trigger gebeurtenis raster:

* `JObject`
* `string`
* `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`-Definieert de eigenschappen voor de overeenkomende velden voor alle types van gebeurtenissen. **Dit type is afgeschaft**, maar de vervanging ervan is nog niet gepubliceerd naar NuGet.

Voor JavaScript-functies, de parameter met de naam door de *function.json* `name` eigenschap bevat een verwijzing naar de event-object.

## <a name="event-schema"></a>Gebeurtenisschema

Gegevens voor een gebeurtenis raster gebeurtenis is ontvangen als een JSON-object in de hoofdtekst van een HTTP-aanvraag. De JSON lijkt op het volgende voorbeeld:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Het voorbeeld is een matrix met één element. Gebeurtenis raster altijd verzendt een matrix en meer dan een gebeurtenis in de matrix kan verzenden. De runtime roept de functie eenmaal voor elk matrixelement.

De eigenschappen op het hoogste niveau in de gebeurtenis JSON-gegevens zijn hetzelfde tussen alle gebeurtenistypen, terwijl de inhoud van de `data` eigenschap specifiek zijn voor elk gebeurtenistype. Het voorbeeld is voor een blob storage-gebeurtenis.

Zie voor een uitleg van de algemene en gebeurtenis-specifieke eigenschappen [gebeurteniseigenschappen](../event-grid/event-schema.md#event-properties) in de documentatie van de gebeurtenis raster.

De `EventGridEvent` Hiermee geeft u alleen de eigenschappen die op het hoogste niveau; de `Data` eigenschap is een `JObject`. 

## <a name="create-a-subscription"></a>Een abonnement maken

U start de gebeurtenis raster HTTP-aanvragen ontvangen door een gebeurtenis raster-abonnement waarmee de eindpunt-URL die de functie activeert te maken.

### <a name="azure-portal"></a>Azure Portal

Selecteer voor de functies die u in de Azure portal met de gebeurtenis raster trigger ontwikkelt, **toevoegen gebeurtenis raster abonnement**.

![Abonnement maken in de portal](media/functions-bindings-event-grid/portal-sub-create.png)

De portal wordt geopend wanneer u deze koppeling selecteert, de **maken voor een gebeurtenisabonnement** pagina met de eindpunt-URL automatisch ingevuld.

![De eindpunt-URL automatisch ingevuld](media/functions-bindings-event-grid/endpoint-url.png)

Zie voor meer informatie over het maken van abonnementen met behulp van de Azure-portal [aangepaste gebeurtenis maken - Azure-portal](../event-grid/custom-event-quickstart-portal.md) in de documentatie van de gebeurtenis raster.

### <a name="azure-cli"></a>Azure-CLI

Een abonnement maken met behulp van [de Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), gebruiken de [az eventgrid gebeurtenisabonnement maken](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) opdracht.

De opdracht vereist de eindpunt-URL die de functie activeert. Het volgende voorbeeld ziet u de URL-patroon:

```
https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}
```

De systeemsleutel is een autorisatiesleutel die moet worden opgenomen in de eindpunt-URL voor een trigger gebeurtenis raster. De volgende sectie wordt uitgelegd hoe de systeemsleutel voor het ophalen.

Hier volgt een voorbeeld die zich op een blob storage-account (met een tijdelijke aanduiding voor de systeemsleutel abonneert):

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name glengablobstorage --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://glengastorageevents.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=LUwlnhIsNtSiUjv/sNtSiUjvsNtSiUjvsNtSiUjvYb7XDonDUr/RUg==
```

Zie voor meer informatie over het maken van een abonnement [de blob storage-Quick Start](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) of de andere snelstartgidsen gebeurtenis raster.

### <a name="get-the-system-key"></a>De systeemsleutel ophalen

U kunt de systeemsleutel opgegeven krijgen met de volgende API (HTTP GET):

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={adminkey}
```

Dit is een beheer-API, dus hiervoor uw [administratorsleutel](functions-bindings-http-webhook.md#authorization-keys). Verwar de systeemsleutel (voor het aanroepen van een gebeurtenis raster activeringsfunctie) niet met de beheersleutel (voor het uitvoeren van beheertaken op de functie-app). Wanneer u zich op een gebeurtenis raster onderwerp abonneert, zorg er dan voor dat de systeemsleutel worden gebruikt.

Hier volgt een voorbeeld van het antwoord dat de systeemsleutel opgegeven biedt:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Zie voor meer informatie [autorisatie sleutels](functions-bindings-http-webhook.md#authorization-keys) in het HTTP-trigger verwijzingsartikel. 

U kunt ook verzenden een HTTP PUT waarde van de sleutel zelf opgeven.

## <a name="local-testing-with-requestbin"></a>Lokale testen met RequestBin

> [!NOTE]
> De site RequestBin is momenteel niet beschikbaar, maar u kunt deze methode met https://hookbin.com in plaats daarvan. Als u die site niet actief is, kunt u [ngrok](#local-testing-with-ngrok).

Als u wilt testen van een trigger gebeurtenis raster lokaal hebt om op te halen van de gebeurtenis raster HTTP-aanvragen van hun oorsprong in de cloud naar uw lokale computer verzonden. Een manier om u te doen is door het vastleggen van aanvragen voor online en handmatig opnieuw verzenden van deze op uw lokale machine:

2. [Maak een eindpunt RequestBin](#create-a-RequestBin-endpoint).
3. [Een gebeurtenis raster-abonnement maken](#create-an-event-grid-subscription) die verzendt gebeurtenissen naar het eindpunt RequestBin.
4. [Een aanvraag genereert](#generate-a-request) en de hoofdtekst van de aanvraag van de site RequestBin kopiëren.
5. [De aanvraag handmatig boeken](#manually-post-the-request) naar de localhost-URL van het raster gebeurtenis activeringsfunctie.

Wanneer u klaar bent testen, kunt u hetzelfde abonnement voor productie door het eindpunt bij te werken. Gebruik de [az eventgrid gebeurtenisabonnement update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) Azure CLI-opdracht.

### <a name="create-a-requestbin-endpoint"></a>Een RequestBin-eindpunt maken

RequestBin is een open source-hulpprogramma dat HTTP-aanvragen accepteert en een overzicht van de aanvraagtekst. De http://requestb.in URL speciale behandeling door Azure gebeurtenis raster opgehaald. Om te bevorderen testen, verzendt gebeurtenis raster gebeurtenissen naar de URL RequestBin zonder een juiste reactie op aanvragen voor abonnement-validatie. Een test hulpprogramma krijgt dezelfde behandeling: http://hookbin.com.

RequestBin is niet bedoeld voor gebruik met hoge doorvoer. Als u meer dan een gebeurtenis tegelijk pusht, ziet u mogelijk niet alle gebeurtenissen in het hulpprogramma.

Maak een eindpunt.

![RequestBin-eindpunt maken](media/functions-bindings-event-grid/create-requestbin.png)

Kopieer de eindpunt-URL.

![Kopieer RequestBin eindpunt](media/functions-bindings-event-grid/save-requestbin-url.png)

### <a name="create-an-event-grid-subscription"></a>Een gebeurtenis raster-abonnement maken

Maken van een gebeurtenis raster-abonnement van het type dat u wilt testen en wijs hieraan uw RequestBin-eindpunt. Zie voor meer informatie over het maken van een abonnement [een abonnement maken](#create-a-subscription) eerder in dit artikel.

### <a name="generate-a-request"></a>Een aanvraag genereert

Een gebeurtenis die HTTP-verkeer naar uw eindpunt RequestBin genereren.  Als u een blob storage-abonnement hebt gemaakt, bijvoorbeeld uploaden of verwijderen van een blob. Wanneer een aanvraag wordt weergegeven in uw pagina RequestBin, kopieert u de aanvraagtekst.

De abonnement-validatieaanvraag wordt eerst ontvangen validatie verzoeken negeren en kopieer de gebeurtenis-aanvraag.

![Hoofdtekst van de aanvraag van RequestBin kopiëren](media/functions-bindings-event-grid/copy-request-body.png)

### <a name="manually-post-the-request"></a>De aanvraag handmatig boeken

De functie gebeurtenis raster lokaal uitvoeren.

Gebruik een hulpprogramma zoals [Postman](https://www.getpostman.com/) of [curl](https://curl.haxx.se/docs/httpscripting.html) voor het maken van een HTTP POST-aanvraag:

* Stel een `Content-Type: application/json` header.
* Stel een `aeg-event-type: Notification` header.
* Plak de RequestBin-gegevens in de aanvraagtekst. 
* Post een bericht naar de URL van uw activeringsfunctie gebeurtenis raster met het volgende patroon volgen:

```
http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

De `functionName` parameter moet de naam die is opgegeven in de `FunctionName` kenmerk.

De volgende schermafbeeldingen de kopteksten weergeven en aanvraagtekst in Postman:

![Kopteksten in Postman](media/functions-bindings-event-grid/postman2.png)

![Aanvraagtekst in Postman](media/functions-bindings-event-grid/postman.png)

De gebeurtenis raster trigger-functie wordt uitgevoerd en vergelijkbaar met het volgende voorbeeld ziet u Logboeken:

![Voorbeeld gebeurtenis raster trigger functie Logboeken](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Lokale testen met ngrok

Een gebeurtenis raster trigger lokaal testen op een andere manier is voor het automatiseren van de HTTP-verbinding tussen het Internet en op uw ontwikkelcomputer. Kunt u dat doen met een open source-hulpprogramma met de naam [ngrok](https://ngrok.com/):

3. [Maken van een eindpunt ngrok](#create-an-ngrok-endpoint).
4. [Voer de functie van de trigger gebeurtenis raster](#run-the-event-grid-trigger-function).
5. [Een gebeurtenis raster-abonnement maken](#create-a-subscription) die verzendt gebeurtenissen naar het eindpunt ngrok.
6. [Een gebeurtenis](#trigger-an-event).

Wanneer u klaar bent testen, kunt u hetzelfde abonnement voor productie door het eindpunt bij te werken. Gebruik de [az eventgrid gebeurtenisabonnement update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) Azure CLI-opdracht.

### <a name="create-an-ngrok-endpoint"></a>Een eindpunt ngrok maken

Download *ngrok.exe* van [ngrok](https://ngrok.com/), en worden uitgevoerd met de volgende opdracht:

```
ngrok http -host-header=localhost 7071
```

De - host-headerparameter is nodig omdat de runtime van functions aanvragen van localhost verwacht wanneer deze wordt uitgevoerd op localhost. 7071 is het standaardpoortnummer wanneer de runtime wordt lokaal uitgevoerd.

De opdracht maakt u de volgende uitvoer:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

U gebruikt de https://{subdomain}.ngrok.io-URL voor uw abonnement gebeurtenis raster.

### <a name="run-the-event-grid-trigger-function"></a>De gebeurtenis raster trigger-functie uitvoert

De URL ngrok biedt geen speciale verwerking door gebeurtenis raster krijgt, zodat uw functie moet lokaal worden uitgevoerd wanneer het abonnement is gemaakt. Dit niet het geval is, wordt het antwoord validatie wordt niet verzonden en wordt het maken van het abonnement is mislukt.

### <a name="create-a-subscription"></a>Een abonnement maken

Maken van een gebeurtenis raster-abonnement van het type dat u wilt testen en wijs hieraan uw ngrok-eindpunt met het volgende patroon volgen:

```
https://{subdomain}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

De `functionName` parameter moet de naam die is opgegeven in de `FunctionName` kenmerk.

Hier volgt een voorbeeld met de Azure CLI:

```
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName=EventGridTrigger
```

Zie voor meer informatie over het maken van een abonnement [een abonnement maken](#create-a-subscription) eerder in dit artikel.

### <a name="trigger-an-event"></a>Een gebeurtenis activeren

Een gebeurtenis die HTTP-verkeer naar uw eindpunt ngrok genereren.  Als u een blob storage-abonnement hebt gemaakt, bijvoorbeeld uploaden of verwijderen van een blob.

De gebeurtenis raster trigger-functie wordt uitgevoerd en vergelijkbaar met het volgende voorbeeld ziet u Logboeken:

![Voorbeeld gebeurtenis raster trigger functie Logboeken](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Een HTTP-trigger gebruiken als een trigger gebeurtenis raster

Gebeurtenis raster gebeurtenissen worden ontvangen als HTTP-aanvragen, zodat u gebeurtenissen verwerken kunt met behulp van een HTTP-trigger in plaats van een trigger gebeurtenis raster. Een mogelijke reden voor het uitvoeren moet u meer controle over de eindpunt-URL die de functie activeert. 

Als u een HTTP-trigger gebruikt, hebt u code schrijven voor wat de trigger gebeurtenis raster automatisch doet:

* Stuurt een antwoord validatie naar een [abonnement validatieaanvraag](../event-grid/security-authentication.md#webhook-event-delivery).
* Hiermee wordt de functie eenmaal per element van de matrix gebeurtenis in de aanvraagtekst.

De volgende C#-code voor een HTTP-trigger simuleert gebeurtenis raster trigger gedrag:

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"], 
        "Microsoft.EventGrid.SubscriptionValidationEvent", 
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.Info("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.Info($"Subject: {eventGridEvent.Subject}");
        log.Info($"Time: {eventGridEvent.EventTime}");
        log.Info($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

De volgende JavaScript-code voor een HTTP-trigger simuleert gebeurtenis raster trigger gedrag:

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        context.res = { status: 200, body: messages[0].data.validationCode }
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

Uw code gebeurtenisafhandeling gaat binnen de lus via de `messages` matrix.

Zie voor meer informatie over de URL moet worden gebruikt voor het aanroepen van de functie lokaal of wanneer deze wordt uitgevoerd in Azure de [naslagdocumentatie voor HTTP-trigger-binding](functions-bindings-http-webhook.md) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Meer informatie over de gebeurtenis raster](../event-grid/overview.md)
