---
title: Azure Functions wachtrij opslag bindingen | Microsoft Docs
description: Het gebruik van Azure Storage-triggers en bindingen in de Azure Functions begrijpen.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: b68ce106ceb25d19ee0bbde287891d553a448560
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="azure-functions-queue-storage-bindings"></a>Azure Functions Queue Storage bindingen
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dit artikel wordt beschreven hoe u configureert en code Azure Queue storage bindingen in de Azure Functions. Azure Functions ondersteunt activeren en uitvoer van de bindingen voor Azure wachtrijen. Zie functies die beschikbaar in alle bindingen zijn [Azure Functions triggers en bindingen concepten](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Queue storage trigger
De trigger van Azure Queue storage kunt u de opslag van een wachtrij voor nieuwe berichten controleren en reageren op deze. 

Definieer een wachtrij trigger met de **integreren** tabblad in de portal functies. De portal maakt de volgende definitie in de **bindingen** sectie van *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<The name used to identify the trigger data in your code>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>"
}
```

* De `connection` eigenschap moet bevatten de naam van een app-instelling met een verbindingsreeks voor opslag. In de Azure portal, de standaard editor in de **integreren** tabblad dit app-instelling die u configureert wanneer u een opslagaccount selecteren.

Extra instellingen kunnen worden opgegeven in een [host.json bestand](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) naar queue storage-triggers verder te verfijnen. U kunt bijvoorbeeld de polling-interval in host.json wachtrij wijzigen.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Met behulp van een trigger wachtrij
In een Node.js-functies, toegang heeft tot de wachtrij gegevens met `context.bindings.<name>`.


Toegang tot de nettolading van de wachtrij met een parameter van de methode, zoals in de .NET-functies `CloudQueueMessage paramName`. Hier `paramName` is de waarde die u hebt opgegeven in de [trigger configuratie](#trigger). Bericht uit de wachtrij kan worden gedeserialiseerd met een van de volgende typen:

* POCO-object. Gebruik deze optie als de nettolading van de wachtrij een JSON-object is. De runtime van Functions deserializes de nettolading in de POCO-object. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Wachtrij trigger metagegevens
De trigger wachtrij biedt verschillende eigenschappen voor metagegevens. Deze eigenschappen kunnen worden gebruikt als onderdeel van de expressies voor gegevensbinding in andere bindingen of als parameters in uw code. De waarden hebben dezelfde betekenis als [ `CloudQueueMessage` ].

* **QueueTrigger** -nettolading van de wachtrij (als een geldige tekenreeks)
* **DequeueCount** -Type `int`. Het aantal keren dat dit bericht uit wachtrij is geplaatst.
* **ExpirationTime** -Type `DateTimeOffset?`. De tijd waarop het bericht is verlopen.
* **Id** -Type `string`. Wachtrij bericht-ID.
* **InsertionTime** -Type `DateTimeOffset?`. De tijd die het bericht is toegevoegd aan de wachtrij.
* **NextVisibleTime** -Type `DateTimeOffset?`. De tijd die het bericht vervolgens zichtbaar zijn.
* **PopReceipt** -Type `string`. Pop ontvangst van het bericht.

Informatie over het gebruiken van de metagegevens van de wachtrij in [Trigger voorbeeld](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Voorbeeld van de trigger
Stel dat u hebt de volgende function.json die een trigger wachtrij definieert:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

Zie het voorbeeld taalspecifieke die worden opgehaald en logboeken van de metagegevens van de wachtrij.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Voorbeeld van de trigger in C# #
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

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Voorbeeld van de trigger in Node.js

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a>Verwerken van verontreinigde berichten
Wanneer een functie van de trigger wachtrij mislukt pogingen Azure Functions die functie maximaal vijf keer voor een bepaalde wachtrij-bericht, met inbegrip van de eerste poging. Als alle vijf pogingen mislukken, wordt een bericht met de runtime van functions toegevoegd aan de opslag van een wachtrij met de naam  *&lt;originalqueuename >-verontreinigd*. U kunt schrijven om een functie verwerken van berichten uit de wachtrij verontreinigd door registratie of het verzenden van een melding dat handmatige aandacht nodig is. 

Voor het afhandelen van verontreinigde berichten handmatig, Controleer de `dequeueCount` van het bericht uit de wachtrij (Zie [wachtrij trigger metagegevens](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Queue storage uitvoer binding
De Azure queue storage uitvoer binding kunt u berichten schrijven naar een wachtrij. 

Definieer een wachtrij uitvoer binding met de **integreren** tabblad in de portal functies. De portal maakt de volgende definitie in de **bindingen** sectie van *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<The name used to identify the trigger data in your code>",
   "queueName": "<Name of queue to write to>",
   "connection":"<Name of app setting - see below>"
}
```

* De `connection` eigenschap moet bevatten de naam van een app-instelling met een verbindingsreeks voor opslag. In de Azure portal, de standaard editor in de **integreren** tabblad dit app-instelling die u configureert wanneer u een opslagaccount selecteren.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Binding met behulp van een wachtrij uitvoer
In een Node.js-functies, opent u de uitvoer wachtrij via `context.bindings.<name>`.

In de .NET-functies, kunt u uitvoeren met een van de volgende typen. Wanneer er een typeparameter `T`, `T` moet een van de ondersteunde uitvoermethode typen, zoals `string` of een POCO.

* `out T`(geserialiseerd als JSON)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

U kunt het retourtype van methode ook gebruiken als de uitvoer-binding.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Voorbeeld van wachtrij-uitvoer
De volgende *function.json* definieert een HTTP-trigger met een wachtrij binding uitvoer:

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
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

Zie het voorbeeld taalspecifieke die een wachtrijbericht met de nettolading van de binnenkomende HTTP-uitvoer.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Voorbeeld van uitvoer wachtrij in C# #

```cs
// C# example of HTTP trigger binding to a custom POCO, with a queue output binding
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

Meerdere om berichten te verzenden, gebruikt u een `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Voorbeeld van uitvoer wachtrij in Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Of om meerdere berichten te verzenden

```javascript
module.exports = function(context) {
    // Define a message array for the myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

Zie voor een voorbeeld van een functie die gebruikmaakt van queue storage-triggers en bindingen [maken van een functie die wordt geactiveerd door Azure Queue storage](functions-create-storage-queue-triggered-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage']: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
