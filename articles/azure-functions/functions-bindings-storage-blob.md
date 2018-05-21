---
title: Azure Blob storage-bindingen voor Azure Functions
description: Begrijpen hoe Azure Blob storage triggers en bindingen in de Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/12/2018
ms.author: tdykstra
ms.openlocfilehash: f74a44ed1b26458ad77e5de43a67a961aee70ec1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="azure-blob-storage-bindings-for-azure-functions"></a>Azure Blob storage-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met Azure Blob storage bindingen in de Azure Functions. Azure Functions ondersteunt activeren, invoer en uitvoer van de bindingen voor blobs. Dit artikel bevat een sectie voor elke binding:

* [BLOB-trigger](#trigger)
* [BLOB-invoer binding](#input)
* [BLOB-uitvoer binding](#output)

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> De gebeurtenis raster trigger gebruiken in plaats van de Blob-opslag-trigger voor alleen-blob storage-accounts, voor grote schaal of om koude start vertragingen te voorkomen. Zie voor meer informatie de [Trigger](#trigger) sectie. 

## <a name="packages"></a>Pakketten

De Blob-opslag-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet-pakket. De broncode voor het pakket bevindt zich in de [sdk van azure webjobs](https://github.com/Azure/azure-webjobs-sdk/tree/master/src) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-package-versions](../../includes/functions-package-versions.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="trigger"></a>Trigger

De Blob-opslag-trigger wordt een functie gestart wanneer een nieuwe of bijgewerkte blob wordt gedetecteerd. De blobinhoud worden geleverd als invoer voor de functie.

De [gebeurtenis raster trigger](functions-bindings-event-grid.md) ingebouwde ondersteuning voor [blob-gebeurtenissen](../storage/blobs/storage-blob-event-overview.md) en kan ook worden gebruikt een functie wordt gestart wanneer een nieuwe of bijgewerkte blob wordt gedetecteerd. Zie voor een voorbeeld de [afbeelding vergroten of verkleinen met gebeurtenis raster](../event-grid/resize-images-on-storage-blob-upload-event.md) zelfstudie.

Gebeurtenis raster gebruiken in plaats van de Blob-opslag-trigger voor de volgende scenario's:

* Alleen-BLOB storage-accounts
* Grote schaal
* Koude-startvertraging

### <a name="blob-only-storage-accounts"></a>Alleen-BLOB storage-accounts

[Alleen-BLOB storage-accounts](../storage/common/storage-create-storage-account.md#blob-storage-accounts) worden ondersteund voor blob-invoer en uitvoer bindingen maar niet voor blob-triggers. BLOB storage triggers vereisen een algemeen opslagaccount.

### <a name="high-scale"></a>Grote schaal

Grote schaal los worden gedefinieerd als containers die meer dan 100.000 blobs of opslagaccounts die beschikken over meer dan 100 blob updates per seconde.

### <a name="cold-start-delay"></a>Koude-startvertraging

Als uw app in de functie op het plan verbruik is, kunnen er maximaal 10 minuten duren in verwerking van nieuwe blobs als een functie-app niet actief is geworden. Om te voorkomen dat deze vertraging koude start, kunt u overschakelen naar een App Service-abonnement met altijd op ingeschakeld of gebruik een andere triggertype.

### <a name="queue-storage-trigger"></a>Queue storage trigger

Een ander alternatief voor het verwerken van BLOB's is de wachtrij opslag trigger naast gebeurtenis raster, maar heeft geen ingebouwde ondersteuning voor blob-gebeurtenissen. U zou moeten maken van berichten in wachtrij plaatsen bij het maken of bijwerken van blobs. Voor een voorbeeld wordt ervan uitgegaan u dat hebt gedaan, raadpleegt u de [blob invoer binding voorbeeld verderop in dit artikel](#input---example).

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die schrijft een logboek wanneer een blob is toegevoegd of bijgewerkt in de `samples-workitems` container.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [bindende expressie](functions-triggers-bindings.md#binding-expressions-and-patterns) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie [Blob bestandsnaampatronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over de `BlobTrigger` kenmerk, Zie [Trigger - kenmerken](#trigger---attributes).

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een blob-trigger binding in een *function.json* bestand en [C# script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie een logboek wordt geschreven wanneer een blob is toegevoegd of bijgewerkt de `samples-workitems` container.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [bindende expressie](functions-triggers-bindings.md#binding-expressions-and-patterns) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie [Blob bestandsnaampatronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over *function.json* bestand eigenschappen, Zie de [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt C# script-code die wordt gekoppeld aan een `Stream`:

```cs
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Hier volgt C# script-code die wordt gekoppeld aan een `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een blob-trigger binding in een *function.json* bestand en [JavaScript-code](functions-reference-node.md) die gebruikmaakt van de binding. De functie een logboek wordt geschreven wanneer een blob is toegevoegd of bijgewerkt de `samples-workitems` container.

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

De tekenreeks `{name}` in het pad van de trigger blob `samples-workitems/{name}` maakt een [bindende expressie](functions-triggers-bindings.md#binding-expressions-and-patterns) die u in de functiecode gebruiken kunt voor toegang tot de bestandsnaam van de activerende blob. Zie voor meer informatie [Blob bestandsnaampatronen](#trigger---blob-name-patterns) verderop in dit artikel.

Voor meer informatie over *function.json* bestand eigenschappen, Zie de [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken voor het configureren van een blob-trigger:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobTriggerAttribute.cs)

  De constructor van het kenmerk werkt met een tekenreeks die aangeeft van de container om te kijken en optioneel een [blob naampatroon](#trigger---blob-name-patterns). Hier volgt een voorbeeld:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Zie voor een compleet voorbeeld [Trigger - C#-voorbeeld](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Biedt een andere manier om op te geven van de storage-account te gebruiken. De constructor, wordt de naam van een app-instelling met een verbindingsreeks voor opslag. Het kenmerk kan worden toegepast op het parameter, klasseniveau of methode. Het volgende voorbeeld ziet u klasseniveau en methode:

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

Het opslagaccount moet worden gebruikt, wordt bepaald in de volgende volgorde:

* De `BlobTrigger` van het kenmerk `Connection` eigenschap.
* De `StorageAccount` kenmerk toegepast op de dezelfde parameter als de `BlobTrigger` kenmerk.
* De `StorageAccount` kenmerk toegepast op de functie.
* De `StorageAccount` kenmerk toegepast op de klasse.
* Het standaardopslagaccount voor de functie-app (app-instelling 'AzureWebJobsStorage').

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `BlobTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `blobTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**direction** | N.v.t. | moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. Uitzonderingen worden vermeld de [gebruik](#trigger---usage) sectie. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de blob in de functiecode. | 
|**Pad** | **BlobPath** |De container om te controleren.  Kan een [blob naampatroon](#trigger-blob-name-patterns). | 
|**Verbinding** | **Verbinding** | De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.<br><br>De verbindingsreeks moet voor een algemeen opslagaccount niet een [alleen blob storage-account](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

In C# en C# script, kunt u de volgende parametertypen voor de activerende blob:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* Een POCO serialiseerbaar als JSON
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> vereist 'inout' binding `direction` in *function.json* of `FileAccess.ReadWrite` in een C#-klassenbibliotheek.

Het binden aan `string`, `Byte[]`, of POCO wordt alleen aanbevolen als de blob is klein, als de volledige blob-inhoud in het geheugen wordt geladen. In het algemeen is het raadzaam om het gebruik van een `Stream` of `CloudBlockBlob` type. Zie voor meer informatie [gelijktijdigheid van taken en geheugengebruik](#trigger---concurrency-and-memory-usage) verderop in dit artikel.

Toegang tot de blob-invoerbron gegevens in JavaScript, `context.bindings.<name from function.json>`.

## <a name="trigger---blob-name-patterns"></a>Trigger - patronen voor blob-naam

U kunt opgeven dat een naampatroon blob in de `path` eigenschap in *function.json* of in de `BlobTrigger` kenmerkconstructor. Het naampatroon mag een [filter of binding expressie](functions-triggers-bindings.md#binding-expressions-and-patterns). De volgende secties bevatten voorbeelden.

### <a name="get-file-name-and-extension"></a>Get-bestandsnaam en extensie

Het volgende voorbeeld ziet u hoe u verbinding maken met de blob-bestandsnaam en de extensie afzonderlijk:

```json
"path": "input/{blobname}.{blobextension}",
```
Als de naam van de blob *oorspronkelijke Blob1.txt*, de waarden van de `blobname` en `blobextension` variabelen in de functiecode zijn *oorspronkelijke Blob1* en *txt*.

### <a name="filter-on-blob-name"></a>Filter op de blob-naam

Het volgende voorbeeld-triggers alleen op blobs in de `input` -container die beginnen met de tekenreeks 'oorspronkelijke-':

```json
"path": "input/original-{name}",
```
 
Als de blobnaam van de is *oorspronkelijke Blob1.txt*, de waarde van de `name` variabele in functiecode is `Blob1`.

### <a name="filter-on-file-type"></a>Filteren op bestandstype

Het volgende voorbeeld wordt alleen geactiveerd *PNG* bestanden:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filteren op accolades in bestandsnamen

Accolades in bestandsnamen zoekt, escape voor de accolades met behulp van twee accolades. Het volgende voorbeeld-filters voor blobs die accolades hebben met de naam:

```json
"path": "images/{{20140101}}-{name}",
```

Als de naam van de blob  *{20140101}-soundfile.mp3*, wordt de `name` variabele waarde in de functiecode *soundfile.mp3*. 

## <a name="trigger---metadata"></a>Trigger - metagegevens

De blob-trigger biedt verschillende eigenschappen voor metagegevens. Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies voor gegevensbinding in andere bindingen of als parameters in uw code. Deze waarden hebben dezelfde heeft als de [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet) type.

|Eigenschap  |Type  |Beschrijving  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Het pad naar de activerende blob.|
|`Uri`|`System.Uri`|De blob-URI voor de primaire locatie.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties)|Eigenschappen van de blob. |
|`Metadata` |`IDictionary<string,string>`|De gebruiker gedefinieerde metagegevens voor de blob.|

Bijvoorbeeld Meld de volgende C#-script en JavaScript-voorbeelden u het pad naar de activerende blob, met inbegrip van de container:

```csharp
public static void Run(string myBlob, string blobTrigger, TraceWriter log)
{
    log.Info($"Full blob path: {blobTrigger}");
} 
```

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

## <a name="trigger---blob-receipts"></a>Trigger - blob ontvangstbevestigingen

De Azure Functions-runtime zorgt ervoor dat er is geen blob-activeringsfunctie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Om te bepalen of een versie van de opgegeven blob is verwerkt, wordt bijgehouden *blob ontvangstbevestigingen*.

Azure Functions winkels blob ontvangstbevestigingen in een container met de naam *webjobs-azure-hosts* in de Azure storage-account voor de functie-app (gedefinieerd door de app-instelling `AzureWebJobsStorage`). De ontvangst van een blob heeft de volgende informatie:

* De geactiveerde functie ('*&lt;functie app-naam >*. Functies.  *&lt;functienaam >*', bijvoorbeeld: 'MyFunctionApp.Functions.CopyBlob')
* De containernaam
* Het blobtype ('BlockBlob' of 'PageBlob')
* De blob-naam
* De ETag (een blob-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

Verwijder de ontvangst van de blob voor blob uit om af te dwingen opnieuw verwerken van een blob, de *webjobs-azure-hosts* container handmatig.

## <a name="trigger---poison-blobs"></a>Trigger - verontreinigd blobs

Als een functie van de trigger blob is mislukt voor een bepaalde functies van Azure-blob pogingen die een totaal van 5 maal standaard functioneren. 

Als alle 5 pogingen mislukken, wordt een bericht met Azure Functions toegevoegd aan een opslagwachtrij met de naam *webjobs-blobtrigger-poison*. Bericht uit de wachtrij voor verontreinigde blobs is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie  *&lt;functie app-naam >*. Functies.  *&lt;functienaam >*)
* BlobType ('BlockBlob' of 'PageBlob')
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

## <a name="trigger---concurrency-and-memory-usage"></a>Trigger - gelijktijdigheid van taken en geheugengebruik

De blob-trigger een wachtrij wordt intern gebruikt, dus het maximum aantal gelijktijdige functie aanroepen wordt bepaald door de [wachtrijen configuratie in host.json](functions-host-json.md#queues). De standaardinstellingen beperken gelijktijdigheid van 24 aanroepen. Deze beperking geldt afzonderlijk voor elke functie die gebruikmaakt van een blob-trigger.

[Het plan verbruik](functions-scale.md#how-the-consumption-plan-works) een functie-app op een virtuele machine (VM) 1,5 GB geheugen beperkt. Geheugen wordt gebruikt door elk gelijktijdig worden uitgevoerd exemplaar van de functie en door de runtime van Functions zelf. Als een functie blob geactiveerd wordt de hele blob in het geheugen geladen, is de maximale hoeveelheid geheugen die wordt gebruikt door deze functie voor blobs 24 * maximale blob-grootte. Voor een functie-app met drie blob-geactiveerde functies en de standaardinstellingen zou bijvoorbeeld een maximum aantal VM-gelijktijdigheid van 3 * 24 = 72 functie aanroepen.

JavaScript-functies de hele blob in het geheugen geladen en C#-functies doen als u verbinding met maken `string`, `Byte[]`, of POCO.

## <a name="trigger---polling"></a>Activeren - polling

Als de blob-container die wordt bewaakt meer dan 10.000 blobs bevat, logboekbestanden de functies runtime scans moeten worden gecontroleerd voor nieuwe of gewijzigde blobs. Dit proces kan leiden tot vertragingen. Een functie mogelijk niet ophalen geactiveerd tot enkele minuten of langer nadat de blob is gemaakt. Bovendien [opslag logboeken worden gemaakt op een 'best effort'](/rest/api/storageservices/About-Storage-Analytics-Logging) basis. Er is geen garantie dat alle gebeurtenissen worden vastgelegd. Onder bepaalde omstandigheden wellicht Logboeken niet opgehaald. Als u sneller of betrouwbaarder blob verwerking vereist, kunt u een [wachtrijbericht](../storage/queues/storage-dotnet-how-to-use-queues.md) bij het maken van de blob. Gebruik vervolgens een [wachtrij trigger](functions-bindings-storage-queue.md) in plaats van een blob-trigger voor het verwerken van de blob. Een andere optie is het gebruik van de gebeurtenis raster; Zie de zelfstudie [automatisch vergroten/verkleinen geüpload met behulp van gebeurtenis raster installatiekopieën](../event-grid/resize-images-on-storage-blob-upload-event.md).

## <a name="input"></a>Invoer

Gebruik een binding van Blob-opslag-invoer blobs lezen.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#input---c-example)
* [C# script (.csx)](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example"></a>Invoer - C#-voorbeeld

Het volgende voorbeeld is een [C#-functie](functions-dotnet-class-library.md) die gebruikmaakt van een trigger wachtrij en een binding van de blob-invoerbron. Bericht uit de wachtrij bevat de naam van de blob, en de functie Logboeken de grootte van de blob.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```        

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

<!--Same example for input and output. -->

Het volgende voorbeeld ziet blob-invoer en uitvoer bindingen in een *function.json* bestand en [C# script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de bindingen. De functie wordt een kopie van een blob tekst. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-kopie*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de blob-naam in de `path` eigenschappen:

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input---javascript-example"></a>Invoer - JavaScript-voorbeeld

<!--Same example for input and output. -->

Het volgende voorbeeld ziet blob-invoer en uitvoer bindingen in een *function.json* bestand en [JavaScript-code](functions-reference-node.md) die gebruikmaakt van de bindingen. De functie wordt een kopie van een blob. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-kopie*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de blob-naam in de `path` eigenschappen:

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs).

De constructor van het kenmerk wordt het pad naar de blob en een `FileAccess` parameter die aangeeft dat lezen of schrijven, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

U kunt de `StorageAccount` het kenmerk met de storage-account op niveau van de klasse, methode of parameter opgeven. Zie voor meer informatie [Trigger - kenmerken](#trigger---attributes).

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Blob` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `blob`. |
|**direction** | N.v.t. | moet worden ingesteld op `in`. Uitzonderingen worden vermeld de [gebruik](#input---usage) sectie. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de blob in de functiecode.|
|**Pad** |**BlobPath** | Het pad naar de blob. | 
|**Verbinding** |**Verbinding**| De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.<br><br>De verbindingsreeks moet voor een algemeen opslagaccount niet een [alleen blob storage-account](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|
|N.v.t. | **Toegang** | Geeft aan of u wordt lezen of schrijven. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Invoer - gebruik

In C# en C# script, kunt u de volgende parametertypen voor de binding van blob-invoer:

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

<sup>1</sup> vereist 'inout' binding `direction` in *function.json* of `FileAccess.ReadWrite` in een C#-klassenbibliotheek.

Het binden aan `string` of `Byte[]` wordt alleen aanbevolen als de Blobgrootte van de klein is als de hele blobinhoud in het geheugen geladen. In het algemeen is het raadzaam om het gebruik van een `Stream` of `CloudBlockBlob` type. Zie voor meer informatie [gelijktijdigheid van taken en geheugengebruik](#trigger---concurrency-and-memory-usage) eerder in dit artikel.

In JavaScript, toegang heeft tot de blob-gegevens met `context.bindings.<name from function.json>`.

## <a name="output"></a>Uitvoer

Gebruik Blob storage uitvoer bindingen blobs schrijven.

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld is een [C#-functie](functions-dotnet-class-library.md) die gebruikmaakt van een blob-trigger en twee uitvoer blob-bindingen. De functie wordt geactiveerd door het maken van een blob installatiekopie in de *voorbeeld installatiekopieën* container. Grootte voor kleine en middelgrote kopieën van de blob van de installatiekopie wordt gemaakt. 

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

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

<!--Same example for input and output. -->

Het volgende voorbeeld ziet blob-invoer en uitvoer bindingen in een *function.json* bestand en [C# script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de bindingen. De functie wordt een kopie van een blob tekst. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-kopie*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de blob-naam in de `path` eigenschappen:

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

<!--Same example for input and output. -->

Het volgende voorbeeld ziet blob-invoer en uitvoer bindingen in een *function.json* bestands- en [JavaScript-code] (functies verwijzing node.md) die gebruikmaakt van de bindingen. De functie wordt een kopie van een blob. De functie wordt geactiveerd door een wachtrijbericht met de naam van de blob te kopiëren. De nieuwe blob heet *{originalblobname}-kopie*.

In de *function.json* -bestand, de `queueTrigger` metagegevenseigenschap wordt gebruikt om op te geven van de blob-naam in de `path` eigenschappen:

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs).

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

U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - C#-voorbeeld](#output---c-example).

U kunt de `StorageAccount` het kenmerk met de storage-account op niveau van de klasse, methode of parameter opgeven. Zie voor meer informatie [Trigger - kenmerken](#trigger---attributes).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Blob` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `blob`. |
|**direction** | N.v.t. | Moet worden ingesteld op `out` voor een binding van de uitvoer. Uitzonderingen worden vermeld de [gebruik](#output---usage) sectie. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de blob in de functiecode.  Ingesteld op `$return` om te verwijzen naar de retourwaarde van de functie.|
|**Pad** |**BlobPath** | Het pad naar de blob. | 
|**Verbinding** |**Verbinding**| De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.<br><br>De verbindingsreeks moet voor een algemeen opslagaccount niet een [alleen blob storage-account](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|
|N.v.t. | **Toegang** | Geeft aan of u wordt lezen of schrijven. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik

In C# en C# script, kunt u binden aan de volgende typen blobs schrijven:

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

<sup>1</sup> 'in' binding vereist `direction` in *function.json* of `FileAccess.Read` in een C#-klassenbibliotheek. U kunt echter het containerobject dat de runtime biedt voor het schrijven van bewerkingen, zoals het uploaden van BLOB's naar de container gebruiken.

<sup>2</sup> vereist 'inout' binding `direction` in *function.json* of `FileAccess.ReadWrite` in een C#-klassenbibliotheek.

Gebruik in async-functies, de retourwaarde of `IAsyncCollector` in plaats van een `out` parameter.

Het binden aan `string` of `Byte[]` wordt alleen aanbevolen als de Blobgrootte van de klein is als de hele blobinhoud in het geheugen geladen. In het algemeen is het raadzaam om het gebruik van een `Stream` of `CloudBlockBlob` type. Zie voor meer informatie [gelijktijdigheid van taken en geheugengebruik](#trigger---concurrency-and-memory-usage) eerder in dit artikel.


In JavaScript, toegang heeft tot de blob-gegevens met `context.bindings.<name from function.json>`.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding |  Referentie |
|---|---|
| Blob | [BLOB-foutcodes](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| BLOB, Table, wachtrij |  [Foutcodes voor opslag](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, Table, wachtrij |  [Problemen oplossen](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een Quick Start die gebruikmaakt van een Blob storage-trigger](functions-create-storage-blob-triggered-function.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
