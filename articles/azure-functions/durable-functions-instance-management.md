---
title: Duurzame functies - Azure-instanties beheren
description: Informatie over het beheren van instanties met de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 72ea5e54bf86ce408700c0456f6d37f5f3c29924
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091779"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>-Exemplaren in duurzame functies (Azure Functions) beheren

[Duurzame functies](durable-functions-overview.md) orchestration-exemplaren kunnen worden gestart, beëindigd, opgevraagd en meldingsgebeurtenissen verzonden. Alle instantiebeheer wordt gedaan met behulp van de [orchestration-client-binding](durable-functions-bindings.md). In dit artikel gaat in op de details van elke bewerking voor het beheer van exemplaar.

## <a name="starting-instances"></a>De exemplaren starten

De [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) start een nieuw exemplaar van een orchestrator-functie. Instanties van deze klasse kunnen worden verkregen met behulp van de `orchestrationClient` binding. Intern maakt de enqueues van deze methode een bericht in de wachtrij besturingselement, dat vervolgens wordt het begin van een functie met de opgegeven naam die gebruikmaakt van de `orchestrationTrigger` binding activeren. 

De taak is voltooid wanneer de orchestration-proces wordt gestart. De orchestration-proces moet worden gestart binnen 30 seconden. Als het langer duurt een `TimeoutException` wordt gegenereerd. 

De parameters voor [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) zijn als volgt:

* **Naam**: de naam van de orchestrator-functie te plannen.
* **Invoer**: JSON-geserialiseerd gegevens die moet worden doorgegeven als invoer voor de orchestrator-functie.
* **InstanceId**: (optioneel) de unieke ID van het exemplaar. Indien niet opgegeven, wordt een willekeurig exemplaar-ID worden gegenereerd.

Hier is een eenvoudig C#-voorbeeld:

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

Voor niet-.NET-talen, de functie-Uitvoerbinding kan worden gebruikt om nieuwe exemplaren ook worden gestart. In dit geval kan een JSON-geserialiseerd object met de bovenstaande drie parameters als velden worden gebruikt. Bijvoorbeeld, houd rekening met de volgende JavaScript-functie:

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
> Het is raadzaam dat u een willekeurige id voor de exemplaar-ID gebruiken. Dit helpt zorgen voor een gelijke verdeling wanneer orchestrator-functies schalen op meerdere virtuele machines. De juiste tijd voor het gebruik van niet-willekeurig exemplaar-id's is als de ID afkomstig van een externe bron zijn moet, of bij het implementeren van de [singleton orchestrator](durable-functions-singletons.md) patroon.

## <a name="querying-instances"></a>Een query uitvoeren op exemplaren

