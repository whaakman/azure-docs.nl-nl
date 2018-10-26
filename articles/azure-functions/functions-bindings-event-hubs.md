---
title: Azure Event Hubs-bindingen voor Azure Functions
description: Over het gebruik van Azure Event Hubs-bindingen in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/08/2017
ms.author: glenga
ms.openlocfilehash: 401517abf75c5144a2e166663408be52934c92e3
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086825"
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

Elk exemplaar van een event hub-geactiveerde functie wordt ondersteund door slechts één [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) exemplaar. Eventhubs zorgt ervoor dat er slechts één [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) exemplaar krijgt een lease op een bepaalde partitie.

Neem bijvoorbeeld een Event Hub als volgt:

* 10-partities.
* 1000 gebeurtenissen gelijkmatig verdeeld over alle partities, met 100 berichten in elke partitie.

Wanneer uw functie is ingeschakeld, is er slechts één exemplaar van de functie. Noemen we dit exemplaar van de functie `Function_0`. `Function_0` heeft één [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) -doelexemplaar waarvoor een lease op alle tien partities. Dit exemplaar lezen van gebeurtenissen van partities 0-9. Vanaf dit punt gebeurt het volgende:

* **Nieuwe instanties van de functie niet nodig zijn**: `Function_0` kan alle 1000 gebeurtenissen voordat de logica voor vergroten/verkleinen begint in Functions te verwerken. In dit geval alle 1000 berichten worden verwerkt door `Function_0`.

* **Een extra functie-exemplaar wordt toegevoegd**: de functies schalen logische bepaalt dat `Function_0` heeft meer berichten dan deze kan verwerken. In dit geval een nieuwe functie app-exemplaar (`Function_1`) is gemaakt, samen met een nieuwe [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) exemplaar. Eventhubs detecteert dat een nieuwe instantie van de host wordt geprobeerd berichten lezen. De partities in Event Hubs-verdeelt het de hostexemplaren. Bijvoorbeeld, 0-4-partities kunnen worden toegewezen aan `Function_0` en 5-9 voor partities `Function_1`. 

* **N meer functie-exemplaren worden toegevoegd**: de functies schalen logische bepaalt dat beide `Function_0` en `Function_1` er meer berichten dan ze kunnen verwerken. Nieuwe functie-app-instanties `Function_2`... `Functions_N` zijn gemaakt, waarbij `N` groter is dan het aantal event hub-partities. In ons voorbeeld nogmaals Event Hubs verdeelt de belasting van de partities in dit geval over de exemplaren `Function_0`... `Functions_9`. 

Houd er rekening mee dat wanneer functies kan worden geschaald naar `N` instanties, wat een getal groter dan het aantal event hub-partities. Dit wordt gedaan om ervoor te zorgen dat er altijd zijn [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) exemplaren beschikbaar zijn voor het verkrijgen van vergrendelingen op partities zodra deze beschikbaar zijn van andere exemplaren. U betaalt alleen voor de resources die worden gebruikt wanneer het exemplaar van de functie wordt uitgevoerd; u betaalt geen voor deze capaciteit in te richten.

