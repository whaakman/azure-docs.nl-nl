---
title: -Exemplaren in duurzame functies - Azure beheren
description: Informatie over het beheren van exemplaren in de uitbreiding duurzame functies voor Azure Functions.
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
ms.openlocfilehash: cbf7731c0faa82ebd3e662eb6d2a8fb0acd65c97
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>-Exemplaren in duurzame functies (Azure Functions) beheren

[Duurzame functies](durable-functions-overview.md) orchestration exemplaren kunnen worden gestart, is beëindigd, opgevraagd en meldingsgebeurtenissen verzonden. Alle exemplaar management wordt uitgevoerd met behulp van de [orchestration client binding](durable-functions-bindings.md). In dit artikel gaat naar de details van elke bewerking voor het beheer van exemplaar.

## <a name="starting-instances"></a>Exemplaren wordt gestart

De [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) methode op de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) start een nieuw exemplaar van een orchestrator-functie. Instanties van deze klasse kunnen worden opgehaald met behulp van de `orchestrationClient` binding. Intern maakt de enqueues van deze methode een bericht in de wachtrij besturingselement, die vervolgens activeert het begin van een functie met de opgegeven naam die gebruikmaakt van de `orchestrationTrigger` binding activeren.

De parameters voor [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) zijn als volgt:

* **Naam**: de naam van de orchestrator-functie te plannen.
* **Invoer**: JSON-serialiseerbaar gegevens die moeten worden doorgegeven als invoer voor de orchestrator-functie.
* **InstanceId**: (optioneel) de unieke ID van het exemplaar. Als niet wordt opgegeven, wordt er een willekeurig exemplaar-ID gegenereerd.

Dit is een eenvoudig C#-voorbeeld:

```csharp
[FunctionName("HelloWorldManualStart")]
public static Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Voor niet-.NET-talen, de functie binding uitvoer kan worden gebruikt voor nieuwe exemplaren ook starten. In dit geval kan een JSON-geserialiseerd object met de bovenstaande drie parameters als velden worden gebruikt. Neem bijvoorbeeld de volgende Node.js-functie:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```

> [!NOTE]
> Het is raadzaam dat u een willekeurige id voor de exemplaar-ID gebruiken. Dit helpt zorgen voor een distributiepunt gelijk load wanneer orchestrator-functies schalen op meerdere virtuele machines. De juiste tijd voor het gebruik van niet-willekeurig exemplaar-id is als de ID afkomstig van een externe bron zijn moet of bij het implementeren van de [singleton orchestrator](durable-functions-singletons.md) patroon.

## <a name="querying-instances"></a>Opvragen van exemplaren

De [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) methode op de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse de status van een exemplaar van de orchestration opgevraagd. Het duurt een `instanceId` als een parameter en retourneert een object met de volgende eigenschappen:

* **Naam**: de naam van de orchestrator-functie.
* **InstanceId**: de exemplaar-ID van de orchestration (moet hetzelfde zijn als de `instanceId` invoer).
* **CreatedTime**: de tijd waarop de orchestrator-functie is gestart.
* **LastUpdatedTime**: het tijdstip waarop de orchestration laatste controlepunt.
* **Invoer**: de invoer van de functie als een JSON-waarde.
* **Uitvoer**: de uitvoer van de functie als een JSON-waarde (als de functie is voltooid). Als de orchestrator-functie is mislukt, wordt deze eigenschap de foutgegevens bevatten. Als de orchestrator-functie is beëindigd, bevat deze eigenschap de opgegeven reden voor de beëindiging (indien aanwezig).
* **RuntimeStatus**: een van de volgende waarden:
    * **Met**: het exemplaar eenmaal is gestart.
    * **Voltooid**: het exemplaar normaal is voltooid.
    * **ContinuedAsNew**: het exemplaar zelf een nieuwe geschiedenis is opgestart. Dit is een tijdelijke status.
    * **Kan geen**: het exemplaar is mislukt met een fout opgetreden.
    * **Beëindigd**: het exemplaar is onverwacht beëindigd.
    
Deze methode retourneert `null` als het exemplaar bestaat niet of is nog niet begonnen uitgevoerd.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

> [!NOTE]
> Instantie-query is momenteel alleen ondersteund voor C# orchestrator-functies.

## <a name="terminating-instances"></a>Afsluitende exemplaren

Een actief exemplaar kan worden beëindigd met de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. De twee parameters zijn een `instanceId` en een `reason` tekenreeks, die in de logboeken en status van het exemplaar wordt geschreven. Een beëindigde exemplaar niet meer zodra het de volgende bereikt `await` punt, of het wordt onmiddellijk beëindigd als deze al ingeschakeld is een `await`.

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Instantie-beëindiging is momenteel alleen ondersteund voor C# orchestrator-functies.

## <a name="sending-events-to-instances"></a>Het verzenden van gebeurtenissen naar exemplaren

Gebeurtenismeldingen kunnen worden verzonden voor het uitvoeren van exemplaren die gebruikmaken van de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. -Exemplaren die deze gebeurtenissen kunnen verwerken zijn die wachten op een aanroep van [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

De parameters voor [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) zijn als volgt:

* **InstanceId**: de unieke ID van het exemplaar.
* **EventName**: de naam van de gebeurtenis te verzenden.
* **EventData**: een JSON-serialiseerbaar nettolading wilt verzenden naar het exemplaar.

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!NOTE]
> Gebeurtenissen verhogen wordt momenteel alleen ondersteund voor C# orchestrator-functies.

> [!WARNING]
> Als er geen orchestration-exemplaar met de opgegeven is *instantie-ID* of als het exemplaar niet op de opgegeven wachten is *gebeurtenisnaam*, het gebeurtenisbericht is verwijderd. Zie voor meer informatie over dit gedrag, de [GitHub probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van de HTTP-APIs bijvoorbeeld management](durable-functions-http-api.md)
