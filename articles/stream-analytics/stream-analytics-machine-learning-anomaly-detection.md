---
title: Anomaliedetectie in Azure Stream Analytics (Preview)
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics en Azure Machine Learning samen gebruiken voor het detecteren van afwijkingen.
services: stream-analytics
author: dubansal
ms.author: dubansal
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: 2f35f54c7ec5ad169673aebe08602294270f470a
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364452"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Anomaliedetectie in Azure Stream Analytics

> [!IMPORTANT]
> Deze functionaliteit wordt afgeschaft, maar wordt vervangen door nieuwe functies. Voor meer informatie gaat u naar de [acht nieuwe functies in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) blogbericht.

De **AnomalyDetection** operator wordt gebruikt voor het detecteren van verschillende typen anomalieën in gebeurtenisstromen. Bijvoorbeeld, een trage afname in geheugen vrij te maken over een lange periode kan zijn voor een geheugenlek, of het aantal aanvragen van de web-service die zich in een bereik stabiel aanzienlijk kan vergroten of verkleinen.  

De operator AnomalyDetection detecteert drie typen anomalieën: 

* **Niveau wijzigen van bidirectionele**: een aanhoudende vergroten of verkleinen van het niveau van de waarden, omhoog en omlaag. Deze waarde verschilt van pieken en dips n, die onmiddellijk of eenvoudige wijzigingen zijn.  

* **Browserpagina's met trage positieve Trend**: een trage toename van de trend gedurende een periode.  

* **Browserpagina's met trage negatieve Trend**: een trage afname in de trend gedurende een periode.  

Wanneer u de operator AnomalyDetection gebruikt, moet u de **Limit Duration** component. Deze component Hiermee geeft u dat het tijdsinterval (hoe ver terug in de geschiedenis van de huidige gebeurtenis) moet worden gehouden bij het detecteren van afwijkingen. Deze operator kan eventueel worden beperkt tot alleen de gebeurtenissen die overeenkomen met een bepaalde eigenschap of voorwaarde met behulp van de **wanneer** component. Deze operator kan eventueel ook verwerken voor groepen van gebeurtenissen afzonderlijk op basis van de sleutel die is opgegeven de **partitioneren door** component. Trainen en voorspellen optreden afzonderlijk voor elke partitie. 

## <a name="syntax-for-anomalydetection-operator"></a>Syntaxis voor de operator AnomalyDetection

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Voorbeeld van gebruik**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Argumenten

* **scalar_expression** -de scalaire expressie die de detectie van afwijkingen wordt uitgevoerd. Toegestane waarden voor deze parameter Float bevatten of dat een enkele (scalaire) retourwaarde van de gegevenstypen Bigint. De expressie met jokertekens **\*** is niet toegestaan. Scalaire expressie die bevatten geen andere analytische functies of externe functies. 

* **partition_by_clause** : de `PARTITION BY <partition key>` component verdeelt het onderwijs en training over afzonderlijke partities. Met andere woorden, een afzonderlijke model zouden worden gebruikt per waarde van `<partition key>` en alleen gebeurtenissen met de waarde voor onderwijs en training in dit model zou worden gebruikt. Bijvoorbeeld, de volgende query treinen en scores a lezen op basis van andere meetwaarden van alleen de dezelfde sensor:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **limit_duration component** `DURATION(<unit>, <length>)` -Hiermee geeft u het tijdsinterval (hoe ver terug in de geschiedenis van de huidige gebeurtenis) moet worden gehouden bij het detecteren van afwijkingen. Zie [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) voor een gedetailleerde beschrijving van de ondersteunde eenheden en hun afkortingen. 

* **when_clause** -Hiermee geeft u een Boole-voorwaarde voor de gebeurtenissen die zijn opgenomen in de berekening van de detectie van afwijkingen.

### <a name="return-types"></a>Typen retourneren

