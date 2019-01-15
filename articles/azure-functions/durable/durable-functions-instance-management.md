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
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 19abd905bbc5e8ab724f7a2f25cb6da90db82841
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262639"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>-Exemplaren in duurzame functies in Azure beheren

[Duurzame functies](durable-functions-overview.md) orchestration-exemplaren kunnen worden gestart, beëindigd, opgevraagd en meldingsgebeurtenissen verzonden. Alle instantiebeheer wordt gedaan met behulp van de [orchestration-client-binding](durable-functions-bindings.md). In dit artikel gaat in op de details van elke bewerking voor het beheer van exemplaar.

## <a name="starting-instances"></a>De exemplaren starten

De [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) of `startNew` op de `DurableOrchestrationClient` (JavaScript) start een nieuw exemplaar van een orchestrator-functie. Instanties van deze klasse kunnen worden verkregen met behulp van de `orchestrationClient` binding. Intern maakt de enqueues van deze methode een bericht in de wachtrij besturingselement, dat vervolgens wordt het begin van een functie met de opgegeven naam die gebruikmaakt van de `orchestrationTrigger` binding activeren.

Deze asynchrone bewerking is voltooid wanneer de orchestration-proces met succes is gepland. De orchestration-proces moet worden gestart binnen 30 seconden. Als het langer duurt een `TimeoutException` wordt gegenereerd.

> [!WARNING]
> Bij het ontwikkelen van lokaal in JavaScript, moet u de omgevingsvariabele instellen `WEBSITE_HOSTNAME` naar `localhost:<port>`, bijvoorbeeld. `localhost:7071` gebruik van methoden op `DurableOrchestrationClient`. Zie voor meer informatie over deze vereiste de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="net"></a>.NET

De parameters voor [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) zijn als volgt:

* **Naam**: De naam van de orchestrator-functie te plannen.
* **Invoer**: JSON-geserialiseerd gegevens die moet worden doorgegeven als invoer voor de orchestrator-functie.
* **InstanceId**: (Optioneel) De unieke ID van het exemplaar. Indien niet opgegeven, wordt een willekeurig exemplaar-ID worden gegenereerd.

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

De parameters voor `startNew` zijn als volgt:

* **Naam**: De naam van de orchestrator-functie te plannen.
* **InstanceId**: (Optioneel) De unieke ID van het exemplaar. Indien niet opgegeven, wordt een willekeurig exemplaar-ID worden gegenereerd.
* **Invoer**: (Optioneel) JSON-geserialiseerd gegevens die moet worden doorgegeven als invoer voor de orchestrator-functie.

Hier volgt een voorbeeld van een eenvoudige JavaScript:

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

> [!NOTE]
> Gebruik een willekeurige id voor de exemplaar-ID. Dit helpt zorgen voor een gelijke verdeling wanneer orchestrator-functies schalen op meerdere virtuele machines. De juiste tijd voor het gebruik van niet-willekeurig exemplaar-id's is als de ID afkomstig van een externe bron zijn moet, of bij het implementeren van de [singleton orchestrator](durable-functions-singletons.md) patroon.

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk om te beginnen een exemplaar rechtstreeks via de [Azure Functions Core Tools](../functions-run-local.md) `durable start-new` opdracht. Het bevat de volgende parameters:

* **`function-name` (vereist)** : Naam van de functie om te starten
* **`input` (optioneel)** : Voer voor de functie, in line- of via een JSON-bestand. Voor bestanden, het pad naar het bestand met het voorvoegsel `@`, zoals `@path/to/file.json`.
* **`id` (optioneel)** : ID van de orchestration-exemplaar. Als niet is opgegeven, wordt een willekeurige GUID gegenereerd.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

> [!NOTE]
> Core-hulpprogramma's voor opdrachten wordt ervan uitgegaan dat ze worden uitgevoerd vanuit de hoofdmap van een functie-app. Als `connection-string-setting` en `task-hub-name` expliciet zijn opgegeven, wordt de opdrachten kunnen worden uitgevoerd vanuit elke gewenste map. Terwijl deze opdrachten kunnen worden uitgevoerd zonder een functie-app-host waarop wordt uitgevoerd, kunnen sommige effecten niet worden waargenomen, tenzij de host wordt uitgevoerd. Bijvoorbeeld, de `start-new` opdracht wordt in de wachtrij plaatsen een startbericht naar de hub target-taak, maar de indeling wordt niet daadwerkelijk wordt uitgevoerd, tenzij er een functie-app hostproces wordt uitgevoerd die het bericht kan worden verwerkt.

