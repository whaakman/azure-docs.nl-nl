---
title: Azure Application Insights Snapshot Debugger problemen oplossen met | Microsoft Docs
description: Veelgestelde vragen over Azure Application Insights Snapshot Debugger.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: ''
ms.service: application-insights
ms.workload: ''
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 285f42a6b52819077b92abce78c1f51756780604
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644258"
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Snapshot Debugger: Problemen oplossen met

Application Insights Snapshot Debugger kunt u automatisch een momentopname voor foutopsporing verzamelen van live web-apps. De momentopname toont de status van de broncode en variabelen op dit moment die is een uitzondering opgetreden. In dit artikel wordt uitgelegd hoe het foutopsporingsprogramma werkt en voorziet in oplossingen voor veelvoorkomende problemen.

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Hoe werkt Application Insights Snapshot Debugger

Application Insights Snapshot Debugger maakt deel uit van de Application Insights-telemetrie-pijplijn (een exemplaar van ITelemetryProcessor). De Snapshot Collector bewaakt zowel de uitzonderingen in uw code (AppDomain.FirstChanceException) en de uitzonderingstelemetrie dat is gerapporteerd aan Application Insights via `TelemetryClient.TrackException`. Als u het pakket Snapshot Collector hebt toegevoegd aan uw project en er is een gebeurtenis in de pijplijn telemetrie, een aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en 'SnapshotCollectorEnabled' in de aangepaste gegevens worden gedetecteerd verzonden. Op hetzelfde moment, een proces genaamd 'SnapshotUploader.exe' wordt gestart (versie 1.2.0 of hoger) of 'MinidumpUploader.exe' (vóór versie 1.2.0), het uploaden van de verzamelde momentopnamen gegevensbestanden naar Application Insights.  Wanneer het proces gegevensuploader wordt gestart, stuurt een aangepaste gebeurtenis met de naam 'UploaderStart'. Daarna voert de snapshot collector het normale gedrag van de bewaking.

Terwijl de snapshot collector wordt bewaakt door Application Insights-uitzonderingstelemetrie, het maakt gebruik van de parameters (bijvoorbeeld ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) is gedefinieerd in de de configuratie om te bepalen wanneer een momentopname verzamelen. Wanneer alle regels is voldaan, wordt een momentopname van de volgende uitzondering veroorzaakt op dezelfde plaats in de collector aanvragen. Tegelijkertijd wordt een aangepaste gebeurtenis van de Application Insights met de naam 'AppInsightsSnapshotCollectorLogs' en 'RequestSnapshots' worden verzonden. Omdat de compiler 'Release' code optimaliseren wordt, worden lokale variabelen niet meer zichtbaar in de verzamelde momentopnamen. De snapshot collector probeert te deoptimize van de methode die de uitzondering is opgetreden na het aanvragen van momentopnamen. Gedurende deze tijd wordt een Application Insights aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en 'ProductionBreakpointsDeOptimizeMethod' in de aangepaste gegevens worden verzonden.  Wanneer de momentopname van de volgende uitzondering is verzameld, wordt de lokale variabelen beschikbaar zijn. Nadat de momentopname zijn verzameld, wordt het optimaliseren van de code. 

> [!NOTE]
> Deoptimization is vereist voor de Application Insights-site-extensie worden geïnstalleerd.

Wanneer een momentopname voor een specifieke uitzondering is aangevraagd, wordt de momentopname-collector gestart bewaking van uw toepassing uitzonderingsverwerking-pijplijn (AppDomain.FirstChanceException). Wanneer de uitzondering zich weer voordoet, wordt een momentopname (aangepaste gebeurtenis van de Application Insights met de naam 'AppInsightsSnapshotCollectorLogs' en 'SnapshotStart' in de aangepaste gegevens) door de collector gestart. En vervolgens een schaduwkopie van het actieve proces wordt gemaakt (in de tabel pagina wordt worden gedupliceerd). Dit wordt normaal gesproken 10 tot 20 milliseconden duren. Hierna wordt een aangepaste gebeurtenis van de Application Insights met de naam 'AppInsightsSnapshotCollectorLogs' en 'SnapshotStop' in de aangepaste gegevens worden verzonden. Wanneer het Gevorkte proces wordt gemaakt, wordt de totale hoeveelheid wisselbaar geheugen worden verhoogd met de dezelfde waarde als het wisselbaar geheugen van de toepassing wordt uitgevoerd (de werkset zijn veel kleiner is). Terwijl het toepassingsproces van uw wordt uitgevoerd normaal gesproken de schaduwkopieën worden procesgeheugen van gedumpt naar schijf en geüpload naar Application Insights. Nadat de momentopname is geüpload, kunt u een aangepaste gebeurtenis Application Insights met de naam 'UploadSnapshotFinish' worden verzonden.

## <a name="is-the-snapshot-collector-working-properly"></a>De snapshot collector correct werkt?

### <a name="how-to-find-snapshot-collector-logs"></a>Snapshot Collector logboeken zoeken
Snapshot collector logboeken worden verzonden naar uw Application Insights-account als de [Snapshot Collector NuGet-pakket](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) versie 1.1.0 of hoger. Zorg ervoor dat de *ProvideAnonymousTelemetry* niet is ingesteld op false (de waarde is standaard ingesteld op true).

* Navigeer naar uw Application Insights-resource in Azure portal.
* Klik op *zoeken* in de sectie overzicht.
* Voer de volgende tekenreeks in het zoekvak:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Opmerking: Wijzig de *tijdsbereik* indien nodig.

