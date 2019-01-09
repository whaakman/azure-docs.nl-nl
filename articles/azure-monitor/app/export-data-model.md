---
title: Azure Application Insights-gegevensmodel | Microsoft Docs
description: Beschrijving van eigenschappen van continue export in de JSON hebt geëxporteerd en gebruikt als filters.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: mbullwin
ms.openlocfilehash: 12025dfb93bbcfc86ae301f8fb63e7ac74697cf2
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119269"
---
# <a name="application-insights-export-data-model"></a>Application Insights Export Data Model
Deze tabel bevat de eigenschappen van telemetrie verzonden vanaf de [Application Insights](../../azure-monitor/app/app-insights-overview.md) SDK's aan de portal.
U ziet deze eigenschappen in de gegevensuitvoer van [continue Export](export-telemetry.md).
Ze worden ook weergegeven in de eigenschappenfilters in [Metric Explorer](../../azure-monitor/app/metrics-explorer.md) en [diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md).

Die u moet weten:

* `[0]` in deze tabel geeft een punt in het pad waar u moet een index; invoegen maar het is niet altijd 0.
* Tijdsduur van de zijn in tienden van een microseconde, dus 10000000 == 1 seconde.
* Datums en tijden zijn UTC en zijn vermeld in de ISO-indeling `yyyy-MM-DDThh:mm:ss.sssZ`


## <a name="example"></a>Voorbeeld
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }

## <a name="context"></a>Context
Alle typen telemetrie wordt gecombineerd met een sectie context. Niet al deze velden worden verzonden met elk gegevenspunt.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| context.Custom.Dimensions [0] |object] |Sleutel / waarde-tekenreeks-paren die zijn ingesteld door de parameter aangepaste eigenschappen. Maximale sleutellengte 100, waarden voor de maximale lengte van 1024. Meer dan 100 unieke waarden, de eigenschap kan worden doorzocht, maar kan niet worden gebruikt voor de segmentering. Maximaal 200 sleutels per ikey. |
| context.Custom.metrics [0] |object] |Sleutel / waarde-paren ingesteld door de parameter aangepaste metingen en TrackMetrics. Maximale sleutellengte 100 waarden mogen alleen cijfers bevatten. |
| context.data.eventTime |string |UTC |
| context.data.isSynthetic |booleaans |Er verschijnt een aanvraag afkomstig zijn van een bot of web-test. |
| context.data.samplingRate |getal |Percentage van de telemetrie die is gegenereerd door de SDK die wordt verzonden naar de portal. Het bereik 0,0 100,0. |
| context.Device |object |Client-apparaat |
| context.Device.browser |string |IE, Chrome... |
| context.device.browserVersion |string |Chrome 48,0... |
| context.device.deviceModel |string | |
| context.device.deviceName |string | |
| context.Device.id |string | |
| context.Device.locale |string |en-GB, nl-nl... |
| context.Device.Network |string | |
| context.device.oemName |string | |
| context.Device.OS |string | |
| context.device.osVersion |string |Hostbesturingssysteem |
| context.device.roleInstance |string |ID van server-host |
| context.device.roleName |string | |
| context.device.screenResolution |string | |
| context.Device.type |string |PC, Browser... |
| context.Location |object |Client-IP is afgeleid. |
| context.Location.City |string |Afgeleid van client-IP, indien bekend |
| context.Location.clientip |string |Laatste achthoek zijn geanonimiseerd op 0. |
| context.Location.continent |string | |
| context.Location.Country |string | |
| context.Location.Province |string |Staat of provincie |
| context.Operation.id |string |Items die dezelfde bewerking-id worden weergegeven als verwante Items in de portal. Meestal de aanvraag-id. |
| context.Operation.name |string |URL of de aanvraag |
| context.operation.parentId |string |Kan de geneste verwante items. |
| context.Session.id |string |Id van een groep van bewerkingen van dezelfde bron. Een periode van 30 minuten zonder een bewerking geeft aan het einde van een sessie. |
| context.session.isFirst |booleaans | |
| context.user.accountAcquisitionDate |string | |
| context.user.accountId |string | |
| context.user.anonAcquisitionDate |string | |
| context.user.anonId |string | |
| context.user.authAcquisitionDate |string |[Geverifieerde gebruiker](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users) |
| context.user.authId |string | |
| context.user.isAuthenticated |booleaans | |
| context.user.storeRegion |string | |
| internal.data.documentVersion |string | |
| Internal.Data.id |string | Unieke id die wordt toegewezen wanneer een item wordt opgenomen naar Application Insights |

