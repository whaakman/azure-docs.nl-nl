---
title: Oneindige indelingen in duurzame functies - Azure
description: Informatie over het implementeren van oneindige indelingen met behulp van de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 7e92b4a10036ceffd2b777e8ce75d58615209fe6
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340196"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Oneindige indelingen in duurzame functies (Azure Functions)

*Oneindige indelingen* orchestrator-functies die nooit eindigen zijn. Ze zijn handig als u wilt gebruiken [duurzame functies](durable-functions-overview.md) voor lezers en elk scenario waarvoor een oneindige lus.

## <a name="orchestration-history"></a>Orchestration-geschiedenis

Zoals uitgelegd in [plaatsen van controlepunten en herhaling](durable-functions-checkpointing-and-replay.md), het duurzame taak Framework houdt van de geschiedenis van elke functie-indeling. Dit overzicht groeit voortdurend, mits de orchestrator-functie nog steeds nieuwe werk plannen. Als de orchestrator-functie in een oneindige lus gaat en continu wordt gepland werk, kan deze geschiedenis zeer groot en leiden tot prestatieproblemen ondervinden. De *eeuwige orchestration* concept is ontworpen om dit soort problemen voor toepassingen waarvoor een oneindige lus.

## <a name="resetting-and-restarting"></a>Opnieuw in te stellen en opnieuw starten

In plaats van oneindige lus, orchestrator-functies hun status herstellen door het aanroepen van de [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) methode. Deze methode heeft één JSON-geserialiseerd parameter, die de nieuwe invoer voor de volgende generatie van de orchestrator-functie wordt.

Wanneer `ContinueAsNew` wordt aangeroepen, de enqueues exemplaar van een bericht naar zichzelf voordat deze wordt afgesloten. Het bericht opnieuw wordt opgestart met het exemplaar met de nieuwe waarde voor de invoer. De dezelfde exemplaar-ID wordt bewaard, maar de geschiedenis van de orchestrator-functie effectief is afgekapt.

> [!NOTE]
> Het duurzame taak Framework onderhoudt de dezelfde exemplaar-ID, maar intern maakt u een nieuwe *uitvoerings-ID* voor de orchestrator-functie die opnieuw wordt ingesteld door `ContinueAsNew`. Deze uitvoerings-ID in het algemeen niet beschikbaar is extern, maar mogelijk handig om te weten over het opsporen van fouten in indeling kan worden uitgevoerd.

## <a name="periodic-work-example"></a>Voorbeeld van de periodieke werk

Een use-case voor oneindige indelingen is code die moet periodiek om werk te doen voor onbepaalde tijd.

### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup);

    context.df.continueAsNew(undefined);
});
```

Het verschil tussen het volgende voorbeeld en een timer geactiveerde-functie is dat opschonen trigger tijden hier niet zijn gebaseerd op een schema. Bijvoorbeeld een CRON-planning die wordt uitgevoerd van een functie elk uur wordt uitgevoerd om 1:00 uur, 2:00 uur, 3:00 enz. en kan mogelijk overlappende problemen ondervindt. In dit voorbeeld, echter, als het opruimen van de 30 minuten in beslag neemt vervolgens wordt gepland om 1:00 uur, 2:30, 4:00, enz. en er is geen kans van elkaar overlappen.

## <a name="exit-from-an-eternal-orchestration"></a>Afsluiten van een oneindige indeling

Als een orchestrator-functie moet worden uiteindelijk is voltooid, dan hoeft u alleen maar *niet* aanroepen `ContinueAsNew` en laat u de functie exit.

Als een orchestrator-functie in een oneindige lus en moet worden gestopt, gebruikt u de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) methode om deze te stoppen. Zie voor meer informatie, [Instantiebeheer](durable-functions-instance-management.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van enkelvoudige indelingen](durable-functions-singletons.md)
