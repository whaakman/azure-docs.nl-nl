---
title: Duurzame functies patronen en technische concepten - Azure
description: Vindt u informatie over de werking van duurzame functies in Azure om in te schakelen van de voordelen van stateful code wordt uitgevoerd in de cloud.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: azfuncdf
ms.openlocfilehash: cecf7f7fb79b6d7ebeed051b018a1e18375d68b2
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54045918"
---
# <a name="durable-functions-patterns-and-technical-concepts"></a>Duurzame functies patronen en technische concepten

*Duurzame functies* is een uitbreiding van [Azure Functions](../functions-overview.md) en [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) waarmee u het schrijven van stateful functies in een serverloze omgeving. Met de extensie worden status, controlepunten en het opnieuw opstarten voor u beheerd. In dit artikel bevat meer gedetailleerde informatie over gedrag van de extensie duurzame functies voor Azure Functions en algemene patronen voor implementatie.

> [!NOTE]
> Duurzame functies is een geavanceerde-extensie voor Azure-functies die niet geschikt is voor alle toepassingen. De rest van dit artikel wordt ervan uitgegaan dat u een sterke vertrouwd zijn met hebt [Azure Functions](../functions-overview.md) concepten en de uitdagingen bij het ontwikkelen van serverloze toepassingen.

## <a name="patterns"></a>Patronen

Deze sectie beschrijft enkele typische toepassingspatronen die van duurzame functies profiteren kunnen.

### <a name="chaining"></a>Patroon #1: Functiekoppeling

*Functie-koppeling* verwijst naar het patroon van het uitvoeren van een reeks functies in een bepaalde volgorde. De uitvoer van een functie moet vaak worden toegepast op de invoer van een andere functie.

![Functie chaining diagram](./media/durable-functions-concepts/function-chaining.png)

Duurzame functies kunt u dit patroon beknopte in code te implementeren.

#### <a name="c-script"></a>C#-script

```cs
public static async Task<object> Run(DurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1");
        var y = await context.CallActivityAsync<object>("F2", x);
        var z = await context.CallActivityAsync<object>("F3", y);
        return  await context.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```

> [!NOTE]
> Er zijn subtiele verschillen tijdens het schrijven van een vooraf gecompileerde duurzame functie in C# Visual Studio het C#-script voorbeeld weergegeven voordat. Een vooraf geschreven voor C#-functie vereist duurzame parameters om te worden voorzien van de bijbehorende kenmerken. Een voorbeeld is `[OrchestrationTrigger]` voor het kenmerk `DurableOrchestrationContext` parameter. Als de parameters niet correct voorzien zijn, wordt de runtime zou niet mogelijk om toe te voegen van de variabelen voor de functie en fout krijgt. Ga naar [voorbeeld](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples) voor meer voorbeelden.

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```js
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return yield context.df.callActivity("F4", z);
});
```

De waarden 'F1', 'V2', 'V3' en "F4" zijn de namen van andere functies in de functie-app. Controlestroom is geïmplementeerd met behulp van de normale imperatieve constructies coderen. Dat wil zeggen, code wordt uitgevoerd boven naar beneden en kan worden gemaakt van bestaande taal control flow semantiek, zoals voorwaarden en lussen.  Foutafhandelingslogica kan worden opgenomen in een try/catch/finally-blokken.

