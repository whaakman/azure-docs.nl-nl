---
title: Time series-anomaliedetectie en prognoses in Azure Data Explorer
description: Meer informatie over het analyseren van time series-gegevens voor detectie van afwijkingen en prognoses met behulp van Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: 934dfd3334b6f433c7acdf9816a3fb5e24f0430f
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64872012"
---
# <a name="anomaly-detection-and-forecasting-in-azure-data-explorer"></a>Detectie van afwijkingen en prognoses in Azure Data Explorer

Azure Data Explorer wordt uitgevoerd op gaan verzamelen van telemetriegegevens van cloudservices of IoT-apparaten. Deze gegevens worden geanalyseerd voor verschillende inzichten, zoals bewaking servicestatus, fysieke productieprocessen, trends in gebruik en load prognose. De analyse is uitgevoerd op de tijdreeksen van geselecteerde metrische gegevens te vinden van een patroon afwijking van de metrische gegevens ten opzichte van de typische normale basislijn-patroon. Azure Data Explorer bevat systeemeigen ondersteuning voor het maken, bewerken en analyse van meerdere tijdreeksen. Het kan maken en analyseren van duizenden tijdreeks in seconden, inschakelen in de buurt van real-time bewaking van oplossingen en -werkstromen.

Dit artikel worden de Azure Data Explorer tijd reeks anomaliedetectie detectie worden gevolgd en voorspeld mogelijkheden. De functies van de reeks tijd van toepassing zijn op basis van een robuuste bekende ontleding-model, waarbij elke oorspronkelijke tijdreeks is opgesplitst in seizoensgebonden, trend, en de resterende onderdelen. Afwijkingen worden gedetecteerd door uitbijters op het resterende onderdeel tijdens het maken van prognoses wordt uitgevoerd door de seizoensgebonden extrapoleren en trend van onderdelen. De uitvoering Azure Data Explorer is aanzienlijk beter voor het model basic ontleding door automatische seizoensgebondenheid detectie, robuuste uitschieter analyse en vectorized implementatie naar proces duizenden van tijdreeks in seconden.

## <a name="prerequisites"></a>Vereisten

Lezen [Time series-analyse in Azure Data Explorer](/azure/data-explorer/time-series-analysis) voor een overzicht van time series-mogelijkheden.

## <a name="time-series-decomposition-model"></a>Time series-ontleding model

Azure Data Explorer systeemeigen uitvoering voor tijdreeksvoorspelling en detectie van afwijkingen maakt gebruik van een bekende ontleding-model. Dit model wordt toegepast op de tijdreeksen van metrische gegevens verwacht aan het licht periodieke en het gedrag van de trend, zoals serviceverkeer onderdeel heartbeats en IoT periodieke metingen voorspellen van toekomstige metrische waarden en het opsporen van afwijkende bestanden. De veronderstelling van dit proces regressie is dat dan eerder bekende seizoensgebonden en trend gedrag, de tijd reeks willekeurig wordt gedistribueerd. U kunt vervolgens voorspellen van toekomstige metrische waarden uit de seizoensgebonden en trend-onderdelen, samen met de naam van basislijn, en het resterende deel negeren. U kunt ook afwijkende waarden op basis van uitschieter analyse met behulp van alleen het resterende gedeelte detecteren.
Gebruik de functie voor het maken van een model ontleding [ `series_decompose()` ](/azure/kusto/query/series-decomposefunction). De `series_decompose()` functie gebruikt een set van tijdreeks en automatisch ontleedt elke timeseries die moet de seizoensgebonden, trend, resterende en onderdelen van de basislijn. 

