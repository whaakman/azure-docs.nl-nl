---
title: .NET-traceerlogboeken in Application Insights verkennen
description: Zoeken in logboeken die worden gegenereerd door de tracering, NLog en Log4Net.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbullwin
ms.openlocfilehash: d366f363b7bd1d5306d598c9b38258eb78076b7c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65472053"
---
# <a name="explore-netnet-core-trace-logs-in-application-insights"></a>.NET/.NET Core in Application Insights traceerlogboeken verkennen

Diagnostische traceringslogboeken voor de ASP.NET/ASP.NET Core-App verzenden via ILogger, NLog, log4Net of System.Diagnostics.Trace te [Azure Application Insights][start]. U kunt vervolgens verkennen en de logboeken doorzoeken. Deze logboeken worden samengevoegd met de andere logboekbestanden van uw toepassing, zodat u traces die zijn gekoppeld aan elke gebruikersaanvraag en deze correleren met andere gebeurtenissen en uitzonderingenrapporten kunt identificeren.

> [!NOTE]
> Moet u de module gegevens vastleggen? Er is een nuttig adapter voor derde partij kunt. Maar als u niet al NLog, log4Net of System.Diagnostics.Trace gebruikt, kunt u overwegen alleen aanroepende [ **Application Insights TrackTrace()** ](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) rechtstreeks.
>
>
## <a name="install-logging-on-your-app"></a>Logboekregistratie op uw app installeren
Installeer uw framework voor gekozen logboekregistratie in uw project, in een vermelding in het app.config- of web.config resulteren moet.

```XML
    <configuration>
      <system.diagnostics>
    <trace autoflush="true" indentsize="0">
      <listeners>
        <add name="myAppInsightsListener" type="Microsoft.ApplicationInsights.TraceListener.ApplicationInsightsTraceListener, Microsoft.ApplicationInsights.TraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
   </configuration>
```

## <a name="configure-application-insights-to-collect-logs"></a>Application Insights configureren voor het verzamelen van Logboeken
[Application Insights toevoegen aan uw project](../../azure-monitor/app/asp-net.md) als u dat nog niet hebt gedaan. Hier ziet u een optie om op te nemen van de logboekverzamelaar.

Of met de rechtermuisknop op uw project in Solution Explorer op **Application Insights configureren**. Selecteer de **tracering verzamelen configureren** optie.

