---
title: Overzicht van de functietypen en -functies voor duurzame functies - Azure
description: Meer informatie over de typen van functies en functies die voor de communicatie functie als onderdeel van een indeling duurzame functie zorgen.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 07/04/2018
ms.author: azfuncdf
ms.openlocfilehash: 24889765369a2af7d87ddb0fdc8f7c9f91ac66fd
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091388"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>Overzicht van de functietypen en -functies voor duurzame functies (Azure Functions)

Azure Duurzame functies zorgt voor een stateful indeling van een functie wordt uitgevoerd.  Een duurzame functie is een oplossing die bestaat uit verschillende Azure-functies.  Elk van deze functies kan verschillende rollen als onderdeel van een indeling worden afgespeeld.  Het volgende document bevat een overzicht van de typen van functies die betrokken zijn bij een duurzame functie-indeling.  Dit omvat ook sommige algemene patronen in het met elkaar verbinden van functies.  

![Typen duurzame functies][1]  

## <a name="types-of-functions"></a>Typen functies

### <a name="activity-functions"></a>Activiteitsfuncties

Activiteit functions is de basiseenheid voor werk in een duurzame orchestration.  Activiteitsfuncties zijn de functies en taken die in het proces wordt georganiseerd.  U kunt bijvoorbeeld een duurzame functie maken om te verwerken van een order - Controleer de voorraad, de klant in rekening en maken van een verzending.  Elk van deze taken een zijn een functie van de activiteit.  Activiteitsfuncties geen beperkingen in het type werk dat u erin kunt doen.  Ze kunnen worden geschreven in elke taal die wordt ondersteund door Azure Functions.  Het framework duurzame taak zorgt ervoor dat ten minste eenmaal opent gedurende een orchestration elke functie aangeroepen activiteit wordt uitgevoerd.

