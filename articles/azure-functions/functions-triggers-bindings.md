---
title: Triggers en bindingen in de Azure-functies
description: Informatie over het gebruik van triggers en bindingen in de Azure Functions verbinding maken met de uitvoering van uw code online gebeurtenissen en cloudservices.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: a122271b5fdffd9db33a7dca5908e15f002041d7
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions-triggers en bindingen concepten

Dit artikel is een conceptueel overzicht van triggers en bindingen in de Azure Functions. Functies die gemeenschappelijk voor alle bindingen en alle ondersteunde talen zijn worden hier beschreven.

## <a name="overview"></a>Overzicht

Een *trigger* definieert hoe een functie wordt aangeroepen. Een functie moet exact één trigger hebben. Triggers hebt gekoppeld aan gegevens, die is meestal de nettolading waarmee de functie is geactiveerd.

Invoer en uitvoer *bindingen* bieden een declaratieve manier verbinding maken met gegevens vanuit uw code. Bindingen zijn optioneel en een functie kunt meerdere invoer en uitvoer bindingen. 

Triggers en bindingen kunnen u de details van de services die u met werkt voor hardcoderen voorkomen. U functie ontvangt gegevens (bijvoorbeeld de inhoud van een wachtrijbericht) in functieparameters. Verzenden van gegevens (bijvoorbeeld voor het maken van een wachtrijbericht) met behulp van de geretourneerde waarde van de functie een `out` parameter, of een [object collector](functions-reference-csharp.md#writing-multiple-output-values).

Als u functies ontwikkelen met behulp van de Azure-portal, triggers en bindingen zijn geconfigureerd in een *function.json* bestand. De portal biedt een gebruikersinterface voor deze configuratie, maar u kunt het bestand bewerken rechtstreeks door te wijzigen in de **geavanceerde editor**.

Wanneer u functies ontwikkelen met behulp van Visual Studio voor het maken van een class-bibliotheek, configureert u triggers en bindingen door versieren methoden en parameters met kenmerken.

## <a name="supported-bindings"></a>Ondersteunde bindingen

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Zie voor informatie over welke bindingen zijn Preview-versie of voor gebruik in productieomgevingen zijn goedgekeurd, [ondersteunde talen](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Voorbeeld: wachtrij trigger en tabel uitvoer binding

Stel dat u wilt een nieuwe rij naar Azure Table storage schrijven telkens wanneer een nieuw bericht wordt weergegeven in Azure Queue storage. Dit scenario kan worden geïmplementeerd met behulp van een Azure Queue storage trigger en een Azure-tabelopslag uitvoer binding. 

Hier volgt een *function.json* -bestand voor dit scenario. 

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

Het eerste element in de `bindings` matrix is de trigger van Queue storage. De `type` en `direction` eigenschappen geven de trigger. De `name` eigenschap identificeert de functieparameter die de inhoud van het wachtrij-bericht ontvangen. De naam van de wachtrij voor het bewaken van `queueName`, en de verbindingsreeks in de app-instelling geïdentificeerd door `connection`.

Het tweede element in de `bindings` matrix is de Azure-tabelopslag uitvoer van de binding. De `type` en `direction` eigenschappen geven de binding. De `name` -eigenschap geeft u op hoe de functie biedt de nieuwe rij van de tabel in dit geval met behulp van de geretourneerde waarde van de functie. De naam van de tabel wordt `tableName`, en de verbindingsreeks in de app-instelling geïdentificeerd door `connection`.

Weergeven en bewerken van de inhoud van *function.json* in de Azure-portal klikt u op de **geavanceerde editor** kiezen op de **integreren** tabblad van de functie.

> [!NOTE]
> De waarde van `connection` is de naam van een app-instelling met de verbindingsreeks, niet de verbindingsreeks zelf. Bindingen verbinding gebruiken tekenreeksen die zijn opgeslagen in de instellingen van de app om af te dwingen de beste praktijk die *function.json* bevat geen geheimen van de service.

Hier volgt een C#-scriptcode die geschikt is voor deze trigger en binding. U ziet dat de naam van de parameter waarmee de inhoud van het wachtrij-bericht is `order`; deze naam is vereist omdat de `name` eigenschapwaarde in *function.json* is`order` 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
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

Het bestand met dezelfde function.json kan worden gebruikt met een JavaScript-functie:

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

In een class-bibliotheek, dezelfde trigger gebruikt en de bindingsgegevens &mdash; wachtrij- en tabelnamen, storage-accounts, werken de parameters voor invoer en uitvoer &mdash; wordt geleverd door de kenmerken:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Richting van de binding

Alle triggers en bindingen hebben een `direction` eigenschap in de *function.json* bestand:

- Voor triggers is de richting altijd`in`
- Invoer- en uitvoergegevens bindingen gebruiken `in` en`out`
- Sommige bindingen ondersteuning voor een speciale richting `inout`. Als u `inout`, alleen de **geavanceerde editor** is beschikbaar in de **integreren** tabblad.

Als u werkt met [kenmerken in een class-bibliotheek](functions-dotnet-class-library.md) voor het configureren van triggers en bindingen, de richting is opgegeven in een kenmerkconstructor of afgeleid van het parametertype.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Het retourtype van functie gebruiken om te retourneren van één uitvoer

Het vorige voorbeeld laat zien hoe de geretourneerde waarde van de functie gebruiken voor uitvoer naar een binding die is opgegeven in *function.json* met behulp van de speciale waarde `$return` voor de `name` eigenschap. (Dit wordt alleen ondersteund in de talen die u een retourwaarde, zoals C# script, JavaScript en F hebt #.) Als een functie meerdere bindingen van de uitvoer heeft, gebruikt u `$return` voor slechts één van de uitvoer-bindingen. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

De voorbeelden hieronder tonen retourneren hoe typen worden gebruikt met bindingen in C#-script, JavaScript en F # uitvoer.

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

Gebruik de parameter van het type voor het definiëren van het gegevenstype voor invoergegevens in .NET. Gebruik bijvoorbeeld `string` verbinding maken met de tekst van een wachtrij worden geactiveerd, een bytematrix lezen als binaire bestanden en een aangepast type voor het deserialiseren van een POCO-object.

Voor de talen die dynamisch worden getypeerd zoals JavaScript, gebruikt u de `dataType` eigenschap in de *function.json* bestand. Bijvoorbeeld, om te lezen van de inhoud van een HTTP-aanvraag in binaire indeling, stelt `dataType` naar `binary`:

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

U kunt dezelfde manier gebruiken in de klassenbibliotheken van de:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-metadata-properties"></a>Eigenschappen van de trigger-metagegevens

Naast de nettolading van de gegevens die is geleverd door een trigger (zoals een bericht in de wachtrij waarmee een functie is geactiveerd), bieden veel triggers aanvullende metagegevenswaarden. Deze waarden kunnen worden gebruikt als de invoerparameters in C# en F # of eigenschappen op de `context.bindings` -object in JavaScript. 

Een Azure Queue storage-trigger ondersteunt bijvoorbeeld de volgende eigenschappen:

* QueueTrigger - activering van de inhoud van het bericht als een geldige tekenreeks
* DequeueCount
* expirationTime
* Id
* InsertionTime
* NextVisibleTime
* PopReceipt

De metagegevenswaarden van deze zijn beschikbaar in *function.json* bestandseigenschappen. Stel dat u een trigger wachtrij gebruiken en het bericht uit de wachtrij bevat de naam van een blob die u wilt lezen. In de *function.json* -bestand, kunt u `queueTrigger` metagegevenseigenschap in de blob `path` eigenschap, zoals in het volgende voorbeeld:

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

Details van de eigenschappen van de metagegevens voor elke trigger worden beschreven in het bijbehorende verwijzingsartikel. Zie voor een voorbeeld [wachtrij trigger metagegevens](functions-bindings-storage-queue.md#trigger---message-metadata). Documentatie is ook beschikbaar in de **integreren** tabblad van de portal in de **documentatie** hieronder de configuratiegebied binding.  

## <a name="binding-expressions-and-patterns"></a>Expressies voor gegevensbinding en patronen

Een van de meest krachtige functies van triggers en bindingen is *bindingsexpressies*. U kunt in de configuratie voor een binding patroon expressies die vervolgens kunnen worden gebruikt in andere bindingen of uw code definiëren. Metagegevens van de trigger kan ook worden gebruikt in expressies voor gegevensbinding, zoals wordt weergegeven in de vorige sectie.

Stel bijvoorbeeld dat u wilt dat het formaat van afbeeldingen in een bepaalde blob storage-container, vergelijkbaar met de **Image-aanwijzer Formaat** sjabloon in de **nieuwe functie** pagina van de Azure-portal (Zie de **voorbeelden**  scenario). 

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

U ziet dat de `filename` parameter wordt gebruikt in zowel de definitie van de blob-trigger en de blob uitvoer van de binding. Deze parameter kan ook worden gebruikt in de functiecode.

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

Dezelfde mogelijkheid om het gebruik van de expressies voor gegevensbinding en patronen is van toepassing op kenmerken in de klassenbibliotheken van de. Dit is bijvoorbeeld een installatiekopie van het formaat van de functie in een class-bibliotheek:

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```

### <a name="create-guids"></a>GUID's maken

De `{rand-guid}` bindende expressie maakt een GUID. Het volgende voorbeeld wordt een GUID voor het maken van een unieke blob-naam: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Huidige tijd

De expressie voor gegevensbinding `DateTime` wordt omgezet naar `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties"></a>Verbinding maken met aangepaste eigenschappen voor de invoer

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

In C# en andere .NET-talen, kunt u een patroon imperatieve binding in plaats van de declaratieve bindingen in *function.json* en kenmerken. Imperatieve binding is handig wanneer bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Zie voor meer informatie, [Binding tijdens runtime via imperatieve bindingen](functions-reference-csharp.md#imperative-bindings) in de C# naslaginformatie voor ontwikkelaars.

## <a name="functionjson-file-schema"></a>Function.JSON bestand schema

De *function.json* bestand schema is beschikbaar op [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over een specifieke binding:

- [HTTP en webhooks](functions-bindings-http-webhook.md)
- [Timer](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Event Hub](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Extern bestand](functions-bindings-external-file.md)
