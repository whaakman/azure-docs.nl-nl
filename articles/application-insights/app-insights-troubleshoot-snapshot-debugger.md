---
title: Azure Application Insights Snapshot-foutopsporingsprogramma Troubleshooting Guide | Microsoft Docs
description: Veelgestelde vragen over Azure Application Insights momentopname foutopsporingsprogramma.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 
ms.service: application-insights
ms.workload: 
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 5d6a2fe4c3ee373172f5324b6c7a39e4f94f4652
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Momentopname foutopsporingsprogramma: Troubleshooting Guide

Application Insights momentopname foutopsporing kunt u automatisch een debug-momentopname verzamelen van live-webtoepassingen. De momentopname toont de status van de broncode en variabelen op dit moment die is een uitzondering opgetreden. Als u problemen bij het ophalen van de Application Insights momentopname foutopsporing ondervindt van en uitvoeren van dit artikel leert u hoe het foutopsporingsprogramma samen met oplossingen voor algemene scenario's voor het oplossen van problemen werkt. 

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Hoe werkt de Application Insights momentopname foutopsporingsprogramma

Application Insights momentopname foutopsporingsprogramma deel uitmaakt van de Application Insights telemetrie pijplijn (een exemplaar van ITelemetryProcessor), de momentopname-collector bewaakt zowel de uitzonderingen die in uw code (AppDomain.FirstChanceException) en de uitzonderingen die door de pijplijn Application Insights-Uitzonderingstelemetrie bijgehouden. Nadat u hebt de collector momentopname aan uw project toegevoegd en er een uitzondering in de Application Insights-telemetrie pipeline, Application Insights aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en ' SnapshotCollectorEnabled' in de aangepaste gegevens worden verzonden. Op hetzelfde moment wordt een proces gestart met de naam van 'MinidumpUploader.exe' de verzamelde momentopnamen gegevensbestanden uploaden naar Application Insights.  Wanneer de 'MinidumpUploader.exe' wordt gestart verwerken, wordt er een aangepaste gebeurtenis met de naam 'UploaderStart' verzonden. Na de vorige stappen wordt de momentopname-collector de normale bewaking gedrag invoeren.

Terwijl de momentopname-collector wordt bewaakt door Application Insights-uitzonderingstelemetrie, gebruikt de parameters (bijvoorbeeld ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) zijn gedefinieerd de de configuratie om te bepalen wanneer een momentopname verzamelen. Wanneer alle regels is voldaan, wordt een momentopname voor de volgende uitzondering veroorzaakt op dezelfde plaats door de collector aanvragen. Tegelijk, wordt een Application Insights aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en 'RequestSnapshots' worden verzonden. Omdat de compiler 'Release' code optimaliseert, lokale variabelen mogelijk niet zichtbaar in de verzamelde momentopnamen. De momentopname-verzamelaar probeert te deoptimize van de methode die de uitzondering opgetreden bij het momentopnamen aanvraagt. Tijdens deze periode worden een Application Insights aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en 'ProductionBreakpointsDeOptimizeMethod' in de aangepaste gegevens verzonden.  Wanneer de momentopname van de volgende uitzondering wordt verzameld, wordt de lokale variabelen beschikbaar zijn. Nadat de momentopname wordt verzameld, wordt deze de code om de prestaties te optimaliseren. 

> [!NOTE]
> Deoptimization moet de extensie Application Insights site worden geïnstalleerd.

Wanneer een momentopname is aangevraagd voor een specifieke uitzondering, gaat de collector momentopname bewaking van uw toepassing uitzonderingsverwerking-pijplijn (AppDomain.FirstChanceException). Als de uitzondering zich weer voordoet, begint de collector een momentopname (Application Insights aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en 'SnapshotStart' in de aangepaste gegevens). En vervolgens een schaduwkopie van het actieve proces wordt gemaakt (de tabel pagina gedupliceerd). Dit duurt normaal gesproken 10 tot 20 milliseconden. Hierna wordt een Application Insights aangepaste gebeurtenis met de naam 'AppInsightsSnapshotCollectorLogs' en 'SnapshotStop' in de aangepaste gegevens worden verzonden. Wanneer het Gevorkte proces wordt gemaakt, wordt het totale wisselbaar geheugen worden verhoogd door even groot zijn als de wisselbaar geheugen van uw actieve toepassing (de werkset zijn veel kleiner). Tijdens het toepassingsproces van uw wordt uitgevoerd normaal gesproken de schaduwkopie wordt procesgeheugen van gedumpt naar schijf en geüpload naar Application Insights. Nadat de momentopname is geüpload, wordt er een Application Insights aangepaste gebeurtenis met de naam 'UploadSnapshotFinish' verzonden.

## <a name="is-the-snapshot-collector-working-properly"></a>De momentopname-collector goed werkt?

### <a name="how-to-find-snapshot-collector-logs"></a>Het zoeken naar Logboeken van de momentopname-Collector
Momentopname collector logboeken naar uw toepassing inzicht-account worden verzonden als de [momentopname Collector NuGet-pakket](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) versie 1.1.0 of hoger is geïnstalleerd. Zorg ervoor dat de *ProvideAnonymousTelemetry* niet is ingesteld op false (de waarde is standaard ingesteld op true).

* Navigeer naar uw Application Insights-resource in de Azure-portal.
* Klik op *Search* in de sectie overzicht.
* Voer de volgende tekenreeks in het zoekvak:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Opmerking: Wijzig de *tijdsbereik* indien nodig.