De volgende opdracht start u de functie met de naam HelloWorld en plak de inhoud van het bestand ' teller-data.json' toe:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="querying-instances"></a>Een query uitvoeren op exemplaren

De [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET) of de `getStatus` methode voor de `DurableOrchestrationClient` klasse (JavaScript) de status van een indeling opvragen het exemplaar.

Het duurt voordat een `instanceId` (vereist), `showHistory` (optioneel), `showHistoryOutput` (optioneel), en `showInput` (optioneel, alleen .NET) als parameters.

* **`showHistory`**: Indien ingesteld op `true`, het antwoord bevat de uitvoeringsgeschiedenis.
* **`showHistoryOutput`**: Indien ingesteld op `true`, de uitvoeringsgeschiedenis wordt uitvoer voor activiteiten bevatten.
* **`showInput`**: Indien ingesteld op `false`, het antwoord bevat niet de invoer van de functie. De standaardwaarde is `true`. (Alleen voor .NET)

De methode retourneert een JSON-object met de volgende eigenschappen:

* **Naam**: De naam van de orchestrator-functie.
* **InstanceId**: De exemplaar-ID van de indeling (moet gelijk zijn aan de `instanceId` invoer).
* **Aanmaaktijd**: De tijd waarop de orchestrator-functie is gestart.
* **LastUpdatedTime**: Het tijdstip waarop de orchestration laatste controlepunt.
* **Invoer**: De invoer van de functie als een JSON-waarde. Dit veld niet ingevuld als `showInput` is ingesteld op false.
* **CustomStatus**: De status van de aangepaste indeling in JSON-indeling.
* **Uitvoer**: De uitvoer van de functie als een JSON-waarde (als de functie is voltooid). Als de orchestrator-functie is mislukt, bevat deze eigenschap de foutdetails. Als de orchestrator-functie is beëindigd, bevat deze eigenschap de opgegeven reden voor de beëindiging (indien aanwezig).
* **RuntimeStatus**: Een van de volgende waarden:
  * **In behandeling**: Het exemplaar is gepland, maar is nog niet begonnen uitgevoerd.
  * **Uitvoeren**: Het exemplaar is gestart.
  * **Voltooid**: Het exemplaar is normaal gesproken voltooid.
  * **ContinuedAsNew**: Het exemplaar opnieuw is opgestart zelf met de geschiedenis van een nieuwe. Dit is een tijdelijke situatie.
  * **Mislukt**: Het exemplaar is mislukt met een fout.
  * **Beëindigd**: Het exemplaar is onverwacht gestopt.
* **Geschiedenis**: De uitvoeringsgeschiedenis van de indeling. Dit veld wordt alleen ingevuld als `showHistory` is ingesteld op `true`.

Deze methode retourneert `null` als het exemplaar bestaat niet of is nog niet begonnen uitgevoerd.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk dat de status van een exemplaar van de orchestration rechtstreeks via de [Azure Functions Core Tools](../functions-run-local.md) `durable get-runtime-status` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`show-input` (optioneel)** : Indien ingesteld op `true`, het antwoord bevat de invoer van de functie. De standaardwaarde is `false`.
* **`show-output` (optioneel)** : Indien ingesteld op `true`, het antwoord bevat de uitvoer van de functie. De standaardwaarde is `false`.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht wordt de status (met inbegrip van de invoer en uitvoer) van een exemplaar met de exemplaar-ID orchestration van 0ab8c55a66644d68a3a8b220b12d209c ophalen. Wordt ervan uitgegaan dat de `func` opdracht wordt uitgevoerd vanuit de hoofdmap van de functie-app:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

De `durable get-history` opdracht kan worden gebruikt om op te halen van de geschiedenis van een orchestration-exemplaar. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Het kan ook worden ingesteld in host.json via durableTask:HubName.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="querying-all-instances"></a>Uitvoeren van query's alle exemplaren

