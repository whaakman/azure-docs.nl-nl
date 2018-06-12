---
title: Azure Application Insights-gegevensmodel | Microsoft Docs
description: Beschrijft de eigenschappen van continue export in de JSON geëxporteerd en gebruikt als filters.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: mbullwin
ms.openlocfilehash: ee6597b78ac8de8fc3a7f3796010f22919243b23
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294891"
---
# <a name="application-insights-export-data-model"></a>Application Insights Export-gegevensmodel
Deze tabel bevat de eigenschappen van telemetrie verzonden vanaf de [Application Insights](app-insights-overview.md) SDK's naar de portal.
Ziet u deze eigenschappen in de gegevensuitvoer van [continue Export](app-insights-export-telemetry.md).
Ze worden ook weergegeven in de eigenschappenfilters in [metriek Explorer](app-insights-metrics-explorer.md) en [diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md).

Verwijst naar Let op:

* `[0]` Geeft een punt in het pad waar u moet een index; invoegen in deze tabellen. maar het is niet altijd 0.
* Tijdsduren zijn in tienden van een microseconden, dus 10000000 == 1 seconde.
* Datums en tijden UTC zijn en zijn opgegeven in de ISO-indeling `yyyy-MM-DDThh:mm:ss.sssZ`


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
Alle typen telemetrie vergezeld gaan van een sectie context. Niet al deze velden zijn verzonden met elk gegevenspunt.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| context.Custom.Dimensions [0] |object] |Tekenreeks van sleutel / waarde-paren ingesteld door aangepaste eigenschappen-parameter. Maximale sleutellengte 100, waarden voor de maximale lengte van 1024. Meer dan 100 unieke waarden, de eigenschap kan worden doorzocht, maar kan niet worden gebruikt voor de segmentering. Maximaal 200 sleutels per ikey. |
| context.Custom.metrics [0] |object] |Sleutel-waardeparen ingesteld door de parameter aangepaste metingen en TrackMetrics. Maximale sleutellengte 100 waarden mogen alleen cijfers bevatten. |
| context.data.eventTime |tekenreeks |UTC |
| context.data.isSynthetic |booleaans |Er verschijnt een aanvraag afkomstig zijn van een bot of web-test. |
| context.data.samplingRate |getal |Percentage van de telemetrie die is gegenereerd door de SDK die wordt verzonden naar de portal. Het bereik 0,0 100,0. |
| context.Device |object |Client-apparaat |
| context.Device.browser |tekenreeks |IE, Chrome... |
| context.device.browserVersion |tekenreeks |Chrome 48,0... |
| context.device.deviceModel |tekenreeks | |
| context.device.deviceName |tekenreeks | |
| context.Device.id |tekenreeks | |
| context.Device.locale |tekenreeks |NL-GB, nl-nl... |
| context.Device.Network |tekenreeks | |
| context.device.oemName |tekenreeks | |
| context.device.osVersion |tekenreeks |Host-OS |
| context.device.roleInstance |tekenreeks |ID van server-host |
| context.device.roleName |tekenreeks | |
| context.Device.type |tekenreeks |PC, Browser... |
| context.Location |object |Client-IP is afgeleid. |
| context.Location.City |tekenreeks |Afgeleid van de client-IP, indien bekend |
| context.Location.clientip |tekenreeks |Laatste achthoek is geanonimiseerde op 0. |
| context.Location.continent |tekenreeks | |
| context.Location.Country |tekenreeks | |
| context.Location.Province |tekenreeks |Provincie |
| context.Operation.id |tekenreeks |Items die dezelfde bewerking-id worden als betrokken Items in de portal weergegeven. Meestal de aanvraag-id. |
| context.Operation.name |tekenreeks |URL of de aanvraag |
| context.operation.parentId |tekenreeks |Kan geneste verwante items. |
| context.Session.id |tekenreeks |Id van een groep bewerkingen uit dezelfde bron. Een periode van 30 minuten zonder een bewerking geeft het einde van een sessie. |
| context.session.isFirst |booleaans | |
| context.user.accountAcquisitionDate |tekenreeks | |
| context.user.anonAcquisitionDate |tekenreeks | |
| context.user.anonId |tekenreeks | |
| context.user.authAcquisitionDate |tekenreeks |[Geverifieerde gebruiker](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated |booleaans | |
| internal.data.documentVersion |tekenreeks | |
| Internal.Data.id |tekenreeks | Unieke id die wordt toegewezen wanneer een item wordt ingenomen naar Application Insights |

## <a name="events"></a>Gebeurtenissen
Aangepaste gebeurtenissen die worden gegenereerd door [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| aantal gebeurtenissen [0] |geheel getal |100 / ([steekproeven](app-insights-sampling.md) snelheid). Voorbeeld 4 =&gt; 25%. |
| naam van de gebeurtenis [0] |tekenreeks |De naam van de gebeurtenis.  De maximale lengte 250. |
| url van de gebeurtenis [0] |tekenreeks | |
| de gebeurtenis [0] urlData.base |tekenreeks | |
| de gebeurtenis [0] urlData.host |tekenreeks | |

## <a name="exceptions"></a>Uitzonderingen
Rapporten [uitzonderingen](app-insights-asp-net-exceptions.md) op de server en in de browser.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| de assembly [0] basicException |tekenreeks | |
| aantal basicException [0] |geheel getal |100 / ([steekproeven](app-insights-sampling.md) snelheid). Voorbeeld 4 =&gt; 25%. |
| exceptionGroup basicException [0] |tekenreeks | |
| exceptionType basicException [0] |tekenreeks | |
| failedUserCodeMethod basicException [0] |tekenreeks | |
| failedUserCodeAssembly basicException [0] |tekenreeks | |
| handledAt basicException [0] |tekenreeks | |
| hasFullStack basicException [0] |booleaans | |
| basicException [0]-id |tekenreeks | |
| de methode basicException [0] |tekenreeks | |
| bericht basicException [0] |tekenreeks |Bericht van uitzondering. De maximale lengte 10k. |
| outerExceptionMessage basicException [0] |tekenreeks | |
| outerExceptionThrownAtAssembly basicException [0] |tekenreeks | |
| outerExceptionThrownAtMethod basicException [0] |tekenreeks | |
| outerExceptionType basicException [0] |tekenreeks | |
| outerId basicException [0] |tekenreeks | |
| basicException [0] [0] parsedStack assembly |tekenreeks | |
| basicException [0] [0] parsedStack fileName |tekenreeks | |
| basicException [0] [0] parsedStack niveau |geheel getal | |
| basicException [0] [0] parsedStack regel |geheel getal | |
| basicException [0] [0] parsedStack methode |tekenreeks | |
| stack basicException [0] |tekenreeks |Max. lengte 10k |
| typeName basicException [0] |tekenreeks | |

## <a name="trace-messages"></a>Traceringsberichten
Verzonden door [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), en door de [logboekregistratie adapters](app-insights-asp-net-trace-logs.md).

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| Loggernaam bericht [0] |tekenreeks | |
| [0]-berichtparameters |tekenreeks | |
| onbewerkte bericht [0] |tekenreeks |Het logboekbericht maximumlengte 10k. |
| bericht [0], foutcode |tekenreeks | |

## <a name="remote-dependency"></a>Externe afhankelijkheden
Door TrackDependency verzonden. Gebruikt voor het rapportprestaties en het gebruik van [aanroepen van afhankelijkheden](app-insights-asp-net-dependencies.md) in de server en AJAX-aanroepen in de browser.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| asynchrone remoteDependency [0] |booleaans | |
| baseName remoteDependency [0] |tekenreeks | |
| commandName remoteDependency [0] |tekenreeks |Bijvoorbeeld "Start/index" |
| aantal remoteDependency [0] |geheel getal |100 / ([steekproeven](app-insights-sampling.md) snelheid). Voorbeeld 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] |tekenreeks |HTTP, SQL... |
| durationMetric.value remoteDependency [0] |getal |Tijd voor voltooiing van het antwoord van afhankelijkheid aanroep |
| remoteDependency [0]-id |tekenreeks | |
| de naam van de remoteDependency [0] |tekenreeks |De URL. De maximale lengte 250. |
| resultCode remoteDependency [0] |tekenreeks |van afhankelijkheid van HTTP |
| remoteDependency [0] geslaagd |booleaans | |
| type remoteDependency [0] |tekenreeks |HTTP, Sql... |
| de url remoteDependency [0] |tekenreeks |Max. lengte 2000 |
| urlData.base remoteDependency [0] |tekenreeks |Max. lengte 2000 |
| urlData.hashTag remoteDependency [0] |tekenreeks | |
| urlData.host remoteDependency [0] |tekenreeks |Max. lengte 200 |

## <a name="requests"></a>Aanvragen
Verzonden door [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest). De standaard-modules gebruiken deze om rapporten serverreactietijd, gemeten op de server.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| aantal verzoeken [0] |geheel getal |100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld: 4 =&gt; 25%. |
| aanvraag [0] durationMetric.value |getal |Tijd van aanvraag aan antwoord die binnenkomen. 1e7 == 1s |
| [0] aanvraag-id |tekenreeks |Bewerkings-id |
| de Aanvraagnaam van de [0] |tekenreeks |GET/POST + url-basis.  Max. lengte 250 |
| aanvraag [0] responseCode |geheel getal |HTTP-antwoord verzonden naar client |
| aanvraag [0] geslaagd |booleaans |Standaard == (responseCode &lt; 400) |
| [0] aanvraag-url |tekenreeks |Met uitzondering van host |
| aanvraag [0] urlData.base |tekenreeks | |
| aanvraag [0] urlData.hashTag |tekenreeks | |
| aanvraag [0] urlData.host |tekenreeks | |

## <a name="page-view-performance"></a>Prestaties van paginaweergave
Door de browser wordt verzonden. Hiermee wordt de tijdsduur voor het verwerken van een pagina van gebruiker voor het initiëren van de aanvraag om weer te geven (met uitzondering van asynchrone AJAX-aanroepen) voltooid.

Context waarden weergeven clientbesturingssysteem en browserversie.

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| clientProcess.value clientPerformance [0] |geheel getal |Tijd van het einde van het ontvangen van de HTML-code op de weergave van de pagina. |
| de naam van de clientPerformance [0] |tekenreeks | |
| networkConnection.value clientPerformance [0] |geheel getal |De tijd die nodig is voor een netwerkverbinding tot stand brengen. |
| receiveRequest.value clientPerformance [0] |geheel getal |Tijd van het einde van de aanvraag wordt verzonden naar de HTML-code in antwoord ontvangen. |
| sendRequest.value clientPerformance [0] |geheel getal |Van tijd die nodig is voor het verzenden van de HTTP-aanvraag. |
| total.value clientPerformance [0] |geheel getal |Tijd van begint met het verzenden van de aanvraag naar de pagina om weer te geven. |
| de url clientPerformance [0] |tekenreeks |URL van deze aanvraag |
| urlData.base clientPerformance [0] |tekenreeks | |
| urlData.hashTag clientPerformance [0] |tekenreeks | |
| urlData.host clientPerformance [0] |tekenreeks | |
| urlData.protocol clientPerformance [0] |tekenreeks | |

## <a name="page-views"></a>Paginaweergaven
Verzonden door trackPageView() of [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| aantal paginaweergaven [0] |geheel getal |100 / ([steekproeven](app-insights-sampling.md) snelheid). Voorbeeld 4 =&gt; 25%. |
| weergave [0] durationMetric.value |geheel getal |Waarde desgewenst instellen in trackPageView() of startTrackPage() - stopTrackPage(). Niet hetzelfde zijn als clientPerformance waarden. |
| Weergavenaam [0] |tekenreeks |Paginatitel.  Max. lengte 250 |
| url weergeven [0] |tekenreeks | |
| weergave [0] urlData.base |tekenreeks | |
| weergave [0] urlData.hashTag |tekenreeks | |
| weergave [0] urlData.host |tekenreeks | |

## <a name="availability"></a>Beschikbaarheid
Rapporten [webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md).

| Pad | Type | Opmerkingen |
| --- | --- | --- |
| beschikbaarheid [0] availabilityMetric.name |tekenreeks |availability |
| beschikbaarheid [0] availabilityMetric.value |getal |1.0 of 0,0 |
| telling beschikbaarheid [0] |geheel getal |100 / ([steekproeven](app-insights-sampling.md) snelheid). Voorbeeld 4 =&gt; 25%. |
| beschikbaarheid [0] dataSizeMetric.name |tekenreeks | |
| beschikbaarheid [0] dataSizeMetric.value |geheel getal | |
| beschikbaarheid [0] durationMetric.name |tekenreeks | |
| beschikbaarheid [0] durationMetric.value |getal |De duur van de test. 1e7 == 1s |
| beschikbaarheidsbericht [0] |tekenreeks |Diagnostische is mislukt |
| resultaat van de beschikbaarheid [0] |tekenreeks |Doorslaggevend |
| beschikbaarheid [0] runLocation |tekenreeks |Geo-bron van de HTTP-aanvragen |
| beschikbaarheid [0] testName |tekenreeks | |
| beschikbaarheid [0] testRunId |tekenreeks | |
| beschikbaarheid [0] testTimestamp |tekenreeks | |

## <a name="metrics"></a>Metrische gegevens
Gegenereerd door TrackMetric().

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

## <a name="about-metric-values"></a>Over metrische waarden
Metrische waarden, zowel in metrische rapporten en ergens anders worden gemeld met de structuur van een standaard-object. Bijvoorbeeld:

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

Momenteel - maar dit kan worden gewijzigd in de toekomst: bij alle waarden die zijn gerapporteerd door de standaard SDK-modules `count==1` en alleen de `name` en `value` velden zijn nuttig. Het enige geval waar zij verschillende worden zou zou zijn als u uw eigen aanroepen TrackMetric schrijven in waarmee u de andere parameters instellen.

Het doel van de andere velden is dat de metrische gegevens worden geaggregeerd in de SDK om verkeer te beperken tot de portal. U kan bijvoorbeeld meerdere opeenvolgende metingen gemiddelde voordat elk rapport metrische gegevens worden verzonden. Vervolgens wilt u de min, max, standaardafwijking en cumulatieve waarde (sum of gemiddelde) berekenen en aantal ingesteld op het aantal waarden dat wordt vertegenwoordigd door het rapport.

In de tabellen, hebben we de velden zelden gebruikt count, min, max, stdDev en sampledValue weggelaten.

U kunt gebruiken in plaats van vooraf verzamelen meetgegevens [steekproeven](app-insights-sampling.md) als u wilt beperken van het volume van telemetrie.

### <a name="durations"></a>Duur
Duur worden weergegeven in tienden van een microseconden, tenzij anders vermeld, zodat 10000000.0 betekent 1 seconde dat.

## <a name="see-also"></a>Zie ook
* [Application Insights](app-insights-overview.md)
* [Continue Export](app-insights-export-telemetry.md)
* [Codevoorbeelden](app-insights-export-telemetry.md#code-samples)
