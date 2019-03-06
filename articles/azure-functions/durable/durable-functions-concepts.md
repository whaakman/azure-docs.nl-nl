---
title: Duurzame functies patronen en technische concepten in Azure Functions
description: Meer informatie over hoe de extensie duurzame functies in Azure Functions biedt u de voordelen van stateful code wordt uitgevoerd in de cloud.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: azfuncdf
ms.openlocfilehash: e5be81efcd655f1f0361d8c00d978a81c3e6caa5
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443416"
---
# <a name="durable-functions-patterns-and-technical-concepts-azure-functions"></a>Duurzame functies patronen en technische concepten (Azure Functions)

Duurzame functies is een uitbreiding van [Azure Functions](../functions-overview.md) en [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md). Duurzame functies kunt u het schrijven van stateful functies in een serverloze omgeving. Met de extensie worden status, controlepunten en het opnieuw opstarten voor u beheerd. 

Dit artikel biedt u gedetailleerde informatie over het gedrag van de extensie duurzame functies voor Azure Functions en algemene patronen voor implementatie. De informatie kunt u bepalen hoe u duurzame functies om u te helpen bij het oplossen van uw ontwikkelingsuitdagingen.

> [!NOTE]
> Duurzame functies is een geavanceerde-extensie voor Azure-functies die niet geschikt is voor alle toepassingen. In dit artikel wordt ervan uitgegaan dat u hebt een sterke bekend bent met concepten in [Azure Functions](../functions-overview.md) en de uitdagingen bij het ontwikkelen van serverloze toepassingen.

## <a name="patterns"></a>Patronen

Deze sectie beschrijft enkele algemene patronen waar duurzame functies kan nuttig zijn.

### <a name="chaining"></a>Patroon #1: Functiekoppeling

In de functie patroon koppelen, voert een reeks functies in een specifieke volgorde. In dit patroon wordt de uitvoer van een functie toegepast op de invoer van een andere functie.

![Een diagram van de functie chaining-patroon](./media/durable-functions-concepts/function-chaining.png)

U kunt duurzame functies gebruiken voor het implementeren van de functie chaining patroon beknopte zoals wordt weergegeven in het volgende voorbeeld:

#### <a name="c-script"></a>C#-script

```csharp
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
        // Error handling or compensation goes here.
    }
}
```

