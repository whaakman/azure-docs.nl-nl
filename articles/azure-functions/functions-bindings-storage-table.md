---
title: Azure Table storage-bindingen voor Azure Functions
description: Begrijpen hoe Azure Table storage bindingen in de Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
ms.openlocfilehash: 5cfb968b201f49d5b7029a0b677e3ce2a8aa6cb9
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Table storage-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met Azure Table storage bindingen in de Azure Functions. Azure Functions ondersteunt invoer en uitvoer van de bindingen voor Azure Table storage.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="input"></a>Invoer

De invoer binding voor Azure Table storage gebruiken om te lezen van een tabel in een Azure Storage-account.

## <a name="input---example"></a>Invoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C# lezen één entiteit](#input---c-example-1)
* [Lezen van meerdere entiteiten van C#](#input---c-example-2)
* [C# script - één entiteit lezen](#input---c-script-example-1)
* [C# script - meerdere entiteiten lezen](#input---c-script-example-2)
* [F#](#input---f-example-2)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example-1"></a>Invoer - C#-voorbeeld 1

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een enkele tabelrij leest. 

De waarde '{queueTrigger}' van de rij geeft aan dat de rijsleutel is afkomstig uit de wachtrij bericht-tekenreeks.

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
        TraceWriter log)
    {
        log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}";
    }
}
```

### <a name="input---c-example-2"></a>Invoer - C#-voorbeeld 2

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die meerdere rijen leest. Houd er rekening mee dat de `MyPoco` klasse is afgeleid van `TableEntity`.

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
        TraceWriter log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}";
        }
    }
}
```

### <a name="input---c-script-example-1"></a>Invoer - C# scriptvoorbeeld 1

Het volgende voorbeeld ziet u een tabel invoer binding in een *function.json* bestand en [C# script](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie maakt gebruik van een trigger wachtrij lezen van een enkele tabelrij. 

De *function.json* bestand geeft een `partitionKey` en een `rowKey`. De `rowKey` '{queueTrigger}' van de waarde geeft aan dat de rijsleutel is afkomstig uit de wachtrij bericht-tekenreeks.

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example-2"></a>Invoer - C# scriptvoorbeeld 2

Het volgende voorbeeld ziet u een tabel invoer binding in een *function.json* bestand en [C# script](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie leest entiteiten voor een partitiesleutel die is opgegeven in een wachtrijbericht.

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

De C#-scriptcode wordt een verwijzing naar de Azure-opslag-SDK toegevoegd zodat het entiteitstype kan worden afgeleid van `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---f-example"></a>Invoer - F #-voorbeeld

Het volgende voorbeeld ziet u een tabel invoer binding in een *function.json* bestand en [F # script](functions-reference-fsharp.md) code die gebruikmaakt van de binding. De functie maakt gebruik van een trigger wachtrij lezen van een enkele tabelrij. 

De *function.json* bestand geeft een `partitionKey` en een `rowKey`. De `rowKey` '{queueTrigger}' van de waarde geeft aan dat de rijsleutel is afkomstig uit de wachtrij bericht-tekenreeks.

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de F #-code:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Invoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een tabel invoer binding in een *function.json* bestands- en [JavaScript-code] (functies verwijzing node.md) die gebruikmaakt van de binding. De functie maakt gebruik van een trigger wachtrij lezen van een enkele tabelrij. 

De *function.json* bestand geeft een `partitionKey` en een `rowKey`. De `rowKey` '{queueTrigger}' van de waarde geeft aan dat de rijsleutel is afkomstig uit de wachtrij bericht-tekenreeks.

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

De [configuratie](#input---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

## <a name="input---attributes"></a>Invoer - kenmerken
 
In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken voor het configureren van een tabel invoer binding:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

  De constructor van het kenmerk werkt met de tabelnaam, partitiesleutel en rijsleutel. Deze kan worden gebruikt op een out-parameter of op de geretourneerde waarde van de functie, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      TraceWriter log)
  {
      ...
  }
  ```

  U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      TraceWriter log)
  {
      ...
  }
  ```

  Zie voor een compleet voorbeeld [invoer - C#-voorbeeld](#input---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Biedt een andere manier om op te geven van de storage-account te gebruiken. De constructor, wordt de naam van een app-instelling met een verbindingsreeks voor opslag. Het kenmerk kan worden toegepast op het parameter, klasseniveau of methode. Het volgende voorbeeld ziet u klasseniveau en methode:

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

Het opslagaccount moet worden gebruikt, wordt bepaald in de volgende volgorde:

* De `Table` van het kenmerk `Connection` eigenschap.
* De `StorageAccount` kenmerk toegepast op de dezelfde parameter als de `Table` kenmerk.
* De `StorageAccount` kenmerk toegepast op de functie.
* De `StorageAccount` kenmerk toegepast op de klasse.
* Het standaardopslagaccount voor de functie-app (app-instelling 'AzureWebJobsStorage').

## <a name="input---configuration"></a>Invoer - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Table` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `table`. Deze eigenschap wordt automatisch ingesteld bij het maken van de binding in de Azure portal.|
|**richting** | N.v.t. | moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld bij het maken van de binding in de Azure portal. |
|**naam** | N.v.t. | De naam van de variabele die staat voor de tabel of de entiteit in functiecode. | 
|**tableName** | **TableName** | De naam van de tabel.| 
|**partitionKey** | **PartitionKey** |Optioneel. De partitiesleutel van de Tabelentiteit om te lezen. Zie de [gebruik](#input---usage) sectie voor hulp bij het gebruik van deze eigenschap.| 
|**rowKey** |**RowKey** | Optioneel. De rijsleutel van de Tabelentiteit om te lezen. Zie de [gebruik](#input---usage) sectie voor hulp bij het gebruik van deze eigenschap.| 
|**duren** |**Duren** | Optioneel. Het maximum aantal entiteiten kunnen worden gelezen in JavaScript. Zie de [gebruik](#input---usage) sectie voor hulp bij het gebruik van deze eigenschap.| 
|**filter** |**Filter** | Optioneel. Een OData-filterexpressie voor de tabel invoer in JavaScript. Zie de [gebruik](#input---usage) sectie voor hulp bij het gebruik van deze eigenschap.| 
|**verbinding** |**Verbinding** | De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Invoer - gebruik

De tabel opslag invoer binding ondersteunt de volgende scenario's:

* **Lezen van een rij in C# of C#-script**

  Stel `partitionKey` en `rowKey`. Toegang tot gegevens in de tabel met behulp van een methodeparameter `T <paramName>`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T`is meestal een type dat wordt geïmplementeerd `ITableEntity` of is afgeleid van `TableEntity`. De `filter` en `take` eigenschappen worden niet gebruikt in dit scenario. 

* **Een of meer rijen in C# of C# script lezen**

  Toegang tot gegevens in de tabel met behulp van een methodeparameter `IQueryable<T> <paramName>`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T`moet een type dat implementeert `ITableEntity` of is afgeleid van `TableEntity`. U kunt `IQueryable` methoden wilt filteren vereist. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen worden niet gebruikt in dit scenario.  

> [!NOTE]
> `IQueryable`werkt niet in .NET Core, zodat deze werkt niet in de [runtime van Functions v2](functions-versions.md).

  Een alternatief is een `CloudTable paramName` methodeparameter om te lezen van de tabel met behulp van de Azure-opslag-SDK.

* **Lezen van een of meer rijen in JavaScript**

  Stel de `filter` en `take` eigenschappen. Stelt niet `partitionKey` of `rowKey`. Toegang tot de invoer tabel entiteit (of entiteiten) met behulp van `context.bindings.<name>`. De gedeserialiseerde objecten hebben `RowKey` en `PartitionKey` eigenschappen.

## <a name="output"></a>Uitvoer

Een Azure Table storage uitvoer binding entiteiten schrijven naar een tabel in een Azure Storage-account gebruiken.

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die gebruikmaakt van een HTTP-trigger voor het schrijven van een enkele tabelrij. 

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
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

Het volgende voorbeeld ziet u de tabeluitvoer van een het binden van een *function.json* bestand en [C# script](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie schrijft meerdere tabelentiteiten.

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
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

### <a name="output---f-example"></a>Output - F #-voorbeeld

Het volgende voorbeeld ziet u de tabeluitvoer van een het binden van een *function.json* bestand en [F # script](functions-reference-fsharp.md) code die gebruikmaakt van de binding. De functie schrijft meerdere tabelentiteiten.

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de F #-code:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

Het volgende voorbeeld ziet u de tabeluitvoer van een het binden van een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie schrijft meerdere tabelentiteiten.

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

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

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

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

De constructor van het kenmerk wordt de tabelnaam. Kan worden gebruikt op een `out` parameter of op de geretourneerde waarde van de functie, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
{
    ...
}
```

U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - C#-voorbeeld](#output---c-example).

U kunt de `StorageAccount` het kenmerk met de storage-account op niveau van de klasse, methode of parameter opgeven. Zie voor meer informatie [invoer - kenmerken](#input---attributes).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Table` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `table`. Deze eigenschap wordt automatisch ingesteld bij het maken van de binding in de Azure portal.|
|**richting** | N.v.t. | moet worden ingesteld op `out`. Deze eigenschap wordt automatisch ingesteld bij het maken van de binding in de Azure portal. |
|**naam** | N.v.t. | De naam van de variabele gebruikt in de functiecode die de tabel of een entiteit vertegenwoordigt. Ingesteld op `$return` om te verwijzen naar de retourwaarde van de functie.| 
|**tableName** |**TableName** | De naam van de tabel.| 
|**partitionKey** |**PartitionKey** | De partitiesleutel van de Tabelentiteit om te schrijven. Zie de [sectie gebruik](#output---usage) voor hulp bij het gebruik van deze eigenschap.| 
|**rowKey** |**RowKey** | De rijsleutel van de Tabelentiteit om te schrijven. Zie de [sectie gebruik](#output---usage) voor hulp bij het gebruik van deze eigenschap.| 
|**verbinding** |**Verbinding** | De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik

De Table storage uitvoer binding ondersteunt de volgende scenario's:

* **Schrijven van een rij in een andere taal**

  In C# en C# script, toegang krijgen tot de entiteit van de tabel uitvoer met een methodeparameter zoals `out T paramName` of de functie retourwaarde bevat. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T`kan serialiseerbaar type zijn als de partitiesleutel en de rijsleutel worden geleverd door de *function.json* bestand of de `Table` kenmerk. Anders `T` moet een type met `PartitionKey` en `RowKey` eigenschappen. In dit scenario `T` doorgaans implementeert `ITableEntity` of is afgeleid van `TableEntity`, maar heeft geen aan.

* **Een of meer rijen in C# of C# schrijven**

  In C# en C# script, toegang krijgen tot de entiteit van de tabel uitvoer met een methodeparameter `ICollector<T> paramName` of `ICollectorAsync<T> paramName`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. `T`Hiermee geeft u het schema van de entiteiten die u wilt toevoegen. Normaal gesproken `T` is afgeleid van `TableEntity` of implementeert `ITableEntity`, maar heeft geen aan. De partitiesleutel en rij waarden in sleutel *function.json* of de `Table` kenmerkconstructor worden niet gebruikt in dit scenario.

  Een alternatief is een `CloudTable paramName` methodeparameter om te schrijven naar de tabel met behulp van de Azure-opslag-SDK.

* **Schrijven van een of meer rijen in JavaScript**

  Toegang tot de tabel met behulp van de uitvoer in JavaScript-functies `context.bindings.<name>`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
