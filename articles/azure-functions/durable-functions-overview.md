---
title: Overzicht van duurzame Functions - Azure
description: Inleiding tot de extensie duurzame functies voor Azure Functions.
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
ms.date: 04/30/2018
ms.author: azfuncdf
ms.openlocfilehash: d253562e0ecb0d53739a4cdc5f9747e33d7e1171
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="durable-functions-overview"></a>Overzicht van duurzame functies

*Duurzame functies* is een uitbreiding van [Azure Functions](functions-overview.md) en [Azure WebJobs](../app-service/web-sites-create-web-jobs.md) die u kunt schrijven stateful functies in een omgeving zonder server. De extensie beheert de status, controlepunten en opnieuw opstarten voor u.

De uitbreiding kunt u definiëren stateful werkstromen in een nieuw type aangeroepen functie een *orchestrator-functie*. Hier volgen enkele van de voordelen van orchestrator-functies:

* Deze definiëren werkstromen in code. Er is geen JSON-schema's of ontwerpers zijn vereist.
* Ze kunnen andere functies aanroepen synchroon en asynchroon. De uitvoer van opgeroepen functies kan worden opgeslagen op lokale variabelen.
* Ze automatisch controlepunt hun uitgevoerd wanneer de functie wacht op. Lokale status is nooit verloren gaan als het proces wordt gerecycled of de virtuele machine opnieuw is opgestart.

> [!NOTE]
> Duurzame Functions is een geavanceerde extensie voor Azure-functies die niet geschikt voor alle toepassingen. De rest van dit artikel wordt ervan uitgegaan dat u een sterke bekend bent met hebt [Azure Functions](functions-overview.md) concepten en de uitdagingen die zijn betrokken bij de ontwikkeling van de toepassing zonder server.

Het primaire gebruiksvoorbeeld voor duurzame functies is coördinatie van complexe, stateful problemen in zonder server toepassingen vereenvoudigen. De volgende secties worden enkele typische toepassing patronen die van duurzame functies profiteren kunnen.

## <a name="pattern-1-function-chaining"></a>Patroon #1: Functie chaining

*Functie chaining* verwijst naar het patroon van een reeks functies uitvoeren in een bepaalde volgorde. Vaak moet de uitvoer van een functie worden toegepast op de invoer van een andere functie.

![Functie chaining diagram](media/durable-functions-overview/function-chaining.png)

Duurzame functies kunt u dit patroon bondig in code te implementeren.

#### <a name="c"></a>C#

```cs
public static async Task<object> Run(DurableOrchestrationContext ctx)
{
    try
    {
        var x = await ctx.CallActivityAsync<object>("F1");
        var y = await ctx.CallActivityAsync<object>("F2", x);
        var z = await ctx.CallActivityAsync<object>("F3", y);
        return  await ctx.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```js
const df = require("durable-functions");

