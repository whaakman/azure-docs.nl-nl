---
title: Azure Functions timertrigger | Microsoft Docs
description: Begrijpen hoe timer triggers in Azure Functions.
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 12beb090a95a31c7e83ae03a920016bdfbf474e3
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-functions-timer-trigger"></a>Azure Functions timertrigger

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dit artikel wordt uitgelegd hoe u configureert en code timer triggers in Azure Functions. Azure Functions heeft een timer trigger binding waarmee u de functiecode op basis van een ingesteld schema uitvoeren. 

De timertrigger ondersteunt meerdere exemplaren scale-out. Slechts één exemplaar van een bepaalde timerfunctie wordt uitgevoerd in alle exemplaren.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Timertrigger
De timertrigger aan een functie maakt gebruik van de volgende JSON-object in de `bindings` matrix van function.json:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

De waarde van `schedule` is een [CRON expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) die deze zes velden bevat: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Veel van de cron-expressies die u online vinden weglaten de `{second}` veld. Als u van één van beide kopieert, moet u voor de extra aanpassen `{second}` veld. Zie voor specifieke voorbeelden [voorbeelden plannen](#examples) hieronder.

De standaardtijdzone gebruikt met de CRON-expressies is Coordinated Universal Time (UTC). Als u wilt dat de CRON-expressie op basis van een andere tijdzone, maakt u een nieuwe appinstelling voor de functie-app met de naam `WEBSITE_TIME_ZONE`. De waarde ingesteld op de naam van de gewenste tijdzone, zoals wordt weergegeven in de [Microsoft tijdzone Index](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Bijvoorbeeld: *Eastern (standaardtijd)* UTC-05:00. Om uw timer fire activeren op 10:00 AM GMT elke dag, gebruikt u de volgende CRON-expressie die voor de UTC-tijdzone-accounts:

```json
"schedule": "0 0 15 * * *",
``` 

U kunt ook kunt u een nieuwe appinstelling toevoegen voor de functie-app met de naam `WEBSITE_TIME_ZONE` en stel de waarde op **Eastern (standaardtijd)**.  De volgende CRON-expressie kan vervolgens worden gebruikt voor 10:00 AM GMT: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Voorbeelden van de planning
Hier volgen enkele voorbeelden van CRON-expressies die u kunt gebruiken voor de `schedule` eigenschap. 

Om de vijf minuten activeren:

```json
"schedule": "0 */5 * * * *"
```

Voor het activeren van één keer aan de bovenkant van elk uur:

```json
"schedule": "0 0 * * * *",
```

Voor het activeren van elke twee uur:

```json
"schedule": "0 0 */2 * * *",
```

Voor het activeren van één keer per uur van 9: 00 uur tot 5 PM:

```json
"schedule": "0 0 9-17 * * *",
```

Om te activeren op elke dag 9:30 uur:

```json
"schedule": "0 30 9 * * *",
```

Om te activeren om 9:30 AM elke weekdag:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Gebruik van de trigger
Wanneer een functie van de trigger timer wordt opgeroepen, de [timerobject](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) wordt doorgegeven aan de functie. De volgende JSON is een voorbeeld-weergave van het timerobject. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Voorbeeld van de trigger
Stel dat u hebt de volgende timertrigger in de `bindings` matrix van function.json:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Zie het voorbeeld taalspecifieke die de timerobject om te zien leest of deze later wordt uitgevoerd.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Voorbeeld van de trigger in C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Voorbeeld van de trigger in F # #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Voorbeeld van de trigger in Node.js
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

