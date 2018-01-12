---
title: Afwijkingsdetectie in Azure Usage Guide (Preview) | Microsoft Docs
description: Gebruik stream analytics en machine learning-afwijkingen gedetecteerd.
services: stream-analytics
documentationcenter: 
author: dubansal
manager: jhubbard
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: dubansal
ms.openlocfilehash: ff8571c6447f32ef9a435f5200803e76f6013ffa
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="using-the-anomalydetection-operator"></a>Met behulp van de operator ANOMALYDETECTION

> [!IMPORTANT]
> Deze functionaliteit is in preview. We raden niet gebruiken in productie.

De **ANOMALYDETECTION** operator kan worden gebruikt voor het detecteren van afwijkingen in gebeurtenisstromen.
Bijvoorbeeld, een trage afname in het vrije geheugen gedurende een lange periode indicatief voor een geheugenlek kan worden of het aantal web service-aanvragen dat in een bereik stabiel mogelijk aanzienlijk verhogen of verlagen.

Er wordt gecontroleerd op de volgende typen afwijkingen kunnen worden gedetecteerd via de opgegeven duur:

- De wijzigingen in twee richtingen
- Trage positieve trend
- Trage negatieve trend

Het tijdsinterval voor hoe ver terug in de geschiedenis van de huidige gebeurtenis moet worden gekeken is beperkt met behulp van de **LIMIT DURATION** component. **ANOMALYDETECTION** kan eventueel worden beperkt tot alleen de gebeurtenissen die overeenkomen met een bepaalde eigenschap of voorwaarde met behulp van de **wanneer** component.

Deze kan eventueel ook verwerken groepen gebeurtenissen afzonderlijk op basis van de sleutel die is opgegeven in de **PARTITION BY** component. Training en voorspelling optreden onafhankelijk in elke partitie.

## <a name="syntax"></a>Syntaxis

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 


