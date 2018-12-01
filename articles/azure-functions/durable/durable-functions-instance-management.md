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
ms.date: 11/27/2018
ms.author: azfuncdf
ms.openlocfilehash: cf408cc125362d2ea05c606a0b2e148d60647281
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679576"
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
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
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

De bovenstaande code wordt ervan uitgegaan dat een-binding met de naam als in het bestand function.json hebt gedefinieerd `starter` en typ als `orchestrationClient`. Als de binding is niet gedefinieerd, wordt het exemplaar duurzame functie niet gemaakt.

Voor de functie duurzame worden aangeroepen, moet de function.json worden gewijzigd als u wilt dat een binding voor de orchestration-client, zoals hieronder wordt beschreven

```js
{
    "bindings": [{
        "name":"starter",
        "type":"orchestrationClient",
        "direction":"out"
    }]
}
```

> [!NOTE]
> Gebruik een willekeurige id voor de exemplaar-ID. Dit helpt zorgen voor een gelijke verdeling wanneer orchestrator-functies schalen op meerdere virtuele machines. De juiste tijd voor het gebruik van niet-willekeurig exemplaar-id's is als de ID afkomstig van een externe bron zijn moet, of bij het implementeren van de [singleton orchestrator](durable-functions-singletons.md) patroon.

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk om te beginnen een exemplaar rechtstreeks via de [Azure Functions Core Tools](../functions-run-local.md) `durable start-new` opdracht. Het bevat de volgende parameters:

* **`function-name` (vereist)** : Naam van de functie om te starten
* **`input` (optioneel)** : Invoer voor de functie, in line- of via een JSON-bestand. Voor bestanden, het pad naar het bestand met het voorvoegsel `@`, zoals `@path/to/file.json`.
* **`id` (optioneel)** : ID van de orchestration-instantie. Als niet is opgegeven, wordt een willekeurige GUID gegenereerd.
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName. 

> [!NOTE]
> Core-hulpprogramma's voor opdrachten wordt ervan uitgegaan dat ze worden uitgevoerd vanuit de hoofdmap van een functie-app. Als `connection-string-setting` en `task-hub-name` expliciet zijn opgegeven, wordt de opdrachten kunnen worden uitgevoerd vanuit elke gewenste map. Terwijl deze opdrachten kunnen worden uitgevoerd zonder een functie-app-host waarop wordt uitgevoerd, kunnen sommige effecten niet worden waargenomen, tenzij de host wordt uitgevoerd. Bijvoorbeeld, de `start-new` opdracht wordt in de wachtrij plaatsen een startbericht naar de hub target-taak, maar de indeling wordt niet daadwerkelijk wordt uitgevoerd, tenzij er een functie-app hostproces wordt uitgevoerd die het bericht kan worden verwerkt.

