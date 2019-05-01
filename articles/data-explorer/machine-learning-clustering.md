---
title: Machine learning-mogelijkheden in Azure Data Explorer
description: Gebruik machine learning clusters voor analyses van hoofdoorzaken in Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: 2358cb2ea411a0077f34798183da30bd32ae067b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925131"
---
# <a name="machine-learning-capability-in-azure-data-explorer"></a>Machine learning-mogelijkheden in Azure Data Explorer

Azure Data Explorer, een Big Data analytics-platform wordt gebruikt voor het bewaken van de servicestatus, QoS of functioneert niet goed apparaten voor afwijkend gedrag met behulp van ingebouwde [anomaliedetectie en prognoses](/azure/data-explorer/anomaly-detection) functies. Zodra een afwijkende patroon wordt gedetecteerd, wordt een hoofdmap oorzaak Analysis (RCA) uitgevoerd om te beperken of de anomalie oplossen.

Het proces diagnose is complex en langdurige en van de domein-experts. Het proces omvat het ophalen en het toevoegen van extra gegevens uit verschillende bronnen voor dezelfde periode, op zoek naar wijzigingen in de distributie van de waarden op meerdere dimensies, voor extra variabelen, grafieken en andere technieken die zijn gebaseerd op kennis van het domein en intuition. Omdat deze diagnose-scenario's gebruikt in Azure Data Explorer worden, zijn machine learning-invoegtoepassingen beschikbaar voor de fase diagnose te vergemakkelijken en verkort de duur van de RCA.

Azure Data Explorer heeft drie Machine Learning-invoegtoepassingen: [ `autocluster` ](/azure/kusto/query/autoclusterplugin), [ `basket` ](/azure/kusto/query/basketplugin), en [ `diffpatterns` ](/azure/kusto/query/diffpatternsplugin). Alle invoegtoepassingen implementeren clustering algoritmen. De `autocluster` en `basket` invoegtoepassingen een recordset van één cluster en de `diffpatterns` invoegtoepassing de verschillen tussen de twee recordsets-clusters.

## <a name="clustering-a-single-record-set"></a>Clustering van een enkele record-set

Een veelvoorkomend scenario omvat een set gegevens die door een bepaald criterium voldoen, zoals tijdvenster op dat vertoont afwijkend gedrag, hoogste temperatuur, serverlogs, apparaten, lange duur van de opdrachten en boven uitgaven van gebruikers is geselecteerd. Wij willen graag een eenvoudige en snelle manier om algemene patronen (segmenten) niet vinden in de gegevens. Patronen zijn een subset van de gegevensset waarvan u de records dezelfde waarden via meerdere dimensies (categorische kolommen delen). De volgende query bouwt en ziet u een tijdreeks uitzonderingen voor de service gedurende een week in tien minuten durende opslaglocaties:

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m
| render timechart with(title="Service exceptions over a week, 10 minutes resolution")
```

![Service uitzonderingen tijdgrafiek](media/machine-learning-clustering/service-exceptions-timechart.png)

Het aantal service-uitzonderingen komt overeen met het totale verkeer-service. Duidelijk ziet u de dagelijkse patroon voor werkdagen van maandag tot en met vrijdag, met een toename van de service uitzondering telt halverwege dag en komt in aantallen 's nachts. Vaste laag aantal worden weergegeven tijdens het weekend. Uitzondering pieken kunnen worden gedetecteerd met behulp van [time series-anomaliedetectie](/azure/data-explorer/anomaly-detection?#time-series-anomaly-detection) in Azure Data Explorer.

De tweede piek in de gegevens wordt uitgevoerd op dinsdag middag. De volgende query wordt gebruikt om deze piek nader onderzoek. Gebruik van de query voor het vernieuwen van het grafiekgebied rond de piek in hogere resolutie (acht uur in één minuut opslaglocaties) om te controleren of het een sharp piek is en de randen weergeven.

```kusto
let min_t=datetime(2016-08-23 11:00);
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to min_t+8h step 1m
| render timechart with(title="Zoom on the 2nd spike, 1 minute resolution")
```

![Richt u op piek tijdgrafiek](media/machine-learning-clustering/focus-spike-timechart.png)

Ziet u een smal piek van twee minuten vanaf 15:00 op 15:02. Aantal uitzonderingen in dit venster twee minuten in de volgende query:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| count
```

|Count |
|---------|
|972    |

