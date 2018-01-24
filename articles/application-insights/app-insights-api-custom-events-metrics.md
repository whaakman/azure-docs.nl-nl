---
title: Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens | Microsoft Docs
description: Plaats een paar regels code in uw apparaat of een bureaublad-app, webpagina of service, gebruik bijhouden en analyseren van problemen.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: mbullwin
ms.openlocfilehash: 7d797716fb98ac85f11f956e732e08820b56affc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens

Voeg een paar regels code in uw toepassing om erachter te komen wat gebruikers doen met het of om u te helpen bij het analyseren van problemen. U kunt telemetrie verzenden van apparaat- en bureaublad-apps, webserver en webclients en webservers. Gebruik de [Azure Application Insights](app-insights-overview.md) telemetrie-API voor het verzenden van aangepaste gebeurtenissen en metrische gegevens en uw eigen versies standaardtelemetrie core. Deze API is de dezelfde API die gebruikmaken van de standaard Application Insights gegevens collectors.

## <a name="api-summary"></a>API-overzicht
De API is uniform op alle platforms, naast een paar kleine verschillen.

| Methode | Gebruikt voor |
| --- | --- |
| [`TrackPageView`](#page-views) |Pagina's, schermen, blades of formulieren. |
| [`TrackEvent`](#trackevent) |Acties van gebruikers en andere gebeurtenissen. Gebruikt voor het gedrag van de gebruiker bijhouden of om prestaties te bewaken. |
| [`TrackMetric`](#trackmetric) |Prestatiemetingen zoals wachtrij lengten niet gerelateerd aan specifieke gebeurtenissen. |
| [`TrackException`](#trackexception) |Logboekregistratie uitzonderingen voor diagnose. Traceren waarbij ze ten opzichte van andere gebeurtenissen optreden en stack-traces onderzoeken. |
| [`TrackRequest`](#trackrequest) |De frequentie en duur van serveraanvragen voor prestatieanalyse logboekregistratie. |
| [`TrackTrace`](#tracktrace) |Diagnostische logboeken-berichten. U kunt ook de logboeken van derden vastleggen. |
| [`TrackDependency`](#trackdependency) |De duur en frequentie van aanroepen naar externe onderdelen die uw app afhankelijk van is logboekregistratie. |

U kunt [eigenschappen en metrische gegevens koppelen](#properties) tot de meeste van deze aanroepen telemetrie.

## <a name="prep"></a>Voordat u begint
Als u nog een verwijzing op Application Insights-SDK hebt:

* De Application Insights-SDK toevoegen aan uw project:

  * [ASP.NET project](app-insights-asp-net.md)
  * [Java-project](app-insights-java-get-started.md)
  * [Node.js-project](app-insights-nodejs.md)
  * [JavaScript in elke webpagina](app-insights-javascript.md) 
* In uw servercode apparaat of een webtoepassing omvatten:

    *C#:* `using Microsoft.ApplicationInsights;`

    *Visual Basic:*`Imports Microsoft.ApplicationInsights`

    *Java:*`import com.microsoft.applicationinsights.TelemetryClient;`
    
    *Node.js:* `var applicationInsights = require("applicationinsights");`

## <a name="get-a-telemetryclient-instance"></a>Een instantie TelemetryClient ophalen
Geen instantie ophalen van `TelemetryClient` (behalve in JavaScript in webpagina's):

*C#*

    private TelemetryClient telemetry = new TelemetryClient();

*Visual Basic*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();
    
*Node.js*

    var telemetry = applicationInsights.defaultClient;


TelemetryClient is thread-safe.

U wordt aangeraden dat u een exemplaar van TelemetryClient voor elke module van uw app maakt voor ASP.NET en Java-projecten. Bijvoorbeeld: mogelijk hebt u één exemplaar van TelemetryClient in uw webservice voor het rapporteren van binnenkomende HTTP-aanvragen en andere in een klasse middleware rapport zakelijke logica gebeurtenissen. U kunt eigenschappen instellen, zoals `TelemetryClient.Context.User.Id` voor het bijhouden van gebruikers en sessies, of `TelemetryClient.Context.Device.Id` om de machine te bepalen. Deze informatie is gekoppeld aan alle gebeurtenissen die door het exemplaar wordt verzonden.

In een Node.js-projecten, kunt u `new applicationInsights.TelemetryClient(instrumentationKey?)` voor het maken van een nieuw exemplaar, maar dit wordt alleen aanbevolen voor scenario's waarvoor geïsoleerde configuratie van de singleton `defaultClient`.

## <a name="trackevent"></a>TrackEvent
In Application Insights, een *aangepaste gebeurtenis* is van een gegevenspunt geldt dat u kunt weergeven in [Metrics Explorer](app-insights-metrics-explorer.md) als een verzamelde aantal en in [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md) als afzonderlijke exemplaren. (Deze wordt niet verwant zijn aan de MVC- of andere framework 'gebeurtenissen'.)

Invoegen `TrackEvent` aanroepen in uw code te tellen van diverse gebeurtenissen. Hoe vaak gebruikers kiest voor een bepaalde functie, hoe vaak ze bepaalde doelen bereiken of zij mogelijk hoe vaak voor bepaalde soorten fouten.

Bijvoorbeeld in een gameapps een gebeurtenis wanneer een gebruiker het spel wins verzenden:

*JavaScript*

    appInsights.trackEvent("WinGame");

*C#*

    telemetry.TrackEvent("WinGame");

*Visual Basic*

    telemetry.TrackEvent("WinGame")

*Java*

    telemetry.trackEvent("WinGame");
    
*Node.js*

    telemetry.trackEvent({name: "WinGame"});

### <a name="view-your-events-in-the-microsoft-azure-portal"></a>Gebeurtenissen weergeven in de Microsoft Azure-portal
Open een aantal van uw gebeurtenissen vindt een [Metrics Explorer](app-insights-metrics-explorer.md) blade, Voeg een nieuwe grafiek toe en selecteer **gebeurtenissen**.  

![Zie een aantal aangepaste gebeurtenissen](./media/app-insights-api-custom-events-metrics/01-custom.png)

Als u wilt vergelijken de aantallen voor verschillende gebeurtenissen, stelt u het grafiektype naar **raster**, and -groep met de gebeurtenisnaam:

![Het grafiektype en groepering instellen](./media/app-insights-api-custom-events-metrics/07-grid.png)

Klik op het raster door de naam van een gebeurtenis om te zien van afzonderlijke exemplaren van deze gebeurtenis. Voor meer details - klikt u op elk exemplaar in de lijst.

![Drillthrough-gebeurtenissen](./media/app-insights-api-custom-events-metrics/03-instances.png)

Als u wilt richten op specifieke gebeurtenissen in de zoekopdracht of Metrics Explorer, stelt u de blade-filter op de gebeurtenisnamen van de die u geïnteresseerd bent in:

![Open Filters, vouw de naam van de gebeurtenis en selecteer een of meer waarden](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a>Aangepaste gebeurtenissen in Analytics

De telemetrie is beschikbaar in de `customEvents` in tabel [Application Insights Analytics](app-insights-analytics.md). Elke rij vertegenwoordigt een aanroep van `trackEvent(..)` in uw app. 

Als [steekproeven](app-insights-sampling.md) is uitgevoerd, de eigenschap itemCount geeft een waarde groter dan 1. Voor een voorbeeld itemCount == 10 betekent dat van 10 aanroepen van trackEvent(), het proces steekproeven alleen één van beide verzonden. Als u het juiste aantal aangepaste gebeurtenissen, moet u daarom code gebruiken zoals `customEvent | summarize sum(itemCount)`.


## <a name="trackmetric"></a>TrackMetric

Application Insights kunnen metrische gegevens die niet zijn gekoppeld aan bepaalde gebeurtenissen van grafiekgebied. U kan bijvoorbeeld een wachtrijlengte bewaken met regelmatige tussenpozen. Met metrische gegevens en de afzonderlijke metingen zijn van belang zijn kleiner dan de variaties en trends en dus statistische kolomdiagrammen zijn nuttig.

Om metrische gegevens naar Application Insights verzendt, kunt u de `TrackMetric(..)` API. Er zijn twee manieren een metriek verzenden: 

* Enkele waarde. Telkens wanneer u een meting in uw toepassing uitvoert, kunt u de overeenkomstige waarde verzenden naar Application Insights. Bijvoorbeeld, wordt ervan uitgegaan dat u hebt een metriek met een beschrijving van het aantal items in een container. U voor het eerst drie items in de container plaatsen en verwijder vervolgens twee items gedurende een bepaalde periode. Dienovereenkomstig, roept u `TrackMetric` tweemaal: de waarde eerst doorgeven `3` en vervolgens de waarde `-2`. Application Insights worden beide waarden opgeslagen namens jou. 

* Aggregatie. Als u werkt met metrische gegevens, wordt elke meting zelden is van belang. In plaats daarvan is een overzicht van wat er gebeurd gedurende een bepaalde periode is belangrijk. Dergelijke samenvatting heet _aggregatie_. In het bovenstaande voorbeeld wordt de som van het cumulatieve metrische gegevens voor deze periode is `1` en het aantal van de metrische waarden is `2`. Wanneer u de aggregatie-methode gebruikt, u alleen aanroepen `TrackMetric` eenmaal per periode en de verzamelde waarden te verzenden. Dit is de aanbevolen aanpak omdat deze de kosten en prestaties overhead door minder gegevenspunten verzenden naar Application Insights tijdens het verzamelen van alle relevante gegevens nog steeds aanzienlijk kan verminderen.

### <a name="examples"></a>Voorbeelden:

#### <a name="single-values"></a>Enkele waarden

Metrische waarde voor een enkele verzenden:

*JavaScript*

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

*C#, Java*

```csharp
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

*Node.js*

 ```Javascript
     telemetry.trackMetric({name: "queueLength", value: 42.0});
 ```

#### <a name="aggregating-metrics"></a>Samenvoegen van metrische gegevens

Het is raadzaam met cumulatieve metrische gegevens voordat ze worden verzonden vanuit uw app of als u bandbreedte, kosten en prestaties te verbeteren.
Hier volgt een voorbeeld van code verzamelen:

*C#*

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends the aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of the aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap the current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute the actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct the metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a>Aangepaste metrische gegevens in Metrics Explorer

U kunt de resultaten, open Metrics Explorer en voeg een nieuwe grafiek toe. De grafiek om weer te geven de metrische gegevens bewerken.

> [!NOTE]
> Uw aangepaste metrische gegevens kan enkele minuten worden weergegeven in de lijst met beschikbare metrische gegevens duren.
>

![Voeg een nieuwe grafiek toe of Selecteer een grafiek en onder aangepast, selecteer uw metrische gegevens](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a>Aangepaste metrische gegevens in Analytics

De telemetrie is beschikbaar in de `customMetrics` in tabel [Application Insights Analytics](app-insights-analytics.md). Elke rij vertegenwoordigt een aanroep van `trackMetric(..)` in uw app.
* `valueSum`-Dit is de som van de metingen. Als u de gemiddelde waarde, wordt gedeeld door `valueCount`.
* `valueCount`-Het aantal metingen die zijn samengevoegd in dit `trackMetric(..)` aanroepen.

## <a name="page-views"></a>Paginaweergaven
In een app-apparaat of webpagina telemetrie van paginaweergaven standaard verzonden wanneer elke pagina of het scherm wordt geladen. Maar u kunt deze instelling wijzigen om bij te houden van paginaweergaven op aanvullende of verschillende tijdstippen. Bijvoorbeeld in een app die wordt weergegeven voor tabbladen of blades, raadzaam om bij te houden van een pagina wanneer de gebruiker een nieuwe blade opent.

![Gebruik lens op de blade overzicht](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Gebruikers- en sessiegegevens gegevens verzonden als eigenschappen samen met paginaweergaven, zodat de gebruikers- en sessiegegevens grafieken tot leven komen als er telemetrie van paginaweergaven.

### <a name="custom-page-views"></a>Aangepaste paginaweergaven
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*Visual Basic*

    telemetry.TrackPageView("GameReviewPage")


Als u meerdere tabbladen binnen andere HTML-pagina's hebt, kunt u de URL te opgeven:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a>Timing van paginaweergaven
Standaard worden de tijden gerapporteerd als **laadtijd voor paginaweergave** worden gemeten vanaf de browser de aanvraag verzendt totdat de browser-gebeurtenis voor het laden van pagina wordt aangeroepen.

In plaats daarvan kunt u:

* Stel een expliciete duur de [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) aanroepen: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Gebruik de paginaweergave timing aanroepen `startTrackPage` en `stopTrackPage`.

*JavaScript*

    // To start timing a page:
    appInsights.startTrackPage("Page1");

...

    // To stop timing and log the page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

De naam die u als de eerste parameter gebruikt wordt gekoppeld aan de aanroepen starten en stoppen. Wordt standaard de naam van de huidige pagina.

De resulterende pagina load duur weergegeven in Metrics Explorer zijn afgeleid van het interval tussen de aanroepen starten en stoppen. Het is aan u welke interval die u daadwerkelijk tijd.

### <a name="page-telemetry-in-analytics"></a>Pagina telemetrie in Analytics

In [Analytics](app-insights-analytics.md) twee tabellen bevatten gegevens van de browser bewerkingen:

* De `pageViews` tabel bevat gegevens over de URL- en -titel
* De `browserTimings` tabel bevat gegevens over de prestaties van de client, zoals de tijd voor de inkomende gegevens

Zoeken op hoe lang de browser nodig is om verschillende pagina's:

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

Voor detectie van de popularities van verschillende browsers:

```
pageViews | summarize count() by client_Browser
```

Om te koppelen van paginaweergaven voor AJAX-aanroepen, aanmelden met afhankelijkheden:

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest
De SDK-server gebruikt TrackRequest HTTP-aanvragen moeten worden geregistreerd.

U kunt deze zelf ook aanroepen als u verzoeken wilt simuleren in een context waarin er geen module van de web-service uitgevoerd.

De aanbevolen manier om het verzenden van aanvraagtelemetrie is echter waar de aanvraag fungeert als een <a href="#operation-context">bewerking context</a>.

## <a name="operation-context"></a>Bewerking context
U kunt telemetrie-items bij elkaar correleren door ze te koppelen met bewerking context. De standaard-bijhouden module doet dit voor uitzonderingen en andere gebeurtenissen die worden verzonden tijdens het verwerken van een HTTP-aanvraag. In [Search](app-insights-diagnostic-search.md) en [Analytics](app-insights-analytics.md), kunt u gemakkelijk alle gebeurtenissen die zijn gekoppeld aan de aanvraag met de bewerking-id vinden

Zie [correlatie van telemetrie in Application Insights](application-insights-correlation.md) voor meer informatie over de correlatie.

Bij het volgen van telemetrie handmatig de eenvoudigste manier om ervoor te zorgen telemetrie correlatie met behulp van dit patroon:

*C#*

```csharp
// Establish an operation context and associated telemetry item:
using (var operation = telemetryClient.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use the same operation ID.
    ...
    telemetryClient.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetryClient.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

Samen met het instellen van de context van een bewerking `StartOperation` maakt een telemetrie-item van het type dat u opgeeft. Het item telemetrie verzendt wanneer het verwijderen van de bewerking, of als u niet expliciet aanroepen `StopOperation`. Als u `RequestTelemetry` als de telemetrie-type, de duur is ingesteld op het getimed interval tussen starten en stoppen.

Telemetrie-items die zijn gerapporteerd binnen een bereik van de bewerking worden 'kinderen' van deze bewerking. Bewerking contexten kunnen worden genest. 

In de zoekopdracht op de context van de bewerking gebruikt voor het maken de **verwante Items** lijst:

![Verwante items](./media/app-insights-api-custom-events-metrics/21.png)

Zie [bijhouden van aangepaste bewerkingen met Application Insights-SDK voor .NET](application-insights-custom-operations-tracking.md) voor meer informatie over aangepaste bewerkingen bijhouden.

### <a name="requests-in-analytics"></a>Aanvragen in Analytics 

In [Application Insights Analytics](app-insights-analytics.md), weergeven van aanvragen in de `requests` tabel.

Als [steekproeven](app-insights-sampling.md) is uitgevoerd, de eigenschap itemCount ziet een waarde groter dan 1. Voor een voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar trackRequest() het proces steekproeven alleen één van beide verzonden. Als u een juiste aantal aanvragen en gemiddelde duur gesegmenteerd op aanvraag namen, zoals code gebruiken:

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a>TrackException
Uitzonderingen naar Application Insights verzenden:

* Naar [tellen](app-insights-metrics-explorer.md), als een indicatie van de frequentie van een probleem.
* Naar [onderzoeken van afzonderlijke exemplaren](app-insights-diagnostic-search.md).

De rapporten bevatten de stack-traces.

*C#*

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

*JavaScript*

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }
    
*Node.js*

    try
    {
       ...
    }
    catch (ex)
    {
       telemetry.trackException({exception: ex});
    }

De SDK's catch veel uitzonderingen automatisch, dus u niet altijd hebt TrackException expliciet aanroepen.

* ASP.NET: [schrijven van code voor het onderscheppen](app-insights-asp-net-exceptions.md).
* J2EE: [uitzonderingen worden aangetroffen, automatisch](app-insights-java-get-started.md#exceptions-and-request-failures).
* JavaScript: Uitzonderingen worden automatisch aangetroffen. Als u uitschakelen van automatische verzameling wilt, moet u een regel toegevoegd aan het codefragment die u in uw webpagina's invoegt:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a>Uitzonderingen in Analytics

In [Application Insights Analytics](app-insights-analytics.md), uitzonderingen weergegeven in de `exceptions` tabel.

Als [steekproeven](app-insights-sampling.md) is uitgevoerd, de `itemCount` eigenschap geeft een waarde groter dan 1. Voor een voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar trackException() het proces steekproeven alleen één van beide verzonden. Als u een juiste aantal uitzonderingen gesegmenteerd op type uitzondering, zoals code gebruiken:

```
exceptions | summarize sum(itemCount) by type
```

De meeste informatie belangrijk stack al wordt geëxtraheerd in verschillende variabelen, maar u kunt ophalen uit elkaar de `details` structuur om meer te halen. Aangezien deze structuur dynamisch is, moet u het resultaat dat het verwachte type converteren. Bijvoorbeeld:

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

Om te koppelen uitzonderingen met hun verwante aanvragen, gebruikt u een join:

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a>TrackTrace
Gebruik TrackTrace om u te helpen bij het analyseren van problemen door een spoor' breadcrumb' te sturen naar Application Insights. U kunt segmenten van diagnostische gegevens verzenden en controleren ze op in [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md).

[Meld u adapters](app-insights-asp-net-trace-logs.md) deze API gebruikt van derden om Logboeken te verzenden naar de portal.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);
    
*Node.js*

    telemetry.trackTrace({message: message, severity:applicationInsights.Contracts.SeverityLevel.Warning, properties:properties});


U kunt zoeken op de inhoud van het bericht, maar (in tegenstelling tot eigenschapswaarden) u kunt niet filteren op het.

De maximale grootte op `message` veel hoger is dan de limiet voor eigenschappen.
U kunt relatief lange gegevens plaatsen in het bericht heeft als voordeel van TrackTrace. U kunt bijvoorbeeld er postgegevens coderen.  

U kunt bovendien een urgentieniveau toevoegen aan het bericht. En net als andere telemetrie u eigenschapswaarden om u te helpen filter of zoeken naar verschillende sets van traceringen kunt toevoegen. Bijvoorbeeld:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

In [Search](app-insights-diagnostic-search.md), u kunt eenvoudig filteren vervolgens alle berichten van een specifieke ernst die betrekking op een bepaalde database hebben.


### <a name="traces-in-analytics"></a>Traceringen in Analytics

In [Application Insights Analytics](app-insights-analytics.md), aanroepen naar TrackTrace weergegeven in de `traces` tabel.

Als [steekproeven](app-insights-sampling.md) is uitgevoerd, de eigenschap itemCount geeft een waarde groter dan 1. Voor een voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar `trackTrace()`, een van deze alleen in het proces steekproeven worden verzonden. Als u het juiste aantal traceringsaanroepen, moet u daarom code zoals `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency
Gebruik de TrackDependency-aanroep voor het bijhouden van de responstijden en het succespercentage van aanroepen naar externe code. De resultaten worden weergegeven in de afhankelijkheidsgrafiek in de portal.

```csharp
var success = false;
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
try
{
    success = dependency.Call();
}
finally
{
    timer.Stop();
    telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
}
```

```Javascript
var success = false;
var startTime = new Date().getTime();
try
{
    success = dependency.Call();
}
finally
{
    var elapsed = new Date() - startTime;
    telemetry.trackDependency({dependencyTypeName: "myDependency", name: "myCall", duration: elapsed, success:success});
}
```

Houd er rekening mee dat de server SDK's bevatten een [afhankelijkheid module](app-insights-asp-net-dependencies.md) die worden gedetecteerd en bijgehouden bepaalde afhankelijkheidsaanroepen automatisch--bijvoorbeeld databases en REST-API's. U moet een agent installeren op uw server om te maken van de module werken. U kunt deze aanroep gebruiken als u wilt bijhouden van de oproepen die de geautomatiseerde tracering niet catch- of als u niet wilt dat de agent te installeren.

Schakel uit de standaard afhankelijkheid bijhouden module bewerken [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) en verwijder de verwijzing naar `DependencyCollector.DependencyTrackingTelemetryModule`.

### <a name="dependencies-in-analytics"></a>Afhankelijkheden in Analytics

In [Application Insights Analytics](app-insights-analytics.md), trackDependency belt weergeven de `dependencies` tabel.

Als [steekproeven](app-insights-sampling.md) is uitgevoerd, de eigenschap itemCount geeft een waarde groter dan 1. Voor een voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar trackDependency() het proces steekproeven alleen één van beide verzonden. Als u een juiste aantal gesegmenteerd op doelonderdeel afhankelijkheden, zoals code gebruiken:

```
dependencies | summarize sum(itemCount) by target
```

Om te koppelen afhankelijkheden met hun verwante aanvragen, gebruikt u een join:

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a>Gegevens leegmaken
Normaal gesproken verzendt de SDK op tijdstippen wilt minimaliseren, de gevolgen voor de gebruiker. Echter, in sommige gevallen u mogelijk wilt leegmaken van de buffer--bijvoorbeeld als u de SDK gebruikt in een toepassing die wordt afgesloten.

*C#*

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);
    
*Node.js*

    telemetry.flush();

Houd er rekening mee dat de functie asynchroon is voor de [telemetrie-serverkanaal](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

## <a name="authenticated-users"></a>Geverifieerde gebruikers
In een web-app, worden gebruikers (standaard) geïdentificeerd door cookies. Een gebruiker mogelijk meer dan één keer worden geteld als ze toegang krijgen tot uw app uit een andere computer of de browser, of als het verwijderen van cookies.

Als gebruikers zich bij uw app aanmelden, kunt u een meer nauwkeurige telling krijgen door de geverifieerde gebruiker-ID in te stellen in de browser-code:

*JavaScript*

```JS
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

In een ASP.NET-webtoepassing MVC-toepassing, bijvoorbeeld:

*Razor*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Het is niet nodig om werkelijke van de gebruiker aanmelden namen te gebruiken. Alleen er moet een ID die uniek is voor die gebruiker. Mag geen spaties of een van de tekens bevatten `,;=|`.

De gebruikers-ID is ook in een sessiecookie ingesteld en verzonden naar de server. Als de SDK-server is geïnstalleerd, wordt de geverifieerde gebruiker-ID verzonden als onderdeel van de contexteigenschappen van zowel client als server telemetrie. Vervolgens kunt u filteren en zoekt u erop.

Als uw app gebruikers naar accounts groepen, kunt u ook een id voor het account (met de tekenbeperkingen voor dezelfde) doorgeven.

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

In [Metrics Explorer](app-insights-metrics-explorer.md), kunt u een diagram waarin telt **geverifieerde gebruikers,**, en **gebruikersaccounts**.

U kunt ook [search](app-insights-diagnostic-search.md) voor client gegevenspunten met specifieke gebruikersnamen en accounts.

## <a name="properties"></a>Filteren, zoeken en segmenteren van uw gegevens met behulp van eigenschappen
U kunt eigenschappen en metingen koppelen aan uw gebeurtenissen (en ook met metrische gegevens en pagina weergaven, uitzonderingen en andere telemetriegegevens).

*Eigenschappen* tekenreekswaarden die u gebruiken kunt voor het filteren van uw telemetrie in de rapporten zijn. Bijvoorbeeld, als uw app verschillende games biedt, kunt u de naam van het spel aan koppelen elke gebeurtenis zodat u kunt zien welke games populairder zijn.

Er is een limiet van 8192 op de string-lengte. (Als u grote reeksen gegevens verzenden wilt, gebruikt u de bericht-parameter van [TrackTrace](#track-trace).)

*Metrische gegevens* zijn numerieke waarden die kunnen worden weergegeven als geïllustreerd. U wilt zien of er een geleidelijke toename van de scores die uw gamers bereiken. De grafieken kunnen worden gesegmenteerd op de eigenschappen die worden verzonden met de gebeurtenis, zodat u kunt afzonderlijke ophalen of gestapeld grafieken voor verschillende games.

Ze moeten groter zijn dan of gelijk zijn aan 0 zijn voor metrische waarden moet correct worden weergegeven.

Er zijn een aantal [beperkingen met betrekking tot het aantal eigenschappen en eigenschapswaarden metrische gegevens](#limits) die u kunt gebruiken.

*JavaScript*

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


*C#*

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics);

*Node.js*

    // Set up some properties and metrics:
    var properties = {"game": currentGame.Name, "difficulty": currentGame.Difficulty};
    var metrics = {"Score": currentGame.Score, "Opponents": currentGame.OpponentCount};

    // Send the event:
    telemetry.trackEvent({name: "WinGame", properties: properties, measurements: metrics});


*Visual Basic*

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics)


*Java*

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> Zorgt niet voor logboekregistratie van persoonsgegevens in de eigenschappen.
>
>

*Als u metrische gegevens gebruikt*, Metrics Explorer openen en selecteer de metrische gegevens van de **aangepaste** groep:

![Openen van Metrics Explorer, selecteert u de grafiek en selecteer de metriek](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> Als uw metriek niet wordt weergegeven, of als de **aangepaste** kop niet het geval is, sluit de blade selectie en probeer het later opnieuw. Metrische gegevens kunt soms uur duren voordat de een aggregatie worden uitgevoerd via de pipeline.

*Als u de eigenschappen en metrische gegevens gebruikt*, de metriek segmenteren door de eigenschap:

![Stel groeperen en selecteer vervolgens de eigenschap onder groeperen op](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

*In Diagnostic Search*, kunt u de eigenschappen en metrische gegevens van afzonderlijke instanties van een gebeurtenis bekijken.

![Selecteer een exemplaar en selecteer vervolgens '...'](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Gebruik de **Search** veld voor een overzicht van de gebeurtenis-instanties die een bepaalde eigenschappenwaarde hebben.

![Typ een term in zoeken](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

[Meer informatie over zoekopdracht expressies](app-insights-diagnostic-search.md).

### <a name="alternative-way-to-set-properties-and-metrics"></a>Eigenschappen en metrische gegevens ook instellen
Als het is eenvoudiger, kunt u de parameters van een gebeurtenis in een afzonderlijk object verzamelen:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> De hetzelfde exemplaar van de telemetrie-item niet hergebruiken (`event` in dit voorbeeld) Track*() meerdere keren aanroepen. Dit kan leiden tot telemetrie met onjuiste configuratie worden verzonden.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Aangepaste metingen en eigenschappen in Analytics

In [Analytics](app-insights-analytics.md), eigenschappen en aangepaste metrische gegevens weergeven in de `customMeasurements` en `customDimensions` kenmerken van elke record telemetrie.

Bijvoorbeeld, als u een eigenschap genaamd 'spel' de aanvraagtelemetrie van uw hebt toegevoegd, deze query telt instanties van verschillende waarden van 'spel' en het gemiddelde van de aangepaste metrische 'score' weergeven:

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

U ziet dat:

* Wanneer u een waarde van de customDimensions of customMeasurements JSON uitpakt, hieraan dynamische type, en zodat u dit casten `tostring` of `todouble`.
* Rekening te houden met de mogelijkheid van [steekproeven](app-insights-sampling.md), moet u `sum(itemCount)`, niet `count()`.



## <a name="timed"></a>Timing van gebeurtenissen
Soms wilt u grafiek hoe lang het duurt om een actie uitvoert. Bijvoorbeeld, u wilt mogelijk weten hoe lang gebruikers nemen op in een spel te overwegen. Hiervoor kunt u de parameter meting.

*C#*

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform the timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send the event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <a name="defaults"></a>Standaard-eigenschappen voor aangepaste telemetrie
Als u standaard eigenschapswaarden instellen voor een aantal aangepaste gebeurtenissen die u schrijft wilt, kunt u ze in een exemplaar van TelemetryClient instellen. Deze zijn gekoppeld aan elk telemetrie-item dat wordt verzonden door de client.

*C#*

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame");

*Visual Basic*

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame")

*Java*

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");
    
*Node.js*

    var gameTelemetry = new applicationInsights.TelemetryClient();
    gameTelemetry.commonProperties["Game"] = currentGame.Name;

    gameTelemetry.TrackEvent({name: "WinGame"});



Afzonderlijke telemetrie aanroepen kunnen u de standaardwaarden in hun woordenlijsten eigenschap overschrijven.

*Web voor JavaScript-clients*, [gebruik van JavaScript telemetrie initalisatiefuncties](#js-initializer).

*Eigenschappen toevoegen aan alle telemetrie*, met inbegrip van de gegevens van de standaardregel voor verzameling modules, [implementeren `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Steekproef nemen en verwerken van telemetrie filteren
U kunt de code voor het verwerken van uw telemetrie voordat deze wordt verzonden van de SDK schrijven. De verwerking omvat gegevens dat wordt verzonden door de standaard telemetrie-modules, zoals de verzameling van de HTTP-aanvragen en afhankelijkheid verzameling.

[Eigenschappen toevoegen](app-insights-api-filtering-sampling.md#add-properties) naar telemetrie door het implementeren van `ITelemetryInitializer`. U kunt bijvoorbeeld versienummers of berekende waarden toevoegen van andere eigenschappen.

[Filteren](app-insights-api-filtering-sampling.md#filtering) kunt wijzigen of verwijderen van telemetrie voordat het wordt verzonden door de SDK door het implementeren van `ITelemetryProcesor`. U bepaalt wat wordt verzonden naar of verwijderd, maar u moet voor het effect op de metrische gegevens over uw account. Afhankelijk van hoe u items negeren, kunt u de mogelijkheid om te navigeren tussen verwante items verliezen.

[Steekproef nemen](app-insights-api-filtering-sampling.md) is een ingepakte oplossing te verminderen de hoeveelheid gegevens die door uw app wordt verzonden naar de portal. Hierbij wordt zonder de weergegeven metrische gegevens. En zonder de mogelijkheid om te analyseren van problemen door te navigeren tussen verwante items zoals uitzonderingen, aanvragen en paginaweergaven beïnvloeden.

[Meer informatie](app-insights-api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Telemetrie uitschakelen
Naar *dynamisch stoppen en starten* het verzamelen en verzenden van telemetrie:

*C#*

```csharp

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

Naar *uitschakelen geselecteerde standaard collectors*--bijvoorbeeld, prestatiemeteritems, HTTP-aanvragen of afhankelijkheden--verwijderen of de relevante regels in uitcommentariëren [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). U kunt dit bijvoorbeeld doen als u wilt de gegevens van uw eigen TrackRequest verzenden.

*Node.js*

```Javascript

    telemetry.config.disableAppInsights = true;
```

Naar *uitschakelen geselecteerde standaard collectors*--bijvoorbeeld keten configuratiemethoden toe aan uw code van de initialisatie van de SDK van prestatiemeteritems, HTTP-aanvragen of afhankelijkheden--bij het initialiseren:

```Javascript

    applicationInsights.setup()
        .setAutoCollectRequests(false)
        .setAutoCollectPerformance(false)
        .setAutoCollectExceptions(false)
        .setAutoCollectDependencies(false)
        .setAutoCollectConsole(false)
        .start();
```

Schakel deze collectors na de initialisatie van de Configuration-object te gebruiken:`applicationInsights.Configuration.setAutoCollectRequests(false)`

## <a name="debug"></a>Modus voor ontwikkelaars
Tijdens de foutopsporing, is het nuttig om uw telemetrie afgehandeld via de pipeline, zodat u resultaten onmiddellijk kunt zien. U ook get extra berichten sturen die u helpen traceren problemen met de telemetrie. Schakel deze uit in productie, omdat dit kan uw app vertragen.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*Visual Basic*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="ikey"></a>De instrumentatiesleutel voor de geselecteerde aangepaste telemetrie instellen
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="dynamic-ikey"></a>Dynamische instrumentatiesleutel
Om te voorkomen dat de combinatie van telemetrie van ontwikkeling, testen en productieomgevingen, kunt u [afzonderlijke Application Insights-resources maken](app-insights-create-new-resource.md) en wijzigt u de sleutels, afhankelijk van de omgeving.

In plaats van de instrumentatiesleutel ophalen uit het configuratiebestand, kunt u dit instellen in uw code. Stel de sleutel in een initialiseringsmethode, zoals global.aspx.cs in een ASP.NET-beheerservice:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

*JavaScript*

    appInsights.config.instrumentationKey = myKey;



In webpagina's, is het raadzaam om deze te stellen van de webserver status, in plaats van letterlijk coderen in het script. Bijvoorbeeld in een webpagina in een ASP.NET-app gegenereerd:

*JavaScript in Razor*

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a>TelemetryContext
TelemetryClient heeft een contexteigenschap die waarden bevat die samen met alle telemetriegegevens worden verzonden. Normaal gesproken zijn ingesteld door de standaard telemetrie-modules, maar u kunt ook instellen ze zelf. Bijvoorbeeld:

    telemetry.Context.Operation.Name = "MyOperationName";

Als u een van deze waarden zelf instellen, kunt u overwegen verwijderen van de betreffende regel uit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), zodat uw waarden en de standaard waarden niet veranderen.

* **Onderdeel**: de app en de versie ervan.
* **Apparaat**: gegevens over het apparaat waarop de app wordt uitgevoerd. (In web-apps is dit de server of client-apparaat dat de telemetrie van wordt verzonden.)
* **InstrumentationKey**: de Application Insights-resource in Azure waar de telemetrie worden weergegeven. Dit wordt gewoonlijk opgenomen in ApplicationInsights.config.
* **Locatie**: de geografische locatie van het apparaat.
* **Bewerking**: In web-apps, de huidige HTTP-aanvraag. In andere typen Apps die kunt u dit instellen op gebeurtenissen groeperen samen.
  * **Id**: een gegenereerde waarde die verschillende gebeurtenissen, zodat wanneer u een gebeurtenis in Diagnostic Search inspecteert, u vindt verwante objecten.
  * **Naam**: een meestal de URL van de HTTP-aanvraag-id.
  * **SyntheticSource**: als dat niet null of leeg is, moet een tekenreeks die aangeeft dat de bron van de aanvraag is geïdentificeerd als een robot of web-test. Standaard wordt het uitgesloten van berekeningen in Metrics Explorer.
* **Eigenschappen**: eigenschappen die worden verzonden met alle telemetriegegevens. Deze kan worden genegeerd in afzonderlijke bijhouden * aanroepen.
* **Sessie**: de gebruikerssessie. De ID wordt ingesteld op een gegenereerde waarde, die wordt gewijzigd wanneer de gebruiker niet actief is geweest even.
* **Gebruiker**: gebruikersgegevens.

## <a name="limits"></a>Limieten
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

Gebruiken om te voorkomen roept de frequentielimiet gegevens, [steekproeven](app-insights-sampling.md).

Om te bepalen hoe lang gegevens wordt opgeslagen, Zie [bewaren van gegevens en privacy](app-insights-data-retention-privacy.md).

## <a name="reference-docs"></a>Referentiedocumenten
* [ASP.NET-verwijzing](https://msdn.microsoft.com/library/dn817570.aspx)
* [Naslaginformatie over Java](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [Verwijzing in JavaScript](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android-SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS-SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK-code
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [Windows Server-pakketten](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [Node.js SDK](https://github.com/Microsoft/ApplicationInsights-Node.js)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)
* [Alle platforms](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Vragen
* *Welke uitzonderingen's Track_() aanroepen weggooien*

    Geen. U hoeft niet laten teruglopen ze in trycatch-componenten. Als de SDK-problemen, wordt deze berichten aanmelden uitvoer van de console voor foutopsporing en--als de berichten via--in diagnostische gegevens doorzoeken ophalen.
* *Is er een REST-API om gegevens te verkrijgen via de portal?*

    Ja, de [data access-API](https://dev.applicationinsights.io/). Andere manieren om gegevens te extraheren, bijvoorbeeld [van analytische gegevens exporteren naar Power BI](app-insights-export-power-bi.md) en [continue export](app-insights-export-telemetry.md).

## <a name="next"></a>Volgende stappen
* [Zoekopdracht gebeurtenissen en Logboeken](app-insights-diagnostic-search.md)

* [Problemen oplossen](app-insights-troubleshoot-faq.md)