## <a name="example-usage"></a>Voorbeeld van syntaxis

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`|


## <a name="arguments"></a>Argumenten

- **scalar_expression**

  De scalaire expressie gedurende welke de afwijkingsdetectie zouden worden uitgevoerd. Er is een expressie van het type float of bigint die resulteert in een enkele waarde (scalaire). De expressie met jokertekens  **\***  is niet toegestaan. **scalar_expression** mag geen andere analytische functies en externe functies.

- **VIA ([partition_by_clause] limit_duration_clause [when_clause])**

- **partition_by_clause** 

  De `PARTITION BY <partition key>` component verdeelt de learning en training van meerdere afzonderlijke partities. Met andere woorden, een afzonderlijk model moet worden gebruikt per waarde voor `<partition key>` en alleen gebeurtenissen met de waarde moeten worden gebruikt voor learning en opleiding in dit model. Bijvoorbeeld:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

  wordt trainen en te beoordelen op basis van andere metingen van de dezelfde sensor alleen een bij het lezen.

- **de component limit_duration** duur (\<eenheid\>, \<lengte\>)

  Hiermee geeft u op hoeveel van de geschiedenis van de huidige gebeurtenis wordt beschouwd als de **ANOMALYDETECTION** berekening. Zie DATEDIFF voor een gedetailleerde beschrijving van de ondersteunde maateenheden en hun afkortingen.

- **when_clause** 

  Hiermee geeft u een Boole-voorwaarde voor de gebeurtenissen worden overwogen in het **ANOMALYDETECTION** berekening.

## <a name="return-types"></a>Retourtypen

De functie retourneert een Record die alle drie scores als uitvoer ervan weergegeven. De eigenschappen die zijn gekoppeld aan de verschillende soorten afwijkingsdetectie detectoren worden genoemd:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Als u wilt de afzonderlijke waarden uit de record extraheren, gebruikt u de **GetRecordPropertyValue** functie. Bijvoorbeeld:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 


Een afwijkingsdetectie van een bepaald type wordt gedetecteerd wanneer een van deze scores afwijkingsdetectie een drempelwaarde overschrijdt. De drempelwaarde mag een drijvende-kommagetal \>= 0. De drempelwaarde is een afweging tussen het gevoeligheid en vertrouwen. Bijvoorbeeld, zou een lagere drempelwaarde detectie gevoeliger voor wijzigingen aanbrengen en meer waarschuwingen genereren, terwijl een hogere drempelwaarde kan ervoor zorgen dat detectie minder gevoelig en meer vertrouwen, maar sommige afwijkingen maskeren. De exacte drempelwaarde moet worden gebruikt, is afhankelijk van het scenario. Er is geen bovengrens maar aanbevolen bereik 3,25 5 is.

## <a name="algorithm"></a>Algoritme

**ANOMALYDETECTION** venster semantiek, wat betekent dat de berekening wordt uitgevoerd per gebeurtenis die voert u de functie en een score wordt geproduceerd voor die gebeurtenis Verschuivend gebruikt. De berekening is gebaseerd op kunnen Martingales die werken door te controleren of de distributie van de waarden van de gebeurtenis is gewijzigd. Zo ja, er is een mogelijke afwijkingsdetectie gedetecteerd. De geretourneerde score is een indicatie van het niveau van betrouwbaarheid van die afwijkingsdetectie. Als een interne optimalisatie **ANOMALYDETECTION** berekent de score van de afwijkingsdetectie van een gebeurtenis op basis van *d* naar *2d* gebeurtenissen, waarbij *d*is de opgegeven detectie venstergrootte.

**ANOMALYDETECTION** invoer tijdreeks uniform worden verwacht. Een stroom gebeurtenissen kan uniform worden gemaakt door via een daling aggregeren of hopping venster. In scenario's waarin het hiaat tussen gebeurtenissen altijd kleiner zijn dan het venster aggregatie, is een tumblingvenster voldoende zijn voor het maken van de tijdreeks uniform. Wanneer de ruimte kan niet groter zijn, kunnen de hiaten door te herhalen van de laatste waarde met behulp van een hoppingvenster worden ingevuld. Deze beide scenario's kunnen worden verwerkt door het volgende voorbeeld.

## <a name="performance-guidance"></a>Richtlijnen voor prestaties

- Gebruik 6 SU voor taken. 
- Verzenden van gebeurtenissen ten minste 1 seconde uit elkaar liggen.
- Een niet-gepartitioneerde query met de **ANOMALYDETECTION** functie resultaten kan worden verkregen met een berekening latentie van ongeveer 25 MS gemiddeld.
- De latentie is opgetreden door een gepartitioneerde query verschilt enigszins met het aantal partities, zoals het aantal berekeningen hoger is. De latentie is echter hetzelfde als de aanvraag niet-gepartitioneerde voor een vergelijkbare totaal aantal gebeurtenissen voor alle partities.
- Tijdens het lezen van gegevens real time, een grote hoeveelheid gegevens snel ingenomen. Verwerken van deze gegevens is momenteel aanzienlijk tragere. De latentie bij scenario's is te verhogen Lineair met het aantal gegevenspunten in het venster in plaats van het venster grootte of gebeurtenis interval per definitie gevonden. Als u de latentie in real time gevallen, kunt u overwegen een kleinere venster. Houd ook rekening met uw taak starten vanuit de huidige tijd. Enkele voorbeelden van latenties in een niet-gepartitioneerde query: 
    - 60 gegevenspunten in het venster detectie kunnen resulteren in een latentie van 10 seconden met een doorvoersnelheid van 3 MBps. 
    - De latentie kan ongeveer 80 seconden met een doorvoersnelheid van 0,4 MBps bereiken op 600 gegevenspunten.

## <a name="example"></a>Voorbeeld

De volgende query kan worden gebruikt voor uitvoer van een waarschuwing als er een afwijkingsdetectie wordt gedetecteerd.
Wanneer de invoerstroom niet uniform, moet u de stap aggregatie kunt transformeren in een uniform tijdreeks. In het voorbeeld wordt **Gem** maar het specifieke type van de aggregatie is afhankelijk van het gebruikersscenario. Bovendien, als een tijdreeks hiaten groter is dan het venster aggregatie heeft, zal er geen gebeurtenissen in de tijdreeks voor afwijkingsdetectie trigger (volgens de semantiek voor venster Verschuivend). De veronderstelling van homogeniteit worden hierdoor verbroken wanneer de volgende gebeurtenis ontvangen. In dergelijke gevallen moet een manier invullen in de hiaten in de tijdreeks. Een mogelijke aanpak is het nemen van de laatste gebeurtenis in alle vensters hop, zoals hieronder wordt weergegeven.

    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) >= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) >=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) >=
       3.25

## <a name="references"></a>Verwijzingen

* [Afwijkingsdetectie – gebruik van Machine Learning afwijkingen in de reeks tijdgegevens detecteren](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Machine Learning eigen Afwijkingsdetectie API](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Time Series-Afwijkingsdetectie](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