## <a name="events"></a>Gebeurtenissen
Aangepaste gebeurtenissen die worden gegenereerd door [TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| aantal gebeurtenissen [0] |geheel getal |100 / ([steekproeven](../../azure-monitor/app/sampling.md) tarief). Voorbeeld 4 =&gt; 25%. |
| de naam van de gebeurtenis [0] |string |De naam van de gebeurtenis.  Maximale lengte van 250. |
| url van de gebeurtenis [0] |string | |
| gebeurtenis [0] urlData.base |string | |
| gebeurtenis [0] urlData.host |string | |

## <a name="exceptions"></a>Uitzonderingen
Rapporten [uitzonderingen](../../azure-monitor/app/asp-net-exceptions.md) op de server en in de browser.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| de assembly basicException [0] |string | |
| aantal basicException [0] |geheel getal |100 / ([steekproeven](../../azure-monitor/app/sampling.md) tarief). Voorbeeld 4 =&gt; 25%. |
| exceptionGroup basicException [0] |string | |
| exceptionType basicException [0] |string | |
| failedUserCodeMethod basicException [0] |string | |
| failedUserCodeAssembly basicException [0] |string | |
| handledAt basicException [0] |string | |
| hasFullStack basicException [0] |booleaans | |
| basicException [0]-id |string | |
| methode basicException [0] |string | |
| bericht basicException [0] |string |Bericht van uitzondering. Maximale lengte van 10 kB. |
| outerExceptionMessage basicException [0] |string | |
| outerExceptionThrownAtAssembly basicException [0] |string | |
| outerExceptionThrownAtMethod basicException [0] |string | |
| outerExceptionType basicException [0] |string | |
| outerId basicException [0] |string | |
| basicException [0] [0] parsedStack assembly |string | |
| basicException [0] [0] parsedStack bestandsnaam |string | |
| basicException [0] [0] parsedStack niveau |geheel getal | |
| basicException [0] [0] parsedStack regel |geheel getal | |
| basicException [0] [0] parsedStack methode |string | |
| stack basicException [0] |string |Max. lengte 10k |
| typeName basicException [0] |string | |

## <a name="trace-messages"></a>Traceringsberichten
Verzonden door [TrackTrace](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace), en door de [logboekregistratie adapters](../../azure-monitor/app/asp-net-trace-logs.md).

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| bericht [0] Loggernaam |string | |
| [0]-berichtparameters |string | |
| bericht [0] onbewerkte |string |Het logboekbericht maximumlengte 10k. |
| Foutcode bericht [0] |string | |

## <a name="remote-dependency"></a>Externe afhankelijkheden
Verzonden door TrackDependency. Gebruikt voor het rapportprestaties en het gebruik van [aanroepen van afhankelijkheden](../../azure-monitor/app/asp-net-dependencies.md) in de server en AJAX-aanroepen in de browser.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| asynchrone remoteDependency [0] |booleaans | |
| baseName remoteDependency [0] |string | |
| Opdrachtnaam remoteDependency [0] |string |Bijvoorbeeld "home/index" |
| aantal remoteDependency [0] |geheel getal |100 / ([steekproeven](../../azure-monitor/app/sampling.md) tarief). Voorbeeld 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] |string |HTTP, SQL... |
| durationMetric.value remoteDependency [0] |getal |Tijd van de aanroep van antwoord met de afhankelijkheid is voltooid |
| remoteDependency [0]-id |string | |
| de naam van de remoteDependency [0] |string |De URL. Maximale lengte van 250. |
| resultCode remoteDependency [0] |string |van HTTP-afhankelijkheid |
| remoteDependency [0] geslaagd |booleaans | |
| type remoteDependency [0] |string |HTTP, Sql... |
| url van remoteDependency [0] |string |Max. lengte 2000 |
| urlData.base remoteDependency [0] |string |Max. lengte 2000 |
| urlData.hashTag remoteDependency [0] |string | |
| urlData.host remoteDependency [0] |string |Max. lengte 200 |

## <a name="requests"></a>Aanvragen
Verzonden door [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest). De standard-modules gebruiken deze om rapporten serverreactietijd, gemeten op de server.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| aantal aanvragen [0] |geheel getal |100 / ([steekproeven](../../azure-monitor/app/sampling.md) tarief). Bijvoorbeeld: 4 =&gt; 25%. |
| aanvraag [0] durationMetric.value |getal |Tijd van aanvraag dat binnenkomt op antwoord. 1e7 1s == |
| aanvraag [0]-id |string |Bewerkings-id |
| de Aanvraagnaam van de [0] |string |GET/POST + basis-url.  Maximale lengte van 250 |
| aanvraag [0] responseCode |geheel getal |HTTP-antwoord verzonden naar client |
| aanvraag [0]-geslaagd |booleaans |Standaard == (responseCode &lt; 400) |
| aanvraag [0]-url |string |Niet met inbegrip van host |
| aanvraag [0] urlData.base |string | |
| aanvraag [0] urlData.hashTag |string | |
| aanvraag [0] urlData.host |string | |

## <a name="page-view-performance"></a>Prestaties van paginaweergave
Verzonden door de browser. Hiermee wordt de tijd voor het verwerken van een pagina van de gebruiker de aanvraag om weer te geven voltooid (met uitzondering van asynchrone AJAX-aanroepen).

