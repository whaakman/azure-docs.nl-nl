---
title: Azure Data Explorer tijdseries analyseren
description: 'Meer informatie over tijdseries analyseren in Azure Data Explorer '
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 63182657e7c5793a2102efecabeb7d51fa1086a9
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55729486"
---
# <a name="time-series-analysis-in-azure-data-explorer"></a>Tijdseries analyseren in Azure Data Explorer

Azure Data Explorer (ADX) wordt uitgevoerd op gaan verzamelen van telemetriegegevens van cloudservices of IoT-apparaten. Deze gegevens kan worden geanalyseerd voor verschillende inzichten, zoals bewaking servicestatus, fysieke productieprocessen en trends in gebruik. Analyse is uitgevoerd op de tijdreeksen van geselecteerde metrische gegevens een afwijking te vinden in het patroon in vergelijking met het gebruikspatroon typische basislijn.
ADX bevat systeemeigen ondersteuning voor het maken, bewerken en analyse van meerdere tijdreeksen. In dit onderwerp leert u hoe ADX wordt gebruikt om te maken en te analyseren **duizenden tijdreeks in een paar seconden**, inschakelen in de buurt van real-time bewaking oplossingen en -werkstromen.

## <a name="time-series-creation"></a>Time series maken

In deze sectie maken we een groot aantal regelmatige tijds-reeks eenvoudig en intuïtief met het `make-series` operator en de ontbrekende waarden invullen indien nodig.
De eerste stap in tijdseries analyseren is voor het partitioneren en transformeren van de oorspronkelijke tabel telemetrie naar een set van tijdreeks. De tabel bevat meestal een timestamp-kolom, contextuele dimensies en optionele metrische gegevens. De dimensies worden gebruikt voor het partitioneren van de gegevens. Het doel is het maken van duizenden tijdreeks per partitie met regelmatige tussenpozen.

De invoertabel *demo_make_series1* 600 K records van willekeurige web serviceverkeer bevat. Gebruik de onderstaande opdracht dat er een steekproef van 10 records:

```kusto
demo_make_series1 | take 10 
```

De resulterende tabel bevat een timestamp-kolom, drie dimensiekolommen van contextuele en geen metrische gegevens:

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Tijdstempel | BrowserVer | OsVer | Land/regio |
|   | 2016-08-25 09:12:35.4020000 | Chrome 51,0 | Windows 7 | Verenigd Koninkrijk |
|   | 2016-08-25 09:12:41.1120000 | Chrome 52.0 | Windows 10 |   |
|   | 2016-08-25 09:12:46.2300000 | Chrome 52.0 | Windows 7 | Verenigd Koninkrijk |
|   | 2016-08-25 09:12:46.5100000 | Chrome 52.0 | Windows 10 | Verenigd Koninkrijk |
|   | 2016-08-25 09:12:46.5570000 | Chrome 52.0 | Windows 10 | Republiek Litouwen |
|   | 2016-08-25 09:12:47.0470000 | Chrome 52.0 | Windows 8.1 | India |
|   | 2016-08-25 09:12:51.3600000 | Chrome 52.0 | Windows 10 | Verenigd Koninkrijk |
|   | 2016-08-25 09:12:51.6930000 | Chrome 52.0 | Windows 7 | Nederland |
|   | 2016-08-25 09:12:56.4240000 | Chrome 52.0 | Windows 10 | Verenigd Koninkrijk |
|   | 2016-08-25 09:13:08.7230000 | Chrome 52.0 | Windows 10 | India |

