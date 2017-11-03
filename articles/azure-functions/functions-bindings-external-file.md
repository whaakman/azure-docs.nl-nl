---
title: Azure Functions-bestand met externe Bindingen (Preview) | Microsoft Docs
description: Met behulp van extern bestand bindingen in de Azure-functies
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 7e3b396d290212d3875385521bd7ae92da196b95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a>Azure Functions-bestand met externe Bindingen (Preview)
Dit artikel laat zien hoe u kunt bestanden van verschillende aanbieders van SaaS (zoals OneDrive, Dropbox) bewerken binnen de functie met behulp van de ingebouwde bindingen. Azure functions ondersteunt activeren, invoer en uitvoer van de bindingen voor extern bestand.

Deze binding API-verbindingen met SaaS-providers maakt of bestaande API-verbindingen van de resourcegroep van de functie-App wordt gebruikt.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Ondersteunde bestand-verbindingen

|Connector|Trigger|Invoer|Uitvoer|
|:-----|:---:|:---:|:---:|
|[Vak](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive voor Bedrijven](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Externe verbindingen van het bestand kunnen ook worden gebruikt [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Bestand met externe binding activeren

De trigger Azure extern bestand kunt u een externe map bewaken en de functiecode uitvoeren als er wijzigingen worden gedetecteerd.

De trigger-bestand met externe gebruikt de volgende JSON-objecten in de `bindings` matrix van function.json

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder to monitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of the following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Bestandsnaampatronen
U kunt opgeven dat een bestandsnaampatroon in de `path` eigenschap. De map waarnaar wordt verwezen, moet bestaan in de SaaS-provider.
Voorbeelden:

```json
"path": "input/original-{name}",
```

Dit pad wilt zoeken naar een bestand met de naam *oorspronkelijke Bestand1.txt* in de *invoer* map en de waarde van de `name` variabele in functiecode zou worden `File1.txt`.

Nog een voorbeeld:

```json
"path": "input/{filename}.{fileextension}",
```

Dit pad zou ook een bestand met de naam vinden *oorspronkelijke Bestand1.txt*, en de waarde van de `filename` en `fileextension` variabelen in de functiecode zou worden *oorspronkelijke File1* en *txt* .

U kunt het type van bestanden beperken met behulp van een vaste waarde voor de bestandsextensie. Bijvoorbeeld:

```json
"path": "samples/{name}.png",
```

In dit geval wordt alleen *PNG* bestanden in de *voorbeelden* map activeringsfunctie.

Accolades zijn speciale tekens in de naam van patronen. Dubbelklik om op te geven bestandsnamen die accolades hebben met de naam, de accolades.
Bijvoorbeeld:

```json
"path": "images/{{20140101}}-{name}",
```

Dit pad wilt zoeken naar een bestand met de naam *{20140101}-soundfile.mp3* in de *installatiekopieën* map, en de `name` variabele waarde in de functiecode *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Verwerken van verontreinigde bestanden
Wanneer een bestand met externe activeringsfunctie mislukt pogingen Azure Functions die functie maximaal 5 maal standaard (inclusief de eerste poging) voor een bepaald bestand.
Als alle 5 pogingen mislukken, wordt een bericht met functies toegevoegd aan een opslagwachtrij met de naam *webjobs-apihubtrigger-poison*. Bericht uit de wachtrij voor verontreinigde bestanden is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie  *&lt;functie app-naam >*. Functies.  *&lt;functienaam >*)
* Bestandstype
* Mapnaam
* Bestandsnaam
* ETag (een bestand versie-id, bijvoorbeeld: '0x8D1DC6E70A277EF')


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Gebruik van de trigger
In de C# functies, bindt u tot de gegevens van het bestand voor invoer met behulp van een benoemde parameter in de functiehandtekening, zoals `<T> <name>`.
Waar `T` is het gegevenstype dat u wilt deserialiseren van de gegevens in, en `paramName` is de naam die u hebt opgegeven in de [activeert JSON](#trigger). In een Node.js-functies, opent u het invoerbestand gegevens met `context.bindings.<name>`.

Het bestand kan worden gedeserialiseerd in een van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - nuttig voor JSON-geserialiseerd bestandsgegevens.
  Als u een aangepaste invoertype declareren (bijvoorbeeld `FooType`), Azure Functions geprobeerd te deserialiseren van de JSON-gegevens in het opgegeven type.
* Tekenreeks - nuttig voor tekst-bestandsgegevens.

U kunt ook binden aan een van de volgende typen in C#-functies, en de runtime van Functions geprobeerd te deserialiseren van de gegevens uit een bestand met behulp van dat type:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Voorbeeld van de trigger
Stel dat u hebt de volgende function.json die de trigger van een extern bestand definieert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Zie het voorbeeld taalspecifieke waarmee de inhoud van elk bestand dat wordt toegevoegd aan de bewaakte map zich aanmeldt.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Gebruik van de trigger in C# #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Gebruik van de trigger in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Bestand met externe invoer binding
De invoer binding Azure extern bestand kunt u een bestand te gebruiken vanaf een externe map in de functie.

De invoer van het externe bestand aan een functie maakt gebruik van de volgende JSON-objecten in de `bindings` matrix van function.json:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Houd rekening met het volgende:

* `path`moet de naam van de map en de bestandsnaam bevatten. Als u hebt bijvoorbeeld een [wachtrij trigger](functions-bindings-storage-queue.md) in de functie, kunt u `"path": "samples-workitems/{queueTrigger}"` om te verwijzen naar een bestand in de `samples-workitems` map met een naam die overeenkomt met de bestandsnaam in het bericht trigger opgegeven.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Invoer-gebruik
In de C# functies, bindt u tot de gegevens van het bestand voor invoer met behulp van een benoemde parameter in de functiehandtekening, zoals `<T> <name>`.
Waar `T` is het gegevenstype dat u wilt deserialiseren van de gegevens in, en `paramName` is de naam die u hebt opgegeven in de [invoer binding](#input). In een Node.js-functies, opent u het invoerbestand gegevens met `context.bindings.<name>`.

Het bestand kan worden gedeserialiseerd in een van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - nuttig voor JSON-geserialiseerd bestandsgegevens.
  Als u een aangepaste invoertype declareren (bijvoorbeeld `InputType`), Azure Functions geprobeerd te deserialiseren van de JSON-gegevens in het opgegeven type.
* Tekenreeks - nuttig voor tekst-bestandsgegevens.

U kunt ook binden aan een van de volgende typen in C#-functies, en de runtime van Functions geprobeerd te deserialiseren van de gegevens uit een bestand met behulp van dat type:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Bestand met externe uitvoer binding
De Azure bestand met externe uitvoer binding kunt u bestanden naar een externe map in uw functie te schrijven.

Het externe bestand uitvoer voor een functie maakt gebruik van de volgende JSON-objecten in de `bindings` matrix van function.json:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Houd rekening met het volgende:

* `path`moet de naam van de map en de naam van het bestand om te schrijven naar bevatten. Als u hebt bijvoorbeeld een [wachtrij trigger](functions-bindings-storage-queue.md) in de functie, kunt u `"path": "samples-workitems/{queueTrigger}"` om te verwijzen naar een bestand in de `samples-workitems` map met een naam die overeenkomt met de bestandsnaam in het bericht trigger opgegeven.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Gebruik voor uitvoer
In de C# functies, bindt u naar het uitvoerbestand met behulp van de benoemde `out` parameter in de functiehandtekening, zoals `out <T> <name>`, waarbij `T` is het gegevenstype dat u wilt serialiseren van de gegevens in, en `paramName` is de naam die u hebt opgegeven in de [uitvoer binding](#output). In een Node.js-functies, opent u de uitvoer-bestand met `context.bindings.<name>`.

U kunt schrijven naar het uitvoerbestand met behulp van een van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - nuttig voor JSON-serialisatie.
  Als u een aangepaste uitvoertype declareren (bijvoorbeeld `out OutputType paramName`), Azure Functions probeert om object te serialiseren naar JSON. Als de output-parameter null is wanneer de functie wordt afgesloten, wordt in de runtime van Functions een bestand gemaakt als een null-object.
* String - (`out string paramName`) nuttig voor tekst-bestandsgegevens. de runtime van Functions maakt een bestand alleen als de tekenreeksparameter niet-null wanneer de functie wordt afgesloten.

In C#-functies kunt u ook uitvoeren met een van de volgende typen:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Invoer en uitvoer-voorbeeld
Stel dat u hebt de volgende function.json, die definieert een [opslag wachtrij trigger](functions-bindings-storage-queue.md), voert u een extern bestand en een bestand met externe uitvoer:

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
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Zie de taalspecifieke steekproef die het invoerbestand gekopieerd naar het uitvoerbestand.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>Gebruik in C# #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>Gebruik in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
