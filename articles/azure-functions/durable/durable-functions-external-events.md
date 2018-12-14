---
title: Externe gebeurtenissen in duurzame functies - Azure verwerken
description: Informatie over het verwerken van externe gebeurtenissen in de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: fefbdffdeb3db86447038a3b3d4d24e8c7cd3803
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340433"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Externe gebeurtenissen in duurzame functies (Azure Functions) verwerken

Orchestrator-functies hebben de mogelijkheid om te wachten en luisteren naar externe gebeurtenissen. Deze functie van [duurzame functies](durable-functions-overview.md) is vaak handig voor het verwerken van menselijke tussenkomst of andere externe triggers.

## <a name="wait-for-events"></a>Wachten op gebeurtenissen

De [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) methode kunt u een orchestrator-functie voor het asynchroon wacht en luisteren naar een externe gebeurtenis. De luisterende orchestrator-functie verklaart de *naam* van de gebeurtenis en de *vorm van de gegevens* het verwacht te ontvangen.

### <a name="c"></a>C#

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

Het vorige voorbeeld luistert naar een specifieke enkelvoudige gebeurtenis en actie onderneemt wanneer die wordt ontvangen.

U kunt luisteren naar meerdere gebeurtenissen gelijktijdig, zoals in het volgende voorbeeld wordt gewacht op een van drie mogelijke gebeurtenismeldingen.

#### <a name="c"></a>C#

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const event1 = context.df.waitForExternalEvent("Event1");
    const event2 = context.df.waitForExternalEvent("Event2");
    const event3 = context.df.waitForExternalEvent("Event3");

    const winner = yield context.df.Task.any([event1, event2, event3]);
    if (winner === event1) {
        // ...
    } else if (winner === event2) {
        // ...
    } else if (winner === event3) {
        // ...
    }
});
```

Het vorige voorbeeld luistert naar *eventuele* van meerdere gebeurtenissen. Het is ook mogelijk om te wachten op *alle* gebeurtenissen.

#### <a name="c"></a>C#

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivity("IssueBuildingPermit", applicationId);
});
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) gewacht voor onbepaalde tijd op sommige invoer.  De functie-app kan worden veilig verwijderd tijdens het wachten. Als een gebeurtenis voor dit exemplaar orchestration binnenkomt, wordt automatisch geactiveerd en onmiddellijk de gebeurtenis wordt verwerkt.

> [!NOTE]
> Als uw functie-app gebruikmaakt van het verbruik van plan bent, geen kosten worden berekend terwijl een orchestrator-functie is in afwachting van een taak in de `WaitForExternalEvent` (.NET) of `waitForExternalEvent` (JavaScript), ongeacht hoe lang deze moet wachten.

In .NET, als de nettolading van de gebeurtenis kan niet worden omgezet in het verwachte type `T`, wordt er een uitzondering gegenereerd.

## <a name="send-events"></a>Gebeurtenissen verzenden

De [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse verzendt de gebeurtenissen die `WaitForExternalEvent` (.NET) of `waitForExternalEvent` wacht op (JavaScript).  De `RaiseEventAsync` methode duurt *eventName* en *eventData* als parameters. Gegevens van de gebeurtenis moet JSON-geserialiseerd.

Hieronder volgt een voorbeeld van de wachtrij geactiveerde functie die een gebeurtenis 'Goedkeuring' worden verzonden naar een exemplaar van de orchestrator-functie. De orchestration-exemplaar-ID zijn afkomstig uit de hoofdtekst van bericht uit de wachtrij.

### <a name="c"></a>C#

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    await client.raiseEvent(instanceId, "Approval", true);
};
```

Intern, `RaiseEventAsync` (.NET) of `raiseEvent` (JavaScript) enqueues een bericht dat wordt opgehaald door de orchestrator-functie voor wachten.

> [!WARNING]
> Als er geen orchestration-exemplaar met de opgegeven *exemplaar-ID* of als het exemplaar niet op de opgegeven wachten is *gebeurtenisnaam*, het gebeurtenisbericht wordt verwijderd. Zie voor meer informatie over dit gedrag, de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

> [!WARNING]
> Bij het ontwikkelen van lokaal in JavaScript, moet u de omgevingsvariabele instellen `WEBSITE_HOSTNAME` naar `localhost:<port>`, bijvoorbeeld. `localhost:7071` gebruik van methoden op `DurableOrchestrationClient`. Zie voor meer informatie over deze vereiste de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-js/issues/28).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het instellen van oneindige indelingen](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Een voorbeeld dat wordt gewacht externe gebeurtenissen uitvoeren](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [Een voorbeeld dat wordt gewacht menselijke tussenkomst uitvoeren](durable-functions-phone-verification.md)