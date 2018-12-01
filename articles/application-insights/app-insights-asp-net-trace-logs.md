---
title: .NET-traceerlogboeken in Application Insights verkennen
description: Zoeken in logboeken gegenereerd met Trace, NLog en Log4Net.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: 258147498c4b37a5a00482090d5408f7c5619557
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52681293"
---
# <a name="explore-net-trace-logs-in-application-insights"></a>.NET-traceerlogboeken in Application Insights verkennen
Als u NLog, log4Net of System.Diagnostics.Trace voor diagnostische tracering in uw ASP.NET-toepassing, kunt u uw logboeken zijn verzonden naar hebben [Azure Application Insights][start], waar u kunt verkennen en zoeken ze. Uw logboeken wordt samengevoegd met de andere telemetrie die afkomstig zijn van uw toepassing, zodat u kunt de traces die zijn gekoppeld aan het onderhoud van elke gebruikersaanvraag identificeren en correlaties met andere gebeurtenissen en uitzonderingenrapporten.

> [!NOTE]
> Moet u de module toepassingslogboek vastleggen? Het is een handig adapter voor 3rd derden kunt, maar als u niet al gebruikt NLog, log4Net of System.Diagnostics.trace werkt, kunt u overwegen alleen aanroepende [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) rechtstreeks.
>
>

## <a name="install-logging-on-your-app"></a>Logboekregistratie op uw app installeren
Installeer het framework voor uw gekozen logboekregistratie in uw project. Dit moet resulteren in een vermelding in het app.config- of web.config.

Als u van System.Diagnostics.Trace gebruikmaakt, moet u een vermelding toevoegen aan web.config:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-to-collect-logs"></a>Application Insights configureren voor het verzamelen van Logboeken
**[Application Insights toevoegen aan uw project](app-insights-asp-net.md)**  als u dat nog niet hebt gedaan. Hier ziet u een optie om op te nemen van de logboekverzamelaar.

Of **Application Insights configureren** met de rechtermuisknop op uw project in Solution Explorer. Selecteer de optie voor **tracering verzamelen configureren**.

