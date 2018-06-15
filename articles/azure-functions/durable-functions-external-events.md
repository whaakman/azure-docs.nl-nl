---
title: Verwerken van externe gebeurtenissen in duurzame functies - Azure
description: Informatie over het verwerken van externe gebeurtenissen in de uitbreiding duurzame functies voor Azure Functions.
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
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 77087f04ea641c24a92edd2091432cbcb4329ecd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763192"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Verwerken van externe gebeurtenissen in duurzame functies (Azure-functies)

Orchestrator-functies hebben de mogelijkheid om te wachten en het luisteren naar externe gebeurtenissen. Deze functie van [duurzame functies](durable-functions-overview.md) is het vaak nuttig voor het verwerken van menselijke tussenkomst of andere externe triggers.

## <a name="wait-for-events"></a>Wachten op gebeurtenissen

De [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) methode biedt de mogelijkheid een orchestrator-functie asynchroon wachten en het luisteren naar een externe gebeurtenis. De luisterende orchestrator-functie verklaart de *naam* van de gebeurtenis en de *vorm van de gegevens* deze verwacht te ontvangen.

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

Het vorige voorbeeld luistert naar een specifieke enkelvoudige gebeurtenis en actie wordt uitgevoerd wanneer het ontvangen.

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
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

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivityAsync("IssueBuildingPermit", applicationId);
});
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) voor onbepaalde tijd gewacht op sommige invoer.  De functie-app kan worden veilig verwijderd tijdens het wachten. Als een gebeurtenis voor dit exemplaar orchestration binnenkomt, wordt automatisch geactiveerd en onmiddellijk de gebeurtenis wordt verwerkt.

> [!NOTE]
> Als de functie-app het verbruik van plan bent gebruikt, geen kosten verbonden zijn terwijl een orchestrator-functie in afwachting van een taak vanuit `WaitForExternalEvent`, ongeacht hoe lang deze wacht.

In .NET, als de nettolading kan niet worden geconverteerd naar het verwachte type `T`, er een uitzondering gegenereerd.

## <a name="send-events"></a>Gebeurtenissen verzenden

De [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse verzendt de gebeurtenissen die `WaitForExternalEvent` wordt gewacht op.  De `RaiseEventAsync` methode vergt *eventName* en *eventData* als parameters. Gegevens van de gebeurtenis moet JSON-serialiseerbaar zijn.

Hieronder volgt een voorbeeld van de wachtrij geactiveerd functie die een gebeurtenis 'Goedkeuring' naar een exemplaar van de functie orchestrator verzendt. De exemplaar-ID orchestration afkomstig van de hoofdtekst van bericht in de wachtrij.

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

Intern `RaiseEventAsync` enqueues een bericht dat wordt opgehaald door de orchestrator-functie wachten.

> [!WARNING]
> Als er geen orchestration-exemplaar met de opgegeven is *instantie-ID* of als het exemplaar niet op de opgegeven wachten is *gebeurtenisnaam*, het gebeurtenisbericht is verwijderd. Zie voor meer informatie over dit gedrag, de [GitHub probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het instellen van eeuwige integraties](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Uitvoeren van een steekproef die wordt gewacht op externe gebeurtenissen](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [Uitvoeren van een steekproef die wordt gewacht op menselijke tussenkomst](durable-functions-phone-verification.md)