U kunt de `GetStatusAsync` (.NET) of `getStatusAll` methode om op te vragen van de status van alle exemplaren van de indeling (JavaScript). In .NET geeft u een `CancellationToken` object in het geval u wilt annuleren. De methode retourneert objecten met dezelfde eigenschappen als de `GetStatusAsync` methode met parameters.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instances = await client.getStatusAll();
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk query exemplaren rechtstreeks via de [Azure Functions Core Tools](../functions-run-local.md) `durable get-instances` opdracht. Het bevat de volgende parameters:

* **`top` (optioneel)** : Met deze opdracht biedt ondersteuning voor paginering. Deze parameter komt overeen met het aantal exemplaren per aanvraag wordt opgehaald. De standaardwaarde is 10.
* **`continuation-token` (optioneel)** : Een token om aan te geven welke pagina/sectie van instanties om op te halen. Elke `get-instances` uitvoering retourneert een token met de volgende exemplaren.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable get-instances
```

## <a name="querying-instances-with-filters"></a>Een query uitvoeren op exemplaren met filters

U kunt ook de `GetStatusAsync` (.NET) of `getStatusBy` (JavaScript)-methode voor het ophalen van een lijst van de orchestration-instanties die overeenkomen met een set vooraf gedefinieerde filters. Mogelijke filteropties zijn onder andere de orchestration-aanmaaktijd en de status van de orchestration-runtime.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const runtimeStatus = [
        df.OrchestrationRuntimeStatus.Completed,
        df.OrchestrationRuntimeStatus.Running,
    ];
    const instances = await client.getStatusBy(
        new Date(2018, 3, 10, 10, 1, 0),
        new Date(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    );
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="using-the-functions-core-tools"></a>Met behulp van de Functions Core Tools

De `durable get-instances` opdracht kan ook worden gebruikt met filters. Naast de voornoemde `top`, `continuation-token`, `connection-string-setting`, en `task-hub-name` parameters, drie filterparameters (`created-after`, `created-before`, en `runtime-status`), kan worden gebruikt.

* **`created-after` (optioneel)** : Ophalen van de exemplaren die zijn gemaakt na deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`created-before` (optioneel)** : Ophalen van de exemplaren die zijn gemaakt vóór deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`runtime-status` (optioneel)** : De instanties waarvan u de status overeenkomen met deze ('actief', 'voltooid', enzovoort) worden opgehaald. Biedt meerdere statussen van (gescheiden door spaties).
* **`top` (optioneel)** : Aantal instanties worden opgehaald per aanvraag. De standaardwaarde is 10.
* **`continuation-token` (optioneel)** : Een token om aan te geven welke pagina/sectie van instanties om op te halen. Elke `get-instances` uitvoering retourneert een token met de volgende exemplaren.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

Als er geen filters (`created-after`, `created-before`, of `runtime-status`) zijn opgegeven, klikt u vervolgens `top` instanties worden opgehaald met geen betrekking op de runtime-status of het maken van tijd.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminating-instances"></a>Exemplaren beëindigen

Een actief exemplaar van de indeling kan worden afgesloten met behulp van de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET), of de `terminate` -methode van de `DurableOrchestrationClient` klasse () JavaScript). De twee parameters zijn een `instanceId` en een `reason` tekenreeks, die worden geschreven naar Logboeken en de status van het exemplaar. Een beëindigde exemplaar werkt niet meer als het de volgende bereikt `await` (.NET) of `yield` punt (JavaScript), of deze wordt onmiddellijk beëindigd als deze al op een `await` (.NET) of `yield` (JavaScript).

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

> [!NOTE]
> Beëindiging van de instantie wordt momenteel niet doorgegeven. Activiteitsfuncties en subquery indelingen wordt uitgevoerd tot voltooiing, ongeacht of de orchestration-exemplaar dat ze met de naam is beëindigd.

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk om te beëindigen van een exemplaar van de orchestration rechtstreeks via de [Core Tools](../functions-run-local.md) `durable terminate` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar is beëindigd
* **`reason` (optioneel)** : De reden van beëindiging
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht wordt een orchestration-exemplaar met de ID van 0ab8c55a66644d68a3a8b220b12d209c beëindigen:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="sending-events-to-instances"></a>Verzenden van gebeurtenissen naar exemplaren

Meldingen van gebeurtenissen kunnen worden verzonden naar het uitvoeren van exemplaren met behulp van de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET) of de `raiseEvent` -methode van de `DurableOrchestrationClient` klasse () JavaScript). Instanties die deze gebeurtenissen kunnen worden verwerkt zijn die in afwachting van een aanroep van [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.NET) of `waitForExternalEvent` (JavaScript).

De parameters voor [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) (.NET) en `raiseEvent` (JavaScript) zijn als volgt:

* **InstanceId**: De unieke ID van het exemplaar.
* **EventName**: De naam van de gebeurtenis te verzenden.
* **EventData**: De nettolading van een JSON-serialisatie om te verzenden naar het exemplaar.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

> [!WARNING]
> Als er geen orchestration-exemplaar met de opgegeven *exemplaar-ID* of als het exemplaar niet op de opgegeven wachten is *gebeurtenisnaam*, het gebeurtenisbericht wordt verwijderd. Zie voor meer informatie over dit gedrag, de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk om te genereren van een gebeurtenis naar een exemplaar van de orchestration rechtstreeks via de [Core Tools](../functions-run-local.md) `durable raise-event` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`event-name` (optioneel)** : De naam van de gebeurtenis te verhogen. De standaardwaarde is `$"Event_{RandomGUID}"`
* **`event-data` (optioneel)** : Gegevens worden verzonden naar de orchestration-exemplaar. Dit kan het pad naar een JSON-bestand of de gegevens kunnen rechtstreeks op de opdrachtregel worden opgegeven.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Wachten op voltooiing van de orchestration

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse toont een [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) -API in .NET die kunnen worden gebruikt om op te halen synchroon de werkelijke uitvoer van een indeling het exemplaar. In JavaScript, de `DurableOrchestrationClient` klasse toont een `waitForCompletionOrCreateCheckStatusResponse` -API voor hetzelfde doel. De methoden gebruiken een standaardwaarde van tien seconden totdat de `timeout` en 1 seconde voor `retryInterval` wanneer ze niet worden ingesteld.  

Hier volgt een voorbeeld van de HTTP-trigger-functie die wordt gedemonstreerd hoe u deze API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

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
> De indeling van de webhook-URL's kan verschillen afhankelijk van welke versie van de Azure Functions-host die u uitvoert. Het vorige voorbeeld is voor de Azure Functions 2.x-host.

## <a name="retrieving-http-management-webhook-urls"></a>Bij het ophalen van HTTP-beheer Webhook-URL 's

Externe systemen kunnen communiceren met duurzame functies via de webhook-URL's die deel van het standaardantwoord dat wordt beschreven uitmaken in [HTTP API-URL detectie](durable-functions-http-api.md). Echter, de webhook-URL's ook toegankelijk via een programma in de orchestration-client of in een functie van de activiteit via de [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)klasse (.NET) of de `createHttpManagementPayload` -methode van de `DurableOrchestrationClient` klasse (JavaScript).

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) en `createHttpManagementPayload` één parameter hebben:

* **instanceId**: De unieke ID van het exemplaar.

De methoden retourneren een exemplaar van [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) (.NET) of een object (JavaScript) met de volgende eigenschappen van een verbindingsreeks:

* **Id**: De exemplaar-ID van de indeling (moet gelijk zijn aan de `InstanceId` invoer).
* **StatusQueryGetUri**: De URL van de status van de orchestration-exemplaar.
* **SendEventPostUri**: De URL 'raise gebeurtenis' van de orchestration-exemplaar.
* **TerminatePostUri**: De URL 'beëindigd' van de orchestration-exemplaar.
* **RewindPostUri**: De URL 'terugspoelen' van de orchestration-exemplaar.

Activiteitsfuncties kunnen u een exemplaar van deze objecten verzenden naar externe systemen om te controleren of het genereren van gebeurtenissen naar een indeling:

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, ctx) {
    const client = df.getClient(context);

    const payload = client.createHttpManagementPayload(ctx.instanceId);

    // send the payload to Cosmos DB
    context.bindings.document = JSON.stringify({
        id: ctx.instanceId,
        payload,
    });
};
```