De [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse query naar de status van een orchestration-exemplaar. Het duurt voordat een `instanceId` (vereist), `showHistory` (optioneel), en `showHistoryOutput` (optioneel) als parameters. Als `showHistory` is ingesteld op `true`, het antwoord bevat de uitvoeringsgeschiedenis. Als `showHistoryOutput` is ingesteld op `true` ook de uitvoeringsgeschiedenis uitvoer voor activiteiten bevat. De methode retourneert een object met de volgende eigenschappen:

* **Naam**: de naam van de orchestrator-functie.
* **InstanceId**: de exemplaar-ID van de indeling (moet gelijk zijn aan de `instanceId` invoer).
* **Aanmaaktijd**: de tijd waarop de orchestrator-functie is gestart.
* **LastUpdatedTime**: het tijdstip waarop de orchestration laatste controlepunt.
* **Invoer**: de invoer van de functie als een JSON-waarde.
* **CustomStatus**: status van de aangepaste indeling in JSON-indeling. 
* **Uitvoer**: de uitvoer van de functie als een JSON-waarde (als de functie is voltooid). Als de orchestrator-functie is mislukt, bevat deze eigenschap de foutdetails. Als de orchestrator-functie is beëindigd, bevat deze eigenschap de opgegeven reden voor de beëindiging (indien aanwezig).
* **RuntimeStatus**: een van de volgende waarden:
    * **In behandeling**: is gepland, maar is nog niet begonnen met het exemplaar.
    * **Met**: het exemplaar is gestart.
    * **Voltooid**: het exemplaar normaal gesproken is voltooid.
    * **ContinuedAsNew**: de instantie zelf is gestart met een nieuwe geschiedenis. Dit is een tijdelijke situatie.
    * **Kan geen**: de instantie is mislukt met een fout.
    * **Beëindigd**: het exemplaar is onverwacht beëindigd.
* **Geschiedenis**: de uitvoeringsgeschiedenis van de indeling. Dit veld wordt alleen ingevuld als `showHistory` is ingesteld op `true`.
    
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
## <a name="querying-all-instances"></a>Uitvoeren van query's alle exemplaren

U kunt de `GetStatusAsync` methode om op te vragen van de status van alle exemplaren van de indeling. Deze parameters niet uitvoeren, of u kunt doorgeven een `CancellationToken` object in het geval u wilt annuleren. De methode retourneert objecten met dezelfde eigenschappen als de `GetStatusAsync` methode met de parameters, behalve dat het geen geschiedenis retourneert. 

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

## <a name="terminating-instances"></a>Exemplaren beëindigen

Een actief exemplaar van de indeling kan worden afgesloten met behulp van de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. De twee parameters zijn een `instanceId` en een `reason` tekenreeks, die worden geschreven naar Logboeken en de status van het exemplaar. Een beëindigde exemplaar werkt niet meer als het de volgende bereikt `await` punt, of deze wordt onmiddellijk beëindigd als deze al op een `await`. 

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
> Beëindiging van de instantie wordt momenteel niet doorgegeven. Activiteitsfuncties en subquery indelingen wordt uitgevoerd tot voltooiing, ongeacht of de orchestration-exemplaar dat ze met de naam is beëindigd.

## <a name="sending-events-to-instances"></a>Verzenden van gebeurtenissen naar exemplaren

Meldingen van gebeurtenissen kunnen worden verzonden naar het uitvoeren van exemplaren met behulp van de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. Instanties die deze gebeurtenissen kunnen worden verwerkt zijn die in afwachting van een aanroep van [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

De parameters voor [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) zijn als volgt:

* **InstanceId**: de unieke ID van het exemplaar.
* **EventName**: de naam van de gebeurtenis te verzenden.
* **EventData**: een JSON-geserialiseerd nettolading verzenden naar het exemplaar.

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
> Als er geen orchestration-exemplaar met de opgegeven *exemplaar-ID* of als het exemplaar niet op de opgegeven wachten is *gebeurtenisnaam*, het gebeurtenisbericht wordt verwijderd. Zie voor meer informatie over dit gedrag, de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Wachten op voltooiing van de orchestration

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse toont een [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API die kan worden gebruikt om op te halen synchroon de werkelijke uitvoer van een orchestration-exemplaar. De methode maakt gebruik van de standaardwaarde van tien seconden totdat de `timeout` en 1 seconde voor `retryInterval` wanneer ze niet worden ingesteld.  

Hier volgt een voorbeeld van de HTTP-trigger-functie die wordt gedemonstreerd hoe u deze API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

De functie kan worden aangeroepen met de volgende regel met behulp van 2 seconden time-out en het interval voor opnieuw proberen 0,5 seconde:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Er zijn twee mogelijke situaties, afhankelijk van de tijd die nodig is om op te halen van het antwoord van de orchestration-exemplaar:

1. De orchestration-exemplaren is voltooid binnen de gedefinieerde time-out (in dit geval 2 seconden), het antwoord is de daadwerkelijke orchestration exemplaar uitvoer synchroon geleverd:

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

2. De orchestration-exemplaren kunnen niet worden voltooid binnen de gedefinieerde time-out (in dit geval 2 seconden), het antwoord is de standaard één dat wordt beschreven in **HTTP API-URL detectie**:

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
> De indeling van de webhook-URL's kan verschillen afhankelijk van welke versie van de Azure Functions-host die u uitvoert. Het vorige voorbeeld is voor de Azure Functions 2.0-host.

## <a name="retrieving-http-management-webhook-urls"></a>Bij het ophalen van HTTP-beheer Webhook-URL 's

Externe systemen kunnen communiceren met duurzame functies via de webhook-URL's die deel van het standaardantwoord dat wordt beschreven uitmaken in [HTTP API-URL detectie](durable-functions-http-api.md). Echter, de webhook-URL's ook toegankelijk via een programma in de orchestration-client of in een functie van de activiteit via de [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)klasse. 

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) heeft één parameter:

* **InstanceId**: de unieke ID van het exemplaar.

De methode retourneert een exemplaar van de [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) met de volgende tekenreekseigenschappen:

* **Id**: de exemplaar-ID van de indeling (moet gelijk zijn aan de `InstanceId` invoer).
* **StatusQueryGetUri**: de URL van de status van de orchestration-instantie.
* **SendEventPostUri**: de ' raise '-URL van de orchestration-instantie.
* **TerminatePostUri**: de URL 'beëindigd' van de orchestration-exemplaar.

Activiteitsfuncties kunnen een exemplaar van verzenden [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) met externe systemen om te controleren of het genereren van gebeurtenissen naar een indeling:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van de HTTP-APIs bijvoorbeeld management](durable-functions-http-api.md)