> [!NOTE]
> Er zijn subtiele verschillen tussen het schrijven van een vooraf gecompileerde duurzame functie in C# en het schrijven van een vooraf gecompileerde duurzame functie de C# script dat wordt weergegeven in het voorbeeld. In een C# vooraf geschreven functie, duurzame parameters moeten worden voorzien van de bijbehorende kenmerken. Een voorbeeld is de `[OrchestrationTrigger]` kenmerk voor de `DurableOrchestrationContext` parameter. In een C# duurzame functie vooraf geschreven als de parameters worden niet correct voorzien, de runtime de variabelen kan niet in de functie invoeren en is een fout optreedt. Zie voor meer voorbeelden van de [azure-functions-durable-extension-voorbeelden op GitHub](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples).

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return yield context.df.callActivity("F4", z);
});
```

In dit voorbeeld de waarden `F1`, `F2`, `F3`, en `F4` zijn de namen van andere functies in de functie-app. U kunt Controlestroom implementeren met behulp van de normale imperatieve constructies coderen. Code wordt uitgevoerd vanaf de bovenkant omlaag. De code kan betrekking hebben op bestaande taal control flow semantiek, zoals de voorwaarden en lussen. U kunt opnemen foutafhandelingslogica in `try` / `catch` / `finally` blokken.

U kunt de `context` parameter [DurableOrchestrationContext] \(.NET\) en de `context.df` object (JavaScript) naar andere functies aanroepen met de naam, parameters doorgeven en retourneren van de functie de uitvoer. Elke keer dat de code roept `await` (C#) of `yield` (JavaScript), het framework duurzame functies controlepunten de voortgang van het huidige exemplaar van de functie. Als het proces of de virtuele machine wordt gerecycled halverwege de uitvoering, het exemplaar van de functie wordt hervat in de voorgaande `await` of `yield` aanroepen. Zie voor meer informatie de volgende sectie, patroon #2: Ventilator out/ventilator.

> [!NOTE]
> De `context` in JavaScript-object vertegenwoordigt de volledige [functie context](../functions-reference-node.md#context-object), niet alleen de [DurableOrchestrationContext] parameter.

### <a name="fan-in-out"></a>Patroon #2: Ventilator out/ventilator in

In de ventilator out/ventilator in patroon, u meerdere functies parallel uitvoeren en wacht tot alle functies om te voltooien. Werk aggregatie is vaak uitgevoerd op de resultaten die worden geretourneerd van de functies.

![Een diagram van de ventilator out/ventilator patroon](./media/durable-functions-concepts/fan-out-fan-in.png)

Met normale functies, kunt u ventilator uit door de functie meerdere berichten verzenden naar een wachtrij. Terug in airconditioningapparatuur is veel moeilijker. Als u wilt ventilator, in een normale functie kunt u code om bij te houden bij het einde van de wachtrij geactiveerde functies, en vervolgens store functie uitvoer schrijven. 

De extensie duurzame functies verwerkt dit patroon met relatief eenvoudig code:

#### <a name="c-script"></a>C#-script

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // Get a list of N work items to process in parallel.
    object[] workBatch = await context.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // Get a list of N work items to process in parallel.
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

Het werk fanout wordt gedistribueerd naar meerdere exemplaren van de `F2` functie. Het werk wordt bijgehouden met behulp van een dynamische lijst met taken. De .NET `Task.WhenAll` API of JavaScript `context.df.Task.all` API wordt aangeroepen, om te wachten tot de aangeroepen functies om te voltooien. Vervolgens, de `F2` functie voert worden samengevoegd in de lijst met dynamische en doorgegeven aan de `F3` functie.

De controlepunten die plaatsvindt op de `await` of `yield` aanroepen voor `Task.WhenAll` of `context.df.Task.all` zorgt ervoor dat een potentiële halverwege crash of opnieuw opstarten niet nodig een reeds voltooide taak opnieuw te starten.

### <a name="async-http"></a>Patroon #3: Asynchrone HTTP-API 's

De asynchrone patroon HTTP APIs lost het probleem van het coördineren van de status van langlopende bewerkingen met externe clients. Een veelgebruikte manier voor het implementeren van dit patroon is door een trigger aanroepen de actie langdurige HTTP. De client vervolgens omgeleid naar een status-eindpunt dat de client controleert als u wilt weten wanneer de bewerking is voltooid.

![Een diagram van het HTTP-API-patroon](./media/durable-functions-concepts/async-http-api.png)

Duurzame functies biedt ingebouwde API's die de code die u schrijft om te communiceren met de functie-uitvoeringen langlopende vereenvoudigen. De voorbeelden van de Snelstartgids duurzame functies ([ C# ](durable-functions-create-first-csharp.md) en [JavaScript](quickstart-js-vscode.md)) een eenvoudige REST-opdracht die u gebruiken kunt om te starten van de nieuwe orchestrator-functie exemplaren weergeven. Nadat een exemplaar is gestart, wordt de extensie webhook HTTP APIs waarmee query uitvoeren op de status van de orchestrator-functie. 

Het volgende voorbeeld ziet de REST-opdrachten die een orchestrator start en de status ervan op te vragen. Sommige gegevens zijn voor alle duidelijkheid worden weggelaten uit het voorbeeld.

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

Omdat de runtime duurzame functies staat beheert, moet u niet uw eigen status bij te houden-mechanisme implementeren.

De extensie duurzame functies heeft ingebouwde webhooks waarmee langlopende indelingen beheren. U kunt ook zelf dit patroon implementeren met behulp van uw eigen functie-triggers (zoals HTTP, een wachtrij of Azure Event Hubs) en de `orchestrationClient` binding. U kunt bijvoorbeeld een wachtrijbericht voor het activeren van beëindiging. Of u kunt een HTTP-trigger die wordt beveiligd door een verificatiebeleid van Azure Active Directory in plaats van de ingebouwde webhooks die gebruikmaken van een gegenereerde sleutel voor verificatie gebruiken.

Hier volgen enkele voorbeelden van het gebruik van het HTTP-API-patroon:

#### <a name="c"></a>C#

```csharp
// An HTTP-triggered function starts a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // The function name comes from the request URL.
    // The function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
