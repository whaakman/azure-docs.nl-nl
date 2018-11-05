---
title: Prestatiemeteritems in Application Insights | Microsoft Docs
description: Monitor het systeem en aangepaste .NET-prestatiemeteritems in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/11/2016
ms.author: mbullwin
ms.openlocfilehash: e5915f18799386ae92019073fb50dac96da107ea
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960103"
---
# <a name="system-performance-counters-in-application-insights"></a>Systeemprestatiemeteritems in Application Insights
Windows biedt een groot aantal verschillende [prestatiemeteritems](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) , zoals CPU-bezetting, geheugen, schijf en netwerkgebruik. U kunt ook uw eigen links definiëren. [Application Insights](app-insights-overview.md) weergeven van deze prestatiemeteritems als uw toepassing onder IIS wordt uitgevoerd op een on-premises-host of virtuele machine waarop u beheerderstoegang hebben. De grafieken geven van de beschikbare resources voor uw live-toepassing en helpen bij het identificeren van niet-gebalanceerde load tussen exemplaren van de server.

Prestatiemeteritems worden weergegeven in de blade Servers, waaronder een tabel die segmenten van server-exemplaar.

![Prestatiemeteritems die zijn gerapporteerd in Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Prestatiemeteritems zijn niet beschikbaar voor Azure Web Apps. Maar u kunt de [Azure diagnostische gegevens verzenden naar Application Insights](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).)

## <a name="view-counters"></a>Items weergeven
De blade Servers wordt een standaardset prestatiemeteritems. 

Als u wilt zien van andere items, op de grafieken op de blade Servers bewerken of open een nieuw [Metrics Explorer](app-insights-metrics-explorer.md) blade en toevoegen van nieuwe grafieken. 

De beschikbare items worden weergegeven als de metrische gegevens over wanneer u een grafiek bewerken.

![Prestatiemeteritems die zijn gerapporteerd in Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

Als u wilt zien van de handigste grafieken op één plek, maakt u een [dashboard](app-insights-dashboards.md) en deze vastmaken aan het.

## <a name="add-counters"></a>Items toevoegen
Als het prestatiemeteritem dat u wilt dat niet wordt weergegeven in de lijst met metrische gegevens, wordt dat komt doordat de Application Insights-SDK wordt niet verzamelen in uw webserver. U kunt configureren dat deze om dit te doen.

1. Ontdek welke items beschikbaar in de server zijn met behulp van deze PowerShell-opdracht op de server:
   
    `Get-Counter -ListSet *`
   
    (Zie [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Open ApplicationInsights.config.
   
   * Als u Application Insights aan uw app toegevoegd tijdens de ontwikkeling, bewerk ApplicationInsights.config in uw project en vervolgens opnieuw op uw servers implementeert.
   * Als u Status Monitor naar een web-app tijdens runtime instrumenteren gebruikt, moet u ApplicationInsights.config vinden in de hoofdmap van de app in IIS. Deze er op elke server-exemplaar bijwerken.
3. De richtlijn van de collector prestaties bewerken:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

U kunt vastleggen zowel standard tellers en die dat u zelf hebt geïmplementeerd. `\Objects\Processes` is een voorbeeld van een standard prestatiemeteritem beschikbaar is op alle Windows-systemen. `\Sales(photo)\# Items Sold` is een voorbeeld van een aangepaste teller die kan worden geïmplementeerd in een webservice. 

De indeling is `\Category(instance)\Counter"`, of alleen voor categorieën waarvoor geen instanties, `\Category\Counter`.

`ReportAs` is vereist voor de namen van prestatiemeteritems die niet overeenkomen met `[a-zA-Z()/-_ \.]+` -dat wil zeggen, ze zich niet in de volgende sets tekens bevatten: letters, ronde haken, schuine streep, streepje, onderstrepingsteken, spatie, punt.

Als u een exemplaar opgeeft, wordt deze als een dimensie "CounterInstanceName' van de gerapporteerde metrische gegevens worden verzameld.

### <a name="collecting-performance-counters-in-code"></a>Verzamelen van prestatiemeteritems in code
Als u wilt verzamelen van prestatiemeteritems van systeem en ze verzenden naar Application Insights, kunt u het onderstaande codefragment aanpassen:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```
Of u kunt hetzelfde doen met aangepaste metrische gegevens die u hebt gemaakt:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Prestatiemeteritems in Analytics
U kunt zoeken en weergeven van prestaties teller rapporten in [Analytics](app-insights-analytics.md).

De **performanceCounters** schema wordt aangegeven dat de `category`, `counter` naam, en `instance` naam van elk prestatiemeteritem.  In de telemetrie voor elke toepassing ziet u alles alleen de items voor de toepassing. Bijvoorbeeld, als u wilt zien zijn welke items beschikbaar: 

![Prestatiemeteritems in Application Insights analytics](./media/app-insights-performance-counters/analytics-performance-counters.png)

('Instantie' verwijst hier naar het exemplaar van prestatiemeteritem, niet de functie of exemplaar van de machine. De naam exemplaar Prestatiemeter doorgaans segmenten items zoals processortijd door de naam van het proces of de toepassing.)

Een grafiek van het beschikbare geheugen in de recente periode ophalen: 

![Geheugen tijdgrafiek in Application Insights analytics](./media/app-insights-performance-counters/analytics-available-memory.png)

Andere telemetrie, zoals **performanceCounters** heeft ook een kolom `cloud_RoleInstance` die aangeeft dat de identiteit van het host-server-exemplaar waarop uw app wordt uitgevoerd. Als u bijvoorbeeld kunnen de prestaties van uw app op verschillende computers: 

![Prestaties gesegmenteerd op rolinstantie in Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET en Application Insights tellingen
*Wat is het verschil tussen het aantal uitzonderingen en uitzonderingen metrische gegevens?*

* *Aantal uitzonderingen* is van een prestatiemeteritem system. De CLR telt alle de verwerkte en onverwerkte uitzonderingen die worden gegenereerd en de totale in een interval van steekproeven worden gedeeld door de lengte van het interval. De Application Insights SDK dit resultaat verzamelt en verzendt ze naar de portal.
* *Uitzonderingen* is een telling van de TrackException-rapporten ontvangen door de portal in het controle-interval van de grafiek. Het bevat alleen de verwerkte uitzonderingen waar u TrackException aanroepen in uw code en bevat geen alle hebt geschreven [onverwerkte uitzonderingen](app-insights-asp-net-exceptions.md). 

## <a name="performance-counters-in-aspnet-core-applications"></a>Prestatiemeteritems in Asp.Net Core-toepassingen
Prestatiemeteritems worden alleen ondersteund als de toepassing is die gericht is op het volledige .NET Framework. Er is geen mogelijkheid voor het verzamelen van prestatiemeteritems voor .net Core-toepassingen.

## <a name="alerts"></a>Waarschuwingen
Net als andere metrische gegevens, kunt u [een melding instellen](app-insights-alerts.md) om u te waarschuwen als een prestatiemeteritem gaat buiten een grens die u opgeeft. Open de blade waarschuwingen en klikt u op een waarschuwing toevoegen.

## <a name="next"></a>Volgende stappen
* [Bijhouden van afhankelijkheid](app-insights-asp-net-dependencies.md)
* [Uitzonderingen bijhouden](app-insights-asp-net-exceptions.md)

