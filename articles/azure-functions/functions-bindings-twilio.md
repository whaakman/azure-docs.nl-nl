---
title: Azure Functions Twilio-binding
description: Begrijpen hoe Twilio-bindingen met Azure Functions.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 52a45f1b67e3194739fe97daad56de2d3515dee3
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="twilio-binding-for-azure-functions"></a>Twilio-binding voor Azure Functions

In dit artikel wordt uitgelegd hoe u om berichten te verzenden met behulp van [Twilio](https://www.twilio.com/) bindingen in de Azure Functions. Azure Functions ondersteunt uitvoer bindingen voor Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die verstuurt een tekstbericht wanneer deze worden geactiveerd door een wachtrijbericht.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

In dit voorbeeld wordt de `TwilioSms` kenmerk met de geretourneerde waarde van de methode. Een alternatief is met het kenmerk met een `out SMSMessage` parameter of een `ICollector<SMSMessage>` of `IAsyncCollector<SMSMessage>` parameter.

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld wordt een Twilio-uitvoer binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie gebruikt een `out` -parameter voor het verzenden van een SMS-bericht.

Hier zijn bindingsgegevens de *function.json* bestand:

Voorbeeld function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Hier volgt een C#-scriptcode:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

U kunt niet out-parameters in de asynchrone code gebruiken. Hier volgt een asynchrone C# script codevoorbeeld:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld wordt een Twilio-uitvoer binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Hier zijn bindingsgegevens de *function.json* bestand:

Voorbeeld function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Hier volgt de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) kenmerk, die is gedefinieerd in NuGet-pakket [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio).

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [configuratie](#configuration). Hier volgt een `TwilioSms` kenmerk voorbeeld in een handtekening voor methode:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(
    AccountSidSetting = "TwilioAccountSid", 
    AuthTokenSetting = "TwilioAuthToken", 
    From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    ...
}
 ```

Zie voor een compleet voorbeeld [C#-voorbeeld](#c-example).

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `TwilioSms` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type**|| moet worden ingesteld op `twilioSms`.|
|**richting**|| moet worden ingesteld op `out`.|
|**naam**|| Naam van de variabele in functiecode gebruikt voor de Twilio SMS-bericht. |
|**accountSid**|**AccountSid**| Deze waarde moet worden ingesteld op de naam van een app-instelling van de Sid van uw Twilio-Account.|
|**authToken**|**AuthToken**| Deze waarde moet worden ingesteld op de naam van een app-instelling die uw Twilio-verificatietoken bevat.|
|**Aan**|**Aan**| Deze waarde is ingesteld op het telefoonnummer dat naar de SMS-tekst wordt verzonden.|
|**Van**|**Van**| Deze waarde is ingesteld op het telefoonnummer dat door de SMS-tekst wordt verzonden.|
|**hoofdtekst**|**Hoofdtekst**| Deze waarde kan worden gebruikt voor de SMS-bericht harde code als u niet hoeft worden dynamisch ingesteld in de code voor de functie. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)


