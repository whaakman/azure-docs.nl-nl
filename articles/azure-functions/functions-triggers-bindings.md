---
title: Triggers en bindingen in Azure Functions
description: Informatie over het gebruik van triggers en bindingen in Azure Functions verbinding maken met de uitvoering van uw code online evenementen en cloud-gebaseerde services.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/24/2018
ms.author: glenga
ms.openlocfilehash: 694dd98caadb12571c58f9d615cf75325654c772
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801302"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions-triggers en bindingen concepten

Dit artikel is een conceptueel overzicht van triggers en bindingen in Azure Functions. Functies die gemeenschappelijk voor alle bindingen en alle ondersteunde talen zijn worden hier beschreven.

## <a name="overview"></a>Overzicht

Een *trigger* definieert hoe een functie is aangeroepen. Een functie moet exact één trigger hebben. Aan triggers zijn gegevens gekoppeld. Meestal is dit de nettolading waarmee de functie is geactiveerd.

Invoer en uitvoer *bindingen* bieden een declaratieve manier om verbinding maken met gegevens vanuit uw code. Bindingen zijn optioneel en een functie verschillende invoer beschikken en uitvoerbindingen. 

Triggers en bindingen kunnen u voorkomen hardcoderen de details van de services waarmee u werkt. De functie ontvangt gegevens (bijvoorbeeld de inhoud van een wachtrijbericht) in de parameters van de functie. U kunt gegevens (bijvoorbeeld om een wachtrijbericht te maken) verzenden met behulp van de geretourneerde waarde van de functie. In C# en C#-script, andere manieren om gegevens te verzenden zijn `out` parameters en [collector objecten](functions-reference-csharp.md#writing-multiple-output-values).

Bij het ontwikkelen van functies met behulp van Azure portal, triggers en bindingen zijn geconfigureerd in een *function.json* bestand. De portal biedt een gebruikersinterface voor deze configuratie, maar u kunt het bestand bewerken rechtstreeks door te wijzigen in de **geavanceerde editor**.

Wanneer u functies ontwikkelen met behulp van Visual Studio te maken van een klassebibliotheek, configureert u triggers en bindingen door met het inrichten van methoden en parameters met kenmerken.

## <a name="example-trigger-and-binding"></a>Voorbeeld van de trigger en -binding

Stel dat u wilt een nieuwe rij naar Azure-tabelopslag geschreven wanneer een nieuw bericht wordt weergegeven in Azure Queue storage. In dit scenario kan worden geïmplementeerd met behulp van een Azure Queue storage-trigger en een Azure Table storage-Uitvoerbinding. 

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

Het eerste element in de `bindings` matrix is de trigger voor Queue storage. De `type` en `direction` eigenschappen van de trigger identificeren. De `name` eigenschap geeft u de parameter van de functie die de inhoud van het wachtrij-bericht ontvangt. De naam van de wachtrij voor het bewaken van wordt `queueName`, en de verbindingsreeks is in de app-instelling die is geïdentificeerd door `connection`.

Het tweede element in de `bindings` matrix is de Azure Table Storage-Uitvoerbinding. De `type` en `direction` eigenschappen geven de binding. De `name` eigenschap geeft aan hoe de functie biedt de nieuwe rij in de tabel, met behulp van de functie wordt in dit geval waarde retourneren. De naam van de tabel wordt `tableName`, en de verbindingsreeks is in de app-instelling die is geïdentificeerd door `connection`.

Bekijken en bewerken van de inhoud van *function.json* in Azure portal, klikt u op de **geavanceerde editor** kiezen op de **integreren** tabblad van uw functie.

> [!NOTE]
> De waarde van `connection` is de naam van een app-instelling met de verbindingsreeks, niet de connection string zelf. Bindings-verbinding gebruiken tekenreeksen die zijn opgeslagen in de app-instellingen af te dwingen de beste praktijk die *function.json* bevat geen geheimen van de service.

Hier volgt een C#-script-code die geschikt is voor deze trigger en een binding. U ziet dat de naam van de parameter waarmee de inhoud van de wachtrij bericht is `order`; deze naam is vereist omdat de `name` eigenschapswaarde in *function.json* is `order` 

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

In een klassebibliotheek, de dezelfde trigger en informatie over de binding &mdash; wachtrij- en tabelnamen, opslagaccounts, functie parameters voor invoer en uitvoer &mdash; wordt geleverd door de kenmerken in plaats van een bestand function.json. Hier volgt een voorbeeld:

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

Zie voor meer informatie over welke bindingen zijn beschikbaar als preview of zijn goedgekeurd voor gebruik in productieomgevingen, [ondersteunde talen](supported-languages.md).

## <a name="register-binding-extensions"></a>Binding extensies registreren

