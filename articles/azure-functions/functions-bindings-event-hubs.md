---
title: Azure Event Hubs-bindingen voor Azure Functions
description: Over het gebruik van Azure Event Hubs-bindingen in Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
ms.openlocfilehash: 51f64f6f74875c6afac350dc9cc235573b89c524
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989585"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure Event Hubs-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bindingen voor Azure Functions. Azure Functions ondersteunt activeren en uitvoerbindingen voor Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

Voor Azure Functions-versie 1.x, de Event Hubs-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet-pakket versie 2.x.
Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub-opslagplaats.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

Voor functies 2.x gebruikt, gebruik de [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) pakket, versie 3.x.
Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

De trigger van Event Hubs gebruiken om te reageren op een gebeurtenis verzonden naar een event hub-gebeurtenisstroom. U moet leestoegang hebben tot de event hub voor het instellen van de trigger.

Wanneer een functie van Event Hubs-trigger wordt geactiveerd, wordt het bericht dat deze wordt geactiveerd als een tekenreeks in de functie doorgegeven.

## <a name="trigger---scaling"></a>Trigger - schaling

Elk exemplaar van een Event Hub-Triggered-functie wordt ondersteund door slechts 1 exemplaar van EventProcessorHost (EPH). Eventhubs zorgt ervoor dat slechts 1 EPH een lease op een bepaalde partitie kunt krijgen.

Stel bijvoorbeeld dat we beginnen met de volgende instellingen en veronderstellingen voor een Event Hub:

1. 10-partities.
1. 1000 gebeurtenissen gelijkmatig verdeeld over alle partities = > 100 berichten in elke partitie.

Wanneer uw functie is ingeschakeld, is er slechts 1 exemplaar van de functie. Noemen we dit exemplaar van de functie Function_0. Function_0 hebben 1 EPH die worden beheerd om op te halen van een lease op alle 10 partities. Start het lezen van gebeurtenissen van partities 0-9. Vanaf dit punt, een van de volgende gebeurt:

* **Functie slechts 1 exemplaar is nodig** -Function_0 kan voor het verwerken van alle 1000 voordat vergroten/verkleinen logica van de Azure Functions begint. Daarom kan worden alle 1000 berichten verwerkt door Function_0.

* **1 meer functie-exemplaar toevoegen** -Azure-Functions vergroten/verkleinen logische bepaalt dat Function_0 meer berichten dan deze verwerken, heeft kan zodat een nieuw exemplaar, Function_1, wordt gemaakt. Eventhubs detecteert dat een nieuw exemplaar van de EPH probeert berichten lezen. Eventhubs de partities te verdelen over de EPH-exemplaren wordt gestart, bijvoorbeeld 0-4-partities zijn toegewezen aan Function_0 en partities 5-9 zijn toegewezen aan Function_1. 

* **Voeg N functie meer exemplaren** -vergroten/verkleinen logische Azure-Functions bepaalt dat zowel Function_0 en Function_1 er meer berichten dan ze kunnen verwerken. Er wordt opnieuw geschaald voor Function_2... N, waarbij N groter dan de Event Hub-partities is. Eventhubs wordt geladen worden verdeeld de partities Function_0... 9-exemplaren.

Uniek is voor de huidige Azure-Functions schalen logica is het feit dat N groter dan het aantal partities is. Dit wordt gedaan om ervoor te zorgen dat er altijd exemplaren van EPH direct beschikbaar zodra deze beschikbaar zijn van andere exemplaren om snel een vergrendeling ophalen voor de partitie (s). Gebruikers betalen alleen voor de resources die worden gebruikt wanneer het exemplaar van de functie wordt uitgevoerd, en worden niet in rekening gebracht voor deze capaciteit in te richten.