// An HTTP-triggered function starts a new orchestrator function instance.
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);

    // The function name comes from the request URL.
    // The function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(req, instanceId);
};
```

> [!WARNING]
> Wanneer u lokaal ontwikkelen in JavaScript, gebruik van methoden op `DurableOrchestrationClient`, moet u de omgevingsvariabele instellen `WEBSITE_HOSTNAME` naar `localhost:<port>` (bijvoorbeeld `localhost:7071`). Zie voor meer informatie over deze vereiste [GitHub-probleem 28](https://github.com/Azure/azure-functions-durable-js/issues/28).

In .NET, de [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parameter is een waarde van de `orchestrationClient` uitvoer verbinding maakt, deze maakt deel uit van de extensie duurzame functies. In JavaScript, dit object wordt geretourneerd door het aanroepen van `df.getClient(context)`. Deze objecten bieden methoden die u gebruiken kunt om te starten, gebeurtenissen te verzenden, beëindigen en query's uitvoeren voor nieuwe of bestaande orchestrator-functie-exemplaren.

In de voorgaande voorbeelden een HTTP-geactiveerde functie omvat een `functionName` waarde van de binnenkomende URL en de waarde die moet worden doorgegeven [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). De [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) vervolgens binden van de API retourneert een antwoord met een `Location` -header en aanvullende informatie over het exemplaar. U kunt de informatie later gebruiken om te controleren of de status van het exemplaar van de slag te gaan of de sessie is beëindigd.

### <a name="monitoring"></a>Patroon #4: Controleren

Het patroon monitor verwijst naar een flexibele, terugkerende proces in een werkstroom. Een voorbeeld is polling tot bepaalde voorwaarden wordt voldaan. U kunt een normale [timertrigger](../functions-bindings-timer.md) om een eenvoudige scenario, zoals een periodieke opschoontaak, maar het interval is statisch en beheren van de levensduur van het exemplaar wordt complexe. Duurzame functies kunt u flexibele terugkeerpatroon intervallen maken, beheren van de levensduur van de taak en monitor voor meerdere processen maken van een enkele orchestration.

Een voorbeeld van het patroon voor de monitor is om terug te draaien het eerdere scenario van asynchrone HTTP-API. In plaats van een eindpunt voor een externe client voor het bewaken van een langdurige bewerking, de langlopende monitor gebruikt een extern eindpunt en wacht dan tot een wijziging in de status.

![Een diagram van de monitor-patroon](./media/durable-functions-concepts/monitor.png)

In een paar regels code, kunt u duurzame functies te maken van meerdere beeldschermen dat willekeurige eindpunten waargenomen. De monitors uitvoering kunnen beëindigen als een voorwaarde wordt voldaan, of de [DurableOrchestrationClient](durable-functions-instance-management.md) de monitors kunt beëindigen. U kunt wijzigen van een monitor `wait` interval op basis van een bepaalde voorwaarde (bijvoorbeeld exponentieel uitstel.) 

De volgende code wordt een eenvoudige monitor geïmplementeerd:

#### <a name="c-script"></a>C#-script

```csharp
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
            // Perform an action when a condition is met.
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform more work here, or let the orchestration end.
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform an action when a condition is met.
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform more work here, or let the orchestration end.
});
```

Wanneer een aanvraag wordt ontvangen, wordt een nieuw orchestration-exemplaar gemaakt voor die taak-ID. Het exemplaar pollt status totdat een voorwaarde wordt voldaan en de lus wordt afgesloten. Een duurzame timer bepaalt het polling-interval. Vervolgens meer werk kan worden uitgevoerd, of de indeling kunt beëindigen. Wanneer de `context.CurrentUtcDateTime` (.NET) of `context.df.currentUtcDateTime` (JavaScript) groter is dan de `expiryTime` waarde, de ends monitor.

### <a name="human"></a>Patroon #5: Menselijke tussenkomst

Veel geautomatiseerde processen hebben betrekking op een soort menselijke tussenkomst. Met betrekking tot mensen in een geautomatiseerd proces is lastig, omdat gebruikers niet als maximaal beschikbaar en optimaal reageert als cloudservices. Een geautomatiseerd proces mogelijk voor dit met behulp van de time-outs en compensatie logica.

Een goedkeuringsproces is een voorbeeld van een bedrijfsproces dat betrekking heeft op menselijke tussenkomst. Goedkeuring van een manager kan vereist zijn voor een onkostennota die langer is dan een bepaald bedrag. Als de manager niet goedkeuren van het rapport binnen 72 uur (op vakantie ging misschien de manager), begint het proces van een uitbreiding om op te halen van de goedkeuring van iemand anders (bijvoorbeeld de manager van de manager).

![Een diagram van het patroon menselijke tussenkomst](./media/durable-functions-concepts/approval.png)

In dit voorbeeld kunt u het patroon implementeren met behulp van een orchestrator-functie. De orchestrator gebruikt een [duurzame timer](durable-functions-timers.md) om goedkeuring te vragen. De orchestrator escaleert als time-out optreedt. Wacht tot de orchestrator een [externe gebeurtenis](durable-functions-external-events.md), zoals een melding die wordt gegenereerd door een menselijke tussenkomst.

Deze voorbeelden maken een goedkeuringsproces ter demonstratie van het patroon menselijke tussenkomst:

#### <a name="c-script"></a>C#-script

```csharp
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

