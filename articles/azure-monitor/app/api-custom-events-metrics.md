---
title: Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens | Microsoft Docs
description: Een paar regels code invoegen in uw apparaat of bureaublad-app, webpagina of service, bijhouden en problemen diagnosticeren.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: mbullwin
ms.openlocfilehash: 8c1ff4d2ca9ab7257b28a2b14aaf5a796183a286
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576261"
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens

Een paar regels code invoegen in uw toepassing om erachter te komen wat gebruikers met deze doen, of om problemen vast te stellen. U kunt de telemetrie verzenden van apparaat- en bureaublad-apps, web-clients en webservers. Gebruik de [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) core telemetrie-API voor het verzenden van aangepaste gebeurtenissen en metrische gegevens en uw eigen versies van telemetrie. Deze API is dezelfde API die gebruikmaken van de standaard Application Insights-gegevens-collectors.

> [!NOTE]
> `TrackMetric()` is niet langer de voorkeursmethode voor het verzenden van aangepaste metrische gegevens voor uw .NET-toepassingen. In [versie 2,60 Bèta 3](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/CHANGELOG.md#version-260-beta3) van de Application Insights SDK voor .NET een nieuwe methode [ `TelemetryClient.GetMetric()` ](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.getmetric?view=azure-dotnet) werd geïntroduceerd. Vanaf de Application Insights .NET SDK [versie 2.72](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.getmetric?view=azure-dotnet) deze functionaliteit maakt nu deel uit van de stabiele versie.

## <a name="api-summary"></a>API-overzicht

De core API uniform wordt voor alle platformen, naast enkele variaties zoals `GetMetric`(alleen voor .NET).

| Methode | Gebruikt voor |
| --- | --- |
| [`TrackPageView`](#page-views) |Pagina's, schermen, blades of formulieren. |
| [`TrackEvent`](#trackevent) |Acties van gebruikers en andere gebeurtenissen. Gebruikt voor het bijhouden van gebruikersgedrag, of om prestaties te bewaken. |
| [`GetMetric`](#getmetric) |Nul en multi-dimensionale metrische gegevens, centraal geconfigureerde aggregatie, C# alleen. |
| [`TrackMetric`](#trackmetric) |De metingen van de prestaties, zoals lengtes niet gerelateerd zijn aan specifieke gebeurtenissen. |
| [`TrackException`](#trackexception) |Logboekregistratie van uitzonderingen voor diagnose. Traceer waar ze ten opzichte van andere gebeurtenissen optreden en bekijk stack-traces. |
| [`TrackRequest`](#trackrequest) |Logboekregistratie van de frequentie en duur van de serveraanvragen voor analyse van prestaties. |
| [`TrackTrace`](#tracktrace) |Diagnostische logboekberichten. U kunt ook de logboeken van derden vastleggen. |
| [`TrackDependency`](#trackdependency) |De duur en de frequentie van aanroepen naar externe onderdelen die uw app afhankelijk van is logboekregistratie. |

U kunt [eigenschappen en metrische gegevens koppelen](#properties) voor de meeste van deze telemetrie-aanroepen.

## <a name="prep"></a>Voordat u begint

Als u nog een verwijzing op Application Insights-SDK hebt:

* Voeg de Application Insights SDK toe aan uw project:

  * [ASP.NET-project](../../azure-monitor/app/asp-net.md)
  * [Java-project](../../azure-monitor/app/java-get-started.md)
  * [Node.js-project](../../azure-monitor/app/nodejs.md)
  * [JavaScript in elke webpagina](../../azure-monitor/app/javascript.md) 
* Neem in uw apparaat- of webservercode het volgende op:

    *C#:* `using Microsoft.ApplicationInsights;`

    *Visual Basic:* `Imports Microsoft.ApplicationInsights`

    *Java:* `import com.microsoft.applicationinsights.TelemetryClient;`

    *Node.js:* `var applicationInsights = require("applicationinsights");`

## <a name="get-a-telemetryclient-instance"></a>Een TelemetryClient-exemplaar ophalen

Een exemplaar van `TelemetryClient` (behalve in JavaScript in webpagina's):

*C#*

```csharp
private TelemetryClient telemetry = new TelemetryClient();
```

*Visual Basic*

```vb
Private Dim telemetry As New TelemetryClient
```

*Java*

```java
private TelemetryClient telemetry = new TelemetryClient();
``` 

*Node.js*

```javascript
var telemetry = applicationInsights.defaultClient;
```

TelemetryClient is-thread-veilig.

ASP.NET en Java-projecten, worden binnenkomende HTTP-aanvragen automatisch vastgelegd. Het is raadzaam om extra exemplaren van de TelemetryClient voor andere module van uw app te maken. Bijvoorbeeld, misschien één TelemetryClient-exemplaar in uw klasse middleware rapport zakelijke logica gebeurtenissen. U kunt eigenschappen, zoals gebruikers-id en de apparaat-id voor het identificeren van de computer instellen. Deze informatie is gekoppeld aan alle gebeurtenissen die de instantie wordt verzonden. 

*C#*

```csharp
TelemetryClient.Context.User.Id = "...";
TelemetryClient.Context.Device.Id = "...";
```

*Java*

```java
telemetry.getContext().getUser().setId("...");
telemetry.getContext().getDevice().setId("...");
```

In Node.js-projecten, kunt u `new applicationInsights.TelemetryClient(instrumentationKey?)` te maken van een nieuwe instantie, maar dit wordt alleen aanbevolen voor scenario's waarin geïsoleerde configuratie van de singleton `defaultClient`.

## <a name="trackevent"></a>TrackEvent

In Application Insights, een *aangepaste gebeurtenis* is van een gegevenspunt geldt dat u kunt weergeven in [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) als een samengevoegd aantal, en in [diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md) als afzonderlijke exemplaren. (Deze is niet verwant aan de MVC- of andere framework "events".)

Invoegen `TrackEvent` aanroepen in uw code voor het tellen van diverse gebeurtenissen. Hoe vaak gebruikers ervoor kiezen een bepaalde functie, hoe vaak ze specifieke doelen kan bereiken of misschien hoe vaak ze bepaalde typen fouten maken.

Bijvoorbeeld in een game-Apps, een gebeurtenis wanneer een gebruiker, het spel wint verzenden:

*JavaScript*

```javascript
appInsights.trackEvent("WinGame");
```

*C#*

```csharp
telemetry.TrackEvent("WinGame");
```

*Visual Basic*

```vb
telemetry.TrackEvent("WinGame")
```

*Java*

```java
telemetry.trackEvent("WinGame");
```

*Node.js*

```javascript
telemetry.trackEvent({name: "WinGame"});
```

### <a name="custom-events-in-analytics"></a>Aangepaste gebeurtenissen in Analytics

De telemetrie is beschikbaar in de `customEvents` in tabel [Application Insights Analytics](analytics.md). Elke rij vertegenwoordigt een aanroep van `trackEvent(..)` in uw app.

Als [steekproeven](../../azure-monitor/app/sampling.md) worden uitgevoerd, de eigenschap itemCount geeft een waarde groter dan 1. Voor voorbeeld itemCount == 10 betekent dat van 10 aanroepen van trackEvent(), het proces steekproeven alleen een van beide overgedragen. Als u het juiste aantal aangepaste gebeurtenissen, moet u daarom code gebruiken zoals `customEvents | summarize sum(itemCount)`.

## <a name="getmetric"></a>GetMetric

### <a name="examples"></a>Voorbeelden

*C#*

```csharp
#pragma warning disable CA1716  // Namespace naming

namespace User.Namespace.Example01
{
    using System;
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;

    /// <summary>
    /// Most simple cases are one-liners.
    /// This is all possible without even importing an additional namespace.
    /// </summary>

    public class Sample01
    {
        /// <summary />
        public static void Exec()
        {
            // *** SENDING METRICS ***

            // Recall how you send custom telemetry with Application Insights in other cases, e.g. Events.
            // The following will result in an EventTelemetry object to be sent to the cloud right away.
            TelemetryClient client = new TelemetryClient();
            client.TrackEvent("SomethingInterestingHappened");

            // Metrics work very similar. However, the value is not sent right away.
            // It is aggregated with other values for the same metric, and the resulting summary (aka "aggregate" is sent automatically every minute.
            // To mark this difference, we use a pattern that is similar, but different from the established TrackXxx(..) pattern that sends telemetry right away:

            client.GetMetric("CowsSold").TrackValue(42);

            // *** MULTI-DIMENSIONAL METRICS ***

            // The above example shows a zero-dimensional metric.
            // Metrics can also be multi-dimensional.
            // In the initial version we are supporting up to 2 dimensions, and we will add support for more in the future as needed.
            // Here is an example for a one-dimensional metric:

            Metric animalsSold = client.GetMetric("AnimalsSold", "Species");

            animalsSold.TrackValue(42, "Pigs");
            animalsSold.TrackValue(24, "Horses");

            // The values for Pigs and Horses will be aggregated separately from each other and will result in two distinct aggregates.
            // You can control the maximum number of number data series per metric (and thus your resource usage and cost).
            // The default limits are no more than 1000 total data series per metric, and no more than 100 different values per dimension.
            // We discuss elsewhere how to change them.
            // We use a common .NET pattern: TryXxx(..) to make sure that the limits are observed.
            // If the limits are already reached, Metric.TrackValue(..) will return False and the value will not be tracked. Otherwise it will return True.
            // This is particularly useful if the data for a metric originates from user input, e.g. a file:

            Tuple<int, string> countAndSpecies = ReadSpeciesFromUserInput();
            int count = countAndSpecies.Item1;
            string species = countAndSpecies.Item2;

            if (!animalsSold.TrackValue(count, species))

            {
                client.TrackTrace($"Data series or dimension cap was reached for metric {animalsSold.Identifier.MetricId}.", SeverityLevel.Error);
            }

            // You can inspect a metric object to reason about its current state. For example:
            int currentNumberOfSpecies = animalsSold.GetDimensionValues(1).Count;
        }

        private static void ResetDataStructure()
        {
            // Do stuff
        }

        private static Tuple<int, string> ReadSpeciesFromUserInput()
        {
            return Tuple.Create(18, "Cows");
        }

        private static int AddItemsToDataStructure()
        {
            // Do stuff
            return 5;
        }
    }
}
```

## <a name="trackmetric"></a>TrackMetric

> [!NOTE]
> Microsoft.ApplicationInsights.TelemetryClient.TrackMetric is afgeschaft in de .NET SDK. Metrische gegevens moet altijd vooraf worden samengevoegd in een bepaalde periode voordat het wordt verzonden. Gebruik een van de overloads GetMetric(..) ophalen van een metrische object voor toegang tot SDK vooraf aggregatie mogelijkheden. Als u uw eigen logica vooraf aggregatie implementeert, kunt u de Track (ITelemetry metricTelemetry)-methode voor het verzenden van de resulterende statistische functies. Als uw toepassing een afzonderlijke Telemetrisch item verzenden bij elke zonder aggregatie in de tijd vereist, hebt u waarschijnlijk een use-case voor telemetrie van gebeurtenissen; Zie TelemetryClient.TrackEvent (Microsoft.ApplicationInsights.DataContracts.EventTelemetry).

Application Insights kunnen metrische gegevens die niet zijn gekoppeld aan bepaalde gebeurtenissen van grafiek. Bijvoorbeeld, kan u de lengte van een wachtrij met regelmatige tussenpozen controleren. De afzonderlijke metingen zijn minder interessant zijn dan de variaties en trends met metrische gegevens, en dus statistische kolomdiagrammen zijn nuttig.

Om te kunnen metrische gegevens verzenden naar Application Insights, kunt u de `TrackMetric(..)` API. Er zijn twee manieren voor het verzenden van een metrische waarde:

* Één waarde. Telkens wanneer u een meting in uw toepassing uitvoert, kunt u de overeenkomstige waarde verzenden naar Application Insights. Bijvoorbeeld, wordt ervan uitgegaan dat u hebt een metrische waarde met een beschrijving van het aantal items in een container. Tijdens een bepaalde periode, u voor het eerst drie items in de container geplaatst en verwijder vervolgens u twee items. Dienovereenkomstig, roept u `TrackMetric` tweemaal: geeft de eerst de waarde `3` en vervolgens de waarde `-2`. Application Insights slaat beide waarden uit uw naam.

* Aggregatie. Als u werkt met metrische gegevens, wordt elke meting zelden is van belang. In plaats daarvan is een samenvatting van wat is er gebeurd gedurende een bepaalde periode belangrijk. Een samenvatting van dergelijke heet _aggregatie_. In het bovenstaande voorbeeld wordt de som van het cumulatieve metrische gegevens voor die periode is `1` en het aantal van de metrische waarden is `2`. Wanneer u de aggregatie-methode gebruikt, u alleen aanroepen `TrackMetric` eenmaal per periode en de samengevoegde waarden te verzenden. Dit is de aanbevolen aanpak omdat deze de kosten en de prestaties aanzienlijk verkorten kan overhead door minder gegevenspunten te verzenden naar Application Insights, terwijl u nog steeds alle relevante informatie te verzamelen.

### <a name="examples"></a>Voorbeelden

#### <a name="single-values"></a>Enkele waarden

Voor het verzenden van één metrische waarde:

*JavaScript*

 ```javascript
appInsights.trackMetric("queueLength", 42.0);
 ```

*C#*

```csharp
var sample = new MetricTelemetry();
sample.Name = "metric name";
sample.Value = 42.3;
telemetryClient.TrackMetric(sample);
```

*Java*

```java
telemetry.trackMetric("queueLength", 42.0);
```

*Node.js*

 ```javascript
telemetry.trackMetric({name: "queueLength", value: 42.0});
 ```

### <a name="custom-metrics-in-analytics"></a>Aangepaste metrische gegevens in Analytics

De telemetrie is beschikbaar in de `customMetrics` in tabel [Application Insights Analytics](analytics.md). Elke rij vertegenwoordigt een aanroep van `trackMetric(..)` in uw app.

* `valueSum` -Dit is de som van de metingen. Als u de gemiddelde waarde, waardoor `valueCount`.
* `valueCount` -Het aantal van de metingen die zijn samengevoegd in dit `trackMetric(..)` aanroepen.

## <a name="page-views"></a>Paginaweergaven

In een apparaat of webpagina-app, telemetrie van paginaweergaven worden standaard verzonden wanneer elk scherm of pagina wordt geladen. Maar u kunt dit wijzigen voor het volgen van paginaweergaven op aanvullende of verschillende tijdstippen. Bijvoorbeeld in een app die tabbladen of blades wordt weergegeven, kunt u voor het bijhouden van een pagina wanneer de gebruiker wordt een nieuwe blade geopend.

Gebruikers- en -gegevens worden verzonden als eigenschappen samen met paginaweergaven, zodat de gebruikers- en -grafieken tot leven komen wanneer er telemetrie van paginaweergaven.

### <a name="custom-page-views"></a>Aangepaste paginaweergaven

*JavaScript*

```javascript
appInsights.trackPageView("tab1");
```

*C#*

```csharp
telemetry.TrackPageView("GameReviewPage");
```

*Visual Basic*

```vb
telemetry.TrackPageView("GameReviewPage")
```

*Java*

```java
telemetry.trackPageView("GameReviewPage");
```

Als u meerdere tabbladen in verschillende HTML-pagina's hebt, kunt u de URL te opgeven:

```javascript
appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");
```

### <a name="timing-page-views"></a>Timing paginaweergaven

Standaard worden de tijden die zijn gerapporteerd als **paginalaadtijden** worden gemeten vanaf het moment de browser de aanvraag verzendt totdat gebeurtenis voor het laden van pagina van de browser wordt aangeroepen.

In plaats daarvan kunt u deze:

* Stel een expliciete duur de [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) aanroepen: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Gebruik de paginaweergave timing aanroepen `startTrackPage` en `stopTrackPage`.

*JavaScript*

```javascript
// To start timing a page:
appInsights.startTrackPage("Page1");

...

// To stop timing and log the page:
appInsights.stopTrackPage("Page1", url, properties, measurements);
```

De naam die u als de eerste parameter gebruiken koppelt de gesprekken starten en stoppen. Wordt standaard de naam van de huidige pagina.

De resulterende pagina laden duur weergegeven in Metrics Explorer zijn afgeleid van het interval tussen de begin- en stop-aanroepen. Het is aan u wat u daadwerkelijk tijd interval.

### <a name="page-telemetry-in-analytics"></a>Pagina-telemetrie in Analytics

In [Analytics](analytics.md) twee tabellen bevatten gegevens uit de bewerkingen van de browser:

* De `pageViews` tabel bevat gegevens over de URL- en titel
* De `browserTimings` tabel bevat gegevens over de prestaties van de client, zoals de tijd voor het verwerken van de binnenkomende gegevens

Om te zoeken hoe lang de browser nodig is om verschillende pagina's:

```kusto
browserTimings
| summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name
```

Voor het detecteren van de popularities van verschillende browsers:

```kusto
pageViews
| summarize count() by client_Browser
```

Als u wilt koppelen paginaweergaven, AJAX-aanroepen, moet u lid met afhankelijkheden:

```kusto
pageViews
| join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest

De SDK-server maakt gebruik van TrackRequest om aan te melden HTTP-aanvragen.

U kunt deze zelf ook aanroepen als u verzoeken wilt simuleren in een context waarin u de web service-module uitgevoerd hebt.

Echter, de aanbevolen manier voor het verzenden van aanvraagtelemetrie is waar de aanvraag fungeert als een <a href="#operation-context">bewerkingscontext</a>.

## <a name="operation-context"></a>Bewerkingscontext

U kunt telemetrie-items bij elkaar correleren door deze te koppelen met de bewerkingscontext. De standard-aanvraag bij te houden module doet dit voor uitzonderingen en andere gebeurtenissen die worden verzonden terwijl een HTTP-aanvraag wordt verwerkt. In [zoeken](../../azure-monitor/app/diagnostic-search.md) en [Analytics](analytics.md), vindt u eenvoudig alle gebeurtenissen die zijn gekoppeld aan de aanvraag met behulp van de bewerking-ID.

Zie [telemetriecorrelatie in Application Insights](../../azure-monitor/app/correlation.md) voor meer informatie over correlatie.

Wanneer het bijhouden van telemetrie handmatig, de eenvoudigste manier om ervoor te zorgen telemetriecorrelatie met behulp van dit patroon:

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

Samen met het instellen van de bewerkingscontext van een, `StartOperation` wordt gemaakt van een Telemetrisch item van het type dat u opgeeft. Stuurt de telemetrie-item wanneer u de bewerking verwijderen, of als u expliciet aanroepen `StopOperation`. Als u `RequestTelemetry` als het telemetrietype, de duur is ingesteld op de getimed interval tussen starten en stoppen.

Telemetrie-items dat is gerapporteerd binnen een bereik van opnieuw worden 'kinderen' van deze bewerking. Bewerking contexten kunnen worden genest.

In het zoekvak, de bewerkingscontext wordt gebruikt om de **verwante Items** lijst:

![Gerelateerde items](./media/api-custom-events-metrics/21.png)

Zie [aangepaste bewerkingen met Application Insights .NET-SDK bijhouden](../../azure-monitor/app/custom-operations-tracking.md) voor meer informatie over aangepaste bewerkingen bijhouden.

### <a name="requests-in-analytics"></a>Aanvragen in Analytics

In [Application Insights Analytics](analytics.md), weergeven van aanvragen in de `requests` tabel.

Als [steekproeven](../../azure-monitor/app/sampling.md) is uitgevoerd, de eigenschap itemCount ziet u een waarde groter is dan 1. Voor voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar trackRequest() het proces steekproeven alleen een van beide overgedragen. Als u een juiste aantal aanvragen en gemiddelde duur gesegmenteerd op aanvraag namen, zoals code gebruiken:

```kusto
requests
| summarize count = sum(itemCount), avgduration = avg(duration) by name
```

## <a name="trackexception"></a>TrackException

Uitzonderingen verzenden naar Application Insights:

* Naar [ze tellen](../../azure-monitor/app/metrics-explorer.md), als een indicatie van de frequentie van een probleem.
* Naar [afzonderlijke exemplaren bekijken](../../azure-monitor/app/diagnostic-search.md).

De rapporten bevatten de stack-traces.

*C#*

```csharp
try
{
    ...
}
catch (Exception ex)
{
    telemetry.TrackException(ex);
}
```

*Java*

```java
try {
    ...
} catch (Exception ex) {
    telemetry.trackException(ex);
}
```

*JavaScript*

```javascript
try
{
    ...
}
catch (ex)
{
    appInsights.trackException(ex);
}
```

*Node.js*

```javascript
try
{
    ...
}
catch (ex)
{
    telemetry.trackException({exception: ex});
}
```

De SDK's catch veel uitzonderingen automatisch, zodat u altijd niet hoeven te TrackException expliciet aanroepen.

* ASP.NET: [Schrijf code afvangen van uitzonderingen](../../azure-monitor/app/asp-net-exceptions.md).
* Java EE: [Uitzonderingen automatisch zijn opgepikt](../../azure-monitor/app/java-get-started.md#exceptions-and-request-failures).
* JavaScript: Uitzonderingen worden automatisch onderschept. Als u uitschakelen automatisch verzamelen wilt, moet u een regel toegevoegd aan het codefragment die u in uw webpagina's invoegen:

```javascript
({
    instrumentationKey: "your key",
    disableExceptionTracking: true
})
```

### <a name="exceptions-in-analytics"></a>Uitzonderingen in Analytics

In [Application Insights Analytics](analytics.md), uitzonderingen weergegeven in de `exceptions` tabel.

Als [steekproeven](../../azure-monitor/app/sampling.md) worden uitgevoerd, de `itemCount` eigenschap bevat een waarde groter dan 1. Voor voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar trackException() het proces steekproeven alleen een van beide overgedragen. Als u een juiste aantal uitzonderingen gesegmenteerd op type uitzondering, zoals code gebruiken:

```kusto
exceptions
| summarize sum(itemCount) by type
```

De meeste belangrijke stack informatie al wordt geëxtraheerd in verschillende variabelen, maar u kunt ophalen uit elkaar de `details` structuur om meer te halen. Omdat deze structuur dynamisch is, moet u het resultaat dat het verwachte type cast-conversie. Bijvoorbeeld:

```kusto
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

Als u wilt koppelen uitzonderingen met hun verwante aanvragen, gebruikt u een join:

```kusto
exceptions
| join (requests) on operation_Id
```

## <a name="tracktrace"></a>TrackTrace

Gebruik TrackTrace om u te helpen bij het vaststellen van problemen door een 'navigatiepad' verzenden naar Application Insights. U kunt segmenten van diagnostische gegevens verzenden en controleren in [diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md).

In .NET [adapters zich](../../azure-monitor/app/asp-net-trace-logs.md) gebruik van deze API van derden om Logboeken te verzenden naar de portal.

In Java voor [Standard kunt zoals Log4J, Logback](../../azure-monitor/app/java-trace-logs.md) Application Insights Log4j of Logback Appenders van derden om Logboeken te verzenden naar de portal gebruiken.

*C#*

```csharp
telemetry.TrackTrace(message, SeverityLevel.Warning, properties);
```

*Java*

```java
telemetry.trackTrace(message, SeverityLevel.Warning, properties);
```

*Node.js*

```javascript
telemetry.trackTrace({
    message: message,
    severity: applicationInsights.Contracts.SeverityLevel.Warning,
    properties: properties
});
```

*Browser-client-side-JavaScript*

```javascript
trackTrace(message: string, properties?: {[string]:string}, severityLevel?: AI.SeverityLevel)
```

Meld u aan een diagnostische gebeurtenissen zoals invoeren of een methode verlaten.

 Parameter | Description
---|---
`message` | Diagnostische gegevens. Kan veel langer zijn dan een naam.
`properties` | Toewijzing van tekenreeks naar een tekenreeks: Aanvullende gegevens die worden gebruikt voor het [filteren uitzonderingen](https://azure.microsoft.com/documentation/articles/app-insights-api-custom-events-metrics/#properties) in de portal. De standaardkleur is leeg.
`severityLevel` | Ondersteunde waarden: [SeverityLevel.ts](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/JavaScript/JavaScriptSDK.Interfaces/Contracts/Generated/SeverityLevel.ts)

U kunt zoeken op inhoud van het bericht, maar (in tegenstelling tot eigenschapswaarden) u kunt niet filteren op het.

De maximale grootte op `message` is veel hoger dan de limiet voor eigenschappen.
Een voordeel van TrackTrace is u kunt relatief veel gegevens in het bericht te plaatsen. Bijvoorbeeld, kunt u er postgegevens coderen.  

Bovendien kunt u een ernstniveau toevoegen aan uw bericht. En net als andere telemetrie, kunt u eigenschapswaarden om te filteren of zoeken naar verschillende sets van traceringen toevoegen. Bijvoorbeeld:

*C#*

```csharp
var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
telemetry.TrackTrace("Slow database response",
                SeverityLevel.Warning,
                new Dictionary<string,string> { {"database", db.ID} });
```

*Java*

```java
Map<String, Integer> properties = new HashMap<>();
properties.put("Database", db.ID);
telemetry.trackTrace("Slow Database response", SeverityLevel.Warning, properties);
```

In [zoeken](../../azure-monitor/app/diagnostic-search.md), u kunt eenvoudig filteren vervolgens alle berichten van een specifieke ernst op die betrekking op een bepaalde database hebben.

### <a name="traces-in-analytics"></a>Traceringen in Analytics

In [Application Insights Analytics](analytics.md), aanroepen van TrackTrace weergegeven in de `traces` tabel.

Als [steekproeven](../../azure-monitor/app/sampling.md) worden uitgevoerd, de eigenschap itemCount geeft een waarde groter dan 1. Voor voorbeeld itemCount == 10 houdt in dat van 10 aanroepen naar `trackTrace()`, een van deze alleen in het proces steekproeven worden verzonden. Als u het juiste aantal trace-aanroepen, moet u daarom code zoals `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency

Gebruik de TrackDependency-aanroep voor het bijhouden van de responstijden en succespercentages van aanroepen naar externe code. De resultaten worden weergegeven in de afhankelijkheidsgrafiek in de portal.

*C#*

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
     // The call above has been made obsolete in the latest SDK. The updated call follows this format:
     // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
}
```

*Java*

```java
boolean success = false;
long startTime = System.currentTimeMillis();
try {
    success = dependency.call();
}
finally {
    long endTime = System.currentTimeMillis();
    long delta = endTime - startTime;
    RemoteDependencyTelemetry dependencyTelemetry = new RemoteDependencyTelemetry("My Dependency", "myCall", delta, success);
    telemetry.setTimeStamp(startTime);
    telemetry.trackDependency(dependencyTelemetry);
}
```

*JavaScript*

```javascript
var success = false;
var startTime = new Date().getTime();
try
{
    success = dependency.Call();
}
finally
{
    var elapsed = new Date() - startTime;
    telemetry.trackDependency({
        dependencyTypeName: "myDependency",
        name: "myCall",
        duration: elapsed,
        success: success
    });
}
```

Houd er rekening mee dat de server SDK's bevatten een [afhankelijkheid module](../../azure-monitor/app/asp-net-dependencies.md) die worden gedetecteerd en bijgehouden bepaalde afhankelijkheidsaanroepen automatisch--bijvoorbeeld tot databases en REST-API's. U moet een agent installeren op uw server om te maken van de module werken. 

In Java, bepaalde afhankelijkheidsaanroepen kunnen worden automatisch bijgehouden met behulp van [Java-Agent](../../azure-monitor/app/java-agent.md).

U gebruikt deze aanroep als u wilt aanroepen die de geautomatiseerde tracering niet werkelijk bijhouden, of als u niet wilt dat de agent te installeren.

Als u de standaard afhankelijkheid bijhouden-module in C# uitschakelen, bewerken [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) en verwijder de verwijzing naar `DependencyCollector.DependencyTrackingTelemetryModule`. In Java,. Installeer niet java-agent als u niet wilt standard afhankelijkheden automatisch verzamelen.

### <a name="dependencies-in-analytics"></a>Afhankelijkheden in Analytics

In [Application Insights Analytics](analytics.md), trackDependency belt weergeven de `dependencies` tabel.

Als [steekproeven](../../azure-monitor/app/sampling.md) worden uitgevoerd, de eigenschap itemCount geeft een waarde groter dan 1. Voor voorbeeld itemCount == 10 betekent dat van 10 aanroepen naar trackDependency() het proces steekproeven alleen een van beide overgedragen. Als u een juiste aantal afhankelijkheden gesegmenteerd op target-component, zoals code gebruiken:

```kusto
dependencies
| summarize sum(itemCount) by target
```

Als u wilt koppelen afhankelijkheden met hun verwante aanvragen, gebruikt u een join:

```kusto
dependencies
| join (requests) on operation_Id
```

## <a name="flushing-data"></a>Gegevens leegmaken

Normaal gesproken verzendt de SDK soms ervoor gekozen om te beperken de gevolgen voor de gebruiker. Echter, in sommige gevallen kunt u leegmaken van de buffer--bijvoorbeeld, als u de SDK in een toepassing die wordt afgesloten.

*C#*

 ```csharp
telemetry.Flush();
// Allow some time for flushing before shutdown.
System.Threading.Thread.Sleep(5000);
```

*Java*

```java
telemetry.flush();
//Allow some time for flushing before shutting down
Thread.sleep(5000);
```

*Node.js*

```javascript
telemetry.flush();
```

De functie voor asynchrone is de [server telemetrie kanaal](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

In het ideale geval moet flush() methode worden gebruikt in de activiteit afsluiten van de toepassing.

## <a name="authenticated-users"></a>Geverifieerde gebruikers

In een web-app, worden gebruikers (standaard) geïdentificeerd door cookies. Een gebruiker kan meer dan één keer worden geteld als ze toegang krijgen tot uw app uit een andere computer of de browser, of als het verwijderen van cookies.

Als gebruikers zich bij uw app aanmelden, krijgt u een meer nauwkeurige telling door in te stellen van de geverifieerde gebruikers-ID in de browsercode:

*JavaScript*

```javascript
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

In een ASP.NET-web MVC-toepassing, bijvoorbeeld:

*Razor*

```cshtml
@if (Request.IsAuthenticated)
{
    <script>
        appInsights.setAuthenticatedUserContext("@User.Identity.Name
            .Replace("\\", "\\\\")"
            .replace(/[,;=| ]+/g, "_"));
    </script>
}
```

Het is niet nodig om werkelijke van de gebruiker aanmelden naam te gebruiken. Alleen er moet een ID die uniek is voor die gebruiker. Mogen geen spaties of een van de tekens `,;=|`.

De gebruikers-ID is ook in een sessiecookie ingesteld en naar de server verzonden. Als de server-SDK is geïnstalleerd, wordt de geverifieerde gebruikers-ID verzonden als onderdeel van de contexteigenschappen van de client- en telemetrie. U kunt vervolgens filteren en zoek naar deze.

Als uw app gebruikers in accounts groepen, kunt u ook een id voor het account (met de dezelfde tekenbeperkingen) doorgeven.

```javascript
appInsights.setAuthenticatedUserContext(validatedId, accountId);
```

In [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md), kunt u een grafiek telt **gebruikers, geverifieerd**, en **gebruikersaccounts**.

U kunt ook [zoeken](../../azure-monitor/app/diagnostic-search.md) voor gegevenspunten van de client met de namen van de specifieke gebruikers en accounts.

## <a name="properties"></a>Filteren, zoeken en uw gegevens segmenteren met behulp van eigenschappen

U kunt eigenschappen en metingen toevoegen aan uw gebeurtenissen (en ook tot metrische gegevens over, pagina weergaven, uitzonderingen en andere telemetriegegevens).

*Eigenschappen* tekenreekswaarden die u gebruiken kunt voor het filteren van uw telemetrie in de rapporten over gebruik zijn. Bijvoorbeeld, als uw app verschillende games biedt, kunt u koppelen de naam van het spel aan elke gebeurtenis zodat u kunt zien welke games meer populair zijn.

Er is een limiet van 8192 liggen op de lengte van de tekenreeks. (Als u wilt voor het verzenden van grote hoeveelheden gegevens, gebruikt u de parameter bericht van TrackTrace.)

*Metrische gegevens* zijn numerieke waarden grafisch kunnen worden weergegeven. Bijvoorbeeld, als u wilt zien of er een geleidelijke toename van de scores die uw gamers behalen. De grafieken kunnen worden gesegmenteerd op de eigenschappen die worden verzonden met de gebeurtenis, zodat u kunt afzonderlijke ophalen of gestapelde diagrammen voor verschillende games.

Voor metrische waarden correct wordt weergegeven, moeten ze groter dan of gelijk zijn aan 0 zijn.

Er zijn een aantal [beperkingen met betrekking tot het aantal eigenschappen en eigenschapswaarden metrische gegevens](#limits) die u kunt gebruiken.

*JavaScript*

```javascript
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
```

*C#*

```csharp
// Set up some properties and metrics:
var properties = new Dictionary <string, string>
    {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
var metrics = new Dictionary <string, double>
    {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

// Send the event:
telemetry.TrackEvent("WinGame", properties, metrics);
```

*Node.js*

```javascript
// Set up some properties and metrics:
var properties = {"game": currentGame.Name, "difficulty": currentGame.Difficulty};
var metrics = {"Score": currentGame.Score, "Opponents": currentGame.OpponentCount};

// Send the event:
telemetry.trackEvent({name: "WinGame", properties: properties, measurements: metrics});
```

*Visual Basic*

```vb
' Set up some properties:
Dim properties = New Dictionary (Of String, String)
properties.Add("game", currentGame.Name)
properties.Add("difficulty", currentGame.Difficulty)

Dim metrics = New Dictionary (Of String, Double)
metrics.Add("Score", currentGame.Score)
metrics.Add("Opponents", currentGame.OpponentCount)

' Send the event:
telemetry.TrackEvent("WinGame", properties, metrics)
```

*Java*

```java
Map<String, String> properties = new HashMap<String, String>();
properties.put("game", currentGame.getName());
properties.put("difficulty", currentGame.getDifficulty());

Map<String, Double> metrics = new HashMap<String, Double>();
metrics.put("Score", currentGame.getScore());
metrics.put("Opponents", currentGame.getOpponentCount());

telemetry.trackEvent("WinGame", properties, metrics);
```

> [!NOTE]
> Let erop dat niet voor logboekregistratie van persoonsgegevens in de eigenschappen.
>
>

### <a name="alternative-way-to-set-properties-and-metrics"></a>Alternatieve manier om de eigenschappen en metrische gegevens

Als het is beter uitkomt, kunt u de parameters van een gebeurtenis in een afzonderlijke object verzamelen:

```csharp
var event = new EventTelemetry();

event.Name = "WinGame";
event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
event.Properties["game"] = currentGame.Name;
event.Properties["difficulty"] = currentGame.Difficulty;
event.Metrics["Score"] = currentGame.Score;
event.Metrics["Opponents"] = currentGame.Opponents.Length;

telemetry.TrackEvent(event);
```

> [!WARNING]
> Niet opnieuw gebruiken de hetzelfde exemplaar van de telemetrie-item (`event` in dit voorbeeld) om aan te roepen Track*() meerdere keren. Dit kan leiden tot telemetrie wordt verzonden met een onjuiste configuratie.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Aangepaste metingen en eigenschappen in Analytics

In [Analytics](analytics.md), eigenschappen en aangepaste metrische gegevens weergeven in de `customMeasurements` en `customDimensions` kenmerken van elke record telemetrie.

Bijvoorbeeld, als u een eigenschap met de naam 'game' aan de aanvraagtelemetrie van uw hebt toegevoegd, deze query telt de instanties van andere waarden van 'spel' en het gemiddelde van de aangepaste metrische 'score' weergeven:

```kusto
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game)
```

U ziet dat:

* Wanneer u een waarde uit de customDimensions of customMeasurements JSON halen, het dynamisch type heeft, en dus moet u dit casten `tostring` of `todouble`.
* Rekening te houden met de mogelijkheid om [steekproeven](../../azure-monitor/app/sampling.md), moet u `sum(itemCount)`, niet `count()`.

## <a name="timed"></a> Timing-gebeurtenissen

Soms wilt u grafiek hoe lang het duurt een actie uit te voeren. Bijvoorbeeld, u mogelijk wilt weten hoe lang gebruikers rekening houden met opties in een game voor toets maken. Hiervoor kunt u de parameter meting.

*C#*

```csharp
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
```

*Java*

```java
long startTime = System.currentTimeMillis();

// Perform timed action

long endTime = System.currentTimeMillis();
Map<String, Double> metrics = new HashMap<>();
metrics.put("ProcessingTime", endTime-startTime);

// Setup some properties
Map<String, String> properties = new HashMap<>();
properties.put("signalSource", currentSignalSource.getName());

// Send the event
telemetry.trackEvent("SignalProcessed", properties, metrics);
```

## <a name="defaults"></a>Standaard-eigenschappen voor aangepaste telemetrie

Als u het instellen van de standaard eigenschapswaarden voor enkele van de aangepaste gebeurtenissen die u schrijft wilt, kunt u ze kunt instellen in een TelemetryClient-exemplaar. Ze zijn gekoppeld aan elk telemetrie-item dat wordt verzonden van die client.

*C#*

```csharp
using Microsoft.ApplicationInsights.DataContracts;

var gameTelemetry = new TelemetryClient();
gameTelemetry.Context.Properties["Game"] = currentGame.Name;
// Now all telemetry will automatically be sent with the context property:
gameTelemetry.TrackEvent("WinGame");
```

*Visual Basic*

```vb
Dim gameTelemetry = New TelemetryClient()
gameTelemetry.Context.Properties("Game") = currentGame.Name
' Now all telemetry will automatically be sent with the context property:
gameTelemetry.TrackEvent("WinGame")
```

*Java*

```java
import com.microsoft.applicationinsights.TelemetryClient;
import com.microsoft.applicationinsights.TelemetryContext;
...


TelemetryClient gameTelemetry = new TelemetryClient();
TelemetryContext context = gameTelemetry.getContext();
context.getProperties().put("Game", currentGame.Name);

gameTelemetry.TrackEvent("WinGame");
```

*Node.js*

```javascript
var gameTelemetry = new applicationInsights.TelemetryClient();
gameTelemetry.commonProperties["Game"] = currentGame.Name;

gameTelemetry.TrackEvent({name: "WinGame"});
```

Aanroepen van afzonderlijke telemetrie kunnen de standaardwaarden in de eigenschap woordenlijsten overschrijven.

*Voor JavaScript-clients*, JavaScript telemetrie initializers gebruiken.

*Eigenschappen toevoegen aan alle telemetrie*, met inbegrip van de gegevens uit de verzameling modules, [implementeren `ITelemetryInitializer` ](../../azure-monitor/app/api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Steekproeven, te filteren en verwerken van telemetrie

U kunt de code voor het verwerken van uw telemetrie voordat deze wordt verzonden van de SDK schrijven. De verwerking van bevat gegevens die worden verzonden vanaf de telemetrie-modules, zoals de verzameling van de HTTP-aanvragen en afhankelijkheid verzameling.

[Voeg eigenschappen toe](../../azure-monitor/app/api-filtering-sampling.md#add-properties) tot telemetrie door het implementeren van `ITelemetryInitializer`. U kunt bijvoorbeeld waarden die worden berekend of versienummers toevoegen van andere eigenschappen.

[Filteren](../../azure-monitor/app/api-filtering-sampling.md#filtering) kunt wijzigen of verwijderen van telemetrie voordat het wordt verzonden door de SDK door het implementeren van `ITelemetryProcessor`. U bepalen wat wordt verzonden of verwijderd, maar u moet rekening voor de gevolgen zijn voor uw metrische gegevens. Afhankelijk van hoe u items negeren, verliest u mogelijk de mogelijkheid om te navigeren tussen verwante items.

[Sampling](../../azure-monitor/app/api-filtering-sampling.md) is een oplossing voor verpakte te verminderen de hoeveelheid gegevens die vanuit uw app wordt verzonden naar de portal. Dit gebeurt zonder gevolgen voor de weergegeven metrische gegevens. En dit gebeurt zonder de mogelijkheid om problemen te diagnosticeren door te navigeren tussen verwante items zoals uitzonderingen, aanvragen en paginaweergaven.

[Meer informatie](../../azure-monitor/app/api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Telemetrie uitschakelen

Naar *dynamisch stoppen en starten* het verzamelen en verzenden van telemetrie:

*C#*

```csharp
using  Microsoft.ApplicationInsights.Extensibility;

TelemetryConfiguration.Active.DisableTelemetry = true;
```

*Java*

```java
telemetry.getConfiguration().setTrackingDisabled(true);
```

Naar *uitschakelen geselecteerde standard collectors*--bijvoorbeeld, prestatiemeteritems, HTTP-aanvragen of afhankelijkheden--verwijderen of opmerkingen bij de desbetreffende regels in [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md). U kunt dit bijvoorbeeld doen als u wilt de gegevens van uw eigen TrackRequest verzenden.

*Node.js*

```javascript
telemetry.config.disableAppInsights = true;
```

Naar *uitschakelen geselecteerde standard collectors*--bijvoorbeeld, prestatiemeteritems, HTTP-aanvragen of afhankelijkheden--tijdens de initialisatie keten configuratiemethoden toe aan uw code van de initialisatie van SDK:

```javascript
applicationInsights.setup()
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    .setAutoCollectDependencies(false)
    .setAutoCollectConsole(false)
    .start();
```

Schakel deze collectors na de initialisatie van de configuratie-object te gebruiken: `applicationInsights.Configuration.setAutoCollectRequests(false)`

## <a name="debug"></a>Modus voor ontwikkelaars

Tijdens de foutopsporing, is dit handig om uw telemetrie afgehandeld via de pijplijn, zodat u onmiddellijk de resultaten kunt bekijken. U ook get extra berichten die u helpen bij traceren problemen met de telemetrie. Functies uitschakelen in de productieomgeving, omdat deze kan uw app vertragen.

*C#*

```csharp
TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;
```

*Visual Basic*

```vb
TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True
```

*Node.js*

Voor Node.js, kunt u de modus voor ontwikkelaars inschakelen door in te schakelen interne logboekregistratie via `setInternalLogging` en de instelling `maxBatchSize` op 0, waardoor uw telemetrie wordt verzonden zodra deze is verzameld.

```js
applicationInsights.setup("ikey")
  .setInternalLogging(true, true)
  .start()
applicationInsights.defaultClient.config.maxBatchSize = 0;
```

## <a name="ikey"></a> De instrumentatiesleutel voor geselecteerde aangepaste telemetrie instellen

*C#*

```csharp
var telemetry = new TelemetryClient();
telemetry.InstrumentationKey = "---my key---";
// ...
```

## <a name="dynamic-ikey"></a> Dynamische instrumentatiesleutel

Om te voorkomen dat een combinatie van telemetrie van ontwikkeling, testen en productie-omgevingen, kunt u [maakt u afzonderlijke Application Insights-resources](../../azure-monitor/app/create-new-resource.md ) en wijzigt u de sleutels, afhankelijk van de omgeving.

In plaats van de instrumentatiesleutel ophalen uit het configuratiebestand, kunt u dit instellen in uw code. Stel de sleutel in een initialiseringsmethode, zoals global.aspx.cs in een ASP.NET-service:

*C#*

```csharp
protected void Application_Start()
{
    Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey =
        // - for example -
        WebConfigurationManager.Settings["ikey"];
    ...
}
```

*JavaScript*

```javascript
appInsights.config.instrumentationKey = myKey;
```

In webpagina's, is het raadzaam om in te stellen deze van de webserver status, in plaats van letterlijk coderen in het script. Bijvoorbeeld, in een webpagina die in een ASP.NET-app is gegenereerd:

*JavaScript in de Razor*

```cshtml
<script type="text/javascript">
// Standard Application Insights webpage script:
var appInsights = window.appInsights || function(config){ ...
// Modify this part:
}({instrumentationKey:  
    // Generate from server property:
    @Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey;
}) // ...
```

## <a name="telemetrycontext"></a>TelemetryContext

TelemetryClient heeft een contexteigenschap, waarin de waarden die samen met alle telemetriegegevens worden verzonden. Deze normaal gesproken zijn ingesteld door de modules standaardtelemetrie voor, maar u kunt ook deze zelf instellen. Bijvoorbeeld:

```csharp
telemetry.Context.Operation.Name = "MyOperationName";
```

Als u een van deze waarden zelf instellen, kunt u de desbetreffende regel verwijderen [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), zodat uw waarden en de standaard geen verwarring.

* **Onderdeel**: De app en de versie ervan.
* **Apparaat**: Gegevens over het apparaat waarop de app wordt uitgevoerd. (In web-apps, dit is de server of client-apparaat dat door de telemetrie wordt verzonden.)
* **InstrumentationKey**: De Application Insights-resource in Azure waar de telemetrie wordt weergegeven. Dit wordt meestal in ApplicationInsights.config opgehaald.
* **Locatie**: De geografische locatie van het apparaat.
* **Bewerking**: In de web-apps, de huidige HTTP-aanvraag. In andere typen Apps kunt u dit instellen op groepsgebeurtenissen samen.
  * **Id**: Een gegenereerde waarde die overeenkomt met verschillende gebeurtenissen, zodat wanneer u een gebeurtenis in de diagnostische gegevens doorzoeken inspecteren, u verwante items vindt.
  * **Naam**: Een id, meestal de URL van de HTTP-aanvraag.
  * **SyntheticSource**: Als dat niet null of leeg is, moet een tekenreeks die aangeeft dat de bron van de aanvraag is geïdentificeerd als een robot of web-test. Standaard wordt het uitgesloten van berekeningen in Metrics Explorer.
* **Eigenschappen**: De eigenschappen die worden verzonden met alle telemetriegegevens. Deze kan worden genegeerd in afzonderlijke bijhouden * aanroepen.
* **Sessie**: De gebruikerssessie. De ID is ingesteld op een gegenereerde waarde die wordt gewijzigd wanneer de gebruiker niet actief is geweest even.
* **Gebruiker**: Gebruikersgegevens.

## <a name="limits"></a>Limieten

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

Gebruiken om te voorkomen dat de limiet hebt bereikt, [steekproeven](../../azure-monitor/app/sampling.md).

Om te bepalen hoe lang gegevens worden bewaard, Zie [bewaren van gegevens en privacy](../../azure-monitor/app/data-retention-privacy.md).

## <a name="reference-docs"></a>Referentiedocumenten

* [ASP.NET-naslaginformatie](https://msdn.microsoft.com/library/dn817570.aspx)
* [Naslaginformatie over Java](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [JavaScript-referentie](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android-SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS-SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK-code

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [Pakketten van Windows Server](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [Node.js SDK](https://github.com/Microsoft/ApplicationInsights-Node.js)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)
* [Alle platforms](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Vragen

* *Welke uitzonderingen mogelijk Track_() aanroepen genereren?*

    Geen. U hoeft niet te zabalit trycatch-componenten. Als er problemen voor de SDK, wordt deze berichten zich in de console-uitvoer voor foutopsporing en--als de berichten van--doorzoeken van diagnostische gegevens ontvangt.
* *Is er een REST-API voor het ophalen van gegevens uit de portal?*

    Ja, de [data access-API](https://dev.applicationinsights.io/). Andere manieren om gegevens te extraheren, bijvoorbeeld [van Analytics exporteren naar Power BI](../../azure-monitor/app/export-power-bi.md ) en [continue export](../../azure-monitor/app/export-telemetry.md).

## <a name="next"></a>Volgende stappen

* [Logboeken en gebeurtenissen voor zoeken](../../azure-monitor/app/diagnostic-search.md)
* [Problemen oplossen](../../azure-monitor/app/troubleshoot-faq.md)
