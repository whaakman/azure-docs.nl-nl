---
title: Diagnostische gegevens in duurzame functies - Azure
description: Informatie over het analyseren van problemen met de extensie duurzame functies voor Azure Functions.
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
ms.openlocfilehash: 5ebab8660dfe21984e1a7f9a1cb925aea60de213
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-in-durable-functions-azure-functions"></a>Diagnostische gegevens in duurzame functies (Azure-functies)

Er zijn diverse opties voor het oplossen van problemen met [duurzame functies](durable-functions-overview.md). Sommige van deze opties zijn hetzelfde voor reguliere functies en enkele ervan zijn uniek voor duurzame functies.

## <a name="application-insights"></a>Application Insights

[Application Insights](../application-insights/app-insights-overview.md) is de aanbevolen manier om diagnostische gegevens en bewaken in Azure Functions. Dit geldt ook voor duurzame functies. Zie voor een overzicht van hoe u Application Insights in uw app functie [Monitor Azure Functions](functions-monitoring.md).

De Azure Functions duurzame extensie verzendt ook *traceringsgebeurtenissen* waarmee u kunt de uitvoering van de end-to-end van een orchestration traceren. Deze kan worden gevonden en opgevraagd met de [Application Insights Analytics](../application-insights/app-insights-analytics.md) hulpprogramma in de Azure portal.

### <a name="tracking-data"></a>Gegevens voor het bijhouden

Elke gebeurtenis van de levenscyclus van een exemplaar van de orchestration wordt een gebeurtenis bijhouden worden geschreven naar de **traceringen** verzameling in Application Insights. Deze gebeurtenis bevat een **customDimensions** nettolading met enkele velden.  Veldnamen worden voorafgegaan door `prop__`.

* **hubName**: de naam van de taak hub waarin uw integraties worden uitgevoerd.
* **appName**: de naam van de functie-app. Dit is handig wanneer u meerdere functie apps delen hetzelfde exemplaar van de Application Insights hebt.
* **slotName**: de [implementatiesleuf](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) in de huidige functie-app wordt uitgevoerd. Dit is handig wanneer u uw integraties implementatiesites naar versie gebruikmaken.
* **Functienaam**: de naam van de functie orchestrator of activiteit.
* **functionType**: het type van de functie, zoals **Orchestrator** of **activiteit**.
* **instanceId**: de unieke ID van de orchestration-exemplaar.
* **status**: de uitvoeringsstatus van de levenscyclus van het exemplaar. Geldige waarden zijn:
    * **Geplande**: de functie is gepland voor uitvoering maar nog niet begonnen nog uitgevoerd.
    * **Gestart**: de functie eenmaal is gestart, maar niet nog wordt afgewacht of voltooid.
    * **Afgewacht**: de orchestrator heeft gepland werk en wacht tot deze is voltooid.
    * **Luisteren**: de orchestrator op een externe gebeurtenismelding luistert.
    * **Voltooid**: de functie is voltooid.
    * **Kan geen**: de functie is mislukt met een fout.
* **reden**: aanvullende gegevens die zijn gekoppeld aan de gebeurtenis bijhouden. Als u een exemplaar wacht op een externe gebeurtenismelding, geeft dit veld de naam van de gebeurtenis die wordt gewacht. Als een functie is mislukt, wordt dit de foutdetails bevatten.
* **isReplay**: Booleaanse waarde die aangeeft of de tracerings-gebeurtenis voor opnieuw uitvoeren.
* **extensionVersion**: de versie van de uitbreiding duurzame taak. Dit is vooral belangrijk gegevens tijdens het rapporteren van mogelijke fouten in de uitbreiding. Langlopende exemplaren mogelijk meerdere versies rapporteren als een update treedt op wanneer deze wordt uitgevoerd. 

De uitgebreidheid voor het bijhouden van gegevens naar Application Insights die kan worden geconfigureerd in de `logger` sectie van de `host.json` bestand.

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

Standaard worden alle bijhouden gebeurtenissen verzonden. De hoeveelheid gegevens kan worden verkleind door het instellen van `Host.Triggers.DurableTask` naar `"Warning"` of `"Error"` in dat geval bijhouden van gebeurtenissen wordt alleen worden verzonden voor uitzonderlijke situaties.

