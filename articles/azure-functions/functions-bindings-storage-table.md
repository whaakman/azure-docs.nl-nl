---
title: Azure Storage-tabel functies bindingen | Microsoft Docs
description: Het gebruik van Azure Storage-bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 486b7c31c914ba7bb2d75e3f83ccf346a09104e8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-storage-table-bindings"></a>Azure Functions opslag Tabelverbindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dit artikel wordt uitgelegd hoe u configureert en code Azure Storage Tabelverbindingen in de Azure Functions. Azure Functions ondersteunt invoer en uitvoer van de bindingen voor Azure Storage-tabellen.

De binding van de tabel Storage ondersteunt de volgende scenario's:

* **Lezen van een enkele rij in een C# of Node.js-functie** : Stel `partitionKey` en `rowKey`. De `filter` en `take` eigenschappen worden niet gebruikt in dit scenario.
* **Lezen van meerdere rijen in een C#-functie** -de runtime van Functions biedt een `IQueryable<T>` object gekoppeld aan de tabel. Type `T` moet worden afgeleid van `TableEntity` of implementeert `ITableEntity`. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen niet in dit scenario worden gebruikt; u kunt de `IQueryable` -object om alle filters vereist. 
* **Lezen van meerdere rijen in een functie knooppunt** : Stel de `filter` en `take` eigenschappen. Stelt niet `partitionKey` of `rowKey`.
* **Schrijven van een of meer rijen in een C#-functie** -de runtime van Functions biedt een `ICollector<T>` of `IAsyncCollector<T>` gekoppeld aan de tabel waar `T` Hiermee geeft u het schema van de entiteiten die u wilt toevoegen. Normaal gesproken Typ `T` is afgeleid van `TableEntity` of implementeert `ITableEntity`, maar deze hoeft niet te. De `partitionKey`, `rowKey`, `filter`, en `take` eigenschappen worden niet gebruikt in dit scenario.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Invoer tabelbinding opslag
Invoer tabelbinding van Azure Storage kunt u een tabel met opslag in de functie gebruiken. 

De invoer van de tabel opslag aan een functie maakt gebruik van de volgende JSON-objecten in de `bindings` matrix van function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Houd rekening met het volgende: 

* Gebruik `partitionKey` en `rowKey` samen te lezen van één entiteit. Deze eigenschappen zijn optioneel. 
* `connection`moet de naam van een app-instelling met een verbindingsreeks voor opslag bevatten. In de Azure portal, de standaard editor in de **integreren** tabblad configureert u deze appinstelling voor wanneer u een opslagruimte maakt account of een bestaande selecteert. U kunt ook [configureren van deze app handmatig instellen](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Invoer-gebruik
In C#-functies, u koppelt aan de invoertabel entiteit (of entiteiten) met behulp van een benoemde parameter in de functiehandtekening, zoals `<T> <name>`.
Waar `T` is het gegevenstype dat u wilt deserialiseren van de gegevens in, en `paramName` is de naam die u hebt opgegeven in de [invoer binding](#input). In Node.js-functies, opent u de invoertabel entiteit (of entiteiten) met behulp van `context.bindings.<name>`.

De ingevoerde gegevens kunnen worden gedeserialiseerd in Node.js- of C#-functies. De gedeserialiseerde objecten hebben `RowKey` en `PartitionKey` eigenschappen.

U kunt ook binden aan een van de volgende typen in C#-functies, en de runtime van Functions wordt geprobeerd te deserialiseren met behulp van dat type gegevens in de tabel:

* Type dat wordt geïmplementeerd`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Voorbeeld van invoer
Stel, dat u hebt de volgende function.json dat gebruikmaakt van een trigger wachtrij om te lezen van een enkele tabelrij. De JSON geeft `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`Hiermee wordt aangegeven dat de rijsleutel is afkomstig uit de wachtrij bericht-tekenreeks.

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
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Zie het voorbeeld taalspecifieke die een met één Tabelentiteit leest.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Invoer voorbeeld in C# #
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

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>Invoer voorbeeld in F # #
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

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Invoer voorbeeld in Node.js
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Table Storage uitvoer binding
De uitvoer van de Azure Storage-tabel binding kunt tabel u entiteiten schrijven naar een opslag in de functie. 

De opslag tabel uitvoer voor een functie maakt gebruik van de volgende JSON-objecten in de `bindings` matrix van function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Houd rekening met het volgende: 

* Gebruik `partitionKey` en `rowKey` samen om te schrijven één entiteit. Deze eigenschappen zijn optioneel. U kunt ook opgeven `PartitionKey` en `RowKey` wanneer u de entiteitsobjecten in uw functiecode maakt.
* `connection`moet de naam van een app-instelling met een verbindingsreeks voor opslag bevatten. In de Azure portal, de standaard editor in de **integreren** tabblad configureert u deze appinstelling voor wanneer u een opslagruimte maakt account of een bestaande selecteert. U kunt ook [configureren van deze app handmatig instellen](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Gebruik voor uitvoer
In C# functies, bindt u aan de tabeluitvoer van de met behulp van de benoemde `out` parameter in de functiehandtekening, zoals `out <T> <name>`, waarbij `T` is het gegevenstype dat u wilt serialiseren van de gegevens in, en `paramName` is de naam die u hebt opgegeven in de [uitvoer binding](#output). In een Node.js-functies, opent u de uitvoer met behulp van tabel `context.bindings.<name>`.

Objecten in Node.js- of C#-functies kunnen worden geserialiseerd. In C# functies, kunt u ook koppelen aan de volgende typen:

* Type dat wordt geïmplementeerd`ITableEntity`
* `ICollector<T>`(om de uitvoer van meerdere entiteiten. Zie [voorbeeld](#outcsharp).)
* `IAsyncCollector<T>`(async-versie van `ICollector<T>`)
* `CloudTable`(met behulp van de Azure-opslag-SDK. Zie [voorbeeld](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Voorbeeld van uitvoer
De volgende *function.json* en *run.csx* voorbeeld ziet u het schrijven van meerdere tabelentiteiten.

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
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Zie het voorbeeld taalspecifieke die meerdere tabelentiteiten worden gemaakt.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Voorbeeld van uitvoer in C# #
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
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Voorbeeld van uitvoer in F # #
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

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Voorbeeld van uitvoer in Node.js
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

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Voorbeeld: Meerdere tabelentiteiten in C# lezen  #
De volgende *function.json* en C#-codevoorbeeld entiteiten voor een partitiesleutel die is opgegeven in het bericht uit de wachtrij staat.

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
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De C#-code een verwijzing naar de Azure-opslag-SDK wordt toegevoegd zodat het entiteitstype kan worden afgeleid van `TableEntity`.

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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

