---
title: Azure Functions-bindingen expressies en patronen
description: Informatie over het maken van andere expressies met Azure Functions-binding op basis van algemene patronen.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 0c1dbbae5e4be965f195b5ea4fc88b1bc5fb4f87
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56887179"
---
# <a name="azure-functions-binding-expression-patterns"></a>Azure Functions expressiepatronen-binding

Een van de krachtigste functies van [triggers en bindingen](./functions-triggers-bindings.md) is *expressies binding*. In de *function.json* -bestand en in de parameters van de functie en code, kunt u expressies die wordt omgezet in waarden van verschillende bronnen.

De meeste expressies worden geïdentificeerd door deze tussen accolades. Bijvoorbeeld in een functie van de trigger wachtrij `{queueTrigger}` wordt omgezet naar de wachtrij berichttekst. Als de `path` eigenschap voor een blob van uitvoer binding `container/{queueTrigger}` en de functie wordt geactiveerd door een wachtrijbericht `HelloWorld`, een blob met de naam `HelloWorld` wordt gemaakt.

Typen expressies voor gegevensbinding

* [App-instellingen](#binding-expressions---app-settings)
* [Trigger-bestandsnaam](#trigger-file-name)
* [De metagegevens van de trigger](#trigger-metadata)
* [JSON-nettolading](#json-payloads)
* [Nieuwe GUID](#create-guids)
* [Huidige datum en tijd](#current-time)

## <a name="binding-expressions---app-settings"></a>Binding expressies - app-instellingen

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
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-file-name"></a>Trigger-bestandsnaam

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
public static void Run(Stream image, string filename, Stream imageSmall, ILogger log)  
{
    log.LogInformation($"Blob trigger processing: {filename}");
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
    ILogger log)
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
}

```

U kunt ook expressies voor het delen van de bestandsnaam, zoals de extensie maken. Zie voor meer informatie over het gebruik van expressies en patronen in de tekenreeks van de Blob-pad, de [naslaginformatie voor Storage blob binding](functions-bindings-storage-blob.md).

## <a name="trigger-metadata"></a>De metagegevens van de trigger

Naast de nettolading van de gegevens die is geleverd door een trigger (zoals de inhoud van het wachtrijbericht dat een functie geactiveerd), bieden veel triggers waarden van aanvullende metagegevens. Deze waarden kunnen worden gebruikt als invoerparameters die zijn opgegeven in C# en F# of de eigenschappen van de `context.bindings` -object in JavaScript. 

Een trigger voor Azure Queue storage ondersteunt bijvoorbeeld de volgende eigenschappen:

* QueueTrigger - inhoud van het bericht wordt geactiveerd als een geldige tekenreeks
* DequeueCount
* ExpirationTime
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

## <a name="json-payloads"></a>JSON-nettolading

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

Dit werkt C# en F#, moet u een klasse die de velden die moeten worden gedeserialiseerd, zoals in het volgende voorbeeld definieert:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents, ILogger log)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    log.LogInformation($"Processing: {info.BlobName}");

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

### <a name="dot-notation"></a>Een puntnotering

Als sommige van deze eigenschappen in de JSON-nettolading objecten met eigenschappen, kunt u verwijzen naar die rechtstreeks via een puntnotering. Stel bijvoorbeeld dat uw JSON er als volgt uitzien:

```json
{
  "BlobName": {
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

## <a name="create-guids"></a>GUID's maken

De `{rand-guid}` expressie binding maakt een GUID. Het volgende blobpad in een `function.json` bestand maakt u een blob met een naam, zoals *50710cb5-84b9 - 4d-9-87 d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

## <a name="current-time"></a>Huidige tijd

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

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Met behulp van de geretourneerde waarde van de Azure-functie](./functions-bindings-return-value.md)