In sommige omgevingen ontwikkeling, u moet expliciet *registreren* een binding die u wilt gebruiken. Bindinguitbreidingen vindt u in het NuGet-pakketten en voor het registreren van een extensie die u installeert een pakket. De volgende tabel geeft aan wanneer en hoe u bindinguitbreidingen registreren.

|Ontwikkelomgeving |Registratie<br/> in de functies 1.x  |Registratie<br/> in de functies 2.x  |
|---------|---------|---------|
|Azure Portal|Automatisch|[Automatisch met prompt](#azure-portal-development)|
|Lokale met behulp van Azure Functions Core Tools|Automatisch|[Core-hulpprogramma's-CLI-opdrachten gebruiken](#local-development-azure-functions-core-tools)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio 2017|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2017)|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2017)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio Code|N/A|[.NET Core CLI gebruiken](#c-class-library-with-visual-studio-code)|

De volgende bindingstypen zijn uitzonderingen waarvoor geen expliciete registratie is vereist omdat ze automatisch worden geregistreerd in alle versies en omgevingen: http- en timer.

### <a name="azure-portal-development"></a>Azure portal-ontwikkeling

In deze sectie geldt alleen voor functies 2.x. Bindinguitbreidingen geen expliciet worden geregistreerd in functies 1.x.

Wanneer u een functie maken of een binding toevoegt, wordt u gevraagd wanneer de uitbreiding voor de trigger of binding moet worden geregistreerd. Door te klikken op de prompt beantwoorden **installeren** voor het registreren van de extensie. Installatie kan tot tien minuten duren in een verbruiksabonnement.

U moet elke uitbreiding slechts één keer voor een bepaalde functie-app installeren. Voor ondersteunde bindingen die niet beschikbaar in de portal of bij te werken de geïnstalleerde extensie, u kunt ook [handmatig installeren of bijwerken van Azure Functions binding extensies vanuit de portal](install-update-binding-extensions-manual.md).  

### <a name="local-development-azure-functions-core-tools"></a>Azure Functions Core Tools voor lokale ontwikkeling

In deze sectie geldt alleen voor functies 2.x. Bindinguitbreidingen geen expliciet worden geregistreerd in functies 1.x.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
### <a name="c-class-library-with-visual-studio-2017"></a>C#-klassenbibliotheek vormt met Visual Studio 2017

In **Visual Studio 2017**, u kunt pakketten installeren vanuit de Package Manager-Console met behulp van de [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Install-Package Microsoft.Azure.WebJobs.ServiceBus --Version <target_version>
```

De naam van het pakket moet worden gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<target_version>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

### <a name="c-class-library-with-visual-studio-code"></a>C#-klassenbibliotheek vormt met Visual Studio Code

In **Visual Studio Code**, u kunt pakketten installeren vanuit de opdrachtprompt met de [dotnet-pakket toevoegen](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) opdracht in de .NET Core CLI, zoals wordt weergegeven in het volgende voorbeeld:

```terminal
dotnet add package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

De .NET Core-CLI kan alleen worden gebruikt voor het ontwikkelen van Azure Functions 2.x.

De naam van het pakket moet worden gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<target_version>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="binding-direction"></a>Richting van de binding

Alle triggers en bindingen hebben een `direction` eigenschap in de *function.json* bestand:

- Triggers is de richting altijd `in`
- Gebruik van de invoer- en uitvoerbindingen `in` en `out`
- Sommige bindingen ondersteunt een speciale richting `inout`. Als u `inout`, alleen de **geavanceerde editor** is beschikbaar in de **integreren** tabblad.

Bij het gebruik [kenmerken in een klassebibliotheek](functions-dotnet-class-library.md) voor het configureren van triggers en bindingen, de richting is opgegeven in een kenmerkconstructie of afgeleid van het parametertype.

## <a name="using-the-function-return-value"></a>Met behulp van de geretourneerde waarde van de functie

In de talen waarvoor de geretourneerde waarde, kunt u een Uitvoerbinding binden aan de geretourneerde waarde:

* In een C#-klassenbibliotheek vormt, het kenmerk van de binding uitvoer van toepassing op de geretourneerde waarde van de methode.
* In andere talen, stelt u de `name` eigenschap in *function.json* naar `$return`.

Als er meerdere uitvoerbindingen, gebruikt u de geretourneerde waarde voor slechts één provider.

In C# en C#-script, andere manieren om gegevens te verzenden naar een Uitvoerbinding zijn `out` parameters en [collector objecten](functions-reference-csharp.md#writing-multiple-output-values).

Zie het gebruik van de geretourneerde waarde van taalspecifieke-voorbeeld:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Hier volgen enkele redenen C#-code die gebruikmaakt van de geretourneerde waarde voor een Uitvoerbinding, gevolgd door een asynchrone-voorbeeld:

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

Hier volgt de Uitvoerbinding de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Dit is de C#-scriptcode, gevolgd door een voorbeeld van de asynchrone:

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

Hier volgt de Uitvoerbinding de *function.json* bestand:

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

Hier volgt de Uitvoerbinding de *function.json* bestand:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

De geretourneerde waarde in JavaScript, komt de tweede parameter voor `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

## <a name="binding-datatype-property"></a>De eigenschap dataType binding

In .NET, gebruikt u het parametertype voor het definiëren van het gegevenstype voor de invoergegevens. Gebruik bijvoorbeeld `string` verbinding maken met de tekst van een wachtrijtrigger, een matrix van bytes lezen als binaire bestanden en een aangepast type te deserialiseren naar een POCO-object.

Voor de talen die dynamisch worden getypt, zoals JavaScript, gebruikt u de `dataType` eigenschap in de *function.json* bestand. Bijvoorbeeld, als u wilt de inhoud van een HTTP-aanvraag in binaire indeling lezen, ingesteld `dataType` naar `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andere opties voor `dataType` zijn `stream` en `string`.

## <a name="binding-expressions-and-patterns"></a>Binding-expressies en patronen

Een van de krachtigste functies van triggers en bindingen is *expressies binding*. In de *function.json* -bestand en in de parameters van de functie en code, kunt u expressies die wordt omgezet in waarden van verschillende bronnen.

De meeste expressies worden geïdentificeerd door deze tussen accolades. Bijvoorbeeld in een functie van de trigger wachtrij `{queueTrigger}` wordt omgezet naar de wachtrij berichttekst. Als de `path` eigenschap voor een blob van uitvoer binding `container/{queueTrigger}` en de functie wordt geactiveerd door een wachtrijbericht `HelloWorld`, een blob met de naam `HelloWorld` wordt gemaakt.

Typen expressies voor gegevensbinding

* [App-instellingen](#binding-expressions---app-settings)
* [Trigger-bestandsnaam](#binding-expressions---trigger-file-name)
* [De metagegevens van de trigger](#binding-expressions---trigger-metadata)
* [JSON-nettolading](#binding-expressions---json-payloads)
* [Nieuwe GUID](#binding-expressions---create-guids)
* [Huidige datum en tijd](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Binding expressies - app-instellingen

Als een best practice, moeten geheimen en verbindingsreeksen worden beheerd met behulp van app-instellingen, in plaats van-configuratiebestanden. Dit beperkt de toegang tot deze geheime gegevens en maakt deze veilig voor het opslaan van bestanden, zoals *function.json* in openbare broncodebeheeropslagplaatsen.

App-instellingen zijn ook handig wanneer u configuratie wilt wijzigen op basis van de omgeving. Bijvoorbeeld in een testomgeving, kunt u voor het bewaken van een andere wachtrij of blob storage-container.

App-instelling binding expressies anders zijn geïdentificeerd vanuit andere binding-expressies: ze zijn verpakt in procenttekens in plaats van accolades. Bijvoorbeeld als de blob-uitvoerpad binding is `%Environment%/newblob.txt` en de `Environment` is de waarde van de app-instelling `Development`, een blob wordt gemaakt de `Development` container.

Wanneer een door de functie lokaal wordt uitgevoerd, app-instellingswaarden afkomstig zijn van de *local.settings.json* bestand.

Houd er rekening mee dat de `connection` eigenschap van triggers en bindingen is een speciaal geval en waarden, automatisch opgelost als app-instellingen, zonder procenttekens. 

Het volgende voorbeeld wordt een Azure Queue Storage-trigger die gebruikmaakt van een app-instelling `%input-queue-name%` voor het definiëren van de wachtrij moet worden geactiveerd.

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

U kunt dezelfde benadering gebruiken in klassenbibliotheken:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

### <a name="binding-expressions---trigger-file-name"></a>Binding expressies - trigger-bestandsnaam

De `path` voor een Blob trigger een patroon waarmee u kunt verwijzen naar de naam van de activerende blob in andere bindingen en functie van code kan zijn. Het patroon kan ook filtercriteria die opgeeft welke blobs kunnen activeren een functieaanroepen bevatten.

Bijvoorbeeld, in de volgende Blob-trigger verbinding maakt, de `path` patroon is `sample-images/{filename}`, die wordt gemaakt met de bindingexpressie voor een met de naam `filename`:

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

De expressie `filename` kan vervolgens worden gebruikt in een Uitvoerbinding om op te geven van de naam van de blob die wordt gemaakt:

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

Functiecode heeft toegang tot deze dezelfde waarde met behulp van `filename` als een parameternaam:

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

Dezelfde mogelijkheid om expressies voor gegevensbinding en patronen te gebruiken is van toepassing op kenmerken in klassenbibliotheken. De parameters van de constructor kenmerk zijn in het volgende voorbeeld wordt hetzelfde `path` waarden als de voorgaande *function.json* voorbeelden: 

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

U kunt ook expressies voor het delen van de bestandsnaam, zoals de extensie maken. Zie voor meer informatie over het gebruik van expressies en patronen in de tekenreeks van de Blob-pad, de [naslaginformatie voor Storage blob binding](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Binding expressies - trigger metagegevens

Naast de nettolading van de gegevens die is geleverd door een trigger (zoals de inhoud van het wachtrijbericht dat een functie geactiveerd), bieden veel triggers waarden van aanvullende metagegevens. Deze waarden kunnen worden gebruikt als invoerparameters die zijn opgegeven in C# en F # of eigenschappen op de `context.bindings` -object in JavaScript. 

Een trigger voor Azure Queue storage ondersteunt bijvoorbeeld de volgende eigenschappen:

* QueueTrigger - inhoud van het bericht wordt geactiveerd als een geldige tekenreeks
* DequeueCount
* expirationTime
* Id
* InsertionTime
* NextVisibleTime
* PopReceipt

De metagegevenswaarden van deze zijn beschikbaar in *function.json* eigenschappen van het bestand. Stel bijvoorbeeld dat u een wachtrijtrigger te gebruiken en het wachtrijbericht bevat de naam van een blob die u wilt lezen. In de *function.json* -bestand, kunt u `queueTrigger` metagegevenseigenschap in de blob `path` eigenschap, zoals wordt weergegeven in het volgende voorbeeld:

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

Details van de eigenschappen van de metagegevens voor elke trigger worden beschreven in de bijbehorende verwijzing artikel. Zie voor een voorbeeld [metagegevens in de wachtrij trigger](functions-bindings-storage-queue.md#trigger---message-metadata). Documentatie is ook beschikbaar in de **integreren** tabblad van de portal, in de **documentatie** sectie hieronder het gedeelte van de configuratie van binding.  

### <a name="binding-expressions---json-payloads"></a>Binding expressies - JSON-nettolading

Als een trigger-nettolading JSON is, kunt u verwijzen naar de eigenschappen in de configuratie voor andere bindingen in dezelfde functie en functiecode aan te geven.

Het volgende voorbeeld wordt de *function.json* -bestand voor een webhook-functie die de naam van een blob in JSON ontvangt: `{"BlobName":"HelloWorld.txt"}`. Een Blob-Invoerbinding leest de blob en de HTTP-output binding retourneert de inhoud van een blob in het HTTP-antwoord. U ziet dat de Blob-Invoerbinding haalt u de blobnaam door rechtstreeks naar de `BlobName` eigenschap (`"path": "strings/{BlobName}"`)

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

Om dit te werken in C# en F #, moet u een klasse die de velden die moeten worden gedeserialiseerd, zoals in het volgende voorbeeld definieert:

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents, TraceWriter log)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    log.Info($"Processing: {info.BlobName}");

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

#### <a name="dot-notation"></a>Een puntnotering

Als sommige van deze eigenschappen in de JSON-nettolading objecten met eigenschappen, kunt u verwijzen naar die rechtstreeks via een puntnotering. Stel bijvoorbeeld dat uw JSON er als volgt uitzien:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

U kunt rechtstreeks naar verwijzen `FileName` als `BlobName.FileName`. Met deze JSON-indeling, dit is wat de `path` eigenschap in het voorgaande voorbeeld zou er als volgt uitzien:

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

De `{rand-guid}` expressie binding maakt een GUID. Het volgende blobpad in een `function.json` bestand maakt u een blob met een naam, zoals *50710cb5-84b9 - 4d-9-87 d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Huidige tijd van expressies - binding

De bindingexpressie `DateTime` wordt omgezet naar `DateTime.UtcNow`. Het volgende blobpad in een `function.json` bestand maakt u een blob met een naam, zoals *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="binding-at-runtime"></a>Binding tijdens runtime

In C# en andere .NET-talen, kunt u een patroon imperatieve binding, in plaats van de declaratieve bindingen in *function.json* en kenmerken. Imperatieve binding is handig als de bindende parameters moeten worden berekend tijdens runtime in plaats van ontwerp. Zie voor meer informatie, de [C#-naslaginformatie](functions-dotnet-class-library.md#binding-at-runtime) of de [C# script developer reference](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>Function.JSON bestandsschema

De *function.json* bestandsschema is beschikbaar op [ http://json.schemastore.org/function ](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Afhandeling van fouten-binding

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Voor koppelingen naar alle relevante fout onderwerpen voor de verschillende services die door de functies worden ondersteund, Zie de [Binding foutcodes](functions-bindings-error-pages.md#binding-error-codes) sectie van de [Azure Functions-foutafhandeling](functions-bindings-error-pages.md) overzichtsonderwerp.  

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
