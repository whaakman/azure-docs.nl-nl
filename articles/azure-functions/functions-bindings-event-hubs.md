---
title: Azure Functions Event Hubs-bindingen
description: Het gebruik van Azure Event Hubs bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: wesmc
ms.openlocfilehash: c2660a3ca8ee7569d49a6998d0dfd5a98a97d294
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Azure Functions Event Hubs-bindingen

Dit artikel wordt uitgelegd hoe u werkt met [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindingen voor Azure Functions. Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="event-hubs-trigger"></a>Event Hubs activeren

Gebruik de Event Hubs-trigger om te reageren op een gebeurtenis verzonden naar de gebeurtenisstroom van een event hub. U moet leestoegang hebben tot de event hub voor het instellen van de trigger.

Wanneer een functie van Event Hubs trigger wordt geactiveerd, wordt het bericht dat deze wordt geactiveerd als een tekenreeks in de functie doorgegeven.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [Vooraf gecompileerde C#](#trigger---c-example)
* [C#-script](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt [vooraf gecompileerd C#](functions-dotnet-class-library.md) code die de logboeken van de berichttekst van de event hub-trigger.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Als u toegang tot de metagegevens van de gebeurtenis, binden aan een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (vereist een `using` -instructie voor `Microsoft.ServiceBus.Messaging`).

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```
Zorg voor het ontvangen van gebeurtenissen in een batch, `string` of `EventData` een matrix:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie Logboeken de berichttekst van de event hub-trigger.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```
Dit is de C#-scriptcode:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Als u toegang tot de metagegevens van de gebeurtenis, binden aan een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (vereist een met-instructie voor `Microsoft.ServiceBus.Messaging`).

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Zorg voor het ontvangen van gebeurtenissen in een batch, `string` of `EventData` een matrix:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Trigger - F #-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie Logboeken de berichttekst van de event hub-trigger.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Dit is de F #-code:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie Logboeken de berichttekst van de event hub-trigger.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

## <a name="trigger---attributes-for-precompiled-c"></a>Trigger - kenmerken voor vooraf gecompileerde C#

Voor [vooraf gecompileerd C#](functions-dotnet-class-library.md) functies, gebruiken de [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

De constructor van het kenmerk werkt met de naam van de event hub, de naam van de consumergroep en de naam van een app-instelling met de verbindingsreeks. Zie voor meer informatie over deze instellingen de [activeren configuratiesectie](#trigger---configuration). Hier volgt een `EventHubTriggerAttribute` kenmerk voorbeeld:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
```

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `EventHubTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | moet worden ingesteld op `eventHubTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**richting** | N.v.t. | moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**naam** | N.v.t. | De naam van de variabele die staat voor de gebeurtenis in de functiecode. | 
|**pad** |**EventHubName** | De naam van de event hub. | 
|**consumerGroup** |**ConsumerGroup** | Een optionele eigenschap die bepaalt de [consumergroep](../event-hubs/event-hubs-features.md#event-consumers) gebruikt om u te abonneren op gebeurtenissen in de hub. Als u dit weglaat, de `$Default` consumergroep wordt gebruikt. | 
|**verbinding** |**Verbinding** | De naam van een app-instelling met de verbindingsreeks naar de event hub-naamruimte. Kopieer deze verbindingsreeks door te klikken op de **verbindingsgegevens** knop voor de *naamruimte*, niet de event hub zelf. Deze verbindingsreeks moet ten minste leesmachtigingen hebben voor de trigger wordt geactiveerd.<br/>Wanneer u lokaal ontwikkelt, app-instellingen gaan in de waarden van de [local.settings.json bestand](functions-run-local.md#local-settings-file).|

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen host.json

De [host.json](functions-host-json.md#eventhub) bestand bevat instellingen voor Event Hubs trigger gedrag.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="event-hubs-output-binding"></a>Event Hubs uitvoer binding

De uitvoer van de Event Hubs binding gebeurtenissen schrijven naar een stroom gebeurtenissen gebruiken. U moet gemachtigd verzenden naar een event hub gebeurtenissen om ernaar te schrijven.

## <a name="output---example"></a>Output - voorbeeld

Zie het voorbeeld taalspecifieke:

* [Vooraf gecompileerde C#](#output---c-example)
* [C#-script](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output - C#-voorbeeld

Het volgende voorbeeld wordt een [vooraf gecompileerd C#-functie](functions-dotnet-class-library.md) die een bericht naar een event hub, met behulp van de geretourneerde waarde van de methode als uitvoer geschreven:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Output - voorbeeld van C#-script

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie schrijft een bericht naar een event hub.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Hier volgt een C# script-code die een bericht worden gemaakt:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Hier volgt C# script-code die wordt gemaakt van meerdere berichten:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Output - F #-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [F # functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie schrijft een bericht naar een event hub.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Dit is de F #-code:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Output - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie schrijft een bericht naar een event hub.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Hier volgt JavaScript-code die door een enkel bericht verzonden:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Hier volgt JavaScript-code waarmee meerdere berichten worden verzonden:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes-for-precompiled-c"></a>Output - kenmerken voor vooraf gecompileerde C#

Voor [vooraf gecompileerd C#](functions-dotnet-class-library.md) functies, gebruiken de [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

De constructor van het kenmerk werkt met de naam van de event hub en de naam van een app-instelling met de verbindingsreeks. Zie voor meer informatie over deze instellingen [Output - configuratie](#output---configuration). Hier volgt een `EventHub` kenmerk voorbeeld:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
```

## <a name="output---configuration"></a>Output - configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `EventHub` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'eventHub'. |
|**richting** | N.v.t. | Moet worden ingesteld op 'out'. Deze parameter wordt automatisch ingesteld bij het maken van de binding in de Azure portal. |
|**naam** | N.v.t. | De naam van de variabele gebruikt in de functiecode waarmee de gebeurtenis. | 
|**pad** |**EventHubName** | De naam van de event hub. | 
|**verbinding** |**Verbinding** | De naam van een app-instelling met de verbindingsreeks naar de event hub-naamruimte. Kopieer deze verbindingsreeks door te klikken op de **verbindingsgegevens** knop voor de *naamruimte*, niet de event hub zelf. Deze verbindingsreeks moet verzenden machtigingen hebben voor het bericht naar de stroom gebeurtenissen sturen.<br/>Wanneer u lokaal ontwikkelt, app-instellingen gaan in de waarden van de [local.settings.json bestand](functions-run-local.md#local-settings-file).|

## <a name="output---usage"></a>Output - gebruik

In C# en C# script, kunt u berichten verzenden met behulp van een methodeparameter zoals `out string paramName`. In C# script `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. U kunt gebruiken voor het schrijven van meerdere berichten `ICollector<string>` of `IAsyncCollector<string>` in plaats van `out string`.

In JavaScript, opent u de uitvoergebeurtenis met behulp van `context.bindings.<name>`. `<name>`de waarde is opgegeven in de `name` eigenschap van *function.json*.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
