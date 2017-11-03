---
title: Azure Functions Service Bus-triggers en bindingen | Microsoft Docs
description: Het gebruik van Azure Service Bus-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: christopheranderson
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
ms.author: glenga
ms.openlocfilehash: 71149aaacc940a62e085cf1ce103a0214d05bd1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure Functions Service Bus-bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe configureren en werken met Azure Service Bus-bindingen in de Azure Functions. 

Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Service Bus-wachtrijen en onderwerpen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Service Bus-trigger
Gebruik de Service Bus-trigger om te reageren op berichten van een Service Bus-wachtrij of onderwerp. 

De Service Bus-wachtrij en onderwerp triggers zijn gedefinieerd door de volgende JSON-objecten in de `bindings` matrix van function.json:

* *wachtrij* trigger:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* *onderwerp* trigger:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Houd rekening met het volgende:

* Voor `connection`, [maken van een app-instelling in uw app functie](functions-how-to-use-azure-function-app-settings.md) die de verbindingsreeks naar uw Service Bus-naamruimte bevat, geeft u de naam van de app-instelling in de `connection` eigenschap in de trigger. Verkrijgen van de verbindingsreeks door de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  De verbindingsreeks moet voor een Service Bus-naamruimte niet beperkt tot een specifieke wachtrij of onderwerp.
  Als u niets `connection` leeg is, wordt de trigger wordt ervan uitgegaan dat er een standaard Service Bus-verbindingsreeks is opgegeven in een app instelling met de naam `AzureWebJobsServiceBus`.
* Voor `accessRights`, beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, wat aangeeft dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` naar `listen`. De runtime mislukken kan voor bewerkingen waarvoor probeert functies beheren anders rechten.

## <a name="trigger-behavior"></a>Gedrag van de trigger
* **Single-threading** - standaard de functies runtime-processen die meerdere tegelijkertijd berichten. Stel rechtstreeks runtime slechts een enkele wachtrij of onderwerp bericht tegelijkertijd wordt verwerkt, `serviceBus.maxConcurrentCalls` op 1 in *host.json*. 
  Voor informatie over *host.json*, Zie [mapstructuur](functions-reference.md#folder-structure) en [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Verwerking van gevaarlijke berichten** -Service Bus biedt een eigen verwerken van verontreinigde berichten die niet kan worden beheerd of geconfigureerd in de configuratie van Azure Functions of code. 
* **PeekLock gedrag** -de runtime van Functions ontvangt een bericht in [ `PeekLock` modus](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) en aanroepen `Complete` op het bericht als de functie wordt voltooid of aanroepen `Abandon` als de functie mislukt. 
  Als de functie wordt uitgevoerd langer dan de `PeekLock` een time-out opgetreden, de vergrendeling wordt automatisch verlengd.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Gebruik van de trigger
Deze sectie laat zien hoe uw Service Bus-trigger te gebruiken in uw functiecode. 

In C# en F #, kan het bericht Service Bus-trigger worden gedeserialiseerd met een van de volgende invoertypen:

* `string`-nuttig voor string-berichten
* `byte[]`-handig voor binaire gegevens
* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) - handig voor gegevens JSON-geserialiseerd.
  Als u een aangepaste invoertype zoals declareren `CustomType`, Azure Functions geprobeerd te deserialiseren van de JSON-gegevens in het opgegeven type.
* `BrokeredMessage`-kunt u het gedeserialiseerde bericht met de [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) methode.

In Node.js, wordt het bericht Service Bus-trigger in de functie als een tekenreeks- of JSON-object doorgegeven.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Voorbeeld van de trigger
Stel dat u hebt de volgende function.json:

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

Zie het voorbeeld taalspecifieke die een Service Bus-wachtrijbericht verwerkt.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Voorbeeld van de trigger in C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Voorbeeld van de trigger in F # #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Voorbeeld van de trigger in Node.js

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Service Bus uitvoer binding
De Service Bus-wachtrij en onderwerp uitvoer voor een functie gebruiken de volgende JSON-objecten in de `bindings` matrix van function.json:

* *wachtrij* uitvoer:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* *onderwerp* uitvoer:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Houd rekening met het volgende:

* Voor `connection`, [maken van een app-instelling in uw app functie](functions-how-to-use-azure-function-app-settings.md) die de verbindingsreeks naar uw Service Bus-naamruimte bevat, geeft u de naam van de app-instelling in de `connection` eigenschap in de uitvoer-binding. Verkrijgen van de verbindingsreeks door de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  De verbindingsreeks moet voor een Service Bus-naamruimte niet beperkt tot een specifieke wachtrij of onderwerp.
  Als u niets `connection` leeg is, wordt de uitvoer-binding wordt ervan uitgegaan dat er een standaard Service Bus-verbindingsreeks is opgegeven in een app instelling met de naam `AzureWebJobsServiceBus`.
* Voor `accessRights`, beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, wat aangeeft dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` naar `listen`. De runtime mislukken kan voor bewerkingen waarvoor probeert functies beheren anders rechten.

<a name="outputusage"></a>

## <a name="output-usage"></a>Gebruik voor uitvoer
In C# en F # kunt Azure Functions een Service Bus-wachtrijbericht maken van elk van de volgende typen:

* Alle [Object](https://msdn.microsoft.com/library/system.object.aspx) -parameterdefinitie ziet eruit als `out T paramName` (C#).
  Functies deserializes het object in een JSON-bericht. Als de uitvoerwaarde is null wanneer de functie wordt afgesloten, wordt het bericht in functies gemaakt met een null-object.
* `string`-Parameterdefinitie ziet eruit als `out string paraName` (C#). Als de parameterwaarde is niet-null wanneer de functie wordt afgesloten, wordt een bericht door functies gemaakt.
* `byte[]`-Parameterdefinitie ziet eruit als `out byte[] paraName` (C#). Als de parameterwaarde is niet-null wanneer de functie wordt afgesloten, wordt een bericht door functies gemaakt.
* `BrokeredMessage`Parameterdefinitie ziet eruit als `out BrokeredMessage paraName` (C#). Als de parameterwaarde is niet-null wanneer de functie wordt afgesloten, wordt een bericht door functies gemaakt.

U kunt gebruiken voor het maken van meerdere berichten in een C#-functie, `ICollector<T>` of `IAsyncCollector<T>`. Een bericht wordt gemaakt bij het aanroepen van de `Add` methode.

U kunt een tekenreeks, een bytematrix of een Javascript-object (gedeserialiseerd naar JSON) in Node.js, toewijzen aan `context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Voorbeeld van uitvoer
Stel dat u hebt de volgende function.json die een Service Bus-wachtrij-uitvoer definieert:

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

Zie de taalspecifieke-voorbeeldtoepassing die u een bericht naar de service bus-wachtrij verzendt.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Voorbeeld van uitvoer in C# #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Of om meerdere berichten te maken:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Voorbeeld van uitvoer in F # #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Voorbeeld van uitvoer in Node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Of om meerdere berichten te maken:

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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

