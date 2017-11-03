---
title: .NET-traceerlogboeken in Application Insights verkennen
description: Zoeken naar Logboeken die worden gegenereerd met Trace, NLog en Log4Net.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: 6da0bf009fa71885d7d8e3bd5376c5a7c9d4a344
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a>.NET-traceerlogboeken in Application Insights verkennen
Als u NLog, log4Net of System.Diagnostics.Trace voor diagnostische tracering in uw ASP.NET-toepassing kunt u uw verzonden naar Logboeken hebben [Azure Application Insights][start], waar u kunt verkennen en ze te zoeken. Uw logboeken worden samengevoegd met de andere telemetrie die afkomstig zijn van uw toepassing, zodat de traceringen die zijn gekoppeld aan het onderhoud van de aanvraag van elke gebruiker te identificeren en ze met andere gebeurtenissen en de uitzonderingenrapporten correleren.

> [!NOTE]
> Moet u de module toepassingslogboek vastleggen? Het is een nuttig adapter voor 3rd derden voorkomen, maar als u niet al gebruikt NLog, log4Net of System.Diagnostics.Trace, kunt u alleen aanroepen [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) rechtstreeks.
>
>

## <a name="install-logging-on-your-app"></a>Aanmelden met uw app installeren
Uw framework voor logboekregistratie gekozen installeren in uw project. Dit moet resulteren in een vermelding in het app.config- of web.config.

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
## <a name="configure-application-insights-to-collect-logs"></a>Application Insights voor het verzamelen van logboeken configureren
**[Application Insights toevoegen aan uw project](app-insights-asp-net.md)**  als u die nog niet hebt gedaan. Hier ziet u een optie om op te nemen van de logboekverzamelaar.

Of **Configure Application Insights** met de rechtermuisknop op het project in Solution Explorer. Selecteer de optie voor **trace verzamelen configureren**.

