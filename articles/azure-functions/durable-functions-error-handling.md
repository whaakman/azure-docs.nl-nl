---
title: Afhandeling van fouten in duurzame functies - Azure
description: Leer hoe u voor het afhandelen van fouten in de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: azfuncdf
ms.openlocfilehash: 0b19fe7441d3c2c5222095c31d9c3677b8c9cf34
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092714"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Afhandeling van fouten in duurzame functies (Azure Functions)

Duurzame functie indelingen zijn geïmplementeerd in de code en de mogelijkheden voor foutafhandeling van de programmeertaal kunnen gebruiken. Met deze waarmee u rekening moet er echt zijn niet alle nieuwe concepten die u meer informatie wilt over wanneer foutafhandeling en compensatie opnemen in uw indelingen. Er zijn echter enkele problemen die u moet rekening houden met.

## <a name="errors-in-activity-functions"></a>Fouten in de activiteitsfuncties

Elke uitzondering die is gegenereerd in een functie van de activiteit is samengevoegd terug naar de orchestrator-functie en gegenereerd als een `FunctionFailedException`. U kunt schrijven voor de verwerking en compensatie foutcode die aansluit bij uw behoeften in de orchestrator-functie.

Bijvoorbeeld, houd rekening met de volgende orchestrator-functie die wordt overgedragen middelen van één account naar een andere:

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

Als de aanroep van de **CreditAccount** functie mislukt voor de doelaccount, de orchestrator-functie voor deze worden gecompenseerd door de bestaande fondsen creditering terug naar de bronaccount.

## <a name="automatic-retry-on-failure"></a>Automatische nieuwe pogingen bij fout

Als u activiteitsfuncties of subquery orchestration functies aanroept, kunt u een beleid voor automatische opnieuw proberen. Het volgende voorbeeld roept een functie maximaal drie keer en wacht vijf seconden tussen nieuwe pogingen:

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

De `CallActivityWithRetryAsync` API duurt een `RetryOptions` parameter. Suborchestration aanroept met behulp van de `CallSubOrchestratorWithRetryAsync` API kunt gebruiken deze dezelfde beleid voor opnieuw proberen.

Er zijn verschillende opties voor het aanpassen van het beleid voor automatische opnieuw proberen. Hieronder vallen de volgende landen:

* **Maximumaantal pogingen**: het maximum aantal nieuwe pogingen.
* **Eerste interval voor opnieuw proberen**: de hoeveelheid tijd moet worden gewacht voordat de eerste nieuwe poging.
* **Uitstel coëfficiënt**: de coëfficiënt gebruikt om te bepalen van de snelheid van de toename van uitstel. Standaard ingesteld op 1.
* **Interval voor opnieuw proberen van max**: de maximale hoeveelheid tijd moet wachten tussen nieuwe pogingen.
* **Opnieuw proberen**: de maximale hoeveelheid tijd te besteden aan dit nieuwe pogingen. Het standaardgedrag is voor onbepaalde tijd opnieuw uit te voeren.
* **Aangepaste**: een door de gebruiker gedefinieerde callback kunt opgeven dat bepaalt of een aanroep van de functie opnieuw moet worden uitgevoerd.

## <a name="function-timeouts"></a>Functie time-outs

Mogelijk wilt u een functieaanroep binnen een orchestrator-functie afbreken als duurt te lang om te voltooien. De juiste manier om dit te doen vandaag is door het maken van een [duurzame timer](durable-functions-timers.md) met behulp van `context.CreateTimer` in combinatie met `Task.WhenAny`, zoals in het volgende voorbeeld:

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

> [!NOTE]
> Dit mechanisme uitvoeren van de activiteit wordt uitgevoerd niet daadwerkelijk wordt beëindigd. In plaats daarvan kunt deze gewoon de orchestrator-functie voor het negeren van het resultaat en op verplaatsen. Zie voor meer informatie de [Timers](durable-functions-timers.md#usage-for-timeout) documentatie.

## <a name="unhandled-exceptions"></a>Niet-verwerkte uitzonderingen

Als een orchestrator-functie is mislukt met een niet-verwerkte uitzondering, de details van de uitzondering worden geregistreerd en het exemplaar is voltooid met een `Failed` status.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het analyseren van problemen](durable-functions-diagnostics.md)