De `context` parameter [DurableOrchestrationContext] \(.NET\) en de `context.df` object (JavaScript) bieden methoden voor het aanroepen van andere functies met de naam, het doorgeven van parameters, en functie-uitvoer wordt geretourneerd. Elke keer dat de code roept `await` (C#) of `yield` (JavaScript), het framework duurzame functies *controlepunten* de voortgang van het huidige exemplaar van de functie. Als het proces of de virtuele machine wordt gerecycled halverwege de uitvoering, het exemplaar van de functie wordt hervat vanaf de vorige `await` of `yield` aanroepen. Meer informatie over dit gedrag voor opnieuw opstarten later opnieuw.

> [!NOTE]
> De `context` in JavaScript-object vertegenwoordigt de [functie context](../functions-reference-node.md#context-object) als een hele, niet de [DurableOrchestrationContext].

### <a name="fan-in-out"></a>Patroon #2: Uit-/inwaaieren

*Fan-uitgaand/fan-in* verwijst naar het patroon voor het uitvoeren van meerdere functies tegelijk, en vervolgens wachten te voltooien.  Werk aggregatie is vaak uitgevoerd op de resultaten van de functies.

![Diagram fan-uitgaand/fan-in](./media/durable-functions-concepts/fan-out-fan-in.png)

Met normale functions kan airconditioningapparatuur uit worden gedaan door de functie meerdere berichten verzenden naar een wachtrij. Terug in airconditioningapparatuur is echter veel moeilijker. U moet code schrijven om te controleren wanneer de wachtrij-geactiveerde functies beëindigen en functie-uitvoer opslaan. De extensie duurzame functies verwerkt dit patroon met relatief eenvoudige code.

#### <a name="c-script"></a>C#-script

```cs
public static async Task Run(DurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // get a list of N work items to process in parallel
    object[] workBatch = await context.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```js
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // get a list of N work items to process in parallel
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // aggregate all N outputs and send result to F3
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

Het werk fanout wordt gedistribueerd naar meerdere exemplaren van de functie `F2`, en het werk wordt bijgehouden met behulp van een dynamische lijst met taken. De .NET `Task.WhenAll` API of JavaScript `context.df.Task.all` -API wordt aangeroepen om te wachten tot alle van de aangeroepen functies om te voltooien. Vervolgens wordt de `F2` functie voert worden samengevoegd in de lijst met dynamische en doorgegeven aan bij de `F3` functie.

De controlepunten die plaatsvindt op de `await` of `yield` aanroepen voor `Task.WhenAll` of `context.df.Task.all` zorgt ervoor dat een crash of opnieuw opstarten halverwege niet vereist opnieuw opstarten van een voltooide taken.

### <a name="async-http"></a>Patroon #3: Asynchrone HTTP-API 's

Het derde patroon draait alles om het probleem van het coördineren van de status van langlopende bewerkingen met externe clients. Een veelgebruikte manier voor het implementeren van dit patroon is door de actie langdurig is geactiveerd door een HTTP-aanroep, en vervolgens de client wordt omgeleid naar een status-eindpunt dat u ze controleren kunnen als u wilt weten wanneer de bewerking is voltooid.

![Diagram van de HTTP-API](./media/durable-functions-concepts/async-http-api.png)

Duurzame functies biedt ingebouwde API's waarmee de code die u schrijft voor interactie met langlopende functie uitvoeringen. De Quick Start-voorbeelden ([C#](durable-functions-create-first-csharp.md), [JavaScript](quickstart-js-vscode.md)) weergeven van een eenvoudige REST-opdracht die kan worden gebruikt om nieuwe orchestrator-functie-exemplaren worden gestart. Nadat een exemplaar is gestart, wordt de extensie webhook HTTP APIs deze query de status van de orchestrator-functie. Het volgende voorbeeld ziet de REST-opdrachten om te starten van een orchestrator en de status opvragen. Sommige gegevens zijn voor alle duidelijkheid worden weggelaten uit het voorbeeld.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

U hebt geen omdat de status wordt beheerd door de runtime duurzame functies, de status van uw eigen bijhouden mechanisme implementeren.

Hoewel de extensie duurzame functies ingebouwde webhooks heeft voor het beheren van langlopende indelingen, u kunt implementeren om dit patroon zelf met behulp van uw eigen functie-triggers (zoals HTTP-, wachtrij of Event Hub) en de `orchestrationClient` binding. U kunt bijvoorbeeld een wachtrijbericht gebruiken voor het activeren van beëindiging.  Of u een HTTP-trigger die is beveiligd met een verificatiebeleid van Azure Active Directory in plaats van de ingebouwde webhooks die gebruikmaken van een gegenereerde sleutel voor verificatie kan gebruiken.

#### <a name="c"></a>C#

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
// HTTP-triggered function to start a new orchestrator function instance.
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);

    // Function name comes from the request URL.
    // Function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(req, instanceId);
};
```

> [!WARNING]
> Bij het ontwikkelen van lokaal in JavaScript, moet u de omgevingsvariabele instellen `WEBSITE_HOSTNAME` naar `localhost:<port>`, bijvoorbeeld. `localhost:7071` gebruik van methoden op `DurableOrchestrationClient`. Zie voor meer informatie over deze vereiste de [GitHub-probleem](https://github.com/Azure/azure-functions-durable-js/issues/28).

In .NET, de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parameter is een waarde van de `orchestrationClient` uitvoer verbinding maakt, deze maakt deel uit van de extensie duurzame functies. In JavaScript, dit object wordt geretourneerd door het aanroepen van `df.getClient(context)`. Deze objecten bieden methoden voor het starten, verzenden van gebeurtenissen, beëindigen en query's uitvoeren voor nieuwe of bestaande orchestrator-functie-exemplaren.

In het vorige voorbeeld wordt een HTTP-geactiveerde-functie maakt in een `functionName` waarde van de binnenkomende URL en stadia waarin de waarde die moet worden [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). De [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) vervolgens binden van de API retourneert een antwoord met een `Location` -header en aanvullende informatie over de instantie die later kan worden gebruikt om te controleren of u de status van het exemplaar gestart of te beëindigen het is.

### <a name="monitoring"></a>Patroon #4: Bewaking

Het patroon monitor verwijst naar een flexibele *terugkerende* proces in een werkstroom - bijvoorbeeld polling totdat aan bepaalde voorwaarden wordt voldaan. Een gewone [timertrigger](../functions-bindings-timer.md) kunt adres een eenvoudig scenario, zoals een periodieke opschoontaak, maar het interval is statisch en beheren van de levensduur van het exemplaar wordt complexe. Duurzame functies maakt flexibele terugkeerpatroon intervallen, kan beheer van de taak en de mogelijkheid het maken van de monitor voor meerdere processen uit een enkele orchestration.

Een voorbeeld zou het eerdere scenario van asynchrone HTTP-API worden omgekeerd. In plaats van een eindpunt voor een externe client voor het bewaken van een langdurige bewerking, verbruikt de monitor langlopende een extern eindpunt wachten op een andere status.

![Diagram van de monitor](./media/durable-functions-concepts/monitor.png)

Duurzame functies gebruikt, kunnen meerdere beeldschermen houden van willekeurige eindpunten worden gemaakt in een paar regels code. De monitors uitvoering kunnen beëindigen wanneer bepaalde voorwaarde wordt voldaan, of worden afgesloten met de [DurableOrchestrationClient](durable-functions-instance-management.md), en hun wachtinterval kan worden gewijzigd op basis van bepaalde voorwaarde (dat wil zeggen exponentieel uitstel.) De volgende code wordt een eenvoudige monitor geïmplementeerd.

#### <a name="c-script"></a>C#-script

```cs
public static async Task Run(DurableOrchestrationContext context)
{
    int jobId = context.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();

    while (context.CurrentUtcDateTime < expiryTime)
    {
        var jobStatus = await context.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform action when condition met
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform further work here, or let the orchestration end
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform action when condition met
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform further work here, or let the orchestration end
});
```

Wanneer een aanvraag wordt ontvangen, wordt een nieuw orchestration-exemplaar gemaakt voor die taak-ID. Het exemplaar pollt status totdat een voorwaarde wordt voldaan en de lus wordt afgesloten. Een duurzame timer wordt gebruikt om te bepalen het polling-interval. Meer werk kan vervolgens worden uitgevoerd of de indeling kunt beëindigen. Wanneer de `context.CurrentUtcDateTime` (.NET) of `context.df.currentUtcDateTime` (JavaScript) groter is dan de `expiryTime`, de ends monitor.

### <a name="human"></a>Patroon #5: Menselijke tussenkomst

Veel processen hebben betrekking op een soort menselijke tussenkomst. Het lastig wat over met betrekking tot mensen in een geautomatiseerd proces is dat mensen nog niet altijd als maximaal beschikbaar en responsief zijn als cloudservices. Geautomatiseerde processen moeten toestaan voor deze, en ze vaak doen met behulp van de time-outs en compensatie logica.

Een voorbeeld van een bedrijfsproces dat betrekking heeft op menselijke tussenkomst is een goedkeuringsproces. Goedkeuring van een manager kan bijvoorbeeld zijn vereist voor een onkostennota die hoger is dan een bepaalde hoeveelheid. Als de manager worden niet goedgekeurd binnen 72 uur (mogelijk omdat zij op vakantie ging), begint het proces van een uitbreiding om op te halen van de goedkeuring van iemand anders (bijvoorbeeld de manager van de manager).

![Diagram van menselijke tussenkomst](./media/durable-functions-concepts/approval.png)

Dit patroon kan worden geïmplementeerd met behulp van een orchestrator-functie. De orchestrator gebruikt een [duurzame timer](durable-functions-timers.md) goedkeuring aanvragen en in het geval van time-out escaleren. Het zou wachten op een [externe gebeurtenis](durable-functions-external-events.md), die de melding die worden gegenereerd door sommige menselijke tussenkomst zou zijn.

#### <a name="c-script"></a>C#-script

```cs
public static async Task Run(DurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = context.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = context.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = context.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await context.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await context.CallActivityAsync("Escalate");
        }
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```js
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("RequestApproval");

    const dueTime = moment.utc(context.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = context.df.createTimer(dueTime.toDate());

    const approvalEvent = context.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield context.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield context.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield context.df.callActivity("Escalate");
    }
});
```

