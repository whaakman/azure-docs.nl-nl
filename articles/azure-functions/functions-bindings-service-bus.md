---
title: Azure Service Bus-bindingen voor Azure Functions
description: Het gebruik van Azure Service Bus-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: tdykstra
ms.openlocfilehash: 2df003d47291570b31e1091f34994e4023000981
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Service Bus-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met Azure Service Bus-bindingen in de Azure Functions. Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Service Bus-wachtrijen en onderwerpen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

Gebruik de Service Bus-trigger om te reageren op berichten van een Service Bus-wachtrij of onderwerp. 

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die zich aanmeldt met een Service Bus-wachtrij-bericht.

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie registreert een Service Bus-wachtrij-bericht.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Dit is de C#-scriptcode:

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---f-example"></a>Trigger - F #-voorbeeld

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie registreert een Service Bus-wachtrij-bericht. 

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Dit is de code F # script:

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie registreert een Service Bus-wachtrij-bericht. 

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Hier volgt de JavaScript-scriptcode:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken voor het configureren van een Service Bus-trigger:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusTriggerAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  De constructor van het kenmerk wordt de naam van de wachtrij of het onderwerp en een abonnement. U kunt ook de toegangsrechten voor de verbinding opgeven. Als u geen toegangsrechten opgeeft, wordt standaard `Manage`. Het kiezen van de instelling rechten wordt uitgelegd in de [Trigger - configuratie](#trigger---configuration) sectie. Hier volgt een voorbeeld waarin het kenmerk met een tekenreeksparameter gebruikt:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  U kunt instellen de `Connection` eigenschap om de Service Bus-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Zie voor een compleet voorbeeld [Trigger - C#-voorbeeld](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Biedt een andere manier om op te geven van de Service Bus-account moet worden gebruikt. De constructor, wordt de naam van een app-instelling die een Service Bus-verbindingsreeks bevat. Het kenmerk kan worden toegepast op het parameter, klasseniveau of methode. Het volgende voorbeeld ziet u klasseniveau en methode:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

De Service Bus-account moet worden gebruikt, wordt bepaald in de volgende volgorde:

* De `ServiceBusTrigger` van het kenmerk `Connection` eigenschap.
* De `ServiceBusAccount` kenmerk toegepast op de dezelfde parameter als de `ServiceBusTrigger` kenmerk.
* De `ServiceBusAccount` kenmerk toegepast op de functie.
* De `ServiceBusAccount` kenmerk toegepast op de klasse.
* De 'AzureWebJobsServiceBus' app-instelling.

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `ServiceBusTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'serviceBusTrigger'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | N.v.t. | Moet worden ingesteld op 'in'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij of onderwerp bericht in de functiecode. Ingesteld op '$return' om te verwijzen naar de retourwaarde van de functie. | 
|**Wachtrijnaam**|**Wachtrijnaam**|De naam van de wachtrij om te controleren.  Alleen ingesteld als een wachtrij, niet voor een onderwerp bewaking.
|**topicName**|**TopicName**|De naam van het onderwerp om te controleren. Alleen ingesteld als een onderwerp, voor een wachtrij niet controleren.|
|**subscriptionName**|**SubscriptionName**|Naam van het abonnement om te controleren. Alleen ingesteld als een onderwerp, voor een wachtrij niet controleren.|
|**verbinding**|**Verbinding**|De naam van een app-instelling met de Service Bus-verbindingsreeks moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam opgeven. Als u bijvoorbeeld `connection` naar 'MyServiceBus', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyServiceBus'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard Service Bus-verbindingsreeks in de app-instelling met de naam 'AzureWebJobsServiceBus'.<br><br>Als u een verbindingsreeks, volgt u de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). De verbindingsreeks moet voor een Service Bus-naamruimte niet beperkt tot een specifieke wachtrij of onderwerp. |
|**accessRights**|**Toegang**|Toegangsrechten voor de verbindingsreeks. Beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, wat aangeeft dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` ' luisteren '. De runtime mislukken kan voor bewerkingen waarvoor probeert functies beheren anders rechten.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

In C# en C# script, opent u het bericht wachtrij of onderwerp via een methodeparameter zoals `string paramName`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt een van de volgende typen in plaats van `string`:

* `byte[]`-Handig voor binaire gegevens.
* Een aangepast type - als het bericht JSON bevat, Azure Functions geprobeerd te deserialiseren van de JSON-gegevens.
* `BrokeredMessage`-Kunt u het gedeserialiseerde bericht met de [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) methode.

In JavaScript, opent u het bericht wachtrij of onderwerp via `context.bindings.<name>`. `<name>`de waarde is opgegeven in de `name` eigenschap van *function.json*. Het Service Bus-bericht wordt doorgegeven aan de functie als een tekenreeks- of JSON-object.

## <a name="trigger---poison-messages"></a>Trigger - verontreinigde berichten

Verwerken van verontreinigde berichten kan niet worden beheerd of geconfigureerd in Azure Functions. Service Bus verwerkt verontreinigde berichten zelf.

## <a name="trigger---peeklock-behavior"></a>Trigger - PeekLock gedrag

De runtime van Functions ontvangt een bericht in [PeekLock modus](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Roept `Complete` op het bericht als de functie wordt voltooid of aanroepen `Abandon` als de functie is mislukt. Als de functie wordt uitgevoerd langer dan de `PeekLock` een time-out opgetreden, de vergrendeling wordt automatisch verlengd.

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen host.json

De [host.json](functions-host-json.md#servicebus) bestand bevat instellingen die Service Bus-trigger besturen.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-service-bus.md)]

## <a name="output"></a>Uitvoer

Azure Service Bus-uitvoer binding gebruiken voor het verzenden van berichten in wachtrij of onderwerp.

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een Service Bus-wachtrij-bericht verzonden:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

Het volgende voorbeeld ziet u de uitvoer van een Service Bus binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie gebruikt een timertrigger te verzenden van een wachtrijbericht elke 15 seconden.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Hier volgt een C# script-code die een enkel bericht worden gemaakt:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Hier volgt C# script-code die wordt gemaakt van meerdere berichten:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Output - F #-voorbeeld

Het volgende voorbeeld ziet u de uitvoer van een Service Bus binding in een *function.json* bestand en een [F # scriptfunctie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie gebruikt een timertrigger te verzenden van een wachtrijbericht elke 15 seconden.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Hier volgt F # scriptcode die wordt gemaakt van een enkel bericht:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

Het volgende voorbeeld ziet u de uitvoer van een Service Bus binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie gebruikt een timertrigger te verzenden van een wachtrijbericht elke 15 seconden.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Hier volgt een JavaScript-scriptcode die één bericht worden gemaakt:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Hier volgt een JavaScript-scriptcode die meerdere berichten worden gemaakt:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

De constructor van het kenmerk wordt de naam van de wachtrij of het onderwerp en een abonnement. U kunt ook de toegangsrechten voor de verbinding opgeven. Het kiezen van de instelling rechten wordt uitgelegd in de [Output - configuratie](#output---configuration) sectie. Hier volgt een voorbeeld waarin het kenmerk dat wordt toegepast op de geretourneerde waarde van de functie:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

U kunt instellen de `Connection` eigenschap om de Service Bus-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - C#-voorbeeld](#output---c-example).

U kunt de `ServiceBusAccount` kenmerk om op te geven van de Service Bus-account moet worden gebruikt op niveau van de klasse, methode of parameter.  Zie voor meer informatie [Trigger - kenmerken](#trigger---attributes).

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `ServiceBus` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'Service Bus'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | N.v.t. | Moet worden ingesteld op 'out'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij of onderwerp in de functiecode. Ingesteld op '$return' om te verwijzen naar de retourwaarde van de functie. | 
|**Wachtrijnaam**|**Wachtrijnaam**|De naam van de wachtrij.  Alleen ingesteld als Wachtrijberichten, niet voor een onderwerp verzenden.
|**topicName**|**TopicName**|De naam van het onderwerp om te controleren. Alleen ingesteld als onderwerp berichten, niet voor een wachtrij te verzenden.|
|**subscriptionName**|**SubscriptionName**|Naam van het abonnement om te controleren. Alleen ingesteld als onderwerp berichten, niet voor een wachtrij te verzenden.|
|**verbinding**|**Verbinding**|De naam van een app-instelling met de Service Bus-verbindingsreeks moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam opgeven. Als u bijvoorbeeld `connection` naar 'MyServiceBus', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyServiceBus'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard Service Bus-verbindingsreeks in de app-instelling met de naam 'AzureWebJobsServiceBus'.<br><br>Als u een verbindingsreeks, volgt u de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). De verbindingsreeks moet voor een Service Bus-naamruimte niet beperkt tot een specifieke wachtrij of onderwerp.|
|**accessRights**|**Toegang** |Toegangsrechten voor de verbindingsreeks. Beschikbare waarden zijn 'beheren' en 'luisteren'. De standaardwaarde is 'beheren', wat aangeeft dat de verbinding heeft **beheren** machtigingen. Als u een verbindingsreeks die geen **beheren** machtigingen ingesteld `accessRights` ' luisteren '. De runtime mislukken kan voor bewerkingen waarvoor probeert functies beheren anders rechten.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik

In C# en C# script, toegang krijgen tot de wachtrij of onderwerp met een methodeparameter zoals `out string paramName`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt een van de volgende parametertypen gebruiken:

* `out T paramName` - `T`JSON-serialiseerbaar type kan zijn. Als de waarde van parameter null is wanneer de functie wordt afgesloten, wordt het bericht in functies gemaakt met een null-object.
* `out string`-Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `out byte[]`-Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `out BrokeredMessage`-Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.

U kunt gebruiken voor het maken van meerdere berichten in een C# of functie van C#-script, `ICollector<T>` of `IAsyncCollector<T>`. Een bericht wordt gemaakt bij het aanroepen van de `Add` methode.

Opent de wachtrij of onderwerp via in JavaScript, `context.bindings.<name>`. `<name>`de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt een tekenreeks, een bytematrix of een Javascript-object (gedeserialiseerd naar JSON) toewijzen aan `context.binding.<name>`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
