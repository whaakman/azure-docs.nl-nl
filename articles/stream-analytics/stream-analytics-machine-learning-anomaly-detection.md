---
title: Anomaliedetectie in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics en Azure Machine Learning samen gebruiken voor het detecteren van afwijkingen.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: e2fd226f1c605821f0fd595832b2cbe26d994fb4
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612341"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Anomaliedetectie in Azure Stream Analytics

Beschikbaar in de cloud en de Azure IoT Edge, Azure Stream Analytics biedt ingebouwde machine learning-detectiemogelijkheden op basis van afwijkingen die kunnen worden gebruikt voor het bewaken van de twee meest voorkomende anomalieën: tijdelijke en permanente. Met de **AnomalyDetection_SpikeAndDip** en **AnomalyDetection_ChangePoint** functies, rechtstreeks in uw Stream Analytics-taak kunt u anomaliedetectie uitvoeren.

De machine learning-modellen wordt ervan uitgegaan dat een uniforme wijze sample tijdreeks. Als de tijdreeks niet uniform is, kunt u een stap aggregatie met een tumblingvenster vóór het aanroepen van detectie van afwijkingen kunt invoegen.

De machine learning-bewerkingen ondersteunen geen periodieke variatie trends of meerdere variate correlaties op dit moment.

## <a name="model-behavior"></a>Model-gedrag

Over het algemeen verbetert de nauwkeurigheid van het model met meer gegevens in de sliding window van. De gegevens in de opgegeven sliding window van wordt behandeld als onderdeel van het normale bereik van waarden voor die periode wordt voltooid. Het model alleen rekening gehouden met geschiedenis van gebeurtenissen gedurende de sliding window van moet worden gecontroleerd als de huidige gebeurtenis afwijkend. Als u de sliding window van verplaatst, worden oude waarden uit de training van het model verwijderd.

De functies worden uitgevoerd door het tot stand brengen van een bepaalde normale op basis van wat ze tot nu toe hebben gezien. Uitbijters worden geïdentificeerd door het vergelijken met het tot stand gebrachte normaal, binnen het niveau van betrouwbaarheid. De venstergrootte moet worden gebaseerd op de minimale gebeurtenissen die zijn vereist voor het trainen van het model voor het normale gedrag, zodat wanneer een anomalie wordt gedetecteerd, het kunnen herkennen deze zou zijn.

Reactietijd van het model wordt verhoogd met een grootte van de geschiedenis omdat nodig is om te vergelijken met een groter aantal gebeurtenissen in het verleden. Het verdient aanbeveling om op te nemen alleen het benodigde aantal gebeurtenissen voor betere prestaties.

Hiaten in de tijdreeks kunnen het gevolg zijn van het model niet ontvangen van gebeurtenissen op bepaalde tijdstippen in de tijd. Deze situatie wordt verzorgd door Stream Analytics met behulp van toerekening logica. De grootte van de geschiedenis, evenals een tijdsduur voor de sliding window van dezelfde wordt gebruikt voor het berekenen van de gemiddelde frequentie van waarmee gebeurtenissen worden verwacht aankomt.

