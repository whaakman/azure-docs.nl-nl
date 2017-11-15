---
title: Werken met triggers en bindingen in de Azure Functions | Microsoft Docs
description: Informatie over het gebruik van triggers en bindingen in de Azure Functions verbinding maken met de uitvoering van uw code online gebeurtenissen en cloudservices.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: 7d22a6749216486de6132a6d39e2dcf683d0e678
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions-triggers en bindingen concepten
Azure Functions, kunt u code te schrijven in reactie op gebeurtenissen in Azure en andere services via *triggers* en *bindingen*. Dit artikel is een conceptueel overzicht van triggers en bindingen voor alle programmeertalen worden ondersteund. Functies die gemeenschappelijk voor alle bindingen zijn worden hier beschreven.

## <a name="overview"></a>Overzicht

Triggers en bindingen zijn een declaratieve manier om te definiëren hoe een functie is aangeroepen en wat dit proces met werkt gegevens. Een *trigger* definieert hoe een functie wordt aangeroepen. Een functie moet exact één trigger hebben. Triggers hebt gekoppeld aan gegevens, die is meestal de nettolading waarmee de functie is geactiveerd. 

Invoer en uitvoer *bindingen* bieden een declaratieve manier verbinding maken met gegevens vanuit uw code. Net als de triggers, geeft u verbindingsreeksen en andere eigenschappen in de configuratie van de functie. Bindingen zijn optioneel en een functie kunt meerdere invoer en uitvoer bindingen. 

Met behulp van triggers en bindingen kunt u code schrijven die meer algemeen en heeft geen hardcode de details van de services met waarmee deze communiceert. Gegevens die afkomstig zijn van de invoerwaarden services simpelweg zijn voor uw functiecode. Gebruik de retourwaarde van de methode om de uitvoer van gegevens op een andere service (zoals het maken van een nieuwe rij in de Azure Table Storage). Of als u uitvoer van meerdere waarden moet, gebruikt u een helperobject. Triggers en bindingen hebben een **naam** eigenschap, een id die u in uw code gebruiken voor toegang tot de binding.

U kunt configureren, triggers en bindingen in de **integreren** tabblad in de Azure Functions-portal. Onder de behandelt de gebruikersinterface een bestand met de naam wijzigt *function.json* bestand in de map van de functie. U kunt dit bestand bewerken door te wijzigen in de **geavanceerde editor**.

De volgende tabel toont de triggers en bindingen die worden ondersteund door Azure Functions. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Voorbeeld: wachtrij trigger en tabel uitvoer binding

Stel dat u een nieuwe rij naar Azure Table Storage schrijven telkens wanneer een nieuw bericht wordt weergegeven in Azure Queue Storage. Dit scenario kan worden geïmplementeerd met behulp van een Azure-wachtrij trigger en een Azure-tabelopslag uitvoer binding. 

Een trigger Azure Queue Storage vereist de volgende gegevens in de **integreren** tabblad:

* De naam van de app-instelling met de verbindingsreeks voor Azure Storage-account voor de Azure Queue Storage
* Naam van de wachtrij
* De id in uw code te lezen van de inhoud van het bericht uit de wachtrij, zoals `order`.

Voor het schrijven naar Azure Table Storage, gebruikt u een uitvoer-binding met de volgende details:

* De naam van de app-instelling met de verbindingsreeks voor Azure Storage-account voor de Azure-tabelopslag
* Naam van de tabel
* De id in uw code maken Uitvoeritems, of de retourwaarde van de functie.

Bindingen gebruik verbindingsreeksen met waarden die zijn opgeslagen in de instellingen van de app om af te dwingen de beste praktijk die *function.json* geen service geheimen en in plaats daarvan gewoon bevatten de namen van de app-instellingen.