U kunt bijvoorbeeld verkeer van een interne webservice afbreken met behulp van de volgende query uit:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (baseline, seasonal, trend, residual) = series_decompose(num, -1, 'linefit')  //  decomposition of a set of time series to seasonal, trend, residual, and baseline (seasonal+trend)
| render timechart with(title='Web app. traffic of a month, decomposition', ysplit=panels)
```

![Time series-ontleding](media/anomaly-detection/series-decompose-timechart.png)

* De oorspronkelijke tijdreeksen heet **num** (in rood). 
* Het proces wordt gestart door automatische detectie van de periodieke variatie met behulp van de functie [ `series_periods_detect()` ](/azure/kusto/query/series-periods-detectfunction) en extraheert de **seizoensgebonden** patroon (in paars).
* De seizoenspatroon wordt afgetrokken van de oorspronkelijke tijdreeksen en een lineaire regressie wordt uitgevoerd met behulp van de functie [ `series_fit_line()` ](/azure/kusto/query/series-fit-linefunction) vinden de **trend** onderdeel (in lichtblauw).
* Trekt de functie de trend en de rest is de **resterende** onderdeel (in het groen).
* Ten slotte de functie wordt toegevoegd de seizoensgebonden en trend van onderdelen voor het genereren van de **basislijn** (in het blauw).

## <a name="time-series-anomaly-detection"></a>Time series-anomaliedetectie

De functie [ `series_decompose_anomalies()` ](/azure/kusto/query/series-decompose-anomaliesfunction) afwijkende punten op een set van tijdreeks vindt. Deze functie aanroepen `series_decompose()` aan het bouwen van de ontleding model en wordt uitgevoerd [ `series_outliers()` ](/azure/kusto/query/series-outliersfunction) op het resterende onderdeel. `series_outliers()` Berekent de anomaliedetectie-scores voor elk punt van het resterende onderdeel met behulp van Tukey omheining test. Anomaliedetectie scores boven 1.5 of onder-1.5 geven een stijging van de lichte afwijkingen of te weigeren, respectievelijk. Anomaliedetectie scores boven 3.0 of onder -3,0 geven een sterke anomaliedetectie. 

De volgende query kunt u voor het detecteren van afwijkingen in het interne web serviceverkeer:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (anomalies, score, baseline) = series_decompose_anomalies(num, 1.5, -1, 'linefit')
| render anomalychart with(anomalycolumns=anomalies, title='Web app. traffic of a month, anomalies') //use "| render anomalychart with anomalycolumns=anomalies" to render the anomalies as bold points on the series charts.
```

![Time series-anomaliedetectie](media/anomaly-detection/series-anomaly-detection.png)

* De oorspronkelijke tijdreeksen (in rood). 
* De basislijn (seizoensgebonden + trend) onderdeel (in het blauw).
* De afwijkende punten (in paars) boven op de oorspronkelijke tijdreeksen. De afwijkende punten worden aanzienlijk afwijken van de verwachte basislijnwaarden.

## <a name="time-series-forecasting"></a>Time series-prognoses

De functie [ `series_decompose_forecast()` ](/azure/kusto/query/series-decompose-forecastfunction) toekomstige waarden van een set van tijdreeks worden voorspeld. Deze functie aanroepen `series_decompose()` extrapolates bouwen de ontleding modellen en klik vervolgens voor elke tijdreeks de basislijn-component in de toekomst.

De volgende query kunt u om te voorspellen van de volgende week web serviceverkeer:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week by Time Series Decmposition')
```

![Time series-prognoses](media/anomaly-detection/series-forecasting.png)

* Oorspronkelijke metrische gegevens (in rood). Toekomstige waarden ontbreken en standaard ingesteld op 0.
* Het onderdeel van de basislijn (in het blauw) om te voorspellen van de volgende week waarden afleiden.

## <a name="scalability"></a>Schaalbaarheid

Azure Data Explorer query-syntaxis kunt één aanroep voor het verwerken van meerdere tijdreeksen. De unieke geoptimaliseerde implementatie kunt u snelle prestaties, wat van essentieel belang voor anomaliedetectie effectief worden gevolgd en voorspeld bij het bewaken van duizenden items in bijna realtime scenario's.

De volgende query ziet u de verwerking van drie tijdreeks tegelijk:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid
| extend offset=case(sid=='TS3', 4000000, sid=='TS2', 2000000, 0)   //  add artificial offset for easy visualization of multiple time series
| extend num=series_add(num, offset)
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week for 3 time series')
```

![Time series schaalbaarheid](media/anomaly-detection/series-scalability.png)

## <a name="summary"></a>Samenvatting

In dit document worden systeemeigen Azure-Data Explorer-functies voor time series-anomaliedetectie en prognoses, die aanzienlijk beter zijn de fundamentele ontleding model waarop deze gebaseerd. Elke oorspronkelijke tijdreeks is opgesplitst, worden afwijkingen gedetecteerd, en prognose uitgevoerd. De tijd reeks anomaliedetectie detectie worden gevolgd en voorspeld functies worden gebruikt voor bijna realtime bewakingsscenario's, zoals foutenopsporing, Voorspellend onderhoud, en de vraag en prognoses te laden.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Machine learning-mogelijkheden](/azure/data-explorer/machine-learning-clustering) in Azure Data Explorer.