> [!NOTE]
> Geen Application Insights menu of het logboek collector optie? Probeer [probleemoplossing](#troubleshooting).

## <a name="manual-installation"></a>Handmatige installatie
Gebruik deze methode als uw projecttype wordt niet ondersteund door de Application Insights-installatieprogramma (bijvoorbeeld een Windows desktop-project).

1. Als u van plan bent om log4Net of NLog te gebruiken, installeert u deze in uw project.
2. Klik in Solution Explorer met de rechtermuisknop op uw project en selecteer **NuGet-pakketten beheren**.
3. Zoek naar 'Application Insights'.
4. Selecteer een van de volgende pakketten:

   - Voor ILogger: [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.Extensions.Logging.ApplicationInsights.svg)](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
   - Voor NLog: [Microsoft.ApplicationInsights.NLogTarget](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.NLogTarget.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
   - Voor Log4Net: [Microsoft.ApplicationInsights.Log4NetAppender](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.Log4NetAppender.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
   - Voor System.Diagnostics: [Microsoft.ApplicationInsights.TraceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.TraceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
   - [Microsoft.ApplicationInsights.DiagnosticSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.DiagnosticSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
   - [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EtwCollector.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
   - [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EventSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)

Het NuGet-pakket installeert de benodigde assembly en past web.config of app.config als die van toepassing is.

## <a name="ilogger"></a>ILogger

Zie voor meer voorbeelden van het gebruik van de implementatie van de Application Insights ILogger met consoletoepassingen en ASP.NET Core [ApplicationInsightsLoggerProvider voor .NET Core ILogger registreert](ilogger.md).

## <a name="insert-diagnostic-log-calls"></a>Diagnostische logboeken aanroepen invoegen
Als u gebruikmaakt van System.Diagnostics.Trace, zou een typische aanroep:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Als u liever log4net of NLog, gebruikt:

    logger.Warn("Slow response - database01");

## <a name="use-eventsource-events"></a>Gebeurtenisbron gebeurtenissen gebruiken
U kunt configureren [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen worden verzonden naar Application Insights als traceringen. Installeer eerst de `Microsoft.ApplicationInsights.EventSourceListener` NuGet-pakket. Bewerk vervolgens de `TelemetryModules` sectie van de [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) bestand.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Voor elke bron, kunt u de volgende parameters instellen:
 * **Naam** geeft de naam van de gebeurtenisbron te verzamelen.
 * **Niveau** Hiermee geeft u het niveau van logboekregistratie voor het verzamelen van: *Kritieke*, *fout*, *informatief*, *LogAlways*, *uitgebreide*, of *waarschuwing*.
 * **Trefwoorden** (optioneel) Geef de integerwaarde van combinaties van trefwoorden te gebruiken.

## <a name="use-diagnosticsource-events"></a>DiagnosticSource gebeurtenissen gebruiken
U kunt configureren [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) gebeurtenissen worden verzonden naar Application Insights als traceringen. Installeer eerst de [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-pakket. Bewerk vervolgens de sectie 'TelemetryModules' van de [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) bestand.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Voor elke DiagnosticSource die u traceren wilt, Voeg een vermelding met de **naam** kenmerk ingesteld op de naam van uw DiagnosticSource.

## <a name="use-etw-events"></a>ETW-gebeurtenissen gebruiken
U kunt Event Tracing voor Windows (ETW) gebeurtenissen worden verzonden naar Application Insights als traceringen configureren. Installeer eerst de `Microsoft.ApplicationInsights.EtwCollector` NuGet-pakket. Bewerk vervolgens de sectie 'TelemetryModules' van de [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) bestand.

> [!NOTE] 
> ETW-gebeurtenissen kunnen alleen worden verzameld als het proces dat als host fungeert voor de SDK wordt uitgevoerd onder een identiteit die deel uitmaakt van Prestatielogboekgebruikers of de groep Administrators.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Voor elke bron, kunt u de volgende parameters instellen:
 * **ProviderName** is de naam van de ETW-provider om te verzamelen.
 * **ProviderGuid** Hiermee geeft u de GUID van de ETW-provider om te verzamelen. Het kan worden gebruikt in plaats van `ProviderName`.
 * **Niveau** Hiermee stelt u het niveau van logboekregistratie voor het verzamelen van. Kan het zijn *kritieke*, *fout*, *informatief*, *LogAlways*, *uitgebreid*, of *Waarschuwing*.
 * **Trefwoorden** (optioneel) Stel de integerwaarde van combinaties van trefwoorden te gebruiken.

## <a name="use-the-trace-api-directly"></a>De API-tracering rechtstreeks gebruiken
U kunt de Application Insights-tracering API rechtstreeks aanroepen. De logboekregistratie adapters gebruik deze API.

Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Een voordeel van TrackTrace is u kunt relatief veel gegevens in het bericht te plaatsen. Bijvoorbeeld, kunt u er postgegevens coderen.

U kunt ook een ernstniveau toevoegen aan uw bericht. En net als andere telemetrie, kunt u eigenschapswaarden om te filteren of zoeken naar verschillende sets van traceringen toevoegen. Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Dit zou kunnen u gemakkelijk kunt uitfilteren [zoeken] [ diagnostic] alle berichten van een specifieke ernst op die betrekking op een bepaalde database hebben.

## <a name="explore-your-logs"></a>Verken uw logboeken
Uw app in de foutopsporingsmodus uitvoeren of deze live te implementeren.

In het overzichtsvenster van uw app in [de Application Insights-portal][portal], selecteer [zoeken][diagnostic].

U kunt doen, bijvoorbeeld:

* Filteren op logboektraceringen of items met specifieke eigenschappen.
* Een specifiek item in detail bekijken.
* Andere system-logboekgegevens die is gekoppeld aan dezelfde gebruikersaanvraag vinden (heeft dezelfde bewerkings-ID).
* De configuratie van een pagina als favoriet opslaan.

> [!NOTE]
>Als uw toepassing grote hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger, gebruikt de *adaptieve steekproeven* functie kan werken en slechts een gedeelte van uw telemetrie te verzenden. [Meer informatie over steekproeven.](../../azure-monitor/app/sampling.md)
>

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="how-do-i-do-this-for-java"></a>Hoe moet ik dit doen voor Java?
Gebruik de [Java log adapters](../../azure-monitor/app/java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Er bestaat geen optie Application Insights in het contextmenu project
* Zorg ervoor dat Developer Analytics Tools is geïnstalleerd op de ontwikkelcomputer. In Visual Studio **extra** > **extensies en Updates**, zoek naar **Developer Analytics Tools**. Als dit niet op de **geïnstalleerde** tabblad, open de **Online** tabblad en installeer deze.
* Dit wordt mogelijk een projecttype dat biedt geen ondersteuning voor Devloper Analytics-hulpprogramma's. Gebruik [handmatige installatie](#manual-installation).

### <a name="theres-no-log-adapter-option-in-the-configuration-tool"></a>Er is geen log-adapter-optie in het configuratieprogramma
* Installeer eerst het framework voor logboekregistratie.
* Als u van System.Diagnostics.Trace gebruikmaakt, zorg ervoor dat u deze hebt [geconfigureerd in *web.config*](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Zorg ervoor dat u de nieuwste versie van Application Insights hebt. Ga in Visual Studio naar **extra** > **extensies en Updates**, en open de **Updates** tabblad. Als **Developer Analytics Tools** is, selecteert u deze bij te werken.

### <a name="emptykey"></a>Ik krijg het foutbericht 'instrumentatiesleutel mag niet leeg zijn'
U waarschijnlijk de logboekregistratie adapter Nuget-pakket geïnstalleerd zonder te installeren van Application Insights. Klik in Solution Explorer met de rechtermuisknop op *ApplicationInsights.config*, en selecteer **Update Application Insights**. U wordt gevraagd om te melden bij Azure en maak een Application Insights-resource of opnieuw gebruiken van een bestaande. Dat het probleem moet oplossen.

### <a name="i-can-see-traces-but-not-other-events-in-diagnostic-search"></a>Ik kan zien traceringen, maar geen andere gebeurtenissen in diagnostische gegevens doorzoeken
Het kan even voor alle gebeurtenissen en aanvragen voor het ophalen via de pijplijn.

### <a name="limits"></a>Hoeveel gegevens worden bewaard?
Diverse factoren van invloed zijn op de hoeveelheid gegevens die behouden blijven. Zie voor meer informatie de [limieten](../../azure-monitor/app/api-custom-events-metrics.md#limits) sectie van de pagina klant gebeurtenis metrische gegevens.

### <a name="i-dont-see-some-log-entries-that-i-expected"></a>Ik zie niet sommige logboekvermeldingen die ik verwacht
Als uw toepassing omvangrijke hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger gebruikt, wordt de functie voor adaptieve steekproeven werken en wordt alleen een gedeelte van uw telemetrie te verzenden. [Meer informatie over steekproeven.](../../azure-monitor/app/sampling.md)

## <a name="add"></a>Volgende stappen

* [Diagnosefouten en uitzonderingen in ASP.NET][exceptions]
* [Meer informatie over zoeken][diagnostic]
* [Beschikbaarheid en reactiesnelheid tests instellen][availability]
* [Problemen oplossen][qna]

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[start]: ../../azure-monitor/app/app-insights-overview.md