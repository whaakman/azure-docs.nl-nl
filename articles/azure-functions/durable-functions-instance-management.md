---
title: -Exemplaren in duurzame functies - Azure beheren
description: Informatie over het beheren van exemplaren in de uitbreiding duurzame functies voor Azure Functions.
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
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: d6f7c924491614190952ce620f33572307a22c22
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265432"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>-Exemplaren in duurzame functies (Azure Functions) beheren

[Duurzame functies](durable-functions-overview.md) orchestration exemplaren kunnen worden gestart, is beëindigd, opgevraagd en meldingsgebeurtenissen verzonden. Alle exemplaar management wordt uitgevoerd met behulp van de [orchestration client binding](durable-functions-bindings.md). In dit artikel gaat naar de details van elke bewerking voor het beheer van exemplaar.

## <a name="starting-instances"></a>Exemplaren wordt gestart

De [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) methode op de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) start een nieuw exemplaar van een orchestrator-functie. Instanties van deze klasse kunnen worden opgehaald met behulp van de `orchestrationClient` binding. Intern maakt de enqueues van deze methode een bericht in de wachtrij besturingselement, die vervolgens activeert het begin van een functie met de opgegeven naam die gebruikmaakt van de `orchestrationTrigger` binding activeren. 

De taak is voltooid als de orchestration-proces wordt gestart. De orchestration-proces moet worden gestart binnen 30 seconden. Als het duurt langer, een `TimeoutException` gegenereerd. 

De parameters voor [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) zijn als volgt:

* **Naam**: de naam van de orchestrator-functie te plannen.
* **Invoer**: JSON-serialiseerbaar gegevens die moeten worden doorgegeven als invoer voor de orchestrator-functie.
* **InstanceId**: (optioneel) de unieke ID van het exemplaar. Als niet wordt opgegeven, wordt er een willekeurig exemplaar-ID gegenereerd.

Dit is een eenvoudig C#-voorbeeld:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Voor niet-.NET-talen, de functie binding uitvoer kan worden gebruikt voor nieuwe exemplaren ook starten. In dit geval kan een JSON-geserialiseerd object met de bovenstaande drie parameters als velden worden gebruikt. Neem bijvoorbeeld de volgende JavaScript-functie:

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

De [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) methode op de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse de status van een exemplaar van de orchestration opgevraagd. Het duurt een `instanceId` (vereist), `showHistory` (optioneel) en `showHistoryOutput` (optioneel) als parameters. Als `showHistory` is ingesteld op `true`, het antwoord bevat de geschiedenis van de uitvoering. Als `showHistoryOutput` is ingesteld op `true` , evenals de uitvoergeschiedenis van de uitvoer voor activiteiten bevat. De methode retourneert een object met de volgende eigenschappen:

* **Naam**: de naam van de orchestrator-functie.
* **InstanceId**: de exemplaar-ID van de orchestration (moet hetzelfde zijn als de `instanceId` invoer).
* **CreatedTime**: de tijd waarop de orchestrator-functie is gestart.
* **LastUpdatedTime**: het tijdstip waarop de orchestration laatste controlepunt.
* **Invoer**: de invoer van de functie als een JSON-waarde.
* **CustomStatus**: status van de aangepaste orchestration in JSON-indeling. 
* **Uitvoer**: de uitvoer van de functie als een JSON-waarde (als de functie is voltooid). Als de orchestrator-functie is mislukt, wordt deze eigenschap de foutgegevens bevatten. Als de orchestrator-functie is beëindigd, bevat deze eigenschap de opgegeven reden voor de beëindiging (indien aanwezig).
* **RuntimeStatus**: een van de volgende waarden:
    * **Met**: het exemplaar eenmaal is gestart.
    * **Voltooid**: het exemplaar normaal is voltooid.
    * **ContinuedAsNew**: het exemplaar zelf een nieuwe geschiedenis is opgestart. Dit is een tijdelijke status.
    * **Kan geen**: het exemplaar is mislukt met een fout opgetreden.
    * **Beëindigd**: het exemplaar is onverwacht beëindigd.
* **Geschiedenis**: de geschiedenis van de uitvoering van de orchestration. Dit veld wordt alleen ingevuld als `showHistory` is ingesteld op `true`.
    
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
## <a name="querying-all-instances"></a>Opvragen van alle exemplaren

U kunt de `GetStatusAsync` methode query uitvoeren op de status van alle exemplaren van de orchestration. Parameters hoeft u alleen, of u kunt doorgeven een `CancellationToken` object als u wilt annuleren. De methode retourneert objecten met dezelfde eigenschappen als de `GetStatusAsync` methode met parameters, behalve dat het geschiedenis niet retourneren. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    TraceWriter log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.Info(JsonConvert.SerializeObject(instance));
    };
}
```

## <a name="terminating-instances"></a>Afsluitende exemplaren

Een actief exemplaar van de orchestration kan worden beëindigd met de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. De twee parameters zijn een `instanceId` en een `reason` tekenreeks, die in de logboeken en status van het exemplaar wordt geschreven. Een beëindigde exemplaar niet meer zodra het de volgende bereikt `await` punt, of het wordt onmiddellijk beëindigd als deze al ingeschakeld is een `await`. 

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
> Exemplaar beëindiging wordt momenteel niet doorgegeven. Activiteit-functies en onderdelen integraties wordt uitgevoerd voor voltooiing ongeacht of de orchestration-exemplaar dat ze aangeroepen is beëindigd.

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

> [!WARNING]
> Als er geen orchestration-exemplaar met de opgegeven is *instantie-ID* of als het exemplaar niet op de opgegeven wachten is *gebeurtenisnaam*, het gebeurtenisbericht is verwijderd. Zie voor meer informatie over dit gedrag, de [GitHub probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Wachten op voltooiing van de orchestration

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse zichtbaar gemaakt een [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API die kan worden gebruikt om op te halen synchroon de werkelijke uitvoer van een orchestration-exemplaar. De methode wordt de standaardwaarde van 10 seconden voor `timeout` en 1 seconde voor `retryInterval` wanneer ze niet zijn ingesteld.  

Hier volgt een voorbeeld van HTTP-trigger-functie die laat zien hoe u deze API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

De functie kan worden aangeroepen met de volgende regel met behulp van 2 seconden time-out en het interval voor opnieuw proberen 0,5 seconde:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Er zijn twee gevallen afhankelijk van de tijd om op te halen van het antwoord van de orchestration-exemplaar:

1. De orchestration-exemplaren binnen de gedefinieerde time-out (in dit geval 2 seconden) is voltooid, het antwoord is de uitvoer van de werkelijke orchestration exemplaar synchroon geleverd:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. De orchestration-exemplaren kunnen niet worden voltooid binnen de gedefinieerde time-out (in dit geval 2 seconden), het antwoord is de standaardinstelling een beschreven in **HTTP-URL van de API-detectie**:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> De indeling van de webhook-URL's kan verschillen, afhankelijk van welke versie van de Azure Functions-host die u uitvoert. Het vorige voorbeeld is voor de Azure Functions 2.0-host.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van de HTTP-APIs bijvoorbeeld management](durable-functions-http-api.md)
