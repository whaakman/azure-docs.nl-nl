---
title: Singletons voor duurzame functies - Azure
description: Het gebruik van singletons in de duurzame Functons-extensie voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 8177fb6e8dea83ab2b8b12183cdcca6e43f92ade
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095093"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton-orchestrators in duurzame functies (Azure Functions)

Voor achtergrondtaken of actor-stijl-indelingen, u vaak nodig om ervoor te zorgen dat slechts één exemplaar van een bepaalde orchestrator tegelijkertijd wordt uitgevoerd. Dit kan worden gedaan [duurzame functies](durable-functions-overview.md) door het toewijzen van een specifiek exemplaar-ID voor een orchestrator bij het maken van deze.

## <a name="singleton-example"></a>Singleton-voorbeeld

De volgende C#-voorbeeld ziet u een HTTP-trigger-functie waarmee een Taakindeling voor singleton-achtergrond. De code zorgt ervoor dat slechts één exemplaar bestaat voor een opgegeven exemplaar-ID.

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

Exemplaar-id's willekeurig zijn gegenereerd standaard GUID's. Maar in dit geval de exemplaar-ID van de URL in de routegegevens wordt doorgegeven. De code roept [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) om te controleren als er al een exemplaar met de opgegeven ID is uitgevoerd. Als dit niet het geval is, is een exemplaar gemaakt met die ID.

> [!NOTE]
> Er is een mogelijke racevoorwaarde in dit voorbeeld. Als twee exemplaren van **HttpStartSingle** uitvoeren gelijktijdig, het resultaat kan worden twee verschillende gemaakt exemplaren van de singleton, die de andere worden overschreven. Dit kan neveneffecten hebben, afhankelijk van uw vereisten. Daarom is het belangrijk om ervoor te zorgen dat geen twee aanvragen gelijktijdig deze triggerfunctie kunnen uitvoeren.

Details over de implementatie van de orchestrator-functie niet werkelijk van belang. Dit kan een reguliere orchestrator-functie die wordt gestart en is voltooid, of kan de oorzaak zijn dat wordt altijd uitgevoerd (dat wil zeggen, een [eeuwige Orchestration](durable-functions-eternal-orchestrations.md)). Wat belangrijk is dat er altijd maar één instantie die wordt uitgevoerd op een tijdstip.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het aanroepen van onderliggende indelingen](durable-functions-sub-orchestrations.md)
