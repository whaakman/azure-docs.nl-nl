---
title: Externe bestand bindingen voor Azure Functions (experimentele)
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
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: 4e9c2c336df465d7488de84bd2a02cc5d9e42f30
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Azure Functions-bestand met externe Bindingen (experimentele)
Dit artikel laat zien hoe u kunt bestanden van verschillende aanbieders van SaaS (zoals Dropbox of Google Drive) in Azure Functions bewerken. Azure Functions ondersteunt activeren, invoer en uitvoer van de bindingen voor externe bestanden. Deze bindingen API-verbindingen met SaaS-providers maken of bestaande API-verbindingen van de resourcegroep van de functie-App gebruiken.

> [!IMPORTANT]
> Het externe bestand-bindingen zijn experimentele en mogelijk nooit in het algemeen beschikbaar (GA) status bereikt. Ze zijn opgenomen in Azure alleen 1.x fungeert, en er zijn geen plannen om toe te voegen aan de Azure Functions 2.x. Voor scenario's waarvoor toegang tot gegevens in de SaaS-providers, kunt u overwegen [logische apps die gebruikmaken van functies](functions-twitter-email.md). Zie de [connector Logic Apps-bestandssysteem](../logic-apps/logic-apps-using-file-connector.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="available-file-connections"></a>Beschikbare verbindingen

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
> Externe verbindingen van het bestand kunnen ook worden gebruikt [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="trigger"></a>Trigger

De trigger-bestand met externe kunt u een externe map bewaken en de functiecode uitvoeren als er wijzigingen worden gedetecteerd.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#-script](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een extern bestand trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie registreert de inhoud van elk bestand dat wordt toegevoegd aan de bewaakte map.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

Dit zijn de bindingsgegevens de *function.json* bestand:

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

Hier volgt de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|---------|----------------------|
|**type** | moet worden ingesteld op `apiHubFileTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode. | 
|**verbinding**| Identificeert de app-instelling die de verbindingsreeks opslaat. De app-instelling wordt automatisch gemaakt wanneer u een verbinding in de integreren gebruikersinterface in de Azure portal toevoegen.|
|**pad** | De map voor het bewaken van, en desgewenst een bestandsnaampatroon.|

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

## <a name="trigger---usage"></a>Trigger - gebruik

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

## <a name="trigger---poison-files"></a>Trigger - poison-bestanden

Wanneer een bestand met externe activeringsfunctie mislukt pogingen Azure Functions die functie maximaal 5 maal standaard (inclusief de eerste poging) voor een bepaald bestand.
Als alle 5 pogingen mislukken, wordt een bericht met functies toegevoegd aan een opslagwachtrij met de naam *webjobs-apihubtrigger-poison*. Bericht uit de wachtrij voor verontreinigde bestanden is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie  *&lt;functie app-naam >*. Functies.  *&lt;functienaam >*)
* Bestandstype
* Mapnaam
* Bestandsnaam
* ETag (een bestand versie-id, bijvoorbeeld: '0x8D1DC6E70A277EF')

## <a name="input"></a>Invoer

De invoer binding Azure extern bestand kunt u een bestand te gebruiken vanaf een externe map in de functie.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#-script](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

Het volgende voorbeeld ziet extern bestand invoer en uitvoer bindingen in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie wordt een bestand voor invoer kopieert naar een bestand voor uitvoer.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

Het volgende voorbeeld ziet extern bestand invoer en uitvoer bindingen in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie wordt een bestand voor invoer kopieert naar een bestand voor uitvoer.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

Hier volgt de JavaScript-code:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|---------|----------------------|
|**type** | moet worden ingesteld op `apiHubFile`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode. | 
|**verbinding**| Identificeert de app-instelling die de verbindingsreeks opslaat. De app-instelling wordt automatisch gemaakt wanneer u een verbinding in de integreren gebruikersinterface in de Azure portal toevoegen.|
|**pad** | Moet de naam van de map en de bestandsnaam bevatten. Als u hebt bijvoorbeeld een [wachtrij trigger](functions-bindings-storage-queue.md) in de functie, kunt u `"path": "samples-workitems/{queueTrigger}"` om te verwijzen naar een bestand in de `samples-workitems` map met een naam die overeenkomt met de bestandsnaam in het bericht trigger opgegeven.   

## <a name="input---usage"></a>Invoer - gebruik

In de C# functies, bindt u tot de gegevens van het bestand voor invoer met behulp van een benoemde parameter in de functiehandtekening, zoals `<T> <name>`. `T`is het gegevenstype dat u wilt deserialiseren van de gegevens in, en `name` is de naam die u hebt opgegeven in de invoer binding. In een Node.js-functies, opent u het invoerbestand gegevens met `context.bindings.<name>`.

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

## <a name="output"></a>Uitvoer

De Azure bestand met externe uitvoer binding kunt u bestanden naar een externe map in uw functie te schrijven.

## <a name="output---example"></a>Output - voorbeeld

Zie de [invoer binding voorbeeld](#input---example).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand.

|de eigenschap Function.JSON | Beschrijving|
|---------|---------|----------------------|
|**type** | moet worden ingesteld op `apiHubFile`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | moet worden ingesteld op `out`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | De naam van de variabele die staat voor de gebeurtenis in de functiecode. | 
|**verbinding**| Identificeert de app-instelling die de verbindingsreeks opslaat. De app-instelling wordt automatisch gemaakt wanneer u een verbinding in de integreren gebruikersinterface in de Azure portal toevoegen.|
|**pad** | Moet de naam van de map en de bestandsnaam bevatten. Als u hebt bijvoorbeeld een [wachtrij trigger](functions-bindings-storage-queue.md) in de functie, kunt u `"path": "samples-workitems/{queueTrigger}"` om te verwijzen naar een bestand in de `samples-workitems` map met een naam die overeenkomt met de bestandsnaam in het bericht trigger opgegeven.   

## <a name="output---usage"></a>Output - gebruik

In de C# functies, bindt u naar het uitvoerbestand met behulp van de benoemde `out` parameter in de functiehandtekening, zoals `out <T> <name>`, waarbij `T` is het gegevenstype dat u wilt serialiseren van de gegevens in, en `name` is de naam die u hebt opgegeven in de uitvoer-binding. In een Node.js-functies, opent u de uitvoer-bestand met `context.bindings.<name>`.

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

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
