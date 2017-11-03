---
title: Azure DB Cosmos-bindingen voor functies | Microsoft Docs
description: Het gebruik van Azure DB die Cosmos-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.openlocfilehash: d05c0342e771e229a7175570ad227c4359980990
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-cosmos-db-bindings-for-functions"></a>Azure DB Cosmos-bindingen voor functies
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dit artikel wordt uitgelegd hoe u configureert en bindingen van Azure DB die Cosmos code in Azure Functions. Functions ondersteunt activeren, invoer en uitvoer van de bindingen voor Azure Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Zie voor meer informatie over het zonder server computing met Azure Cosmos DB [Azure Cosmos DB: zonder Server database computing met behulp van Azure Functions](..\cosmos-db\serverless-computing-database.md).

<a id="trigger"></a>
<a id="cosmosdbtrigger"></a>

## <a name="azure-cosmos-db-trigger"></a>Azure DB Cosmos-trigger

De Azure Cosmos DB Trigger gebruikt de [Azure Cosmos DB wijzigen Feed](../cosmos-db/change-feed.md) om te luisteren naar wijzigingen meerdere partities. De trigger vereist een tweede collectie dat wordt gebruikt voor het opslaan van _leases_ via de partities.

Zowel de verzameling wordt bewaakt als de verzameling met de leases moet beschikbaar zijn voor de trigger werkt.

De Azure DB die Cosmos-trigger ondersteunt de volgende eigenschappen:

|Eigenschap  |Beschrijving  |
|---------|---------|
|**type** | moet worden ingesteld op `cosmosDBTrigger`. |
|**naam** | De naam van de variabele gebruikt in functiecode die de lijst van documenten met wijzigingen vertegenwoordigt. | 
|**richting** | moet worden ingesteld op `in`. Deze parameter wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**connectionStringSetting** | De naam van een app-instelling met de verbindingsreeks waarmee verbinding met de Azure DB die Cosmos-account wordt bewaakt. |
|**databaseName** | De naam van de Azure DB die Cosmos-database met de verzameling wordt bewaakt. |
|**collectionName** | De naam van de verzameling wordt bewaakt. |
| **leaseConnectionStringSetting** | (Optioneel) De naam van een app-instelling met de verbindingsreeks aan de service die de lease-verzameling bevat. Wanneer niet is ingesteld, de `connectionStringSetting` waarde wordt gebruikt. Deze parameter wordt automatisch ingesteld wanneer de binding in de portal is gemaakt. |
| **leaseDatabaseName** | (Optioneel) De naam van de database waarin de verzameling gebruikt voor het opslaan van leases. Wanneer niet is ingesteld, de waarde van de `databaseName` instelling wordt gebruikt. Deze parameter wordt automatisch ingesteld wanneer de binding in de portal is gemaakt. |
| **leaseCollectionName** | (Optioneel) De naam van de verzameling gebruikt voor het opslaan van leases. Wanneer niet is ingesteld, de waarde `leases` wordt gebruikt. |
| **createLeaseCollectionIfNotExists** | (Optioneel) Als de waarde `true`, de verzameling leases wordt automatisch gemaakt wanneer deze niet al bestaat. De standaardwaarde is `false`. |
| **leaseCollectionThroughput** | (Optioneel) Hiermee definieert u de hoeveelheid Aanvraageenheden toewijzen bij het maken van de verzameling van leases. Deze instelling wordt alleen gebruikt bij `createLeaseCollectionIfNotExists` is ingesteld op `true`. Deze parameter wordt automatisch ingesteld als de binding is gemaakt met behulp van de portal.

>[!NOTE] 
>De verbindingsreeks waarmee verbinding met de verzameling leases moet schrijfmachtigingen hebben.

Deze eigenschappen kunnen worden ingesteld op het tabblad integreren voor de functie in de Azure portal of door het bewerken van de `function.json` projectbestand.

## <a name="using-an-azure-cosmos-db-trigger"></a>Een Azure DB die Cosmos-trigger gebruikt

Deze sectie bevat voorbeelden van het gebruik van de Azure DB die Cosmos-trigger. De voorbeelden wordt ervan uitgegaan dat de metagegevens van een trigger die op het volgende lijkt:

```json
{
  "type": "cosmosDBTrigger",
  "name": "documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "<connection-app-setting>",
  "databaseName": "Tasks",
  "collectionName": "Items",
  "createLeaseCollectionIfNotExists": true
}
```
 
Zie voor een voorbeeld van een trigger Azure Cosmos DB maken vanuit een functie-app in de portal [maken van een functie die wordt geactiveerd door Azure Cosmos DB](functions-create-cosmos-db-triggered-function.md). 

### <a name="trigger-sample-in-c"></a>Voorbeeld van de trigger in C# #
```cs 
    #r "Microsoft.Azure.Documents.Client"
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using System;
    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Verbose("Documents modified " + documents.Count);
        log.Verbose("First document Id " + documents[0].Id);
    }
```


