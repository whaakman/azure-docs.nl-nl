---
title: Azure Application Insights telemetrie in uw Java-web-app filteren | Microsoft Docs
description: Verminderen het verkeer van telemetrie door het filteren van de gebeurtenissen die u hoeft niet te bewaken.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbullwin
ms.openlocfilehash: f9e061c010667bc18ac54e6546cc25339e9c0e3e
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a>Filteren van telemetrie in uw Java-web-app

Filters bieden een manier om de telemetrie selecteren die uw [Java-web-app naar Application Insights verzendt](app-insights-java-get-started.md). Er zijn een aantal out-of-the-box-filters die u kunt gebruiken en u kunt ook uw eigen aangepaste filters schrijven.

De out-of-the-box-filters omvatten:

* Traceerniveau ernst
* Specifieke URL's, trefwoorden of reactiecodes
* Snel op netwerkaanvragen - aanvragen dat wil zeggen, waarop uw app heeft geantwoord op snel
* Namen van de specifieke gebeurtenis

> [!NOTE]
> Filters leiden tot onjuiste de metrische gegevens van uw app. U kunt bijvoorbeeld besluiten dat een filter te verwijderen van snelle responstijden om op te sporen trage reacties, wordt ingesteld. Maar u moet rekening houden dat de gemiddelde reactietijd van gerapporteerd door de Application Insights vervolgens lager dan de waarde true snelheid zijn en het aantal aanvragen wordt niet kleiner zijn dan het werkelijke aantal.
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




[Inspecteer de volledige set van ingebouwde processors](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).

## <a name="built-in-filters"></a>Ingebouwde filters

### <a name="metric-telemetry-filter"></a>Metrische telemetrie-filter

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* `NotNeeded`-Met door komma's gescheiden lijst met aangepaste metrische namen.


### <a name="page-view-telemetry-filter"></a>Filter telemetrie van paginaweergaven

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* `DurationThresholdInMS`-Duur verwijst naar de tijd om de pagina te laden. Als deze optie is ingesteld, worden pagina's die sneller dan de opgegeven tijd wordt geladen niet gerapporteerd.
* `NotNeededNames`-Met door komma's gescheiden lijst met paginanamen.
* `NotNeededUrls`-Door komma's gescheiden lijst met URL-fragmenten. Bijvoorbeeld: `"home"` gefilterd op alle pagina's die u hebt "home" in de URL.


### <a name="request-telemetry-filter"></a>Telemetrie-Filter aanvragen


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

* `NotNeeded`-Met door komma's gescheiden lijst met namen van synthetische gegevensbronnen.

### <a name="telemetry-event-filter"></a>Gebeurtenisfilter telemetrie

Aangepaste gebeurtenissen gefilterd (vastgelegd met behulp van [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* `NotNeededNames`-Met door komma's gescheiden lijst met gebeurtenisnamen.


### <a name="trace-telemetry-filter"></a>Tracering telemetrie filter

Meld u filters traceringen (vastgelegd met behulp van [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) of een [logboekregistratie framework collector](app-insights-java-trace-logs.md)).

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* `FromSeverityLevel`geldige waarden zijn:
 *  BUITEN - alle traceringen filteren
 *  TRACE - geen filters. is gelijk aan het traceringsniveau
 *  INFO - uitfilteren traceerniveau
 *  Waarschuwen - uitfilteren TRACE en gegevens
 *  Fout - uitfilteren waarschuwen, INFO, traceren
 *  KRITIEKE - filter uit alle kritieke


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


### <a name="2-invoke-your-filter-in-the-configuration-file"></a>2. Aanroepen van het filter in het configuratiebestand

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

* Controleer of u geldige parameterwaarden hebt opgegeven. Duur moet bijvoorbeeld gehele getallen zijn. Ongeldige waarden zorgt ervoor dat het filter moet worden genegeerd. Als u een aangepaste filter er een uitzondering gegenereerd vanuit een constructor of een set-methode, worden genegeerd.

## <a name="next-steps"></a>Volgende stappen

* [Steekproef nemen](app-insights-sampling.md) -steekproeven als alternatief die niet leiden tot metrische gegevens over uw onjuiste komt overwegen.