De operator AnomalyDetection retourneert een record die alle drie scores als de uitvoer ervan weergegeven. De eigenschappen die zijn gekoppeld aan de verschillende typen afwijkingen detectoren zijn:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Als u wilt de afzonderlijke waarden uit de record ophalen, gebruikt u de **GetRecordPropertyValue** functie. Bijvoorbeeld:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Anomaliedetectie van een type wordt gedetecteerd wanneer een van de anomaliedetectie-scores een drempel overschrijden. De drempelwaarde kan een getal met drijvende komma zijn > = 0. De drempelwaarde is een verschil tussen de gevoeligheid en het vertrouwen. Bijvoorbeeld, een lagere drempelwaarde detectie gevoeliger voor wijzigingen aanbrengen en meer waarschuwingen genereren, terwijl een hogere drempelwaarde kan detectie minder gevoelig en meer vertrouwen maken, maar sommige afwijkingen maskeren. De exacte drempelwaarde te gebruiken, is afhankelijk van het scenario. Er is geen bovengrens, maar de aanbevolen bereik is 3,25 5. 

De waarde 3,25 weergegeven in het voorbeeld is slechts een voorgestelde uitgangspunt. De waarde door het uitvoeren van de bewerkingen op uw eigen gegevensset verfijnen, en bekijk de uitvoerwaarde totdat u een toegestane drempelwaarde bereikt.

## <a name="anomaly-detection-algorithm"></a>Algoritme voor de detectie

* Maakt gebruik van de operator AnomalyDetection een **leren zonder supervisie** benadering waar deze wordt niet vanuit gegaan elk type distributie in de gebeurtenissen. In het algemeen worden twee modellen bijgehouden in parallel op elk moment, waarbij een van deze wordt gebruikt voor het scoren van en de andere op de achtergrond wordt getraind. De detectie van afwijkingen modellen worden getraind gegevens uit de huidige stream gebruiken in plaats van met behulp van een out-of-band-mechanisme. De hoeveelheid gegevens die worden gebruikt voor trainingen, is afhankelijk van het venster grootte d is opgegeven door de gebruiker binnen de Limit Duration-parameter. Elk model eindigt trainingen op basis van d van 2d aan gebeurtenissen. Het wordt aanbevolen dat ten minste 50 gebeurtenissen in elk venster voor de beste resultaten. 

* Maakt gebruik van de operator AnomalyDetection **venster semantiek Verschuivend** met het trainen van modellen en score gebeurtenissen. Wat betekent dat elke gebeurtenis wordt geëvalueerd voor anomaliedetectie en een score wordt geretourneerd. De score is een indicatie van het niveau van betrouwbaarheid van die afwijkingen. 

* De operator AnomalyDetection biedt een **herhaalbaarheid garantie** dezelfde invoer altijd produceert de dezelfde score, ongeacht de uitvoer van de begintijd. De begintijd van de taak uitvoer geeft de tijd waarop de eerste uitvoergebeurtenis wordt geproduceerd door de taak. Deze waarde is ingesteld door de gebruiker naar de huidige tijd, een aangepaste waarde of de laatste uitvoertijd (als de taak eerder uitvoer heeft geproduceerd). 

### <a name="training-the-models"></a>De modellen trainen 

Als de voortgang van de tijd, modellen worden getraind met andere gegevens. Als u wilt zinvol in de scores, kunt zo u inzicht in de onderliggende mechanisme waarmee de modellen worden getraind. Hier **t<sub>0</sub>**  is de **starttijd voor taakuitvoer** en **d** is de **venstergrootte** van de duur van de limiet de parameter. Wordt ervan uitgegaan dat het tijd is onderverdeeld in **hops van grootte d**, vanaf `01/01/0001 00:00:00`. De volgende stappen uit worden om te trainen het model en score de gebeurtenissen gebruikt:

* Wanneer een taak wordt gestart, is het ingesteld op gegevens die beginnen op het tijdstip t<sub>0</sub> – 2d.  
* Wanneer u de volgende hop wordt bereikt, een nieuw model M1 wordt gemaakt en gestart trainingen.  
* Wanneer de tijd wordt bestuurd door een andere hop, een nieuw model M2 wordt gemaakt en gestart trainingen.  
* Wanneer de tijd t bereikt<sub>0</sub>M1 actief is gemaakt en de score wordt gestart met het ophalen van output.  
* Op de volgende hop drie dingen gebeuren op hetzelfde moment:  

  * M1 is niet meer nodig en wordt deze verwijderd.  
  * M2 is voldoende getraind, zodat deze wordt gebruikt voor het scoren.  
  * Een nieuw model M3 wordt gemaakt en wordt gestart op de achtergrond ophalen getraind.  

* Deze cyclus wordt herhaald voor elke hop wanneer het actieve model wordt verwijderd, Ga naar de parallelle model, en start met het trainen van een derde model op de achtergrond. 

Schematisch aangegeven, de stappen er als volgt uitzien: 

![Modellen voor training](media/stream-analytics-machine-learning-anomaly-detection/training_model.png)

|**Model** | **Begintijd van de training** | **Tijd om te gaan met behulp van de score** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Model M1 training om 11:20 uur, dit is de volgende hop nadat de taak wordt gestart lezen om 11:13 am wordt gestart. De eerste uitvoer wordt geproduceerd van M1 om 11:33 uur na de training met 13 minuten aan gegevens. 

* Een nieuw model M2 gestart ook training om 11:30 uur, maar de score is niet tot 11:40 uur, dit is nadat deze is getraind met 10 minuten aan gegevens ophalen gebruikt. 

* M3 volgt hetzelfde patroon als M2. 

### <a name="scoring-with-the-models"></a>Met de modellen scoren 

Op het niveau van de Machine Learning berekent het algoritme voor de detectie een strangeness-waarde voor elke inkomende gebeurtenis door deze te vergelijken met gebeurtenissen in een geschiedenisvenster. De berekening strangeness verschilt voor elk type afwijkingen.  

Laten we de berekening strangeness in detail bekijken (wordt ervan uitgegaan dat een set van historische windows met gebeurtenissen bestaat): 

1. **Niveau wijzigen van bidirectionele:** op basis van het geschiedenisvenster, een normale operationele bereik wordt berekend als [10th percentiel, 90e percentiel] dat wil zeggen, 10 percentielwaarde als de ondergrens en 90e percentielwaarde als de bovengrens. Een strangeness-waarde voor de huidige gebeurtenis is als volgt berekend:  

   - Als event_value in normale operationele bereik is 0  
   - event_value/90th_percentile als event_value > 90th_percentile  
   - 10th_percentile/event_value, als de event_value < 10th_percentile  

2. **Trage positieve trend:** een trendlijn via de waarden van de gebeurtenis in het geschiedenisvenster wordt berekend en wordt de bewerking wordt gekeken naar een positieve trend binnen de regel. De waarde strangeness is als volgt berekend:  

   - Helling, als helling positief is  
   - 0 is, anders 

3. **Trage negatieve trend:** een trendlijn via de waarden van de gebeurtenis in het geschiedenisvenster wordt berekend en wordt de bewerking wordt gekeken naar negatieve trend binnen de regel. De waarde strangeness is als volgt berekend: 

   - Helling, als helling negatief is  
   - 0 is, anders  

Zodra de strangeness-waarde voor de inkomende gebeurtenis wordt berekend, een martingale-waarde wordt berekend op basis van de waarde strangeness (Zie de [Machine Learning-blog](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) voor meer informatie over hoe de martingale-waarde is berekend). Deze waarde martingale is retuned als de anomaliedetectie-score. De waarde martingale verhoogd langzaam in reactie op vreemde waarden, zodat de detector robuuste sporadisch wijzigingen blijven en vermindert valse waarschuwingen. Het bevat ook een handige eigenschap: 

Waarschijnlijkheid [er t dergelijke M<sub>t</sub> > λ] < 1/λ, waar M<sub>t</sub> is de waarde martingale instant t en λ is een werkelijke waarde. Bijvoorbeeld, als er een waarschuwing wanneer M<sub>t</sub>> 100 en de kans van fout-positieven is minder dan 1/100.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>Richtlijnen voor het gebruik van het niveau van bidirectionele detector wijzigen 

