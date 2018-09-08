---
title: Timertrigger voor Azure Functions
description: Over het gebruik van de timer triggers in Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/08/2018
ms.author: glenga
ms.custom: ''
ms.openlocfilehash: e6a3df79bf0786b536dc4c454d19beea2730125a
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093121"
---
# <a name="timer-trigger-for-azure-functions"></a>Timertrigger voor Azure Functions 

In dit artikel wordt uitgelegd hoe u werken met triggers in Azure Functions timer. Een timertrigger kunt u een functie uitvoeren volgens een schema. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De timertrigger is opgegeven in de [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet-pakket versie 2.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Timers/) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De timertrigger is opgegeven in de [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die wordt uitgevoerd om de vijf minuten:

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

Het volgende voorbeeld ziet u een timertrigger binding in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie schrijft een logboek die aangeeft of deze functie-aanroep vanwege een schema voor gemiste-exemplaar is.

Hier volgt de binding-gegevens de *function.json* bestand:

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

Het volgende voorbeeld ziet u een timertrigger binding in een *function.json* bestand en een [F #-scriptfunctie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie schrijft een logboek die aangeeft of deze functie-aanroep vanwege een schema voor gemiste-exemplaar is.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Dit is de F #-scriptcode:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u een timertrigger binding in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie schrijft een logboek die aangeeft of deze functie-aanroep vanwege een schema voor gemiste-exemplaar is.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Dit is het JavaScript-script-code:

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

### <a name="java-example"></a>Java-voorbeeld

Het volgende van de voorbeeldfunctie wordt geactiveerd en wordt uitgevoerd om de vijf minuten. De `@TimerTrigger` aantekening op de functie definieert het schema met behulp van de indeling van de dezelfde tekenreeks als [CRON-expressies](http://en.wikipedia.org/wiki/Cron#CRON_expression).

```java
@FunctionName("keepAlive")
public void keepAlive(
  @TimerTrigger(name = "keepAliveTrigger", schedule = "0 *&#47;5 * * * *") String timerInfo,
      ExecutionContext context
 ) {
     // timeInfo is a JSON string, you can deserialize it to an object using your favorite JSON library
     context.getLogger().info("Timer is triggered: " + timerInfo);
}
```

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

De constructor van het kenmerk heeft een CRON-expressie of een `TimeSpan`. U kunt `TimeSpan` alleen als de functie-app wordt uitgevoerd op een App Service-plan. Het volgende voorbeeld ziet u een CRON-expressie:

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

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `TimerTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
|**type** | N.v.t. | Moet worden ingesteld op 'timerTrigger'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt.|
|**direction** | N.v.t. | Moet worden ingesteld op 'in'. Deze eigenschap wordt automatisch ingesteld wanneer u de trigger in Azure portal maakt. |
|**De naam** | N.v.t. | De naam van de variabele die staat voor het timerobject in functiecode aan te geven. | 
|**schedule**|**ScheduleExpression**|Een [CRON-expressie](#cron-expressions) of een [TimeSpan](#timespan) waarde. Een `TimeSpan` kan alleen worden gebruikt voor een functie-app die wordt uitgevoerd op een App Service-Plan. U kunt de schema-expressie opnemen in een app-instelling en deze eigenschap instellen op de app-instelling van de naam van verpakt in **%** tekenen, zoals in dit voorbeeld: "% ScheduleAppSetting %". |
|**runOnStartup**|**RunOnStartup**|Als `true`, de functie wordt aangeroepen wanneer de runtime wordt gestart. De runtime wordt bijvoorbeeld gestart wanneer de functie-app, ontwaakt nadat er een niet-actieve vanwege inactiviteit. Wanneer de functie-app opnieuw wordt gestart vanwege functie wijzigingen, en wanneer de functie-app wordt geschaald. Dus **runOnStartup** moet zelden of nooit worden ingesteld op `true`, zoals brengt de code uitvoeren op zeer onvoorspelbare tijdstippen.|
|**useMonitor**|**UseMonitor**|Ingesteld op `true` of `false` om aan te geven of het schema moet worden gecontroleerd. Bewaking van de planning zich blijft voordoen planning exemplaren om te helpen ervoor te zorgen dat de planning correct wordt bijgehouden, zelfs wanneer de functie app-exemplaren opnieuw starten. Als niet expliciet is ingesteld, de standaardinstelling is `true` voor schema's met een interval dat groter is dan 1 minuut. Voor schema's die meer dan één keer per minuut activeren is de standaardwaarde is `false`.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Gebruik

Wanneer een timer trigger-functie is aangeroepen, de [timerobject](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) wordt doorgegeven in de functie. De volgende JSON is een voorbeeld van de weergave van de timerobject. 

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

De `IsPastDue` eigenschap `true` wanneer de huidige functie-aanroep is later dan gepland. Bijvoorbeeld, een functie-app opnieuw opstarten kan leiden tot een aanroep om te worden overgeslagen.

## <a name="cron-expressions"></a>CRON-expressies 

Azure Functions maakt gebruik van de [NCronTab](https://github.com/atifaziz/NCrontab) bibliotheek met het interpreteren van de CRON-expressies. Een CRON-expressie bevat zes velden:

`{second} {minute} {hour} {day} {month} {day-of-week}`

Elk veld kan een van de volgende typen waarden hebben:

|Type  |Voorbeeld  |Wanneer ze worden geactiveerd  |
|---------|---------|---------|
|Een specifieke waarde |<nobr>"0 5 * * * *"</nobr>|op hh:05:00 waarbij UU staat voor elk uur (eenmaal per uur)|
|Alle waarden (`*`)|<nobr>"0 * 5 * * *"</nobr>|op 5:mm: 00 elke dag mm is waar elke minuut van het uur (60 keer per dag)|
|Een bereik (`-` operator)|<nobr>"5-7 * * * * *"</nobr>|hh:mm:05, hh:mm:06 en hh:mm:07 waarbij UU: mm elke minuut van elk uur (3 keer een minuut wordt)|  
|Een set waarden (`,` operator)|<nobr>"5,8,10 **** *"</nobr>|hh:mm:05, hh:mm:08 en hh:mm:10 waarbij UU: mm elke minuut van elk uur (3 keer een minuut wordt)|
|Een waarde voor interval (`/` operator)|<nobr>"0 */5 * * * *"</nobr>|hh:05:00, hh:10:00, hh:15:00, enzovoort via hh:55:00 waarbij UU staat voor elk uur (12 keer per uur)|

U kunt numerieke waarden, namen of afkortingen van namen gebruiken als u wilt opgeven, maanden of dagen kwijt bent:

* Voor dagen zijn de numerieke waarden 0 tot en met 6 waarbij 0 wordt gestart met zondag.
* Namen zijn in het Engels. Bijvoorbeeld: `Monday`, `January`.
* Namen zijn niet hoofdlettergevoelig.
* Namen kunnen worden afgekort. Drie letters is de afkorting van aanbevolen.  Bijvoorbeeld: `Mon`, `Jan`. 

### <a name="cron-examples"></a>CRON-voorbeelden

Hier volgen enkele voorbeelden van de CRON-expressies die u voor de timertrigger in Azure Functions gebruiken kunt.

|Voorbeeld|Wanneer ze worden geactiveerd  |
|---------|---------|
|`"0 */5 * * * *"`|om de vijf minuten|
|`"0 0 * * * *"`|één keer aan de bovenkant van elk uur|
|`"0 0 */2 * * *"`|elke twee uur|
|`"0 0 9-17 * * *"`|één keer per uur van 9: 00 op 17: 00 uur|
|`"0 30 9 * * *"`|om 9:30 uur elke dag|
|`"0 30 9 * * 1-5"`|om 9:30 uur elke weekdag|
|`"0 30 9 * Jan Mon"`|om 9:30 uur elke maandag in januari|
>[!NOTE]   
>U vindt online CRON-expressie-voorbeelden, maar veel van deze laat de `{second}` veld. Als u vanuit een van beide kopieert, voeg de ontbrekende `{second}` veld. Doorgaans wilt u een nul in het veld, niet een sterretje.

### <a name="cron-time-zones"></a>CRON-tijdzones

De getallen in een CRON-expressie verwijzen naar een tijd en datum, niet een tijdsduur. Bijvoorbeeld, een 5 in de `hour` veld verwijst naar 5:00 uur, niet elke vijf uur.

De standaardtijdzone gebruikt in combinatie met de CRON-expressies is Coordinated Universal Time (UTC). Als u wilt uw CRON-expressie op basis van een andere tijdzone, maakt u een app-instelling voor uw functie-app met de naam `WEBSITE_TIME_ZONE`. De waarde ingesteld op de naam van de gewenste tijdzone, zoals wordt weergegeven in de [Microsoft tijdzone Index](https://technet.microsoft.com/library/cc749073). 

Bijvoorbeeld, *Eastern (standaardtijd)* wordt UTC-05:00. De timer trigger worden gestart op 10:00 uur EST elke dag, gebruikt u de volgende CRON-expressie die voor de UTC-tijdzone-accounts hebben:

```json
"schedule": "0 0 15 * * *"
``` 

Of maak een app-instelling voor uw functie-app met de naam `WEBSITE_TIME_ZONE` en stel de waarde voor **Eastern (standaardtijd)**.  Vervolgens gebruikt de volgende CRON-expressie: 

```json
"schedule": "0 0 10 * * *"
``` 

Bij het gebruik `WEBSITE_TIME_ZONE`, de tijd wordt aangepast aan wijzigingen in de specifieke tijdzone, zoals de zomer-en wintertijd. 

## <a name="timespan"></a>TimeSpan

 Een `TimeSpan` kan alleen worden gebruikt voor een functie-app die wordt uitgevoerd op een App Service-Plan.

In tegenstelling tot een CRON-expressie, een `TimeSpan` waarde geeft het interval tussen elke functieaanroep. Wanneer een functie is voltooid na het uitvoeren van meer dan het opgegeven interval, roept de timer onmiddellijk de functie opnieuw.

Uitgedrukt als een tekenreeks, de `TimeSpan` indeling is `hh:mm:ss` wanneer `hh` is minder dan 24 uur per dag. Als de eerste twee cijfers 24 of hoger, de indeling is `dd:hh:mm`. Hier volgen enkele voorbeelden:

|Voorbeeld |Wanneer ze worden geactiveerd  |
|---------|---------|
|"01:00:00" | elk uur        |
|"00:01:00"|elke minuut         |
|"24:00:00" | elke 24 dagen        |

## <a name="scale-out"></a>Uitschalen

Als een functie-app wordt geschaald naar meerdere exemplaren, wordt slechts één exemplaar van een timer geactiveerde functie wordt uitgevoerd voor alle instanties.

## <a name="function-apps-sharing-storage"></a>Functie-apps delen van opslag

Als u een opslagaccount voor meerdere functie-apps deelt, zorg ervoor dat elke functie-app een andere heeft `id` in *host.json*. U kunt weglaten de `id` eigenschap of elke functie-app handmatig in te stellen `id` op een andere waarde. De timertrigger gebruikt een opslag-vergrendeling om ervoor te zorgen dat er slechts één instantie van de timer als een functie-app wordt geschaald naar meerdere exemplaren. Als twee functie-apps dezelfde delen `id` en elk een timertrigger gebruikt, slechts één timer wordt uitgevoerd.

## <a name="retry-behavior"></a>Gedrag voor opnieuw proberen

In tegenstelling tot de wachtrijtrigger opnieuw niet de timertrigger nadat een functie is mislukt. Als een functie mislukt, is niet het op de planning die tot de volgende keer opnieuw genoemd.

## <a name="troubleshooting"></a>Problemen oplossen

Zie voor informatie over wat er moet gebeuren wanneer de timertrigger werkt niet zoals verwacht, [Investigating en rapporteren van problemen met timer geactiveerde functies niet geactiveerd,](https://github.com/Azure/azure-functions-host/wiki/Investigating-and-reporting-issues-with-timer-triggered-functions-not-firing).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ga naar een snelstartgids die gebruikmaakt van een timertrigger](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