De volgende opdracht start u de functie met de naam HelloWorld en plak de inhoud van het bestand ' teller-data.json' toe:
```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="querying-instances"></a>Een query uitvoeren op exemplaren

De [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse query naar de status van een orchestration-exemplaar.

Het duurt voordat een `instanceId` (vereist), `showHistory` (optioneel), `showHistoryOutput` (optioneel), en `showInput` (optioneel) als parameters. 
* **`showHistory`**: Als het ingesteld op `true`, het antwoord bevat de uitvoeringsgeschiedenis. 
* **`showHistoryOutput`**: Als het ingesteld op `true`, de uitvoeringsgeschiedenis wordt uitvoer voor activiteiten bevatten. 
* **`showInput`**: Als het ingesteld op `false`, het antwoord bevat niet de invoer van de functie. De standaardwaarde is `true`.

De methode retourneert een JSON-object met de volgende eigenschappen:

* **Naam**: de naam van de orchestrator-functie.
* **InstanceId**: de exemplaar-ID van de indeling (moet gelijk zijn aan de `instanceId` invoer).
* **Aanmaaktijd**: de tijd waarop de orchestrator-functie is gestart.
* **LastUpdatedTime**: het tijdstip waarop de orchestration laatste controlepunt.
* **Invoer**: de invoer van de functie als een JSON-waarde. Dit veld niet ingevuld als `showInput` is ingesteld op false.
* **CustomStatus**: status van de aangepaste indeling in JSON-indeling. 
* **Uitvoer**: de uitvoer van de functie als een JSON-waarde (als de functie is voltooid). Als de orchestrator-functie is mislukt, bevat deze eigenschap de foutdetails. Als de orchestrator-functie is beëindigd, bevat deze eigenschap de opgegeven reden voor de beëindiging (indien aanwezig).
* **RuntimeStatus**: een van de volgende waarden:
    * **In behandeling**: is gepland, maar is nog niet begonnen met het exemplaar.
    * **Met**: het exemplaar is gestart.
    * **Voltooid**: het exemplaar normaal gesproken is voltooid.
    * **ContinuedAsNew**: de instantie zelf is gestart met een nieuwe geschiedenis. Dit is een tijdelijke situatie.
    * **Kan geen**: de instantie is mislukt met een fout.
    * **Beëindigd**: het exemplaar is onverwacht gestopt.
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

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk dat de status van een exemplaar van de orchestration rechtstreeks via de [Azure Functions Core Tools](../functions-run-local.md) `durable get-runtime-status` opdracht. Het bevat de volgende parameters: 

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`show-input` (optioneel)** : Indien ingesteld op `true`, het antwoord bevat de invoer van de functie. De standaardwaarde is `false`.
* **`show-output` (optioneel)** : Indien ingesteld op `true`, het antwoord bevat de uitvoer van de functie. De standaardwaarde is `false`.
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht wordt de status (met inbegrip van de invoer en uitvoer) van een exemplaar met de exemplaar-ID orchestration van 0ab8c55a66644d68a3a8b220b12d209c ophalen. Wordt ervan uitgegaan dat de `func` opdracht wordt uitgevoerd vanuit de hoofdmap van de functie-app:
```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

De `durable get-history` opdracht kan worden gebruikt om op te halen van de geschiedenis van een orchestration-exemplaar. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Het kan ook worden ingesteld in host.json via durableTask:HubName.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="querying-all-instances"></a>Uitvoeren van query's alle exemplaren

U kunt de `GetStatusAsync` methode om op te vragen van de status van alle exemplaren van de indeling. Deze parameters niet uitvoeren, of u kunt doorgeven een `CancellationToken` object in het geval u wilt annuleren. De methode retourneert objecten met dezelfde eigenschappen als de `GetStatusAsync` methode met de parameters, behalve dat het geen geschiedenis retourneert. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk query exemplaren rechtstreeks via de [Azure Functions Core Tools](../functions-run-local.md) `durable get-instances` opdracht. Het bevat de volgende parameters:

* **`top` (optioneel)** : Met deze opdracht paginering ondersteunt. Deze parameter komt overeen met het aantal exemplaren per aanvraag wordt opgehaald. De standaardwaarde is 10.
* **`continuation-token` (optioneel)** : Een token om aan te geven welke pagina/sectie van instanties om op te halen. Elke `get-instances` uitvoering retourneert een token met de volgende exemplaren.
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable get-instances
```

## <a name="querying-instances-with-filters"></a>Een query uitvoeren op exemplaren met filters

U kunt ook de `GetStatusAsync` methode voor het ophalen van een lijst van de orchestration-instanties die overeenkomen met een set vooraf gedefinieerde filters. Mogelijke filteropties zijn onder andere de orchestration-aanmaaktijd en de status van de orchestration-runtime.

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="using-the-functions-core-tools"></a>Met behulp van de Functions Core Tools

De `durable get-instances` opdracht kan ook worden gebruikt met filters. Naast de voornoemde `top`, `continuation-token`, `connection-string-setting`, en `task-hub-name` parameters, drie filterparameters (`created-after`, `created-before`, en `runtime-status`), kan worden gebruikt. 

* **`created-after` (optioneel)** : Ophalen van de exemplaren die zijn gemaakt na deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`created-before` (optioneel)** : Ophalen van de exemplaren die zijn gemaakt vóór deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`runtime-status` (optioneel)** : Ophalen van de instanties waarvan u de status overeenkomen met deze ('actief', 'voltooid', enz.). Biedt meerdere statussen van (gescheiden door spaties).
* **`top` (optioneel)** : Aantal exemplaren per aanvraag wordt opgehaald. De standaardwaarde is 10.
* **`continuation-token` (optioneel)** : Een token om aan te geven welke pagina/sectie van instanties om op te halen. Elke `get-instances` uitvoering retourneert een token met de volgende exemplaren.
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

Als er geen filters (`created-after`, `created-before`, of `runtime-status`) zijn opgegeven, klikt u vervolgens `top` instanties worden opgehaald met geen betrekking op de runtime-status of het maken van tijd.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
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

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk om te beëindigen van een exemplaar van de orchestration rechtstreeks via de [Core Tools](../functions-run-local.md) `durable terminate` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar is beëindigd
* **`reason` (optioneel)** : De reden van beëindiging
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht wordt een orchestration-exemplaar met de ID van 0ab8c55a66644d68a3a8b220b12d209c beëindigen:
```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="sending-events-to-instances"></a>Verzenden van gebeurtenissen naar exemplaren