### <a name="trigger-sample-in-javascript"></a>Voorbeeld van de trigger in JavaScript
```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>DocumentDB-API invoer binding
De invoer DocumentDB API-binding een document Azure Cosmos DB opgehaald en doorgegeven aan de benoemde invoerparameter van de functie. De ID kan worden bepaald document is gebaseerd op de trigger die de functie activeert. 

De invoer DocumentDB API-binding heeft de volgende eigenschappen *function.json*:

|Eigenschap  |Beschrijving  |
|---------|---------|
|**naam**     | Naam van de binding-parameter die het document in de functie vertegenwoordigt.  |
|**type**     | moet worden ingesteld op `documentdb`.        |
|**databaseName** | De database met het document.        |
|**collectionName**  | De naam van de verzameling waarin het document. |
|**ID**     | De ID van het document om op te halen. Deze eigenschap ondersteunt bindingen parameters. Zie voor meer informatie, [binden aan aangepaste eigenschappen voor de invoer in een expressie voor gegevensbinding](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression). |
|**sqlQuery**     | Een Azure Cosmos DB SQL-query die wordt gebruikt voor het ophalen van meerdere documenten. De query ondersteunt runtime bindingen, zoals in het voorbeeld: `SELECT * FROM c where c.departmentId = {departmentId}`.        |
|**verbinding**     |De naam van de app-instelling met de verbindingsreeks voor Azure Cosmos DB.        |
|**richting**     | moet worden ingesteld op `in`.         |

U kunt beide niet instellen de **id** en **sqlQuery** eigenschappen. Als geen van beide zijn ingesteld, wordt de volledige verzameling worden opgehaald.

## <a name="using-a-documentdb-api-input-binding"></a>Met behulp van een DocumentDB-API invoer binding

* In C# en F # functies, wanneer de functie wordt voltooid, wordt afgesloten worden wijzigingen in het invoerdocument via benoemde invoerparameters automatisch doorgevoerd. 
* In de JavaScript-functies worden niet automatisch updates gesteld bij functie beëindigen. Gebruik in plaats daarvan `context.bindings.<documentName>In` en `context.bindings.<documentName>Out` om updates te maken. Zie de [JavaScript voorbeeld](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Invoer voorbeeld voor één document
Stel dat u hebt de volgende invoer DocumentDB API-binding in de `bindings` matrix van function.json:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

Zie de taalspecifieke-voorbeeldtoepassing die u deze invoer binding gebruikt voor het bijwerken van de tekstwaarde van het document.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Invoer voorbeeld in C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Invoer voorbeeld in F # #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Dit voorbeeld vereist een `project.json` -bestand dat Hiermee geeft u de `FSharp.Interop.Dynamic` en `Dynamitey` NuGet afhankelijkheden:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Om toe te voegen een `project.json` bestand, Zie [F # pakket management](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Invoer voorbeeld in JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Invoer voorbeeld met meerdere documenten

Stel dat u ophalen van meerdere documenten die zijn opgegeven met een SQL-query met behulp van een wachtrij-trigger wilt voor het aanpassen van de queryparameters. 

In dit voorbeeld wordt de trigger wachtrij een parameter opgegeven `departmentId`. Een wachtrijbericht van `{ "departmentId" : "Finance" }` alle records voor de afdeling Financiën zou retourneren. Gebruik de volgende in *function.json*:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Invoer voorbeeld met meerdere documenten in C#

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Invoer voorbeeld met meerdere documenten in JavaScript

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <a id="docdboutput"></a>DocumentDB-API uitvoer binding
De uitvoer van de DocumentDB-API binding kunt schrijven u een nieuw document naar een Azure DB die Cosmos-database. Deze heeft de volgende eigenschappen in *function.json*:

|Eigenschap  |Beschrijving  |
|---------|---------|
|**naam**     | Naam van de binding-parameter die het document in de functie vertegenwoordigt.  |
|**type**     | moet worden ingesteld op `documentdb`.        |
|**databaseName** | De database met de verzameling waarin het document is gemaakt.     |
|**collectionName**  | De naam van de verzameling waarin het document is gemaakt. |
|**createIfNotExists**     | Een Booleaanse waarde die aangeeft of de verzameling is gemaakt als deze nog niet bestaat. De standaardwaarde is *false*. Dit komt doordat de nieuwe verzamelingen worden gemaakt met gereserveerde doorvoer, wat gevolgen heeft kosten. Voor meer informatie raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**verbinding**     |De naam van de app-instelling met de verbindingsreeks voor Azure Cosmos DB.        |
|**richting**     | moet worden ingesteld op `out`.         |

## <a name="using-a-documentdb-api-output-binding"></a>Binding met een DocumentDB-API uitvoer
Deze sectie wordt beschreven hoe u uw DocumentDB-API-uitvoer in uw functiecode binding.

Standaard, wanneer u naar de output-parameter in de functie schrijft is een document gemaakt in uw database. Dit document heeft een automatisch gegenereerde GUID als het document-ID. U kunt de document-ID van uitvoerdocument opgeven door te geven de `id` eigenschap in het JSON-object doorgegeven aan de output-parameter. 

>[!Note]  
>Wanneer u de ID van een bestaand document opgeeft, wordt deze door het nieuwe uitvoerdocument overschreven. 

Als u wilt uitvoeren op meerdere documenten, kunt u ook binden aan `ICollector<T>` of `IAsyncCollector<T>` waar `T` is een van de ondersteunde typen.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>DocumentDB-API uitvoer binding-voorbeeld
Stel dat u hebt de volgende DocumentDB API uitvoer binding in de `bindings` matrix van function.json:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

En u een wachtrij invoer binding voor een wachtrij die JSON in de volgende indeling ontvangt hebben:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

En u wilt maken van Azure DB die Cosmos-documenten in de volgende notatie voor elke record:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Zie het voorbeeld taalspecifieke die gebruikmaakt van deze binding uitvoer documenten toevoegen aan uw database.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Voorbeeld van uitvoer in C# #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Voorbeeld van uitvoer in F # #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

Dit voorbeeld vereist een `project.json` -bestand dat Hiermee geeft u de `FSharp.Interop.Dynamic` en `Dynamitey` NuGet afhankelijkheden:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Om toe te voegen een `project.json` bestand, Zie [F # pakket management](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Voorbeeld van uitvoer in JavaScript

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```