Een generator anomaliedetectie beschikbaar [hier](https://aka.ms/asaanomalygenerator) kan worden gebruikt om de feed van een Iot-Hub met gegevens met verschillende anomaliedetectie patronen. Een ASA-taak kan worden ingesteld met deze functies van de detectie van afwijkingen om te lezen van deze Iot Hub en afwijkingen.

## <a name="spike-and-dip"></a>Piek- en dip

Tijdelijke afwijkingen in de gebeurtenisstroom van een time-serie zijn voorzien van zoals pieken en dalen. Pieken en dips n de kunnen worden bewaakt met behulp van de operator op Machine Learning gebaseerde [AnomalyDetection_SpikeAndDip](https://docs.microsoft.com/stream-analytics-query/anomalydetection-spikeanddip-azure-stream-analytics
).

![Voorbeeld van een piek- en dip-anomaliedetectie](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-spike-dip.png)

In de sliding window van dezelfde, als een tweede piek kleiner dan de eerste versie is, is de berekende score voor de kleinere piek waarschijnlijk niet aanzienlijke voldoende ten opzichte van de score voor de eerste piek in het niveau van betrouwbaarheid opgegeven. U kunt proberen de betrouwbaarheid van het model voor het detecteren van dergelijke afwijkingen verlagen. Als u begint met te veel waarschuwingen ontvangen, kunt u echter een hogere betrouwbaarheidsinterval gebruiken.

De volgende voorbeeldquery wordt ervan uitgegaan dat een uniform invoer tarief van één gebeurtenis per seconde in een sliding window van 2 minuten met een geschiedenis van 120 gebeurtenissen. De laatste SELECT-instructie worden uitgepakt en levert de score en anomaliedetectie status met een vertrouwensniveau van 95%.

```SQL
WITH AnomalyDetectionStep AS
(
    SELECT
        EVENTENQUEUEDUTCTIME AS time,
        CAST(temperature AS float) AS temp,
        AnomalyDetection_SpikeAndDip(CAST(temperature AS float), 95, 120, 'spikesanddips')
            OVER(LIMIT DURATION(second, 120)) AS SpikeAndDipScores
    FROM input
)
SELECT
    time,
    temp,
    CAST(GetRecordPropertyValue(SpikeAndDipScores, 'Score') AS float) AS
    SpikeAndDipScore,
    CAST(GetRecordPropertyValue(SpikeAndDipScores, 'IsAnomaly') AS bigint) AS
    IsSpikeAndDipAnomaly
INTO output
FROM AnomalyDetectionStep
```

## <a name="change-point"></a>Punt wijzigen

Permanente afwijkingen in de gebeurtenisstroom van een time-series zijn wijzigingen in de distributie van de waarden in de gebeurtenisstroom, zoals wijzigingen op en trends. In Stream Analytics, dergelijke afwijkingen worden gedetecteerd met behulp van de Machine Learning op basis van [AnomalyDetection_ChangePoint](https://docs.microsoft.com/stream-analytics-query/anomalydetection-changepoint-azure-stream-analytics) operator.

Permanente wijzigingen veel langer dan pieken en dalen en kunnen duiden op catastrofale gebeurtenis(sen). Permanente wijzigingen zijn niet meestal zichtbaar is voor het blote oog, maar kan worden gedetecteerd met de **AnomalyDetection_ChangePoint** operator.

De volgende afbeelding is een voorbeeld van een niveau wijzigen:

![Voorbeeld van het niveau wijzigen van afwijkingen](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-level-change.png)

De volgende afbeelding is een voorbeeld van een wijziging trend:

![Voorbeeld van de trend wijzigen anomaliedetectie](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-trend-change.png)

De volgende voorbeeldquery wordt ervan uitgegaan dat een uniform invoer tarief van één gebeurtenis per seconde in een sliding window van 20 minuten met een grootte van de geschiedenis van 1200 gebeurtenissen. De laatste SELECT-instructie worden uitgepakt en levert de score en anomaliedetectie status met een vertrouwensniveau van 80%.

```SQL
WITH AnomalyDetectionStep AS
(
    SELECT
        EVENTENQUEUEDUTCTIME AS time,
        CAST(temperature AS float) AS temp,
        AnomalyDetection_ChangePoint(CAST(temperature AS float), 80, 1200) 
        OVER(LIMIT DURATION(minute, 20)) AS ChangePointScores
    FROM input
)
SELECT
    time,
    temp,
    CAST(GetRecordPropertyValue(ChangePointScores, 'Score') AS float) AS
    ChangePointScore,
    CAST(GetRecordPropertyValue(ChangePointScores, 'IsAnomaly') AS bigint) AS
    IsChangePointAnomaly
INTO output
FROM AnomalyDetectionStep

```

## <a name="performance-characteristics"></a>Prestatiekenmerken

De prestaties van deze modellen is afhankelijk van de grootte van de geschiedenis, de duur van het venster, de gebeurtenisbelasting, en of de functie level partitioneren is gebruikt. In deze sectie gaat over deze configuraties en bevat voorbeelden voor het handhaven van de tarieven voor gegevensopname van 1K, 5K en 10K gebeurtenissen per seconde.

* **Grootte van de geschiedenis** -deze modellen uitvoeren Lineair met **geschiedenis grootte**. Hoe langer de grootte van de geschiedenis, hoe langer duren de modellen om een nieuwe gebeurtenis te beoordelen. Dit komt doordat de modellen Vergelijk de nieuwe gebeurtenis met elk van de gebeurtenissen in de buffer in het verleden.
* **Duur van het venster** : de **duur van het venster** moet vergelijkbaar zijn met hoe lang het duurt om zo veel gebeurtenissen zoals opgegeven door de grootte van de geschiedenis ontvangen. Azure Stream Analytics zou ontbrekende waarden rekenen zonder dat veel gebeurtenissen in het venster. CPU-verbruik is daarom een functie van de grootte van de geschiedenis.
* **Gebeurtenisbelasting** : des te groter de **gebeurtenisbelasting**, hoe meer werk dat wordt uitgevoerd door de modellen die van invloed is op CPU-verbruik. De taak kan worden uitgebreid door deze perfect parallelle, ervan uitgaande dat het zinvol voor zakelijke logica voor het gebruik van meer invoer partities te maken.
* **Functie niveau partitioneren** - **functie niveau partitioneren** vindt plaats via ```PARTITION BY``` binnen de functieaanroep voor detectie van afwijkingen. Dit type partitioneren toegevoegd overhead, zoals status moet worden onderhouden voor meerdere modellen op hetzelfde moment. Functie niveau partitioneren wordt gebruikt in scenario's zoals apparaat niveau partitioneren.

### <a name="relationship"></a>Relatie
De geschiedenis van grootte, de duur van het venster en de totale gebeurtenisbelasting hebben betrekking op de volgende manier:

windowDuration (in ms) = 1000 * historySize / (totaal aantal invoer-gebeurtenissen Per seconde / aantal invoer-partities)

Wanneer u de functie door de apparaat-id, voeg toe 'partitie met de apparaat-id"aan de functieaanroep voor detectie van afwijkingen.

### <a name="observations"></a>Opmerkingen
De volgende tabel bevat de doorvoer-opmerkingen voor een enkel knooppunt (6 SU) voor het geval van niet-gepartitioneerde:

| Geschiedenis-grootte (gebeurtenissen) | Duur van het venster (ms) | Totaal aantal invoer gebeurtenissen per seconde |
| --------------------- | -------------------- | -------------------------- |
| 60 | 55 | 2,200 |
| 600 | 728 | 1,650 |
| 6,000 | 10,910 | 1,100 |

De volgende tabel bevat de doorvoer-opmerkingen voor een enkel knooppunt (6 SU) voor de gepartitioneerde aanvraag:

| Geschiedenis-grootte (gebeurtenissen) | Duur van het venster (ms) | Totaal aantal invoer gebeurtenissen per seconde | Aantal apparaten |
| --------------------- | -------------------- | -------------------------- | ------------ |
| 60 | 1,091 | 1,100 | 10 |
| 600 | 10,910 | 1,100 | 10 |
| 6,000 | 218,182 | <550 | 10 |
| 60 | 21,819 | 550 | 100 |
| 600 | 218,182 | 550 | 100 |
| 6,000 | 2,181,819 | <550 | 100 |

Van voorbeeldcode voor het uitvoeren van de bovenstaande niet-gepartitioneerde configuraties bevindt zich in de [Streaming op schaal opslagplaats](https://github.com/Azure-Samples/streaming-at-scale/blob/f3e66fa9d8c344df77a222812f89a99b7c27ef22/eventhubs-streamanalytics-eventhubs/anomalydetection/create-solution.sh) van Azure-voorbeelden. De code maakt u een stream analytics-taak met geen functie niveau partitionering, dat gebruikmaakt van Event Hub als invoer en uitvoer. De invoer belasting is gegenereerd met behulp van testclients. Elke invoer gebeurtenis is een json-document van 1KB. Gebeurtenissen simuleren een IoT-apparaat voor het verzenden van JSON-gegevens (voor maximaal 1 K-apparaten). De geschiedenis van grootte, de duur van het venster en de totale gebeurtenisbelasting zijn via 2 invoer partities verschillend.

> [!Note]
> Voor een meer nauwkeurige schatting en pas de voorbeelden voor uw scenario.

### <a name="identifying-bottlenecks"></a>Identificeren van knelpunten
Gebruik het deelvenster met metrische gegevens in uw Azure Stream Analytics-taak voor het identificeren van knelpunten in de pijplijn. Beoordeling **i/o-gebeurtenissen** voor doorvoer en ["Watermerk vertraging"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) of **van achterstallige gebeurtenissen** om te zien als de taak is dat de invoer snelheid. Zoek voor metrische gegevens van Event Hub, **aanvragen beperkt** en de drempelwaarde-eenheden overeenkomstig aanpassen. Raadpleeg voor metrische gegevens voor Cosmos DB, **maximaal aantal gebruikte RU/s per partitiesleutelbereik** onder doorvoer om te controleren of de partitie sleutelbereiken op uniforme wijze worden verbruikt. Controleer voor de Azure SQL DB **logboek-IO** en **CPU**.

## <a name="anomaly-detection-using-machine-learning-in-azure-stream-analytics"></a>Afwijkingsdetectie met machine learning in Azure Stream Analytics

De volgende video ziet u hoe u een anomalie detecteren in realtime met machine learning-functies in Azure Stream Analytics. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Anomaly-detection-using-machine-learning-in-Azure-Stream-Analytics/player]

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

