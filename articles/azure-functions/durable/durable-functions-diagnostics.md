---
title: Diagnostische gegevens in duurzame functies - Azure
description: Informatie over het analyseren van problemen met de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: ac9abaaea7f33627332a9bc7563745b5efdf3d12
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57436237"
---
# <a name="diagnostics-in-durable-functions-in-azure"></a>Diagnostische gegevens in duurzame functies in Azure

Er zijn verschillende opties voor het oplossen van problemen met [duurzame functies](durable-functions-overview.md). Sommige van deze opties zijn hetzelfde voor gewone functies en sommige zijn uniek voor Durable Functions.

## <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) is de aanbevolen manier om te doen, diagnose en controle in Azure Functions. Dit geldt ook voor duurzame functies. Zie voor een overzicht van hoe u Application Insights in uw functie-app, [Monitor Azure Functions](../functions-monitoring.md).

Azure Functions duurzame Extension verzendt ook *bijhouden van gebeurtenissen* waarmee u kunt de uitvoering van de end-to-end van een indeling traceren. Deze kan worden gevonden en opgevraagd met de [Application Insights Analytics](../../azure-monitor/app/analytics.md) hulpprogramma in de Azure-portal.

### <a name="tracking-data"></a>Gegevens voor het bijhouden

Elke gebeurtenis van de levenscyclus van een exemplaar van de orchestration zorgt ervoor dat een traceringsgebeurtenis naar worden geschreven naar de **traceringen** verzameling in Application Insights. Deze gebeurtenis bevat een **customDimensions** nettolading met verschillende velden.  Veldnamen worden voorafgegaan door `prop__`.

* **hubName**: De naam van de taak hub waarin uw indelingen worden uitgevoerd.
* **appName**: De naam van de functie-app. Dit is handig wanneer u meerdere functie-apps delen de dezelfde Application Insights-exemplaar hebt.
* **slotName**: De [implementatiesite](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) in de huidige functie-app wordt uitgevoerd. Dit is handig als u uw indelingen implementatiesites naar versie gebruiken.
* **Functienaam**: De naam van de functie orchestrator of activiteit.
* **functionType**: Het type van de functie, zoals **Orchestrator** of **activiteit**.
* **instanceId**: De unieke ID van de orchestration-instantie.
* **status**: De uitvoeringsstatus van de levenscyclus van het exemplaar. Geldige waarden zijn:
  * **Geplande**: De functie is gepland voor uitvoering, maar nog niet begonnen nog wordt uitgevoerd.
  * **Aan de slag**: De functie is gestart, maar niet nog gestopt of voltooid.
  * **Gestopt**: De orchestrator werk is gepland en wordt gewacht tot deze is voltooid.
  * **Luisteren**: De orchestrator is beschikbaar voor de melding van een externe gebeurtenis.
  * **Voltooid**: De functie is voltooid.
  * **Mislukt**: De functie is mislukt met een fout.
* **reden**: Aanvullende gegevens die zijn gekoppeld aan de traceringsgebeurtenis. Bijvoorbeeld, als een exemplaar wordt gewacht tot een externe gebeurtenismelding, dit veld geeft aan dat de naam van de gebeurtenis die is in afwachting van. Als een functie is mislukt, wordt dit de foutdetails bevatten.
* **isReplay**: Booleaanse waarde die aangeeft of de-traceringsgebeurtenis voor herhaalde uitvoering.
* **extensionVersion**: De versie van de extensie duurzame taak. Dit is vooral belangrijk gegevens tijdens het rapporteren van mogelijke fouten in de uitbreiding. Langlopende exemplaren mogelijk meerdere versies rapporteren als een update treedt op wanneer deze wordt uitgevoerd.
* **sequenceNumber**: Volgnummer kan worden uitgevoerd voor een gebeurtenis. In combinatie met de tijdstempel helpt om de gebeurtenissen met de uitvoeringstijd. *Houd er rekening mee dat dit aantal zal opnieuw instellen op nul als de host opnieuw is opgestart tijdens het exemplaar wordt uitgevoerd, dus is het belangrijk dat u altijd eerst sorteren op timestamp maken en vervolgens sequenceNumber.*

Het detailniveau van het bijhouden van gegevens die worden verzonden naar Application Insights kan worden geconfigureerd de `logger` (1.x werkt) of `logging` (werkt 2.x) sectie van de `host.json` bestand.

#### <a name="functions-1x"></a>Functions 1.x

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

#### <a name="functions-2x"></a>Functions 2.x

```json
{
    "logging": {
        "logLevel": {
          "Host.Triggers.DurableTask": "Information",
        },
    }
}
```