![Schermopname van zoeken Snapshot Collector-Logboeken](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Snapshot collector logboeken onderzoeken
Bij het zoeken naar Logboeken Snapshot Collector, moet dit worden veroorzaakt door 'UploadSnapshotFinish' gebeurtenissen in het betreffende tijdsbereik. Als u de knop 'Open Debug Snapshot' voor het openen van de momentopname niet ziet, stuurt u e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel.

![Schermafbeelding van onderzoeken snapshot collector Logboeken](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Ik kan een momentopname op Open niet vinden
Als de volgende stappen niet, u het probleem kunt oplossen kunt, stuurt u e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>Stap 1: Zorg ervoor dat uw toepassing telemetriegegevens en uitzonderingsgegevens verzenden naar Application Insights
Ga naar Application Insights-resource, Controleer of er gegevens die worden verzonden vanuit uw toepassing.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>Stap 2: Zorg ervoor dat Snapshot collector correct wordt toegevoegd aan uw toepassing van Application Insights Telemetry pijplijn
Als u Logboeken kunt vinden in de stap 'Over het vinden van Snapshot Collector logboeken', de snapshot collector correct wordt toegevoegd aan uw project en kunt u deze stap overslaan.

Als er geen Snapshot collector-Logboeken, controleert u het volgende:
* Voor klassieke ASP.NET-toepassingen, Controleer u voor deze regel *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">* in de *ApplicationInsights.config* bestand.

* Voor ASP.NET Core-toepassingen, zorg ervoor dat de *ITelemetryProcessorFactory* met *SnapshotCollectorTelemetryProcessor* wordt toegevoegd aan *IServiceCollection* services .

* Controleer ook dat u de juiste instrumentatiesleutel in uw gepubliceerde toepassing.

* De Snapshot collector biedt geen ondersteuning voor meerdere instrumentatiesleutels in één toepassing, momentopnamen wordt verzonden naar de instrumentatiesleutel van de eerste uitzondering die het neemt.

* Als u de *InstrmentationKey* handmatig in uw code bijwerken de *InstrumentationKey* -element van de *ApplicationInsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>Stap 3: Controleer of dat de uploader minidump is gestart
Zoek in de logboeken van de collector momentopname *UploaderStart* (UploaderStart typt in het zoekvak). Er moet een gebeurtenis wanneer de momentopname-collector de eerste uitzondering bewaakt. Als deze gebeurtenis niet bestaat, controleert u andere logboeken voor meer informatie. Een mogelijke manier voor het oplossen van dit probleem is uw toepassing opnieuw te starten.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>Stap 4: Controleer of dat Snapshot Collector uitgedrukt de bedoeling-momentopnamen verzamelen
Zoek in de logboeken van de collector momentopname *RequestSnapshots* (type ```RequestSnapshots``` in het zoekvak).  Als er geen een, Controleer uw configuratie. Bijvoorbeeld, *ThresholdForSnapshotting*, wat aangeeft dat het nummer van een specifieke uitzondering die optreden kan voordat er begonnen wordt met het verzamelen van momentopnamen.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>Stap 5: Zorg ervoor dat de momentopname niet is uitgeschakeld vanwege een geheugen-beveiliging
Ter bescherming van de prestaties van uw toepassing, een momentopname van een wordt alleen vastgelegd wanneer er een goede geheugenbuffer is. Zoek in de logboeken van de collector snapshot 'CannotSnapshotDueToMemoryUsage'. In de aangepaste gegevens van de gebeurtenis, is er een gedetailleerde reden. Als uw toepassing wordt uitgevoerd in een Azure-Web-App, is het mogelijk dat de beperking strikte. Omdat Azure-Web-App uw app opnieuw starten wordt wanneer aan bepaalde regels geheugen wordt voldaan. U kunt proberen de service-plan voor computers met meer geheugen om op te lossen dit probleem kan worden uitgebreid.

### <a name="step-6-make-sure-snapshots-were-captured"></a>Stap 6: Zorg ervoor dat momentopnamen zijn vastgelegd
Zoek in de logboeken van de collector momentopname ```RequestSnapshots```.  Als geen aanwezig is, controleert u uw configuratie. Bijvoorbeeld, *ThresholdForSnapshotting*, wat aangeeft dat het nummer van een specifieke uitzondering opgetreden voor het verzamelen van een momentopname.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>Stap 7: Zorg ervoor dat momentopnamen correct worden geüpload
Zoek in de logboeken van de collector momentopname ```UploadSnapshotFinish```.  Als dat niet aanwezig is, stuurt u e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel. Als deze gebeurtenis bestaat, opent u een van de logboeken en kopieer de waarde 'SnapshotId' in de aangepaste gegevens. Zoek vervolgens de waarde in. Hiermee wordt de uitzondering die overeenkomt met de momentopname vinden. Klik vervolgens op de uitzondering en de momentopname voor foutopsporing openen. (Als er geen bijbehorende uitzondering, de uitzonderingstelemetrie mogen voorbeelden worden gemaakt vanwege een hoog volume. Probeer een andere snapshotId.)

![Schermopname van het zoeken naar SnapshotId](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Schermafbeelding van Open-uitzondering](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Schermafbeelding van de Open momentopname voor foutopsporing](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Momentopname weergave lokale variabelen zijn niet voltooid

Sommige van de lokale variabelen ontbreken. Als uw toepassing wordt release code worden uitgevoerd, wordt de compiler enkele variabelen direct optimaliseren. Bijvoorbeeld:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Als uw aanvraag anders is, kan dit wijzen op een fout. E-mailbericht verzendt snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel samen met het codefragment.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Momentopname: Kan de waarde van de lokale variabele of argument niet ophalen
Zorg ervoor dat de [Application Insights site-extensie](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) is geïnstalleerd. Als het probleem zich blijft voordoen, stuurt u e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel.
