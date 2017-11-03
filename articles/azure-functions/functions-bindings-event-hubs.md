---
title: Azure Functions Event Hubs-bindingen | Microsoft Docs
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
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: 85eb6985ef3579b1b2313db3ce5f91c3471da72f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Azure Functions Event Hubs-bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en gebruikt [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindingen voor Azure Functions.
Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Als u niet bekend met Azure Event Hubs bent, raadpleegt u de [overzicht van Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Event hub trigger
Gebruik de Event Hubs-trigger om te reageren op een gebeurtenis verzonden naar de gebeurtenisstroom van een event hub. U moet leestoegang hebben tot de event hub voor het instellen van de trigger.

De trigger Event Hubs-functie maakt gebruik van de volgende JSON-object in de `bindings` matrix van function.json:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup`is een optionele eigenschap gebruikt om de [consumergroep](../event-hubs/event-hubs-features.md#event-consumers) gebruikt om u te abonneren op gebeurtenissen in de hub. Als u dit weglaat, de `$Default` consumergroep wordt gebruikt.  
`connection`moet de naam van een app-instelling met de verbindingsreeks naar de event hub-naamruimte.
Kopieer deze verbindingsreeks door te klikken op de **verbindingsgegevens** knop voor de *naamruimte*, niet de event hub zelf. Deze verbindingsreeks moet ten minste leesmachtigingen hebben voor de trigger wordt geactiveerd.

[Extra instellingen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) kan worden opgegeven in een bestand host.json verder verbeteren van de Event Hubs-triggers.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Gebruik van de trigger
Wanneer een functie van Event Hubs trigger wordt geactiveerd, wordt het bericht dat deze wordt geactiveerd als een tekenreeks in de functie doorgegeven.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Voorbeeld van de trigger
Stel dat u hebt de volgende Event Hubs activeren de `bindings` matrix van function.json:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Zie de taalspecifieke-voorbeeldtoepassing die u de berichttekst van de event hub-trigger registreert.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Voorbeeld van de trigger in C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

U kunt ook ontvangen de gebeurtenis als een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object, waarmee u toegang krijgt tot de metagegevens van de gebeurtenis.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Voor het ontvangen van gebeurtenissen in een batch, wijzigt de methodehandtekening voor `string[]` of `EventData[]`.

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Voorbeeld van de trigger in F # #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Voorbeeld van de trigger in Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Event Hubs uitvoer binding
De uitvoer van de Event Hubs binding gebeurtenissen schrijven naar de gebeurtenisstroom van een event hub gebruiken. U moet gemachtigd verzenden naar een event hub gebeurtenissen om ernaar te schrijven.

De uitvoer-binding gebruikt de volgende JSON-object in de `bindings` matrix van function.json:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`moet de naam van een app-instelling met de verbindingsreeks naar de event hub-naamruimte.
Kopieer deze verbindingsreeks door te klikken op de **verbindingsgegevens** knop voor de *naamruimte*, niet de event hub zelf. Deze verbindingsreeks moet verzenden machtigingen hebben voor het bericht naar de stroom gebeurtenissen sturen.

## <a name="output-usage"></a>Gebruik voor uitvoer
Deze sectie wordt beschreven hoe u de uitvoer van uw Event Hubs in uw functiecode binding.

U kunt berichten naar de geconfigureerde event hub uitvoeren met de volgende parametertypen:

* `out string`
* `ICollector<string>`(om uit te voeren op meerdere berichten)
* `IAsyncCollector<string>`(async-versie van `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Voorbeeld van uitvoer
Stel dat u hebt de volgende Event Hubs uitvoer binding in de `bindings` matrix van function.json:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Zie het voorbeeld taalspecifieke die een gebeurtenis naar de zelfs stroom geschreven.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Voorbeeld van uitvoer in C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Of om meerdere berichten te maken:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Voorbeeld van uitvoer in F # #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Voorbeeld van uitvoer voor Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Of om meerdere berichten te verzenden

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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
