---
title: Anomaliedetectie in Azure Stream Analytics (Preview)
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics en Azure Machine Learning samen gebruiken voor het detecteren van afwijkingen.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.custom: seodec18
ms.openlocfilehash: bdd512972f1a684a3b76ae0323bbadd87bf0d659
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238314"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Anomaliedetectie in Azure Stream Analytics

Azure Stream Analytics biedt ingebouwde machine learning-detectiemogelijkheden op basis van afwijkingen die kunnen worden gebruikt voor het bewaken van de twee meest voorkomende anomalieën: tijdelijke en permanente. Met de **AnomalyDetection_SpikeAndDip** en **AnomalyDetection_ChangePoint** functies, rechtstreeks in uw Stream Analytics-taak kunt u anomaliedetectie uitvoeren.

De machine learning-modellen wordt ervan uitgegaan dat een uniforme wijze sample tijdreeks. Als de tijdreeks niet uniform is, kunt u een stap aggregatie met een tumblingvenster vóór het aanroepen van detectie van afwijkingen kunt invoegen.

De machine learning-bewerkingen ondersteunen geen periodieke variatie trends of meerdere variate correlaties.

## <a name="model-accuracy-and-performance"></a>Model nauwkeurigheid en prestaties

Over het algemeen verbetert de nauwkeurigheid van het model met meer gegevens in de sliding window van. De gegevens in de opgegeven sliding window van wordt behandeld als onderdeel van het normale bereik van waarden voor die periode wordt voltooid. Het model alleen rekening gehouden met geschiedenis van gebeurtenissen gedurende de sliding window van moet worden gecontroleerd als de huidige gebeurtenis afwijkend. Als u de sliding window van verplaatst, worden oude waarden uit de training van het model verwijderd.

De functies worden uitgevoerd door het tot stand brengen van een bepaalde normale op basis van wat ze tot nu toe hebben gezien. Uitbijters worden geïdentificeerd door het vergelijken met het tot stand gebrachte normaal, binnen het niveau van betrouwbaarheid. De venstergrootte moet worden gebaseerd op de minimale gebeurtenissen die zijn vereist voor het trainen van het model voor het normale gedrag, zodat wanneer een anomalie wordt gedetecteerd, het kunnen herkennen deze zou zijn.

Houd er rekening mee dat de reactietijd van het model wordt verhoogd met een grootte van de geschiedenis omdat nodig is om te vergelijken met een groter aantal gebeurtenissen in het verleden. Het verdient aanbeveling om op te nemen alleen het benodigde aantal gebeurtenissen voor betere prestaties.

Hiaten in de tijdreeks kunnen het gevolg zijn van het model niet ontvangen van gebeurtenissen op bepaalde tijdstippen in de tijd. Deze situatie wordt verzorgd door Stream Analytics met behulp van toerekening. De grootte van de geschiedenis, evenals een tijdsduur voor de sliding window van dezelfde wordt gebruikt voor het berekenen van de gemiddelde frequentie van waarmee gebeurtenissen worden verwacht aankomt.

## <a name="spike-and-dip"></a>Piek- en dip

Tijdelijke afwijkingen in de gebeurtenisstroom van een time-serie zijn voorzien van zoals pieken en dalen. Pieken en dips n de kunnen worden bewaakt met behulp van de operator op Machine Learning gebaseerde **AnomalyDetection_SpikeAndDip**.

![Voorbeeld van een piek- en dip-anomaliedetectie](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-spike-dip.png)

In de sliding window van dezelfde, als een tweede piek kleiner dan de eerste versie is, is de berekende score voor de kleinere piek waarschijnlijk niet aanzienlijke voldoende ten opzichte van de score voor de eerste piek in het niveau van betrouwbaarheid opgegeven. U kunt vertrouwen dat de instelling van het model om af te vangen van dergelijke afwijkingen verlagen. Als u begint met te veel waarschuwingen ontvangen, kunt u echter een hogere betrouwbaarheidsinterval gebruiken.

De volgende voorbeeldquery wordt ervan uitgegaan dat een uniform invoer snelheid van 1 gebeurtenis per seconde in een 2 minuten-venster met een geschiedenis van 120 gebeurtenissen schuiven. De laatste SELECT-instructie worden uitgepakt en levert de score en anomaliedetectie status met een vertrouwensniveau van 95%.

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

Permanente afwijkingen in de gebeurtenisstroom van een time-series zijn wijzigingen in de distributie van de waarden in de gebeurtenisstroom, zoals wijzigingen op en trends. In Stream Analytics, dergelijke afwijkingen worden gedetecteerd met behulp van de Machine Learning op basis van **AnomalyDetection_ChangePoint** operator.

Permanente wijzigingen veel langer dan pieken en dalen en kunnen duiden op catastrofale gebeurtenis(sen). Permanente wijzigingen zijn meestal niet gemakkelijk met het blote oog zichtbaar, maar kan worden gedetecteerd met de **AnomalyDetection_ChangePoint** operator.

De volgende afbeelding is een voorbeeld van een niveau wijzigen:

![Voorbeeld van het niveau wijzigen van afwijkingen](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-level-change.png)

De volgende afbeelding is een voorbeeld van een wijziging trend:

![Voorbeeld van de trend wijzigen anomaliedetectie](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-trend-change.png)

De volgende voorbeeldquery wordt ervan uitgegaan dat een uniform invoer snelheid van 1 gebeurtenis per seconde in een 20 minuten Verschuivend-venster met een grootte van de geschiedenis van 1200-gebeurtenissen. De laatste SELECT-instructie worden uitgepakt en levert de score en anomaliedetectie status met een vertrouwensniveau van 80%.

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

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.ASpx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