Het niveau wijzigen van bidirectionele detector kan worden gebruikt in scenario's, zoals power onderbreking en herstel of spits verkeer, enzovoort. Het werkt echter zodanig dat nadat een model wordt getraind met bepaalde gegevens, een ander niveau wijziging afwijkende is als de nieuwe waarde hoger dan de vorige bovengrens is (hoofdlettergebruik opwaartse niveau) of lager is dan de vorige lagere limiet (neerwaartse niveau aanvraag wijzigen). Daarom kan ziet een model niet gegevenswaarden in het bereik van de nieuwe niveau (omhoog of omlaag) in het venster training om te worden beschouwd als afwijkend. 

De volgende punten moeten worden gehouden bij met deze detectie: 

1. Wanneer de tijdreeksen plotseling ziet een toename of verwijderen in de waarde, de operator AnomalyDetection wordt dit gedetecteerd. Maar terug te keren naar de normale detecteren vereist meer planning. Als er zich een tijdreeks in onveranderlijke voordat de afwijkingen die kan worden bereikt door het venster voor detectie van de operator AnomalyDetection instellen op maximaal helft van de lengte van de afwijkingen. In dit scenario wordt ervan uitgegaan dat de minimale duur van de afwijkingen kan worden geschat vooraf en er onvoldoende gebeurtenissen in die periode wordt voltooid zijn met het trainen van het model genoeg (ten minste 50 gebeurtenissen). 

   Dit wordt weergegeven in afbeelding 1 en 2 hieronder met behulp van een wijziging in de bovenste limiet (dezelfde logica is van toepassing op een lagere limiet wijzigen). In beide afbeeldingen zijn de golfvormen een afwijkende niveau wijzigen. Verticale oranje regels geven hop grenzen en de grootte van de vensterverplaatsing is hetzelfde als de opgegeven in de operator AnomalyDetection venster voor detectie. De groene regels geven aan de grootte van het venster training. In afbeelding 1 is de grootte van de vensterverplaatsing hetzelfde als de tijd voor welke anomaliedetectie duurt. In afbeelding 2 is de grootte van de vensterverplaatsing helft van de tijd die de anomalie duurt. In alle gevallen wordt een opwaartse wijzigen omdat het model dat wordt gebruikt voor het scoren is getraind op normale gegevens gedetecteerd. Maar op basis van de werking van de detector bidirectionele niveau wijzigen, moet deze de normale waarden uitsluiten van de training venster dat wordt gebruikt voor het model dat terug te keren naar de normale beoordeelt. In afbeelding 1 bevat de scoring modeltraining sommige normale gebeurtenissen, zodat terug te keren naar de normale kan niet worden gedetecteerd. Maar in afbeelding 2, bevat de training alleen de afwijkende onderdeel, dat zorgt ervoor dat er terug te keren naar de normale is gedetecteerd. Codes kleiner dan de helft werkt ook voor dezelfde reden dat iets groter wordt uiteindelijk waaronder een deel van de normale gebeurtenissen. 

   ![AD met de lengte van venster Grootte gelijk anomaliedetectie](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_anomaly_length.png)

   ![AD met venstergrootte gelijk is aan de helft van de lengte van afwijkingen](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_half_anomaly_length.png)

2. In gevallen waarbij de lengte van de afwijkingen kan niet worden voorspeld, werkt deze detector ten best inspanning. Echter kiezen dat een smaller tijdvenster beperkt de trainingsgegevens, die zou vergroot de kans op terug te keren naar de normale detecteren. 

3. In het volgende scenario, is niet de langer anomalie als het venster training al een afwijking van dezelfde hoge waarde bevat gedetecteerd. 

   ![Afwijkingen met dezelfde grootte](media/stream-analytics-machine-learning-anomaly-detection/anomalies_with_same_length.png)

## <a name="example-query-to-detect-anomalies"></a>Voorbeeld van een query voor het detecteren van afwijkingen 