Als alle uitvoeringen van functie voltooid zonder fouten, worden controlepunten toegevoegd aan het bijbehorende opslagaccount. Als gecontroleerd is geslaagd, moeten alle 1000 berichten nooit worden opgehaald.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die de hoofdtekst van de event hub-trigger-Logboeken.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Toegang krijgen tot [gebeurtenis metagegevens](#trigger---event-metadata) in functiecode aan te geven, verbinding maken met een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (moet een instructie voor `Microsoft.ServiceBus.Messaging`). U kunt ook toegang tot dezelfde eigenschappen met behulp van de binding-expressies in de handtekening van de methode.  Het volgende voorbeeld ziet u beide manieren om dezelfde gegevens:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, 
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Zorg voor het ontvangen van gebeurtenissen in een batch, `string` of `EventData` een matrix:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie registreert de hoofdtekst van de event hub-trigger.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
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

Toegang krijgen tot [gebeurtenis metagegevens](#trigger---event-metadata) in functiecode aan te geven, verbinding maken met een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (moet een instructie voor `Microsoft.ServiceBus.Messaging`). U kunt ook toegang tot dezelfde eigenschappen met behulp van de binding-expressies in de handtekening van de methode.  Het volgende voorbeeld ziet u beide manieren om dezelfde gegevens:

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
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

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [F #-functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie registreert de hoofdtekst van de event hub-trigger.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Dit is de F #-code:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie leest [gebeurtenis metagegevens](#trigger---event-metadata) en registreert het bericht.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, eventHubMessage) {
    context.log('Event Hubs trigger function processed message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);
     
    context.done();
};
```

Voor het ontvangen van gebeurtenissen in een batch stelt `cardinality` naar `many` in de *function.json* -bestand, zoals wordt weergegeven in de volgende voorbeelden. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);
    
    eventHubMessages.forEach(message => {
        context.log(`Processed message ${message}`);
    });

    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de naam van de event hub, de naam van de consumentengroep en de naam van een app-instelling met de verbindingsreeks. Zie voor meer informatie over deze instellingen, de [activeren configuratiesectie](#trigger---configuration). Hier volgt een `EventHubTriggerAttribute` kenmerk voorbeeld:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `EventHubTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op `eventHubTrigger`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op `in`. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de gebeurtenis in de functiecode aan te geven. | 
|**Pad** |**EventHubName** | 1.x alleen functies. De naam van de event hub.  | 
|**eventHubName** |**EventHubName** | 2.x alleen functies. De naam van de event hub.  |
|**consumerGroup** |**ConsumerGroup** | Een optionele eigenschap die Hiermee stelt u de [consumergroep](../event-hubs/event-hubs-features.md#event-consumers) gebruikt om u te abonneren op gebeurtenissen in de hub. Als u dit weglaat, de `$Default` consumergroep wordt gebruikt. | 
|**de kardinaliteit** | N.v.t. | Voor Javascript. Ingesteld op `many` om in te schakelen via batchverwerking uitvoeren.  Als weggelaten of ingesteld op `one`, één bericht dat wordt doorgegeven aan functie. | 
|**verbinding** |**verbinding** | De naam van een app-instelling met de verbindingsreeks voor de event hub-naamruimte. Kopieer deze verbindingsreeks door te klikken op de **verbindingsgegevens** knop voor de [naamruimte](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), niet de event hub zelf. Deze verbindingsreeks moet ten minste leesmachtigingen heeft voor de trigger wordt geactiveerd.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Trigger - event-metagegevens

De trigger van Event Hubs biedt verschillende [metagegevenseigenschappen](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies in andere bindingen voor gegevensbinding of als parameters in uw code. Dit zijn de eigenschappen van de [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata) klasse.

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|De `PartitionContext` exemplaar.|
|`EnqueuedTimeUtc`|`DateTime`|De in de wachtrij-tijd in UTC.|
|`Offset`|`string`|De offset van de gegevens ten opzichte van de Event Hub-partitie-stream. De offset is een markering of id voor een gebeurtenis in de Event Hubs-stream. De id is uniek zijn binnen een partitie van de stroom van de Event Hubs.|
|`PartitionKey`|`string`|De partitie waarnaar de gebeurtenis die gegevens moeten worden verzonden.|
|`Properties`|`IDictionary<String,Object>`|De de gebruikerseigenschappen van de gegevens van de gebeurtenis.|
|`SequenceNumber`|`Int64`|Het aantal logische volgorde van de gebeurtenis.|
|`SystemProperties`|`IDictionary<String,Object>`|De eigenschappen, met inbegrip van gegevens van de gebeurtenis.|

Zie [codevoorbeelden](#trigger---example) die eerder in dit artikel gebruikmaken van deze eigenschappen.

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen voor host.json

De [host.json](functions-host-json.md#eventhub) bestand bevat instellingen die het gedrag van Event Hubs-trigger beheren.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Uitvoer

Gebruik de Event Hubs-Uitvoerbinding gebeurtenissen schrijven naar een gebeurtenisstroom. U moet gemachtigd verzenden naar een event hub gebeurtenissen schrijven naar het.

Zorg ervoor dat de referenties vereist pakket wordt voldaan: [functies 1.x](#packages---functions-1.x) of [2.x-functies](#packages---functions-2.x) 

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een bericht naar een event hub, met behulp van de geretourneerde waarde van de methode als de uitvoer wordt geschreven:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie een bericht wordt geschreven naar een event hub.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Hier volgt een C#-script-code die een bericht wordt gemaakt:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Hier volgen enkele redenen C#-script-code die wordt gemaakt van meerdere berichten:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Uitvoer - F #-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [F #-functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie een bericht wordt geschreven naar een event hub.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
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

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie een bericht wordt geschreven naar een event hub.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 1.x en de tweede waarde is voor functies 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Hier volgen enkele redenen JavaScript-code dat een enkel bericht verzendt:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Hier volgen enkele redenen JavaScript-code waarmee meerdere berichten worden verzonden:

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

## <a name="output---attributes"></a>Uitvoer - kenmerken

Voor [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de naam van de event hub en de naam van een app-instelling met de verbindingsreeks. Zie voor meer informatie over deze instellingen [uitvoer - configuratie](#output---configuration). Hier volgt een `EventHub` kenmerk voorbeeld:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - voorbeeld met C#](#output---c-example).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `EventHub` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'eventHub'. |
|**direction** | N.v.t. | Moet worden ingesteld op 'out'. Deze parameter wordt automatisch ingesteld wanneer u de binding in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die wordt gebruikt in de functiecode waarmee de gebeurtenis. | 
|**Pad** |**EventHubName** | 1.x alleen functies. De naam van de event hub.  | 
|**eventHubName** |**EventHubName** | 2.x alleen functies. De naam van de event hub.  |
|**verbinding** |**verbinding** | De naam van een app-instelling met de verbindingsreeks voor de event hub-naamruimte. Kopieer deze verbindingsreeks door te klikken op de **verbindingsgegevens** knop voor de *naamruimte*, niet de event hub zelf. Deze verbindingsreeks moet machtigingen voor verzenden zijn het bericht te verzenden naar de gebeurtenisstroom.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uitvoer - gebruik

In C# en C#-script, kunt u berichten verzenden met behulp van een methodeparameter zoals `out string paramName`. In C#-script, `paramName` is de waarde is opgegeven in de `name` eigenschap van *function.json*. Voor het schrijven van meerdere berichten, kunt u `ICollector<string>` of `IAsyncCollector<string>` in plaats van `out string`.

In JavaScript, toegang krijgen tot de uitvoergebeurtenis met behulp van `context.bindings.<name>`. `<name>` de waarde is opgegeven in de `name` eigenschap van *function.json*.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| Event Hub | [Operations Guide](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
