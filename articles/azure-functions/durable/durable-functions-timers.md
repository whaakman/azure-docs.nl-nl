---
title: Timers in duurzame functies - Azure
description: Informatie over het implementeren van duurzame timers in de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: azfuncdf
ms.openlocfilehash: 7af204ad76cb04c3d71c5108948be4036be1d1e4
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338835"
---
# <a name="timers-in-durable-functions-azure-functions"></a>Timers in duurzame functies (Azure Functions)

[Duurzame functies](durable-functions-overview.md) biedt *duurzame timers* voor gebruik in orchestrator-functies voor het implementeren van vertragingen of voor het instellen van time-outs op async-acties. Duurzame timers moeten worden gebruikt in de orchestrator-functies in plaats van `Thread.Sleep` en `Task.Delay` (C#) of `setTimeout()` en `setInterval()` (JavaScript).

U maakt een duurzame timer door het aanroepen van de [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) -methode van [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) in .NET of de `createTimer` -methode van `DurableOrchestrationContext` in JavaScript. De methode retourneert een taak die wordt hervat op een opgegeven datum en tijd.

## <a name="timer-limitations"></a>Beperkingen van timer

Bij het maken van een timer die verloopt: 30: 00 uur, de onderliggende duurzame taak Framework enqueues een bericht dat zichtbaar alleen om 4:30 uur wordt. Bij het uitvoeren van in het verbruiksabonnement voor Azure Functions, kan het timerbericht pas zichtbaar zorgt ervoor dat de functie-app op een geschikte virtuele machine wordt geactiveerd.

> [!NOTE]
> * Duurzame timers kunnen niet langer dan zeven dagen vanwege beperkingen in Azure Storage. Er wordt gewerkt aan een [functieaanvraag om uit te breiden timers langer dan 7 dagen](https://github.com/Azure/azure-functions-durable-extension/issues/14).
> * Gebruik altijd [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) in plaats van `DateTime.UtcNow` in .NET en `currentUtcDateTime` in plaats van `Date.now` of `Date.UTC` in JavaScript, zoals wordt weergegeven in de onderstaande voorbeelden bij het berekenen van een relatieve deadline van een duurzame timer .

## <a name="usage-for-delay"></a>Gebruik voor vertraging

Het volgende voorbeeld wordt het gebruik van duurzame timers voor het uitstellen van uitvoering. Het voorbeeld verleent een facturering melding elke dag van tien dagen.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```js
const df = require("durable-functions");
const moment = require("moment-js");

module.exports = df.orchestrator(function*(context) {
    for (let i = 0; i < 10; i++) {
        const dayOfMonth = context.df.currentUtcDateTime.getDate();
        const deadline = moment.utc(context.df.currentUtcDateTime).add(1, 'd');
        yield context.df.createTimer(deadline.toDate());
        yield context.df.callActivity("SendBillingEvent");
    }
});
```

> [!WARNING]
> Vermijd oneindige lus in de orchestrator-functies. Zie voor meer informatie over het implementeren van oneindige lus scenario's veilig en efficiënt [oneindige indelingen](durable-functions-eternal-orchestrations.md).

## <a name="usage-for-timeout"></a>Gebruik voor time-out

In dit voorbeeld laat zien hoe u van duurzame timers voor het implementeren van time-outs.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```js
const df = require("durable-functions");
const moment = require("moment-js");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("GetQuote");
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
> Gebruik een `CancellationTokenSource` om te annuleren een duurzame timer (C#) of een aanroep `cancel()` op de geretourneerde `TimerTask` (JavaScript) als uw code wordt niet wacht totdat deze is voltooid. Het duurzame Framework van de taak verandert niet de status van een indeling 'voltooid' totdat alle openstaande taken zijn voltooid of geannuleerd.

Dit mechanisme uitvoeren van de activiteit wordt uitgevoerd niet daadwerkelijk wordt beëindigd. In plaats daarvan kunt deze gewoon de orchestrator-functie voor het negeren van het resultaat en op verplaatsen. Als uw functie-app gebruikmaakt van het abonnement Consumption, wordt u nog steeds gefactureerd voor elke tijd en geheugen die wordt gebruikt door de functie verlaten activiteit. Functies die worden uitgevoerd in het abonnement Consumption hebben standaard een time-out van vijf minuten. Als deze limiet wordt overschreden, wordt de Azure Functions-host gerecycled uitvoering van alle stoppen en te voorkomen dat een overmatig facturering situatie. De [time-out van de functie is configureerbaar](../functions-host-json.md#functiontimeout).

Zie voor een meer diepgaande voorbeeld van het implementeren van time-outs in de orchestrator-functies, de [menselijke tussenkomst & time-outs - verificatie via de telefoon](durable-functions-phone-verification.md) scenario.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het verhogen en de verwerking van externe gebeurtenissen](durable-functions-external-events.md)