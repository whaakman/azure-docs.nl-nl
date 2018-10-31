---
title: Azure Service Bus-bindingen voor Azure Functions
description: Over het gebruik van Azure Service Bus-triggers en bindingen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 04/01/2017
ms.author: cshoe
ms.openlocfilehash: f440e92f62c7c61966145a1e74d3d3be9f6b7825
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50250553"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Service Bus-bindingen voor Azure Functions

In dit artikel wordt uitgelegd hoe u werkt met Azure Service Bus-bindingen in Azure Functions. Azure Functions ondersteunt activeren en uitvoerbindingen voor Service Bus-wachtrijen en onderwerpen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Service Bus-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet-pakket versie 2.x. 

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Service Bus-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

De Service Bus-trigger te gebruiken om te reageren op berichten van een Service Bus-wachtrij of onderwerp. 

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die leest [bericht metagegevens](#trigger---message-metadata) en een Service Bus-wachtrij-bericht vastlegt:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    ILogger log)
{
    log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"DeliveryCount={deliveryCount}");
    log.LogInformation($"MessageId={messageId}");
}
```

In dit voorbeeld is voor Azure Functions-versie 1.x. Deze code werkt voor 2.x maken:

- [Laat de parameter access rechten](#trigger---configuration)
- Wijzig het type van de parameter van het logboek van `TraceWriter` naar `ILogger`
- Wijziging `log.Info` naar `log.LogInformation`
 
### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie leest [bericht metagegevens](#trigger---message-metadata) en een Service Bus-wachtrij-bericht vastlegt.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [F #-functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie vastlegt een Service Bus-wachtrij-bericht. 

Hier volgt de binding-gegevens de *function.json* bestand:

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

Dit is de F #-scriptcode:

```fsharp
let Run(myQueueItem: string, log: ILogger) =
    log.LogInformation(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie leest [bericht metagegevens](#trigger---message-metadata) en een Service Bus-wachtrij-bericht vastlegt. 

Hier volgt de binding-gegevens de *function.json* bestand:

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

Dit is het JavaScript-script-code:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

### <a name="trigger---java-example"></a>Trigger - Java-voorbeeld

Het volgende voorbeeld ziet u een Service Bus-trigger binding in een *function.json* bestand en een [Java functie](functions-reference-java.md) die gebruikmaakt van de binding. De functie wordt geactiveerd door een bericht op een Service Bus-wachtrij geplaatst en de functie de wachtrijbericht vastlegt.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
"bindings": [
    {
    "queueName": "myqueuename",
    "connection": "MyServiceBusConnection",
    "name": "msg",
    "type": "ServiceBusQueueTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Dit is de Java-code:

```java
@FunctionName("sbprocessor")
 public void serviceBusProcess(
    @ServiceBusQueueTrigger(name = "msg",
                             queueName = "myqueuename",
                             connection = "myconnvarname") String message,
   final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), de volgende kenmerken gebruiken om te configureren van een Service Bus-trigger:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  De constructor van het kenmerk wordt de naam van de wachtrij of onderwerp en abonnement. In Azure Functions-versie 1.x, kunt u ook de toegangsrechten van de verbinding opgeven. Als u geen toegangsrechten opgeeft, wordt standaard `Manage`. Zie voor meer informatie de [Trigger - configuratie](#trigger---configuration) sectie.

  Hier volgt een voorbeeld waarin het kenmerk wordt gebruikt met een queryreeks-parameter:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  U kunt instellen dat de `Connection` eigenschap om op te geven van de Service Bus-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAccountAttribute.cs)

  Biedt een andere manier om op te geven van de Service Bus-account moet worden gebruikt. De constructor, wordt de naam van een app-instelling met een Service Bus-verbindingsreeks. Het kenmerk kan worden toegepast op de parameter, klasseniveau of methode. Het volgende voorbeeld toont het klasseniveau en methode:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, ILogger log)
  {
      ...
  }
  ```

De Service Bus-account moet worden gebruikt, wordt bepaald in de volgende volgorde:

* De `ServiceBusTrigger` van kenmerk `Connection` eigenschap.
* De `ServiceBusAccount` kenmerk toegepast op de dezelfde parameter als de `ServiceBusTrigger` kenmerk.
* De `ServiceBusAccount` kenmerk toegepast op de functie.
* De `ServiceBusAccount` kenmerk toegepast op de klasse.
* De 'AzureWebJobsServiceBus' app-instelling.

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `ServiceBusTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'serviceBusTrigger'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op 'in'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij of onderwerp bericht in de functiecode aan te geven. Ingesteld op '$return' om te verwijzen naar de geretourneerde waarde van de functie. | 
|**queueName**|**Wachtrijnaam**|Naam van de wachtrij om te controleren.  Alleen ingesteld als bewaking van een wachtrij, niet voor een onderwerp.
|**topicName**|**topicName**|Naam van het onderwerp om te controleren. Alleen ingesteld als een onderwerp, voor een wachtrij niet controleren.|
|**subscriptionName**|**subscriptionName**|De naam van het abonnement om te controleren. Alleen ingesteld als een onderwerp, voor een wachtrij niet controleren.|
|**verbinding**|**verbinding**|De naam van een app-instelling met de Service Bus-verbindingsreeks moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam opgeven. Als u bijvoorbeeld `connection` naar 'MyServiceBus', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyServiceBus." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de standaard Service Bus-verbindingsreeks in de app-instelling met de naam 'AzureWebJobsServiceBus'.<br><br>Als u een verbindingsreeks, volgt u de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). De verbindingsreeks moet voor een Service Bus-naamruimte, niet beperkt tot een specifieke wachtrij of onderwerp. |
|**accessRights**|**Toegang**|Toegangsrechten voor de verbindingsreeks. Beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, waarmee wordt aangegeven dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` ' luisteren '. Anders wordt het Functions runtime mislukken probeert uit te voeren bewerkingen uit waarbij rechten beheren. In Azure Functions versie 2.x gebruikt, deze eigenschap is niet beschikbaar omdat de nieuwste versie van de Storage-SDK biedt geen ondersteuning voor bewerkingen beheren.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger - gebruik

In C# en C#-script, kunt u de volgende parametertypen voor de wachtrij of onderwerp bericht:

* `string` -Als het bericht tekst is.
* `byte[]` -Dit is handig voor binaire gegevens.
* Een aangepast type - als het bericht JSON bevat, Azure Functions probeert te deserialiseren van de JSON-gegevens.
* `BrokeredMessage` -Biedt u het gedeserialiseerde bericht met de [BrokeredMessage.GetBody<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) methode.

Deze parameters zijn voor Azure Functions-versie 1.x; Gebruik voor 2.x [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) in plaats van `BrokeredMessage`.

In JavaScript, opent u het bericht wachtrij of onderwerp via `context.bindings.<name from function.json>`. De Service Bus-bericht wordt doorgegeven in de functie als een tekenreeks of een JSON-object.

## <a name="trigger---poison-messages"></a>Trigger - Onverwerkbare berichten

Beheer van Onverwerkbare berichten afhandelen, kan niet worden beheerd of geconfigureerd in Azure Functions. Service Bus verwerkt Onverwerkbare berichten zelf.

## <a name="trigger---peeklock-behavior"></a>Trigger - PeekLock gedrag

De Functions-runtime ontvangt een bericht in [PeekLock modus](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Wordt de `Complete` op dat wordt weergegeven als de functie is voltooid of aanroepen `Abandon` als de functie is mislukt. Als de functie langer duurt dan uitvoeren de `PeekLock` time-out voor de vergrendeling automatisch is verlengd, zolang de functie wordt uitgevoerd. 

Functies 1.x kunt u configureren `autoRenewTimeout` in *host.json*, die verwijst naar [OnMessageOptions.AutoRenewTimeout](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout?view=azure-dotnet#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout). De maximaal toegestane aantal voor deze instelling is 5 minuten op basis van de documentatie voor Service Bus, terwijl u de tijdslimiet functies van de standaardwaarde van 5 minuten tot 10 minuten kunt verhogen. Voor Service Bus-functies wouldn't u vervolgens doen omdat u de limiet voor het vernieuwen van Service Bus wordt overschreden.

## <a name="trigger---message-metadata"></a>Trigger - bericht-metagegevens

De Service Bus-trigger bevat diverse [metagegevenseigenschappen](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies in andere bindingen voor gegevensbinding of als parameters in uw code. Dit zijn de eigenschappen van de [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) klasse.

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Het aantal bezorgingen.|
|`DeadLetterSource`|`string`|De dead-letter-bron.|
|`ExpiresAtUtc`|`DateTime`|De vervaldatum tijd in UTC.|
|`EnqueuedTimeUtc`|`DateTime`|De in de wachtrij-tijd in UTC.|
|`MessageId`|`string`|Een door de gebruiker gedefinieerde waarde die Service Bus gebruiken om dubbele berichten te identificeren als ingeschakeld.|
|`ContentType`|`string`|Een inhoudstype-id die door de afzender en de ontvanger voor specifieke logica van toepassingen wordt gebruikt.|
|`ReplyTo`|`string`|Het antwoordadres wachtrij.|
|`SequenceNumber`|`Int64`|Het unieke nummer toegewezen aan een bericht door Service Bus.|
|`To`|`string`|De verzenden naar een adres.|
|`Label`|`string`|De toepassingsspecifiek label.|
|`CorrelationId`|`string`|De correlatie-ID.|
|`UserProperties`|`IDictionary<String,Object>`|De eigenschappen van de toepassing specifieke berichten.|

Zie [codevoorbeelden](#trigger---example) die eerder in dit artikel gebruikmaken van deze eigenschappen.

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen voor host.json

De [host.json](functions-host-json.md#servicebus) bestand bevat instellingen die het gedrag van Service Bus-trigger beheren.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-service-bus.md)]

## <a name="output"></a>Uitvoer

Gebruik Azure Service Bus-Uitvoerbinding voor het verzenden van berichten in wachtrij of onderwerp.

## <a name="output---example"></a>Uitvoer - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)
* [Java](#output--java-example)

### <a name="output---c-example"></a>Uitvoer - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) het bericht van een Service Bus-wachtrij worden verzonden:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, ILogger log)
{
    log.LogInformation($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Uitvoer - voorbeeld van C#-script

Het volgende voorbeeld ziet u een Service Bus-Uitvoerbinding een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie een timertrigger gebruikt voor het verzenden van een wachtrijbericht elke 15 seconden.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Hier volgt een C#-script-code die wordt gemaakt van een enkel bericht:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Hier volgen enkele redenen C#-script-code die wordt gemaakt van meerdere berichten:

```cs
public static void Run(TimerInfo myTimer, ILogger log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Uitvoer - F #-voorbeeld

Het volgende voorbeeld ziet u een Service Bus-Uitvoerbinding een *function.json* bestand en een [F #-scriptfunctie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie een timertrigger gebruikt voor het verzenden van een wachtrijbericht elke 15 seconden.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Hier volgt een F #-scriptcode die wordt gemaakt van een enkel bericht:

```fsharp
let Run(myTimer: TimerInfo, log: ILogger, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.LogInformation(message)
    outputSbQueue = message
```

### <a name="output---javascript-example"></a>Uitvoer - JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Service Bus-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie een timertrigger gebruikt voor het verzenden van een wachtrijbericht elke 15 seconden.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Hier volgt een JavaScript-script-code die wordt gemaakt van een enkel bericht:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Hier volgt een JavaScript-script-code die wordt gemaakt van meerdere berichten:

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


### <a name="output---java-example"></a>Uitvoer - Java-voorbeeld

Het volgende voorbeeld ziet u een Java-functie die een bericht naar een Service Bus-wachtrij verzendt `myqueue` wanneer ze worden geactiveerd door een HTTP-aanvraag.

```java
@FunctionName("httpToServiceBusQueue")
@ServiceBusQueueOutput(name = "message", queueName = "myqueue", connection = "AzureServiceBusConnection")
public String pushToQueue(
  @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
  final String message,
  @HttpOutput(name = "response") final OutputBinding<T> result ) {
      result.setValue(message + " has been sent.");
      return message;
 }
 ```

 In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `@QueueOutput` aantekening op functieparameters waarvan de waarde kan worden geschreven naar een Service Bus-wachtrij.  Het parametertype moet `OutputBinding<T>`, waarbij T alle systeemeigen Java-type van een POJO.

## <a name="output---attributes"></a>Uitvoer - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs).

De constructor van het kenmerk wordt de naam van de wachtrij of onderwerp en abonnement. U kunt ook de toegangsrechten van de verbinding opgeven. Het kiezen van de toegangsrechten instellen wordt uitgelegd in de [uitvoer - configuratie](#output---configuration) sectie. Hier volgt een voorbeeld waarin het kenmerk dat wordt toegepast op de geretourneerde waarde van de functie:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

U kunt instellen dat de `Connection` eigenschap om op te geven van de Service Bus-account moet worden gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Zie voor een compleet voorbeeld [uitvoer - voorbeeld met C#](#output---c-example).

U kunt de `ServiceBusAccount` kenmerk om op te geven van de Service Bus-account moet worden gebruikt op het niveau van klasse, methode of parameter.  Zie voor meer informatie, [Trigger - kenmerken](#trigger---attributes).

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `ServiceBus` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'Service Bus'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op 'out'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor de wachtrij of onderwerp in de functiecode aan te geven. Ingesteld op '$return' om te verwijzen naar de geretourneerde waarde van de functie. | 
|**queueName**|**Wachtrijnaam**|Naam van de wachtrij.  Alleen ingesteld als Wachtrijberichten, niet voor een onderwerp te verzenden.
|**topicName**|**topicName**|Naam van het onderwerp om te controleren. Alleen ingesteld als onderwerp verzonden, niet voor een wachtrij.|
|**verbinding**|**verbinding**|De naam van een app-instelling met de Service Bus-verbindingsreeks moet worden gebruikt voor deze binding. Als de naam van de app-instelling begint met 'AzureWebJobs', kunt u alleen het restant van de naam opgeven. Als u bijvoorbeeld `connection` naar 'MyServiceBus', de Functions-runtime ziet eruit voor een app-instelling die is met de naam "AzureWebJobsMyServiceBus." Als u niets `connection` leeg is, wordt de Functions-runtime maakt gebruik van de standaard Service Bus-verbindingsreeks in de app-instelling met de naam 'AzureWebJobsServiceBus'.<br><br>Als u een verbindingsreeks, volgt u de stappen die wordt weergegeven op [Beheerreferenties](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). De verbindingsreeks moet voor een Service Bus-naamruimte, niet beperkt tot een specifieke wachtrij of onderwerp.|
|**accessRights**|**Toegang**|Toegangsrechten voor de verbindingsreeks. Beschikbare waarden zijn `manage` en `listen`. De standaardwaarde is `manage`, waarmee wordt aangegeven dat de `connection` heeft de **beheren** machtiging. Als u een verbindingsreeks die u niet beschikt over de **beheren** , machtigingenset `accessRights` ' luisteren '. Anders wordt het Functions runtime mislukken probeert uit te voeren bewerkingen uit waarbij rechten beheren. In Azure Functions versie 2.x gebruikt, deze eigenschap is niet beschikbaar omdat de nieuwste versie van de Storage-SDK biedt geen ondersteuning voor bewerkingen beheren.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uitvoer - gebruik

In Azure Functions 1.x, de runtime maakt de wachtrij als deze nog niet bestaat en u hebt ingesteld `accessRights` naar `manage`. In functies versie moet 2.x, de wachtrij of onderwerp al bestaan; Als u een wachtrij of onderwerp die niet bestaat opgeeft, wordt de functie mislukt. 

In C# en C#-script, kunt u de volgende parametertypen voor de Uitvoerbinding:

* `out T paramName` - `T` geen JSON-geserialiseerd type kan zijn. Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt het bericht in functies gemaakt met een null-object.
* `out string` -Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `out byte[]` -Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `out BrokeredMessage` -Als de waarde van parameter is null wanneer de functie wordt afgesloten, wordt in functies een bericht niet maken.
* `ICollector<T>` of `IAsyncCollector<T>` : voor het maken van meerdere berichten. Een bericht wordt gemaakt bij het aanroepen van de `Add` methode.

In het async-functies, gebruikt u de geretourneerde waarde of `IAsyncCollector` in plaats van een `out` parameter.

Deze parameters zijn voor Azure Functions-versie 1.x; Gebruik voor 2.x [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) in plaats van `BrokeredMessage`.

In JavaScript, toegang krijgen tot de wachtrij of onderwerp met behulp van `context.bindings.<name from function.json>`. U kunt een tekenreeks, een bytematrix of een Javascript-object (gedeserialiseerd naar JSON) toewijzen aan `context.binding.<name>`.

## <a name="exceptions-and-return-codes"></a>Uitzonderingen en retourcodes

| Binding | Referentie |
|---|---|
| Service Bus | [Service Bus-foutcodes](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Service Bus-limieten](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>instellingen voor host.JSON

In deze sectie beschrijft de globale configuratie-instellingen beschikbaar voor deze binding in versie 2.x. Het onderstaande voorbeeld host.json bestand bevat alleen de versie 2.x-instellingen voor deze binding. Voor meer informatie over globale configuratie-instellingen in versie 2.x, Zie [naslaginformatie over host.json voor Azure Functions versie 2.x](functions-host-json.md).

> [!NOTE]
> Voor een verwijzing van host.json in functies 1.x, Zie [naslaginformatie over host.json voor Azure Functions 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "serviceBus": {
            "prefetchCount": 100,
            "messageHandlerOptions": {
                "autoComplete": false,
                "maxConcurrentCalls": 32,
                "maxAutoRenewDuration": "00:55:00"
        }
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|autoRenewTimeout|00:05:00|De maximale tijdsduur waarbinnen de vergrendeling van het bericht automatisch wordt vernieuwd.| 
|automatisch aanvullen|false|De trigger moet of onmiddellijk markeren als voltooid (automatisch aanvullen) of wacht op verwerking voor het aanroepen van voltooid.| 
|maxConcurrentCalls|16|Het maximale aantal gelijktijdige oproepen aan de callback die de bericht-pomp moet starten. Standaard verwerkt de Functions-runtime meerdere berichten tegelijkertijd. Instellen om de runtime voor het verwerken van alleen een één wachtrij of onderwerp bericht op een tijdstip, `maxConcurrentCalls` op 1. | 
|prefetchCount|N.v.t.|De standaard PrefetchCount die wordt gebruikt door de onderliggende MessageReceiver.| 


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