De functie van een activiteit moet worden geactiveerd door een [activiteit trigger](durable-functions-bindings.md#activity-triggers).  Deze functie ontvangt een [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als parameter. U kunt ook de trigger binden aan een ander object om door te geven in de invoer voor de functie.  De functie activiteit kan ook waarden retourneren terug naar de orchestrator.  Als het verzenden of die veel waarden van een functie van de activiteit retourneert, kunt u [gebruikmaken van tuples of matrices](durable-functions-bindings.md#passing-multiple-parameters).  Activiteitsfuncties kunnen alleen worden geactiveerd vanuit een orchestration-exemplaar.  Terwijl de code kan worden gedeeld tussen een functie van de activiteit en een andere functie (zoals een door HTTP geactiveerde functie), kan elke functie slechts één trigger hebben.

Meer informatie over en voorbeelden vindt u de [duurzame functies binding artikel](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestrator-functies

Orchestrator-functies zijn de kern van een duurzame functie.  Orchestrator-functies worden beschreven de manier en de volgorde die acties worden uitgevoerd.  Orchestrator-functies beschrijven de indelen in code (C# of JavaScript), zoals wordt weergegeven in de [duurzame functies overzicht](durable-functions-overview.md).  Een indeling kunt hebben verschillende soorten acties, zoals [activiteitsfuncties](#activity-functions), [onderliggende indelingen](#sub-orchestrations), [wachten op externe gebeurtenissen](#external-events), en [ timers](#durable-timers).  

Een orchestrator-functie moet worden geactiveerd door een [orchestration trigger](durable-functions-bindings.md#orchestration-triggers).

Een orchestrator wordt gestart door een [orchestrator client](#client-functions) die kan zelf worden geactiveerd vanuit een bron (http-, wachtrijen, gebeurtenisstromen).  Elk exemplaar van een indeling heeft een exemplaar-id, die kan worden automatisch gegenereerde (aanbevolen) of door gebruikers gegenereerde.  Deze id kan worden gebruikt om [exemplaren beheren](durable-functions-instance-management.md) van de indeling.

Meer informatie over en voorbeelden vindt u de [duurzame functies binding artikel](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Clientfuncties

Clientfuncties zijn de geactiveerde functies u nieuwe instanties van een indeling maakt.  Ze zijn het uitgangspunt voor het maken van een exemplaar van een duurzame orchestration.  Clientfuncties kunnen worden geactiveerd door een trigger (HTTP, wachtrijen, gebeurtenisstreams, enzovoort) en die zijn geschreven in elke taal die wordt ondersteund door de app.  Naast de trigger, clientfuncties hebben een [orchestration-client](durable-functions-bindings.md#orchestration-client) die binding kunt maken en beheren van duurzame indelingen.  De meest eenvoudige voorbeeld van een client-functie is een door HTTP geactiveerde functie die een orchestrator-functie wordt gestart en retourneert een reactie op de status als [wordt weergegeven in het volgende voorbeeld](durable-functions-http-api.md#http-api-url-discovery).

Meer informatie over en voorbeelden vindt u de [duurzame functies binding artikel](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Functies en -patronen

### <a name="sub-orchestrations"></a>Onderliggende indelingen

Naast het aanroepen van activiteitsfuncties, kunnen orchestrator-functies aanroepen van andere orchestrator-functies. U kunt bijvoorbeeld een grotere orchestration uit een bibliotheek van orchestrator-functies bouwen. Of u kunt meerdere exemplaren van een orchestrator-functie parallel uitvoeren.

Meer informatie over en voorbeelden vindt u de [onderliggende orchestration artikel](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Duurzame timers

[Duurzame functies](durable-functions-overview.md) biedt *duurzame timers* voor gebruik in orchestrator-functies voor het implementeren van vertragingen of voor het instellen van time-outs op async-acties. Duurzame timers moeten worden gebruikt in de orchestrator-functies in plaats van `Thread.Sleep` en `Task.Delay` (C#) of `setTimeout()` en `setInterval()` (JavaScript).

Meer informatie over en voorbeelden van duurzame timers kunnen worden gevonden in de [duurzame timers artikel](durable-functions-timers.md)

### <a name="external-events"></a>Externe gebeurtenissen

Orchestrator-functies kunnen wachten op externe gebeurtenissen bijwerken van een orchestration-exemplaar. Deze functie van duurzame functies is vaak handig voor het afhandelen van menselijke tussenkomst of andere externe callbacks.

Meer informatie over en voorbeelden vindt u de [externe gebeurtenissen artikel](durable-functions-external-events.md).

### <a name="error-handling"></a>Foutafhandeling

Duurzame functie indelingen zijn geïmplementeerd in de code en de foutafhandeling functies van de programmeertaal kunnen gebruiken.  Dit betekent dat patronen, zoals "try/catch" in uw orchestration werkt.  Duurzame functies worden ook geleverd met enkele beleidsregels automatisch opnieuw geprobeerd.  Een actie kan vertraging en probeer activiteiten automatisch over uitzonderingen.  Nieuwe pogingen kunnen u voor het afhandelen van tijdelijke uitzonderingen zonder te hoeven verlaten van de indeling.

Meer informatie over en voorbeelden vindt u de [foutafhandeling artikel](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Cross-functie-app-communicatie

Een duurzame orchestration is algemeen zich bevinden in een context van een enkele functie-app, maar er zijn patronen waarmee u kunt de indelingen coördinatie van veel functie-apps.  Hoewel de communicatie van de verschillende Apps kan dat gebeurt via HTTP, met behulp van de duurzame framework voor elke activiteit betekent dat u kunt nog steeds een duurzame proces onderhouden in twee apps.

Hieronder vindt u een voorbeeld van een cross-functie-app-indeling in C#.  De externe orchestration, één activiteit wordt gestart. Een andere activiteit wordt vervolgens ophalen en de status retourneren.  De orchestrator wacht tot de status om te worden voltooid voordat u doorgaat.

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now go do more work...
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

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Lees ook documentatie duurzame functies](durable-functions-bindings.md)

> [!div class="nextstepaction"]
> [Installeer de extensie duurzame functies en -voorbeelden](durable-functions-install.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