Vervolgens gebruikt u de id's die u hebt opgegeven om te integreren met Azure Storage in uw code.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Hier volgt de *function.json* die overeenkomt met de bovenstaande code. Houd er rekening mee dat dezelfde configuratie kan worden gebruikt, ongeacht de taal van de functie-implementatie.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
Weergeven en bewerken van de inhoud van *function.json* in de Azure-portal klikt u op de **geavanceerde editor** kiezen op de **integreren** tabblad van de functie.

Zie voor meer voorbeelden van code en meer informatie over de integratie met Azure Storage [Azure Functions triggers en bindingen voor Azure Storage](functions-bindings-storage.md).

### <a name="binding-direction"></a>Richting van de binding

Alle triggers en bindingen hebben een `direction` eigenschap:

- Voor triggers is de richting altijd`in`
- Invoer- en uitvoergegevens bindingen gebruiken `in` en`out`
- Sommige bindingen ondersteuning voor een speciale richting `inout`. Als u `inout`, alleen de **geavanceerde editor** is beschikbaar in de **integreren** tabblad.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Het retourtype van functie gebruiken om te retourneren van één uitvoer

Het vorige voorbeeld laat zien hoe de geretourneerde waarde van de functie gebruiken voor uitvoer naar een binding die wordt bereikt door middel van de parameter voor de speciale `$return`. (Dit wordt alleen ondersteund in de talen die u een retourwaarde, zoals C#, JavaScript en F hebt #.) Als een functie meerdere bindingen van de uitvoer heeft, gebruikt u `$return` voor slechts één van de uitvoer-bindingen. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

De voorbeelden hieronder tonen retourneren hoe typen worden gebruikt met bindingen in C#, JavaScript en F # uitvoer.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>De eigenschap dataType binding

Gebruik in .NET de typen voor het definiëren van het gegevenstype voor invoergegevens. Gebruik bijvoorbeeld `string` verbinding maken met de tekst van een wachtrij worden geactiveerd, een bytematrix lezen als binaire bestanden en een aangepast type voor het deserialiseren van een POCO-object.

Voor de talen die dynamisch worden getypeerd zoals JavaScript, gebruikt u de `dataType` eigenschap in de definitie van de binding. Gebruik bijvoorbeeld om te lezen van de inhoud van een HTTP-aanvraag in binaire indeling, het type `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andere opties voor `dataType` zijn `stream` en `string`.

## <a name="resolving-app-settings"></a>Het omzetten van app-instellingen
Als een best practice moeten geheimen en verbindingsreeksen worden beheerd met behulp van app-instellingen, in plaats van configuratiebestanden. Dit beperkt de toegang tot deze geheime gegevens en wordt het veilig om op te slaan *function.json* in een openbare resourcebeheerbibliotheek.

App-instellingen zijn ook nuttig wanneer u configuratie wilt wijzigen op basis van de omgeving. In een testomgeving kunt u bijvoorbeeld voor het bewaken van een andere wachtrij of blob storage-container.

Appinstellingen zijn opgelost wanneer een waarde procenttekens, zoals tussen `%MyAppSetting%`. Houd er rekening mee dat de `connection` eigenschap van triggers en bindingen is een speciaal geval en -waarden als de app-instellingen automatisch worden opgelost. 

Het volgende voorbeeld wordt een Azure Queue Storage-trigger die gebruikmaakt van een app-instelling `%input-queue-name%` voor het definiëren van de wachtrij voor het activeren van op.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a>Eigenschappen van de trigger-metagegevens

Naast de nettolading van de gegevens die is geleverd door een trigger (zoals een bericht in de wachtrij waarmee een functie is geactiveerd), bieden veel triggers aanvullende metagegevenswaarden. Deze waarden kunnen worden gebruikt als de invoerparameters in C# en F # of eigenschappen op de `context.bindings` -object in JavaScript. 

Een Azure-Opslagwachtrij-trigger ondersteunt bijvoorbeeld de volgende eigenschappen:

* QueueTrigger - activering van de inhoud van het bericht als een geldige tekenreeks
* DequeueCount
* expirationTime
* Id
* InsertionTime
* NextVisibleTime
* PopReceipt

Details van de eigenschappen van de metagegevens voor elke trigger worden beschreven in het bijbehorende naslagonderwerp. Documentatie is ook beschikbaar in de **integreren** tabblad van de portal in de **documentatie** hieronder de configuratiegebied binding.  

Bijvoorbeeld, aangezien blob triggers sommige vertragingen hebben, kunt u een wachtrij-trigger voor het uitvoeren uw functie (Zie [Blob Storage Trigger](functions-bindings-storage-blob.md#blob-storage-trigger)). Bericht uit de wachtrij kan de blob filename activeren op zou bevatten. Met behulp van de `queueTrigger` metagegevenseigenschap, kunt u dit gedrag in uw configuratie, in plaats van uw code.

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Eigenschappen van de metagegevens van een trigger kunnen ook worden gebruikt een *bindende expressie* voor een andere binding, zoals beschreven in de volgende sectie.

## <a name="binding-expressions-and-patterns"></a>Expressies voor gegevensbinding en patronen

Een van de meest krachtige functies van triggers en bindingen is *bindingsexpressies*. U kunt patroon expressies die vervolgens kunnen worden gebruikt binnen uw binding definiëren in andere bindingen of uw code. Metagegevens van de trigger kan ook worden gebruikt in expressies binding als weergeven in het voorbeeld in de vorige sectie.

Stel bijvoorbeeld dat u wilt dat het formaat van afbeeldingen in bepaalde blob storage-container, vergelijkbaar met de **Image-aanwijzer Formaat** sjabloon in de **nieuwe functie** pagina. Ga naar **nieuwe functie** -> taal **C#** -> Scenario **voorbeelden** -> **ImageResizer CSharp**. 

Hier volgt de *function.json* definitie:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

U ziet dat de `filename` parameter wordt gebruikt in zowel de definitie van de blob, evenals de blob uitvoer van de binding. Deze parameter kan ook worden gebruikt in de functiecode.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a>Willekeurige GUID 's
Azure Functions bevat een gemak-syntaxis voor het genereren van GUID's in uw bindingen via de `{rand-guid}` bindende expressie. Het volgende voorbeeld wordt dit gebruikt voor het genereren van een unieke blob-naam: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Huidige tijd

U kunt de expressie voor gegevensbinding `DateTime`, die wordt omgezet naar `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a>Verbinding maken met aangepaste eigenschappen voor de invoer in een expressie voor gegevensbinding

Bindingsexpressies kan ook verwijzen naar eigenschappen die zijn gedefinieerd in de nettolading van de trigger. U wilt bijvoorbeeld dynamisch binding met een blob storage-bestand uit een bestandsnaam is opgegeven in een webhook.

Bijvoorbeeld de volgende *function.json* maakt gebruik van een eigenschap genaamd `BlobName` van de nettolading van de trigger:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

U kunt dit doen in C# en F #, moet u een POCO waarin de velden die zullen worden gedeserialiseerd in de nettolading van de trigger definiëren.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

JSON-deserialisatie wordt automatisch uitgevoerd in JavaScript, en kunt u de eigenschappen rechtstreeks gebruiken.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Configureren van die gegevens bindt tijdens runtime

In C# en andere .NET-talen, kunt u een patroon imperatieve binding in plaats van de declaratieve bindingen in *function.json*. Imperatieve binding is handig wanneer bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Zie voor meer informatie, [Binding tijdens runtime via imperatieve bindingen](functions-reference-csharp.md#imperative-bindings) in de C# naslaginformatie voor ontwikkelaars.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over een specifieke binding:

- [HTTP en webhooks](functions-bindings-http-webhook.md)
- [Timer](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Event Hub](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-documentdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Extern bestand](functions-bindings-external-file.md)
