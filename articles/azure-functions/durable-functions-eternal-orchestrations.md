---
title: Eeuwige integraties in duurzame functies - Azure
description: Informatie over het implementeren van eeuwige integraties met behulp van de extensie duurzame functies voor Azure Functions.
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
ms.openlocfilehash: 1256e7f0286d9eb6ea6498b024fba41eb9f6a641
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Eeuwige integraties in duurzame functies (Azure-functies)

*Eeuwige integraties* zijn orchestrator-functies die nooit eindigen. Ze zijn nuttig wanneer u wilt gebruiken [duurzame functies](durable-functions-overview.md) voor lezers en een scenario waarbij een oneindige lus.

## <a name="orchestration-history"></a>Orchestration-geschiedenis

Zoals uitgelegd in [plaatsen van controlepunten en Replay](durable-functions-checkpointing-and-replay.md), het duurzame taak Framework houdt van de geschiedenis van elke functie orchestration. Dit overzicht uitbreiden continu zolang de orchestrator-functie nog steeds nieuwe werk plannen. Als de orchestrator-functie in een oneindige lus terechtkomt en werk continu wordt gepland, kan deze geschiedenis uiterste groot en aanzienlijke prestatieproblemen veroorzaken. De *eeuwige orchestration* concept is ontworpen voor het beperken van dit soort problemen voor toepassingen die oneindige lus moeten.

## <a name="resetting-and-restarting"></a>Opnieuw instellen en opnieuw starten

In plaats van oneindige lus, orchestrator-functies hun status herstellen door het aanroepen van de [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) methode. Deze methode heeft één JSON-serialiseerbaar parameter, de nieuwe invoer voor de volgende generatie van de orchestrator-functie wordt.

Wanneer `ContinueAsNew` wordt aangeroepen, het exemplaar enqueues een bericht naar zichzelf voordat deze wordt afgesloten. Het bericht opnieuw wordt opgestart met het exemplaar met de nieuwe waarde van de invoer. Dezelfde exemplaar-ID wordt bewaard, maar de geschiedenis van de orchestrator-functie effectief is afgekapt.

> [!NOTE]
> Het duurzame taak Framework blijft behouden dezelfde exemplaar-ID, maar intern maakt u een nieuwe *uitvoerings-ID* voor de orchestrator-functie die opnieuw wordt ingesteld door `ContinueAsNew`. Deze uitvoerings-ID in het algemeen niet beschikbaar is extern, maar het mogelijk handig te weten over wanneer de uitvoering van de orchestration-foutopsporing.

## <a name="periodic-work-example"></a>Voorbeeld van de periodieke werk

Een gebruiksvoorbeeld voor eeuwige integraties is code die behoeften voor periodieke werk voor onbepaalde tijd.

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer<string>(nextCleanup);

    context.ContinueAsNew(null);
}
```

Het verschil tussen het volgende voorbeeld en een functie timer geactiveerd is dat opschonen trigger keren hier niet zijn gebaseerd op een planning. Bijvoorbeeld, een CRON-schema dat wordt uitgevoerd van een functie elk uur wordt uitgevoerd het om 1:00 uur, 2:00 uur, 3:00 enz. en kan mogelijk worden uitgevoerd in problemen met elkaar overlappen. In dit voorbeeld echter als het opruimen van de 30 minuten duurt vervolgens wordt gepland om 1:00 uur, 2:30 4:00, enz. en er worden niet beïnvloed elkaar overlappen.

## <a name="counter-example"></a>Voorbeeld van de teller

Hier volgt een voorbeeld van een vereenvoudigde van een *teller* functie die naar luistert *verhoging* en *verlagen* eternally gebeurtenissen.

```csharp
[FunctionName("SimpleCounter")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    int counterState = context.GetInput<int>();

    string operation = await context.WaitForExternalEvent<string>("operation");

    if (operation == "incr")
    {
        counterState++;
    }
    else if (operation == "decr")
    {
        counterState--;
    }
    
    context.ContinueAsNew(counterState);
}
```

## <a name="exit-from-an-eternal-orchestration"></a>Een eeuwige orchestration afsluiten

Als een orchestrator-functie moet worden uiteindelijk voltooid, dan hoeft u *niet* aanroepen `ContinueAsNew` en laat u de functie sluiten.

Als een orchestrator-functie in een oneindige lus en moet worden gestopt, gebruikt de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) methode om deze te stoppen. Zie voor meer informatie [Instantiebeheer](durable-functions-instance-management.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van singleton-integraties](durable-functions-singletons.md)

> [!div class="nextstepaction"]
> [Voer een voorbeeldquery eeuwige orchestration](durable-functions-counter.md)
