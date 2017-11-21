---
title: Aangepaste weergaven van Share Azure Time Series Insights delen via geparameteriseerde URL's | Microsoft Docs
description: In dit artikel wordt beschreven hoe u geparameteriseerde URL's samenstelt in Azure Time Series Insights, zodat een aangepaste weergave eenvoudig kan worden gedeeld.
services: time-series-insights
ms.service: time-series-insights
author: kfile
ms.author: kfile
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: rest-api
ms.topic: get-started-article
ms.workload: big-data
ms.date: 11/15/2017
ms.openlocfilehash: d7a4401ff87a94265c588aa5ef070bdc8544fe12
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Een aangepaste weergave delen via een geparameteriseerde URL

Als u een aangepaste weergave in de verkenner van Time Series Insights wilt delen, kunt u via programmacode een geparameteriseerde URL van de aangepaste weergave maken.

De verkenner van Time Series Insights ondersteunt URL-queryparameters om weergaven in de omgeving rechtstreeks op geven via de URL.  U kunt bijvoorbeeld via de URL een doelomgeving, een zoekpredicaat en de gewenste tijdspanne opgeven. Wanneer een gebruiker op de aangepaste URL klikt, biedt de interface een directe koppeling naar dat asset in de Time Series Insights-portal.  Beleid voor gegevenstoegang wordt toegepast. 

## <a name="environment-id"></a>Omgevings-id

De parameter `environmentId=<guid>` geeft de id van de doelomgeving op.  Dit is een onderdeel van de FQDN voor gegevenstoegang en u kunt deze vinden in de rechterbovenhoek van het omgevingsoverzicht in de Azure-portal.  Dit is alles wat voorafgaat aan `env.timeseries.azure.com`. Een voorbeeld van de parameter voor omgevings-id is `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Tijd

U kunt absolute of relatieve tijdwaarden opgeven met een geparameteriseerde URL.

### <a name="absolute-time-values"></a>Absolute tijdwaarden

Voor absolute tijdwaarden gebruikt u de parameters `from=<integer>` en `to=<integer>`. 

`from=<integer>` is een waarde in JavaScript-milliseconden voor de begintijd van de tijdspanne voor zoeken.

`to=<integer>` is een waarde in JavaScript-milliseconden voor de eindtijd van de tijdspanne voor zoeken. 

Zie [Epoch & Unix Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html) om te kijken hoe u de JavaScript-milliseconden voor een datum bepaalt.

### <a name="relative-time-values"></a>Relatieve tijdwaarden

Voor een relatieve tijdwaarde gebruikt u `relativeMillis=<value>`, waarbij *value* een waarde in JavaScript-milliseconden is voor de meest recente gegevens op de back-end.

`&relativeMillis=3600000` geeft bijvoorbeeld de afgelopen 60 minuten aan gegevens weer.

Geaccepteerde waarden komen overeen met het menu **quick time** in de Time Series Insights-verkenner en omvatten de volgende:

- 1800000 (afgelopen 30 minuten)
- 3600000 (afgelopen 60 minuten)
- 10800000 (afgelopen 3 uur)
- 21600000 (afgelopen 6 uur)
- 43200000 (afgelopen 12 uur)
- 86400000 (afgelopen 24 uur)
- 604800000 (afgelopen 7 dagen)
- 2592000000 (afgelopen 30 dagen)

### <a name="optional-parameters"></a>Optionele parameters

Met de parameter `timeSeriesDefinitions=<collection of term objects>` geeft u de onderdelen van een Time Series Insights-weergave op waarbij:

- `name=<string>`
  - De naam van het *onderdeel*.
- `splitBy=<string>`
  - De naam van de kolom waarop moet worden *gesplitst*.
- `measureName=<string>`
  - De kolomnaam van de *meting*.
- `predicate=<string>`
  - De *where*-component voor filteren aan de serverzijde.

 
### <a name="examples"></a>Voorbeelden

Als u bijvoorbeeld definities van een tijdreeks als een URL-parameter wilt toevoegen, kunt u het volgende gebruiken:

```
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Met deze voorbeelddefinities voor een tijdreeks voor 

- omgevings-id
- afgelopen 60 minuten met gegevens
- onderdelen (F1PressureID, F2TempStation en F3VibrationPL) die de optionele parameters vormen
 
kunt u de volgende geparameteriseerde URL voor een weergave maken:

`https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]`

Als u de verkenner van Time Series Insights had gebruikt om de weergave samen te stellen die wordt beschreven door de voorgaande URL, zou deze er als volgt uitzien:

![Onderdelen in Time Series Insights-verkenner](media/parameterized-url/url1.png)

De volledige weergave (inclusief het diagram) zou er als volgt uitzien:

![Diagramweergave](media/parameterized-url/url2.png)

## <a name="next-steps"></a>Volgende stappen
[Gegevens opvragen met C#](time-series-insights-query-data-csharp.md)
