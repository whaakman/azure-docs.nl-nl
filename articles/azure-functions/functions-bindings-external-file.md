---
title: Extern bestand bindingen voor Azure Functions (experimenteel)
description: Met behulp van extern bestand bindingen in Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: be2d34202b88d0d424eb23c4e078c2fdc45c6ab6
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093767"
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Azure Functions-bestand met externe Bindingen (experimenteel)
In dit artikel laat zien hoe om bestanden van verschillende SaaS-providers (zoals Dropbox of Google Drive) in Azure Functions te manipuleren. Azure Functions ondersteunt activeren, invoeren en uitvoerbindingen voor externe bestanden. Deze bindingen API-verbindingen met SaaS-providers maken of bestaande API-verbindingen van de resourcegroep van uw functie-App gebruiken.

> [!IMPORTANT]
> De bindingen voor externe bestanden zijn experimenteel en mogelijk nooit over het algemeen beschikbaar is (GA) status bereikt. Ze zijn opgenomen in Azure Functions 1.x en er zijn geen plannen om toe te voegen aan Azure Functions 2.x. Voor scenario's waarvoor toegang tot gegevens in SaaS-providers, kunt u overwegen [logische apps die gebruikmaken van functies](functions-twitter-email.md). Zie de [bestandssysteem voor Logic Apps connector](../logic-apps/logic-apps-using-file-connector.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="available-file-connections"></a>Beschikbare verbindingen