## <a name="rewinding-instances-preview"></a>Terugspoelen exemplaren (preview)

Een mislukte orchestration-exemplaar kan worden *teruggespoeld* in een eerder status in orde met de [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) (.NET) of `rewindAsync` (JavaScript) API. Dit gebeurt door het plaatsen van de orchestration terug naar de *met* staat en opnieuw uit te voeren de uitvoering van activiteit en/of suborchestration fouten, die de orchestration-fout heeft veroorzaakt.

> [!NOTE]
> Deze API is niet bedoeld om te worden van een vervanging voor de juiste foutafhandeling en beleid voor opnieuw proberen. In plaats daarvan is het bedoeld om te worden gebruikt alleen in gevallen waar de orchestration-instanties om onverwachte redenen mislukken. Zie voor meer informatie over beleid voor fout verwerking en probeer het opnieuw, de [foutafhandeling](durable-functions-error-handling.md) onderwerp.

Een voorbeeld van de use-case voor *terugspoelen* is een werkstroom met betrekking tot een reeks [menselijke goedkeuringen](durable-functions-concepts.md#human). Stel dat er zijn een reeks activiteitsfuncties zodat iemand op de hoogte dat hun goedkeuring is vereist en uit het realtime antwoord wachten. Nadat alle van de goedkeuring activiteiten hebben ontvangen antwoorden of is een time-out, een andere activiteit mislukt vanwege een onjuiste configuratie van toepassing, zoals een ongeldige databaseverbindingsreeks. Het resultaat is een fout orchestration diep in de werkstroom. Met de `RewindAsync` (.NET) of `rewindAsync` (JavaScript) API, fout in de configuratie voor een beheerder van de toepassing kunt oplossen en *terugspoelen* de mislukte orchestration terug naar de status direct vóór de fout. Geen van de stappen menselijke tussenkomst moet opnieuw worden goedgekeurd en de indeling kan nu worden uitgevoerd.

> [!NOTE]
> De *terugspoelen* terugspoelen orchestration exemplaren die gebruikmaken van duurzame timers wordt niet ondersteund door de functie.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

### <a name="using-core-tools"></a>Core-hulpprogramma's gebruiken

Het is ook mogelijk terugspoelen een orchestration-exemplaar rechtstreeks via de [Core Tools](../functions-run-local.md) `durable rewind` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar
* **`reason` (optioneel)** : Reden voor de orchestration-instantie terugspoelen
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Exemplaar Geschiedenis leegmaken

> [!NOTE]
> De `PurgeInstanceHistoryAsync` -API is momenteel alleen beschikbaar voor C#. Deze wordt toegevoegd aan JavaScript in een toekomstige release.

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

* **`created-after` (optioneel)** : Opschonen van de geschiedenis van exemplaren die zijn gemaakt na deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`created-before` (optioneel)** : Opschonen van de geschiedenis van exemplaren die zijn gemaakt vóór deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`runtime-status` (optioneel)** : Opschonen van de geschiedenis van exemplaren met de status overeenkomen met deze ('actief', 'voltooid', enzovoort). Biedt meerdere statussen van (gescheiden door spaties).
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht verwijdert de geschiedenis van alle mislukte exemplaren die zijn gemaakt vóór 14 November 2018 om 19:35 uur (UTC).

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="deleting-a-task-hub"></a>Een taak Hub verwijderen

Met behulp van de [Core Tools](../functions-run-local.md) `durable delete-task-hub` opdracht, is het mogelijk om te verwijderen van alle opslag-artefacten die zijn gekoppeld aan een bepaalde taak hub. Dit omvat Azure storage-tabellen, wachtrijen en blobs. De opdracht heeft twee parameters:

* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub duurzame taak te gebruiken. De standaardwaarde is DurableFunctionsHub. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json) via durableTask:HubName.

De volgende opdracht verwijdert alle Azure storage-gegevens die zijn gekoppeld aan de hub 'UserTest'-taak.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van de HTTP-APIs bijvoorbeeld management](durable-functions-http-api.md)
