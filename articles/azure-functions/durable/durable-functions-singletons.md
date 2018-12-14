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
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: b083b9a09b478ca5ad68e19d3a2133fb529da851
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342949"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton-orchestrators in duurzame functies (Azure Functions)

Voor achtergrondtaken of actor-stijl-indelingen, u vaak nodig om ervoor te zorgen dat slechts één exemplaar van een bepaalde orchestrator tegelijkertijd wordt uitgevoerd. Dit kan worden gedaan [duurzame functies](durable-functions-overview.md) door het toewijzen van een specifiek exemplaar-ID voor een orchestrator bij het maken van deze.

## <a name="singleton-example"></a>Singleton-voorbeeld

De volgende C# en een HTTP-trigger-functie waarmee een singleton achtergrond-Taakindeling enkele voorbeelden van JavaScript. De code zorgt ervoor dat slechts één exemplaar bestaat voor een opgegeven exemplaar-ID.

### <a name="c"></a>C#

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionsName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

Exemplaar-id's willekeurig zijn gegenereerd standaard GUID's. Maar in dit geval de exemplaar-ID van de URL in de routegegevens wordt doorgegeven. De code roept [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) (C#) of `getStatus` (JavaScript) om te controleren als er al een exemplaar met de opgegeven ID is uitgevoerd. Als dit niet het geval is, is een exemplaar gemaakt met die ID.

> [!WARNING]
> Bij het ontwikkelen van lokaal in JavaScript, moet u de omgevingsvariabele instellen `WEBSITE_HOSTNAME` naar `localhost:<port>`, bijvoorbeeld. `localhost:7071` gebruik van methoden op `DurableOrchestrationClient`. Zie voor meer informatie over deze vereiste de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-js/issues/28).

> [!NOTE]
> Er is een mogelijke racevoorwaarde in dit voorbeeld. Als twee exemplaren van **HttpStartSingle** uitvoeren gelijktijdig, het resultaat kan worden twee verschillende gemaakt exemplaren van de singleton, die de andere worden overschreven. Dit kan neveneffecten hebben, afhankelijk van uw vereisten. Daarom is het belangrijk om ervoor te zorgen dat geen twee aanvragen gelijktijdig deze triggerfunctie kunnen uitvoeren.

Details over de implementatie van de orchestrator-functie niet werkelijk van belang. Dit kan een reguliere orchestrator-functie die wordt gestart en is voltooid, of kan de oorzaak zijn dat wordt altijd uitgevoerd (dat wil zeggen, een [eeuwige Orchestration](durable-functions-eternal-orchestrations.md)). Wat belangrijk is dat er altijd maar één instantie die wordt uitgevoerd op een tijdstip.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het aanroepen van onderliggende indelingen](durable-functions-sub-orchestrations.md)
