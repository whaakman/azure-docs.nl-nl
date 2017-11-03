---
title: Singletons voor duurzame functies - Azure
description: Het gebruik van singletons in de uitbreiding duurzame Functons voor Azure Functions.
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
ms.openlocfilehash: 8384d17405653a29207cdfa4f6143504d0db2022
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton-orchestrators in duurzame functies (Azure-functies)

Voor achtergrondtaken of actor-stijl-integraties u vaak nodig om ervoor te zorgen dat slechts één exemplaar van een bepaalde orchestrator tegelijkertijd wordt uitgevoerd. Dit kan worden gedaan [duurzame functies](durable-functions-overview.md) door het toewijzen van een specifieke instantie-ID aan een orchestrator bij het maken van deze.

## <a name="singleton-example"></a>Singleton-voorbeeld

De volgende C#-voorbeeld ziet een HTTP-trigger-functie die een singleton achtergrond taak orchestration maakt. De code zorgt ervoor dat slechts één exemplaar bestaat voor een opgegeven exemplaar-ID.

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    TraceWriter log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.Info($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

Exemplaar-id's willekeurig zijn gegenereerd GUID's. Maar in dit geval de exemplaar-ID wordt doorgegeven in gegevens routeren van de URL. Het aanroepen van de code [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) om te controleren als er al een exemplaar met de opgegeven ID is uitgevoerd. Als dit niet het geval is, is een exemplaar gemaakt met die ID.

Details over de implementatie van de orchestrator-functie niet daadwerkelijk van belang. Het kan een reguliere orchestrator-functie die wordt gestart en is voltooid, of kan zijn dat wordt altijd uitgevoerd (dat wil zeggen, een [eeuwige Orchestration](durable-functions-eternal-orchestrations.md)). Wat belangrijk is, is er ooit slechts één exemplaar tegelijk uitvoeren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het aanroepen van onderliggende integraties](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Een voorbeeld singleton uitvoeren](durable-functions-counter.md)
