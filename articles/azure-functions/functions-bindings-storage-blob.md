---
title: Azure Blob storage-bindingen voor Azure Functions
description: Over het gebruik van Azure Blob storage-triggers en bindingen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/15/2018
ms.author: cshoe
ms.openlocfilehash: 16899c833d996902cf7a0a3f7ab57479869fbdd9
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737778"
---
# <a name="azure-blob-storage-bindings-for-azure-functions"></a>Azure Blob storage-bindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u werkt met Azure Blob storage-bindingen in Azure Functions. Azure Functions ondersteunt activeren, invoeren en uitvoerbindingen voor blobs. Dit artikel bevat een sectie voor elke binding:

* [Blob trigger](#trigger)
* [BLOB-Invoerbinding](#input)
* [BLOB-Uitvoerbinding](#output)

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> De trigger van Event Grid gebruiken in plaats van de Blob storage-trigger voor alleen-blob storage-accounts voor hoge schaalbaarheid of latentie verminderd. Zie voor meer informatie de [Trigger](#trigger) sectie.

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Blob storage-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet-pakket versie 2.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Blob) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Blob storage-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

Wanneer een nieuwe of bijgewerkte blob wordt gedetecteerd, begint de Blob storage-trigger een functie. De inhoud van een blob worden gegeven als invoer voor de functie.

De [trigger Gebeurtenisraster](functions-bindings-event-grid.md) heeft ingebouwde ondersteuning voor [blob-gebeurtenissen](../storage/blobs/storage-blob-event-overview.md) en kan ook worden gebruikt om te beginnen een functie wanneer een nieuwe of bijgewerkte blob wordt gedetecteerd. Zie voor een voorbeeld: de [afbeelding vergroten of verkleinen met Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md) zelfstudie.

Event Grid gebruiken in plaats van de Blob storage-trigger voor de volgende scenario's:

* Blob Storage-accounts
* Grote schaal
* De latentie minimaal

### <a name="blob-storage-accounts"></a>Blob Storage-accounts

[BLOB storage-accounts](../storage/common/storage-account-overview.md#types-of-storage-accounts) worden ondersteund voor blob-invoer- en uitvoerbindingen, maar niet voor blobtriggers. BLOB-opslagtriggers vereisen een algemeen opslagaccount.

### <a name="high-scale"></a>Grote schaal

Grote schaal kan los worden gedefinieerd als containers die meer dan 100.000 blobs erin hebt of storage-accounts die beschikken over meer dan 100 blob-updates per seconde.

### <a name="latency-issues"></a>Latentieproblemen met

Als uw functie-app op het verbruiksabonnement is, mag er maximaal 10 minuten duren in de verwerking van nieuwe blobs als een functie-app niet actief is geworden. Om te voorkomen dat deze latentie, kunt u overschakelen naar een App Service-plan met Always On ingeschakeld. U kunt ook een [trigger van Event Grid](functions-bindings-event-grid.md) met uw Blob storage-account. Zie voor een voorbeeld: de [Event Grid-zelfstudie](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json). 

### <a name="queue-storage-trigger"></a>Trigger voor queue storage

Naast het Event Grid, een ander alternatief voor het verwerken van BLOB's is de trigger voor Queue storage, maar heeft geen ingebouwde ondersteuning voor gebeurtenissen van blob. U hoeft te maken van berichten in wachtrij plaatsen bij het maken of bijwerken van blobs. Voor een voorbeeld waarin wordt ervan uitgegaan u dat dat hebt gedaan, raadpleegt u de [blob Invoerbinding voorbeeld verderop in dit artikel](#input---example).

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die schrijft een logboek wanneer een blob wordt toegevoegd of bijgewerkt in de `samples-workitems` container.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [binding expressie](./functions-bindings-expressions-patterns.md) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie, [Blob-naam patronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over de `BlobTrigger` kenmerk, Zie [Trigger - kenmerken](#trigger---attributes).

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een blobtrigger binding in een *function.json* bestand en [Python-code](functions-reference-python.md) die gebruikmaakt van de binding. De functie wordt een logboek geschreven wanneer een blob wordt toegevoegd of bijgewerkt in de `samples-workitems` [container](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources).

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [binding expressie](./functions-bindings-expressions-patterns.md) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie, [Blob-naam patronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over *function.json* eigenschappen van het bestand, raadpleegt u de [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt C#-script-code die wordt gebonden aan een `Stream`:

```cs
public static void Run(Stream myBlob, string name, ILogger log)
{
   log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Hier volgt C#-script-code die wordt gebonden aan een `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een blobtrigger binding in een *function.json* bestand en [JavaScript-code](functions-reference-node.md) die gebruikmaakt van de binding. De functie wordt een logboek geschreven wanneer een blob wordt toegevoegd of bijgewerkt in de `samples-workitems` container.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [binding expressie](./functions-bindings-expressions-patterns.md) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie, [Blob-naam patronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over *function.json* eigenschappen van het bestand, raadpleegt u de [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

### <a name="trigger---python-example"></a>Trigger - Python-voorbeeld

Het volgende voorbeeld ziet u een blobtrigger binding in een *function.json* bestand en [Python-code](functions-reference-python.md) die gebruikmaakt van de binding. De functie wordt een logboek geschreven wanneer een blob wordt toegevoegd of bijgewerkt in de `samples-workitems` [container](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources).

Hier volgt de *function.json* bestand:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,
    "bindings": [
        {
            "name": "myblob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [binding expressie](./functions-bindings-expressions-patterns.md) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie, [Blob-naam patronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over *function.json* eigenschappen van het bestand, raadpleegt u de [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de Python-code:

```python
import logging
import azure.functions as func

def main(myblob: func.InputStream):
    logging.info('Python Blob trigger function processed %s', myblob.name)
```

### <a name="trigger---java-example"></a>Trigger - Java-voorbeeld

Het volgende voorbeeld ziet u een blobtrigger binding in een *function.json* bestand en [Java-code](functions-reference-java.md) die gebruikmaakt van de binding. De functie wordt een logboek geschreven wanneer een blob wordt toegevoegd of bijgewerkt in de `myblob` container.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "file",
            "type": "blobTrigger",
            "direction": "in",
            "path": "myblob/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Dit is de Java-code:

```java
@FunctionName("blobprocessor")
public void run(
  @BlobTrigger(name = "file",
               dataType = "binary",
               path = "myblob/{name}",
               connection = "MyStorageAccountAppSetting") byte[] content,
  @BindingName("name") String filename,
  final ExecutionContext context
) {
  context.getLogger().info("Name: " + filename + " Size: " + content.length + " bytes");
}
```


## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken om te configureren van een blobtrigger:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobTriggerAttribute.cs)

  De constructor van het kenmerk heeft een padtekenreeks die aangeeft van de container om te bekijken en eventueel een [patroon voor de blob](#trigger---blob-name-patterns). Hier volgt een voorbeeld:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Biedt een andere manier om op te geven van de storage-account te gebruiken. De constructor, wordt de naam van een app-instelling met een verbindingsreeks voor opslag. Het kenmerk kan worden toegepast op de parameter, klasseniveau of methode. Het volgende voorbeeld toont het klasseniveau en methode:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ....
  }
  ```

Het storage-account te gebruiken, wordt bepaald in de volgende volgorde:

* De `BlobTrigger` van kenmerk `Connection` eigenschap.
* De `StorageAccount` kenmerk toegepast op de dezelfde parameter als de `BlobTrigger` kenmerk.
* De `StorageAccount` kenmerk toegepast op de functie.
* De `StorageAccount` kenmerk toegepast op de klasse.
* Het standaardopslagaccount voor de functie-app (app-instelling 'AzureWebJobsStorage').

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `BlobTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `blobTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. Uitzonderingen worden vermeld in de [gebruik](#trigger---usage) sectie. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de blob in functiecode aan te geven. | 
|**path** | **BlobPath** |De [container](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) om te controleren.  Kan een [patroon voor de blob](#trigger---blob-name-patterns). | 
|**verbinding** | **Verbinding** | De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.<br><br>De verbindingsreeks moet zijn voor een algemeen opslagaccount, niet een [Blob storage-account](../storage/common/storage-account-overview.md#types-of-storage-accounts).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

In C# en C#-script, kunt u de volgende parametertypen voor de activerende blob:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* Een POCO geserialiseerd als JSON
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> vereist 'inout' binding `direction` in *function.json* of `FileAccess.ReadWrite` in een C#-klassenbibliotheek vormt.

Als u probeert te binden aan een van de Storage-SDK-typen en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

Binden aan `string`, `Byte[]`, of POCO wordt alleen aanbevolen als de blob is klein, als de volledige-blob-inhoud in het geheugen wordt geladen. Over het algemeen is het beter gebruik van een `Stream` of `CloudBlockBlob` type. Zie voor meer informatie, [gelijktijdigheid-en geheugengebruik](#trigger---concurrency-and-memory-usage) verderop in dit artikel.

Toegang tot de invoerblob gegevens in JavaScript, `context.bindings.<name from function.json>`.

## <a name="trigger---blob-name-patterns"></a>Trigger - patronen voor blob-naam

U kunt opgeven dat een patroon voor de blob in de `path` eigenschap in *function.json* of in de `BlobTrigger` kenmerkconstructie. Het naampatroon mag een [expressie van het filter of binding](./functions-bindings-expressions-patterns.md). De volgende secties vindt u voorbeelden.

### <a name="get-file-name-and-extension"></a>Naam en extensie ophalen

Het volgende voorbeeld ziet u hoe u verbinding maken met de naam van de blob-bestand en de extensie afzonderlijk:

```json
"path": "input/{blobname}.{blobextension}",
```
Als de naam van de blob *oorspronkelijke Blob1.txt*, de waarden van de `blobname` en `blobextension` variabelen in functiecode zijn *oorspronkelijke Blob1* en *txt*.

### <a name="filter-on-blob-name"></a>Filteren op de blobnaam van de

Het volgende voorbeeld triggers alleen voor blobs in de `input` container die beginnen met de tekenreeks 'oorspronkelijke-':

```json
"path": "input/original-{name}",
```

Als de blobnaam van de is *oorspronkelijke Blob1.txt*, de waarde van de `name` variabele in functiecode `Blob1`.

### <a name="filter-on-file-type"></a>Filteren op bestandstype

Het volgende voorbeeld wordt alleen geactiveerd *.png* bestanden:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filteren op accolades in bestandsnamen

Escapeteken moet worden gezocht tussen accolades in bestandsnamen, de accolades met behulp van twee accolades. Het volgende voorbeeld filters voor blobs die in de naam van de accolades:

```json
"path": "images/{{20140101}}-{name}",
```

Als de naam van de blob  *{20140101}-soundfile.mp3*, wordt de `name` variabele waarde in de functiecode *soundfile.mp3*.

## <a name="trigger---metadata"></a>Trigger - metadata

De blobtrigger biedt verschillende eigenschappen van metagegevens. Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies in andere bindingen voor gegevensbinding of als parameters in uw code. Deze waarden hebben dezelfde semantiek als de [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet) type.

|Eigenschap  |Type  |Description  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Het pad naar de activerende blob.|
|`Uri`|`System.Uri`|Van de blob-URI voor de primaire locatie.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties)|Eigenschappen van de blob. |
|`Metadata` |`IDictionary<string,string>`|De gebruiker gedefinieerde metagegevens voor de blob.|

Bijvoorbeeld de volgende C#-script en JavaScript-voorbeelden het foutenlogboek van het pad naar de activerende blob, met inbegrip van de container:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

## <a name="trigger---blob-receipts"></a>Trigger - blob-ontvangsten

De Azure Functions-runtime zorgt ervoor dat geen blob-trigger-functie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Om te bepalen als een bepaalde blob-versie is verwerkt, wordt bijgehouden *blob-ontvangsten*.

Azure Functions-winkels blob ontvangstbevestigingen voor e-mails in een container met de naam *azure-webtaken-hosts* in de Azure storage-account voor uw functie-app (gedefinieerd door de app-instelling `AzureWebJobsStorage`). De ontvangst van een blob bevat de volgende informatie:

* De geactiveerde functie ("*&lt;functie-appnaam >*. Functies.  *&lt;functienaam >*', bijvoorbeeld: "MyFunctionApp.Functions.CopyBlob")
* De containernaam van de
* Het blobtype ("BlockBlob" of "PageBlob")
* Naam van de blob
* De ETag (een blob versie-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Om af te dwingen opnieuw verwerken van een blob, verwijdert u de blob-ontvangstbewijs voor die blob uit de *azure-webtaken-hosts* container handmatig.

## <a name="trigger---poison-blobs"></a>Trigger - poison blobs

Wanneer een blob-triggerfunctie is mislukt voor een bepaalde blob, Azure Functions nieuwe pogingen die een totaal van 5 keer standaard functioneren.

Als alle 5 pogingen mislukken, Azure Functions een bericht toevoegt aan een opslagwachtrij met de naam *webjobs-blobtrigger-poison*. Bericht uit de wachtrij voor beheer van onverwerkbare blobs is een JSON-object met de volgende eigenschappen:

* FunctionId (in de indeling  *&lt;functie-appnaam >*. Functies.  *&lt;functienaam >*)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob versie-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

## <a name="trigger---concurrency-and-memory-usage"></a>Trigger - gelijktijdigheid- en geheugengebruik

De blobtrigger gebruikmaakt van een wachtrij intern, zodat het maximum aantal gelijktijdige functieaanroepen wordt bepaald door de [wachtrijen configuratie in host.json](functions-host-json.md#queues). De standaardinstellingen beperken gelijktijdigheid van taken naar 24 aanroepen. Deze limiet geldt afzonderlijk voor elke functie die gebruikmaakt van een blobtrigger.

[Het verbruiksabonnement](functions-scale.md#how-the-consumption-plan-works) een functie-app op een virtuele machine (VM) tot 1,5 GB geheugen beperkt. Geheugen wordt gebruikt door elk gelijktijdig worden uitgevoerd exemplaar van de functie en de Functions-runtime zelf. Als een blob-geactiveerde functie wordt de hele blob in het geheugen geladen, de maximale hoeveelheid geheugen die wordt gebruikt door deze functie alleen voor blobs is 24 uur per dag * maximale blob-grootte. Een functie-app met drie blob geactiveerde functies en de standaardinstellingen zou bijvoorbeeld een maximum aantal VM-gelijktijdigheid van 3 * 24 uur per dag = 72 functie aanroepen.

JavaScript en Java-functies in het geheugen, de hele blob laden en C# functies dat doen als u met maken `string`, `Byte[]`, of POCO.

## <a name="trigger---polling"></a>Trigger - polling

Als de blob-container die worden bewaakt meer dan 10.000 blobs bevat, logboekbestanden de Functions-runtime-scans om te bekijken voor nieuwe of gewijzigde blobs. Dit proces kan leiden tot vertragingen. Een functie mogelijk niet ophalen geactiveerd totdat enkele minuten of langer nadat de blob is gemaakt. Bovendien [opslaglogboeken worden gemaakt op een "aanbevolen inspanning"](/rest/api/storageservices/About-Storage-Analytics-Logging) uit te voeren. Er is geen garantie dat alle gebeurtenissen worden vastgelegd. Onder bepaalde omstandigheden, logboeken gemist. Als u sneller of betrouwbaarder blob verwerking nodig hebt, kunt u het maken van een [wachtrijbericht](../storage/queues/storage-dotnet-how-to-use-queues.md) bij het maken van de blob. Gebruik vervolgens een [wachtrijtrigger](functions-bindings-storage-queue.md) in plaats van een blobtrigger voor het verwerken van de blob. Een andere optie is het gebruik van Event Grid; Zie de zelfstudie [automatisch vergroten of verkleinen geüploade afbeeldingen met behulp van Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).

## <a name="input"></a>Invoer

Een Invoerbinding van Blob storage gebruiken om te lezen van blobs.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#input---c-example)
* [C# script (.csx)](#input---c-script-example)
* [Java](#input---java-examples)
* [JavaScript](#input---javascript-example)
* [Python](#input---python-example)

### <a name="input---c-example"></a>Invoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die gebruikmaakt van een wachtrijtrigger en een binding van de blob-invoerbestanden. Bericht uit de wachtrij bevat de naam van de blob en de functie registreert de grootte van de blob.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```        

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

<!--Same example for input and output. -->

Het volgende voorbeeld ziet u blob-invoer- en uitvoerbindingen in een *function.json* bestand en [C#-script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de bindingen. De functie maakt een kopie van een blob tekst. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-exemplaar*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de naam van de blob in de `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input---javascript-example"></a>Invoer - JavaScript-voorbeeld

<!--Same example for input and output. -->

Het volgende voorbeeld ziet u blob-invoer- en uitvoerbindingen in een *function.json* bestand en [JavaScript-code](functions-reference-node.md) die gebruikmaakt van de bindingen. De functie maakt een kopie van een blob. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-exemplaar*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de naam van de blob in de `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

### <a name="input---python-example"></a>Invoer - Python-voorbeeld

<!--Same example for input and output. -->

Het volgende voorbeeld ziet u blob-invoer- en uitvoerbindingen in een *function.json* bestand en [Python-code](functions-reference-python.md) die gebruikmaakt van de bindingen. De functie maakt een kopie van een blob. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-exemplaar*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de naam van de blob in de `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de Python-code:

```python
import logging
import azure.functions as func

def main(queuemsg: func.QueueMessage, inputblob: func.InputStream) -> func.InputStream:
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    return inputblob
```

### <a name="input---java-examples"></a>Invoer - Java-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* [HTTP-trigger, zoekt u naar blob-naam van de query-tekenreeks](#http-trigger-look-up-blob-name-from-query-string-java)
* [Trigger in de wachtrij, blob-naam van de wachtrijbericht ontvangen](#queue-trigger-receive-blob-name-from-queue-message-java)

#### <a name="http-trigger-look-up-blob-name-from-query-string-java"></a>HTTP-trigger, zoekt u naar blob-naam van de query-tekenreeks (Java)

 Het volgende voorbeeld ziet u een Java-functie die gebruikmaakt van de ```HttpTrigger``` aantekening voor het ontvangen van een parameter met de naam van een bestand in een blob storage-container. De ```BlobInput``` aantekening vervolgens het bestand gelezen en wordt de inhoud ervan wordt doorgegeven aan de functie als een ```byte[]```.

```java
  @FunctionName("getBlobSizeHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage blobSize(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    final ExecutionContext context) {
      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-receive-blob-name-from-queue-message-java"></a>Trigger in de wachtrij, ontvangen van de blob-naam van wachtrijbericht (Java)

 Het volgende voorbeeld ziet u een Java-functie die gebruikmaakt van de ```QueueTrigger``` aantekening om een bericht met de naam van een bestand in een blob storage-container te ontvangen. De ```BlobInput``` aantekening vervolgens het bestand gelezen en wordt de inhoud ervan wordt doorgegeven aan de functie als een ```byte[]```.

```java
  @FunctionName("getBlobSize")
  @StorageAccount("Storage_Account_Connection_String")
  public void blobSize(
    @QueueTrigger(
      name = "filename", 
      queueName = "myqueue-items-sample") 
    String filename,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{queueTrigger}") 
    byte[] content,
    final ExecutionContext context) {
      context.getLogger().info("The size of \"" + filename + "\" is: " + content.length + " bytes");
  }
```

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@BlobInput` aantekening op parameters waarvan de waarde afkomstig van een blob zijn kan.  Deze aantekening kan worden gebruikt met systeemeigen Java-typen, pojo's of null-waarden met behulp van `Optional<T>`.

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

De constructor van het kenmerk wordt het pad naar de blob en een `FileAccess` parameter die aangeeft dat lezen of schrijven, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

U kunt de `StorageAccount` kenmerk om op te geven van de storage-account op het niveau van klasse, methode of parameter. Zie voor meer informatie, [Trigger - kenmerken](#trigger---attributes).

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Blob` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `blob`. |
|**direction** | N.v.t. | Moet worden ingesteld op `in`. Uitzonderingen worden vermeld in de [gebruik](#input---usage) sectie. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de blob in functiecode aan te geven.|
|**path** |**BlobPath** | Het pad naar de blob. |
|**verbinding** |**Verbinding**| De naam van een app-instelling met de [opslagverbindingsreeks](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-azure-storage-account) moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.<br><br>De verbindingsreeks moet zijn voor een algemeen opslagaccount, niet een [alleen-blob storage-account](../storage/common/storage-account-overview.md#types-of-storage-accounts).|
|N.v.t. | **Toegang** | Geeft aan of u wordt lezen of schrijven. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Invoer - gebruik

In C# en C#-script, kunt u de volgende parametertypen voor de blob-Invoerbinding:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> vereist 'inout' binding `direction` in *function.json* of `FileAccess.ReadWrite` in een C#-klassenbibliotheek vormt.

Als u probeert te binden aan een van de Storage-SDK-typen en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

Binden aan `string` of `Byte[]` wordt alleen aanbevolen als de blob klein, is zoals de gehele blob-inhoud in het geheugen worden geladen. Over het algemeen is het beter gebruik van een `Stream` of `CloudBlockBlob` type. Zie voor meer informatie, [gelijktijdigheid-en geheugengebruik](#trigger---concurrency-and-memory-usage) eerder in dit artikel.

Toegang tot de blob via in JavaScript, `context.bindings.<name from function.json>`.

## <a name="output"></a>Uitvoer

Gebruik Blob storage-uitvoerbindingen schrijven blobs.

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [Java](#output---java-examples)
* [JavaScript](#output---javascript-example)
* [Python](#output---python-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die gebruikmaakt van een blobtrigger en twee uitvoer blob-bindingen. De functie wordt geactiveerd door het maken van een afbeeldingsblob die in de *voorbeeldafbeeldingen* container. Grootte voor kleine en middelgrote exemplaren van de blob van de installatiekopie wordt gemaakt.

```csharp
[FunctionName("ResizeImage")]
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

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

<!--Same example for input and output. -->

Het volgende voorbeeld ziet u blob-invoer- en uitvoerbindingen in een *function.json* bestand en [C#-script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de bindingen. De functie maakt een kopie van een blob tekst. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-exemplaar*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de naam van de blob in de `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

<!--Same example for input and output. -->

Het volgende voorbeeld ziet u blob-invoer- en uitvoerbindingen in een *function.json* bestand en [JavaScript-code](functions-reference-node.md) die gebruikmaakt van de bindingen. De functie maakt een kopie van een blob. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-exemplaar*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de naam van de blob in de `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

### <a name="output---python-example"></a>Uitvoer - Python-voorbeeld

<!--Same example for input and output. -->

Het volgende voorbeeld ziet u blob-invoer- en uitvoerbindingen in een *function.json* bestand en [Python-code](functions-reference-python.md) die gebruikmaakt van de bindingen. De functie maakt een kopie van een blob. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-exemplaar*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de naam van de blob in de `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "outputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de Python-code:

```python
import logging
import azure.functions as func

def main(queuemsg: func.QueueMessage, inputblob: func.InputStream,
         outputblob: func.Out[func.InputStream]):
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    outputblob.set(inputblob)
```

### <a name="output---java-examples"></a>Uitvoer - Java-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* [HTTP-trigger, met behulp van OutputBinding](#http-trigger-using-outputbinding-java)
* [Wachtrijtrigger, met behulp van de retourwaarde van functie](#queue-trigger-using-function-return-value-java)

#### <a name="http-trigger-using-outputbinding-java"></a>HTTP-trigger, met behulp van OutputBinding (Java)

 Het volgende voorbeeld ziet u een Java-functie die gebruikmaakt van de ```HttpTrigger``` aantekening voor het ontvangen van een parameter met de naam van een bestand in een blob storage-container. De ```BlobInput``` aantekening vervolgens het bestand gelezen en wordt de inhoud ervan wordt doorgegeven aan de functie als een ```byte[]```. De ```BlobOutput``` aantekening wordt gebonden aan ```OutputBinding outputItem```, die vervolgens door de functie wordt gebruikt om te schrijven van de inhoud van de invoerblob naar de geconfigureerde storage-container.

```java
  @FunctionName("copyBlobHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage copyBlobHttp(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    @BlobOutput(
      name = "target", 
      path = "myblob/{Query.file}-CopyViaHttp")
    OutputBinding<String> outputItem,
    final ExecutionContext context) {
      // Save blob to outputItem
      outputItem.setValue(new String(content, StandardCharsets.UTF_8));

      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-using-function-return-value-java"></a>Wachtrijtrigger, met behulp van de retourwaarde van functie (Java)

 Het volgende voorbeeld ziet u een Java-functie die gebruikmaakt van de ```QueueTrigger``` aantekening om een bericht met de naam van een bestand in een blob storage-container te ontvangen. De ```BlobInput``` aantekening vervolgens het bestand gelezen en wordt de inhoud ervan wordt doorgegeven aan de functie als een ```byte[]```. De ```BlobOutput``` aantekening wordt gebonden aan de geretourneerde waarde van de functie, die vervolgens wordt gebruikt door de runtime voor het schrijven van de inhoud van de invoerblob naar de geconfigureerde storage-container.

```java
  @FunctionName("copyBlobQueueTrigger")
  @StorageAccount("Storage_Account_Connection_String")
  @BlobOutput(
    name = "target", 
    path = "myblob/{queueTrigger}-Copy")
  public String copyBlobQueue(
    @QueueTrigger(
      name = "filename", 
      dataType = "string",
      queueName = "myqueue-items") 
    String filename,
    @BlobInput(
      name = "file", 
      path = "samples-workitems/{queueTrigger}") 
    String content,
    final ExecutionContext context) {
      context.getLogger().info("The content of \"" + filename + "\" is: " + content);
      return content;
  }
```

 In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@BlobOutput` aantekening op functieparameters waarvan de waarde kan worden geschreven naar een object in blob-opslag.  Het parametertype moet `OutputBinding<T>`, waarbij T een systeemeigen Java-type of een POJO.

## <a name="output---attributes"></a>Uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

De constructor van het kenmerk wordt het pad naar de blob en een `FileAccess` parameter die aangeeft dat lezen of schrijven, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - voorbeeld met C#](#output---c-example).

U kunt de `StorageAccount` kenmerk om op te geven van de storage-account op het niveau van klasse, methode of parameter. Zie voor meer informatie, [Trigger - kenmerken](#trigger---attributes).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Blob` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `blob`. |
|**direction** | N.v.t. | Moet worden ingesteld op `out` voor een Uitvoerbinding. Uitzonderingen worden vermeld in de [gebruik](#output---usage) sectie. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de blob in functiecode aan te geven.  Ingesteld op `$return` om te verwijzen naar de geretourneerde waarde van de functie.|
|**path** |**BlobPath** | Het pad naar de blobco. |
|**verbinding** |**Verbinding**| De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.<br><br>De verbindingsreeks moet zijn voor een algemeen opslagaccount, niet een [alleen-blob storage-account](../storage/common/storage-account-overview.md#types-of-storage-accounts).|
|N.v.t. | **Toegang** | Geeft aan of u wordt lezen of schrijven. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uitvoer - gebruik

In C# en C#-script, kunt u binden aan de volgende typen blobs schrijven:

* `TextWriter`
* `out string`
* `out Byte[]`
* `CloudBlobStream`
* `Stream`
* `CloudBlobContainer`<sup>1</sup>
* `CloudBlobDirectory`
* `ICloudBlob`<sup>2</sup>
* `CloudBlockBlob`<sup>2</sup>
* `CloudPageBlob`<sup>2</sup>
* `CloudAppendBlob`<sup>2</sup>

<sup>1</sup> is vereist 'in' binding `direction` in *function.json* of `FileAccess.Read` in een C#-klassenbibliotheek vormt. U kunt echter het containerobject dat de runtime biedt voor het schrijven van bewerkingen, zoals blobs uploaden naar de container gebruiken.

<sup>2</sup> vereist 'inout' binding `direction` in *function.json* of `FileAccess.ReadWrite` in een C#-klassenbibliotheek vormt.

Als u probeert te binden aan een van de Storage-SDK-typen en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

In het async-functies, gebruikt u de geretourneerde waarde of `IAsyncCollector` in plaats van een `out` parameter.

Binden aan `string` of `Byte[]` wordt alleen aanbevolen als de blob klein, is zoals de gehele blob-inhoud in het geheugen worden geladen. Over het algemeen is het beter gebruik van een `Stream` of `CloudBlockBlob` type. Zie voor meer informatie, [gelijktijdigheid-en geheugengebruik](#trigger---concurrency-and-memory-usage) eerder in dit artikel.


Toegang tot de blob via in JavaScript, `context.bindings.<name from function.json>`.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding |  Referentie |
|---|---|
| Blob | [BLOB-foutcodes](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| BLOB-, tabel, wachtrij |  [Foutcodes voor opslag](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB-, tabel, wachtrij |  [Problemen oplossen](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Blob storage trigger](functions-create-storage-blob-triggered-function.md)
--->