module.exports = df(function*(ctx) {
    const x = yield ctx.df.callActivityAsync("F1");
    const y = yield ctx.df.callActivityAsync("F2", x);
    const z = yield ctx.df.callActivityAsync("F3", y);
    return yield ctx.df.callActivityAsync("F4", z);
});
```

De waarden 'F1', 'F2', 'F3' en 'F4' zijn de namen van andere functies in de functie-app. Controlestroom wordt geïmplementeerd met behulp van de normale imperatieve constructies coderen. Dat wil zeggen, code wordt uitgevoerd boven naar beneden en kan betrekking hebben op bestaande control flow taalsemantiek, zoals voorwaardelijke en lussen.  Fout tijdens het verwerken van de logica kan worden opgenomen in een try/catch/finally-blokken.

De `ctx` parameter ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) biedt methoden voor het aanroepen van andere functies met de naam, de parameters doorgeven, en het retourneren van uitvoer van de functie. Elke keer dat de code aanroepen `await`, het framework duurzame functies *controlepunten* de voortgang van het huidige exemplaar van de functie. Als het proces of de VM wordt gerecycled halverwege via de uitvoering, het exemplaar van de functie wordt hervat vanuit de vorige `await` aanroepen. Meer informatie over dit gedrag voor opnieuw opstarten later.

## <a name="pattern-2-fan-outfan-in"></a>Patroon #2: Fan-uitgaand/fan-in

*Fan-uitgaand/fan-in* verwijst naar het patroon van meerdere functies parallel uitvoeren en vervolgens wachten te voltooien.  Vaak is aggregatie werk uitgevoerd op de resultaten van de functies.

![Diagram fan-uitgaand/fan-in](media/durable-functions-overview/fan-out-fan-in.png)

Met normale functies kan airconditioningapparatuur uit worden gedaan door de functie meerdere berichten verzenden naar een wachtrij. Terug in de airconditioningapparatuur is echter veel moeilijker. U moet schrijven van code om bij te houden wanneer de wachtrij-geactiveerde functies beëindigen en uitvoer van de functie opslaan. De extensie duurzame functies verwerkt dit patroon met relatief eenvoudige code.

#### <a name="c"></a>C#

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    var parallelTasks = new List<Task<int>>();
 
    // get a list of N work items to process in parallel
    object[] workBatch = await ctx.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = ctx.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }
 
    await Task.WhenAll(parallelTasks);
 
    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await ctx.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```js
const df = require("durable-functions");

