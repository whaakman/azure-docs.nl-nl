---
title: Azure DB Cosmos-bindingen voor functies
description: Het gebruik van Azure DB die Cosmos-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: ab55281b6adcc8867f207e6887c88a26c1a8616b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-cosmos-db-bindings-for-azure-functions"></a>Azure DB Cosmos-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) bindingen in de Azure Functions. Azure Functions ondersteunt activeren, invoer en uitvoer van de bindingen voor Azure Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

De Azure Cosmos DB Trigger gebruikt de [Azure Cosmos DB wijzigen Feed](../cosmos-db/change-feed.md) om te luisteren naar wijzigingen meerdere partities. De feed wijziging publiceert inserts en updates, niet verwijderen. 

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt geactiveerd vanuit een specifieke database en verzameling.

```cs
    using System.Collections.Generic;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;

    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
            log.Info("Documents modified " + documents.Count);
            log.Info("First document Id " + documents[0].Id);
    }
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Cosmos-DB-trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie schrijft logboekberichten wanneer Cosmos DB records zijn gewijzigd.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

Dit is de C#-scriptcode:
 
```cs 
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
      log.Verbose("Documents modified " + documents.Count);
      log.Verbose("First document Id " + documents[0].Id);
    }
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Cosmos-DB-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie schrijft logboekberichten wanneer Cosmos DB records zijn gewijzigd.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

Hier volgt de JavaScript-code:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/Trigger/CosmosDBTriggerAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Het kenmerk constructor werkt met de databasenaam en verzamelingsnaam. Zie voor informatie over deze instellingen en andere eigenschappen die u kunt configureren, [Trigger - configuratie](#trigger---configuration). Hier volgt een `CosmosDBTrigger` kenmerk voorbeeld in een handtekening voor methode:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Zie voor een compleet voorbeeld [Trigger - C#-voorbeeld](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `CosmosDBTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** || moet worden ingesteld op `cosmosDBTrigger`. |
|**direction** || moet worden ingesteld op `in`. Deze parameter wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** || De naam van de variabele gebruikt in functiecode die de lijst van documenten met wijzigingen vertegenwoordigt. | 
|**connectionStringSetting**|**ConnectionStringSetting** | De naam van een app-instelling met de verbindingsreeks waarmee verbinding met de Azure DB die Cosmos-account wordt bewaakt. |
|**databaseName**|**DatabaseName**  | De naam van de Azure DB die Cosmos-database met de verzameling wordt bewaakt. |
|**collectionName** |**CollectionName** | De naam van de verzameling wordt bewaakt. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Optioneel) De naam van een app-instelling met de verbindingsreeks aan de service die de lease-verzameling bevat. Wanneer niet is ingesteld, de `connectionStringSetting` waarde wordt gebruikt. Deze parameter wordt automatisch ingesteld wanneer de binding in de portal is gemaakt. De verbindingsreeks voor de verzameling leases moet schrijfmachtigingen hebben.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Optioneel) De naam van de database waarin de verzameling gebruikt voor het opslaan van leases. Wanneer niet is ingesteld, de waarde van de `databaseName` instelling wordt gebruikt. Deze parameter wordt automatisch ingesteld wanneer de binding in de portal is gemaakt. |
|**leaseCollectionName** | **LeaseCollectionName** | (Optioneel) De naam van de verzameling gebruikt voor het opslaan van leases. Wanneer niet is ingesteld, de waarde `leases` wordt gebruikt. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Optioneel) Als de waarde `true`, de verzameling leases wordt automatisch gemaakt wanneer deze niet al bestaat. De standaardwaarde is `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (Optioneel) Hiermee definieert u de hoeveelheid Aanvraageenheden toewijzen bij het maken van de verzameling van leases. Deze instelling wordt alleen gebruikt bij `createLeaseCollectionIfNotExists` is ingesteld op `true`. Deze parameter wordt automatisch ingesteld als de binding is gemaakt met behulp van de portal.
| |**LeaseOptions** | Configureer opties voor lease met het instellen van eigenschappen in een exemplaar van de [ChangeFeedHostOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.changefeedprocessor.changefeedhostoptions) klasse.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - usage

De trigger vereist een tweede collectie dat wordt gebruikt voor het opslaan van _leases_ via de partities. Zowel de verzameling wordt bewaakt als de verzameling met de leases moet beschikbaar zijn voor de trigger werkt.

 >[!IMPORTANT]
 > Als meerdere functies zijn geconfigureerd voor het gebruik van een Cosmos-DB-trigger voor dezelfde verzameling, moet elk van de functies voor een verzameling speciale lease gebruiken. Anders wordt slechts één van de functies worden geactiveerd. 

De trigger wordt niet aangegeven dat of een document is bijgewerkt of ingevoegd, biedt deze alleen het document zelf. Als u nodig hebt voor het afhandelen van updates en toevoegingen anders, kunt u dat doen door het implementeren van tijdstempelvelden voor het invoegen of bijwerken.

## <a name="input"></a>Invoer

De binding van Azure DB die Cosmos-invoer haalt een of meer Azure DB die Cosmos-documenten en geeft deze door aan de invoerparameter van de functie. Het document-ID of query-parameters kunnen worden bepaald op basis van de trigger die de functie activeert. 

>[!NOTE]
> Geen gebruikmaken van Azure DB die Cosmos-invoer of uitvoer bindingen als u MongoDB-API op een Cosmos-DB-account. Beschadiging van gegevens is mogelijk.

## <a name="input---example-1"></a>Invoer - voorbeeld 1

Zie het voorbeeld taalspecifieke die één document leest:

* [C#](#input---c-example)
* [C# script (.csx)](#input---c-script-example)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example"></a>Invoer - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document uit een specifieke database en verzameling ophaalt. 

Eerst `Id` en `Maker` waarden voor een `CarReview` exemplaar worden doorgegeven aan een wachtrij. De binding gebruikt Cosmos-DB `Id` en `Maker` van het bericht uit de wachtrij voor het ophalen van het document uit de database.

```cs
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;

    namespace CosmosDB
    {
        public static class SingleEntry
        {
            [FunctionName("SingleEntry")]
            public static void Run(
                [QueueTrigger("car-reviews", Connection = "StorageConnectionString")] CarReview carReview,
                [DocumentDB("cars", "car-reviews", PartitionKey = "{maker}", Id= "{id}", ConnectionStringSetting = "CarReviewsConnectionString")] CarReview document,
                TraceWriter log)
            {
                log.Info( $"Selected Review - {document?.Review}"); 
            }
        }
    }