Wanneer alle functie-uitvoering is voltooid (met of zonder fouten), worden controlepunten toegevoegd aan het bijbehorende opslagaccount. Als gecontroleerd is geslaagd, worden alle berichten die 1000 nooit opnieuw opgehaald.

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die de hoofdtekst van de event hub-trigger-Logboeken.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Toegang krijgen tot [gebeurtenis metagegevens](#trigger---event-metadata) in functiecode aan te geven, verbinding maken met een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (moet een instructie voor `Microsoft.Azure.EventHubs`). U kunt ook toegang tot dezelfde eigenschappen met behulp van de binding-expressies in de handtekening van de methode.  Het volgende voorbeeld ziet u beide manieren om dezelfde gegevens:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, 
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    ILogger log)
{
    log.LogInformation($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    // Metadata accessed by binding to EventData
    log.LogInformation($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.LogInformation($"Offset={myEventHubMessage.SystemProperties.Offset}");
    // Metadata accessed by using binding expressions in method parameters
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={sequenceNumber}");
    log.LogInformation($"Offset={offset}");
}
```

Zorg voor het ontvangen van gebeurtenissen in een batch, `string` of `EventData` een matrix.  

> [!NOTE]
> Tijdens het ontvangen in een batch met u kan geen binding met de methodeparameters, zoals in het bovenstaande voorbeeld met `DateTime enqueuedTimeUtc` en moet deze uit elke ontvangen `EventData` object  

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData[] eventHubMessages, ILogger log)
{
    foreach (var message in eventHubMessages)
    {
        log.LogInformation($"C# Event Hub trigger function processed a message: {Encoding.UTF8.GetString(message.Body)}");
        log.LogInformation($"EnqueuedTimeUtc={message.SystemProperties.EnqueuedTimeUtc}");
    }
}
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie registreert de hoofdtekst van de event hub-trigger.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
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

Toegang krijgen tot [gebeurtenis metagegevens](#trigger---event-metadata) in functiecode aan te geven, verbinding maken met een [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) object (moet een instructie voor `Microsoft.Azure.EventHubs`). U kunt ook toegang tot dezelfde eigenschappen met behulp van de binding-expressies in de handtekening van de methode.  Het volgende voorbeeld ziet u beide manieren om dezelfde gegevens:

```cs
#r "Microsoft.Azure.EventHubs"

using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;
using Microsoft.Azure.EventHubs;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.SystemProperties.Offset}");

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

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Dit is de F #-code:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Log(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie leest [gebeurtenis metagegevens](#trigger---event-metadata) en registreert het bericht.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
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

Voor het ontvangen van gebeurtenissen in een batch stelt `cardinality` naar `many` in de *function.json* -bestand, zoals wordt weergegeven in de volgende voorbeelden. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 

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

Dit is de JavaScript-code:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);
    
    eventHubMessages.forEach((message, index) => {
        context.log(`Processed message ${message}`);
        context.log(`EnqueuedTimeUtc = ${context.bindingData.enqueuedTimeUtcArray[index]}`);
        context.log(`SequenceNumber = ${context.bindingData.sequenceNumberArray[index]}`);
        context.log(`Offset = ${context.bindingData.offsetArray[index]}`);
    });

    context.done();
};
```

### <a name="trigger---java-example"></a>Trigger - Java-voorbeeld

Het volgende voorbeeld ziet u een Event Hub-trigger binding in een *function.json* bestand en een [Java functie](functions-reference-java.md) die gebruikmaakt van de binding. De functie registreert de berichttekst van de Event Hub-trigger.

```json
{
  "type": "eventHubTrigger",
  "name": "msg",
  "direction": "in",
  "eventHubName": "myeventhubname",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

```java
@FunctionName("ehprocessor")
public void eventHubProcessor(
  @EventHubTrigger(name = "msg",
                  eventHubName = "myeventhubname",
                  connection = "myconnvarname") String message,
       final ExecutionContext context ) 
       {
          context.getLogger().info(message);
 }
 ```

 In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `EventHubTrigger` aantekening op parameters waarvan de waarde afkomstig van de Event Hub zijn kan. Parameters met deze aantekeningen ertoe leiden dat de functie moet worden uitgevoerd wanneer een gebeurtenis wordt ontvangen.  Deze aantekening kan worden gebruikt met systeemeigen Java-typen, pojo's of null-waarden met behulp van optioneel<T>. 

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de naam van de event hub, de naam van de consumentengroep en de naam van een app-instelling met de verbindingsreeks. Zie voor meer informatie over deze instellingen, de [activeren configuratiesectie](#trigger---configuration). Hier volgt een `EventHubTriggerAttribute` kenmerk voorbeeld:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
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
|**Pad** |**EventHubName** | 1.x alleen functies. De naam van de event hub. Wanneer de naam van de event hub ook aanwezig in de connection string is, overschrijft deze waarde deze eigenschap tijdens runtime. | 
|**eventHubName** |**EventHubName** | 2.x alleen functies. De naam van de event hub. Wanneer de naam van de event hub ook aanwezig in de connection string is, overschrijft deze waarde deze eigenschap tijdens runtime. |
|**consumerGroup** |**consumerGroup** | Een optionele eigenschap die Hiermee stelt u de [consumergroep](../event-hubs/event-hubs-features.md#event-consumers) gebruikt om u te abonneren op gebeurtenissen in de hub. Als u dit weglaat, de `$Default` consumergroep wordt gebruikt. | 
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
* [Java](#output---java-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die een bericht naar een event hub, met behulp van de geretourneerde waarde van de methode als de uitvoer wordt geschreven:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie een bericht wordt geschreven naar een event hub.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Hier volgt een C#-script-code die een bericht wordt gemaakt:

```cs
using System;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, ILogger log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.LogInformation(msg);   
    outputEventHubMessage = msg;
}
```

Hier volgen enkele redenen C#-script-code die wordt gemaakt van meerdere berichten:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, ILogger log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.LogInformation(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Uitvoer - F #-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [F #-functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie een bericht wordt geschreven naar een event hub.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Dit is de F #-code:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: ILogger) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.LogInformation(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een event hub-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie een bericht wordt geschreven naar een event hub.

De volgende voorbeelden ziet u gegevens van Event Hubs-binding in de *function.json* bestand. Het eerste voorbeeld is voor functies 2.x, en de tweede waarde is voor functies 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
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

### <a name="output---java-example"></a>Uitvoer - Java-voorbeeld

Het volgende voorbeeld ziet een Java-functie die de huidige tijd van een bericht vastgezet naar een Event Hub schrijft.

```java
@}FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 *&#47;5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
 ```

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruiken de `@EventHubOutput` aantekening op parameters waarvan de waarde poublished naar Event Hub is.  De parameter moet van het type `OutputBinding<T>` , waarbij T een POJO of een systeemeigen Java-type. 

## <a name="output---attributes"></a>Uitvoer - kenmerken

Voor [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) kenmerk.

De constructor van het kenmerk wordt de naam van de event hub en de naam van een app-instelling met de verbindingsreeks. Zie voor meer informatie over deze instellingen [uitvoer - configuratie](#output---configuration). Hier volgt een `EventHub` kenmerk voorbeeld:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
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
|**Pad** |**EventHubName** | 1.x alleen functies. De naam van de event hub. Wanneer de naam van de event hub ook aanwezig in de connection string is, overschrijft deze waarde deze eigenschap tijdens runtime. | 
|**eventHubName** |**EventHubName** | 2.x alleen functies. De naam van de event hub. Wanneer de naam van de event hub ook aanwezig in de connection string is, overschrijft deze waarde deze eigenschap tijdens runtime. |
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
