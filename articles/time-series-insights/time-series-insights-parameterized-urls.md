---
title: Aangepaste weergaven van Share Azure Time Series Insights delen via geparameteriseerde URL's | Microsoft Docs
description: In dit artikel wordt beschreven hoe u geparameteriseerde URL's samenstelt in Azure Time Series Insights, zodat een aangepaste weergave eenvoudig kan worden gedeeld.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: conceptual
ms.workload: big-data
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: dfc04397b1d7e9f3256810cbe469067ae52c99bd
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66238978"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Een aangepaste weergave delen via een geparameteriseerde URL

Voor het delen van een aangepaste weergave in Time Series Insights Explorer, kunt u via programmacode een geparameteriseerde URL van de aangepaste weergave maken.

De Verkenner van Time Series Insights ondersteunt URL-queryparameters om weergaven in de ervaring van rechtstreeks vanuit de URL opgeven. U kunt bijvoorbeeld via de URL een doelomgeving, een zoekpredicaat en de gewenste tijdspanne opgeven. Wanneer een gebruiker op de aangepaste URL klikt, biedt de interface een directe koppeling naar dat asset in de Time Series Insights-portal. Beleid voor gegevenstoegang wordt toegepast.

> [!TIP]
> * Bekijk de gratis [Time Series Insights-demo](https://insights.timeseries.azure.com/samples).
> * Lees het bijbehorende [Time Series Insights Explorer](./time-series-insights-explorer.md) documentatie.

## <a name="environment-id"></a>Omgevings-id

De parameter `environmentId=<guid>` geeft de id van de doelomgeving op. Het is een onderdeel van de FQDN voor gegevenstoegang en u vindt deze in de rechterbovenhoek van het omgevingsoverzicht in Azure portal. Dit is alles wat voorafgaat aan `env.timeseries.azure.com`.

Een voorbeeld van de parameter voor omgevings-id is `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Time

U kunt absolute of relatieve tijdwaarden opgeven met een geparameteriseerde URL.

### <a name="absolute-time-values"></a>Absolute tijdwaarden

Voor absolute tijdwaarden gebruikt u de parameters `from=<integer>` en `to=<integer>`.

* `from=<integer>` is een waarde in JavaScript-milliseconden voor de begintijd van de tijdspanne voor zoeken.
* `to=<integer>` is een waarde in JavaScript-milliseconden voor de eindtijd van de tijdspanne voor zoeken.

Zie [Epoch & Unix Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html) om te kijken hoe u de JavaScript-milliseconden voor een datum bepaalt.

### <a name="relative-time-values"></a>Relatieve tijdwaarden

Voor een relatieve tijdwaarde gebruikt u `relativeMillis=<value>`, waarbij *value* een waarde in JavaScript-milliseconden is voor de meest recente gegevens op de back-end.

`&relativeMillis=3600000` geeft bijvoorbeeld de afgelopen 60 minuten aan gegevens weer.

Geaccepteerde waarden komen overeen met de Verkenner van Time Series Insights **snelle time** in het menu en omvatten:

* `1800000` (Afgelopen 30 minuten)
* `3600000` (Afgelopen 60 minuten)
* `10800000` (Afgelopen 3 uur)
* `21600000` (Afgelopen 6 uur)
* `43200000` (Afgelopen 12 uur)
* `86400000` (Afgelopen 24 uur)
* `604800000` (Afgelopen 7 dagen)
* `2592000000` (Afgelopen 30 uur)

### <a name="optional-parameters"></a>Optionele parameters

De `timeSeriesDefinitions=<collection of term objects>` parameter geeft u de voorwaarden van een Time Series Insights-weergave:

| Parameter | URL-Item | Description |
| --- | --- | --- |
| **name** | `\<string>` | De naam van het *onderdeel*. |
| **splitBy** | `\<string>` | De naam van de kolom waarop moet worden *gesplitst*. |
| **measureName** | `\<string>` | De kolomnaam van de *meting*. |
| **predicate** | `\<string>` | De *where*-component voor filteren aan de serverzijde. |
| **useSum** | `true` | Een optionele parameter waarmee het gebruik van een som voor uw meting. </br>  Opmerking: als `Events` is van de geselecteerde maateenheid count is standaard geselecteerd.  </br>  Als `Events` is niet ingeschakeld, gemiddelde is standaard geselecteerd. |

* De `multiChartStack=<true/false>` sleutel / waarde-paar maakt in de grafiek.
* De `multiChartSameScale=<true/false>` sleutel / waarde-paar biedt dezelfde schaal voor y-as voor termen binnen een optionele parameter.  
* De `timeBucketUnit=<Unit>&timeBucketSize=<integer>` kunt u aan te passen de interval van de schuifregelaar voor een gedetailleerdere of een soepelere, meer geaggregeerde weergave van de grafiek.  
* De `timezoneOffset=<integer>` parameter kunt u instellen dat de tijdzone voor de grafiek om te worden weergegeven als een UTC-verschuiving.

| Klantlicenties | Description |
| --- | --- |
| `multiChartStack=false` | `true` is standaard ingeschakeld zodat doorgeven `false` stapelen. |
| `multiChartStack=false&multiChartSameScale=true` | Stapelen moet zijn ingeschakeld om dezelfde schaling van de Y-as te gebruiken voor verschillende onderdelen.  Er `false` standaard, dus geven 'true' schakelt deze functionaliteit. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Eenheden = dagen, uren, minuten, seconden en milliseconden.  Gebruik altijd een hoofdletter voor de eenheid. </br> Definieer het aantal eenheden door het gewenste gehele getal voor timeBucketSize op te geven.  Rond af naar 7 dagen.  |
| `timezoneOffset=-<integer>` | Het gehele getal is altijd in milliseconden. </br> Opmerking: deze functionaliteit is enigszins afwijken van wat wordt ingeschakeld in de Verkenner van Time Series Insights, waar we Hiermee kunt u kiezen lokaal (browsertijd) of UTC. |

### <a name="examples"></a>Voorbeelden

Time series-definities toevoegen aan een Time Series Insights-omgeving als een URL-parameter, toevoegen:

```plaintext
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Gebruik het voorbeeld van de tijd reeks definities voor:

* De omgevings-ID
* De afgelopen 60 minuten aan gegevens
* De voorwaarden (F1PressureID, F2TempStation en F3VibrationPL) die deel uitmaken van de volgende optionele parameters

u kunt de volgende geparameteriseerde URL voor een weergave maken:

```plaintext
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

> [!TIP]
> Zie de Explorer live [met behulp van de URL](https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]).

De URL die hierboven wordt beschreven en de Time Series Insights Explorer-weergave is gebaseerd:

[![Time Series Insights explorer voorwaarden](media/parameterized-url/url1.png)](media/parameterized-url/url1.png#lightbox)

De volledige weergave (inclusief het diagram):

[![Grafiek weergeven](media/parameterized-url/url2.png)](media/parameterized-url/url2.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [query gegevens met C# ](time-series-insights-query-data-csharp.md).

* Meer informatie over de [Time Series Insights Explorer](./time-series-insights-explorer.md).
