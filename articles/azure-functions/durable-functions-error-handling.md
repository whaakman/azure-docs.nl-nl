---
title: Afhandeling van fouten in duurzame functies - Azure
description: Informatie over het afhandelen van fouten in de uitbreiding duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: ee5362d33bb9dadadb4194457cfd7726f4825f56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Afhandeling van fouten in duurzame functies (Azure-functies)

Duurzame functie integraties zijn geïmplementeerd in de code en de foutafhandeling mogelijkheden van de programmeertaal kunnen gebruiken. Met deze rekening er echt zijn niet alle nieuwe concepten, u moet meer informatie over de foutverwerking van de en compensatie opnemen in uw integraties. Er zijn echter enkele gedrag dat u houden moet rekening.

## <a name="errors-in-activity-functions"></a>Fouten in de functies van de activiteit

Elke uitzondering die is gegenereerd in een functie van de activiteit is samengevoegd terug naar de orchestrator-functie en gegenereerd als een `TaskFailedException`. U kunt schrijven voor de verwerking en compensatie foutcode die aansluit bij uw behoeften in de orchestrator-functie.

Neem bijvoorbeeld de volgende orchestrator-functie die middelen van één account naar een andere overzet:

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

Als de aanroep van de **CreditAccount** functie mislukt voor de doelaccount, de orchestrator-functie gecompenseerd voor dit door de middelen creditering terug naar de bronaccount.

## <a name="automatic-retry-on-failure"></a>Automatische nieuwe pogingen bij fouten

Als u functies van de activiteit aanroepen of subquery orchestration-functies kunt u automatisch beleid voor opnieuw proberen. Het volgende voorbeeld probeert een functie maximaal 3 keer aanroept en wacht 5 seconden tussen nieuwe pogingen:

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions);
    
    // ...
}
```

De `CallActivityWithRetryAsync` API duurt een `RetryOptions` parameter. Onderliggende orchestration-aanroepen met de `CallSubOrchestratorWithRetryAsync` API kunt gebruiken deze dezelfde beleid voor opnieuw proberen.

Er zijn verschillende opties voor het aanpassen van het beleid voor automatisch opnieuw proberen. Hieronder vallen de volgende landen:

* **Maximumaantal pogingen**: het maximum aantal nieuwe pogingen.
* **Interval voor eerste poging**: de hoeveelheid tijd moet worden gewacht voordat de eerste opnieuw proberen.
* **Coefficient backoff**: de coefficient gebruikt om te bepalen van de snelheid van de toename van backoff. De standaardwaarde is 1.
* **Interval voor opnieuw proberen Max**: de maximale hoeveelheid tijd moet wachten tussen nieuwe pogingen.
* **Probeer time-out**: de maximale hoeveelheid tijd te besteden doen opnieuw probeert. Er is het standaardgedrag voor onbepaalde tijd opnieuw.
* **Aangepaste**: een door de gebruiker gedefinieerde callback kan worden opgegeven waarmee wordt bepaald of een functieaanroep moet opnieuw worden geprobeerd.

## <a name="function-timeouts"></a>Functie time-outs

Mogelijk wilt u een functieaanroep binnen een orchestrator-functie afbreken als dit te lang duurt. De juiste manier om dit te doen vandaag is door het maken van een [duurzame timer](durable-functions-timers.md) met `context.CreateTimer` in combinatie met `Task.WhenAny`, zoals in het volgende voorbeeld:

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

## <a name="unhandled-exceptions"></a>Niet-verwerkte uitzonderingen

Als een orchestrator-functie is mislukt met een niet-verwerkte uitzondering, de details van de uitzondering wordt geregistreerd en het exemplaar is voltooid met een `Failed` status.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het analyseren van problemen](durable-functions-diagnostics.md)
