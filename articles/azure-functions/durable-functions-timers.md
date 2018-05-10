---
title: Timers in duurzame functies - Azure
description: Informatie over het implementeren van duurzame timers in de uitbreiding duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/30/2018
ms.author: azfuncdf
ms.openlocfilehash: 4fd86b70965a7be84c72e265af798292819cbe96
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="timers-in-durable-functions-azure-functions"></a>Timers in duurzame functies (Azure-functies)

[Duurzame functies](durable-functions-overview.md) biedt *duurzame timers* voor gebruik in orchestrator-functies voor het implementeren van vertragingen of voor het instellen van time-outs voor asynchrone bewerkingen. Duurzame timers moeten worden gebruikt in de orchestrator-functies in plaats van `Thread.Sleep` en `Task.Delay` (C#) of `setTimeout()` en `setInterval()` (JavaScript).

U maakt een duurzame timer door het aanroepen van de [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) methode in [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html). De methode retourneert een taak die op een opgegeven datum en tijdstip hervat.

## <a name="timer-limitations"></a>Beperkingen voor timer

Wanneer u een timer die verloopt om 4:30 uur, de onderliggende duurzame taak Framework enqueues een bericht waarin alleen om 4:30 uur zichtbaar maken. Wanneer in de Azure Functions verbruik planning wordt uitgevoerd, kan het zojuist zichtbaar timerbericht zorgt ervoor dat de functie-app op een geschikte virtuele machine wordt geactiveerd.

> [!NOTE]
> * Duurzame timers kunnen niet langer dan zeven dagen door beperkingen in Azure Storage. We werken op een [functie aanvraag uit te breiden timers dan 7 dagen](https://github.com/Azure/azure-functions-durable-extension/issues/14).
> * Gebruik altijd [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) in plaats van `DateTime.UtcNow` zoals weergegeven in de volgende voorbeelden zijn bij het berekenen van een relatieve deadline van een duurzame timer.

## <a name="usage-for-delay"></a>Gebruik voor de vertraging

Het volgende voorbeeld laat zien hoe duurzame timers gebruiken voor het uitstellen van de uitvoering. In het voorbeeld verleent een facturering melding elke dag tien dagen.

#### <a name="c"></a>C#

```csharp
[FunctionName("BillingIssuer")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    for (int i = 0; i < 10; i++)
    {
        DateTime deadline = context.CurrentUtcDateTime.Add(TimeSpan.FromDays(1));
        await context.CreateTimer(deadline, CancellationToken.None);
        await context.CallActivityAsync("SendBillingEvent");
    }
}
```

#### <a name="javascript"></a>Javascript

```js
const df = require("durable-functions");
const moment = require("moment-js");

module.exports = df(function*(context) {
    for (let i = 0; i < 10; i++) {
        const dayOfMonth = context.df.currentUtcDateTime.getDate();
        const deadline = moment.utc(context.df.currentUtcDateTime).add(1, 'd');
        yield context.df.createTimer(deadline.toDate());
        yield context.df.callActivityAsync("SendBillingEvent");
    }
});
```

> [!WARNING]
> Vermijd oneindige lus in de orchestrator-functies. Zie voor meer informatie over het veilig en efficiënt oneindige lus scenario's implementeren [eeuwige integraties](durable-functions-eternal-orchestrations.md). 

## <a name="usage-for-timeout"></a>Gebruik voor de time-out

In dit voorbeeld laat zien hoe duurzame timers gebruiken voor het implementeren van time-outs.

#### <a name="c"></a>C#

```csharp
[FunctionName("TryGetQuote")]
public static async Task<bool> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("GetQuote");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

#### <a name="javascript"></a>Javascript

```js
const df = require("durable-functions");
const moment = require("moment-js");

module.exports = df(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, 's');

    const activityTask = context.df.callActivityAsync("GetQuote");
    const timeoutTask = context.df.createTimer(deadline);

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    }
    else
    {
        // timeout case
        return false;
    }
});
```

> [!WARNING]
> Gebruik een `CancellationTokenSource` annuleren van een duurzame timer (C#) of een aanroep van `cancel()` op de geretourneerde `TimerTask` (JavaScript) als uw code wordt niet wachten totdat deze is voltooid. Het duurzame Framework van de taak heeft geen invloed op een orchestration status ' voltooid ' totdat alle openstaande taken zijn voltooid of geannuleerd.

Dit mechanisme uitvoering van een activiteit in uitvoering functie niet daadwerkelijk worden beëindigd. In plaats daarvan gewoon kunt u de orchestrator-functie voor het negeren van het resultaat en op verplaatsen. Als de functie-app gebruikmaakt van het plan verbruik, wordt u nog steeds gefactureerd voor elke tijd en geheugen die door de functie afgebroken activiteit. Functies die worden uitgevoerd in het plan verbruik hebben standaard een time-out van vijf minuten. Als deze limiet wordt overschreden, wordt de host Azure Functions gerecycled alle uitvoering stoppen en te voorkomen dat een buitensporige facturering situatie. De [functie time-out is configureerbaar](functions-host-json.md#functiontimeout).

Zie voor een uitgebreidere voorbeeld van het implementeren van time-outs in de orchestrator-functies, de [menselijke tussenkomst & time-outs - verificatie via de telefoon](durable-functions-phone-verification.md) scenario.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het activeren en externe gebeurtenissen verwerken](durable-functions-external-events.md)

