---
title: Functietypen en -functies in de extensie duurzame functies van Azure Functions
description: Meer informatie over de typen van functies en rollen die ondersteuning bieden voor functie communicatie in een indeling duurzame functies in Azure Functions.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 76b6f013333113d5a24b744bc962d36b1c0e21b3
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57455731"
---
# <a name="durable-functions-types-and-features-azure-functions"></a>Typen voor duurzame functies en onderdelen (Azure Functions)

Duurzame functies is een uitbreiding van [Azure Functions](../functions-overview.md). U kunt duurzame functies gebruiken voor stateful indeling van een functie wordt uitgevoerd. Een duurzame functie is een oplossing die uit verschillende Azure-functies bestaat. Functies kunnen verschillende rollen in een indeling duurzame functie afspelen. 

In dit artikel biedt een overzicht van de typen van functies die u in een indeling duurzame functies kunt gebruiken. Dit artikel bevat een aantal algemene patronen die u gebruiken kunt om functies. Leer hoe duurzame functies helpen u bij het oplossen van uw app ontwikkelingsuitdagingen.

![Een afbeelding die laat zien van de typen duurzame functies][1]  

## <a name="types-of-durable-functions"></a>Typen duurzame functies

U kunt drie duurzame functietypen gebruiken in Azure Functions: activiteit, de orchestrator en de client.

### <a name="activity-functions"></a>Activiteitsfuncties

Activiteit functions is de basiseenheid voor werk in een indeling duurzame functie. Activiteitsfuncties zijn de functies en taken die worden ingedeeld in het proces. U kunt bijvoorbeeld een duurzame functie voor het verwerken van een order maken. De taken betrekking hebben op het controleren van de voorraad, de klant in rekening gebracht en het maken van een verzending. Elke taak is een functie van de activiteit. 