De timer voor het duurzaam is gemaakt door het aanroepen van `context.CreateTimer` (.NET) of `context.df.createTimer` (JavaScript). De melding wordt ontvangen door `context.WaitForExternalEvent` (.NET) of `context.df.waitForExternalEvent` (JavaScript). En `Task.WhenAny` (.NET) of `context.df.Task.any` (JavaScript) aangeroepen om te bepalen of uit te breiden (time-out gebeurt eerst) of goedkeuring verwerken (goedkeuring wordt ontvangen voordat de time-out).

Een externe client kan de gebeurtenismelding leveren aan een wachtrij orchestrator-functie met behulp van de [ingebouwde HTTP APIs](durable-functions-http-api.md#raise-event) of met behulp van [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) -API van een andere functie:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

## <a name="the-technology"></a>De technologie

Achter de schermen, wordt de extensie duurzame functies gebouwd boven de [duurzame taak Framework](https://github.com/Azure/durabletask), een open-source-bibliotheek op GitHub voor het bouwen van duurzame taak indelingen. Zoals Azure Functions hoe de serverloze ontwikkeling van Azure WebJobs is, is veel duurzame functies de serverloze ontwikkeling van duurzame taak Framework. Het duurzame Framework van de taak wordt gebruikt sterk binnen Microsoft en buiten ook om essentiële processen te automatiseren. Het is perfect voor de serverloze Azure Functions-omgeving.

### <a name="event-sourcing-checkpointing-and-replay"></a>' Event sourcing ', plaatsen van controlepunten en herhaling

Orchestrator-functies de status van de scriptuitvoering met behulp van een ontwerppatroon bekend als betrouwbaar kunnen onderhouden [' Event sourcing '](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). In plaats van rechtstreeks opslaan van de *huidige* status van een indeling, de extensie duurzame een archief waaraan alleen gebruikt om vast te leggen de *volledige reeks acties* genomen door de functie-indeling. Dit heeft veel voordelen, waaronder verbetering van prestaties, schaalbaarheid en reactiesnelheid in vergelijking met de volledige runtime-state dumpen 'van'. Andere voordelen omvatten uiteindelijke consistentie voor transactiegegevens en onderhouden van volledige audittrails en geschiedenis. De audittrails zelf betrouwbare compenserende maatregelen mogelijk maken.

Het gebruik van ' Event sourcing ' door deze uitbreiding is transparant. Op de achtergrond de `await` (C#) of `yield` operator in een orchestrator-functie (JavaScript) levert de controle van de orchestrator-thread terug naar het verdeelprogramma duurzame taak Framework. De functie voor berichtverzending doorvoeringen vervolgens nieuwe maatregelen die tegen de orchestrator-functie gepland (zoals een of meer onderliggende functies aanroepen of een duurzame timer plannen) naar de opslag. Deze actie transparante doorvoeren wordt toegevoegd aan de *uitvoeringsgeschiedenis* van de orchestration-instantie. De geschiedenis wordt opgeslagen in een opslagtabel. De commit-actie vervolgens een bericht toevoegt aan een wachtrij voor het plannen van het echte werk. De orchestrator-functie kan op dit moment worden verwijderd uit het geheugen. Facturering voor deze stopt als u de Azure Functions-Verbruiksabonnement.  Wanneer er meer werk te doen, wordt de functie opnieuw is opgestart en wordt de status is opnieuw opgebouwd.

Zodra een orchestration-functie meer werk te doen is opgegeven (bijvoorbeeld een antwoordbericht wordt ontvangen of een duurzame timer verloopt), de orchestrator ontwaakt opnieuw en voert u de gehele functie vanaf het begin opnieuw om de status van de lokale opnieuw. Als u tijdens deze herhaling probeert de code voor het aanroepen van een functie (of een andere asynchrone werk), het duurzame taak Framework consults met de *uitvoeringsgeschiedenis* van de huidige indeling. Als deze wordt gevonden die de [activiteit functie](durable-functions-types-features-overview.md#activity-functions) is al uitgevoerd en i/o sommige resultaat, het resultaat van deze functie opnieuw weergegeven en de orchestrator-code verder wordt uitgevoerd. Dit blijft plaatsvinden totdat de functiecode op een punt waar deze is voltooid of geplande nieuwe tot het asynchrone werk heeft opgehaald.

### <a name="orchestrator-code-constraints"></a>Beperkingen voor orchestrator-code

Het gedrag voor opnieuw afspelen maakt beperkingen met betrekking tot het type van de code die kan worden geschreven in een orchestrator-functie. Orchestrator-code is moet bijvoorbeeld deterministische, omdat deze wordt meerdere keren opnieuw worden afgespeeld en hetzelfde resultaat telkens moet opleveren. De volledige lijst met beperkingen kan worden gevonden de [Orchestrator code beperkingen](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) sectie van de **plaatsen van controlepunten en opnieuw opstarten** artikel.

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose

De extensie duurzame functies verzendt automatisch bijhouden voor gestructureerde gegevens [Application Insights](../functions-monitoring.md) als de functie-app is geconfigureerd met een Application Insights-instrumentatiesleutel. Deze traceergegevens kan worden gebruikt voor het bewaken van het gedrag en de voortgang van de indelingen.

Hier volgt een voorbeeld van hoe de duurzame functies bijhouden van gebeurtenissen in de Application Insights portal met eruit zien [Application Insights Analytics](../../application-insights/app-insights-analytics.md):

![App Insights-queryresultaten](./media/durable-functions-concepts/app-insights-1.png)

Er is een grote hoeveelheid nuttig gestructureerde gegevens die zijn verpakt in de `customDimensions` veld zijn in elke logboekvermelding. Hier volgt een voorbeeld van een dergelijke vermelding volledig worden uitgevouwen.

![customDimensions veld in de App Insights-query](./media/durable-functions-concepts/app-insights-2.png)

Vanwege het gedrag van de herhaling van de verzender duurzame taak Framework, kunt u verwachten redundante logboekvermeldingen voor herhaalde acties. Dit kan nuttig zijn om het gedrag van de herhaling van de core-engine te begrijpen zijn. De [Diagnostics](durable-functions-diagnostics.md) artikel ziet u query's die opnieuw afspelen Logboeken filteren zodat u alleen de 'realtime' Logboeken kunt zien.

## <a name="storage-and-scalability"></a>Opslag en schaalbaarheid

De extensie duurzame functies maakt gebruik van Azure Storage-wachtrijen, tabellen en blobs om vast te leggen van de uitvoering van geschiedenis van status en het activeren een functie wordt uitgevoerd. Het standaardopslagaccount voor de functie-app kan worden gebruikt of u een afzonderlijk opslagaccount kunt configureren. U kunt een afzonderlijk account vanwege de maximale doorvoer van opslag. De orchestrator-code die u schrijft hoeft niet te (en niet) communiceren met de entiteiten in deze storage-accounts. De entiteiten worden rechtstreeks door de duurzame taak Framework beheerd als een implementatiedetail in.

Orchestrator-functies activiteitsfuncties plannen en hun antwoorden via de interne Wachtrijberichten ontvangen. Wanneer een functie-app wordt uitgevoerd in het verbruiksabonnement voor Azure Functions, deze wachtrijen worden gecontroleerd door de [Azure Functions schalen Controller](../functions-scale.md#how-the-consumption-plan-works) en nieuwe compute-exemplaren naar behoefte worden toegevoegd. Wanneer geschaald naar meerdere virtuele machines, wordt een orchestrator-functie kan uitvoeren op één virtuele machine terwijl activiteitsfuncties wordt uitgevoerd op meerdere verschillende virtuele machines. U kunt meer informatie vinden over het gedrag van de schaal van duurzame functies in [prestaties en schaal](durable-functions-perf-and-scale.md).

Table-opslag wordt gebruikt voor het opslaan van de uitvoeringsgeschiedenis voor orchestrator-accounts. Wanneer een exemplaar rehydrates op een bepaalde virtuele machine, haalt het de uitvoeringsgeschiedenis uit table storage ook zodat deze de status van de lokale kunt herstellen. Een van de handige dingen over een van de geschiedenis beschikbaar in de Table storage is dat u kunt bekijken en de geschiedenis van de indelingen met behulp van hulpprogramma's zoals [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md).

Storage-blobs worden hoofdzakelijk als een leasemechanisme gebruikt voor de coördinatie van de scale-out van de orchestration-exemplaren op meerdere virtuele machines. Ze worden ook gebruikt voor het opslaan van gegevens voor grote berichten die rechtstreeks in de tabellen of wachtrijen kunnen niet worden opgeslagen.

![Azure Storage Explorer-schermafbeelding](./media/durable-functions-concepts/storage-explorer.png)

> [!WARNING]
> Het is gemakkelijk en handig om te zien van de uitvoeringsgeschiedenis in de tabelopslag, voorkomen waarbij eventuele afhankelijkheden voor deze tabel. Het verschilt mogelijk als de extensie duurzame functies zich verder ontwikkelt.

## <a name="known-issues-and-faq"></a>Bekende problemen en veelgestelde vragen

Alle bekende problemen moeten worden bijgehouden in de [GitHub issues](https://github.com/Azure/azure-functions-durable-extension/issues) lijst. Als u een probleem opgetreden bij het probleem niet kunt vinden in GitHub, opent u een nieuw probleem en bevat een gedetailleerde beschrijving van het probleem.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over duurzame functies, [overzicht van functies voor duurzame functies (Azure Functions) en de functietypen](durable-functions-types-features-overview.md), of...

> [!div class="nextstepaction"]
> [Uw eerste duurzame functie maken](durable-functions-create-first-csharp.md)

[DurableOrchestrationContext]: https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html