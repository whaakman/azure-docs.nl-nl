---
title: Azure Application Insights-telemetrie in uw Java-web-app filteren | Microsoft Docs
description: Verminder Telemetrisch verkeer gefilterd op de gebeurtenissen die u niet wilt bewaken.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/23/2016
ms.author: mbullwin
ms.openlocfilehash: 6e2af5734299e43d379350b031e239f7b4a421d8
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096054"
---
# <a name="filter-telemetry-in-your-java-web-app"></a>Filteren van telemetrie in uw Java-web-app

Filters bieden een manier om te selecteren van de telemetrie die uw [Java-web-app wordt verzonden naar Application Insights](app-insights-java-get-started.md). Er zijn enkele out-of-the-box-filters die u kunt gebruiken en u kunt ook uw eigen aangepaste filters schrijven.

De out-of-the-box-filters zijn onder andere:

* Ernst traceerniveau
* Specifieke URL's, trefwoorden of responscodes
* Snel op netwerkaanvragen - dat wil zeggen, aanvragen waarop uw app heeft gereageerd op snel
* Namen van de specifieke gebeurtenis

> [!NOTE]
> Filters scheeftrekken de metrische gegevens van uw app. U kunt bijvoorbeeld besluiten dat een filter te verwijderen van snelle responstijden om te kunnen vaststellen trage reacties, wordt ingesteld. Maar u moet zich op de hoogte dat de gemiddelde reactietijden gerapporteerd door Application Insights vervolgens lager dan de waarde true snelheid zijn en het aantal aanvragen kleiner dan het werkelijke aantal is.
> Als dit een probleem is, gebruikt u [steekproeven](app-insights-sampling.md) in plaats daarvan.

## <a name="setting-filters"></a>Filters instellen

Voeg ApplicationInsights.xml, een `TelemetryProcessors` sectie zoals in dit voorbeeld:


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want to see -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




[Inspecteer de volledige set van processors die zijn ingebouwd](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).

## <a name="built-in-filters"></a>Als u ingebouwde filters

### <a name="metric-telemetry-filter"></a>Metrische telemetrie filteren

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* `NotNeeded` -Met door komma's gescheiden lijst met namen van aangepaste metrische gegevens.


### <a name="page-view-telemetry-filter"></a>Telemetrie van paginaweergaven filter

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* `DurationThresholdInMS` -Duur verwijst naar de tijd die nodig om de pagina te laden. Als deze optie is ingesteld, worden pagina's die sneller dan de opgegeven tijd geladen niet gerapporteerd.
* `NotNeededNames` -Met door komma's gescheiden lijst met paginanamen.
* `NotNeededUrls` -Met door komma's gescheiden lijst met URL-fragmenten. Bijvoorbeeld, `"home"` gefilterd op alle pagina's met "home" in de URL.


### <a name="request-telemetry-filter"></a>Telemetrie filteren aanvragen


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a>Synthetische bronfilter

Alle telemetrie met waarden in de eigenschap SyntheticSource gefilterd. Het gaat hierbij om aanvragen van bots, spiders en beschikbaarheidstests.

Telemetrie voor alle synthetische aanvragen filteren:


```XML

           <Processor type="SyntheticSourceFilter" />
```

Telemetrie voor specifieke synthetische bronnen filteren:


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* `NotNeeded` -Met door komma's gescheiden lijst met namen van synthetische gegevensbronnen.

### <a name="telemetry-event-filter"></a>Gebeurtenisfilter voor telemetrie

Aangepaste gebeurtenissen gefilterd (met behulp van het logboek geregistreerd [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* `NotNeededNames` -Met door komma's gescheiden lijst met gebeurtenisnamen.


### <a name="trace-telemetry-filter"></a>Tracering telemetrie filteren

Logtraceringen-filters (met behulp van het logboek geregistreerd [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) of een [logboekregistratie framework collector](app-insights-java-trace-logs.md)).

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* `FromSeverityLevel` Geldige waarden zijn:
 *  UIT; alle traces filteren
 *  TRACE - er wordt geen filtering. gelijk aan het traceringsniveau
 *  INFO - uitfilteren traceerniveau
 *  Waarschuwing - Filter TRACE en informatie
 *  Fout - uitfilteren waarschuwen, gegevens, TRACERING
 *  KRITIEKE - filter om alle essentiële


## <a name="custom-filters"></a>Aangepaste filters

### <a name="1-code-your-filter"></a>1. Code van uw filter

Maak een klasse die wordt geïmplementeerd in uw code `TelemetryProcessor`:

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required to support the filter.*/
       private final String successful;

       /* Initializers for the parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry to be sent.
          Return false to discard it.
          Return true to allow other processors to inspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-the-configuration-file"></a>2. Het filter in het configuratiebestand aanroepen

In ApplicationInsights.xml:

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

## <a name="troubleshooting"></a>Problemen oplossen

*Mijn filter werkt niet.*

* Controleer dat u geldige parameterwaarden hebt opgegeven. Bijvoorbeeld, de duur moeten gehele getallen zijn. Ongeldige waarden zorgt ervoor dat het filter moet worden genegeerd. Als uw aangepaste filter een uitzondering van een constructor of set-methode genereert, wordt dit genegeerd.

## <a name="next-steps"></a>Volgende stappen

* [Sampling](app-insights-sampling.md) -steekproeven als alternatief die heeft niet de metrische gegevens over scheeftrekken overwegen.
