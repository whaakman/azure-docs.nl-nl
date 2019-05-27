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
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mbullwin
ms.openlocfilehash: cff4aaaab97fdcecab9cdf1d0dff2786f86b604b
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966712"
---
# <a name="system-performance-counters-in-application-insights"></a>Systeemprestatiemeteritems in Application Insights

Windows biedt een grote verscheidenheid aan [prestatiemeteritems](https://docs.microsoft.com/windows/desktop/PerfCtrs/about-performance-counters), bijvoorbeeld voor CPU-bezetting, geheugen, schijfruimte en netwerkgebruik. U kunt ook uw eigen prestatiemeteritems definiëren. Prestatieverzameling van tellers wordt ondersteund, zolang uw toepassing onder IIS wordt uitgevoerd op een on-premises-host of virtuele machine waarop u beheerderstoegang hebben. Hoewel toepassingen worden uitgevoerd als Azure Web Apps niet rechtstreeks toegang tot prestatiemeteritems hebben, een subset van de beschikbare items worden verzameld door Application Insights.

## <a name="view-counters"></a>Items weergeven

Het deelvenster metrische gegevens bevat de standaardset van prestatiemeteritems.

![Prestatiemeteritems die zijn gerapporteerd in Application Insights](./media/performance-counters/performance-counters.png)

De huidige standaard-items die zijn geconfigureerd om te worden verzameld voor ASP.NET/ASP.NET Core web-apps zijn:

         - % Process\\Processor Time
         - % Process\\Processor Time Normalized
         - Memory\\Available Bytes
         - ASP.NET Requests/Sec
         - .NET CLR Exceptions Thrown / sec
         - ASP.NET ApplicationsRequest Execution Time
         - Process\\Private Bytes
         - Process\\IO Data Bytes/sec
         - ASP.NET Applications\\Requests In Application Queue
         - Processor(_Total)\\% Processor Time

Als u wilt zien van de handigste grafieken op één plek, maakt u een [dashboard](../../azure-monitor/app/app-insights-dashboards.md) en deze vastmaken aan het.

## <a name="add-counters"></a>Items toevoegen

Als het prestatiemeteritem dat u wilt dat niet is opgenomen in de lijst met metrische gegevens, kunt u deze kunt toevoegen.

1. Ontdek welke items beschikbaar in de server zijn met behulp van deze PowerShell-opdracht op de lokale server:

    `Get-Counter -ListSet *`

    (Zie [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Open ApplicationInsights.config.

   * Als u Application Insights aan uw app toegevoegd tijdens de ontwikkeling, bewerk ApplicationInsights.config in uw project en vervolgens opnieuw te op uw servers implementeren.
3. De richtlijn van de collector prestaties bewerken:

```XML

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

> [!NOTE]
> ASP.NET Core-toepassingen geen `ApplicationInsights.config`, en kan daarom de bovenstaande methode is niet geldig voor ASP.NET Core-toepassingen.

U kunt vastleggen zowel standard tellers en die dat u zelf hebt geïmplementeerd. `\Objects\Processes` is een voorbeeld van een standard-teller die beschikbaar is op alle Windows-systemen. `\Sales(photo)\# Items Sold` is een voorbeeld van een aangepaste teller die kan worden geïmplementeerd in een webservice.

De indeling is `\Category(instance)\Counter"`, of alleen voor categorieën waarvoor geen instanties, `\Category\Counter`.

`ReportAs` is vereist voor de namen van prestatiemeteritems die niet overeenkomen met `[a-zA-Z()/-_ \.]+` -dat wil zeggen, ze zich niet in de volgende sets tekens bevatten: letters, ronde haken, schuine streep, streepje, onderstrepingsteken, spatie, punt.

Als u een exemplaar opgeeft, wordt deze als een dimensie "CounterInstanceName' van de gerapporteerde metrische gegevens worden verzameld.

### <a name="collecting-performance-counters-in-code-for-aspnet-web-applications-or-netnet-core-console-applications"></a>Verzamelen van prestatiemeteritems in de code voor ASP.NET-webtoepassingen of.NET/.NET Core consoletoepassingen
Als u wilt verzamelen van prestatiemeteritems van systeem en ze verzenden naar Application Insights, kunt u het onderstaande codefragment aanpassen:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Process([replace-with-application-process-name])\Page Faults/sec", "PageFaultsPerfSec")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Of u kunt hetzelfde doen met aangepaste metrische gegevens die u hebt gemaakt:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="collecting-performance-counters-in-code-for-aspnet-core-web-applications"></a>Verzamelen van prestatiemeteritems in de code voor ASP.NET Core Web-Apps

Wijzigen `ConfigureServices` methode in uw `Startup.cs` klasse zoals hieronder.

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures PerformanceCollectorModule.
  services.ConfigureTelemetryModule<PerformanceCollectorModule>((module, o) =>
            {
                // the application process name could be "dotnet" for ASP.NET Core self-hosted applications.
                module.Counters.Add(new PerformanceCounterCollectionRequest(
    @"\Process([replace-with-application-process-name])\Page Faults/sec", "DotnetPageFaultsPerfSec"));
            });
    }
```

## <a name="performance-counters-in-analytics"></a>Prestatiemeteritems in Analytics
U kunt zoeken en weergeven van prestaties teller rapporten in [Analytics](../../azure-monitor/app/analytics.md).

De **performanceCounters** schema wordt aangegeven dat de `category`, `counter` naam, en `instance` naam van elk prestatiemeteritem.  In de telemetrie voor elke toepassing ziet u alleen de items voor de toepassing. Bijvoorbeeld, als u wilt zien zijn welke items beschikbaar: 

![Prestatiemeteritems in Application Insights analytics](./media/performance-counters/analytics-performance-counters.png)

('Instance' Hier verwijst naar het exemplaar van prestatiemeteritem, niet de rol of server-machine-exemplaar. De naam exemplaar Prestatiemeter doorgaans segmenten items zoals processortijd door de naam van het proces of de toepassing.)

Een grafiek van het beschikbare geheugen in de recente periode ophalen: 

![Geheugen tijdgrafiek in Application Insights analytics](./media/performance-counters/analytics-available-memory.png)

Andere telemetrie, zoals **performanceCounters** heeft ook een kolom `cloud_RoleInstance` die aangeeft dat de identiteit van het host-server-exemplaar waarop uw app wordt uitgevoerd. Als u bijvoorbeeld kunnen de prestaties van uw app op verschillende computers: 

![Prestaties gesegmenteerd op rolinstantie in Application Insights analytics](./media/performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET en Application Insights tellingen

*Wat is het verschil tussen het aantal uitzonderingen en uitzonderingen metrische gegevens?*

* *Aantal uitzonderingen* is van een prestatiemeteritem system. De CLR telt alle de verwerkte en onverwerkte uitzonderingen die worden gegenereerd en de totale in een interval van steekproeven worden gedeeld door de lengte van het interval. De Application Insights SDK dit resultaat verzamelt en verzendt ze naar de portal.

* *Uitzonderingen* is een telling van de TrackException-rapporten ontvangen door de portal in het controle-interval van de grafiek. Het bevat alleen de verwerkte uitzonderingen waar u TrackException aanroepen in uw code en bevat geen alle hebt geschreven [onverwerkte uitzonderingen](../../azure-monitor/app/asp-net-exceptions.md). 

## <a name="performance-counters-for-applications-running-in-azure-web-apps"></a>Prestatiemeters voor toepassingen die worden uitgevoerd in Azure Web Apps

ASP.NET- en ASP.NET Core toepassingen die zijn geïmplementeerd in Azure Web Apps uitvoeren in een speciale sandbox-omgeving. Directe toegang tot de systeemprestatiemeteritems zijn niet toegestaan voor deze omgeving. Echter, een beperkte subset van items worden weergegeven als omgevingsvariabelen zoals wordt beschreven [hier](https://github.com/projectkudu/kudu/wiki/Perf-Counters-exposed-as-environment-variables). Application Insights SDK voor ASP.NET en ASP.NET Core verzamelt prestatiemeters van Azure Web Apps van deze speciale omgevingsvariabelen. Alleen een subset van items zijn beschikbaar in deze omgeving en de volledige lijst vindt [hier.](https://github.com/microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/PerformanceCollector/Perf.Shared/Implementation/WebAppPerformanceCollector/CounterFactory.cs)

## <a name="performance-counters-in-aspnet-core-applications"></a>Prestatiemeteritems in ASP.NET Core-toepassingen

* [ASP.NET Core-SDK](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versie 2.4.1 en verzamelt prestatiemeters boven als de toepassing wordt uitgevoerd in Azure Web App (Windows)

* SDK-versie 2.7.0-beta3 en verzamelt prestatiemeters boven als de toepassing wordt uitgevoerd in Windows, en die zijn gericht op `NETSTANDARD2.0` of hoger.
* Voor toepassingen die zijn gericht op het .NET Framework, worden prestatiemeteritems ondersteund in alle versies van de SDK.
* In dit artikel wordt bijgewerkt wanneer prestaties teller ondersteuning in niet-Windows wordt toegevoegd.

## <a name="alerts"></a>Waarschuwingen
Net als andere metrische gegevens, kunt u [een melding instellen](../../azure-monitor/app/alerts.md) om u te waarschuwen als een prestatiemeteritem gaat buiten een grens die u opgeeft. Open het deelvenster waarschuwingen en klik op waarschuwing toevoegen.

## <a name="next"></a>Volgende stappen

* [Bijhouden van afhankelijkheid](../../azure-monitor/app/asp-net-dependencies.md)
* [Uitzonderingen bijhouden](../../azure-monitor/app/asp-net-exceptions.md)

