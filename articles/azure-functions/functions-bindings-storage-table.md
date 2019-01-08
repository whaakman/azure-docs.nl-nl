---
title: Azure Table storage-bindingen voor Azure Functions
description: Over het gebruik van Azure Table storage-bindingen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: bd85214efc3c8f67d41563e3ca46a1e2278c4868
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062670"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Table storage-bindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u werkt met Azure Table storage-bindingen in Azure Functions. Azure Functions ondersteunt invoer- en uitvoerbindingen voor Azure Table storage.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Table storage-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet-pakket versie 2.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Table storage-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Invoer

Gebruik de Azure Table storage-Invoerbinding om een tabel in een Azure Storage-account te lezen.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# lezen één entiteit](#input---c-example---one-entity)
* [C#-binding aan IQueryable](#input---c-example---iqueryable)
* [C#-binding aan CloudTable](#input---c-example---cloudtable)
* [C#-script één entiteit lezen](#input---c-script-example---one-entity)
* [C#-script-binding aan IQueryable](#input---c-script-example---iqueryable)
* [C#-script-binding aan CloudTable](#input---c-script-example---cloudtable)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)
* [Java](#input---java-example)

### <a name="input---c-example---one-entity"></a>Invoer - voorbeeld met C# - één entiteit

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een rij in een enkele tabel leest. 

De sleutelwaarde van de rij '{queueTrigger}' geeft aan dat de rijsleutel afkomstig uit de wachtrij bericht-tekenreeks is.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="input---c-example---iqueryable"></a>Invoer - voorbeeld met C# - IQueryable

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die meerdere tabelrijen leest. Houd er rekening mee dat de `MyPoco` klasse is afgeleid van `TableEntity`.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="input---c-example---cloudtable"></a>Invoer - voorbeeld met C# - CloudTable

`IQueryable` wordt niet ondersteund de [v2-Functions-runtime](functions-versions.md). Een alternatief is het gebruik van een `CloudTable` methodeparameter om te lezen van de tabel met behulp van de Azure Storage SDK. Hier volgt een voorbeeld van een 2.x-functie die in een tabel van Azure Functions-logboek zoekt:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

Zie voor meer informatie over het gebruik van CloudTable [aan de slag met Azure Table storage](../cosmos-db/table-storage-how-to-use-dotnet.md).

Als u probeert te binden aan `CloudTable` en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---c-script-example---one-entity"></a>Invoer - C#-voorbeeldscript - een entiteit

Het volgende voorbeeld wordt een tabel Invoerbinding in een *function.json* bestand en [C#-script](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrijtrigger om te lezen van een rij in een enkele tabel. 

De *function.json* bestand bevat een `partitionKey` en een `rowKey`. De `rowKey` waarde '{queueTrigger}' geeft aan dat de rijsleutel afkomstig uit de wachtrij bericht-tekenreeks is.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---iqueryable"></a>Invoer - scriptvoorbeeld met C# - IQueryable

Het volgende voorbeeld wordt een tabel Invoerbinding in een *function.json* bestand en [C#-script](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie leest entiteiten voor een partitiesleutel die is opgegeven in een wachtrijbericht.

Hier volgt de *function.json* bestand:

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
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

De C#-script-code een verwijzing naar de Azure Storage SDK wordt toegevoegd zodat het entiteitstype kan worden afgeleid van `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---cloudtable"></a>Invoer - scriptvoorbeeld met C# - CloudTable

`IQueryable` wordt niet ondersteund de [v2-Functions-runtime](functions-versions.md). Een alternatief is het gebruik van een `CloudTable` methodeparameter om te lezen van de tabel met behulp van de Azure Storage SDK. Hier volgt een voorbeeld van een 2.x-functie die in een tabel van Azure Functions-logboek zoekt:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

Zie voor meer informatie over het gebruik van CloudTable [aan de slag met Azure Table storage](../cosmos-db/table-storage-how-to-use-dotnet.md).

Als u probeert te binden aan `CloudTable` en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---f-example"></a>Invoer - F# voorbeeld

Het volgende voorbeeld wordt een tabel Invoerbinding in een *function.json* bestand en [ F# script](functions-reference-fsharp.md) code die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrijtrigger om te lezen van een rij in een enkele tabel. 

De *function.json* bestand bevat een `partitionKey` en een `rowKey`. De `rowKey` waarde '{queueTrigger}' geeft aan dat de rijsleutel afkomstig uit de wachtrij bericht-tekenreeks is.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de F# code:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.LogInformation(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Invoer - JavaScript-voorbeeld

Het volgende voorbeeld wordt een tabel Invoerbinding in een *function.json* bestand en [JavaScript-code](functions-reference-node.md) die gebruikmaakt van de binding. De functie maakt gebruik van een wachtrijtrigger om te lezen van een rij in een enkele tabel. 

De *function.json* bestand bevat een `partitionKey` en een `rowKey`. De `rowKey` waarde '{queueTrigger}' geeft aan dat de rijsleutel afkomstig uit de wachtrij bericht-tekenreeks is.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De [configuratie](#input---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

### <a name="input---java-example"></a>Invoer - Java-voorbeeld

Het volgende voorbeeld ziet een door HTTP geactiveerde functie die als resultaat het totale aantal van de items in een opgegeven partitie in de tabelopslag geeft.

```java
@FunctionName("getallcount")
public int run(
   @HttpTrigger(name = "req",
                 methods = {"get"},
                 authLevel = AuthorizationLevel.ANONYMOUS) Object dummyShouldNotBeUsed,
   @TableInput(name = "items",
                tableName = "mytablename",  partitionKey = "myparkey",
                connection = "myconnvarname") MyItem[] items
) {
    return items.length;
}
```


## <a name="input---attributes"></a>Invoer - kenmerken
 
In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken om een tabel invoer binding te configureren:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  De constructor van het kenmerk wordt de tabelnaam, partitiesleutel en rijsleutel. Het kan worden gebruikt op een out-parameter of op de geretourneerde waarde van de functie, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Zie voor een compleet voorbeeld [invoer - voorbeeld met C#](#input---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Biedt een andere manier om op te geven van de storage-account te gebruiken. De constructor, wordt de naam van een app-instelling met een verbindingsreeks voor opslag. Het kenmerk kan worden toegepast op de parameter, klasseniveau of methode. Het volgende voorbeeld toont het klasseniveau en methode:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Het storage-account te gebruiken, wordt bepaald in de volgende volgorde:

* De `Table` van kenmerk `Connection` eigenschap.
* De `StorageAccount` kenmerk toegepast op de dezelfde parameter als de `Table` kenmerk.
* De `StorageAccount` kenmerk toegepast op de functie.
* De `StorageAccount` kenmerk toegepast op de klasse.
* Het standaardopslagaccount voor de functie-app (app-instelling 'AzureWebJobsStorage').

## <a name="input---java-annotations"></a>Invoer - Java-aantekeningen

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@TableInput` aantekening op parameters waarvan de waarde afkomstig van Table storage zijn kan.  Deze aantekening kan worden gebruikt met systeemeigen Java-typen, pojo's of null-waarden met behulp van optioneel<T>. 

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Table` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `table`. Deze eigenschap wordt automatisch ingesteld wanneer u de binding in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de binding in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de tabel of entiteit in functiecode aan te geven. | 
|**Tabelnaam** | **Tabelnaam** | De naam van de tabel.| 
|**partitionKey** | **partitionKey** |Optioneel. De partitiesleutel van de Tabelentiteit om te lezen. Zie de [gebruik](#input---usage) gedeelte met richtlijnen over het gebruik van deze eigenschap.| 
|**RowKey** |**RowKey** | Optioneel. De rijsleutel van de Tabelentiteit om te lezen. Zie de [gebruik](#input---usage) gedeelte met richtlijnen over het gebruik van deze eigenschap.| 
|**toets maken** |**toets maken** | Optioneel. Het maximale aantal entiteiten om te lezen in JavaScript. Zie de [gebruik](#input---usage) gedeelte met richtlijnen over het gebruik van deze eigenschap.| 
|**filter** |**Filter** | Optioneel. Een OData-filter-expressie voor de tabel invoer in JavaScript. Zie de [gebruik](#input---usage) gedeelte met richtlijnen over het gebruik van deze eigenschap.| 
|**verbinding** |**Verbinding** | De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Invoer - gebruik

De Invoerbinding van tabel storage ondersteunt de volgende scenario's:

* **Lezen van één rij in C# of C#-script**

  Stel `partitionKey` en `rowKey`. Toegang tot gegevens in de tabel met behulp van een methodeparameter `T <paramName>`. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T` is doorgaans een type dat wordt geïmplementeerd `ITableEntity` of is afgeleid van `TableEntity`. De `filter` en `take` eigenschappen worden niet gebruikt in dit scenario. 

* **Lezen van een of meer rijen in C# of C#-script**

  Toegang tot gegevens in de tabel met behulp van een methodeparameter `IQueryable<T> <paramName>`. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T` moet een type dat implementeert `ITableEntity` of is afgeleid van `TableEntity`. U kunt `IQueryable` methoden voor het doen van een filter vereist. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen worden niet gebruikt in dit scenario.  

  > [!NOTE]
  > `IQueryable` wordt niet ondersteund de [v2-Functions-runtime](functions-versions.md). Een alternatief is [gebruikt u een methodeparameter voor CloudTable paramName](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) lezen in de tabel met de Azure Storage SDK. Als u probeert te binden aan `CloudTable` en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

* **Lezen van een of meer rijen in JavaScript**

  Stel de `filter` en `take` eigenschappen. Stel `partitionKey` of `rowKey`. Toegang tot de invoer tabel entiteit (of entiteiten) met behulp van `context.bindings.<name>`. Het gedeserialiseerde objecten hebben `RowKey` en `PartitionKey` eigenschappen.

## <a name="output"></a>Uitvoer

Gebruik een Azure Table storage-Uitvoerbinding entiteiten schrijven naar een tabel in een Azure Storage-account.

> [!NOTE]
> Deze Uitvoerbinding biedt geen ondersteuning voor het bijwerken van bestaande entiteiten. Gebruik de `TableOperation.Replace` bewerking [van de Azure Storage SDK](https://docs.microsoft.com/azure/cosmos-db/table-storage-how-to-use-dotnet#replace-an-entity) om bij te werken van een bestaande entiteit.   

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een HTTP-trigger gebruikt om te schrijven van een rij in een enkele tabel. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

Het volgende voorbeeld wordt een tabel-Uitvoerbinding een *function.json* bestand en [C#-script](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie schrijft meerdere tabelentiteiten.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### <a name="output---f-example"></a>Uitvoer - F# voorbeeld

Het volgende voorbeeld wordt een tabel-Uitvoerbinding een *function.json* bestand en [ F# script](functions-reference-fsharp.md) code die gebruikmaakt van de binding. De functie schrijft meerdere tabelentiteiten.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de F# code:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: ILogger) =
    for i = 1 to 10 do
        log.LogInformation(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld wordt een tabel-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie schrijft meerdere tabelentiteiten.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## <a name="output---attributes"></a>Uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs).

De constructor van het kenmerk wordt de naam van de tabel. Het kan worden gebruikt op een `out` parameter of op de geretourneerde waarde van de functie, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - voorbeeld met C#](#output---c-example).

U kunt de `StorageAccount` kenmerk om op te geven van de storage-account op het niveau van klasse, methode of parameter. Zie voor meer informatie, [invoer - kenmerken](#input---attributes).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Table` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `table`. Deze eigenschap wordt automatisch ingesteld wanneer u de binding in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op `out`. Deze eigenschap wordt automatisch ingesteld wanneer u de binding in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die wordt gebruikt in de functiecode aan te geven dat de tabel of entiteit vertegenwoordigt. Ingesteld op `$return` om te verwijzen naar de geretourneerde waarde van de functie.| 
|**Tabelnaam** |**Tabelnaam** | De naam van de tabel.| 
|**partitionKey** |**partitionKey** | De partitiesleutel van de Tabelentiteit om te schrijven. Zie de [sectie gebruik](#output---usage) voor informatie over hoe u deze eigenschap wilt gebruiken.| 
|**RowKey** |**RowKey** | De rijsleutel van de Tabelentiteit om te schrijven. Zie de [sectie gebruik](#output---usage) voor informatie over hoe u deze eigenschap wilt gebruiken.| 
|**verbinding** |**Verbinding** | De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uitvoer - gebruik

De Table storage uitvoer binding ondersteunt de volgende scenario's:

* **Schrijven van een rij in een willekeurige taal**

  In C# en C#-script, toegang krijgen tot de entiteit van de tabel uitvoer met behulp van een methodeparameter zoals `out T paramName` of de functie waarde retourneren. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T` elk serialiseerbaar type zijn als de partitiesleutel en rijsleutel worden geleverd door de *function.json* bestand of de `Table` kenmerk. Anders `T` moet een type met `PartitionKey` en `RowKey` eigenschappen. In dit scenario `T` gewoonlijk `ITableEntity` of is afgeleid van `TableEntity`, maar dit hoeft te.

* **Schrijven van een of meer rijen in C# of C# script**

  In C# en C#-script, toegang krijgen tot de entiteit van de tabel uitvoer met behulp van een methodeparameter `ICollector<T> paramName` of `IAsyncCollector<T> paramName`. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T` Hiermee geeft u het schema van de entiteiten die u wilt toevoegen. Normaal gesproken `T` is afgeleid van `TableEntity` of implementeert `ITableEntity`, maar dit hoeft te. De partitiesleutel en de rij waarden in sleutel *function.json* of de `Table` kenmerkconstructie niet in dit scenario worden gebruikt.

  Een alternatief is het gebruik van een `CloudTable` methodeparameter om te schrijven naar de tabel met behulp van de Azure Storage SDK. Als u probeert te binden aan `CloudTable` en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x). Voor een voorbeeld van code die wordt gebonden aan `CloudTable`, Zie de voorbeelden Invoerbinding voor [C#](#input---c-example---cloudtable) of [C#-script](#input---c-script-example---cloudtable) eerder in dit artikel.

* **Een of meer rijen in JavaScript schrijven**

  Toegang tot de tabel met behulp van de uitvoer in JavaScript-functies, `context.bindings.<name>`.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| Tabel | [Foutcodes voor tabel](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| BLOB-, tabel, wachtrij | [Foutcodes voor opslag](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB-, tabel, wachtrij | [Problemen oplossen](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