module.exports = df(function*(ctx) {
    const parallelTasks = [];

    // get a list of N work items to process in parallel
    const workBatch = yield ctx.df.callActivityAsync("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(ctx.df.callActivityAsync("F2", workBatch[i]));
    }

    yield ctx.df.task.all(parallelTasks);

    // aggregate all N outputs and send result to F3
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield ctx.df.callActivityAsync("F3", sum);
});
```

Het werk fan-out wordt gedistribueerd naar meerdere exemplaren van de functie `F2`, en het werk wordt bijgehouden met behulp van een dynamische lijst met taken. De .NET `Task.WhenAll` -API wordt aangeroepen om te wachten tot alle opgeroepen functies om te voltooien. Vervolgens wordt de `F2`functie levert worden samengevoegd in de taaklijst met dynamische en doorgegeven aan bij de `F3` functie.

De controlepunten die plaatsvindt op de `await` aanroepen voor `Task.WhenAll` zorgt ervoor dat eventuele halverwege crash of opnieuw opstarten via geen vereist een herstart van alle taken al voltooid.

## <a name="pattern-3-async-http-apis"></a>Patroon #3: Async HTTP API 's

Het derde patroon gaat over het probleem van coördinatie van de status van langlopende bewerkingen met externe clients. Een veelgebruikte manier voor het implementeren van dit patroon is door de actie langlopende is geactiveerd door een HTTP-aanroep, en het omleiden van de client vervolgens naar een status-eindpunt dat u ze opvragen kunnen voor meer informatie over wanneer de bewerking is voltooid.

![Diagram van de HTTP-API](media/durable-functions-overview/async-http-api.png)

Duurzame functies biedt ingebouwde API's die de code schrijven voor interactie met langlopende functie uitvoeringen vereenvoudigen. De [voorbeelden](durable-functions-install.md) weergeven van een eenvoudige REST-opdracht die kan worden gebruikt voor nieuwe exemplaren van orchestrator-functie te starten. Nadat een exemplaar is gestart, wordt de extensie webhook HTTP APIs die query de status van de orchestrator-functie. Het volgende voorbeeld ziet de REST-opdrachten starten van een orchestrator en de status opvragen. Ter verduidelijking, worden de details van sommige weggelaten uit het voorbeeld.

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

Omdat de status wordt beheerd door de runtime duurzame functies, kunt u niet hebt voor het implementeren van uw eigen mechanisme voor bijhouden van de status.

Hoewel de uitbreiding voor duurzame functies er ingebouwde webhooks voor het beheren van langlopende integraties, kunt u implementeren dit patroon zelf met behulp van uw eigen triggers functie (zoals HTTP, wachtrij of Event Hub) en de `orchestrationClient` binding. U kunt bijvoorbeeld een wachtrijbericht beëindiging activeren.  Of u een HTTP-trigger beveiligd door een verificatiebeleid voor Azure Active Directory in plaats van de ingebouwde webhooks die een gegenereerde sleutel voor verificatie gebruiken kan gebruiken. 

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    TraceWriter log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.Info($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

De [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parameter is een waarde van de `orchestrationClient` uitvoer binding, die deel uitmaakt van de extensie duurzame functies. Het biedt methoden voor begin, verzenden van gebeurtenissen, wordt afgebroken en een query uitvoert voor nieuwe of bestaande orchestrator-functie-exemplaren. In het vorige voorbeeld is een HTTP-geactiveerd-functie wordt in een `functionName` waarde van de binnenkomende URL en geeft die naar waarde [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Deze binding-API retourneert een antwoord met een `Location` header en aanvullende informatie over de instantie die later kan worden gebruikt om te zoeken registreert de status van het exemplaar gestart of beëindigd deze.

## <a name="pattern-4-monitoring"></a>Patroon #4: controleren

Het patroon monitor verwijst naar een flexibel *terugkerende* proces in een werkstroom - bijvoorbeeld polling totdat aan bepaalde voorwaarden wordt voldaan. Een gewone timertrigger een eenvoudig scenario, zoals een periodieke opschoontaak kunt oplossen, maar het interval is statisch en het beheren van de levensduur van het exemplaar wordt complexe. Duurzame functies maakt flexibele terugkeerpatroon intervallen, Taakbeheer levensduur en de mogelijkheid om te maken van meerdere beeldschermen processen van een enkele orchestration.

Een voorbeeld zou de eerdere asynchrone HTTP API scenario worden omgekeerd. In plaats van het blootstellen van een eindpunt voor een externe client voor het bewaken van een langdurige bewerking de monitor langlopende gebruikt een extern eindpunt wachten op een andere status.

![Diagram van de monitor](media/durable-functions-overview/monitor.png)

Duurzame functies gebruikt, kunnen meerdere beeldschermen houden willekeurige eindpunten worden gemaakt in een paar regels code. De monitoren uitvoering kunnen beëindigen wanneer een bepaalde voorwaarde wordt voldaan, of worden afgesloten met de [DurableOrchestrationClient](durable-functions-instance-management.md), en hun wachtinterval kan worden gewijzigd op basis van een bepaalde voorwaarde (dat wil zeggen exponentieel uitstel.) De volgende code implementeert een basic-monitor.

#### <a name="c"></a>C#

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    int jobId = ctx.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();
    
    while (ctx.CurrentUtcDateTime < expiryTime) 
    {
        var jobStatus = await ctx.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform action when condition met
            await ctx.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        var nextCheck = ctx.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await ctx.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform further work here, or let the orchestration end
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```js
const df = require("durable-functions");
const df = require("moment");