Activiteitsfuncties zijn niet beperkt in het type werk dat u erin kunt doen. U kunt de functie van een activiteit schrijven in een [taal die ondersteuning bieden voor duurzame functies](durable-functions-overview.md#language-support). Het framework duurzame taak zorgt ervoor dat ten minste eenmaal opent gedurende een orchestration elke functie aangeroepen activiteit wordt uitgevoerd.

Gebruik een [activiteit trigger](durable-functions-bindings.md#activity-triggers) om een activiteit-functie te activeren. .NET-functies ontvangen een [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als parameter. U kunt ook de trigger binden aan een ander object om door te geven in de invoer voor de functie. In JavaScript, opent u een invoer via de `<activity trigger binding name>` eigenschap op de [ `context.bindings` object](../functions-reference-node.md#bindings).

De functie activiteit kunt ook waarden terugkeren naar de orchestrator. Als u een groot aantal waarden van de functie van een activiteit retourneren of verzendt, kunt u [tuples of matrices](durable-functions-bindings.md#passing-multiple-parameters). U kunt de functie van een activiteit alleen uit een exemplaar van de orchestration activeren. Hoewel de functie van een activiteit en een andere functie (zoals een HTTP-geactiveerde functie) code delen kunnen, kan elke functie slechts één trigger hebben.

Zie voor meer informatie en voorbeelden, [activiteitsfuncties](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestrator-functies

Orchestrator-functies wordt beschreven hoe acties worden uitgevoerd en de volgorde waarin acties worden uitgevoerd. Orchestrator-functies beschrijven de indelen in code (C# of JavaScript) zoals weergegeven in [duurzame functies patronen en technische concepten](durable-functions-concepts.md). Een indeling kan hebben veel verschillende soorten acties, met inbegrip van [activiteitsfuncties](#activity-functions), [onderliggende indelingen](#sub-orchestrations), [wachten op externe gebeurtenissen](#external-events), en [timers](#durable-timers). 

Een orchestrator-functie moet worden geactiveerd door een [orchestration trigger](durable-functions-bindings.md#orchestration-triggers).

Een orchestrator wordt gestart door een [orchestrator client](#client-functions). U kunt de orchestrator van elke bron (http-, wachtrij, gebeurtenisstroom) activeren. Elk exemplaar van een indeling heeft een exemplaar-id. De exemplaar-id kan automatisch gegenereerde (aanbevolen) of door de gebruiker is gegenereerd. U kunt de exemplaar-id aan [exemplaren beheren](durable-functions-instance-management.md) van de indeling.

Zie voor meer informatie en voorbeelden, [Orchestration triggers](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Clientfuncties

Clientfuncties zijn de nieuwe instanties van een indeling geactiveerde functies. Clientfuncties zijn het uitgangspunt voor het maken van een exemplaar van een indeling duurzame functies. U kunt een client-functie van elke bron (http-, wachtrij, gebeurtenisstroom) activeren. U kunt een functie van de client schrijven in elke taal die ondersteuning biedt voor de app. 

Clientfuncties hebben ook een [orchestration-client](durable-functions-bindings.md#orchestration-client) binding. Een client-functie kunnen de orchestration-client binding maken en beheren van duurzame indelingen worden gebruikt. 

De meest eenvoudige voorbeeld van een client-functie is een HTTP-geactiveerde functie die een orchestrator-functie wordt gestart en retourneert vervolgens een reactie op de status. Zie voor een voorbeeld [HTTP API-URL detectie](durable-functions-http-api.md#http-api-url-discovery).

Zie voor meer informatie en voorbeelden, [Orchestration-client](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Functies en -patronen

De volgende secties worden de functies en -patronen van duurzame functies typen.

### <a name="sub-orchestrations"></a>Onderliggende indelingen

Orchestrator-functies kunnen aanroepen activiteitsfuncties, maar ze ook andere orchestrator-functies kunnen aanroepen. U kunt bijvoorbeeld een grotere orchestration uit een bibliotheek van orchestrator-functies bouwen. Of u kunt meerdere exemplaren van een orchestrator-functie parallel uitvoeren.

Zie voor meer informatie en voorbeelden, [onderliggende indelingen](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Duurzame timers

[Duurzame functies](durable-functions-overview.md) biedt *duurzame timers* die u in de orchestrator-functies kunt gebruiken voor het implementeren van vertragingen of voor het instellen van time-outs op async-acties. Duurzame timers gebruiken in de orchestrator-functies in plaats van `Thread.Sleep` en `Task.Delay` (C#) of `setTimeout()` en `setInterval()` (JavaScript).

Zie voor meer informatie en voorbeelden, [duurzame timers](durable-functions-timers.md).

### <a name="external-events"></a>Externe gebeurtenissen

Orchestrator-functies kunnen wachten op externe gebeurtenissen bijwerken van een orchestration-exemplaar. Deze functie duurzame functies is vaak handig voor het afhandelen van een menselijke tussenkomst of andere externe callbacks.

Zie voor meer informatie en voorbeelden, [externe gebeurtenissen](durable-functions-external-events.md).

### <a name="error-handling"></a>Foutafhandeling

Gebruik code voor het implementeren van duurzame functies indelingen. U kunt de foutafhandeling functies van de programmeertaal gebruiken. Patronen, zoals `try` / `catch` werk in uw orchestration. 

Duurzame functies worden ook geleverd met beleid voor automatisch opnieuw geprobeerd. Een actie kan vertraging en activiteiten automatisch opnieuw wanneer er een uitzondering optreedt. U kunt nieuwe pogingen voor het afhandelen van tijdelijke uitzonderingen zonder dat de indeling wordt afgebroken.

Zie voor meer informatie en voorbeelden, [foutafhandeling](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Cross-functie-app-communicatie

Hoewel een duurzame indeling in de context van een enkele functie-app wordt uitgevoerd, kunt u patronen gebruiken voor de coördinatie van indelingen voor veel functie-apps. Communicatie van de verschillende Apps optreden via HTTP, maar kunt u nog steeds een duurzame proces in twee apps beheren met behulp van de duurzame framework voor elke activiteit betekent.

De volgende voorbeelden ziet u cross-functie-app indelen in C# en JavaScript. In elk voorbeeld wordt de externe orchestration gestart door één activiteit. Een andere activiteit opgehaald en wordt de status. De orchestrator wacht tot de status moet `Complete` voordat deze wordt hervat.

Hier volgen enkele voorbeelden van cross-functie-app-indeling:

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration.
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now, go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    // Do some work...

    // Call a remote orchestration.
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now, go do more work...
});
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, orchestratorName) {
    const options = {
        method: "POST",
        uri: `https://appB.azurewebsites.net/orchestrations/${orchestratorName}`,
        body: ""
    };

    const statusUrl = await request(options);
    return statusUrl;
};
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, statusUrl) {
    const options = {
        method: "GET",
        uri: statusUrl,
        resolveWithFullResponse: true,
    };

    const response = await request(options);
    // 200 = Complete, 202 = Running
    return response.statusCode === 200;
};
```

## <a name="next-steps"></a>Volgende stappen

Maak uw eerste duurzame functie in om te beginnen, [ C# ](durable-functions-create-first-csharp.md) of [JavaScript](quickstart-js-vscode.md).

> [!div class="nextstepaction"]
> [Lees meer over duurzame functies](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