```

Hier volgt de `CarReview` POCO:

 ```cs
    public class CarReview
    {
        public string Id { get; set; }
        public string Maker { get; set; }
        public string Description { get; set; }
        public string Model { get; set; }
        public string Image { get; set; }
        public string Review { get; set; }
    }
 ```

### <a name="input---c-script-example"></a>Invoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Cosmos-DB invoer binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie één document leest en updates van de tekstwaarde van het document.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connection": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding 
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

<a name="infsharp"></a>

### <a name="input---f-example"></a>Invoer - F #-voorbeeld

Het volgende voorbeeld ziet u een Cosmos-DB invoer binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie één document leest en updates van de tekstwaarde van het document.

Dit zijn de bindingsgegevens de *function.json* bestand:

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de F #-code:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

In dit voorbeeld heeft een `project.json` -bestand dat Hiermee geeft u de `FSharp.Interop.Dynamic` en `Dynamitey` NuGet afhankelijkheden:

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

### <a name="input---javascript-example"></a>Invoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Cosmos-DB invoer binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie één document leest en updates van de tekstwaarde van het document.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

## <a name="input---example-2"></a>Invoer - voorbeeld 2

Zie het voorbeeld taalspecifieke die meerdere documenten leest:

* [C#](#input---c-example-2)
* [C# script (.csx)](#input---c-script-example-2)
* [JavaScript](#input---javascript-example-2)

### <a name="input---c-example-2"></a>Invoer - C#-voorbeeld 2

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een SQL-query wordt uitgevoerd. Gebruik de `SqlQuery` parameter, moet u de nieuwste versie van de bètaversie van installeren `Microsoft.Azure.WebJobs.Extensions.DocumentDB` NuGet-pakket.

```csharp
    using System.Net;
    using System.Net.Http;
    using System.Collections.Generic;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Extensions.Http;

    [FunctionName("CosmosDBSample")]
    public static HttpResponseMessage Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get")] HttpRequestMessage req,
        [DocumentDB("test", "test", ConnectionStringSetting = "CosmosDB", SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")] IEnumerable<object> documents)
    {
        return req.CreateResponse(HttpStatusCode.OK, documents);
    }
```

### <a name="input---c-script-example-2"></a>Invoer - C# scriptvoorbeeld 2

Het volgende voorbeeld ziet u een Azure Cosmos DB invoer binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie haalt meerdere documenten die zijn opgegeven door een SQL-query met behulp van een wachtrij-trigger voor het aanpassen van de queryparameters.

De trigger wachtrij bevat een parameter `departmentId`. Een wachtrijbericht van `{ "departmentId" : "Finance" }` alle records voor de afdeling Financiën zou retourneren. 

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

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

### <a name="input---javascript-example-2"></a>Invoer - JavaScript-voorbeeld 2

Het volgende voorbeeld ziet u een Azure Cosmos DB invoer binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie haalt meerdere documenten die zijn opgegeven door een SQL-query met behulp van een wachtrij-trigger voor het aanpassen van de queryparameters.

De trigger wachtrij bevat een parameter `departmentId`. Een wachtrijbericht van `{ "departmentId" : "Finance" }` alle records voor de afdeling Financiën zou retourneren. 

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

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

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Het kenmerk constructor werkt met de databasenaam en verzamelingsnaam. Zie voor informatie over deze instellingen en andere eigenschappen die u kunt configureren, [de volgende configuratiesectie](#input---configuration). 

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `DocumentDB` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type**     || moet worden ingesteld op `documentdb`.        |
|**direction**     || moet worden ingesteld op `in`.         |
|**naam**     || Naam van de binding-parameter die het document in de functie vertegenwoordigt.  |
|**databaseName** |**DatabaseName** |De database met het document.        |
|**collectionName** |**CollectionName** | De naam van de verzameling waarin het document. |
|**id**    | **Id** | De ID van het document om op te halen. Deze eigenschap ondersteunt bindingen parameters. Zie voor meer informatie, [binden aan aangepaste eigenschappen voor de invoer in een expressie voor gegevensbinding](functions-triggers-bindings.md#bind-to-custom-input-properties). Stelt beide niet de **id** en **sqlQuery** eigenschappen. Als u een niet instelt, wordt de volledige verzameling worden opgehaald. |
|**sqlQuery**  |**SqlQuery**  | Een Azure Cosmos DB SQL-query die wordt gebruikt voor het ophalen van meerdere documenten. De eigenschap biedt ondersteuning voor bindingen van de runtime, zoals in dit voorbeeld: `SELECT * FROM c where c.departmentId = {departmentId}`. Stelt beide niet de **id** en **sqlQuery** eigenschappen. Als u een niet instelt, wordt de volledige verzameling worden opgehaald.|
|**verbinding**     |**ConnectionStringSetting**|De naam van de app-instelling met de verbindingsreeks voor Azure Cosmos DB.        |
|**partitionKey**|**PartitionKey**|Hiermee geeft u de waarde voor de partitiesleutel voor de zoekactie. Kan bindingparameters bevatten.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Invoer - gebruik

In C# en F # functies, wanneer de functie wordt voltooid, wordt afgesloten worden wijzigingen in het invoerdocument via benoemde invoerparameters automatisch doorgevoerd. 

In de JavaScript-functies worden niet automatisch updates gesteld bij functie beëindigen. Gebruik in plaats daarvan `context.bindings.<documentName>In` en `context.bindings.<documentName>Out` om updates te maken. Zie de [JavaScript voorbeeld](#input---javascript-example).

## <a name="output"></a>Uitvoer

De uitvoer van de Azure DB die Cosmos binding kunt schrijven u een nieuw document naar een Azure DB die Cosmos-database. 

>[!NOTE]
> Geen gebruikmaken van Azure DB die Cosmos-invoer of uitvoer bindingen als u MongoDB-API op een Cosmos-DB-account. Beschadiging van gegevens is mogelijk.

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een document wordt toegevoegd aan een database, met behulp van gegevens in het bericht van Queue storage.

```cs
    using System;
    using Microsoft.Azure.WebJobs;

    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        document = new { Text = myQueueItem, id = Guid.NewGuid() };
    }
```

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

Het volgende voorbeeld ziet u de uitvoer van een Azure Cosmos DB binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrij invoer binding voor een wachtrij die JSON in de volgende indeling ontvangt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

De functie maakt Azure Cosmos DB documenten in de volgende notatie voor elke record:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Dit zijn de bindingsgegevens de *function.json* bestand:

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
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

Als u wilt meerdere documenten maken, kunt u binden aan `ICollector<T>` of `IAsyncCollector<T>` waar `T` is een van de ondersteunde typen.

### <a name="output---f-example"></a>Output - F #-voorbeeld

Het volgende voorbeeld ziet u de uitvoer van een Azure Cosmos DB binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrij invoer binding voor een wachtrij die JSON in de volgende indeling ontvangt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

De functie maakt Azure Cosmos DB documenten in de volgende notatie voor elke record:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Dit zijn de bindingsgegevens de *function.json* bestand:

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
De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de F #-code:

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

In dit voorbeeld heeft een `project.json` -bestand dat Hiermee geeft u de `FSharp.Interop.Dynamic` en `Dynamitey` NuGet afhankelijkheden:

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

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

Het volgende voorbeeld ziet u de uitvoer van een Azure Cosmos DB binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrij invoer binding voor een wachtrij die JSON in de volgende indeling ontvangt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

De functie maakt Azure Cosmos DB documenten in de volgende notatie voor elke record:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Dit zijn de bindingsgegevens de *function.json* bestand:

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

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

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Het kenmerk constructor werkt met de databasenaam en verzamelingsnaam. Zie voor informatie over deze instellingen en andere eigenschappen die u kunt configureren, [Output - configuratie](#output---configuration). Hier volgt een `DocumentDB` kenmerk voorbeeld in een handtekening voor methode:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Zie voor een compleet voorbeeld [uitvoer - C#-voorbeeld](#output---c-example).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `DocumentDB` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type**     || moet worden ingesteld op `documentdb`.        |
|**direction**     || moet worden ingesteld op `out`.         |
|**naam**     || Naam van de binding-parameter die het document in de functie vertegenwoordigt.  |
|**databaseName** | **DatabaseName**|De database met de verzameling waarin het document is gemaakt.     |
|**collectionName** |**CollectionName**  | De naam van de verzameling waarin het document is gemaakt. |
|**createIfNotExists**  |**CreateIfNotExists**    | Een Booleaanse waarde die aangeeft of de verzameling is gemaakt als deze nog niet bestaat. De standaardwaarde is *false* omdat nieuwe verzamelingen worden gemaakt met gereserveerde doorvoer, wat gevolgen heeft kosten. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/documentdb/) voor meer informatie.  |
|**partitionKey**|**PartitionKey** |Wanneer `CreateIfNotExists` is ingesteld op true, wordt het pad van de partitie voor de gemaakte verzameling gedefinieerd.|
|**collectionThroughput**|**CollectionThroughput**| Wanneer `CreateIfNotExists` is ingesteld op true, definieert de [doorvoer](../cosmos-db/set-throughput.md) van de verzameling gemaakt.|
|**verbinding**    |**ConnectionStringSetting** |De naam van de app-instelling met de verbindingsreeks voor Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik

Standaard, wanneer u naar de output-parameter in de functie schrijft is een document gemaakt in uw database. Dit document heeft een automatisch gegenereerde GUID als het document-ID. U kunt de document-ID van het uitvoerdocument opgeven door te geven de `id` eigenschap in het JSON-object doorgegeven aan de output-parameter. 

> [!Note]  
> Wanneer u de ID van een bestaand document opgeeft, wordt deze door het nieuwe uitvoerdocument overschreven. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een Quick Start die gebruikmaakt van een trigger Cosmos-DB](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Meer informatie over computergebruik met Cosmos DB zonder Server-database](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