De volgende query kan worden gebruikt om uit te voeren van een waarschuwing als er een anomalie wordt gedetecteerd.
Wanneer de invoerstroom niet uniform, moet u de aggregatie-stap kunt transformeren in een uniform tijdreeks. Het voorbeeld wordt de gemiddelde, maar het specifieke type aggregatie, is afhankelijk van de gebruikersscenario. Bovendien, als een tijdreeks hiaten groter is dan de aggregatie-venster heeft, er zijn niet alle gebeurtenissen in de tijdreeks anomaliedetectie van trigger (zoals deze graduele venster semantiek). Als gevolg hiervan is de veronderstelling van uniformiteit verbroken wanneer de volgende gebeurtenis ontvangen. In dergelijke gevallen moeten de hiaten in de tijdreeks worden ingevuld. Een mogelijke aanpak is om de laatste gebeurtenis te in elke hop-venster, zoals hieronder wordt weergegeven.

```sql
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
```

## <a name="performance-guidance"></a>Prestatierichtlijnen

* Zes streaming-eenheden gebruik voor taken. 
* Gebeurtenissen verzenden ten minste één seconde uit elkaar liggen.
* Een query met niet-gepartitioneerde die van de operator AnomalyDetection gebruikmaakt kan resultaten opleveren met een latentie van de berekening van ongeveer 25 ms gemiddeld.
* De latentie die wordt ervaren door een gepartitioneerde query wijkt licht af met het aantal partities, zoals het aantal berekeningen hoger is. De latentie is echter hetzelfde als de aanvraag niet-gepartitioneerde voor een vergelijkbare totaal aantal gebeurtenissen voor alle partities.
* Tijdens het lezen van gegevens in realtime plaatsvindt, wordt er een grote hoeveelheid gegevens snel opgenomen. Verwerken van deze gegevens is momenteel langzamer. De latentie in dergelijke scenario's is te verhogen Lineair met het aantal gegevenspunten in het venster in plaats van het venster grootte of gebeurtenis-interval gevonden. Als u wilt de latentie in real time gevallen beperken, kunt u overwegen een kleinere venster. Houd er ook rekening met uw taak starten vanaf de huidige tijd. Enkele voorbeelden van latenties in een niet-gepartitioneerde-query: 
    - 60 gegevenspunten in het venster voor detectie kunnen resulteren in een latentie van 10 seconden met een doorvoersnelheid van 3 MBps. 
    - Op 600 gegevenspunten, kan de latentie van ongeveer 80 seconden met een doorvoersnelheid van 0,4 MBps bereiken.

## <a name="limitations-of-the-anomalydetection-operator"></a>Beperkingen van de operator AnomalyDetection 

* Deze operator biedt momenteel geen ondersteuning piek- en dip-detectie. Pieken en dips n zijn spontaan of eenvoudige wijzigingen in de tijdreeks. 

* Deze operator verwerkt op dit moment niet seizoensgebonden patronen. Periodieke variatie patronen zijn herhaalde patronen in de gegevens, bijvoorbeeld een andere web-verkeer-gedrag tijdens het weekend of andere winkelwagen trends tijdens het Black Friday, die niet de afwijkingen, maar een verwacht patroon in gedrag. 

* Deze operator wordt verwacht dat de ingevoerde timeseries die moet worden uniform. Een gebeurtenissenstroom kan worden gemaakt uniform door via een tumblingvenstertrigger aggregeren of venster Hopping plaatsvindt. In scenario's waarin de kloof tussen gebeurtenissen altijd kleiner zijn dan de aggregatie-venster, wordt een tumblingvenster voldoende zijn voor het maken van de tijdreeks uniform. Wanneer de onderbreking groter zijn kan, kunnen de hiaten door te herhalen van de laatste waarde met een hoppingvenster worden ingevuld. 

## <a name="references"></a>Verwijzingen

* [Detectie van afwijkingen: werken met machine learning voor het detecteren van afwijkingen in time series-gegevens](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Machine learning-anomaliedetectie-API](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Time series-anomaliedetectie](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