|Connector|Trigger|Invoer|Uitvoer|
|:-----|:---:|:---:|:---:|
|[Box](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive voor Bedrijven](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Externe verbindingen van het bestand kunnen ook worden gebruikt [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="trigger"></a>Trigger

De trigger-bestand met externe kunt u een externe map bewaken en voer uw functiecode aan te geven wanneer er wijzigingen worden gedetecteerd.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#-script](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een extern bestand trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie registreert de inhoud van elk bestand dat wordt toegevoegd aan de bewaakte map.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Dit is de C#-scriptcode:

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een extern bestand trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie registreert de inhoud van elk bestand dat wordt toegevoegd aan de bewaakte map.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Dit is de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|---------|----------------------|
|**type** | Moet worden ingesteld op `apiHubFileTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode aan te geven. | 
|**verbinding**| Hiermee geeft u de app-instelling waarin de verbindingsreeks. De app-instelling wordt automatisch gemaakt wanneer u een verbinding in de UI-integreren in Azure portal toevoegt.|
|**Pad** | De map om te controleren, en eventueel een naampatroon.|

### <a name="name-patterns"></a>De naam van patronen

U kunt opgeven dat een bestandsnaampatroon in de `path` eigenschap. De map waarnaar wordt verwezen moet zich in de SaaS-provider.

Voorbeelden:

```json
"path": "input/original-{name}",
```

Dit pad wordt een bestand met de naam gevonden *oorspronkelijke Bestand1.txt* in de *invoer* map en de waarde van de `name` variabele in functiecode `File1.txt`.

Nog een voorbeeld:

```json
"path": "input/{filename}.{fileextension}",
```

Dit pad vindt ook een bestand met de naam *oorspronkelijke Bestand1.txt*, en de waarde van de `filename` en `fileextension` variabelen in functiecode zou worden *oorspronkelijke bestand1* en *txt* .

U kunt het type van bestanden beperken met behulp van een vaste waarde voor de bestandsextensie. Bijvoorbeeld:

```json
"path": "samples/{name}.png",
```

In dit geval alleen *.png* bestanden in de *voorbeelden* map Activeer de functie.

Accolades zijn speciale tekens in de naam van patronen. Dubbelklik om op te geven van bestandsnamen waarvoor accolades in de naam, de accolades.
Bijvoorbeeld:

```json
"path": "images/{{20140101}}-{name}",
```

Dit pad wordt een bestand met de naam gevonden  *{20140101}-soundfile.mp3* in de *installatiekopieën* map, en de `name` variabele waarde in de functiecode *soundfile.mp3*.

## <a name="trigger---usage"></a>Trigger - gebruik

In C#-functies, u koppelt aan de gegevens van het invoerbestand met behulp van een benoemde parameter in uw functiehandtekening, zoals `<T> <name>`.
Waar `T` is het gegevenstype dat u wilt deserialiseer de gegevens in, en `paramName` is de naam die u hebt opgegeven in de [trigger JSON](#trigger). In Node.js-functies, opent u het invoerbestand gegevens met `context.bindings.<name>`.

Het bestand kan worden gedeserialiseerd in een van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - handig voor gegevens uit een bestand met JSON-geserialiseerd.
  Als u een aangepaste invoertype declareren (bijvoorbeeld `FooType`), Azure Functions probeert te deserialiseren van de JSON-gegevens in het opgegeven type.
* Tekenreeks - handig voor gegevens uit een bestand tekst.

U kunt ook gebonden aan een van de volgende typen in C#-functies, en de Functions-runtime probeert te deserialiseren van de gegevens uit een bestand met behulp van dat type:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

<!--- ## Trigger - file receipts
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

## <a name="trigger---poison-files"></a>Trigger - onverwerkbare bestanden

Wanneer een activeringsfunctie voor een extern bestand is mislukt, pogingen Azure Functions die functie maximaal 5 keer standaard (inclusief de eerste poging) voor een bepaald bestand.
Als alle 5 pogingen mislukken, functies een bericht toevoegt aan een opslagwachtrij met de naam *webjobs-apihubtrigger-poison*. Bericht uit de wachtrij voor beheer van onverwerkbare bestanden is een JSON-object met de volgende eigenschappen:

* FunctionId (in de indeling  *&lt;functie-appnaam >*. Functies.  *&lt;functienaam >*)
* Bestandstype
* Mapnaam
* Bestandsnaam
* ETag (een bestand versie-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

## <a name="input"></a>Invoer

De Azure-bestand met externe Invoerbinding kunt u een bestand te gebruiken vanuit een externe map in uw functie.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#-script](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u extern bestand invoer- en uitvoerbindingen in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie kopieert een bestand voor invoer naar een uitvoerbestand.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

### <a name="input---javascript-example"></a>Invoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u extern bestand invoer- en uitvoerbindingen in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie kopieert een bestand voor invoer naar een uitvoerbestand.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Dit is de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|---------|----------------------|
|**type** | Moet worden ingesteld op `apiHubFile`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode aan te geven. | 
|**verbinding**| Hiermee geeft u de app-instelling waarin de verbindingsreeks. De app-instelling wordt automatisch gemaakt wanneer u een verbinding in de UI-integreren in Azure portal toevoegt.|
|**Pad** | Moet de naam van de map en de bestandsnaam bevatten. Als u hebt bijvoorbeeld een [wachtrijtrigger](functions-bindings-storage-queue.md) in uw functie, kunt u `"path": "samples-workitems/{queueTrigger}"` om te verwijzen naar een bestand in de `samples-workitems` map met de naam die overeenkomt met de naam van het opgegeven in de trigger-bericht.   

## <a name="input---usage"></a>Invoer - gebruik

In C#-functies, u koppelt aan de gegevens van het invoerbestand met behulp van een benoemde parameter in uw functiehandtekening, zoals `<T> <name>`. `T` is het gegevenstype dat u wilt deserialiseer de gegevens in, en `name` is de naam die u hebt opgegeven in de Invoerbinding. In Node.js-functies, opent u het invoerbestand gegevens met `context.bindings.<name>`.

Het bestand kan worden gedeserialiseerd in een van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - handig voor gegevens uit een bestand met JSON-geserialiseerd.
  Als u een aangepaste invoertype declareren (bijvoorbeeld `InputType`), Azure Functions probeert te deserialiseren van de JSON-gegevens in het opgegeven type.
* Tekenreeks - handig voor gegevens uit een bestand tekst.

U kunt ook gebonden aan een van de volgende typen in C#-functies, en de Functions-runtime probeert te deserialiseren van de gegevens uit een bestand met behulp van dat type:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="output"></a>Uitvoer

Het Azure externe bestand Uitvoerbinding kunt u bestanden naar een externe map in de functie te schrijven.

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie de [Invoerbinding voorbeeld](#input---example).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|---------|----------------------|
|**type** | Moet worden ingesteld op `apiHubFile`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | Moet worden ingesteld op `out`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode aan te geven. | 
|**verbinding**| Hiermee geeft u de app-instelling waarin de verbindingsreeks. De app-instelling wordt automatisch gemaakt wanneer u een verbinding in de UI-integreren in Azure portal toevoegt.|
|**Pad** | Moet de naam van de map en de bestandsnaam bevatten. Als u hebt bijvoorbeeld een [wachtrijtrigger](functions-bindings-storage-queue.md) in uw functie, kunt u `"path": "samples-workitems/{queueTrigger}"` om te verwijzen naar een bestand in de `samples-workitems` map met de naam die overeenkomt met de naam van het opgegeven in de trigger-bericht.   

## <a name="output---usage"></a>Uitvoer - gebruik

In de C#-functies, bindt u naar het uitvoerbestand met behulp van de benoemde `out` parameter in uw functiehandtekening, zoals `out <T> <name>`, waarbij `T` is het gegevenstype dat u wilt de gegevens in, en `name` is de naam die u hebt opgegeven in de Uitvoerbinding. In Node.js-functies, opent u het uitvoerbestand met `context.bindings.<name>`.

U kunt schrijven naar het uitvoerbestand met behulp van een van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - nuttig voor JSON-serialisatie.
  Als u een aangepaste uitvoertype declareren (bijvoorbeeld `out OutputType paramName`), Azure Functions wordt geprobeerd om object te serialiseren naar JSON. Als de output-parameter null is wanneer de functie wordt afgesloten, wordt een bestand in de Functions-runtime gemaakt als een null-object.
* Tekenreeks - (`out string paramName`) handig voor gegevens uit een bestand tekst. de Functions-runtime maakt een bestand alleen als de queryreeks-parameter niet gelijk zijn aan nul is wanneer de functie wordt afgesloten.

In C#-functies kunt u ook uitvoeren met een van de volgende typen:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