Aangezien er geen metrische gegevens, kunnen we alleen een set die het verkeer aantal zelf, gepartitioneerd op basis van besturingssysteem met behulp van de volgende query uit tijdreeksen bouwen:

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| render timechart 
```

- Gebruik de [ `make-series` ](/azure/kusto/query/make-seriesoperator) operator om te maken van een set met drie tijdreeksen, waarbij:
    - `num=count()`: time series van verkeer
    - `range(min_t, max_t, 1h)`: tijdreeks wordt gemaakt in bins van 1 uur in het tijdsbereik (oudste en nieuwste tijdstempels van tabelrecords)
    - `default=0`: Geef opvulling methode voor het ontbrekende opslaglocaties om regelmatige tijds reeks te maken. U kunt ook gebruik [ `series_fill_const()` ](/azure/kusto/query/series-fill-constfunction), [ `series_fill_forward()` ](/azure/kusto/query/series-fill-forwardfunction), [ `series_fill_backward()` ](/azure/kusto/query/series-fill-backwardfunction) en [ `series_fill_linear()` ](/azure/kusto/query/series-fill-linearfunction) wijzigingen
    - `byOsVer`: partitie door besturingssysteem
- De gegevensstructuur van de werkelijke tijd-serie is een numerieke matrix van de geaggregeerde waarde per elke bin tijd. We gebruiken `render timechart` voor visualisatie.

We hebben drie partities in de bovenstaande tabel. We kunnen een afzonderlijke tijdreeks maken: Windows 10 (rood), 7 (blauw) en 8.1 (groen) voor elke versie van het besturingssysteem zoals te zien is in de grafiek:

![Time series partitie](media/time-series-analysis/time-series-partition.png)

## <a name="time-series-analysis-functions"></a>Time series analysis functies

In deze sectie wordt we typische reeks functies voor het verwerken uitvoeren.
Nadat een set van tijdreeks is gemaakt, ADX biedt ondersteuning voor een groeiende lijst met functies voor het verwerken en analyseren die kan worden gevonden in de [time series-documentatie](/azure/kusto/query/machine-learning-and-tsa). We wordt enkele representatieve functies voor het verwerken en analyseren van de tijdreeks beschreven.

### <a name="filtering"></a>Filteren

Filteren van een gebruikelijk bij het signaal verwerkings- en handig voor tijdreeks standaardtaken voor de verwerking is (bijvoorbeeld een ruis signaal vloeiende, detectie te wijzigen).
- Er zijn twee algemene filters gebruiken om functies:
    - [`series_fir()`](/azure/kusto/query/series-firfunction): BESTANDSNAAMGEDEELTE filter toepassen. Gebruikt voor eenvoudige berekening van gemiddelde en differentiatie van de tijdreeks voor de detectie van de wijziging te verplaatsen.
    - [`series_iir()`](/azure/kusto/query/series-iirfunction): IIR filter toepassen. Gebruikt voor exponentieel vloeiend maken en het cumulatieve totaal.
- `Extend` het formaat van de tijdreeks ingesteld door het toevoegen van een nieuwe zwevend gemiddelde reeks van 5 opslaglocaties (met de naam *ma_num*) op de query:

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| render timechart
```

![Timeseries filteren](media/time-series-analysis/time-series-filtering.png)

### <a name="regression-analysis"></a>Regressie-analyse

ADX ondersteunt gesegmenteerd lineaire regressie-analyse voor een schatting van de trend van de tijdreeks.
- Gebruik [series_fit_line()](/azure/kusto/query/series-fit-linefunction) aanpassen aan de aanbevolen regel om een tijdreeks voor de algemene trend om de detectie.
- Gebruik [series_fit_2lines()](/azure/kusto/query/series-fit-2linesfunction) trend om wijzigingen te herkennen, ten opzichte van de basislijn die handig zijn in de bewaking van scenario's.

Voorbeeld van `series_fit_line()` en `series_fit_2lines()` functies in een time series-query:

```kusto
demo_series2
| extend series_fit_2lines(y), series_fit_line(y)
| project x, y, series_fit_2lines_y_line_fit, series_fit_line_y_line_fit 
| render linechart
```

![Time series regressie](media/time-series-analysis/time-series-regression.png)

- Blauw: de oorspronkelijke tijdreeks
- Groen: voorzien zijn van regel
- Rood: twee gemonteerd regels

> [!NOTE]
> De functie correct gedetecteerd door het punt jump (niveau wijzigen).

### <a name="seasonality-detection"></a>Detectie van seizoensgebondenheid

Veel metrische gegevens gaat u als volgt seizoensgebonden (periodieke) patronen. Gebruikersverkeer van cloudservices bevat meestal dagelijkse en wekelijkse patronen die hoogste rond het midden van de dag en het laagste nachts en tijdens het weekend. IoT sensoren meting periodieke intervallen. Fysieke metingen zoals temperatuur, druk of vochtigheid laten ook zien voor seizoensgebonden gedrag.

Het volgende voorbeeld wordt de detectie van de periodieke variatie op het verkeer van één maand van een webservice (2 uur opslaglocaties):

```kusto
demo_series3
| render timechart 
```

![Time series seizoensgebondenheid](media/time-series-analysis/time-series-seasonality.png)

- Gebruik [series_periods_detect()](/azure/kusto/query/series-periods-detectfunction) automatisch detecteren van de punten in de tijdreeks. 
- Gebruik [series_periods_validate()](/azure/kusto/query/series-periods-validatefunction) als we weten dat een metrische waarde specifieke afzonderlijke periode hebben moet en we controleren willen of ze bestaan.

