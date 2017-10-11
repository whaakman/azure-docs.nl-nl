---
title: Prestatiemeters in Application Insights | Microsoft Docs
description: Systeem- en aangepaste .NET-prestatiemeters in Application Insights bewaken.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: bwren
ms.openlocfilehash: 038d6e051be8112b9264e7efa6485965d11e32c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="system-performance-counters-in-application-insights"></a>Systeemprestatiemeteritems in Application Insights
Windows biedt een groot aantal [prestatiemeteritems](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) zoals bezetting CPU, geheugen, schijf en netwerkgebruik. U kunt ook uw eigen links definiëren. [Application Insights](app-insights-overview.md) weergeven van deze prestatiemeteritems als uw toepassing onder IIS wordt uitgevoerd op een lokale host of virtuele machine waarop u beheerderstoegang hebben. De diagrammen geven de resources die beschikbaar zijn voor uw live-toepassing en helpen bij het identificeren van onbalans load tussen serverexemplaren.

Prestatiemeteritems weergegeven in de blade Servers, waaronder een tabel die segmenten door server-exemplaar.

![Prestatie-items die zijn gerapporteerd in Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Prestatie-items zijn niet beschikbaar voor Web-Apps van Azure. Maar u kunt [diagnostische Azure-gegevens verzenden naar Application Insights](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Items weergeven
De Servers blade ziet u een standaardset prestatiemeteritems. 

Overzicht van andere items op de grafieken op de blade Servers bewerken of open een nieuw [Metrics Explorer](app-insights-metrics-explorer.md) blade en voegen nieuwe grafieken. 

De beschikbare items worden weergegeven als metrische gegevens wanneer u een grafiek bewerken.

![Prestatie-items die zijn gerapporteerd in Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

Overzicht van de handigste grafieken op één plek maken een [dashboard](app-insights-dashboards.md) en vastmaken aan.

## <a name="add-counters"></a>Items toevoegen
Als het prestatiemeteritem dat u wilt dat niet wordt weergegeven in de lijst met metrische gegevens, gebeurt dit omdat de Application Insights-SDK wordt niet worden verzameld in uw webserver. U kunt deze configureren om dit te doen.

1. Ontdek welke items beschikbaar in uw server zijn met behulp van deze PowerShell-opdracht op de server:
   
    `Get-Counter -ListSet *`
   
    (Zie [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Open ApplicationInsights.config.
   
   * Als u Application Insights hebt toegevoegd aan uw app tijdens de ontwikkeling, bewerk ApplicationInsights.config in uw project en vervolgens opnieuw implementeren op uw servers.
   * Als u een web-app tijdens runtime softwareontwikkelaars Status Monitor hebt gebruikt, moet u ApplicationInsights.config vinden in de hoofdmap van de app in IIS. Deze er op elk serverexemplaar bijwerken.
3. De prestaties collector-instructie bewerken:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

U kunt vastleggen standaard tellers en die dat u zelf hebt geïmplementeerd. `\Objects\Processes`een voorbeeld van een standaard prestatiemeteritem is beschikbaar op alle Windows-systemen. `\Sales(photo)\# Items Sold`is een voorbeeld van een aangepaste teller die mogelijk zijn geïmplementeerd in een webservice. 

De indeling is `\Category(instance)\Counter"`, of gewoon voor categorieën waarvoor geen exemplaren `\Category\Counter`.

`ReportAs`is vereist voor de namen van prestatiemeteritems die komen niet overeen met `[a-zA-Z()/-_ \.]+` -dat wil zeggen, ze tekens bevatten die niet zijn opgenomen in de volgende sets: letters, ronde haakjes, schuine streep, afbreekstreepje, onderstrepingstekens, ruimte, punt.

Als u een exemplaar opgeeft, wordt deze als een dimensie 'CounterInstanceName' van de gerapporteerde metrische gegevens worden verzameld.

### <a name="collecting-performance-counters-in-code"></a>Verzamelen van prestatiemeteritems in de code
Om te verzamelen systeemprestatiemeteritems en ze verzenden naar Application Insights, kunt u het onderstaande codefragment aanpassen:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Of u kunt dit ook doen met aangepaste metrische gegevens die u hebt gemaakt:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Prestatiemeters in Analytics
U kunt zoeken en weergeven van de teller prestatierapporten in [Analytics](app-insights-analytics.md).

De **performanceCounters** schema beschrijft de `category`, `counter` naam, en `instance` naam van elk prestatiemeteritem.  In de telemetrie voor elke toepassing ziet u alleen de items voor die toepassing. Bijvoorbeeld, als u wilt zien zijn welke items beschikbaar: 

![Prestatiemeters in Application Insights analytics](./media/app-insights-performance-counters/analytics-performance-counters.png)

(De instantie' verwijst hier naar het exemplaar van het prestatiemeteritem niet de functie of servergroep machine exemplaar. De naam exemplaar Prestatiemeter doorgaans segmenten items zoals processortijd door de naam van de toepassing of proces.)

Een grafiek van het beschikbare geheugen over de afgelopen periode ophalen: 

![Geheugen timechart in Application Insights analytics](./media/app-insights-performance-counters/analytics-available-memory.png)

Zoals u andere telemetrie **performanceCounters** heeft ook een kolom `cloud_RoleInstance` die aangeeft dat de identiteit van de host-server-exemplaar waarop uw app wordt uitgevoerd. Als u bijvoorbeeld wilt vergelijken de prestaties van uw app in de verschillende machines: 

![Prestaties gesegmenteerd op rolinstantie in Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET en Application Insights tellingen
*Wat is het verschil tussen de snelheid van de uitzondering en uitzonderingen metrische gegevens?*

* *Snelheid van de uitzondering* is van een prestatiemeteritem voor het systeem. De CLR telt alle verwerkte en onverwerkte uitzonderingen die worden gegenereerd en de totale binnen een interval van steekproeven worden gedeeld door de lengte van het interval. De Application Insights-SDK dit resultaat verzamelt en verzendt het naar de portal.
* *Uitzonderingen* is een aantal van de TrackException-rapporten ontvangen door de portal in het controle-interval van de grafiek. Het bevat alleen de verwerkte uitzonderingen waar u TrackException aanroept in uw code en niet alle opgenomen hebt geschreven [onverwerkte uitzonderingen](app-insights-asp-net-exceptions.md). 

## <a name="alerts"></a>Waarschuwingen
Net als andere metrische gegevens, kunt u [een waarschuwing instellen](app-insights-alerts.md) om u te waarschuwen als een prestatiemeteritem gaat buiten een grens die u opgeeft. Open de blade waarschuwingen en klik op waarschuwing toevoegen.

## <a name="next"></a>Volgende stappen
* [Bijhouden van afhankelijkheid](app-insights-asp-net-dependencies.md)
* [Uitzonderingen bijhouden](app-insights-asp-net-exceptions.md)