```javascript
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

Aanroepen voor het maken van de timer voor het duurzaam, `context.CreateTimer` (.NET) of `context.df.createTimer` (JavaScript). De melding wordt ontvangen door `context.WaitForExternalEvent` (.NET) of `context.df.waitForExternalEvent` (JavaScript). Vervolgens `Task.WhenAny` (.NET) of `context.df.Task.any` (JavaScript) aangeroepen om te bepalen of uit te breiden (time-out gebeurt eerst) of de goedkeuring (de goedkeuring is ontvangen voordat de time-out optreedt) niet verwerken.

Een externe client kan de gebeurtenismelding leveren aan een wachtrij orchestrator-functie met behulp van de [ingebouwde HTTP APIs](durable-functions-http-api.md#raise-event) of met behulp van de [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) -API van een andere functie:

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

Achter de schermen, wordt de extensie duurzame functies gebouwd boven de [duurzame taak Framework](https://github.com/Azure/durabletask), een open-source-bibliotheek op GitHub die wordt gebruikt voor het bouwen van duurzame taak indelingen. Azure Functions is de serverloze ontwikkeling van Azure WebJobs, is duurzame functies de serverloze ontwikkeling van duurzame taak Framework. Microsoft en andere organisaties gebruiken de duurzame taak Framework uitgebreid om essentiële processen te automatiseren. Het is perfect voor de serverloze Azure Functions-omgeving.

### <a name="event-sourcing-checkpointing-and-replay"></a>' Event sourcing ', plaatsen van controlepunten en herhaling

De status van de scriptuitvoering orchestrator-functies op betrouwbare wijze beheren met behulp van de [' event sourcing '](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) ontwerp. In plaats van rechtstreeks opslaan van de huidige status van een indeling, wordt met de extensie duurzame functies een archief waaraan alleen gebruikt om vast te leggen van de volledige reeks acties die de functie-indeling heeft. Een archief waaraan alleen heeft veel voordelen ten opzichte van de volledige runtime-state dumpen 'van'. Voordelen omvatten betere prestaties, schaalbaarheid en reactiesnelheid. Ook krijgt u uiteindelijke consistentie voor transactiegegevens en volledige audittrails en geschiedenis. De audittrails ondersteunen betrouwbare compenserende acties.

Duurzame functies maakt gebruik van Gebeurtenisbronnen transparant. Achter de schermen, de `await` (C#) of `yield` operator in een orchestrator-functie (JavaScript) levert de controle van de orchestrator-thread terug naar het verdeelprogramma duurzame taak Framework. De functie voor berichtverzending doorvoeringen vervolgens nieuwe maatregelen die tegen de orchestrator-functie gepland (zoals een of meer onderliggende functies aanroepen of een duurzame timer plannen) naar de opslag. De transparante doorvoeractie wordt toegevoegd aan de uitvoeringsgeschiedenis van de orchestration-instantie. De geschiedenis wordt opgeslagen in een opslagtabel. De commit-actie vervolgens een bericht toevoegt aan een wachtrij voor het plannen van het echte werk. De orchestrator-functie kan op dit moment worden verwijderd uit het geheugen. 

Facturering voor de orchestrator-functie wordt gestopt als u het verbruiksabonnement voor Azure Functions. Wanneer er is meer werk te doen, de functie opnieuw wordt opgestart, en de status is opnieuw opgebouwd.

Wanneer een orchestration-functie meer werk te doen is opgegeven (bijvoorbeeld een antwoordbericht wordt ontvangen of een duurzame timer verloopt), de orchestrator ontwaakt en wordt de gehele functie vanaf het begin opnieuw opbouwen van de status van de lokale opnieuw uitgevoerd. 

Tijdens het opnieuw afspelen, als de code probeert aan te roepen een functie (of een andere asynchrone werk), het duurzame taak Framework raadpleegt u de uitvoeringsgeschiedenis van de huidige indeling. Als deze wordt gevonden die de [activiteit functie](durable-functions-types-features-overview.md#activity-functions) al uitgevoerd en kunnen we een resultaat uit, het resultaat van deze functie opnieuw weergegeven en de orchestrator-code blijft om uit te voeren. Opnieuw afspelen gaat door totdat de functiecode is voltooid of deze nieuwe tot het asynchrone werk is ingesteld.

### <a name="orchestrator-code-constraints"></a>Beperkingen voor orchestrator-code

Het gedrag van de herhaling van orchestrator-code maakt beperkingen met betrekking tot het type van de code die u in een orchestrator-functie schrijven kunt. Orchestrator-code is moet bijvoorbeeld deterministische omdat deze zal opnieuw worden afgespeeld meerdere keren, en deze hetzelfde resultaat telkens opleveren moet. Zie voor de volledige lijst van beperkingen [Orchestrator code beperkingen](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints).

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose

De extensie duurzame functies verzendt automatisch bijhouden voor gestructureerde gegevens [Application Insights](../functions-monitoring.md) als u uw functie-app ingesteld met een Azure Application Insights-instrumentatiesleutel. U kunt de traceringsgegevens gebruiken voor het bewaken van de acties en de voortgang van de indelingen.

Hier volgt een voorbeeld van hoe de duurzame functies bijhouden van gebeurtenissen eruit in de Application Insights-portal zien wanneer u [Application Insights Analytics](../../application-insights/app-insights-analytics.md):

![Queryresultaten van Application Insights](./media/durable-functions-concepts/app-insights-1.png)

Vindt u nuttige gestructureerde gegevens in de `customDimensions` veld zijn in elke logboekvermelding. Hier volgt een voorbeeld van een vermelding die volledig worden uitgevouwen:

![Het veld customDimensions in een Application Insights-query](./media/durable-functions-concepts/app-insights-2.png)

Vanwege het gedrag van de herhaling van de verzender duurzame taak Framework, kunt u verwachten redundante logboekvermeldingen voor herhaalde acties. Redundante logboekvermeldingen krijgt u inzicht in het gedrag van de herhaling van de core-engine. De [Diagnostics](durable-functions-diagnostics.md) artikel ziet u query's die replay-Logboeken, wegfilteren zodat u alleen de 'realtime' Logboeken kunt zien.

## <a name="storage-and-scalability"></a>Opslag en schaalbaarheid

De extensie duurzame functies maakt gebruik van wachtrijen, tabellen en blobs in Azure Storage om vast te leggen van de uitvoering van geschiedenis van status en het activeren een functie wordt uitgevoerd. U kunt het standaardopslagaccount gebruiken voor de functie-app, of u een afzonderlijk opslagaccount kunt configureren. U kunt een afzonderlijk account op basis van de maximale doorvoer van opslag. De orchestrator-code die u schrijft communiceren niet met de entiteiten in deze storage-accounts. Het duurzame taak Framework beheert de entiteiten rechtstreeks als een implementatiedetail in.

Orchestrator-functies activiteitsfuncties plannen en hun antwoorden via de interne Wachtrijberichten ontvangen. Als een functie-app wordt uitgevoerd in het verbruiksabonnement voor Azure Functions, de [Azure Functions schalen controller](../functions-scale.md#how-the-consumption-plan-works) controleert deze wachtrijen. Nieuwe compute-exemplaren naar behoefte worden toegevoegd. Wanneer geschaald naar meerdere virtuele machines, kan een orchestrator-functie uitvoeren op een virtuele machine, terwijl de activiteitsfuncties die het aanroepen van de orchestrator-functies mogelijk worden uitgevoerd op meerdere verschillende virtuele machines. Zie voor meer informatie over de werking van de schaal van duurzame functies [prestaties en schaal](durable-functions-perf-and-scale.md).

De uitvoeringsgeschiedenis voor orchestrator-accounts wordt opgeslagen in de tabelopslag. Wanneer een exemplaar rehydrates op een bepaalde virtuele machine, worden de orchestrator de uitvoeringsgeschiedenis opgehaald uit table storage, zodat deze de status van de lokale kunt herstellen. Een handig aspect van de geschiedenis beschikbaar in de tabelopslag met is dat u hulpprogramma's zoals kunt [Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) om te zien van de geschiedenis van de indelingen.

Storage-blobs worden hoofdzakelijk gebruikt als een leasemechanisme voor de coördinatie van de scale-out van de orchestration-exemplaren op meerdere virtuele machines. Storage-blobs bevatten de gegevens voor grote berichten die niet rechtstreeks in de tabellen of wachtrijen kunnen worden opgeslagen.

![Een schermopname van Azure Storage Explorer](./media/durable-functions-concepts/storage-explorer.png)

> [!WARNING]
> Hoewel het is gemakkelijk en handig om te zien van de uitvoeringsgeschiedenis in de table storage, maak niet eventuele afhankelijkheden voor deze tabel. De tabel kan worden gewijzigd omdat de extensie duurzame functies zich verder ontwikkelt.

## <a name="known-issues"></a>Bekende problemen

Alle bekende problemen moeten worden bijgehouden in de [GitHub issues](https://github.com/Azure/azure-functions-durable-extension/issues) lijst. Als u een probleem opgetreden bij het probleem niet kunt vinden in GitHub, opent u een nieuw probleem. Bevat een gedetailleerde beschrijving van het probleem.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over duurzame functies, [duurzame functies functie typen en -functies](durable-functions-types-features-overview.md). 

Aan de slag gaan:

> [!div class="nextstepaction"]
> [Uw eerste duurzame functie maken](durable-functions-create-first-csharp.md)

[DurableOrchestrationContext]: https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html