Standaard worden alle niet-opnieuw traceringsgebeurtenissen verzonden. De hoeveelheid gegevens kan worden teruggebracht door het instellen van `Host.Triggers.DurableTask` naar `"Warning"` of `"Error"` in dat geval bijhouden van gebeurtenissen wordt alleen verzonden voor uitzonderlijke situaties.

Om in te schakelen dat de uitgebreide orchestration opnieuw afspelen gebeurtenissen, verzendt de `LogReplayEvents` kan worden ingesteld op `true` in de `host.json` bestand onder `durableTask` zoals wordt weergegeven:

#### <a name="functions-1x"></a>Functions 1.x

```json
{
    "durableTask": {
        "logReplayEvents": true
    }
}
```

#### <a name="functions-2x"></a>Functions 2.x

```javascript
{
    "extensions": {
        "durableTask": {
            "logReplayEvents": true
        }
    }
}
```

> [!NOTE]
> Standaard Application Insights telemetry verzameld door de Azure Functions-runtime om te voorkomen dat de gegevens te vaak verzendt. Dit kan leiden tot trackinggegevens verloren wanneer er veel lifecycle-gebeurtenissen optreden in een korte periode. De [artikel Azure Functions Monitoring](../functions-monitoring.md#configure-sampling) wordt uitgelegd hoe u dit wilt configureren.

### <a name="single-instance-query"></a>Query voor één exemplaar

De volgende query ziet u historische traceringsgegevens voor één exemplaar van de [Hello reeks](durable-functions-sequence.md) orchestration werken. Ze worden geschreven met behulp van de [Application Insights Query Language (AIQL)](https://aka.ms/LogAnalyticsLanguageReference). Deze gefilterd zodat alleen uitvoering opnieuw afspelen van de *logische* uitvoeringspad wordt weergegeven. Gebeurtenissen kunnen worden besteld door te sorteren door `timestamp` en `sequenceNumber` zoals wordt weergegeven in de onderstaande query:

```AIQL
let targetInstanceId = "ddd1aaa685034059b545eb004b15d4eb";
let start = datetime(2018-03-25T09:20:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend sequenceNumber = tolong(customDimensions["prop__sequenceNumber"])
| where isReplay != true
| where instanceId == targetInstanceId
| sort by timestamp asc, sequenceNumber asc
| project timestamp, functionName, state, instanceId, sequenceNumber, appName = cloud_RoleName
```

Het resultaat is een lijst van het bijhouden van gebeurtenissen die het uitvoeringspad van de indeling geeft, inclusief de activiteitsfuncties van elke door de uitvoeringstijd in oplopende volgorde gesorteerd.

![Application Insights-query](./media/durable-functions-diagnostics/app-insights-single-instance-ordered-query.png)

### <a name="instance-summary-query"></a>Samenvatting query exemplaar

De volgende query geeft de status van alle orchestration-exemplaren die zijn uitgevoerd in een opgegeven tijdperiode.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay != true
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```

Het resultaat is een lijst van exemplaar-id's en hun huidige runtimestatus.

![Application Insights-query](./media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Logboekregistratie

Het is belangrijk de orchestrator gedrag voor opnieuw afspelen in gedachten moeten houden bij het schrijven van Logboeken rechtstreeks vanuit een orchestrator-functie. Bijvoorbeeld, houd rekening met de volgende orchestrator-functie:

### <a name="c"></a>C#

```cs
public static async Task Run(
    DurableOrchestrationContext context,
    ILogger log)
{
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    context.log("Calling F1.");
    yield context.df.callActivity("F1");
    context.log("Calling F2.");
    yield context.df.callActivity("F2");
    context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

De resulterende logboekgegevens zal er ongeveer als volgt uit:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Houd er rekening mee dat de logboeken claim met het aanroepen van v1, v2 en v3, deze functies alleen zijn *daadwerkelijk* naam van de eerste keer dat ze optreden. Volgende aanroepen die tijdens het opnieuw afspelen optreden overgeslagen en de uitvoer opnieuw worden afgespeeld op de orchestrator-logica.

Als u aanmelden op niet opnieuw kan worden uitgevoerd wilt, kunt u een voorwaardelijke expressie schrijven naar log alleen als `IsReplaying` is `false`. Houd rekening met het bovenstaande voorbeeld, maar dit keer met controles voor opnieuw afspelen.

#### <a name="c"></a>C#

```cs
public static async Task Run(
    DurableOrchestrationContext context,
    ILogger log)
{
    if (!context.IsReplaying) log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    if (!context.IsReplaying) log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    if (!context.IsReplaying) log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    if (!context.df.isReplaying) context.log("Calling F1.");
    yield context.df.callActivity("F1");
    if (!context.df.isReplaying) context.log("Calling F2.");
    yield context.df.callActivity("F2");
    if (!context.df.isReplaying) context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Met deze wijziging is de uitvoer als volgt uit:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="custom-status"></a>Aangepaste Status

Status van aangepaste indeling kunt u een aangepaste statuswaarde instellen voor de orchestrator-functie. Deze status wordt geboden via de API-HTTP-status query of het `DurableOrchestrationClient.GetStatusAsync` API. De status van de aangepaste indeling kunt geavanceerdere bewaking voor de orchestrator-functies. Bijvoorbeeld, de orchestrator-functiecode kunt opnemen `DurableOrchestrationContext.SetCustomStatus` aanroepen voor het bijwerken van de voortgang van een langdurige bewerking. Een client, zoals een webpagina of andere extern systeem, kan de HTTP-status-query's voor uitgebreidere voortgangsinformatie vervolgens blijft periodiek vragen. Een voorbeeld met `DurableOrchestrationContext.SetCustomStatus` vindt u hieronder:

### <a name="c"></a>C#

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext context)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { completionPercentage = 90.0, status = "Updating database records" };
    context.SetCustomStatus(customStatus);

    // ...do more work...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    const customStatus = { completionPercentage: 90.0, status: "Updating database records", };
    context.df.setCustomStatus(customStatus);

    // ...do more work...
});
```

Terwijl de indeling wordt uitgevoerd, kunnen deze aangepaste status ophalen van externe clients:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

Clients ontvangt het volgende antwoord:

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "completionPercentage": 90.0, "status": "Updating database records" },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
> De status van de aangepaste nettolading is beperkt tot 16 KB van UTF-16-JSON-tekst omdat deze kunnen zijn moet voor de kolom van een Azure-tabelopslag. U kunt externe opslag gebruiken als u grotere nettolading nodig hebt.

## <a name="debugging"></a>Foutopsporing

Azure Functions ondersteunt functiecode rechtstreeks foutopsporing, en die ondersteuning bieden voor dezelfde uitvoert doorsturen naar duurzame functies, ongeacht of die worden uitgevoerd in Azure of lokaal. Er zijn echter enkele gedrag te houden bij het opsporen van fouten:

* **Opnieuw afspelen**: Orchestrator-functies herhalen regelmatig wanneer nieuwe invoer worden ontvangen. Dit betekent dat één *logische* uitvoering van een orchestrator-functie kan leiden tot meerdere keren te maken met de dezelfde onderbrekingspunt met name als deze al vroeg in de functiecode is ingesteld.
* **Await**: Wanneer een `await` is aangetroffen, het besturingselement terug naar het verdeelprogramma duurzame taak Framework levert. Als dit de eerste keer dat een bepaalde `await` is aangetroffen, de bijbehorende taak is *nooit* hervat. Omdat de taak wordt nooit wordt hervat, stapsgewijs *via* de await (F10 in Visual Studio) is niet daadwerkelijk mogelijk. Stapsgewijs via werkt alleen wanneer een taak is opnieuw wordt afgespeeld.
* **Messaging-time-outs**: Duurzame functies wordt intern gebruikt voor Wachtrijberichten tot het uitvoeren van het station van orchestrator-functies en activiteitsfuncties. In een omgeving met meerdere VM's in de foutopsporing voor langere tijd belangrijke kan ervoor zorgen dat een andere virtuele machine om op te halen het bericht, resulterend in dubbele worden uitgevoerd. Dit gedrag bestaat voor reguliere wachtrijtrigger functies ook, maar het is belangrijk om te wijzen in deze context omdat de wachtrijen zijn een implementatiedetail in.

> [!TIP]
> Bij het instellen van onderbrekingspunten, als u wilt alleen verbreken op niet opnieuw kan worden uitgevoerd, stelt u een voorwaardelijke onderbrekingspunt die alleen als regeleinden zijn opgesplitst `IsReplaying` is `false`.

## <a name="storage"></a>Opslag

Duurzame functies slaat staat standaard in Azure Storage. Dit betekent dat u kunt de status van uw indelingen met behulp van hulpprogramma's zoals inspecteren [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Schermopname van Azure Storage Explorer](./media/durable-functions-diagnostics/storage-explorer.png)

Dit is handig voor foutopsporing omdat u precies welke status een indeling mogelijk ziet in. Berichten in wachtrijen kunnen ook worden onderzocht als u wilt weten welke werk in behandeling (of vastgelopen in sommige gevallen).

> [!WARNING]
> Het is handig om te zien van de uitvoeringsgeschiedenis in de tabelopslag, voorkomen waarbij eventuele afhankelijkheden voor deze tabel. Het verschilt mogelijk als de extensie duurzame functies zich verder ontwikkelt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van duurzame timers](durable-functions-timers.md)