> [!WARNING]
> Standaard is de Azure Functions-runtime om te voorkomen dat gegevens te vaak door Application Insights telemetrie actieve. Hierdoor kan bijhouden gegevens verloren gaan wanneer er veel lifecycle gebeurtenissen optreden in een korte periode. De [Azure Functions Monitoring artikel](functions-monitoring.md#configure-sampling) wordt uitgelegd hoe u dit wilt configureren.

### <a name="single-instance-query"></a>De query één exemplaar

De volgende query ziet u historische traceringsgegevens voor één exemplaar van de [Hello Sequence](durable-functions-sequence.md) orchestration werken. Het geschreven met behulp van de [Application Insights Query Language (AIQL)](https://docs.loganalytics.io/docs/Language-Reference). Deze gefilterd zodat alleen replay-uitvoering de *logische* uitvoeringspad wordt weergegeven.

```AIQL
let targetInstanceId = "bf71335b26564016a93860491aa50c7f";
let start = datetime(2017-09-29T00:00:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| where isReplay == false
| where instanceId == targetInstanceId
| project timestamp, functionName, state, instanceId, appName = cloud_RoleName
```
Het resultaat is een lijst met het bijhouden van gebeurtenissen die het uitvoeringspad van de orchestration, met inbegrip van alle functies van de activiteit weergeven.

![Application Insights-query](media/durable-functions-diagnostics/app-insights-single-instance-query.png)

> [!NOTE]
> Sommige van deze gebeurtenissen bijhouden volgorde vanwege het ontbreken van precisie zijn de `timestamp` kolom. Dit wordt bijgehouden in GitHub als [uitgeven #71](https://github.com/Azure/azure-functions-durable-extension/issues/71).

### <a name="instance-summary-query"></a>Samenvatting query exemplaar

De volgende query wordt de status van alle orchestration-exemplaren die zijn uitgevoerd in een opgegeven periode.

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
| where isReplay == false
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```
Het resultaat is een lijst met exemplaar-id's en de huidige runtimestatus.

![Application Insights-query](media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Logboekregistratie

Het is belangrijk rekening moet houden de orchestrator replay-gedrag bij het schrijven van Logboeken rechtstreeks vanaf een orchestrator-functie. Neem bijvoorbeeld de volgende orchestrator-functie:

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```

De resulterende logboekgegevens gaat er ongeveer als volgt uitzien:

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
> Houd er rekening mee dat de logboeken van de claim met het aanroepen van F1 en F2 F3, deze functies alleen zijn *daadwerkelijk* aangeroepen van de eerste keer dat deze worden gevonden. Volgende aanroepen die ontstaan tijdens replay worden overgeslagen en de uitvoer zijn cookies aan de orchestrator-logica.

Als u niet replay-uitvoering zich alleen aanmelden wilt, kunt u een voorwaardelijke expressies schrijven naar logboek alleen als `IsReplaying` is `false`. Houd rekening met het bovenstaande voorbeeld, maar nu met replay-controles.

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    if (!ctx.IsReplaying) log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    if (!ctx.IsReplaying) log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    if (!ctx.IsReplaying) log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```
Met deze wijziging is de logboekuitvoer als volgt uit:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="debugging"></a>Foutopsporing

Azure Functions ondersteunt functiecode rechtstreeks foutopsporing, en die ondersteuning bieden voor dezelfde uitvoert doorsturen naar duurzame functies worden uitgevoerd in Azure of lokaal. Er zijn echter enkele gedrag moet denken bij het opsporen van fouten:

* **Replay-**: Orchestrator-functies regelmatig opnieuw afspelen wanneer de nieuwe invoer worden ontvangen. Dit betekent dat één *logische* uitvoering van een orchestrator-functie kan ertoe leiden roept de dezelfde onderbrekingspunt meermaals, vooral als vroeg in de functiecode is ingesteld.
* **Await**: wanneer een `await` is opgetreden, het besturingselement levert terug naar de dispatcher duurzame taak Framework. Als dit de eerste keer dat een bepaalde `await` is aangetroffen, de bijbehorende taak is *nooit* hervat. Omdat de taak wordt nooit wordt hervat, selecteert u *via* de await (F10 in Visual Studio) is niet daadwerkelijk mogelijk. Stap voor stap via werkt alleen wanneer er wordt een taak opnieuw wordt afgespeeld.
* **Time-outs Messaging**: duurzame functies gebruikt intern Wachtrijberichten voor het uitvoeren van de schijf van zowel orchestrator en functies van de activiteit. In een omgeving met meerdere VM in de foutopsporing voor langere tijd op te splitsen, kan er een andere virtuele machine om op te halen het bericht, waardoor dubbele kan worden uitgevoerd. Dit gedrag bestaat voor gewone wachtrij-trigger functies ook, maar het is belangrijk te weten in deze context omdat de wachtrijen de details van een implementatie zijn.

> [!TIP]
> Bij het instellen van onderbrekingspunten, als u wilt alleen opsplitsen op niet-replay worden uitgevoerd, stelt u een voorwaardelijke onderbrekingspunt die alleen als regeleinden `IsReplaying` is `false`.

## <a name="storage"></a>Storage

Duurzame functies slaat staat standaard in Azure Storage. Dit betekent dat u kunt de status van uw integraties met hulpprogramma's zoals inspecteren [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Azure Storage Explorer schermafbeelding te maken](media/durable-functions-diagnostics/storage-explorer.png)

Dit is handig voor foutopsporing worden opgehaald omdat er precies welke status een orchestration mogelijk in. Berichten in de wachtrijen kunnen ook worden onderzocht voor meer informatie over welke werkitems is in behandeling (of vastgelopen in sommige gevallen).

> [!WARNING]
> Het is handig om te zien van de uitvoergeschiedenis in de table storage, vermijden eventuele afhankelijkheden in deze tabel. Deze mogelijk wijzigen, omdat de extensie duurzame functies zich verder ontwikkelen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van duurzame timers](durable-functions-timers.md)