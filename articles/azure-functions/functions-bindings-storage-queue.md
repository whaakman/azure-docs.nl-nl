---
title: Azure Queue storage-bindingen voor Azure Functions
description: Begrijpen hoe de Azure Queue storage trigger te gebruiken en de uitvoer van de binding in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/23/2017
ms.author: glenga
ms.openlocfilehash: 2ca511bf0c145878cc80bdbae694f581fd487820
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Azure Queue storage-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met Azure Queue storage bindingen in de Azure Functions. Azure Functions ondersteunt activeren en uitvoer van de bindingen voor wachtrijen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

Gebruik de trigger wachtrij een functie wordt gestart wanneer een nieuw item is ontvangen op een wachtrij. Bericht uit de wachtrij is opgegeven als invoer voor de functie.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die controleert of de `myqueue-items` wachtrij en schrijft een logboek telkens wanneer een wachtrij-item wordt verwerkt.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een blob-trigger binding in een *function.json* bestand en [C# script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie polls de `myqueue-items` wachtrij en schrijft een logboek telkens wanneer een wachtrij-item wordt verwerkt.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Dit is de C#-scriptcode:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

De [gebruik](#trigger---usage) sectie wordt uitgelegd `myQueueItem`, die door de naam de `name` eigenschap in function.json.  De [metagegevenssectie bericht](#trigger---message-metadata) wordt uitgelegd dat alle variabelen weergegeven.

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een blob-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie polls de `myqueue-items` wachtrij en schrijft een logboek telkens wanneer een wachtrij-item wordt verwerkt.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

De [gebruik](#trigger---usage) sectie wordt uitgelegd `myQueueItem`, die door de naam de `name` eigenschap in function.json.  De [metagegevenssectie bericht](#trigger---message-metadata) wordt uitgelegd dat alle variabelen weergegeven.

## <a name="trigger---attributes"></a>Trigger - kenmerken
 
In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken voor het configureren van een trigger wachtrij:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  De constructor van het kenmerk wordt de naam van de wachtrij te bewaken, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Zie voor een compleet voorbeeld [Trigger - C#-voorbeeld](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Biedt een andere manier om op te geven van de storage-account te gebruiken. De constructor, wordt de naam van een app-instelling met een verbindingsreeks voor opslag. Het kenmerk kan worden toegepast op het parameter, klasseniveau of methode. Het volgende voorbeeld ziet u klasseniveau en methode:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Het opslagaccount moet worden gebruikt, wordt bepaald in de volgende volgorde:

* De `QueueTrigger` van het kenmerk `Connection` eigenschap.
* De `StorageAccount` kenmerk toegepast op de dezelfde parameter als de `QueueTrigger` kenmerk.
* De `StorageAccount` kenmerk toegepast op de functie.
* De `StorageAccount` kenmerk toegepast op de klasse.
* De 'AzureWebJobsStorage' app-instelling.

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `QueueTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t.| moet worden ingesteld op `queueTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting**| N.v.t. | In de *function.json* alleen het bestand. moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | N.v.t. |De naam van de variabele die staat voor de wachtrij in functiecode.  | 
|**Wachtrijnaam** | **Wachtrijnaam**| De naam van de wachtrij om te pollen. | 
|**verbinding** | **Verbinding** |De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik
 
In C# en C# script, toegang heeft tot de blob-gegevens met behulp van een methodeparameter zoals `Stream paramName`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt koppelen aan een van de volgende typen:

* Object - de runtime van Functions deserializes POCO een JSON-nettolading in een POCO-object. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Gebruik in JavaScript, `context.bindings.<name>` voor toegang tot de nettolading van de wachtrij-item. Als de nettolading van de JSON, wordt deze in een object gedeserialiseerd.

## <a name="trigger---message-metadata"></a>Trigger - bericht metagegevens

De trigger wachtrij biedt verschillende eigenschappen voor metagegevens. Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies voor gegevensbinding in andere bindingen of als parameters in uw code. De waarden hebben dezelfde betekenis als [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage).

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|`QueueTrigger`|`string`|Wachtrij-nettolading (als een geldige tekenreeks). Als de wachtrij nettolading als een tekenreeks bericht `QueueTrigger` heeft dezelfde waarde als de variabele met de naam door de `name` eigenschap in *function.json*.|
|`DequeueCount`|`int`|Het aantal keren dat dit bericht uit wachtrij is geplaatst.|
|`ExpirationTime`|`DateTimeOffset?`|De tijd waarop het bericht is verlopen.|
|`Id`|`string`|Wachtrij bericht-ID.|
|`InsertionTime`|`DateTimeOffset?`|De tijd die het bericht is toegevoegd aan de wachtrij.|
|`NextVisibleTime`|`DateTimeOffset?`|De tijd die het bericht vervolgens zichtbaar zijn.|
|`PopReceipt`|`string`|Pop ontvangst van het bericht.|

## <a name="trigger---poison-messages"></a>Trigger - verontreinigde berichten

Wanneer een functie van de trigger wachtrij mislukt Azure Functions probeert opnieuw, voor de functie maximaal vijf keer een bepaalde wachtrij-bericht, met inbegrip van de eerste poging. Als alle vijf pogingen mislukken, wordt een bericht met de runtime van functions toegevoegd aan een wachtrij met de naam  *&lt;originalqueuename >-verontreinigd*. U kunt schrijven om een functie verwerken van berichten uit de wachtrij verontreinigd door registratie of het verzenden van een melding dat handmatige aandacht nodig is.

Voor het afhandelen van verontreinigde berichten handmatig, Controleer de [dequeueCount](#trigger---message-metadata) van het bericht uit de wachtrij.

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen host.json

De [host.json](functions-host-json.md#queues) bestand bevat instellingen die wachtrij trigger gedrag bepalen.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Uitvoer

De Azure Queue storage uitvoer binding berichten schrijven naar een wachtrij gebruiken.

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) dat een wachtrijbericht voor elke HTTP-aanvraag ontvangen maakt.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

Het volgende voorbeeld ziet u een blob-trigger binding in een *function.json* bestand en [C# script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie maakt een wachtrij-item met een POCO-nettolading voor elke HTTP-aanvraag ontvangen.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt een C# script-code die een enkel wachtrijbericht maakt:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

U kunt meerdere berichten tegelijkertijd verzenden met behulp van een `ICollector` of `IAsyncCollector` parameter. Hier volgt C#-scriptcode waarmee meerdere berichten, één met de gegevens van de HTTP-aanvragen en één met de vastgelegde waarden worden verzonden:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItem, 
    TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een blob-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie maakt een wachtrij-item voor elke HTTP-aanvraag ontvangen.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

De [configuratie](#output---configuration) sectie wordt uitgelegd deze eigenschappen.

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

U kunt meerdere berichten tegelijk verzenden door te definiëren van de matrix van een bericht voor de `myQueueItem` uitvoer van de binding. De volgende JavaScript-code verzendt twee Wachtrijberichten met vastgelegde waarden voor elke HTTP-aanvraag ontvangen.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes"></a>Output - kenmerken
 
In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

Het kenmerk is van toepassing op een `out` parameter of de retourwaarde van de functie. De constructor van het kenmerk wordt de naam van de wachtrij, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

U kunt instellen de `Connection` eigenschap om de storage-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - C#-voorbeeld](#output---c-example).

U kunt de `StorageAccount` het kenmerk met de storage-account op niveau van de klasse, methode of parameter opgeven. Zie voor meer informatie [Trigger - kenmerken](#trigger---attribute).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Queue` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `queue`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | N.v.t. | moet worden ingesteld op `out`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij in functiecode. Ingesteld op `$return` om te verwijzen naar de retourwaarde van de functie.| 
|**Wachtrijnaam** |**Wachtrijnaam** | De naam van de wachtrij. | 
|**verbinding** | **Verbinding** |De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'MyStorage', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyStorage'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard-verbindingsreeks voor opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik
 
In C# en C# script, schrijft u een enkel wachtrijbericht met behulp van een methodeparameter zoals `out T paramName`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt het retourtype van methode in plaats van een `out` parameter, en `T` kan bestaan uit een van de volgende typen:

* Een POCO serialiseerbaar als JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

Geschreven in C# en C# script, meerdere berichten in wachtrij plaatsen met behulp van een van de volgende typen: 

* `ICollector<T>` of `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Gebruik in JavaScript-functies `context.bindings.<name>` voor toegang tot de wachtrijbericht uitvoer. U kunt een tekenreeks of een JSON-serialiseerbaar object gebruiken voor de nettolading van de wachtrij-item.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een Quick Start die gebruikmaakt van een wachtrij opslag trigger](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Ga naar een zelfstudie die gebruikmaakt van een Queue storage uitvoer binding](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
