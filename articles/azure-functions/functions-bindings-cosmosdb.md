---
title: Azure DB Cosmos-bindingen voor functies 1.x
description: Het gebruik van Azure DB die Cosmos-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: fb9e6eaca31849eb8ef15714978c1ec55b23e02f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796924"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-1x"></a>Azure DB Cosmos-bindingen voor Azure Functions 1.x

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versie 1 - Algemene beschikbaarheid](functions-bindings-cosmosdb.md)
> * [Versie 2 - Preview](functions-bindings-cosmosdb-v2.md)

Dit artikel wordt uitgelegd hoe u werkt met [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) bindingen in de Azure Functions. Azure Functions ondersteunt activeren, invoer en uitvoer van de bindingen voor Azure Cosmos DB.

> [!NOTE]
> In dit artikel is bedoeld voor Azure Functions 1.x.  Voor informatie over het gebruik van deze bindingen in functies 2.x, Zie [Azure Cosmos DB bindingen voor Azure Functions 2.x](functions-bindings-cosmosdb-v2.md).
>
>Deze binding heette oorspronkelijk DocumentDB. In functies versie 1.x, alleen de trigger is gewijzigd van de Cosmos-DB. de binding invoer, uitvoer-binding en NuGet-pakket behoudt de naam van de DocumentDB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Azure DB die Cosmos-bindingen voor functies versie 1.x vindt u in de [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB) NuGet-pakket versie 1.x. Broncode voor de bindingen is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.DocumentDB) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="trigger"></a>Trigger

De Azure Cosmos DB Trigger gebruikt de [Azure Cosmos DB wijzigen Feed](../cosmos-db/change-feed.md) om te luisteren naar invoegingen en updates meerdere partities. De feed wijziging publiceert inserts en updates, niet verwijderen.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Voorbeelden van de trigger overslaan](#trigger---attributes)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt geactiveerd wanneer er zijn ingevoegd of bijgewerkt in de opgegeven database en verzameling.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV1
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[Voorbeelden van de trigger overslaan](#trigger---attributes)

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

[Voorbeelden van de trigger overslaan](#trigger---attributes)

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

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) kenmerk.

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
|**Naam** || De naam van de variabele gebruikt in functiecode die de lijst van documenten met wijzigingen vertegenwoordigt. | 
|**ConnectionStringSetting**|**ConnectionStringSetting** | De naam van een app-instelling met de verbindingsreeks waarmee verbinding met de Azure DB die Cosmos-account wordt bewaakt. |
|**databaseName**|**DatabaseName**  | De naam van de Azure DB die Cosmos-database met de verzameling wordt bewaakt. |
|**CollectionName** |**CollectionName** | De naam van de verzameling wordt bewaakt. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Optioneel) De naam van een app-instelling met de verbindingsreeks aan de service die de lease-verzameling bevat. Wanneer niet is ingesteld, de `connectionStringSetting` waarde wordt gebruikt. Deze parameter wordt automatisch ingesteld wanneer de binding in de portal is gemaakt. De verbindingsreeks voor de verzameling leases moet schrijfmachtigingen hebben.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Optioneel) De naam van de database waarin de verzameling gebruikt voor het opslaan van leases. Wanneer niet is ingesteld, de waarde van de `databaseName` instelling wordt gebruikt. Deze parameter wordt automatisch ingesteld wanneer de binding in de portal is gemaakt. |
|**leaseCollectionName** | **LeaseCollectionName** | (Optioneel) De naam van de verzameling gebruikt voor het opslaan van leases. Wanneer niet is ingesteld, de waarde `leases` wordt gebruikt. |
|**CreateLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Optioneel) Als de waarde `true`, de verzameling leases wordt automatisch gemaakt wanneer deze niet al bestaat. De standaardwaarde is `false`. |
|**LeasesCollectionThroughput**| **LeasesCollectionThroughput**| (Optioneel) Hiermee definieert u de hoeveelheid Aanvraageenheden toewijzen bij het maken van de verzameling van leases. Deze instelling wordt alleen gebruikt bij `createLeaseCollectionIfNotExists` is ingesteld op `true`. Deze parameter wordt automatisch ingesteld als de binding is gemaakt met behulp van de portal.
|**LeaseCollectionPrefix**| **LeaseCollectionPrefix**| (Optioneel) Als de waarde, wordt toegevoegd een voorvoegsel voor de leases gemaakt in de verzameling Lease voor deze functie, effectief zodat twee afzonderlijke Azure Functions dezelfde Lease verzameling delen met andere voorvoegsels.
|**FeedPollDelay**| **FeedPollDelay**| (Optioneel) Wanneer is ingesteld, wordt gedefinieerd, in milliseconden, de vertraging tussen een partitie voor de nieuwe wijzigingen op de feed polling worden nadat alle huidige wijzigingen geleegd. De standaardwaarde is 5000 (5 seconden).
|**LeaseAcquireInterval**| **LeaseAcquireInterval**| (Optioneel) Als de waarde, wordt gedefinieerd, in milliseconden, het interval voor ere van een taak worden berekend als partities gelijkmatig zijn verdeeld over bekende host exemplaren. De standaardwaarde is 13000 (13 seconden).
|**LeaseExpirationInterval**| **LeaseExpirationInterval**| (Optioneel) Als de waarde, wordt gedefinieerd, in milliseconden, het interval waarvoor de lease op een lease voor een partitie wordt gehouden. Als de lease is niet binnen dit interval vernieuwd, wordt deze verlopen en eigendom van de partitie wordt verplaatst naar een ander exemplaar. De standaardwaarde is 60.000 (60 seconden).
|**LeaseRenewInterval**| **LeaseRenewInterval**| (Optioneel) Als de waarde, wordt gedefinieerd, in milliseconden die het vernieuwingsinterval voor alle leases voor partities die momenteel worden vastgehouden door een exemplaar. De standaardwaarde is 17000 (17 seconden).
|**CheckpointFrequency**| **CheckpointFrequency**| (Optioneel) Als de waarde, wordt gedefinieerd, in milliseconden, het interval tussen lease controlepunten. Standaard wordt altijd na een geslaagde functieaanroep.
|**MaxItemsPerInvocation**| **MaxItemsPerInvocation**| (Optioneel) Als de waarde, wordt de maximale hoeveelheid items ontvangen per aanroep van de functie wordt aangepast.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

