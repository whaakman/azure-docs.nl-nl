---
title: Azure Functions Twilio-binding | Microsoft Docs
description: Begrijpen hoe Twilio-bindingen met Azure Functions.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8c5e8f2dfedae26486e1c8afbe0cec3f3228e86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a>SMS-bericht verzenden berichten van Azure Functions met behulp van de Twilio uitvoer binding
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe configureren en gebruiken van Twilio-bindingen met Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure Functions ondersteunt Twilio uitvoer bindingen zodat uw functies voor het verzenden van SMS-berichten met een paar regels code en een [Twilio](https://www.twilio.com/) account. 

## <a name="functionjson-for-the-twilio-output-binding"></a>Function.JSON voor de Twilio uitvoer binding
Het bestand function.json biedt de volgende eigenschappen:

|Eigenschap  |Beschrijving  |
|---------|---------|
|**naam**| Naam van de variabele in functiecode gebruikt voor de Twilio SMS-bericht. |
|**type**| moet worden ingesteld op `twilioSms`.|
|**accountSid**| Deze waarde moet worden ingesteld op de naam van een App-instelling van de Sid van uw Twilio-Account.|
|**authToken**| Deze waarde moet worden ingesteld op de naam van een App-instelling die uw Twilio-verificatietoken bevat.|
|**Aan**| Deze waarde is ingesteld op het telefoonnummer dat naar de SMS-tekst wordt verzonden.|
|**Van**| Deze waarde is ingesteld op het telefoonnummer dat door de SMS-tekst wordt verzonden.|
|**richting**| moet worden ingesteld op `out`.|
|**hoofdtekst**| Deze waarde kan worden gebruikt voor de SMS-bericht harde code als u niet hoeft worden dynamisch ingesteld in de code voor de functie. |

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Voorbeeld van C# wachtrij trigger met Twilio uitvoer binding
#### <a name="synchronous"></a>Synchrone
Deze synchrone voorbeeldcode voor een Azure Storage-wachtrij-trigger gebruikt een out-parameter voor een SMS-bericht verzenden naar een klant die een bestelling geplaatst.

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

#### <a name="asynchronous"></a>Asynchrone
Deze asynchrone voorbeeldcode voor een Azure Storage-trigger voor wachtrij verzendt een SMS-bericht naar een klant die een bestelling geplaatst.

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

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Voorbeeld Node.js wachtrij trigger met Twilio uitvoer binding
In dit voorbeeld Node.js verzendt een SMS-bericht naar een klant die een bestelling geplaatst.

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

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

