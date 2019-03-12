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
ms.openlocfilehash: ee96bc5e17051ab37be34eecbb8e4fe35599cd5d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57547306"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>-Exemplaren in duurzame functies in Azure beheren

Als u de [duurzame functies](durable-functions-overview.md) -extensie voor Azure Functions of wilt starten in dat geval, zorg ervoor dat u het beste gebruik buiten deze krijgt. U kunt uw duurzame functies orchestration-instanties optimaliseren door meer informatie over hoe u ze kunt beheren. In dit artikel gaat in op de details van elke bewerking voor het beheer van exemplaar.

U kunt starten en beëindigen van instanties, bijvoorbeeld, en u kunt een query-instanties, inclusief de mogelijkheid om op te vragen van alle exemplaren en query-exemplaren met filters. Bovendien kunt u gebeurtenissen verzenden naar instanties, wachten op voltooiing van de indeling en HTTP management webhook-URL's ophalen. In dit artikel bevat informatie over andere beheerbewerkingen, met inbegrip van exemplaren terugspoelen, exemplaar Geschiedenis wissen en verwijderen van een taak-hub.

In duurzame functies hebt u opties voor de manier waarop u wilt implementeren op elk van deze bewerkingen. Dit artikel biedt voorbeelden die gebruikmaken van de [Azure Functions Core Tools](../functions-run-local.md) voor zowel .NET (C#) en JavaScript.

## <a name="start-instances"></a>Exemplaren worden gestart

Het is belangrijk dat u om een exemplaar van de indeling te starten. Dit gebeurt vaak wanneer u een binding duurzame functies in een andere functie trigger.

De [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) of `startNew` op de `DurableOrchestrationClient` (JavaScript) start een nieuw exemplaar. Voor het verkrijgen van de exemplaren van deze klasse met behulp van de `orchestrationClient` binding. Intern maakt de enqueues van deze methode een bericht in de wachtrij besturingselement, dat vervolgens wordt het begin van een functie met de opgegeven naam die gebruikmaakt van de `orchestrationTrigger` binding activeren.

Deze asynchrone bewerking is voltooid wanneer de orchestration-proces met succes is gepland. De orchestration-proces moet worden gestart binnen 30 seconden. Als het langer duurt, ziet u een `TimeoutException`.

> [!WARNING]
> Bij het ontwikkelen van lokaal in JavaScript, stel de omgevingsvariabele `WEBSITE_HOSTNAME` naar `localhost:<port>` (bijvoorbeeld `localhost:7071`) te gebruiken methoden op `DurableOrchestrationClient`. Zie voor meer informatie over deze vereiste de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="net"></a>.NET

De parameters voor [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) zijn als volgt:

* **Naam**: De naam van de orchestrator-functie te plannen.
* **Invoer**: JSON-geserialiseerd gegevens die moet worden doorgegeven als invoer voor de orchestrator-functie.
* **InstanceId**: (Optioneel) De unieke ID van het exemplaar. Als u deze parameter niet opgeeft, de methode maakt gebruik van een willekeurige ID.

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
* **InstanceId**: (Optioneel) De unieke ID van het exemplaar. Als u deze parameter niet opgeeft, de methode maakt gebruik van een willekeurige ID.
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

> [!TIP]
> Gebruik een willekeurige id voor de exemplaar-ID. Dit zorgt ervoor een gelijke verdeling wanneer u bent bezig met het orchestrator-functies op meerdere virtuele machines. De juiste tijd voor het gebruik van niet-willekeurig exemplaar-id's is als de ID moet afkomstig zijn van een externe bron, of wanneer u implementeert de [singleton orchestrator](durable-functions-singletons.md) patroon.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

U kunt ook een exemplaar starten via de [Azure Functions Core Tools](../functions-run-local.md) `durable start-new` opdracht. Het bevat de volgende parameters:

* **`function-name` (vereist)** : Naam van de functie om te starten.
* **`input` (optioneel)** : Voer voor de functie, een inline of via een JSON-bestand. Voor bestanden, kunt u een voorvoegsel toegevoegd aan het pad naar het bestand met de `@`, zoals `@path/to/file.json`.
* **`id` (optioneel)** : ID van de orchestration-exemplaar. Als u deze parameter niet opgeeft, gebruikt de opdracht een willekeurige GUID.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is AzureWebJobsStorage.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is DurableFunctionsHub. U kunt dit ook instellen [host.json](durable-functions-bindings.md#host-json) met behulp van durableTask:HubName.

> [!NOTE]
> Core-hulpprogramma's voor opdrachten wordt ervan uitgegaan dat u ze uitvoert vanuit de hoofdmap van een functie-app. Als u expliciet de `connection-string-setting` en `task-hub-name` parameters, kunt u de opdrachten uitvoeren vanuit elke gewenste map. Hoewel u deze opdrachten zonder een functie-app-host waarop wordt uitgevoerd uitvoeren kunt, kan het gebeuren dat u sommige effecten kan niet ziet, tenzij de host wordt uitgevoerd. Bijvoorbeeld, de `start-new` opdracht enqueues een startbericht in de doel-taak hub, maar de indeling niet daadwerkelijk wordt uitgevoerd, tenzij er een hostproces van de functie-app uitgevoerd die het bericht kan verwerken.

De volgende opdracht wordt de functie met de naam HelloWorld gestart en geeft u de inhoud van het bestand `counter-data.json` toe:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="query-instances"></a>Query-instanties

Als onderdeel van uw inspanningen voor het beheren van uw indelingen, moet u waarschijnlijk voor het verzamelen van informatie over de status van een exemplaar van de indeling (bijvoorbeeld, of het is normaal gesproken voltooid of mislukt).

De [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) methode voor het [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET) of de `getStatus` methode voor de `DurableOrchestrationClient` klasse (JavaScript) de status van een indeling opvragen het exemplaar.

Het duurt voordat een `instanceId` (vereist), `showHistory` (optioneel), `showHistoryOutput` (optioneel), en `showInput` (optioneel, alleen .NET) als parameters.

* **`showHistory`**: Indien ingesteld op `true`, het antwoord bevat de uitvoeringsgeschiedenis.
* **`showHistoryOutput`**: Indien ingesteld op `true`, de uitvoeringsgeschiedenis bevat uitvoer voor activiteiten.
* **`showInput`**: Indien ingesteld op `false`, de reactie niet in de invoer van de functie opgenomen. De standaardwaarde is `true`. (Alleen voor .NET)

De methode retourneert een JSON-object met de volgende eigenschappen:

* **Naam**: De naam van de orchestrator-functie.
* **InstanceId**: De exemplaar-ID van de indeling (moet gelijk zijn aan de `instanceId` invoer).
* **Aanmaaktijd**: De tijd waarop de orchestrator-functie is gestart.
* **LastUpdatedTime**: Het tijdstip waarop de orchestration laatste controlepunt.
* **Invoer**: De invoer van de functie als een JSON-waarde. Dit veld wordt niet ingevuld als `showInput` is ingesteld op false.
* **CustomStatus**: De status van de aangepaste indeling in JSON-indeling.
* **Uitvoer**: De uitvoer van de functie als een JSON-waarde (als de functie is voltooid). Als de orchestrator-functie is mislukt, bevat deze eigenschap de foutdetails. Als de orchestrator-functie is beëindigd, bevat deze eigenschap de reden voor de beëindiging (indien aanwezig).
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

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Het is ook mogelijk de status van een exemplaar van de orchestration rechtstreeks ophalen met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable get-runtime-status` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar.
* **`show-input` (optioneel)** : Indien ingesteld op `true`, het antwoord bevat de invoer van de functie. De standaardwaarde is `false`.
* **`show-output` (optioneel)** : Indien ingesteld op `true`, het antwoord bevat de uitvoer van de functie. De standaardwaarde is `false`.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

De volgende opdracht wordt de status (met inbegrip van de invoer en uitvoer) van een exemplaar met de exemplaar-ID orchestration van 0ab8c55a66644d68a3a8b220b12d209c opgehaald. Hierbij wordt ervan uitgegaan dat u gebruikt de `func` opdracht uit vanaf de hoofdmap van de functie-app:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

U kunt de `durable get-history` opdracht voor het ophalen van de geschiedenis van een orchestration-exemplaar. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Het kan ook worden ingesteld in host.json, met behulp van durableTask:HubName.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="query-all-instances"></a>Query uitvoeren op alle instanties

In plaats van één exemplaar van query's in uw orchestration tegelijk, misschien vindt u het efficiënter om op te vragen ze allemaal in één keer.

U kunt de `GetStatusAsync` (.NET) of `getStatusAll` methode om op te vragen van de status van alle exemplaren van de indeling (JavaScript). In .NET, geeft u een `CancellationToken` object in het geval u wilt annuleren. De methode retourneert objecten met dezelfde eigenschappen als de `GetStatusAsync` methode met parameters.

### <a name="c"></a>C#

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
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

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Het is ook mogelijk direct query-exemplaren met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable get-instances` opdracht. Het bevat de volgende parameters:

* **`top` (optioneel)** : Met deze opdracht biedt ondersteuning voor paginering. Deze parameter komt overeen met het aantal exemplaren per aanvraag wordt opgehaald. De standaardwaarde is 10.
* **`continuation-token` (optioneel)** : Een token om aan te geven welke pagina of sectie van instanties om op te halen. Elke `get-instances` uitvoering retourneert een token met de volgende exemplaren.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

```bash
func durable get-instances
```

## <a name="query-instances-with-filters"></a>Query-exemplaren met filters

Wat gebeurt er als u echt hoeft niet alle informatie die een query standaardinstantie kan bieden? Bijvoorbeeld, wat gebeurt er als u alleen zoekt de aanmaaktijd van de indeling of de status van de orchestration-runtime? U kunt uw zoekopdracht verfijnen door filters zijn toegepast.

Gebruik de `GetStatusAsync` (.NET) of `getStatusBy` (JavaScript)-methode voor het ophalen van een lijst van de orchestration-instanties die overeenkomen met een set vooraf gedefinieerde filters.

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

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

In de Azure Functions Core Tools, kunt u ook gebruiken de `durable get-instances` opdracht met filters. Naast de voornoemde `top`, `continuation-token`, `connection-string-setting`, en `task-hub-name` parameters, kunt u drie parameters filteren (`created-after`, `created-before`, en `runtime-status`).

* **`created-after` (optioneel)** : Ophalen van de exemplaren die zijn gemaakt na deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`created-before` (optioneel)** : Ophalen van de exemplaren die zijn gemaakt vóór deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`runtime-status` (optioneel)** : Ophalen van de exemplaren met een bepaalde status (bijvoorbeeld wordt uitgevoerd of voltooid). Biedt meerdere statussen van (gescheiden door spaties).
* **`top` (optioneel)** : Aantal instanties worden opgehaald per aanvraag. De standaardwaarde is 10.
* **`continuation-token` (optioneel)** : Een token om aan te geven welke pagina of sectie van instanties om op te halen. Elke `get-instances` uitvoering retourneert een token met de volgende exemplaren.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

Als u geen filters (`created-after`, `created-before`, of `runtime-status`), de opdracht haalt gewoon `top` exemplaren met geen betrekking op de runtime-status of het maken van tijd.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminate-instances"></a>Exemplaren beëindigen

Als u hebt een orchestration-exemplaar dat duurt te lang om uit te voeren en u alleen hoeft te voorkomen dat deze voordat het klaar voor een bepaalde reden is, hebt u de optie deze te beëindigen.

Kunt u de [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET), of de `terminate` -methode van de `DurableOrchestrationClient` klasse (JavaScript). De twee parameters zijn een `instanceId` en een `reason` tekenreeks, die worden geschreven naar Logboeken en naar de status van het exemplaar. Een beëindigde exemplaar niet meer werkt als het de volgende bereikt `await` (.NET) of `yield` punt (JavaScript), of deze wordt beëindigd onmiddellijk als deze al op een `await` of `yield`.

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
> Exemplaar-beëindiging doorgeven op dit moment niet. Activiteitsfuncties en subquery indelingen uitvoeren tot voltooiing, ongeacht of u de orchestration-exemplaar dat ze met de naam hebt beëindigd.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

U kunt ook een orchestration-exemplaar rechtstreeks beëindigen met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable terminate` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-instantie moet worden afgesloten.
* **`reason` (optioneel)** : De reden van beëindiging.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

De volgende opdracht wordt een orchestration-exemplaar met de ID van 0ab8c55a66644d68a3a8b220b12d209c beëindigd:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="send-events-to-instances"></a>Gebeurtenissen verzenden naar exemplaren

In sommige scenario's is het belangrijk voor uw orchestrator-functies mogelijk te wachten en luisteren naar externe gebeurtenissen. Dit omvat [functions controleren](durable-functions-concepts.md#monitoring) en functies die wachten op [menselijke tussenkomst](durable-functions-concepts.md#human).

Meldingen van gebeurtenissen verzenden naar het uitvoeren van exemplaren met behulp van de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET) of de `raiseEvent` -methode van de `DurableOrchestrationClient` klasse () JavaScript). Instanties die deze gebeurtenissen kunnen worden verwerkt zijn die in afwachting van een aanroep van [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.NET) of `waitForExternalEvent` (JavaScript).

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

> [!IMPORTANT]
> Als er is geen exemplaar orchestration met het opgegeven exemplaar-ID, of als het exemplaar is niet wachten op de naam van de opgegeven gebeurtenis, wordt het gebeurtenisbericht genegeerd. Zie voor meer informatie over dit gedrag, de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-extension/issues/29).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

U kunt ook een gebeurtenis in een orchestration-exemplaar rechtstreeks verhogen met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable raise-event` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar.
* **`event-name` (optioneel)** : De naam van de gebeurtenis te verhogen. De standaardwaarde is `$"Event_{RandomGUID}"`.
* **`event-data` (optioneel)** : Gegevens worden verzonden naar de orchestration-exemplaar. Dit kan het pad naar een JSON-bestand, of kunt u zorgen dat de gegevens rechtstreeks op de opdrachtregel.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Wachten op voltooiing van de orchestration

U kunt in langlopende indelingen, wacht en ophalen van de resultaten van een indeling. In dergelijke gevallen is het ook handig om te kunnen zijn voor het definiëren van een time-outperiode op de indeling. Als de time-out is verstreken, moet de status van de indeling in plaats van de resultaten worden geretourneerd.

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse toont een [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) -API in .NET. Deze API kunt u de werkelijke uitvoer van een exemplaar van de orchestration synchroon ophalen. In JavaScript, de `DurableOrchestrationClient` klasse toont een `waitForCompletionOrCreateCheckStatusResponse` -API voor hetzelfde doel. Wanneer ze niet zijn ingesteld, gebruik de methoden een standaardwaarde van tien seconden totdat de `timeout`, en 1 seconde voor `retryInterval`.  

Hier volgt een voorbeeld van de HTTP-trigger-functie die wordt gedemonstreerd hoe u deze API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

Roep de functie met de volgende regel. Gebruik 2 seconden voor de time-out en 0,5 seconden voor het interval voor opnieuw proberen:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Afhankelijk van de tijd die nodig is om op te halen van het antwoord van de orchestration-exemplaar, zijn er twee mogelijke situaties:

* De orchestration-instanties binnen de gedefinieerde time-out (in dit geval 2 seconden) is voltooid en het antwoord is de daadwerkelijke orchestration exemplaar uitvoer, synchroon geleverd:

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

* De orchestration-exemplaren kunnen niet worden voltooid binnen de gedefinieerde time-out en het antwoord is de standaard één dat wordt beschreven in [HTTP API-URL detectie](durable-functions-http-api.md):

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
> De indeling van de webhook-URL's mogelijk verschillen, afhankelijk van welke versie van de Azure Functions-host die u uitvoert. Het vorige voorbeeld is voor de Azure Functions 2.x-host.

## <a name="retrieve-http-management-webhook-urls"></a>HTTP-beheer webhook-URL's ophalen

U kunt een extern systeem gebruiken om te controleren of het genereren van gebeurtenissen naar een indeling. Externe systemen kunnen communiceren met duurzame functies via de webhook-URL's die deel van het standaardantwoord dat wordt beschreven uitmaken in [HTTP API-URL detectie](durable-functions-http-api.md). Echter, de webhook-URL's ook toegankelijk via een programma in de orchestration-client of in een functie van de activiteit. Dit doen met behulp van de [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) -methode van de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasse (.NET), of de `createHttpManagementPayload` -methode van de `DurableOrchestrationClient` klasse (JavaScript).

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) en `createHttpManagementPayload` één parameter hebben:

* **instanceId**: De unieke ID van het exemplaar.

De methoden retourneren een exemplaar van [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) (.NET) of een object (JavaScript), met de volgende eigenschappen van een verbindingsreeks:

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

## <a name="rewind-instances-preview"></a>Terugspoelen exemplaren (preview)

Als u een orchestration-fout vanwege een onverwachte reden hebt, kunt u *terugspoelen* het exemplaar naar een eerder status met behulp van een API die is gebouwd voor dat doel.

> [!NOTE]
> Deze API is niet bedoeld om te worden van een vervanging voor de juiste foutafhandeling en beleid voor opnieuw proberen. In plaats daarvan is het bedoeld om te worden gebruikt alleen in gevallen waar de orchestration-instanties om onverwachte redenen mislukken. Zie voor meer informatie over beleid voor fout verwerking en probeer het opnieuw, de [foutafhandeling](durable-functions-error-handling.md) onderwerp.

Gebruik de [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) (.NET) of `rewindAsync` API om de indeling (JavaScript) terug in de *met* staat. Opnieuw met het uitvoeren van de uitvoering van activiteit of suborchestration fouten, die de orchestration-fout heeft veroorzaakt.

Bijvoorbeeld: Stel dat u hebt een werkstroom met betrekking tot een reeks [menselijke goedkeuringen](durable-functions-concepts.md#human). Stel dat er een reeks activiteitsfuncties zodat iemand die hun goedkeuring nodig is, en van de realtime-antwoord wacht op de hoogte te zijn. Nadat alle antwoorden hebt ontvangen van de goedkeuringsactiviteiten of is een time-out, Stel dat een andere activiteit vanwege een onjuiste configuratie van toepassing, zoals een ongeldige databaseverbindingsreeks mislukt. Het resultaat is een fout orchestration diep in de werkstroom. Met de `RewindAsync` (.NET) of `rewindAsync` (JavaScript) API, een toepassing-beheerder kan de configuratiefout oplossen, en terugspoelen de mislukte orchestration terug naar de status direct vóór de fout. Geen van de stappen menselijke tussenkomst moet opnieuw worden goedgekeurd en de indeling kan nu worden uitgevoerd.

> [!NOTE]
> De *terugspoelen* functie biedt geen ondersteuning voor terugspoelen orchestration exemplaren die gebruikmaken van duurzame timers.

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

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

U kunt ook een exemplaar van de orchestration terugspoelen rechtstreeks met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable rewind` opdracht. Het bevat de volgende parameters:

* **`id` (vereist)** : ID van de orchestration-exemplaar.
* **`reason` (optioneel)** : De reden voor de orchestration-instantie terugspoelen.
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Exemplaar Geschiedenis leegmaken

Als u wilt verwijderen van alle gegevens die zijn gekoppeld aan een indeling, kunt u de exemplaar-Geschiedenis wissen. Bijvoorbeeld, kunt u allerlei rijen van de Azure Table en grote berichten blobs, indien aanwezig. Om dit te doen, gebruikt u de [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_) API.

> [!NOTE]
> De `PurgeInstanceHistoryAsync` -API is momenteel alleen beschikbaar voor C#.

 De methode heeft twee overloads. Het eerste item Hiermee verwijdert u de geschiedenis van de ID van de orchestration-exemplaar:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

Het tweede voorbeeld ziet een timer geactiveerde functie die Hiermee verwijdert u de geschiedenis voor alle orchestration-exemplaren die voltooid na het opgegeven tijdsinterval. In dit geval verwijdert deze gegevens voor alle exemplaren die meer dan 30 dagen geleden is voltooid. Deze is ingesteld op één keer per dag om 12 uur worden uitgevoerd:

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
> Voor het proces tijd geactiveerde functie te voltooien, de runtimestatus van de moet **voltooid**, **beëindigd**, of **mislukt**.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

U kunt een exemplaar van de orchestration-Geschiedenis wissen met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable purge-history` opdracht. Vergelijkbaar met de tweede C# voorbeeld in de voorgaande sectie is het Hiermee verwijdert u de geschiedenis voor alle orchestration-exemplaren die zijn gemaakt tijdens een opgegeven tijdsinterval. U kunt opgeschoonde exemplaren verder filteren op runtimestatus. De opdracht heeft verschillende parameters:

* **`created-after` (optioneel)** : Opschonen van de geschiedenis van exemplaren die zijn gemaakt na deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`created-before` (optioneel)** : Opschonen van de geschiedenis van exemplaren die zijn gemaakt vóór deze datum/tijd (UTC). ISO 8601 opgemaakt datum/tijd geaccepteerd.
* **`runtime-status` (optioneel)** : Opschonen van de geschiedenis van instanties met een bepaalde status (bijvoorbeeld wordt uitgevoerd of voltooid). Biedt meerdere statussen van (gescheiden door spaties).
* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

De volgende opdracht verwijdert de geschiedenis van alle mislukte exemplaren die zijn gemaakt vóór 14 November 2018 om 19:35 uur (UTC).

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="delete-a-task-hub"></a>Een taak hub verwijderen

Met behulp van de [Azure Functions Core Tools](../functions-run-local.md) `durable delete-task-hub` opdracht, kunt u alle opslag-artefacten die zijn gekoppeld aan een bepaalde taak hub verwijderen. Dit omvat Azure storage-tabellen, wachtrijen en blobs. De opdracht heeft twee parameters:

* **`connection-string-setting` (optioneel)** : De naam van de toepassingsinstelling met daarin de verbindingsreeks voor opslag te gebruiken. De standaardwaarde is `AzureWebJobsStorage`.
* **`task-hub-name` (optioneel)** : Naam van de hub van de taak duurzame functies te gebruiken. De standaardwaarde is `DurableFunctionsHub`. Kan ook worden ingesteld [host.json](durable-functions-bindings.md#host-json), met behulp van durableTask:HubName.

De volgende opdracht verwijdert u alle Azure storage-gegevens die zijn gekoppeld aan de `UserTest` taak hub.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van de HTTP-APIs bijvoorbeeld management](durable-functions-http-api.md)