De trigger vereist een tweede collectie dat wordt gebruikt voor het opslaan van _leases_ via de partities. Zowel de verzameling wordt bewaakt als de verzameling met de leases moet beschikbaar zijn voor de trigger werkt.

>[!IMPORTANT]
> Als meerdere functies zijn geconfigureerd voor het gebruik van een Cosmos-DB-trigger voor dezelfde verzameling, moet elk van de functies gebruiken van een verzameling speciale lease of geef een andere `LeaseCollectionPrefix` voor elke functie. Anders wordt slechts één van de functies worden geactiveerd. Zie voor meer informatie over het voorvoegsel op dat de [configuratiesectie](#trigger---configuration).

De trigger wordt niet aangegeven dat of een document is bijgewerkt of ingevoegd, biedt deze alleen het document zelf. Als u nodig hebt voor het afhandelen van updates en toevoegingen anders, kunt u dat doen door het implementeren van tijdstempelvelden voor het invoegen of bijwerken.

## <a name="input"></a>Invoer

De binding van Azure DB die Cosmos-invoer haalt een of meer Azure DB die Cosmos-documenten en geeft deze door aan de invoerparameter van de functie. Het document-ID of query-parameters kunnen worden bepaald op basis van de trigger die de functie activeert. 

>[!NOTE]
> Geen gebruikmaken van Azure DB die Cosmos-invoer of uitvoer bindingen als u MongoDB-API op een Cosmos-DB-account. Beschadiging van gegevens is mogelijk.

## <a name="input---examples"></a>Invoer - voorbeelden

Zie de taalspecifieke voorbeelden die één document lezen door te geven van een id-waarde:

* [C#](#input---c-examples)
* [C# script (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---c-examples"></a>Invoer - C#-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* [Wachtrij worden geactiveerd, zoekt u naar de ID van JSON](#queue-trigger-look-up-id-from-json-c)
* [HTTP-trigger ID opzoeken van queryreeks](#http-trigger-look-up-id-from-query-string-c)
* [HTTP-trigger ID opzoeken van gegevens routeren](#http-trigger-look-up-id-from-route-data-c)
* [HTTP-trigger, ID van de routegegevens, met SqlQuery opzoeken](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP activeren, kunt u meerdere documenten, met behulp van SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP activeren, kunt u meerdere documenten, DocumentClient gebruiken](#http-trigger-get-multiple-docs-using-documentclient-c)

De voorbeelden verwijzen naar een eenvoudige `ToDoItem` type:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Wachtrij worden geactiveerd, zoekt u naar de ID van JSON (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document opgehaald. De functie wordt geactiveerd door een wachtrijbericht met een JSON-object. De wachtrij-trigger parseert de JSON naar een object met de naam `ToDoItemLookup`, die de ID opzoeken bevat. Dat-ID wordt gebruikt voor het ophalen van een `ToDoItem` document van de opgegeven database en verzameling.

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP-trigger, zoekt u naar ID van de queryreeks (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag die gebruikmaakt van een queryreeks om op te geven van de ID te zoeken. Dat-ID wordt gebruikt voor het ophalen van een `ToDoItem` document van de opgegeven database en verzameling.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP-trigger ID opzoeken van routegegevens (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag dat maakt gebruik van gegevens om op te geven van de ID opzoeken routeren. Dat-ID wordt gebruikt voor het ophalen van een `ToDoItem` document van de opgegeven database en verzameling.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static HttpResponseMessage Run(
            [HttpTrigger(
                AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP-trigger, zoekt u naar ID van de routegegevens, met SqlQuery (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die één document opgehaald. De functie wordt geactiveerd door een HTTP-aanvraag dat maakt gebruik van gegevens om op te geven van de ID opzoeken routeren. Dat-ID wordt gebruikt voor het ophalen van een `ToDoItem` document van de opgegeven database en verzameling.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems2/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "select * from ToDoItems r where r.id = {id}")] IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP activeren, kunt u meerdere documenten, met behulp van SqlQuery (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een lijst met documenten ophaalt. De functie wordt geactiveerd door een HTTP-aanvraag. De query is opgegeven in de `SqlQuery` kenmerkeigenschap.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP activeren, kunt u meerdere documenten, met behulp van de DocumentClient (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een lijst met documenten ophaalt. De functie wordt geactiveerd door een HTTP-aanvraag. De code wordt een `DocumentClient` exemplaar geleverd door de Azure DB die Cosmos-binding met een lijst van documenten lezen. De `DocumentClient` exemplaar kan ook worden gebruikt voor schrijfbewerkingen.

```cs
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
            string searchterm = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
                .Value;

            if (searchterm == null)
            {
                return req.CreateResponse(HttpStatusCode.NotFound);
            }

            log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---c-script-examples"></a>Invoer - voorbeelden van C#-scripts

Deze sectie bevat de volgende voorbeelden die één document gelezen door te geven van een id-waarde uit diverse bronnen:

* Wachtrij worden geactiveerd, ID van het wachtrijbericht opzoeken
* Wachtrij worden geactiveerd, ID van het wachtrijbericht, met behulp van SqlQuery opzoeken

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-c-script"></a>Wachtrij worden geactiveerd, zoekt u naar ID uit wachtrijbericht (C# script)

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

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-using-sqlquery-c-script"></a>Wachtrij worden geactiveerd, zoekt u naar ID van het wachtrijbericht, met behulp van SqlQuery (C# script)

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

[Invoer voorbeelden overslaan](#input---attributes)

### <a name="input---javascript-examples"></a>Invoer - JavaScript-voorbeelden

Deze sectie bevat de volgende voorbeelden die één document gelezen door te geven van een id-waarde uit diverse bronnen:

* Wachtrij worden geactiveerd, ID van het wachtrijbericht opzoeken
* Wachtrij worden geactiveerd, ID van het wachtrijbericht, met behulp van Sqlquery opzoeken

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-javascript"></a>Wachtrij worden geactiveerd, zoekt u naar ID uit wachtrijbericht (JavaScript)

Het volgende voorbeeld ziet u een Cosmos-DB invoer binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie één document leest en updates van de tekstwaarde van het document.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "name": "inputDocumentIn",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
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

[Invoer voorbeelden overslaan](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-using-sqlquery-javascript"></a>Wachtrij worden geactiveerd, zoekt u naar ID van het wachtrijbericht, met behulp van SqlQuery (JavaScript)

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

[Invoer voorbeelden overslaan](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Invoer - F # voorbeelden

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

## <a name="input---attributes"></a>Invoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) kenmerk.

Het kenmerk constructor werkt met de databasenaam en verzamelingsnaam. Zie voor informatie over deze instellingen en andere eigenschappen die u kunt configureren, [de volgende configuratiesectie](#input---configuration). 

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `DocumentDB` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type**     || moet worden ingesteld op `documentdb`.        |
|**direction**     || moet worden ingesteld op `in`.         |
|**Naam**     || Naam van de binding-parameter die het document in de functie vertegenwoordigt.  |
|**databaseName** |**DatabaseName** |De database met het document.        |
|**CollectionName** |**CollectionName** | De naam van de verzameling waarin het document. |
|**id**    | **Id** | De ID van het document om op te halen. Deze eigenschap ondersteunt [bindingsexpressies](functions-triggers-bindings.md#binding-expressions-and-patterns). Stelt beide niet de **id** en **sqlQuery** eigenschappen. Als u een niet instelt, wordt de volledige verzameling worden opgehaald. |
|**sqlQuery**  |**SqlQuery**  | Een Azure Cosmos DB SQL-query die wordt gebruikt voor het ophalen van meerdere documenten. De eigenschap biedt ondersteuning voor bindingen van de runtime, zoals in dit voorbeeld: `SELECT * FROM c where c.departmentId = {departmentId}`. Stelt beide niet de **id** en **sqlQuery** eigenschappen. Als u een niet instelt, wordt de volledige verzameling worden opgehaald.|
|**Verbinding**     |**ConnectionStringSetting**|De naam van de app-instelling met de verbindingsreeks voor Azure Cosmos DB.        |
|**PartitionKey**|**PartitionKey**|Hiermee geeft u de waarde voor de partitiesleutel voor de zoekactie. Kan bindingparameters bevatten.|

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

* [C#](#output---c-examples)
* [C# script (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

Zie ook de [invoer voorbeeld](#input---c-examples) die gebruikmaakt van `DocumentClient`.

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="ouput---c-examples"></a>Uitvoer geproduceerd - C#-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* Wachtrij worden geactiveerd, één doc schrijven
* Wachtrij worden geactiveerd, schrijven documenten met behulp van IAsyncCollector

De voorbeelden verwijzen naar een eenvoudige `ToDoItem` type:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Wachtrij worden geactiveerd, één doc schrijven (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een document wordt toegevoegd aan een database, met behulp van gegevens in het bericht van Queue storage.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV1
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Wachtrij worden geactiveerd, schrijven documenten met behulp van IAsyncCollector (C#)

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) waarmee een verzameling van documenten worden toegevoegd aan een database, met behulp van gegevens die zijn opgegeven in een wachtrijbericht JSON.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---c-script-examples"></a>Output - voorbeelden van C#-scripts

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

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---javascript-examples"></a>Output - JavaScript-voorbeelden

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

[Voorbeelden van uitvoer overslaan](#output---attributes)

### <a name="output---f-examples"></a>Output - F # voorbeelden

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

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) kenmerk.

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
|**Naam**     || Naam van de binding-parameter die het document in de functie vertegenwoordigt.  |
|**databaseName** | **DatabaseName**|De database met de verzameling waarin het document is gemaakt.     |
|**CollectionName** |**CollectionName**  | De naam van de verzameling waarin het document is gemaakt. |
|**CreateIfNotExists**  |**CreateIfNotExists**    | Een Booleaanse waarde die aangeeft of de verzameling is gemaakt als deze nog niet bestaat. De standaardwaarde is *false* omdat nieuwe verzamelingen worden gemaakt met gereserveerde doorvoer, wat gevolgen heeft kosten. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/documentdb/) voor meer informatie.  |
|**PartitionKey**|**PartitionKey** |Wanneer `CreateIfNotExists` is ingesteld op true, wordt het pad van de partitie voor de gemaakte verzameling gedefinieerd.|
|**CollectionThroughput**|**CollectionThroughput**| Wanneer `CreateIfNotExists` is ingesteld op true, definieert de [doorvoer](../cosmos-db/set-throughput.md) van de verzameling gemaakt.|
|**Verbinding**    |**ConnectionStringSetting** |De naam van de app-instelling met de verbindingsreeks voor Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik

Standaard, wanneer u naar de output-parameter in de functie schrijft is een document gemaakt in uw database. Dit document heeft een automatisch gegenereerde GUID als het document-ID. U kunt de document-ID van het uitvoerdocument opgeven door te geven de `id` eigenschap in het JSON-object doorgegeven aan de output-parameter. 

> [!Note]  
> Wanneer u de ID van een bestaand document opgeeft, wordt deze door het nieuwe uitvoerdocument overschreven. 

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| CosmosDB | [Foutcodes CosmosDB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een Quick Start die gebruikmaakt van een trigger Cosmos-DB](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Meer informatie over computergebruik met Cosmos DB zonder Server-database](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