*Er zijn geen menu- of logboekbestand collector de optie Application Insights?* Probeer [probleemoplossing](#troubleshooting).

## <a name="manual-installation"></a>Handmatige installatie
Gebruik deze methode als uw projecttype wordt niet ondersteund door de Application Insights-installatieprogramma (bijvoorbeeld een Windows desktop-project).

1. Als u van plan bent om log4Net of NLog te gebruiken, kunt u deze in uw project installeren.
2. Klik in Solution Explorer met de rechtermuisknop op uw project en kies **NuGet-pakketten beheren**.
3. Naar Application Insights zoeken
4. Selecteer het juiste pakket - een van:

   * Microsoft.ApplicationInsights.TraceListener (om vast te leggen System.Diagnostics.Trace aanroepen)
   * Microsoft.ApplicationInsights.EventSourceListener (vast te leggen EventSource gebeurtenissen)
   * Microsoft.ApplicationInsights.EtwListener (om vast te leggen ETW-gebeurtenissen)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

Het NuGet-pakket installeert de benodigde assembly's en past tevens web.config of app.config.

## <a name="insert-diagnostic-log-calls"></a>Diagnostische logboeken aanroepen invoegen
Als u System.Diagnostics.Trace gebruikt, wordt een aanroep van typische zou zijn:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Als u liever log4net of NLog:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>Het gebruik van EventSource gebeurtenissen
U kunt configureren [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen naar Application Insights als traceringen worden verzonden. Installeer eerst de `Microsoft.ApplicationInsights.EventSourceListener` NuGet-pakket. Bewerk vervolgens `TelemetryModules` sectie van de [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) bestand.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Voor elke bron, kunt u de volgende parameters instellen:
 * `Name`Hiermee geeft u de naam van de EventSource te verzamelen.
 * `Level`Hiermee geeft u het logboekregistratieniveau te verzamelen. Een van `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Optioneel) Hiermee geeft u de gehele waarde van combinaties van trefwoorden te gebruiken.

## <a name="using-diagnosticsource-events"></a>Het gebruik van DiagnosticSource gebeurtenissen
U kunt configureren [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) gebeurtenissen naar Application Insights als traceringen worden verzonden. Installeer eerst de [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-pakket. Bewerk vervolgens de `TelemetryModules` sectie van de [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) bestand.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Voor elke DiagnosticSource die u traceren wilt, Voeg een vermelding met de `Name` -kenmerk ingesteld op de naam van uw DiagnosticSource.

## <a name="using-etw-events"></a>Met behulp van ETW-gebeurtenissen
U kunt configureren ETW-gebeurtenissen naar Application Insights als traceringen worden verzonden. Installeer eerst de `Microsoft.ApplicationInsights.EtwCollector` NuGet-pakket. Bewerk vervolgens `TelemetryModules` sectie van de [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) bestand.

> [!NOTE] 
> ETW-gebeurtenissen kunnen alleen worden verzameld als het proces voor het hosten van de SDK wordt uitgevoerd onder een identiteit die lid is van 'Gebruikers prestatielogboek' of de groep Administrators.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Voor elke bron, kunt u de volgende parameters instellen:
 * `ProviderName`is de naam van de ETW-provider te verzamelen.
 * `ProviderGuid`Geeft de GUID van de ETW-provider moet worden verzameld, kunnen worden gebruikt in plaats van `ProviderName`.
 * `Level`Hiermee stelt u het logboekregistratieniveau te verzamelen. Een van `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Optioneel) stelt de integerwaarde van combinaties van trefwoorden te gebruiken.

## <a name="using-the-trace-api-directly"></a>De tracering API direct gebruik te maken
U kunt rechtstreeks de Application Insights trace API aanroepen. De adapters logboekregistratie gebruiken deze API.

Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

U kunt relatief lange gegevens plaatsen in het bericht heeft als voordeel van TrackTrace. U kan bijvoorbeeld er postgegevens coderen.

U kunt bovendien een urgentieniveau toevoegen aan het bericht. En net als andere telemetrie u eigenschapswaarden die u gebruiken kunt om te filteren of zoeken naar verschillende sets van traceringen kunt toevoegen. Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Hiermee kunt u, zou [zoeken][diagnostic], gemakkelijk kunt uitfilteren alle berichten van een bepaalde ernstniveau met betrekking tot een bepaalde database.

## <a name="explore-your-logs"></a>Verken uw logboeken
Uitvoeren van uw app ofwel in de foutopsporingsmodus of live implementeren.

In de overzichtsblade van uw app in [de Application Insights-portal][portal], kies [Search][diagnostic].

![Kies in de Application Insights zoeken](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Search](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

U kunt, bijvoorbeeld:

* Filteren op logboektraceringen of op items met specifieke eigenschappen
* Een specifiek item in detail te controleren.
* Andere telemetrie met betrekking tot de dezelfde gebruikersaanvraag vinden (dat wil zeggen, met de dezelfde OperationId)
* De configuratie van deze pagina als favoriet opslaan

> [!NOTE]
> **Een steekproef.** Als uw toepassing grote hoeveelheden gegevens verzendt en u de Application Insights-SDK voor ASP.NET-versie 2.0.0-beta3 of hoger gebruikt, stuurt de functie voor adaptieve steekproeven mogelijk slechts een percentage van uw telemetrie. [Meer informatie over steekproeven.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Volgende stappen
[Onderzoeken fouten en uitzonderingen in ASP.NET][exceptions]

[Meer informatie over zoekopdracht][diagnostic].

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="how-do-i-do-this-for-java"></a>Hoe kan ik deze voor Java?
Gebruik de [Java logboek adapters](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Er is geen optie Application Insights in het contextmenu van project
* Controle van Application Insights-hulpprogramma's zijn geïnstalleerd op deze machine ontwikkeling. In Visual Studio-menu Extra Zoek uitbreidingen en Updates, naar Application Insights Tools. Als deze niet in het tabblad geïnstalleerd, opent u het tabblad Online en installeer deze.
* Dit wordt mogelijk een type project niet wordt ondersteund door de Application Insights-hulpprogramma's. Gebruik [handmatige installatie](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Er is geen optie logboek adapter in het configuratiehulpprogramma
* U moet eerst installeren van het framework voor logboekregistratie.
* Als u van System.Diagnostics.Trace gebruikmaakt, controleert u of u [geconfigureerd in `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Hebt u de nieuwste versie van Application Insights hebt? In Visual Studio **extra** menu kiezen **uitbreidingen en Updates**, en open de **Updates** tabblad. Als u hulpprogramma's voor ontwikkelaars webanalyse aanwezig is, klikt u op om bij te werken.

### <a name="emptykey"></a>Een fout ophalen 'instrumentatiesleutel mag niet leeg zijn'
Lijkt erop dat u de logboekregistratie adapter Nuget-pakket geïnstalleerd zonder Application Insights te installeren.

Klik in Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies **Update Application Insights**. U krijgt een dialoogvenster met een uitnodiging aan te melden bij Azure en maak een Application Insights-resource opnieuw of gebruik een bestaande. Die het probleem moet oplossen.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Zie ik traceringen in diagnostische gegevens doorzoeken, maar niet de andere gebeurtenissen
Het kan duren voor de gebeurtenissen en aanvragen voor ophalen via de pipeline.

### <a name="limits"></a>Hoeveel gegevens behouden blijven?
Verschillende factoren van invloed op de hoeveelheid gegevens behouden. Zie de [limieten](app-insights-api-custom-events-metrics.md#limits) sectie van de pagina klant gebeurtenis metrische gegevens voor meer informatie. 

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