Voorbeeld van 20 uitzonderingen buiten 972 in de volgende query:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| take 20
```

| PreciseTimeStamp            | Regio | ScaleUnit | DeploymentId                     | Traceringspunt | ServiceHost                          |
|-----------------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 2016-08-23 15:00:08.7302460 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:09.9496584 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 8d257da1-7a1c-44f5-9acd-f9e02ff507fd |
| 2016-08-23 15:00:10.5911748 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:12.2957912 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | f855fcef-ebfe-405d-aaf8-9c5e2e43d862 |
| 2016-08-23 15:00:18.5955357 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 9d390e07-417d-42eb-bebd-793965189a28 |
| 2016-08-23 15:00:20.7444854 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 6e54c1c8-42d3-4e4e-8b79-9bb076ca71f1 |
| 2016-08-23 15:00:23.8694999 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 36109      | 19422243-19b9-4d85-9ca6-bc961861d287 |
| 2016-08-23 15:00:26.4271786 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 36109      | 3271bae4-1c5b-4f73-98ef-cc117e9be914 |
| 2016-08-23 15:00:27.8958124 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 904498     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:32.9884969 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007007   | d5c7c825-9d46-4ab7-a0c1-8e2ac1d83ddb |
| 2016-08-23 15:00:34.5061623 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:37.4490273 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | f2ee8254-173c-477d-a1de-4902150ea50d |
| 2016-08-23 15:00:41.2431223 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 103200     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:47.2983975 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 423690590  | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:50.5932834 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 2a41b552-aa19-4987-8cdd-410a3af016ac |
| 2016-08-23 15:00:50.8259021 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 0d56b8e3-470d-4213-91da-97405f8d005e |
| 2016-08-23 15:00:53.2490731 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 36109      | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:57.0000946 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 64038      | cb55739e-4afe-46a3-970f-1b49d8ee7564 |
| 2016-08-23 15:00:58.2222707 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | 8215dcf6-2de0-42bd-9c90-181c70486c9c |
| 2016-08-23 15:00:59.9382620 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | 451e3c4c-0808-4566-a64d-84d85cf30978 |

### <a name="use-autocluster-for-single-record-set-clustering"></a>Gebruik `autocluster()` voor één record clustering instellen

Hoewel er minder dan duizend uitzonderingen zijn, is het nog steeds moeilijk te vinden van algemene segmenten, als er meerdere waarden in elke kolom. U kunt [ `autocluster()` ](/azure/kusto/query/autoclusterplugin) invoegtoepassing om direct een kleine lijst met algemene segmenten te vinden van de interessante binnen twee minuten van de piek-clusters zoals te zien is in de volgende query:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate autocluster()
```

| SegmentId | Count | Procent | Regio | ScaleUnit | DeploymentId | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 94 | 9.67078189300411 | scus | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |
| 2 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 3 | 68 | 6.99588477366255 | scus | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |
| 4 | 55 | 5.65843621399177 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |

U ziet in de bovenstaande resultaten die het meest dominante segment 65.74% van de totale uitzondering records bevat en vier dimensies deelt. Het volgende segment is veel minder gebruikelijke, alleen 9.67% van de records bevat en drie dimensies deelt. De andere segmenten zijn ook minder vaak. 

Autocluster maakt gebruik van een eigen algoritme voor meerdere dimensies analysemodel en interessante segmenten extraheren. 'Interessante' betekent dat elk segment aanzienlijke dekking van de set records en de set functies heeft. De segmenten zijn ook opgedeeld, wat betekent dat elke gebeurtenis aanzienlijk verschillen van de andere wordt. Het is mogelijk dat een of meer van deze segmenten relevant zijn voor het RCA-proces. Om te beperken segment onderzoek, extraheert autocluster slechts een klein segment-lijst.

### <a name="use-basket-for-single-record-set-clustering"></a>Gebruik `basket()` voor één record clustering instellen

