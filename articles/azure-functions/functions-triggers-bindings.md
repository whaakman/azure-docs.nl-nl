---
title: Triggers en bindingen in de Azure-functies
description: Informatie over het gebruik van triggers en bindingen in de Azure Functions verbinding maken met de uitvoering van uw code online gebeurtenissen en cloudservices.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/24/2018
ms.author: tdykstra
ms.openlocfilehash: 305f7a54e290b8628401c21f033f8be7017d4a91
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083862"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions-triggers en bindingen concepten

Dit artikel is een conceptueel overzicht van triggers en bindingen in de Azure Functions. Functies die gemeenschappelijk voor alle bindingen en alle ondersteunde talen zijn worden hier beschreven.

## <a name="overview"></a>Overzicht

Een *trigger* definieert hoe een functie wordt aangeroepen. Een functie moet exact één trigger hebben. Triggers hebt gekoppeld aan gegevens, die is meestal de nettolading waarmee de functie is geactiveerd.

Invoer en uitvoer *bindingen* bieden een declaratieve manier verbinding maken met gegevens vanuit uw code. Bindingen zijn optioneel en een functie kunt meerdere invoer en uitvoer bindingen. 

Triggers en bindingen kunnen u de details van de services die u met werkt voor hardcoderen voorkomen. De functie ontvangt gegevens (bijvoorbeeld de inhoud van een wachtrijbericht) in functieparameters. Verzenden van gegevens (bijvoorbeeld voor het maken van een wachtrijbericht) met behulp van de geretourneerde waarde van de functie een `out` parameter, of een [object collector](functions-reference-csharp.md#writing-multiple-output-values).

Als u functies ontwikkelen met behulp van de Azure-portal, triggers en bindingen zijn geconfigureerd in een *function.json* bestand. De portal biedt een gebruikersinterface voor deze configuratie, maar u kunt het bestand bewerken rechtstreeks door te wijzigen in de **geavanceerde editor**.

Wanneer u functies ontwikkelen met behulp van Visual Studio voor het maken van een class-bibliotheek, configureert u triggers en bindingen door versieren methoden en parameters met kenmerken.

## <a name="example-trigger-and-binding"></a>Voorbeeld van de trigger en binding

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

Het eerste element in de `bindings` matrix is de trigger van Queue storage. De `type` en `direction` eigenschappen geven de trigger. De `name` eigenschap identificeert de functieparameter die de inhoud van het wachtrij-bericht ontvangt. De naam van de wachtrij voor het bewaken van `queueName`, en de verbindingsreeks in de app-instelling geïdentificeerd door `connection`.

Het tweede element in de `bindings` matrix is de Azure-tabelopslag uitvoer van de binding. De `type` en `direction` eigenschappen geven de binding. De `name` -eigenschap geeft u op hoe de functie zorgt voor de nieuwe rij in de tabel, met behulp van de functie wordt in dit geval waarde retourneren. De naam van de tabel wordt `tableName`, en de verbindingsreeks in de app-instelling geïdentificeerd door `connection`.

Weergeven en bewerken van de inhoud van *function.json* in de Azure-portal klikt u op de **geavanceerde editor** kiezen op de **integreren** tabblad van de functie.

> [!NOTE]
> De waarde van `connection` is de naam van een app-instelling met de verbindingsreeks, niet de verbindingsreeks zelf. Bindingen verbinding gebruiken tekenreeksen die zijn opgeslagen in de instellingen van de app om af te dwingen de beste praktijk die *function.json* bevat geen geheimen van de service.

Hier volgt een C#-scriptcode die geschikt is voor deze trigger en binding. U ziet dat de naam van de parameter waarmee de inhoud van het wachtrij-bericht is `order`; deze naam is vereist omdat de `name` eigenschapwaarde in *function.json* is `order` 

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

In een class-bibliotheek, dezelfde trigger gebruikt en de bindingsgegevens &mdash; wachtrij- en tabelnamen, storage-accounts, werken de parameters voor invoer en uitvoer &mdash; wordt geleverd door de kenmerken in plaats van een bestand function.json. Hier volgt een voorbeeld:

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

## <a name="supported-bindings"></a>Ondersteunde bindingen

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Zie voor informatie over welke bindingen zijn Preview-versie of voor gebruik in productieomgevingen zijn goedgekeurd, [ondersteunde talen](supported-languages.md).

## <a name="register-binding-extensions"></a>Binding extensies registreren

In sommige ontwikkelomgevingen u moet expliciet *registreren* een binding die u wilt gebruiken. Binding uitbreidingen beschikbaar zijn in NuGet-pakketten en voor het registreren van een uitbreiding die u installeert een pakket. De volgende tabel geeft aan wanneer en hoe u binding extensies registreren.

|Ontwikkelomgeving |Registratie<br/> in de functies 1.x  |Registratie<br/> in de functies 2.x  |
|---------|---------|---------|
|Azure Portal|Automatisch|[Automatisch met prompt](#azure-portal-development)|
|Lokale met behulp van Azure Functions Core-hulpprogramma 's|Automatisch|[Core extra CLI-opdrachten gebruiken](#local-development-azure-functions-core-tools)|
|C# met behulp van Visual Studio 2017 klassenbibliotheek|[Gebruik NuGet-hulpprogramma 's](#c-class-library-with-visual-studio-2017)|[Gebruik NuGet-hulpprogramma 's](#c-class-library-with-visual-studio-2017)|
|C# met behulp van Visual Studio Code klassenbibliotheek|N/A|[.NET Core CLI gebruiken](#c-class-library-with-visual-studio-code)|

De volgende bindingstypen zijn uitzonderingen waarvoor geen expliciete registratie is vereist omdat ze automatisch worden geregistreerd in alle versies en omgevingen: HTTP, timer en Azure Storage (blobs, wachtrijen en tabellen). 

### <a name="azure-portal-development"></a>Ontwikkelen van Azure portal

Deze sectie geldt alleen voor functies 2.x. Uitbreidingen van de binding niet moeten expliciet worden geregistreerd in functies 1.x.

Wanneer u een functie maken of een binding toevoegt, wordt u gevraagd wanneer de uitbreiding voor de trigger of binding moet worden geregistreerd. Reageren op de vraag door te klikken op **installeren** registreren van de extensie. Installatie kan 10 minuten duren voordat een plan verbruik.

U moet elke uitbreiding slechts één keer voor een bepaalde functie-app installeren. 

### <a name="local-development-azure-functions-core-tools"></a>Lokale ontwikkeling Core hulpprogramma's van Azure-functies

Deze sectie geldt alleen voor functies 2.x. Uitbreidingen van de binding niet moeten expliciet worden geregistreerd in functies 1.x.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
### <a name="c-class-library-with-visual-studio-2017"></a>C# met Visual Studio 2017 klassenbibliotheek

In **Visual Studio 2017**, kunt u pakketten installeren vanuit de Package Manager-Console met de [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Install-Package Microsoft.Azure.WebJobs.ServiceBus --Version <target_version>
```

De naam van het pakket moet worden gebruikt voor een bepaalde binding is beschikbaar in het verwijzingsartikel voor binding. Zie voor een voorbeeld de [pakketten sectie van het Service Bus binding verwijzingsartikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<target_version>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden vermeld op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met de runtime van Functions overeenkomen 1.x of 2.x zijn opgegeven in het verwijzingsartikel voor de binding.

### <a name="c-class-library-with-visual-studio-code"></a>C# met Visual Studio Code klassenbibliotheek

In **Visual Studio Code**, kunt u pakketten installeren via de opdrachtprompt de [dotnet pakket toevoegen](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) opdracht in de CLI .NET Core, zoals wordt weergegeven in het volgende voorbeeld:

```terminal
dotnet add package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

De .NET Core CLI kan alleen worden gebruikt voor het ontwikkelen van Azure Functions 2.x.

De naam van het pakket moet worden gebruikt voor een bepaalde binding is beschikbaar in het verwijzingsartikel voor binding. Zie voor een voorbeeld de [pakketten sectie van het Service Bus binding verwijzingsartikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<target_version>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden vermeld op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met de runtime van Functions overeenkomen 1.x of 2.x zijn opgegeven in het verwijzingsartikel voor de binding.

## <a name="binding-direction"></a>Richting van de binding

Alle triggers en bindingen hebben een `direction` eigenschap in de *function.json* bestand:

- Voor triggers is de richting altijd `in`
- Invoer- en uitvoergegevens bindingen gebruiken `in` en `out`
- Sommige bindingen ondersteuning voor een speciale richting `inout`. Als u `inout`, alleen de **geavanceerde editor** is beschikbaar in de **integreren** tabblad.

Als u werkt met [kenmerken in een class-bibliotheek](functions-dotnet-class-library.md) voor het configureren van triggers en bindingen, de richting is opgegeven in een kenmerkconstructor of afgeleid van het parametertype.

## <a name="using-the-function-return-value"></a>Met behulp van de geretourneerde waarde van de functie

In de talen die over een retourwaarde beschikt, kunt u een binding uitvoer binden aan de retourwaarde:

* In C# klassebibliotheek, het kenmerk van de binding uitvoer van toepassing op de geretourneerde waarde van de methode.
* In andere talen, stelt u de `name` eigenschap in *function.json* naar `$return`.

Als u meer dan één item schrijven wilt, gebruikt u een [object collector](functions-reference-csharp.md#writing-multiple-output-values) in plaats van de retourwaarde. Als er meerdere bindingen van de uitvoer, gebruikt u de retourwaarde voor slechts één van beide.

Zie het voorbeeld taalspecifieke:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Hier volgt C#-code die de retourwaarde voor een binding uitvoer, gevolgd door een async-voorbeeld gebruikt:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>Voorbeeld van C#-script

Hier wordt de uitvoer-binding in de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Dit is de C# scriptcode, gevolgd door een async-voorbeeld:

```cs
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>F #-voorbeeld

Hier wordt de uitvoer-binding in de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Dit is de F #-code:

```fsharp
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Hier wordt de uitvoer-binding in de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

In JavaScript, de retourwaarde gaat in de tweede parameter voor `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
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

## <a name="binding-expressions-and-patterns"></a>Expressies voor gegevensbinding en patronen

Een van de meest krachtige functies van triggers en bindingen is *bindingsexpressies*. In de *function.json* -bestand en in functieparameters en code, kunt u expressies dat uit diverse bronnen worden omgezet naar waarden.

De meeste expressies worden geïdentificeerd door ze tussen accolades. Bijvoorbeeld in een functie van de trigger wachtrij `{queueTrigger}` wordt omgezet in de wachtrij berichttekst. Als de `path` eigenschap voor een blob uitvoer-binding is `container/{queueTrigger}` en de functie wordt geactiveerd door een wachtrijbericht `HelloWorld`, een blob met de naam `HelloWorld` wordt gemaakt.

Soorten expressies voor gegevensbinding

* [App-instellingen](#binding-expressions---app-settings)
* [Naam van de trigger-bestand](#binding-expressions---trigger-file-name)
* [Trigger metagegevens](#binding-expressions---trigger-metadata)
* [JSON-nettoladingen](#binding-expressions---json-payloads)
* [Nieuwe GUID](#binding-expressions---create-guids)
* [Huidige datum en tijd](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Expressies voor gegevensbinding - app-instellingen

Als een best practice moeten geheimen en verbindingsreeksen worden beheerd met behulp van app-instellingen, in plaats van configuratiebestanden. Dit beperkt de toegang tot deze geheime gegevens en wordt het veilig voor het opslaan van bestanden, zoals *function.json* in openbare source control-opslagplaatsen.

App-instellingen zijn ook nuttig wanneer u configuratie wilt wijzigen op basis van de omgeving. In een testomgeving kunt u bijvoorbeeld voor het bewaken van een andere wachtrij of blob storage-container.

Expressies voor gegevensbinding van App-instelling anders uit andere expressies voor gegevensbinding zijn geïdentificeerd: ze worden ingepakt in procenttekens in plaats van accolades. Als het blob uitvoer binding-pad is bijvoorbeeld `%Environment%/newblob.txt` en de `Environment` is de waarde van de app-instelling `Development`, een blob's worden aangemaakt in de `Development` container.

Wanneer een functie wordt lokaal uitgevoerd, app-instellingswaarden afkomstig zijn van de *local.settings.json* bestand.

Houd er rekening mee dat de `connection` eigenschap van triggers en bindingen is een speciaal geval en waarden, automatisch opgelost als de appinstellingen, zonder procenttekens. 

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

### <a name="binding-expressions---trigger-file-name"></a>Expressies voor gegevensbinding - trigger-bestandsnaam

De `path` voor een Blob trigger een patroon dat u kunt verwijzen naar de naam van de activerende blob in andere bindingen en code werkt niet. Het patroon kan ook betekenen filtercriteria die opgeeft welke blobs een functie-aanroep kunnen activeren.

In de volgende Blob-trigger binding, bijvoorbeeld de `path` patroon is `sample-images/{filename}`, die wordt gemaakt met de bindingexpressie voor een met de naam `filename`:

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
    ...
```

De expressie `filename` kan vervolgens worden gebruikt in een uitvoer-binding om op te geven van de naam van de blob wordt gemaakt:

```json
    ...
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

Functiecode toegang heeft tot deze dezelfde waarde met behulp van `filename` als een parameternaam:

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

Dezelfde mogelijkheid om het gebruik van de expressies voor gegevensbinding en patronen is van toepassing op kenmerken in de klassenbibliotheken van de. Het kenmerk constructorparameters zijn in het volgende voorbeeld wordt hetzelfde `path` waarden als de voorgaande *function.json* voorbeelden: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    TraceWriter log)
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
}

```

U kunt ook expressies voor het delen van de bestandsnaam in, bijvoorbeeld de extensie maken. Zie voor meer informatie over het gebruik van expressies en patronen in de Blob-padtekenreeks de [opslag binding blobverwijzing](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Expressies voor gegevensbinding - trigger metagegevens

Naast de nettolading van de gegevens die is geleverd door een trigger (zoals de inhoud van het bericht uit de wachtrij waarmee een functie is geactiveerd), bieden veel triggers aanvullende metagegevenswaarden. Deze waarden kunnen worden gebruikt als de invoerparameters in C# en F # of eigenschappen op de `context.bindings` -object in JavaScript. 

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

### <a name="binding-expressions---json-payloads"></a>Expressies voor gegevensbinding - JSON-nettoladingen

Wanneer een trigger nettolading JSON is, raadpleegt u de eigenschappen in de configuratie voor andere bindingen in dezelfde functie en functiecode.

Het volgende voorbeeld wordt de *function.json* -bestand voor een webhook-functie die een blob-naam in JSON ontvangt: `{"BlobName":"HelloWorld.txt"}`. Een Blob invoer binding leest de blob en het HTTP-uitvoer binding retourneert de blobinhoud van het HTTP-antwoord. U ziet dat de binding van Blob-invoer opgehaald van de blob-naam door verwijst rechtstreeks naar de `BlobName` eigenschap (`"path": "strings/{BlobName}"`)

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
      "path": "strings/{BlobName.FileName}.{BlobName.Extension}",
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

Dit werkt in C# en F #, moet u een klasse die de velden om te worden gedeserialiseerd definieert, zoals in het volgende voorbeeld:

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

JSON-deserialisatie in JavaScript, wordt automatisch worden uitgevoerd.

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

#### <a name="dot-notation"></a>Puntnotatie

Als sommige van de eigenschappen in de JSON-nettolading objecten met eigenschappen, kunt u met die via puntnotatie verwijzen. Stel bijvoorbeeld dat uw JSON uitziet:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

U kunt rechtstreeks naar verwijzen `FileName` als `BlobName.FileName`. Met deze JSON-indeling, dit is wat de `path` eigenschap in het voorgaande voorbeeld eruit als:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

In C# moet u twee klassen:

```csharp
public class BlobInfo
{
    public BlobName BlobName { get; set; }
}
public class BlobName
{
    public string FileName { get; set; }
    public string Extension { get; set; }
}
```

### <a name="binding-expressions---create-guids"></a>Expressies voor gegevensbinding - maken GUID 's

De `{rand-guid}` bindende expressie maakt een GUID. Het volgende blobpad in een `function.json` maakt een blob met een naam zoals *50710cb5-84b9 - 4d 87 9d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Expressies voor gegevensbinding - huidige tijd

De expressie voor gegevensbinding `DateTime` wordt omgezet naar `DateTime.UtcNow`. Het volgende blobpad in een `function.json` maakt een blob met een naam zoals *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="binding-at-runtime"></a>Binding tijdens runtime

In C# en andere .NET-talen, kunt u een patroon imperatieve binding in plaats van de declaratieve bindingen in *function.json* en kenmerken. Imperatieve binding is handig wanneer bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Zie voor meer informatie, de [C# referentie voor ontwikkelaars](functions-dotnet-class-library.md#binding-at-runtime) of de [C# script referentie voor ontwikkelaars](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>Function.JSON bestand schema

De *function.json* bestand schema is beschikbaar op [ http://json.schemastore.org/function ](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Afhandeling van bindingsfouten

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Zie voor koppelingen naar alle relevante fout onderwerpen voor de verschillende services wordt ondersteund door de functies, de [foutcodes Binding](functions-bindings-error-pages.md#binding-error-codes) sectie van de [Azure Functions foutafhandeling](functions-bindings-error-pages.md) overzichtsonderwerp.  

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