Meldingen van gebeurtenissen kunnen worden verzonden naar het uitvoeren van exemplaren met behulp van de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse. Instanties die deze gebeurtenissen kunnen worden verwerkt zijn die in afwachting van een aanroep van [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

De parameters voor [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) zijn als volgt:

* **InstanceId**: de unieke ID van het exemplaar.
* **EventName**: de naam van de gebeurtenis te verzenden.
* **EventData**: een JSON-geserialiseerd nettolading verzenden naar het exemplaar.

```csharp
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

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk om te genereren van een gebeurtenis naar een exemplaar van de orchestration rechtstreeks via de [Core Tools](../functions-run-local.md) `durable raise-event` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`event-name` (optioneel)** : Naam van de gebeurtenis te verhogen. De standaardwaarde is `$"Event_{RandomGUID}"`
* **`event-data` (optioneel)** : Gegevens worden verzonden naar de orchestration-exemplaar. Dit kan het pad naar een JSON-bestand of de gegevens kunnen rechtstreeks op de opdrachtregel worden opgegeven.
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```
```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Wachten op voltooiing van de orchestration

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse toont een [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API die kan worden gebruikt om op te halen synchroon de werkelijke uitvoer van een orchestration-exemplaar. De methode maakt gebruik van de standaardwaarde van tien seconden totdat de `timeout` en 1 seconde voor `retryInterval` wanneer ze niet worden ingesteld.  

Hier volgt een voorbeeld van de HTTP-trigger-functie die wordt gedemonstreerd hoe u deze API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

De functie kan worden aangeroepen met de volgende regel met behulp van 2 seconden time-out en het interval voor opnieuw proberen 0,5 seconde:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Afhankelijk van de tijd die nodig is om op te halen van het antwoord van de orchestration-exemplaar, zijn er twee mogelijke situaties:

* De orchestration-exemplaren is voltooid binnen de gedefinieerde time-out (in dit geval 2 seconden), het antwoord is de daadwerkelijke orchestration exemplaar uitvoer synchroon geleverd:

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

* De orchestration-exemplaren kunnen niet worden voltooid binnen de gedefinieerde time-out (in dit geval 2 seconden), het antwoord is de standaard één dat wordt beschreven in **HTTP API-URL detectie**:

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
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
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
* **RewindPostUri**: de URL 'terugspoelen' van de orchestration-exemplaar.

Activiteitsfuncties kunnen een exemplaar van verzenden [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) met externe systemen om te controleren of het genereren van gebeurtenissen naar een indeling:

```csharp
[FunctionName("SendInstanceInfo")]
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

## <a name="rewinding-instances-preview"></a>Terugspoelen exemplaren (preview)

Een mislukte orchestration-exemplaar kan worden *teruggespoeld* in een eerder status in orde met de [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) API. Dit gebeurt door het plaatsen van de orchestration terug naar de *met* staat en opnieuw uit te voeren de uitvoering van activiteit en/of suborchestration fouten, die de orchestration-fout heeft veroorzaakt.

> [!NOTE]
> Deze API is niet bedoeld om te worden van een vervanging voor de juiste foutafhandeling en beleid voor opnieuw proberen. In plaats daarvan is het bedoeld om te worden gebruikt alleen in gevallen waar de orchestration-instanties om onverwachte redenen mislukken. Zie voor meer informatie over beleid voor fout verwerking en probeer het opnieuw, de [foutafhandeling](durable-functions-error-handling.md) onderwerp.

Een voorbeeld van de use-case voor *terugspoelen* is een werkstroom met betrekking tot een reeks [menselijke goedkeuringen](durable-functions-overview.md#pattern-5-human-interaction). Stel dat er zijn een reeks activiteitsfuncties zodat iemand op de hoogte dat hun goedkeuring is vereist en uit het realtime antwoord wachten. Nadat alle van de goedkeuring activiteiten hebben ontvangen antwoorden of is een time-out, een andere activiteit mislukt vanwege een onjuiste configuratie van toepassing, zoals een ongeldige databaseverbindingsreeks. Het resultaat is een fout orchestration diep in de werkstroom. Met de `RewindAsync` API, fout in de configuratie voor een beheerder van de toepassing kunt oplossen en *terugspoelen* de mislukte orchestration terug naar de status direct vóór de fout. Geen van de stappen menselijke tussenkomst moet opnieuw worden goedgekeurd en de indeling kan nu worden uitgevoerd.

> [!NOTE]
> De *terugspoelen* terugspoelen orchestration exemplaren die gebruikmaken van duurzame timers wordt niet ondersteund door de functie.

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk terugspoelen een orchestration-exemplaar rechtstreeks via de [Core Tools](../functions-run-local.md) `durable rewind` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`reason` (optioneel)** : Reden voor de orchestration-instantie terugspoelen
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Exemplaar Geschiedenis leegmaken

Orchestration-geschiedenis kan worden opgeschoond met behulp van [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_). De functionaliteit wordt verwijderd van alle gegevens die zijn gekoppeld aan een orchestration - rijen van de Azure Table en grote berichten blobs als deze bestaan. De methode heeft twee overloads. Het eerste item Hiermee verwijdert u de geschiedenis van de orchestration-exemplaar-ID:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

Het tweede voorbeeld ziet een timer geactiveerde functie die Hiermee verwijdert u de geschiedenis voor alle orchestration-exemplaren die voltooid na het opgegeven tijdsinterval. In dit geval worden deze gegevens voor alle exemplaren voltooid van meer dan 30 dagen geleden verwijderd. Deze is gepland op 12: 00 uur eenmaal per dag wordt uitgevoerd:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync( 
                    DateTime.MinValue,
                    DateTime.UtcNow.AddDays(-30),  
                    new List<OrchestrationStatus> 
                    { 
                        OrchestrationStatus.Completed
                    }); 
}
```

> [!NOTE]
> De *PurgeInstanceHistory* overbelasting accepteren periode als parameter wordt verwerkt alleen orchestration-exemplaren in een van de runtimestatus van de - voltooid, beëindigd of mislukt.

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is mogelijk wissen van de geschiedenis van een orchestration-exemplaar met behulp van de [Core Tools](../functions-run-local.md) `durable purge-history` opdracht. Vergelijkbaar met de tweede C# bovenstaand voorbeeld het Hiermee verwijdert u de geschiedenis voor alle orchestration-exemplaren die zijn gemaakt tijdens een opgegeven tijdsinterval. De exemplaren worden opgeschoond kunnen verder worden gefilterd door de runtimestatus. De opdracht heeft verschillende parameters:

* **`created-after` (optioneel)** : Leegmaken van de geschiedenis van exemplaren die zijn gemaakt na deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`created-before` (optioneel)** : Leegmaken van de geschiedenis van exemplaren die zijn gemaakt vóór deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`runtime-status` (optioneel)** : Leegmaken van de geschiedenis van exemplaren met de status overeenkomen met deze ('actief', 'voltooid', enz.). Biedt meerdere statussen van (gescheiden door spaties).
* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht verwijdert de geschiedenis van alle mislukte exemplaren die zijn gemaakt vóór 14 November 2018 om 19:35 uur (UTC).
```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="deleting-a-task-hub"></a>Een taak Hub verwijderen
Met behulp van de [Core Tools](../functions-run-local.md) `durable delete-task-hub` opdracht, is het mogelijk om te verwijderen van alle opslag-artefacten die zijn gekoppeld aan een bepaalde taak hub. Dit omvat Azure storage-tabellen, wachtrijen en blobs. De opdracht heeft twee parameters: 

* **`connection-string-setting` (optioneel)** : Naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak moet worden gebruikt. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht verwijdert alle Azure storage-gegevens die zijn gekoppeld aan de hub 'UserTest'-taak.
```bash
func durable delete-task-hub --task-hub-name UserTest
```


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van de HTTP-APIs bijvoorbeeld management](durable-functions-http-api.md)
