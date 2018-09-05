---
title: Azure Queue storage-bindingen voor Azure Functions
description: Informatie over het gebruik van de trigger voor Azure Queue storage en Storage-Uitvoerbinding in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
editor: ''
tags: ''
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/03/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 996a53751d6b8c6dd06084da371badb0c31d367f
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43667516"
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Azure Queue storage-bindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u werkt met Azure Queue storage-bindingen in Azure Functions. Azure Functions ondersteunt activeren en uitvoerbindingen voor wachtrijen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Queue storage-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet-pakket versie 2.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Queue) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Queue storage-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

Een functie starten wanneer een nieuw item wordt ontvangen in een wachtrij gebruiken de wachtrijtrigger. Bericht uit de wachtrij wordt geleverd als invoer voor de functie.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---Java-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die pollt de `myqueue-items` in de wachtrij en schrijft een logboek telkens wanneer een wachtrij-item wordt verwerkt.

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

Het volgende voorbeeld ziet u een wachtrijtrigger binding in een *function.json* bestand en [C#-script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie polls de `myqueue-items` in de wachtrij en schrijft een logboek telkens wanneer een wachtrij-item wordt verwerkt.

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

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

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

De [gebruik](#trigger---usage) sectie wordt uitgelegd `myQueueItem`, die met de naam van de `name` eigenschap in de function.json.  De [bericht sectie met metagegevens](#trigger---message-metadata) wordt uitgelegd dat alle andere variabelen die worden weergegeven.

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een wachtrijtrigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie polls de `myqueue-items` in de wachtrij en schrijft een logboek telkens wanneer een wachtrij-item wordt verwerkt.

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

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

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

De [gebruik](#trigger---usage) sectie wordt uitgelegd `myQueueItem`, die met de naam van de `name` eigenschap in de function.json.  De [bericht sectie met metagegevens](#trigger---message-metadata) wordt uitgelegd dat alle andere variabelen die worden weergegeven.

### <a name="trigger---java-example"></a>Trigger - Java-voorbeeld

Het volgende Java-voorbeeld ziet u een trigger voor storage-wachtrij-functies die het geactiveerde bericht in de wachtrij geplaatst `myqueuename`.
 
 ```java
 @FunctionName("queueprocessor")
 public void run(
    @QueueTrigger(name = "msg",
                   queueName = "myqueuename",
                   connection = "myconnvarname") String message,
     final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

## <a name="trigger---attributes"></a>Trigger - kenmerken
 
In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken om te configureren van een wachtrijtrigger:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs)

  De constructor van het kenmerk wordt de naam van de wachtrij om te controleren, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Biedt een andere manier om op te geven van de storage-account te gebruiken. De constructor, wordt de naam van een app-instelling met een verbindingsreeks voor opslag. Het kenmerk kan worden toegepast op de parameter, klasseniveau of methode. Het volgende voorbeeld toont het klasseniveau en methode:

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

Het storage-account te gebruiken, wordt bepaald in de volgende volgorde:

* De `QueueTrigger` van kenmerk `Connection` eigenschap.
* De `StorageAccount` kenmerk toegepast op de dezelfde parameter als de `QueueTrigger` kenmerk.
* De `StorageAccount` kenmerk toegepast op de functie.
* De `StorageAccount` kenmerk toegepast op de klasse.
* De 'AzureWebJobsStorage' app-instelling.

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `QueueTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t.| Moet worden ingesteld op `queueTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction**| N.v.t. | In de *function.json* alleen het bestand. Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | N.v.t. |De naam van de variabele die staat voor de wachtrij in functiecode aan te geven.  | 
|**queueName** | **Wachtrijnaam**| De naam van de wachtrij te peilen. | 
|**verbinding** | **verbinding** |De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik
 
In C# en C#-script, toegang krijgen tot de berichtgegevens met behulp van een methodeparameter zoals `string paramName`. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt koppelen aan een van de volgende typen:

* Object - de Functions-runtime gedeserialiseerd een JSON-nettolading in een exemplaar van een willekeurige klasse is gedefinieerd in uw code. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Als u probeert te binden aan `CloudQueueMessage` en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

Gebruik in JavaScript, `context.bindings.<name>` voor toegang tot de nettolading van de wachtrij-item. Als de nettolading van de JSON is, wordt deze in een object gedeserialiseerd.

## <a name="trigger---message-metadata"></a>Trigger - bericht-metagegevens

De wachtrijtrigger bevat diverse [metagegevenseigenschappen](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies in andere bindingen voor gegevensbinding of als parameters in uw code. Dit zijn de eigenschappen van de [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) klasse.

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|`QueueTrigger`|`string`|Payload van de wachtrij (als een geldige tekenreeks). Als de wachtrij nettolading als een tekenreeks bericht, `QueueTrigger` heeft dezelfde waarde als de variabele met de naam van de `name` eigenschap in *function.json*.|
|`DequeueCount`|`int`|Het aantal keren dat die dit bericht heeft zijn uit wachtrij verwijderd.|
|`ExpirationTime`|`DateTimeOffset`|De tijd die het bericht is verlopen.|
|`Id`|`string`|Wachtrij bericht-ID.|
|`InsertionTime`|`DateTimeOffset`|De tijd die het bericht is toegevoegd aan de wachtrij.|
|`NextVisibleTime`|`DateTimeOffset`|De tijd die het bericht vervolgens zichtbaar zijn.|
|`PopReceipt`|`string`|Van het bericht pop-ontvangstbericht.|

## <a name="trigger---poison-messages"></a>Trigger - Onverwerkbare berichten

Wanneer een activeringsfunctie mislukt, pogingen Azure Functions de functie maximaal vijf keer voor een bepaalde wachtrij-bericht, met inbegrip van de eerste poging. Als alle vijf pogingen mislukken, wordt in de functions-runtime een bericht toevoegt aan een wachtrij met de naam  *&lt;originalqueuename >-onverwerkbare*. U kunt schrijven om een functie voor het verwerken van berichten uit de wachtrij onverwerkbare door registratie of een melding verzonden dat handmatige aandacht is vereist.

Voor het beheer van Onverwerkbare berichten handmatig afhandelen, Controleer de [dequeueCount](#trigger---message-metadata) van bericht uit de wachtrij.

## <a name="trigger---polling-algorithm"></a>Trigger - algoritme voor polling

De wachtrijtrigger implementeert een willekeurig exponentieel uitstel algoritme voor het effect van niet-actieve wachtrij polling op de opslagkosten van de transactie wordt beperkt.  Wanneer een bericht wordt gevonden, de runtime Wacht twee seconden en controleert vervolgens of er een andere message; Wanneer er geen bericht is gevonden, wacht ongeveer vier seconden voordat u doorgaat. Na opeenvolgende mislukte pogingen om een wachtrijbericht, blijft de wachttijd vergroten totdat de maximale wachttijd, die standaard één minuut is bereikt. De maximale wachttijd kan worden geconfigureerd via de `maxPollingInterval` eigenschap in de [host.json bestand](functions-host-json.md#queues).

## <a name="trigger---concurrency"></a>Trigger - gelijktijdigheid

Wanneer er meerdere Wachtrijberichten wachten, wordt de wachtrijtrigger opgehaald van een batch van berichten en exemplaren gelijktijdig voor het verwerken van deze functie wordt aangeroepen. Standaard is de batchgrootte 16. Wanneer het nummer dat wordt verwerkt, u tot en met 8 ontvangt, wordt de runtime opgehaald van een andere batch en start deze berichten worden verwerkt. Daarom is het maximum aantal gelijktijdige berichten worden verwerkt per functie op een virtuele machine (VM) 24 uur per dag. Deze limiet geldt afzonderlijk voor elke wachtrij-geactiveerde functie op elke virtuele machine. Als uw functie-app wordt geschaald naar meerdere virtuele machines, wordt elke virtuele machine wacht voor triggers en probeert uit te voeren van functies. Bijvoorbeeld, als een functie-app kan worden geschaald om naar 3 VM's, is het standaard maximum aantal gelijktijdige exemplaren van een wachtrij geactiveerde functie 72.

De batchgrootte en de drempelwaarde voor het ophalen van een nieuwe batch kunnen worden geconfigureerd in de [host.json bestand](functions-host-json.md#queues). Als u beperken van parallelle uitvoering voor wachtrij geactiveerde functies in een functie-app wilt, kunt u de batchgrootte instellen op 1. Deze instelling wordt voorkomen dat gelijktijdige alleen zo lang uw functie-app wordt uitgevoerd op een enkele virtuele machine (VM). 

De wachtrijtrigger automatisch wordt voorkomen dat een functie verwerken van een wachtrijbericht meerdere keren; functies hoeft niet te worden geschreven naar idempotent zijn.

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen voor host.json

De [host.json](functions-host-json.md#queues) bestand bevat instellingen die het gedrag van wachtrij trigger beheren.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Uitvoer

Gebruik de Azure Queue storage-Uitvoerbinding berichten schrijven naar een wachtrij.

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)
* [Java](#output---java-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die Hiermee maakt u een wachtrijbericht voor elke HTTP-aanvraag ontvangen.

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

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een HTTP-trigger binding in een *function.json* bestand en [C#-script (.csx)](functions-reference-csharp.md) code die gebruikmaakt van de binding. De functie maakt een wachtrij-item met een **CustomQueueMessage** object nettolading voor elke HTTP-aanvraag ontvangen.

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

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt een C#-script-code die wordt gemaakt van een enkel wachtrijbericht:

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

U kunt meerdere berichten in één keer verzenden met behulp van een `ICollector` of `IAsyncCollector` parameter. Hier volgen enkele redenen C#-script-code waarmee meerdere berichten, één met de gegevens van de HTTP-aanvragen en één met de vastgelegde waarden worden verzonden:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    TraceWriter log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een HTTP-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie maakt een wachtrij-item voor elke HTTP-aanvraag ontvangen.

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

De [configuratie](#output---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

U kunt meerdere berichten in één keer verzenden door te definiëren van de matrix van een bericht voor de `myQueueItem` -Uitvoerbinding. De volgende JavaScript-code verzendt twee Wachtrijberichten met vastgelegde waarden voor elke HTTP-aanvraag ontvangen.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

### <a name="output---java-example"></a>Uitvoer - Java-voorbeeld

 Het volgende voorbeeld ziet een Java-functie waarmee een wachtrijbericht voor wanneer ze worden geactiveerd door een HTTP-aanvraag.

```java
@FunctionName("httpToQueue")
@QueueOutput(name = "item", queueName = "myqueue-items", connection = "AzureWebJobsStorage")
 public String pushToQueue(
     @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
     final String message,
     @HttpOutput(name = "response") final OutputBinding&lt;String&gt; result) {
       result.setValue(message + " has been added.");
       return message;
 }
 ```

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@QueueOutput` aantekening op parameters waarvan de waarde kan worden geschreven naar Queue storage.  Het parametertype moet `OutputBinding<T>`, waarbij T alle systeemeigen Java-type van een POJO.


## <a name="output---attributes"></a>Uitvoer - kenmerken
 
In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

Het kenmerk is van toepassing op een `out` parameter of de geretourneerde waarde van de functie. De constructor van het kenmerk wordt de naam van de wachtrij staan, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

U kunt instellen dat de `Connection` eigenschap om op te geven van het opslagaccount dat moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - voorbeeld met C#](#output---c-example).

U kunt de `StorageAccount` kenmerk om op te geven van de storage-account op het niveau van klasse, methode of parameter. Zie voor meer informatie, [Trigger - kenmerken](#trigger---attribute).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `Queue` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `queue`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op `out`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij in functiecode aan te geven. Ingesteld op `$return` om te verwijzen naar de geretourneerde waarde van de functie.| 
|**queueName** |**Wachtrijnaam** | De naam van de wachtrij. | 
|**verbinding** | **verbinding** |De naam van een app-instelling met de verbindingsreeks voor opslag moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam hier opgeven. Als u bijvoorbeeld `connection` naar 'Mijnopslag', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyStorage." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de verbindingsreeks van de standaard-opslag in de app-instelling met de naam `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uitvoer - gebruik
 
Schrijven in C# en C#-script, een enkel wachtrijbericht met behulp van een methodeparameter zoals `out T paramName`. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt het retourtype van de methode in plaats van een `out` parameter en `T` kan dit een van de volgende typen:

* Een geserialiseerd als JSON-object
* `string`
* `byte[]`
* [CloudQueueMessage] 

Als u probeert te binden aan `CloudQueueMessage` en een foutmelding krijgt, zorg ervoor dat u een verwijzing naar [de juiste versie van de Storage-SDK](#azure-storage-sdk-version-in-functions-1x).

Schrijven in C# en C#-script, meerdere berichten in wachtrij plaatsen met behulp van een van de volgende typen: 

* `ICollector<T>` of `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Gebruik in JavaScript-functies, `context.bindings.<name>` voor toegang tot het wachtrijbericht uitvoer. U kunt een tekenreeks of een JSON-geserialiseerd object gebruiken voor de nettolading van de wachtrij-item.


## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding |  Referentie |
|---|---|
| Wachtrij | [Foutcodes voor wachtrij](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| BLOB-, tabel, wachtrij | [Foutcodes voor opslag](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB-, tabel, wachtrij |  [Problemen oplossen](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een snelstartgids die gebruikmaakt van een trigger voor Queue storage](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Ga naar een zelfstudie over het gebruik van een Queue storage-Uitvoerbinding](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
