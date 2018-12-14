---
title: Afhandeling van fouten in duurzame functies - Azure
description: Leer hoe u voor het afhandelen van fouten in de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 7a55e28f34f36cd02b67e56c6262b9e1f06dde8f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338189"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Afhandeling van fouten in duurzame functies (Azure Functions)

Duurzame functie indelingen zijn geïmplementeerd in de code en de mogelijkheden voor foutafhandeling van de programmeertaal kunnen gebruiken. Met deze waarmee u rekening moet er echt zijn niet alle nieuwe concepten die u meer informatie wilt over wanneer foutafhandeling en compensatie opnemen in uw indelingen. Er zijn echter enkele problemen die u moet rekening houden met.

## <a name="errors-in-activity-functions"></a>Fouten in de activiteitsfuncties

Elke uitzondering die is gegenereerd in een functie van de activiteit is samengevoegd terug naar de orchestrator-functie en gegenereerd als een `FunctionFailedException`. U kunt schrijven voor de verwerking en compensatie foutcode die aansluit bij uw behoeften in de orchestrator-functie.

Bijvoorbeeld, houd rekening met de volgende orchestrator-functie die wordt overgedragen middelen van één account naar een andere:

### <a name="c"></a>C#

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        {
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const transferDetails = context.df.getInput();

    yield context.df.callActivity("DebitAccount",
        {
            account = transferDetails.sourceAccount,
            amount = transferDetails.amount,
        }
    );

    try {
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.destinationAccount,
                amount = transferDetails.amount,
            }
        );
    }
    catch (error) {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.sourceAccount,
                amount = transferDetails.amount,
            }
        );
    }
});
```

Als de aanroep van de **CreditAccount** functie mislukt voor de doelaccount, de orchestrator-functie voor deze worden gecompenseerd door de bestaande fondsen creditering terug naar de bronaccount.

## <a name="automatic-retry-on-failure"></a>Automatische nieuwe pogingen bij fout

Als u activiteitsfuncties of subquery orchestration functies aanroept, kunt u een beleid voor automatische opnieuw proberen. Het volgende voorbeeld roept een functie maximaal drie keer en wacht vijf seconden tussen nieuwe pogingen:

### <a name="c"></a>C#

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const retryOptions = new df.RetryOptions(5000, 3);

    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

De `CallActivityWithRetryAsync` (.NET) of `callActivityWithRetry` (JavaScript) API heeft een `RetryOptions` parameter. Suborchestration aanroept met behulp van de `CallSubOrchestratorWithRetryAsync` (.NET) of `callSubOrchestratorWithRetry` (JavaScript) API kunt gebruiken deze dezelfde beleid voor opnieuw proberen.

Er zijn verschillende opties voor het aanpassen van het beleid voor automatische opnieuw proberen. Hieronder vallen de volgende landen:

* **Maximumaantal pogingen**: Het maximale aantal nieuwe pogingen.
* **Eerste interval voor opnieuw proberen**: De hoeveelheid tijd moet worden gewacht voordat de eerste nieuwe poging.
* **Uitstel coëfficiënt**: De coëfficiënt gebruikt om te bepalen van de snelheid van de toename van uitstel. Standaard ingesteld op 1.
* **Interval voor opnieuw proberen van max**: De maximale hoeveelheid tijd moet wachten tussen nieuwe pogingen.
* **Opnieuw proberen**: De maximale hoeveelheid tijd te besteden aan dit nieuwe pogingen. Het standaardgedrag is voor onbepaalde tijd opnieuw uit te voeren.
* **Verwerken**: Een door de gebruiker gedefinieerde callback kunt opgeven dat bepaalt of een aanroep van de functie opnieuw moet worden uitgevoerd.

## <a name="function-timeouts"></a>Functie time-outs

Mogelijk wilt u een functieaanroep binnen een orchestrator-functie afbreken als duurt te lang om te voltooien. De juiste manier om dit te doen vandaag is door het maken van een [duurzame timer](durable-functions-timers.md) met behulp van `context.CreateTimer` (.NET) of `context.df.createTimer` (JavaScript) in combinatie met `Task.WhenAny` (.NET) of `context.df.Task.any` (JavaScript), zoals in het volgende voorbeeld:

### <a name="c"></a>C#

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
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

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("FlakyFunction");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    } else {
        // timeout case
        return false;
    }
});
```

> [!NOTE]
> Dit mechanisme uitvoeren van de activiteit wordt uitgevoerd niet daadwerkelijk wordt beëindigd. In plaats daarvan kunt deze gewoon de orchestrator-functie voor het negeren van het resultaat en op verplaatsen. Zie voor meer informatie de [Timers](durable-functions-timers.md#usage-for-timeout) documentatie.

## <a name="unhandled-exceptions"></a>Onverwerkte uitzonderingen

Als een orchestrator-functie is mislukt met een niet-verwerkte uitzondering, de details van de uitzondering worden geregistreerd en het exemplaar is voltooid met een `Failed` status.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het analyseren van problemen](durable-functions-diagnostics.md)