U kunt ook de [ `basket()` ](/azure/kusto/query/basketplugin) invoegtoepassing zoals te zien is in de volgende query:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate basket()
```

| SegmentId | Count | Procent | Regio | ScaleUnit | DeploymentId | Traceringspunt | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 642 | 66.0493827160494 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |  |
| 2 | 324 | 33.3333333333333 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 0 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 3 | 315 | 32.4074074074074 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 16108 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 4 | 328 | 33.7448559670782 |  |  |  | 0 |  |
| 5 | 94 | 9.67078189300411 | scus | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |  |
| 6 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |  |
| 7 | 68 | 6.99588477366255 | scus | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |  |
| 8 | 167 | 17.1810699588477 | scus |  |  |  |  |
| 9 | 55 | 5.65843621399177 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |  |
| 10 | 92 | 9.46502057613169 |  |  |  | 10007007 |  |
| 11 | 90 | 9.25925925925926 |  |  |  | 10007006 |  |
| 12 | 57 | 5.8641975308642 |  |  |  |  | 00000000-0000-0000-0000-000000000000 |

Mandje implementeert het algoritme Apriori voor item gegevensanalyse en haalt alle segmenten waarvan dekking van de recordset is hoger dan een drempelwaarde (standaard 5%). U kunt zien dat er meer segmenten zijn geëxtraheerd met soortgelijke gezichten (voor bijvoorbeeld, segmenten 0,1 of 2,3).

Beide invoegtoepassingen zijn krachtige en eenvoudig te gebruiken, maar hun belangrijke beperking is vanwege het feit dat ze een enkele record in een zonder supervisie manier (met geen labels)-cluster. Het is daarom niet duidelijk of de uitgepakte patronen in de geselecteerde recordset (de afwijkende records) of de globale Recordset karakteriseren.

## <a name="clustering-the-difference-between-two-records-sets"></a>Het verschil tussen twee records reeksen clustering

De [ `diffpatterns()` ](/azure/kusto/query/diffpatternsplugin) de beperking van de invoegtoepassing worden overwonnen `autocluster` en `basket`. `Diffpatterns` neemt twee recordsets en extraheert de belangrijkste segmenten die verschillen daartussen. Een set bevat doorgaans de afwijkende recordset wordt onderzocht (een geanalyseerd door `autocluster` en `basket`). De andere set bevat de referentie-recordset (basislijn). 

In de onderstaande query gebruiken we `diffpatterns` naar interessante clusters zoeken binnen de piek van twee minuten, die verschillen van clusters in de basislijn. We definiëren de basislijn-venster als de acht minuten vóór 15:00 (wanneer de piek gestart). We moeten ook uitbreiden door een binaire kolom (AB) op te geven of een bepaalde record aan de basislijn of aan de set voor afwijkende behoort. `Diffpatterns` implementeert een algoritme voor leren met supervisie, waarbij de twee klasse labels zijn gegenereerd door de afwijkende ten opzichte van de basislijn-vlag (AB).

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
let min_baseline_t=datetime(2016-08-23 14:50);
let max_baseline_t=datetime(2016-08-23 14:58); // Leave a gap between the baseline and the spike to avoid the transition zone.
let splitime=(max_baseline_t+min_peak_t)/2.0;
demo_clustering1
| where (PreciseTimeStamp between(min_baseline_t..max_baseline_t)) or
        (PreciseTimeStamp between(min_peak_t..max_peak_t))
| extend AB=iff(PreciseTimeStamp > splitime, 'Anomaly', 'Baseline')
| evaluate diffpatterns(AB, 'Anomaly', 'Baseline')
```

| SegmentId | CountA | CountB | PercentA | PercentB | PercentDiffAB | Regio | ScaleUnit | DeploymentId | Traceringspunt |
|-----------|--------|--------|----------|----------|---------------|--------|-----------|----------------------------------|------------|
| 0 | 639 | 21 | 65.74 | 1.7 | 64.04 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |
| 1 | 167 | 544 | 17.18 | 44.16 | 26.97 | scus |  |  |  |
| 2 | 92 | 356 | 9.47 | 28.9 | 19.43 |  |  |  | 10007007 |
| 3 | 90 | 336 | 9.26 | 27.27 | 18.01 |  |  |  | 10007006 |
| 4 | 82 | 318 | 8.44 | 25.81 | 17.38 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 5 | 55 | 252 | 5.66 | 20.45 | 14.8 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |
| 6 | 57 | 204 | 5.86 | 16.56 | 10.69 |  |  |  |  |

De dominantste-segment is hetzelfde segment dat is opgehaald door `autocluster`, de dekking op de afwijkende window van twee minuten is ook 65.74%. Maar de dekking op het venster acht minuten durende basislijn alleen 1.7% is. Het verschil is 64.04%. Dit verschil lijkt te zijn met betrekking tot de afwijkende piek. U kunt deze aanname controleren door het splitsen van de originele grafiek in de records die behoren tot deze problematische segment ten opzichte van de andere segmenten zoals te zien is in de onderstaande query:

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| extend seg = iff(Region == "eau" and ScaleUnit == "su7" and DeploymentId == "b5d1d4df547d4a04ac15885617edba57"
and ServiceHost == "e7f60c5d-4944-42b3-922a-92e98a8e7dec", "Problem", "Normal")
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m by seg
| render timechart
```

!['Diffpattern' segment tijdgrafiek valideren](media/machine-learning-clustering/validating-diffpattern-timechart.png)

In deze grafiek kan we zien dat de piek op dinsdag middag vanwege uitzonderingen van het specifieke segment, gedetecteerd met behulp van de `diffpatterns` invoegtoepassing.

## <a name="summary"></a>Samenvatting

De Azure Data Explorer Machine Learning-modules zijn nuttig voor veel scenario's. De `autocluster` en `basket` implementeren zonder supervisie leeralgoritme en zijn eenvoudig te gebruiken. `Diffpatterns` implementeert leeralgoritme onder supervisie en hoewel complexer worden, het is nog krachtigere bij het ophalen van differentiatie segmenten voor RCA.

Deze invoegtoepassingen zijn interactief gebruikt in scenario's voor ad-hoc en automatisch in de buurt van real-time bewaking services. Time series-anomaliedetectie wordt in Azure Data Explorer gevolgd door een diagnose-proces dat is geoptimaliseerd om te voldoen aan prestatienormen die nodig zijn.