> [!NOTE]
> Het is een anomalie als specifieke verschillende perioden nog niet bestaan

```kusto
demo_series3
| project (periods, scores) = series_periods_detect(num, 0., 14d/2h, 2) //to detect the periods in the time series
| mvexpand periods, scores
| extend days=2h*todouble(periods)/1d
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | perioden | scores | dagen |
|   | 84 | 0.820622786055595 | 7 |
|   | 12 | 0.764601405803502 | 1 |

De functie detecteert dagelijkse en wekelijkse seizoensgebondenheid. De dagelijkse beoordeelt kleiner zijn dan het wekelijkse omdat weekenddagen van doordeweekse dagen verschillen.

### <a name="element-wise-functions"></a>Element-Wise functies

Rekenkundige en logische bewerkingen kunnen worden uitgevoerd op een tijdreeks. Met behulp van [series_subtract()](/azure/kusto/query/series-subtractfunction) we kunnen berekenen van een tijdserie resterende, die is, het verschil tussen de oorspronkelijke onbewerkte metrische gegevens en een vloeiende en zoeken naar afwijkingen in de resterende signaal:

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| extend residual_num=series_subtract(num, ma_num) //to calculate residual time series
| where OsVer == "Windows 10"   // filter on Win 10 to visualize a cleaner chart 
| render timechart
```

![Time series-bewerkingen](media/time-series-analysis/time-series-operations.png)

- Blauw: de oorspronkelijke tijdreeks
- Rood: vloeiend tijdreeks
- Groen: de resterende tijdreeks

## <a name="time-series-workflow-at-scale"></a>Time series-werkstroom op schaal

Het volgende voorbeeld ziet u hoe deze functies kunnen uitvoeren op grote schaal op duizenden tijdreeks in seconden voor detectie van afwijkingen. Vier dagen uitvoeren om te zien van een paar voorbeeld telemetrie records van een DB-service lezen aantal metrische gegevens de volgende query uit:

```kusto
demo_many_series1
| take 4 
```

|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
|   | TIMESTAMP | Loc | anonOp | DB | DataRead |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 262 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 241 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | -865998331941149874 | 262 | 279862 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 371921734563783410 | 255 | 0 |

En eenvoudige statistische gegevens:

```kusto
demo_many_series1
| summarize num=count(), min_t=min(TIMESTAMP), max_t=max(TIMESTAMP) 
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | num | min\_t | max\_t |
|   | 2177472 | 2016-09-08 00:00:00.0000000 | 2016-09-11 23:00:00.0000000 |

Het bouwen van een tijdreeks in bins van 1 uur van de gelezen metrische gegevens (in totaal vier dagen * 24 uur = 96 punten), resulteert in het normale patroon fluctuatie:

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h)
| render timechart 
```

![Tijdreeks op schaal](media/time-series-analysis/time-series-at-scale.png)

De bovenstaande gedrag is misleidend, omdat de één normale tijdreeksen van duizenden verschillende exemplaren dat mogelijk afwijkende patronen wordt geaggregeerd. Daarom maken we een tijdreeks per exemplaar. Een instantie wordt gedefinieerd door Loc (locatie), anonOp (bewerking) en DB (specifieke machine).

Hoeveel tijdreeksen kunnen we maken?

```kusto
demo_many_series1
| summarize by Loc, anonOp, DB
| count
```

|   |   |
| --- | --- |
|   | Count |
|   | 23115 |

We gaan nu een reeks 23115 tijdreeksen van de gelezen aantal metrische gegevens. We voegen de `by` component in de instructie maken-serie lineaire regressie van toepassing, en selecteert u de top twee tijdreeksen waarop de meest significante verlagen trend:

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, anonOp, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc 
| render timechart with(title='Service Traffic Outage for 2 instances (out of 23115)')
```

![Time series-bovenaan twee](media/time-series-analysis/time-series-top-2.png)

De exemplaren worden weergegeven:

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, anonOp, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc
| project Loc, anonOp, DB, slope 
```

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Loc | anonOp | DB | helling |
|   | Loc 15 | -3207352159611332166 | 1151 | -102743.910227889 |
|   | Loc 13 | -3207352159611332166 | 1249 | -86303.2334644601 |

In minder dan twee minuten ADX geanalyseerd van meer dan 20.000 tijdreeksen en twee abnormaal tijdreeks waarin het aantal gelezen plotseling verwijderd gedetecteerd.

Deze geavanceerde mogelijkheden in combinatie met ADX snelle prestaties leveren een unieke en krachtige oplossing voor tijdseries analyseren.
