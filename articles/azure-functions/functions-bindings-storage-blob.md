---
title: Azure Functions Blob Storage-bindingen | Microsoft Docs
description: Het gebruik van Azure Storage-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: 79b9dbee89a4e33a1768343b9242d6b2b1118355
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Azure Functions Blob storage-bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe configureren en werken met Azure Blob storage bindingen in de Azure Functions. Azure Functions ondersteunt worden geactiveerd, invoer en uitvoer van de bindingen voor Azure Blob-opslag. Zie functies die beschikbaar in alle bindingen zijn [Azure Functions triggers en bindingen concepten](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Een [blob storage-account](../storage/common/storage-create-storage-account.md#blob-storage-accounts) wordt niet ondersteund. BLOB storage triggers en bindingen vereisen een algemeen opslagaccount. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>BLOB storage triggers en bindingen

Met behulp van de Azure Blob storage-trigger wordt uw functiecode aangeroepen wanneer een nieuwe of bijgewerkte blob wordt gedetecteerd. De blobinhoud worden geleverd als invoer voor de functie.

Definieer een blob storage trigger met de **integreren** tabblad in de portal functies. De portal maakt de volgende definitie in de **bindingen** sectie van *function.json*:

```json
{
    "name": "<The name used to identify the trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

BLOB-invoer en uitvoer bindingen zijn gedefinieerd met behulp van `blob` als het bindingstype:

```json
{
  "name": "<The name used to identify the blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* De `path` ondersteunt de eigenschap binding expressies en filterparameters. Zie [patronen naam](#pattern).
* De `connection` eigenschap moet bevatten de naam van een app-instelling met een verbindingsreeks voor opslag. In de Azure portal, de standaard editor in de **integreren** tabblad dit app-instelling die u configureert wanneer u een opslagaccount selecteren.

> [!NOTE]
> Wanneer u een blob-trigger op een plan verbruik, kunnen er maximaal 10 minuten vertraging bij de verwerking van nieuwe blobs nadat een functie-app niet actief is geworden. Nadat de functie-app wordt uitgevoerd, worden onmiddellijk blobs verwerkt. Overweeg om te voorkomen dat deze initiële vertraging, een van de volgende opties:
> - Gebruik een App Service-abonnement met altijd op ingeschakeld.
> - Gebruik een ander mechanisme voor het activeren van de blob verwerken, zoals een wachtrijbericht met de blob-naam. Zie voor een voorbeeld [wachtrij trigger met blob invoer binding](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Bestandsnaampatronen
U kunt opgeven dat een naampatroon blob in de `path` eigenschap, die een filter of binding-expressie. Zie [Binding expressies en patronen](functions-triggers-bindings.md#binding-expressions-and-patterns).

Bijvoorbeeld als u wilt filteren op blobs die met de tekenreeks 'oorspronkelijke beginnen', gebruiken de volgende definitie. Dit pad zoeken naar een blob met de naam *oorspronkelijke Blob1.txt* in de *invoer* container en de waarde van de `name` variabele in functiecode is `Blob1`.

```json
"path": "input/original-{name}",
```

Als u wilt koppelen aan de blob-bestandsnaam en de extensie afzonderlijk, gebruikmaken van twee patronen. Dit pad ook zoeken naar een blob met de naam *oorspronkelijke Blob1.txt*, en de waarde van de `blobname` en `blobextension` variabelen in de functiecode zijn *oorspronkelijke Blob1* en *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

U kunt het bestandstype blobs beperken met behulp van een vaste waarde voor de bestandsextensie. Gebruik bijvoorbeeld het volgende patroon volgen zodat alleen op PNG-bestanden:

```json
"path": "samples/{name}.png",
```

Accolades zijn speciale tekens in de naam van patronen. Voor blob-namen die accolades hebben met de naam opgeeft, kunt u de accolades met behulp van twee accolades escape. Het volgende voorbeeld wordt gezocht naar een blob met de naam *{20140101}-soundfile.mp3* in de *installatiekopieën* -container, en de `name` variabele waarde in de functiecode *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Trigger metagegevens

De blob-trigger biedt verschillende eigenschappen voor metagegevens. Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies bindingen in andere bindingen of als parameters in uw code. Deze waarden hebben dezelfde betekenis als [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Typ `string`. Het activerende blobpad
- **URI**. Typ `System.Uri`. De blob-URI voor de primaire locatie.
- **Eigenschappen**. Typ `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`. Eigenschappen van de blob.
- **Metagegevens**. Typ `IDictionary<string,string>`. De gebruiker gedefinieerde metagegevens voor de blob.

<a name="receipts"></a>

### <a name="blob-receipts"></a>BLOB ontvangstbevestigingen
De Azure Functions-runtime zorgt ervoor dat er is geen blob-activeringsfunctie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Om te bepalen of een versie van de opgegeven blob is verwerkt, wordt bijgehouden *blob ontvangstbevestigingen*.

Azure Functions winkels blob ontvangstbevestigingen in een container met de naam *webjobs-azure-hosts* in de Azure storage-account voor de functie-app (gedefinieerd door de app-instelling `AzureWebJobsStorage`). De ontvangst van een blob heeft de volgende informatie:

* De geactiveerde functie ('*&lt;functie app-naam >*. Functies.  *&lt;functienaam >*', bijvoorbeeld: 'MyFunctionApp.Functions.CopyBlob')
* De containernaam
* Het blobtype ('BlockBlob' of 'PageBlob')
* De blob-naam
* De ETag (een blob-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

Verwijder de ontvangst van de blob voor blob uit om af te dwingen opnieuw verwerken van een blob, de *webjobs-azure-hosts* container handmatig.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Verwerken van verontreinigde blobs
Als een functie van de trigger blob is mislukt voor een bepaalde functies van Azure-blob pogingen die een totaal van 5 maal standaard functioneren. 

Als alle 5 pogingen mislukken, wordt een bericht met Azure Functions toegevoegd aan een opslagwachtrij met de naam *webjobs-blobtrigger-poison*. Bericht uit de wachtrij voor verontreinigde blobs is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie  *&lt;functie app-naam >*. Functies.  *&lt;functienaam >*)
* BlobType ('BlockBlob' of 'PageBlob')
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

### <a name="blob-polling-for-large-containers"></a>Er wordt gedelegeerd voor grote containers BLOB
Als de blob-container die wordt bewaakt meer dan 10.000 blobs bevat, logboekbestanden de functies runtime scans moeten worden gecontroleerd voor nieuwe of gewijzigde blobs. Dit proces is niet realtime. Een functie mogelijk niet ophalen geactiveerd tot enkele minuten of langer nadat de blob is gemaakt. Bovendien [opslag logboeken worden gemaakt op een 'best effort'](/rest/api/storageservices/About-Storage-Analytics-Logging) basis. Er is geen garantie dat alle gebeurtenissen worden vastgelegd. Onder bepaalde omstandigheden wellicht Logboeken niet opgehaald. Als u sneller of betrouwbaarder blob verwerking vereist, kunt u een [wachtrijbericht](../storage/queues/storage-dotnet-how-to-use-queues.md) bij het maken van de blob. Vervolgens gebruikt u een [wachtrij trigger](functions-bindings-storage-queue.md) in plaats van een blob-trigger voor het verwerken van de blob.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Met behulp van een blob-trigger en invoer binding
In .NET-functies, toegang tot de blobgegevens met een parameter van de methode, zoals `Stream paramName`. Hier `paramName` is de waarde die u hebt opgegeven in de [trigger configuratie](#trigger). Toegang tot de blob-invoerbron gegevens in Node.js-functies, `context.bindings.<name>`.

U kunt in .NET binden aan een van de typen in de onderstaande lijst. Als gebruikt als een invoer-binding, is enkele van deze typen vereisen een `inout` richting in binding *function.json*. Deze richting wordt niet ondersteund door de standard-editor, zodat u de geavanceerde editor moet gebruiken.

* `TextReader`
* `Stream`
* `ICloudBlob`('inout' binding richting vereist)
* `CloudBlockBlob`('inout' binding richting vereist)
* `CloudPageBlob`('inout' binding richting vereist)
* `CloudAppendBlob`('inout' binding richting vereist)

Als tekst blobs worden verwacht, kunt u ook binden aan een .NET `string` type. Dit wordt alleen aanbevolen als de Blobgrootte van de klein is als de volledige blobinhoud in het geheugen geladen. In het algemeen is het raadzaam om het gebruik van een `Stream` of `CloudBlockBlob` type.

## <a name="trigger-sample"></a>Voorbeeld van de trigger
Stel dat u hebt de volgende function.json die de trigger van een blob-opslag definieert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

Zie het voorbeeld taalspecifieke waarmee de inhoud van elke blob die wordt toegevoegd aan de bewaakte container zich aanmeldt.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>Voorbeelden van BLOB-trigger in C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding to a CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Voorbeeld van de trigger in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Binding met een blob uitvoer

.NET-functies, moet u gebruiken een `out string` parameter in uw functiehandtekening of gebruik een van de typen in de volgende lijst. In een Node.js-functies, opent u de uitvoer-blob via `context.bindings.<name>`.

In de .NET-functies kunt u uitvoeren met een van de volgende typen:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Wachtrij-trigger met blob-invoer en uitvoer voorbeeld
Stel dat u hebt de volgende function.json, die definieert een [Queue Storage trigger](functions-bindings-storage-queue.md), een blob-opslag-invoer en uitvoer van een blob-opslag. Let op het gebruik van de `queueTrigger` metagegevenseigenschap. in de blob-invoer en uitvoer `path` eigenschappen:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Zie het voorbeeld taalspecifieke waarmee de blob-invoerbron worden gekopieerd naar de uitvoer-blob.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>Voorbeeld van BLOB-binding in C# #

```cs
// Copy blob from input to output, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out Stream myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>Voorbeeld van BLOB-binding in Node.js

```javascript
// Copy blob from input to output, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