module.exports = df(function*(ctx) {
    const jobId = ctx.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(ctx.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield ctx.df.callActivityAsync("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform action when condition met
            yield ctx.df.callActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        const nextCheck = moment.utc(ctx.df.currentUtcDateTime).add(pollingInterval, 's');
        yield ctx.df.createTimer(nextCheck.toDate());
    }

    // Perform further work here, or let the orchestration end
});
```

Wanneer een aanvraag wordt ontvangen, is een nieuw exemplaar van de orchestration gemaakt voor die taak-ID. Het exemplaar worden opgevraagd status totdat een voorwaarde wordt voldaan en dat de lus wordt afgesloten. Een duurzame timer wordt gebruikt voor het polling-interval. Werk verder kan worden uitgevoerd of de orchestration kunt beëindigen. Wanneer de `ctx.CurrentUtcDateTime` overschrijdt de `expiryTime`, de ends monitor.

## <a name="pattern-5-human-interaction"></a>Patroon #5: Menselijke tussenkomst

Veel processen hebben betrekking op een soort menselijke tussenkomst. Het lastig wat over met betrekking tot mensen in een geautomatiseerd proces is dat gebruikers niet altijd als maximaal beschikbaar is en reageert als cloudservices zijn. Geautomatiseerde processen moeten voldoende zijn voor dit en ze vaak doen met behulp van de logica voor time-outs en compensatie.

Een voorbeeld van een bedrijfsproces waarbij een menselijke tussenkomst is een goedkeuringsproces. Goedkeuring van een manager kan bijvoorbeeld zijn vereist voor een onkostenrapport die langer is dan een bepaald bedrag. Als de manager worden niet goedgekeurd binnen 72 uur (mogelijk zij op vakantie ging), weer een escalatieproces bij het ophalen van de goedkeuring van iemand anders (mogelijk van de manager manager).

![Diagram van menselijke tussenkomst](media/durable-functions-overview/approval.png)

Dit patroon kan worden geïmplementeerd met behulp van een orchestrator-functie. De orchestrator zou gebruiken een [duurzame timer](durable-functions-timers.md) goedkeuring aanvragen en escaleren in geval van een time-out. Werd gewacht voor een [externe gebeurtenis](durable-functions-external-events.md), die de melding die worden gegenereerd door een aantal menselijke tussenkomst zou zijn.

#### <a name="c"></a>C#

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    await ctx.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = ctx.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = ctx.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = ctx.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await ctx.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await ctx.CallActivityAsync("Escalate");
        }
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

```js
const df = require("durable-functions");
const moment = require('moment');

module.exports = df(function*(ctx) {
    yield ctx.df.callActivityAsync("RequestApproval");

    const dueTime = moment.utc(ctx.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = ctx.df.createTimer(dueTime.toDate());

    const approvalEvent = ctx.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield ctx.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield ctx.df.callActivityAsync("ProcessApproval", approvalEvent.result);
    } else {
        yield ctx.df.callActivityAsync("Escalate");
    }
});
```

De duurzame timer wordt gemaakt door het aanroepen van `ctx.CreateTimer`. De melding wordt ontvangen door `ctx.WaitForExternalEvent`. En `Task.WhenAny` aangeroepen om te bepalen of u wilt escaleren (time-out gebeurt eerst) of goedkeuring verwerken (goedkeuring wordt ontvangen voordat de time-out).

Een externe client de gebeurtenismelding kunt leveren aan een wachtrij orchestrator-functie met behulp van de [ingebouwde HTTP APIs](durable-functions-http-api.md#raise-event) of met behulp van [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) API van een andere functie:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

## <a name="the-technology"></a>De technologie

Achter de schermen worden de functies voor duurzame-extensie is gebouwd boven de [duurzame taak Framework](https://github.com/Azure/durabletask), een open source-bibliotheek op GitHub voor het bouwen van duurzame taak integraties. Veel is duurzame functies zoals Azure Functions hoe de zonder server evolutie van Azure WebJobs is, de zonder server evolutie van het duurzame taak Framework. Het duurzame taak Framework wordt gebruikt sterk binnen Microsoft en buiten evenals bedrijfskritieke processen te automatiseren. Het is een natuurlijke geschikt is voor de server is niet vereist Azure Functions-omgeving.

### <a name="event-sourcing-checkpointing-and-replay"></a>Bron van de gebeurtenis, plaatsen van controlepunten en opnieuw afspelen

Orchestrator-functies hun uitvoeringsstatus met behulp van een cloud-ontwerppatroon bekend als betrouwbaar kunnen onderhouden [gebeurtenis bron](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). In plaats van rechtstreeks opslaan van de *huidige* status van een orchestration, de duurzame uitbreiding gebruikt een archief alleen toevoegen voor het vastleggen van de *volledige reeks acties* die door de functie-orchestration. Dit heeft veel voordelen, waaronder verbetering van prestaties, schaalbaarheid en reactiesnelheid vergeleken met de runtimestatus van de volledige dumpen 'van'. Andere voordelen zijn uiteindelijke consistentie voorziet in transactionele gegevens en onderhouden van volledige audittrails en geschiedenis. De audittrails zelf inschakelen betrouwbare compensatie acties.

Het gebruik van de bron van gebeurtenis door deze uitbreiding is transparant. Onder de behandelt de `await` operator in een orchestrator-functie levert besturingselement van de orchestrator-thread terug naar de dispatcher duurzame taak Framework. De dispatcher voert vervolgens nieuwe acties die de orchestrator-functie gepland (zoals het aanroepen van een of meer onderliggende functies of een duurzame timer plannen) naar de opslag. Deze actie transparante doorvoeren wordt toegevoegd aan de *uitvoergeschiedenis* van de orchestration-exemplaar. De geschiedenis wordt opgeslagen in een tabel van de opslag. Vervolgens de doorvoeractie wordt berichten toegevoegd aan een wachtrij het echte werk te plannen. De orchestrator-functie kan op dit moment worden verwijderd uit het geheugen. Facturering voor stopt als u de Azure Functions verbruik plannen.  Wanneer er meer werk te doen, wordt de functie opnieuw is opgestart en wordt de status is opnieuw opgebouwd.

Wanneer een functie orchestration meer werk krijgt te doen (bijvoorbeeld: Er is een antwoordbericht ontvangen of een duurzame timer verloopt), de orchestrator opnieuw ontwaakt en voert u de gehele functie van het begin opnieuw om de status van de lokale opnieuw. Als tijdens deze replay de code probeert aan te roepen, een functie (of een andere asynchrone doen werk), het duurzame taak Framework raadpleegt met de *uitvoergeschiedenis* van de huidige orchestration. Als het wordt gevonden die de activiteit functie al is uitgevoerd en heeft een aantal resultaat opgeleverd, het resultaat van deze functie opnieuw weergegeven en de orchestrator-code wordt verder verwerkt. Dit blijft plaatsvinden totdat de functiecode opgehaald naar een punt waar dit is voltooid of er geplande nieuwe asynchrone werk.

### <a name="orchestrator-code-constraints"></a>Beperkingen van orchestrator-code

Het gedrag voor opnieuw afspelen maakt beperkingen voor het type code dat kan worden geschreven in een orchestrator-functie. Orchestrator-code is moet bijvoorbeeld deterministische, omdat deze wordt meerdere keren worden cookies en hetzelfde resultaat telkens moet produceren. De volledige lijst met beperkingen vindt u in de [Orchestrator code beperkingen](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) sectie van de **plaatsen van controlepunten en opnieuw opstarten** artikel.

## <a name="language-support"></a>Taalondersteuning

Momenteel C# (functies v1- en v2)- en JavaScript (alleen functies v2) zijn de enige ondersteunde talen voor duurzame functies. Dit omvat de orchestrator-functies en functies van de activiteit. In de toekomst, wordt er ondersteuning voor alle talen die ondersteuning biedt voor Azure Functions toevoegen. Zie de Azure Functions [GitHub-opslagplaats lijst](https://github.com/Azure/azure-functions-durable-extension/issues) om te zien van de laatste status van onze andere talen werken ondersteunen.

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose

De extensie duurzame functies verzendt automatisch bijhouden van gestructureerde gegevens naar [Application Insights](functions-monitoring.md) als de functie-app is geconfigureerd met een Application Insights-instrumentatiesleutel. Deze traceergegevens kan worden gebruikt voor het bewaken van het gedrag en de voortgang van uw integraties.

Hier volgt een voorbeeld van hoe de duurzame functies bijhouden van gebeurtenissen in de Application Insights portal met behulp van eruit [Application Insights Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![App Insights queryresultaten](media/durable-functions-overview/app-insights-1.png)

Er is een grote hoeveelheid nuttig gestructureerde gegevens die zijn verpakt in de `customDimensions` veld in elke logboekvermelding. Hier volgt een voorbeeld van een dergelijke vermelding volledig worden uitgevouwen.

![customDimensions veld in de App Insights query](media/durable-functions-overview/app-insights-2.png)

Vanwege de replay-gedrag van de verzender duurzame taak Framework, kunt u verwachten redundante logboekvermeldingen voor herhaald acties. Dit kan handig zijn om te begrijpen van het gedrag voor opnieuw afspelen van de core-engine zijn. De [Diagnostics](durable-functions-diagnostics.md) artikel bevat voorbeeldquery's die replay-Logboeken filteren zodat u alleen de logboeken van de 'real-time' kunt zien.

## <a name="storage-and-scalability"></a>Opslag en schaalbaarheid

De extensie duurzame functies maakt gebruik van Azure Storage-wachtrijen, tabellen en blobs om vast te leggen van de uitvoering van geschiedenis van status en trigger functie wordt uitgevoerd. Het standaardopslagaccount voor de functie-app kan worden gebruikt of u een afzonderlijke opslagaccount kunt configureren. U kunt een afzonderlijk account vanwege opslaglimieten doorvoer. De orchestrator-code die u schrijft niet hoeft te (en niet moet) communiceren met de entiteiten in deze opslagaccounts. De entiteiten worden rechtstreeks door de duurzame taak Framework beheerd als de details van een implementatie.

Orchestrator-functies activiteit functies plannen en hun antwoorden via de interne Wachtrijberichten ontvangen. Wanneer een functie-app wordt uitgevoerd in het verbruik van Azure Functions-plan, deze wachtrijen moeten worden bewaakt door de [Azure Functions schalen Controller](functions-scale.md#how-the-consumption-plan-works) en nieuwe berekening exemplaren worden toegevoegd, indien nodig. Uitgebreid naar meerdere virtuele machines, in dat een orchestrator-functie kan uitvoeren op één virtuele machine terwijl activiteit functies die wordt aangeroepen op verschillende andere virtuele machines worden uitgevoerd. Vindt u meer informatie over het gedrag van de schaal van duurzame functies in [prestaties en schaalbaarheid](durable-functions-perf-and-scale.md).

Tabelopslag wordt gebruikt voor het opslaan van de uitvoergeschiedenis voor orchestrator-accounts. Wanneer een exemplaar rehydrates op een bepaalde virtuele machine, haalt het de uitvoergeschiedenis uit tabelopslag ook zodat deze de status van de lokale kunt herstellen. Een van de handige dingen over een van de geschiedenis beschikbaar in de Table storage is een kijkje nemen en de geschiedenis van uw integraties met hulpprogramma's zoals Zie [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Azure Storage Explorer schermafbeelding te maken](media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> Het is eenvoudig en handig om te zien van de uitvoergeschiedenis in de table storage, vermijden eventuele afhankelijkheden in deze tabel. Deze mogelijk wijzigen, omdat de extensie duurzame functies zich verder ontwikkelen.

## <a name="known-issues-and-faq"></a>Bekende problemen en veelgestelde vragen

Alle bekende problemen moeten worden bijgehouden in de [GitHub problemen](https://github.com/Azure/azure-functions-durable-extension/issues) lijst. Als u een probleem opgetreden en het probleem niet kunt in GitHub vinden, opent u een nieuwe uitgifte en bevat een gedetailleerde beschrijving van het probleem.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doorgaan duurzame functies documentatie lezen](durable-functions-bindings.md)

> [!div class="nextstepaction"]
> [Installeer de duurzame functies extensie en voorbeelden](durable-functions-install.md)

