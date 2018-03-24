---
title: Timertrigger voor Azure Functions
description: Begrijpen hoe timer triggers in Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure functions, functies, verwerking van gebeurtenissen, dynamische compute zonder server architectuur
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: tdykstra
ms.custom: ''
ms.openlocfilehash: 6f74dd4d9cb78c1316c87bd5a261e751b9b34923
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="timer-trigger-for-azure-functions"></a>Timertrigger voor Azure Functions 

Dit artikel wordt uitgelegd hoe u werkt met timer triggers in Azure Functions. Een timertrigger kunt u een functie uitvoeren volgens een schema. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pakketten

De timertrigger is opgegeven de [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet-pakket. De broncode voor het pakket bevindt zich in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die elke vijf minuten wordt uitgevoerd:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld ziet u een timertrigger binding in een *function.json* bestand en een [C# scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie een logboek die aangeeft of het aanroepen van deze functie vanwege een schema voor gemiste-exemplaar is wordt geschreven.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Dit is de C#-scriptcode:

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

### <a name="f-example"></a>F #-voorbeeld

Het volgende voorbeeld ziet u een timertrigger binding in een *function.json* bestand en een [F # scriptfunctie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie een logboek die aangeeft of het aanroepen van deze functie vanwege een schema voor gemiste-exemplaar is wordt geschreven.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Dit is de code F # script:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u een timertrigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie een logboek die aangeeft of het aanroepen van deze functie vanwege een schema voor gemiste-exemplaar is wordt geschreven.

Dit zijn de bindingsgegevens de *function.json* bestand:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Hier volgt de JavaScript-scriptcode:

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

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruiken de [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

De constructor van het kenmerk heeft een CRON-expressie, zoals weergegeven in het volgende voorbeeld:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
   ...
}
 ```

Kunt u een `TimeSpan` in plaats van een expressie CRON als uw app in de functie wordt uitgevoerd op een App Service-abonnement (niet in een plan verbruik).

Zie voor een compleet voorbeeld [C#-voorbeeld](#c-example).

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `TimerTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'timerTrigger'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op 'in'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de timer-object in de functiecode. | 
|**schedule**|**ScheduleExpression**|U kunt schema's met een CRON-expressie definiëren op het plan verbruik. Als u een App Service-abonnement, kunt u ook gebruiken een `TimeSpan` tekenreeks. De volgende secties worden de CRON-expressies. U kunt de expressie van de planning opnemen in een app-instelling en stel deze eigenschap in op een waarde die zijn ingepakt **%** hekjes, zoals in dit voorbeeld: '% NameOfAppSettingWithCRONExpression %'. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="cron-format"></a>CRON-indeling 

Een [CRON expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) voor de Azure Functions-timer trigger deze zes velden bevat: 

```
{second} {minute} {hour} {day} {month} {day-of-week}
```

>[!NOTE]   
>Veel van de CRON-expressies die u online vinden weglaten de `{second}` veld. Als u een van deze kopieert, voeg de ontbrekende `{second}` veld.

### <a name="cron-time-zones"></a>CRON-tijdzones

De standaardtijdzone gebruikt met de CRON-expressies is Coordinated Universal Time (UTC). Als u wilt dat de CRON-expressie op basis van een andere tijdzone, maakt u een nieuwe appinstelling voor de functie-app met de naam `WEBSITE_TIME_ZONE`. De waarde ingesteld op de naam van de gewenste tijdzone, zoals wordt weergegeven in de [Microsoft tijdzone Index](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Bijvoorbeeld: *Eastern (standaardtijd)* UTC-05:00. Om uw timer fire activeren op 10:00 AM GMT elke dag, gebruikt u de volgende CRON-expressie die voor de UTC-tijdzone-accounts:

```json
"schedule": "0 0 15 * * *",
``` 

U kunt ook kunt u een nieuwe appinstelling toevoegen voor de functie-app met de naam `WEBSITE_TIME_ZONE` en stel de waarde op **Eastern (standaardtijd)**.  De volgende CRON-expressie kan vervolgens worden gebruikt voor 10:00 AM GMT: 

```json
"schedule": "0 0 10 * * *",
``` 
### <a name="cron-examples"></a>CRON-voorbeelden

Hier volgen enkele voorbeelden van CRON-expressies die u voor de timertrigger in Azure Functions gebruiken kunt. 

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

## <a name="usage"></a>Gebruik

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

## <a name="scale-out"></a>Uitschalen

De timertrigger ondersteunt meerdere exemplaren scale-out. Slechts één exemplaar van een bepaalde timerfunctie wordt uitgevoerd in alle exemplaren.

## <a name="function-apps-sharing-storage"></a>Functie apps opslag delen

Als u een opslagaccount voor meerdere apps van de functie deelt, zorg dat elke functie-app een andere heeft `id` in *host.json*. U kunt weglaten de `id` eigenschap of stel handmatig elke functie-app `id` op een andere waarde. De timertrigger maakt gebruik van een vergrendeling opslag om ervoor te zorgen dat er slechts één exemplaar van de timer worden wanneer een functie-app uitgeschaald naar meerdere exemplaren. Als twee functie apps dezelfde delen `id` en elk een timertrigger gebruikt, wordt slechts één timer wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een Quick Start die een timertrigger gebruikt](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