*Geen Application Insights menu of het logboek collector optie?* Probeer [probleemoplossing](#troubleshooting).

## <a name="manual-installation"></a>Handmatige installatie
Gebruik deze methode als uw projecttype wordt niet ondersteund door de Application Insights-installatieprogramma (bijvoorbeeld een Windows desktop-project).

1. Als u van plan bent om log4Net of NLog te gebruiken, installeert u deze in uw project.
2. Klik in Solution Explorer met de rechtermuisknop op uw project en kies **NuGet-pakketten beheren**.
3. Naar Application Insights zoeken
4. Selecteer het juiste pakket - een van:

   * Microsoft.ApplicationInsights.TraceListener (om vast te leggen aanroepen van System.Diagnostics.Trace)
   * Microsoft.ApplicationInsights.EventSourceListener (om vast te leggen EventSource gebeurtenissen)
   * Microsoft.ApplicationInsights.EtwCollector (om vast te leggen ETW-gebeurtenissen)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

Het NuGet-pakket installeert de benodigde assembly's en past tevens web.config of app.config.

## <a name="insert-diagnostic-log-calls"></a>Diagnostische logboeken aanroepen invoegen
Als u gebruikmaakt van System.Diagnostics.Trace, zou een typische aanroep:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Als u liever log4net of NLog:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>Gebeurtenisbron gebeurtenissen gebruiken
U kunt configureren [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen worden verzonden naar Application Insights als traceringen. Installeer eerst de `Microsoft.ApplicationInsights.EventSourceListener` NuGet-pakket. Bewerk vervolgens `TelemetryModules` sectie van de [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) bestand.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Voor elke bron, kunt u de volgende parameters instellen:
 * `Name` Hiermee geeft u de naam van de gebeurtenisbron te verzamelen.
 * `Level` Hiermee geeft u het niveau van logboekregistratie voor het verzamelen van. Een van `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords` (Optioneel) Hiermee geeft u de integerwaarde van combinaties van trefwoorden te gebruiken.

## <a name="using-diagnosticsource-events"></a>Met behulp van DiagnosticSource gebeurtenissen
U kunt configureren [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) gebeurtenissen worden verzonden naar Application Insights als traceringen. Installeer eerst de [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-pakket. Bewerk vervolgens de `TelemetryModules` sectie van de [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) bestand.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Voor elke DiagnosticSource die u traceren wilt, Voeg een vermelding met de `Name` kenmerk ingesteld op de naam van uw DiagnosticSource.

## <a name="using-etw-events"></a>Met behulp van ETW-gebeurtenissen
U kunt de ETW-gebeurtenissen worden verzonden naar Application Insights als traceringen configureren. Installeer eerst de `Microsoft.ApplicationInsights.EtwCollector` NuGet-pakket. Bewerk vervolgens `TelemetryModules` sectie van de [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) bestand.

> [!NOTE] 
> ETW-gebeurtenissen kunnen alleen worden verzameld als het proces voor het hosten van de SDK wordt uitgevoerd onder een identiteit die deel uitmaakt van 'Prestatielogboekgebruikers' of de groep Administrators.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Voor elke bron, kunt u de volgende parameters instellen:
 * `ProviderName` is de naam van de ETW-provider om te verzamelen.
 * `ProviderGuid` Hiermee geeft u de GUID van de ETW-provider voor het verzamelen, kunnen worden gebruikt in plaats van `ProviderName`.
 * `Level` Hiermee stelt u het niveau van logboekregistratie voor het verzamelen van. Een van `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords` (Optioneel) Hiermee stelt u de integerwaarde van combinaties van trefwoorden te gebruiken.

## <a name="using-the-trace-api-directly"></a>Rechtstreeks met behulp van de API-tracering
U kunt de Application Insights-tracering API rechtstreeks aanroepen. De logboekregistratie adapters gebruik deze API.

Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Een voordeel van TrackTrace is u kunt relatief veel gegevens in het bericht te plaatsen. U kunt bijvoorbeeld postgegevens er coderen.

Bovendien kunt u een ernstniveau toevoegen aan uw bericht. En net als andere telemetrie, kunt u eigenschapswaarden die u gebruiken kunt om te filteren of zoeken naar verschillende sets van traceringen toevoegen. Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Hiermee kunt u, zou [zoeken][diagnostic], voor het filteren eenvoudig de berichten van een bepaalde ernstniveau met betrekking tot een bepaalde database.

## <a name="explore-your-logs"></a>Verken uw logboeken
Uw app, hetzij in de foutopsporingsmodus uitvoeren of deze live te implementeren.

In de overzichtsblade van uw app in [de Application Insights-portal][portal], kiest u [zoeken][diagnostic].

![Kies in de Application Insights, zoeken](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Search](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

U kunt doen, bijvoorbeeld:

* Filteren op logboektraceringen of items met specifieke eigenschappen
* Een specifiek item in detail bekijken.
* Andere telemetrie die betrekking hebben op dezelfde gebruikersaanvraag vinden (dat wil zeggen, met dezelfde bewerkings-ID)
* De configuratie van deze pagina als favoriet opslaan

> [!NOTE]
> **Een steekproef.** Als uw toepassing grote hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger gebruikt, stuurt de functie voor adaptieve steekproeven mogelijk slechts een percentage van uw telemetrie. [Meer informatie over steekproeven.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Volgende stappen
[Diagnosefouten en uitzonderingen in ASP.NET][exceptions]

[Meer informatie over Search][diagnostic].

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="how-do-i-do-this-for-java"></a>Hoe moet ik dit doen voor Java?
Gebruik de [Java log adapters](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Er bestaat geen optie Application Insights in het contextmenu project
* Controleer Application Insights-hulpprogramma's is geïnstalleerd op deze machine ontwikkeling. In Visual Studio menu Extra Zoek extensies en Updates, naar Application Insights-hulpprogramma's. Als deze niet in het tabblad geïnstalleerd, opent u het tabblad Online en installeer deze.
* Dit wordt mogelijk een type project dat niet wordt ondersteund door Application Insights-hulpprogramma's. Gebruik [handmatige installatie](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Er is geen adapteroptie log in het configuratieprogramma
* U moet eerst installeren van het framework voor logboekregistratie.
* Als u van System.Diagnostics.Trace gebruikmaakt, zorg ervoor dat u [die zijn geconfigureerd, in `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Hebt u hebt de nieuwste versie van Application Insights? In Visual Studio **extra** menu, kiest u **extensies en Updates**, en open de **Updates** tabblad. Als Developer Analytics tools aanwezig is, klikt u op om bij te werken.

### <a name="emptykey"></a>Ik krijg de foutmelding 'instrumentatiesleutel mag niet leeg zijn'
Hebt u de logboekregistratie adapter Nuget-pakket geïnstalleerd zonder de installatie van Application Insights.

Klik in Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies **Update Application Insights**. U krijgt een dialoogvenster waarin u zich aanmeldt bij Azure uitnodigt en maak een Application Insights-resource, of gebruik een bestaande resourcegroep. Dat het probleem moet oplossen.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Ik kan zien traceringen in het doorzoeken van diagnostische gegevens, maar niet de andere gebeurtenissen
Soms duurt het even voor alle gebeurtenissen en aanvragen voor het ophalen via de pijplijn.

### <a name="limits"></a>Hoeveel gegevens worden bewaard?
Diverse factoren van invloed zijn op de hoeveelheid gegevens die worden bewaard. Zie de [limieten](app-insights-api-custom-events-metrics.md#limits) sectie van de metrische gegevens klantpagina voor meer informatie. 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a>Ik zie niet sommige van de logboekvermeldingen die ik verwacht
Als uw toepassing grote hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger gebruikt, stuurt de functie voor adaptieve steekproeven mogelijk slechts een percentage van uw telemetrie. [Meer informatie over steekproeven.](app-insights-sampling.md)

## <a name="add"></a>Volgende stappen
* [Beschikbaarheid en reactiesnelheid tests instellen][availability]
* [Problemen oplossen][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
