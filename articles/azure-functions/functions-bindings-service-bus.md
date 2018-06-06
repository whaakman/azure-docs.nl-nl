---
title: Azure Service Bus-bindingen voor Azure Functions
description: Het gebruik van Azure Service Bus-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: tdykstra
ms.openlocfilehash: de52078681a830d76c1b419c39972c122f35b40e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724374"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Service Bus-bindingen voor Azure Functions

Dit artikel wordt uitgelegd hoe u werkt met Azure Service Bus-bindingen in de Azure Functions. Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Service Bus-wachtrijen en onderwerpen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Service Bus-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet-pakket versie 2.x. De broncode voor het pakket bevindt zich in de [sdk van azure webjobs](https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Service Bus-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet-pakket versie 3.x. De broncode voor het pakket bevindt zich in de [sdk van azure webjobs](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

Gebruik de Service Bus-trigger om te reageren op berichten van een Service Bus-wachtrij of onderwerp. 

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Trigger - C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die leest [bericht metagegevens](#trigger---message-metadata) en registreert een Service Bus-wachtrijbericht:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

In dit voorbeeld is voor Azure Functions versie 1.x; voor 2.x [laat de parameter van de rechten toegang](#trigger---configuration).
 
### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie leest [bericht metagegevens](#trigger---message-metadata) en registreert het bericht van een Service Bus-wachtrij.

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
using System;

public static void Run(string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
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

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie leest [bericht metagegevens](#trigger---message-metadata) en registreert het bericht van een Service Bus-wachtrij. 

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
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken voor het configureren van een Service Bus-trigger:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusTriggerAttribute.cs)

  De constructor van het kenmerk wordt de naam van de wachtrij of het onderwerp en een abonnement. In Azure Functions versie 1.x, kunt u ook rechten van de verbinding opgeven. Als u geen toegangsrechten opgeeft, wordt standaard `Manage`. Zie voor meer informatie de [Trigger - configuratie](#trigger---configuration) sectie.

  Hier volgt een voorbeeld waarin het kenmerk met een tekenreeksparameter gebruikt:

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

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)

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
|**direction** | N.v.t. | Moet worden ingesteld op 'in'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij of onderwerp bericht in de functiecode. Ingesteld op '$return' om te verwijzen naar de retourwaarde van de functie. | 
|**queueName**|**Wachtrijnaam**|De naam van de wachtrij om te controleren.  Alleen ingesteld als een wachtrij, niet voor een onderwerp bewaking.
|**TopicName**|**TopicName**|De naam van het onderwerp om te controleren. Alleen ingesteld als een onderwerp, voor een wachtrij niet controleren.|
|**SubscriptionName**|**SubscriptionName**|Naam van het abonnement om te controleren. Alleen ingesteld als een onderwerp, voor een wachtrij niet controleren.|
|**Verbinding**|**Verbinding**|De naam van een app-instelling met de Service Bus-verbindingsreeks moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam opgeven. Als u bijvoorbeeld `connection` naar 'MyServiceBus', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyServiceBus'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard Service Bus-verbindingsreeks in de app-instelling met de naam 'AzureWebJobsServiceBus'.<br><br>Als u een verbindingsreeks, volgt u de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). De verbindingsreeks moet voor een Service Bus-naamruimte niet beperkt tot een specifieke wachtrij of onderwerp. |
|**accessRights**|**Toegang**|Toegangsrechten voor de verbindingsreeks. Beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, wat aangeeft dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` ' luisteren '. De runtime mislukken kan voor bewerkingen waarvoor probeert functies beheren anders rechten. In Azure Functions versie 2.x deze eigenschap is niet beschikbaar omdat de nieuwste versie van de opslag-SDK biedt geen ondersteuning voor bewerkingen beheren.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

In C# en C# script, kunt u de volgende parametertypen voor de wachtrij of onderwerp bericht:

* `string` -Als het bericht tekst is.
* `byte[]` -Handig voor binaire gegevens.
* Een aangepast type - als het bericht JSON bevat, Azure Functions geprobeerd te deserialiseren van de JSON-gegevens.
* `BrokeredMessage` -Kunt u het gedeserialiseerde bericht met de [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) methode.

Deze parameters zijn voor Azure Functions versie 1.x; Gebruik voor 2.x [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) in plaats van `BrokeredMessage`.

In JavaScript, opent u het bericht wachtrij of onderwerp via `context.bindings.<name from function.json>`. Het Service Bus-bericht wordt doorgegeven aan de functie als een tekenreeks- of JSON-object.

## <a name="trigger---poison-messages"></a>Trigger - verontreinigde berichten

Verwerken van verontreinigde berichten kan niet worden beheerd of geconfigureerd in Azure Functions. Service Bus verwerkt verontreinigde berichten zelf.

## <a name="trigger---peeklock-behavior"></a>Trigger - PeekLock gedrag

De runtime van Functions ontvangt een bericht in [PeekLock modus](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Roept `Complete` op het bericht als de functie wordt voltooid of aanroepen `Abandon` als de functie is mislukt. Als de functie wordt uitgevoerd langer dan de `PeekLock` een time-out opgetreden, de vergrendeling wordt automatisch verlengd, zolang de functie wordt uitgevoerd. 

Functies 1.x kunt u configureren `autoRenewTimeout` in *host.json*, die wordt toegewezen aan [OnMessageOptions.AutoRenewTimeout](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout?view=azure-dotnet#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout). Het toegestane maximum voor deze instelling is 5 minuten volgens de documentatie van de Service Bus, terwijl u de tijdslimiet functies van de standaardwaarde van 5 minuten tot 10 minuten kunt verhogen. Voor Service Bus-functies u wilt niet dat vervolgens doen omdat u de limiet voor het vernieuwen van Service Bus zou overschrijden.

## <a name="trigger---message-metadata"></a>Trigger - bericht metagegevens

De Service Bus-trigger bevat diverse [eigenschappen voor metagegevens](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies voor gegevensbinding in andere bindingen of als parameters in uw code. Dit zijn de eigenschappen van de [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) klasse.

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Het aantal leveringen.|
|`DeadLetterSource`|`string`|De bron voor onbestelbare berichten.|
|`ExpiresAtUtc`|`DateTime`|De verlooptijd in UTC.|
|`EnqueuedTimeUtc`|`DateTime`|De tijd in de wachtrij in UTC.|
|`MessageId`|`string`|Een door de gebruiker gedefinieerde waarde die Service Bus gebruiken kunt voor het identificeren van dubbele berichten als ingeschakeld.|
|`ContentType`|`string`|Een inhoudstype-id door de afzender en ontvanger voor specifieke toepassingslogica gebruikt.|
|`ReplyTo`|`string`|Het antwoord naar de wachtrijadres.|
|`SequenceNumber`|`Int64`|Het unieke nummer toegewezen aan een bericht met de Service Bus.|
|`To`|`string`|De verzenden naar het adres.|
|`Label`|`string`|Het specifieke label van de toepassing.|
|`CorrelationId`|`string`|De correlatie-ID.|
|`Properties`|`IDictionary<String,Object>`|De toepassing specifieke bericht-eigenschappen.|

Zie [codevoorbeelden](#trigger---example) die gebruikmaken van deze eigenschappen eerder in dit artikel.

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
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Hier volgt een JavaScript-scriptcode die meerdere berichten worden gemaakt:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = [];
    context.bindings.outputSbQueue.push("1 " + message);
    context.bindings.outputSbQueue.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Output - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs).

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
|**direction** | N.v.t. | Moet worden ingesteld op 'out'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij of onderwerp in de functiecode. Ingesteld op '$return' om te verwijzen naar de retourwaarde van de functie. | 
|**queueName**|**Wachtrijnaam**|De naam van de wachtrij.  Alleen ingesteld als Wachtrijberichten, niet voor een onderwerp verzenden.
|**TopicName**|**TopicName**|De naam van het onderwerp om te controleren. Alleen ingesteld als onderwerp berichten, niet voor een wachtrij te verzenden.|
|**Verbinding**|**Verbinding**|De naam van een app-instelling met de Service Bus-verbindingsreeks moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam opgeven. Als u bijvoorbeeld `connection` naar 'MyServiceBus', lijkt de runtime van Functions voor een app die is met de naam 'AzureWebJobsMyServiceBus'. Als u niets `connection` leeg is, wordt de runtime van Functions maakt gebruik van de standaard Service Bus-verbindingsreeks in de app-instelling met de naam 'AzureWebJobsServiceBus'.<br><br>Als u een verbindingsreeks, volgt u de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). De verbindingsreeks moet voor een Service Bus-naamruimte niet beperkt tot een specifieke wachtrij of onderwerp.|
|**accessRights**|**Toegang**|Toegangsrechten voor de verbindingsreeks. Beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, wat aangeeft dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` ' luisteren '. De runtime mislukken kan voor bewerkingen waarvoor probeert functies beheren anders rechten. In Azure Functions versie 2.x deze eigenschap is niet beschikbaar omdat de nieuwste versie van de opslag-SDK biedt geen ondersteuning voor bewerkingen beheren.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Output - gebruik

In de Azure Functions 1.x, de runtime maakt de wachtrij als deze nog niet bestaat en u hebt ingesteld `accessRights` naar `manage`. In functies versie moet 2.x, wachtrij of onderwerp al bestaan; Als u een wachtrij of onderwerp dat niet bestaat opgeeft, mislukt de functie. 

In C# en C# script, kunt u de volgende parametertypen voor de binding uitvoer:

* `out T paramName` - `T` JSON-serialiseerbaar type kan zijn. Als de waarde van parameter null is wanneer de functie wordt afgesloten, wordt het bericht in functies gemaakt met een null-object.
* `out string` -Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `out byte[]` -Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `out BrokeredMessage` -Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `ICollector<T>` of `IAsyncCollector<T>` : voor het maken van meerdere berichten. Een bericht wordt gemaakt bij het aanroepen van de `Add` methode.

Gebruik in async-functies, de retourwaarde of `IAsyncCollector` in plaats van een `out` parameter.

Deze parameters zijn voor Azure Functions versie 1.x; Gebruik voor 2.x [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) in plaats van `BrokeredMessage`.

Opent de wachtrij of onderwerp via in JavaScript, `context.bindings.<name from function.json>`. U kunt een tekenreeks, een bytematrix of een Javascript-object (gedeserialiseerd naar JSON) toewijzen aan `context.binding.<name>`.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| Service Bus | [Service Bus-foutcodes](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Service Bus-limieten](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
