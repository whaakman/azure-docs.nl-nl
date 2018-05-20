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
ms.openlocfilehash: a8844ea44bf604944c5980b0d41ab5d01a30b876
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="timer-trigger-for-azure-functions"></a>Timertrigger voor Azure Functions 

Dit artikel wordt uitgelegd hoe u werkt met timer triggers in Azure Functions. Een timertrigger kunt u een functie uitvoeren volgens een schema. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pakketten

De timertrigger is opgegeven de [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet-pakket. De broncode voor het pakket bevindt zich in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-package-versions](../../includes/functions-package-versions.md)]

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
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
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

De constructor van het kenmerk werkt met een CRON-expressie of een `TimeSpan`. U kunt `TimeSpan` alleen als de functie-app wordt uitgevoerd op een App Service-abonnement. Het volgende voorbeeld ziet u een CRON-expressie:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if (myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
 ```

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding-configuratie-eigenschappen die u instelt in de *function.json* bestand en de `TimerTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'timerTrigger'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op 'in'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in de Azure-portal maakt. |
|**Naam** | N.v.t. | De naam van de variabele die staat voor de timer-object in de functiecode. | 
|**schedule**|**ScheduleExpression**|Een [CRON expressie](#cron-expressions) of een [TimeSpan](#timespan) waarde. Een `TimeSpan` kan alleen worden gebruikt voor een functie-app die wordt uitgevoerd op een App Service-Plan. U kunt de expressie van de planning opnemen in een app-instelling en deze eigenschap instellen op de app verpakt naam van instelling **%** hekjes, zoals in dit voorbeeld: '% ScheduleAppSetting %'. |
|**runOnStartup**|**RunOnStartup**|Als `true`, de functie wordt aangeroepen wanneer de runtime wordt gestart. De runtime wordt bijvoorbeeld gestart wanneer de functie-app ontwaakt na inactiviteit vanwege inactiviteit. Wanneer de functie-app opnieuw wordt gestart als gevolg van wijzigingen van de functie, en wanneer de functie-app uitgeschaald. Dus **runOnStartup** moet zelden of nooit worden ingesteld op `true`, zoals brengt code uitvoeren op maximaal onvoorspelbare tijdstippen.|
|**useMonitor**|**UseMonitor**|Ingesteld op `true` of `false` om aan te geven of het schema moet worden gecontroleerd. Planning bewaking persistente planning voorvallen als hulpmiddel om ervoor te zorgen dat de planning correct wordt bijgehouden, zelfs wanneer de functie app-exemplaren opnieuw opstart. Als niet expliciet is ingesteld, de standaardinstelling is `true` voor schema's die u een interval dat groter is dan 1 minuut hebt. Voor schema's die meer dan één keer per minuut activeren, de standaardwaarde is `false`.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Gebruik

Wanneer een functie van de trigger timer wordt opgeroepen, de [timerobject](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) wordt doorgegeven aan de functie. De volgende JSON is een voorbeeld-weergave van het timerobject. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00+00:00",
        "LastUpdated":"2016-10-04T10:16:00+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

De `IsPastDue` eigenschap is `true` wanneer de huidige functie-aanroep is later dan gepland. Een app opnieuw starten van de functie veroorzaken mogelijk een aanroep naar worden overgeslagen.

## <a name="cron-expressions"></a>CRON-expressies 

Een CRON-expressie voor de trigger van Azure Functions timer bevat zes velden: 

`{second} {minute} {hour} {day} {month} {day-of-week}`

Elk veld kan een van de volgende soorten waarden hebben:

|Type  |Voorbeeld  |Wanneer deze worden geactiveerd  |
|---------|---------|---------|
|Een bepaalde waarde |<nobr>"0 5 * * * *"</nobr>|op hh:05:00 waarbij UU staat voor elk uur (eens per uur)|
|Alle waarden (`*`)|<nobr>"0 * 5 * * *"</nobr>|op 5:mm: 00 per dag uit, waarbij mm wordt elke minuut van het uur (60 keer per dag)|
|Een bereik (`-` operator)|<nobr>"5-7 * * * * *"</nobr>|op hh:mm:05 hh:mm:06 en hh:mm:07 waarbij UU: mm elke minuut van elk uur (3 maal een minuut wordt)|  
|Een set waarden (`,` operator)|<nobr>"5,8,10 **** *"</nobr>|op hh:mm:05 hh:mm:08 en hh:mm:10 waarbij UU: mm elke minuut van elk uur (3 maal een minuut wordt)|
|Een intervalwaarde (`/` operator)|<nobr>"0 */5 * * * *"</nobr>|op hh:05:00, hh:10:00, hh:15:00 tot en met hh:55:00 waarbij UU staat voor elk uur (12 keer een uur)|

### <a name="cron-examples"></a>CRON-voorbeelden

Hier volgen enkele voorbeelden van CRON-expressies die u voor de timertrigger in Azure Functions gebruiken kunt.

|Voorbeeld|Wanneer deze worden geactiveerd  |
|---------|---------|
|"0 */5 * * * *"|om de vijf minuten|
|"0 0 * * * *"|eenmaal boven aan elk uur|
|"0 0 * / 2 ** *"|elke twee uur|
|"0 0-9-17 ** *"|één keer per uur van 9: 00 uur op 17: 00 uur|
|"0 30 9 * * *"|op elke dag 9:30 uur|
|"0 30 9 * * 1-5"|om 9:30 AM elke weekdag|

>[!NOTE]   
>U vindt online CRON expressie voorbeelden, maar veel van deze laat de `{second}` veld. Als u een van deze kopieert, voeg de ontbrekende `{second}` veld. Doorgaans wilt u een nul in dat veld niet een sterretje.

### <a name="cron-time-zones"></a>CRON-tijdzones

De getallen in een expressie CRON verwijzen naar een tijd en datum, niet voor een periode. Bijvoorbeeld, een 5 in de `hour` veld verwijst naar 5:00 uur, niet elke vijf uur.

De standaardtijdzone gebruikt met de CRON-expressies is Coordinated Universal Time (UTC). Als u wilt dat de CRON-expressie op basis van een andere tijdzone, maakt u een app-instelling voor de functie-app met de naam `WEBSITE_TIME_ZONE`. De waarde ingesteld op de naam van de gewenste tijdzone, zoals wordt weergegeven in de [Microsoft tijdzone Index](https://technet.microsoft.com/library/cc749073). 

Bijvoorbeeld: *Eastern (standaardtijd)* UTC-05:00. Om uw timer fire activeren op 10:00 AM GMT elke dag, gebruikt u de volgende CRON-expressie die voor de UTC-tijdzone-accounts:

```json
"schedule": "0 0 15 * * *",
``` 

Of maak een app-instelling voor de functie-app met de naam `WEBSITE_TIME_ZONE` en stel de waarde op **Eastern (standaardtijd)**.  Vervolgens gebruikt de volgende CRON-expressie: 

```json
"schedule": "0 0 10 * * *",
``` 

## <a name="timespan"></a>TimeSpan

 Een `TimeSpan` kan alleen worden gebruikt voor een functie-app die wordt uitgevoerd op een App Service-Plan.

In tegenstelling tot een expressie CRON een `TimeSpan` waarde geeft het interval tussen elke functie-aanroep. Wanneer een functie is voltooid na het uitvoeren van meer dan het opgegeven interval, roept de timer onmiddellijk de functie opnieuw.

Uitgedrukt als een tekenreeks, de `TimeSpan` indeling is `hh:mm:ss` wanneer `hh` is minder dan 24. Wanneer de eerste twee cijfers 24 of hoger zijn, de indeling is `dd:hh:mm`. Hier volgen enkele voorbeelden:

|Voorbeeld |Wanneer deze worden geactiveerd  |
|---------|---------|
|"01:00:00" | elk uur        |
|"00:01:00"|elke minuut         |
|"24:00:00" | elke 24 dagen        |

## <a name="scale-out"></a>Uitschalen

Als een functie-app uitgeschaald naar meerdere exemplaren, wordt slechts één exemplaar van een functie timer geactiveerd in alle exemplaren uitgevoerd.

## <a name="function-apps-sharing-storage"></a>Functie apps opslag delen

Als u een opslagaccount voor meerdere apps van de functie deelt, zorg dat elke functie-app een andere heeft `id` in *host.json*. U kunt weglaten de `id` eigenschap of stel handmatig elke functie-app `id` op een andere waarde. De timertrigger maakt gebruik van een vergrendeling opslag om ervoor te zorgen dat er slechts één exemplaar van de timer worden wanneer een functie-app uitgeschaald naar meerdere exemplaren. Als twee functie apps dezelfde delen `id` en elk een timertrigger gebruikt, wordt slechts één timer wordt uitgevoerd.

## <a name="retry-behavior"></a>Opnieuw proberen

In tegenstelling tot de wachtrij worden geactiveerd opnieuw niet de timertrigger nadat een functie is mislukt. Als er een functie uitvalt, het is't tot de volgende keer opnieuw worden aangeroepen voor de planning.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een Quick Start die een timertrigger gebruikt](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions triggers en bindingen](functions-triggers-bindings.md)