Context waarden weergeven clientbesturingssysteem en browserversie.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| clientProcess.value clientPerformance [0] |geheel getal |Tijd van het einde van het ontvangen van de HTML-code voor het weergeven van de pagina. |
| de naam van de clientPerformance [0] |string | |
| networkConnection.value clientPerformance [0] |geheel getal |Gebruikte tijd voor een netwerkverbinding tot stand brengen. |
| receiveRequest.value clientPerformance [0] |geheel getal |Tijd van het einde van de aanvraag te verzenden naar de HTML-code in antwoord ontvangen. |
| sendRequest.value clientPerformance [0] |geheel getal |Tijd van genomen om de HTTP-aanvraag te verzenden. |
| total.value clientPerformance [0] |geheel getal |De tijd niet kan worden gestart voor het verzenden van de aanvraag voor het weergeven van de pagina. |
| url van clientPerformance [0] |string |URL van deze aanvraag |
| urlData.base clientPerformance [0] |string | |
| urlData.hashTag clientPerformance [0] |string | |
| urlData.host clientPerformance [0] |string | |
| urlData.protocol clientPerformance [0] |string | |

## <a name="page-views"></a>Paginaweergaven
Verzonden door trackPageView() of [stopTrackPage](../../azure-monitor/app/api-custom-events-metrics.md#page-views)

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| aantal paginaweergaven [0] |geheel getal |100 / ([steekproeven](../../azure-monitor/app/sampling.md) tarief). Voorbeeld 4 =&gt; 25%. |
| durationMetric.value weergeven [0] |geheel getal |Waarde (optioneel) instellen in trackPageView() of startTrackPage() - stopTrackPage(). Niet gelijk zijn aan clientPerformance waarden. |
| Weergavenaam [0] |string |Paginatitel.  Maximale lengte van 250 |
| url weergeven [0] |string | |
| urlData.base weergeven [0] |string | |
| urlData.hashTag weergeven [0] |string | |
| urlData.host weergeven [0] |string | |

## <a name="availability"></a>Beschikbaarheid
Rapporten [webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md).

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| beschikbaarheid [0] availabilityMetric.name |string |availability |
| beschikbaarheid [0] availabilityMetric.value |getal |1.0 of 0,0 |
| telling beschikbaarheid [0] |geheel getal |100 / ([steekproeven](../../azure-monitor/app/sampling.md) tarief). Voorbeeld 4 =&gt; 25%. |
| beschikbaarheid [0] dataSizeMetric.name |string | |
| beschikbaarheid [0] dataSizeMetric.value |geheel getal | |
| beschikbaarheid [0] durationMetric.name |string | |
| beschikbaarheid [0] durationMetric.value |getal |Duur van de test. 1e7 1s == |
| bericht over beschikbaarheid [0] |string |Diagnose-fout |
| resultaat van beschikbaarheid [0] |string |Geslaagd/mislukt |
| beschikbaarheid [0] runLocation |string |Geo-bron van HTTP-aanvragen |
| beschikbaarheid [0] testName |string | |
| beschikbaarheid [0] testRunId |string | |
| beschikbaarheid [0] testTimestamp |string | |

## <a name="metrics"></a>Metrische gegevens
Door TrackMetric() gegenereerd.

De metrische waarde is gevonden in context.custom.metrics[0]

Bijvoorbeeld:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Over de metrische waarden
Metrische waarden, zowel in de rapporten met metrische gegevens en ergens anders worden gerapporteerd met de structuur van een standaard-object. Bijvoorbeeld:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Op dit moment - maar dit kan worden gewijzigd in de toekomst - alle waarden die zijn gerapporteerd door de standaard SDK-modules, `count==1` en alleen de `name` en `value` velden zijn nuttig. Het enige geval waarbij ze verschillende worden kunnen zou zijn als u uw eigen aanroepen TrackMetric schrijven in waarop u de andere parameters instellen.

Het doel van de andere velden is om toe te staan van metrische gegevens kunnen worden samengevoegd in de SDK, om verkeer te beperken tot de portal. U kan bijvoorbeeld verschillende opeenvolgende metingen gemiddelde voordat elk rapport met metrische gegevens worden verzonden. Vervolgens zou u de min, max, standaarddeviatie en aggregatiewaarde (som of het gemiddelde) berekenen en aantal ingesteld op het aantal, serverlogs, vertegenwoordigd door het rapport.

In de bovenstaande tabellen hebben we de velden zelden gebruikt count, min, max, stdDev en sampledValue weggelaten.

In plaats van vooraf verzamelen metrische gegevens, kunt u [steekproeven](../../azure-monitor/app/sampling.md) als u nodig hebt om te verminderen de hoeveelheid telemetrie.

### <a name="durations"></a>Duur
Duur worden weergegeven in tienden van een wachttijden van microseconden, tenzij anders vermeld, zodat 10000000.0 betekent 1 seconde dat.

## <a name="see-also"></a>Zie ook
* [Application Insights](../../azure-monitor/app/app-insights-overview.md)
* [Continue Export](export-telemetry.md)
* [Codevoorbeelden](export-telemetry.md#code-samples)