![Schermopname van zoeken momentopname Collector Logboeken](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Bekijk momentopname collector logboekbestanden
Bij het zoeken naar Logboeken in momentopname verzamelen, moet er 'UploadSnapshotFinish' gebeurtenissen in de betreffende periode. Als u de knop 'Fouten opsporen in momentopname openen' te openen van de momentopname niet ziet, stuur e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel.

![Schermopname van onderzoeken momentopname collector Logboeken](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Ik kan een momentopname openen niet vinden.
Als de volgende stappen niet, u het probleem worden opgelost kunt, stuurt u e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>Stap 1: Zorg ervoor dat uw toepassing is verzenden van telemetriegegevens en uitzonderingsgegevens naar Application Insights
Ga naar Application Insights-resource, Controleer of er gegevens die worden verzonden vanuit uw App.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>Stap 2: Controleer of momentopname collector correct is toegevoegd aan uw toepassing Application Insights Telemetry pijplijn
Als u Logboeken in de stap 'Het zoeken naar Logboeken in momentopname Collector vinden kunt' de collector momentopname correct wordt toegevoegd aan uw project, kunt u deze stap overslaan.

Als er geen momentopname collector Logboeken, Controleer het volgende:
* Controleer voor klassieke ASP.NET-toepassingen voor deze regel  *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">*  in de *ApplicationInsights.config* bestand.

* Zorg ervoor dat voor ASP.NET Core toepassingen, de *ITelemetryProcessorFactory* met *SnapshotCollectorTelemetryProcessor* wordt toegevoegd aan *IServiceCollection* services .

* Controleer ook of u de juiste instrumentatiesleutel in uw gepubliceerde toepassing.

* De momentopname-collector biedt geen ondersteuning voor meerdere instrumentatiesleutels in één toepassing, wordt deze momentopnamen verzonden naar de instrumentatiesleutel van de eerste uitzondering die het neemt.

* Als u de *InstrmentationKey* handmatig in uw code, werk de *InstrumentationKey* element uit de *ApplicationInsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>Stap 3: Controleer of dat de uploader minidump is gestart
Zoeken in de logboeken van de collector momentopname *UploaderStart* (UploaderStart typt in het zoekvak). Er moet een gebeurtenis geactiveerd wanneer de momentopname-collector de eerste uitzondering bewaakt. Als deze gebeurtenis niet bestaat, controleert u andere logboeken voor meer informatie. Een mogelijke manier om dit probleem op te lossen, is uw toepassing opnieuw te starten.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>Stap 4: Zorg ervoor dat momentopname Collector uitgedrukt in de doel-momentopnamen verzamelen
Zoeken in de logboeken van de collector momentopname *RequestSnapshots* (type ```RequestSnapshots``` in het zoekvak).  Als er geen een, Controleer uw configuratie, bijvoorbeeld *ThresholdForSnapshotting* wat aangeeft dat het nummer van een bepaalde uitzonderingen dat mag optreden voordat er begonnen wordt met het verzamelen van momentopnamen.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>Stap 5: Zorg ervoor dat de momentopname niet is uitgeschakeld vanwege de beveiliging van geheugen
Ter bescherming van de prestaties van uw toepassing, wordt een momentopname alleen worden vastgelegd als er een goede geheugenbuffer. Zoek in de logboeken van de collector snapshot 'CannotSnapshotDueToMemoryUsage'. Aangepaste gegevens voor de gebeurtenis heeft dit een gedetailleerde reden. Als uw toepassing wordt uitgevoerd in een Azure-Web-App, is het mogelijk dat de beperking strict. Omdat Azure-Web-App wordt uw app opnieuw wanneer aan bepaalde regels geheugen wordt voldaan. U kunt opschalen van uw service-plan voor computers met meer geheugen dit probleem kunt oplossen.

### <a name="step-6-make-sure-snapshots-were-captured"></a>Stap 6: Zorg ervoor dat momentopnamen zijn opgenomen
Zoeken in de logboeken van de collector momentopname ```RequestSnapshots```.  Als geen aanwezig zijn, controleert u uw configuratie, bijvoorbeeld ```ThresholdForSnapshotting``` dit geeft het getal van een bepaalde uitzonderingen voor het verzamelen van een momentopname.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>Stap 7: Zorg ervoor dat momentopnamen correct worden geüpload
Zoeken in de logboeken van de collector momentopname ```UploadSnapshotFinish```.  Als dit niet aanwezig is, stuurt u een e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel. Als deze gebeurtenis aanwezig is, opent u een van de logboeken en de waarde 'SnapshotId' in de aangepaste gegevens te kopiëren. Zoek vervolgens naar de waarde. Dit vindt u de uitzondering die overeenkomt met de momentopname. Klik vervolgens op de uitzondering en open foutopsporing momentopname. (Als er geen bijbehorende uitzondering, de uitzonderingstelemetrie mogelijk door actieve, vanwege de grote volumes, probeer een andere snapshotId.)

![Schermopname van zoeken SnapshotId](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Schermopname van Open uitzondering](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Schermopname openen foutopsporing momentopname](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Momentopname weergave lokale variabelen zijn niet voltooid

Sommige van de lokale variabelen ontbreken. Als uw toepassing kan release code wordt uitgevoerd, wordt de compiler sommige variabelen opslag te optimaliseren. Bijvoorbeeld:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Als uw geval is, kan een probleem zijn. Stuur een e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel samen met het volgende codefragment.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Snapshot-weergave: Kan de waarde van de lokale variabele of het argument niet ophalen
Controleer of de [Application Insights site-uitbreiding](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) is geïnstalleerd. Als het probleem zich blijft voordoen, stuurt u e-mail naar snapshothelp@microsoft.com met uw Application Insights-Instrumentatiesleutel.
