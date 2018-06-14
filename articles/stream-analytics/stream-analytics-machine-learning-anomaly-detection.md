---
title: Afwijkingsdetectie in Azure Stream Analytics (Preview)
description: In dit artikel wordt beschreven hoe Azure Stream Analytics en Azure Machine Learning samen gebruiken voor het detecteren van afwijkingen.
services: stream-analytics
author: dubansal
ms.author: dubansal
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: e7274e4507d901a209ed5832e98ca630feefda4f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31420092"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Afwijkingsdetectie in Azure Stream Analytics

> [!IMPORTANT]
> Deze functionaliteit is een Preview-versie, is niet raadzaam om met de productie-workloads.

De **AnomalyDetection** operator wordt gebruikt voor het detecteren van verschillende typen afwijkingen in gebeurtenisstromen. Bijvoorbeeld, een trage afname in het vrije geheugen gedurende een lange periode indicatief voor een geheugenlek kan worden of het aantal web service-aanvragen dat in een bereik stabiel mogelijk aanzienlijk verhogen of verlagen.  

De operator AnomalyDetection detecteert drie typen afwijkingen: 

* **Bidirectionele niveau wijzigen**: een volgehouden vergroten of verkleinen van het niveau van de waarden, omhoog en omlaag. Deze waarde verschilt van pieken en dips krijgen onmiddellijk of tijdelijke wijzigingen zijn.  

* **Trage positieve Trend**: een trage toename van de trend gedurende een bepaalde periode.  

* **Trage negatieve Trend**: een trage afname in de trend gedurende een bepaalde periode.  

Wanneer u de operator AnomalyDetection gebruikt, moet u de **Limit Duration** component. Deze component Hiermee geeft u dat het tijdsinterval (hoe ver terug in de geschiedenis van de huidige gebeurtenis) moet worden beschouwd bij het detecteren van afwijkingen. Deze operator kan eventueel worden beperkt tot alleen de gebeurtenissen die overeenkomen met een bepaalde eigenschap of voorwaarde met behulp van de **wanneer** component. Deze operator kan eventueel ook verwerken voor groepen van gebeurtenissen afzonderlijk op basis van de sleutel die is opgegeven in de **partitioneren door** component. Training en voorspelling optreden onafhankelijk voor elke partitie. 

## <a name="syntax-for-anomalydetection-operator"></a>Syntaxis voor AnomalyDetection operator

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Voorbeeld van syntaxis**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Argumenten

* **scalar_expression** -de scalaire expressie gedurende welke de afwijkingsdetectie wordt uitgevoerd. Toegestane waarden voor deze parameter Float opnemen of dat een single (scalaire) retourwaarde van de gegevenstypen Bigint. De expressie met jokertekens **\*** is niet toegestaan. Scalaire expressie die bevatten geen andere analytische functies of externe functies. 

* **partition_by_clause** : de `PARTITION BY <partition key>` component verdeelt de learning en training van meerdere afzonderlijke partities. Met andere woorden, een afzonderlijk model moet worden gebruikt per waarde voor `<partition key>` en alleen gebeurtenissen met de waarde moeten worden gebruikt voor learning en opleiding in dit model. Bijvoorbeeld de volgende query treinen en scores per lezen tegen andere metingen van de dezelfde sensor alleen:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **de component limit_duration** `DURATION(<unit>, <length>)` -Hiermee geeft u het tijdsinterval (hoe ver terug in de geschiedenis van de huidige gebeurtenis) moet worden beschouwd bij het detecteren van afwijkingen. Zie [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) voor een gedetailleerde beschrijving van de ondersteunde maateenheden en hun afkortingen. 

* **when_clause** -Hiermee geeft u een Boole-voorwaarde voor de gebeurtenissen in de berekening van de detectie afwijkingsdetectie behandeld.

### <a name="return-types"></a>Retourtypen

De operator AnomalyDetection retourneert een record die alle drie scores als uitvoer ervan weergegeven. De eigenschappen die zijn gekoppeld aan de verschillende soorten afwijkingsdetectie detectoren zijn:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Als u wilt de afzonderlijke waarden uit de record extraheren, gebruikt u de **GetRecordPropertyValue** functie. Bijvoorbeeld:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Afwijkingsdetectie van een type wordt gedetecteerd wanneer een van de afwijkingsdetectie scores een drempelwaarde overschrijdt. De drempelwaarde is een drijvende-nummer > = 0. De drempelwaarde is een afweging tussen het gevoeligheid en vertrouwen. Bijvoorbeeld, zou een lagere drempelwaarde detectie gevoeliger voor wijzigingen aanbrengen en meer waarschuwingen genereren, terwijl een hogere drempelwaarde kan ervoor zorgen dat detectie minder gevoelig en meer vertrouwen, maar sommige afwijkingen maskeren. De exacte drempelwaarde moet worden gebruikt, is afhankelijk van het scenario. Er is geen bovengrens, maar de aanbevolen bereik 3,25 5 is. 

De waarde weergegeven in het voorbeeld 3,25 is slechts een voorgestelde beginpunt. De waarde aanpassen door het uitvoeren van de bewerkingen in uw eigen set gegevens en de uitvoerwaarde zien totdat u een toegestane drempel bereikt.

## <a name="anomaly-detection-algorithm"></a>Anomaliedetectie-algoritme

* AnomalyDetection operator gebruikt een **leren zonder supervisie** benadering waar deze wordt niet vanuit gegaan elk type distributiepunt in de gebeurtenissen. In het algemeen worden twee modellen bijgehouden in parallel op elk gewenst waarbij een van deze wordt gebruikt voor score berekenen en de andere op de achtergrond wordt getraind. De detectie afwijkingsdetectie modellen worden getraind met behulp van gegevens uit de huidige stroom in plaats van met behulp van een out-of-band-mechanisme. De hoeveelheid gegevens die worden gebruikt voor training, is afhankelijk van het venster grootte d opgegeven door de gebruiker binnen de Limit Duration-parameter. Elk model belandt ophalen op basis van d 2d aan gebeurtenissen getraind. Het verdient aanbeveling om ten minste 50 gebeurtenissen in elk venster voor de beste resultaten. 

* Maakt gebruik van de operator AnomalyDetection **venster semantiek Verschuivend** voor het trainen van modellen en score-gebeurtenissen. Dat betekent dat elke gebeurtenis wordt geëvalueerd voor de anomaliedetectie en een score wordt geretourneerd. De score is een indicatie van het niveau van betrouwbaarheid van die afwijkingsdetectie. 

* AnomalyDetection operator biedt een **herhaalbaarheid garantie** dezelfde invoer altijd begintijd van de dezelfde score ongeacht de taakuitvoer van de wordt gegenereerd. De begintijd van taak uitvoer geeft de tijd waarop de eerste uitvoergebeurtenis wordt geproduceerd door de taak. Deze waarde is ingesteld door de gebruiker op de huidige tijd, een aangepaste waarde of de laatste keer dat uitvoer (als de taak was eerder uitvoer geproduceerd). 

### <a name="training-the-models"></a>De modellen trainen 

Tijdens de voortgang van tijd modellen worden getraind met andere gegevens. Om het zinvol zijn van de scores, is het nuttig om het begrijpen van het onderliggende mechanisme waarmee de modellen worden getraind. Hier **t<sub>0</sub>**  is de **taakuitvoer begintijd** en **d** is de **venstergrootte** van de maximale duur parameter. Wordt ervan uitgegaan dat de tijd is onderverdeeld in **hops van de grootte van d**, te beginnen vanaf `01/01/0001 00:00:00`. De volgende stappen uit worden voor het trainen van het model en score de gebeurtenissen gebruikt:

* Wanneer een taak wordt gestart, het leest de gegevens die zijn gestart op het tijdstip t<sub>0</sub> – 2d.  
* Wanneer de tijd heeft de volgende hop bereikt, een nieuw model M1 wordt gemaakt en begint het ophalen van getraind.  
* Wanneer de tijd wordt bestuurd door een andere hop, een nieuw model M2 is gemaakt en gestart trainingen.  
* Wanneer de tijd heeft bereikt t<sub>0</sub>, M1 actief is gemaakt en de score begint het ophalen van output.  
* Op de volgende hop gebeuren drie dingen op hetzelfde moment:  

  * M1 niet meer nodig en wordt deze verwijderd.  
  * M2 is voldoende getraind zodat deze wordt gebruikt voor score berekenen.  
  * Een nieuw model M3 wordt gemaakt en wordt gestart op de achtergrond ophalen getraind.  

* Deze cyclus wordt herhaald voor elke hop wanneer het actieve model is verwijderd, overschakelen naar de parallelle model en start het trainen van een derde model op de achtergrond. 

Schematisch aangegeven, de stappen er als volgt uitzien: 

![Training modellen](media/stream-analytics-machine-learning-anomaly-detection/training_model.png)

|**Model** | **Training begintijd** | **Tijd om aan de slag met de score** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Model M1 training om 11:20 am, dit is de volgende hop nadat de taak is gestart lezen om 11:13 am wordt gestart. De eerste uitvoer wordt geproduceerd van M1 om 11:33 uur na de training met 13 minuten van gegevens. 

* Een nieuw model M2 gestart ook training om 11:30 am, maar de score komt niet ophalen gebruikt tot en met 11:40 am, namelijk nadat deze is getraind met 10 minuten van gegevens. 

* M3 volgt hetzelfde patroon als M2. 

### <a name="scoring-with-the-models"></a>Score berekenen met de modellen 

Op het niveau van Machine Learning berekent de anomaliedetectie-algoritme de waarde van een strangeness voor elke inkomende gebeurtenis door deze te vergelijken met gebeurtenissen in een geschiedenisvenster. De berekening strangeness verschilt voor elk type afwijkingsdetectie.  

Bekijken we de berekening strangeness in detail (wordt ervan uitgegaan dat er bestaat een set van historische windows met gebeurtenissen): 

1. **De wijzigingen in twee richtingen:** op basis van het geschiedenisvenster, een normale operationele bereik wordt berekend als [10 percentiel, 90 percentiel] dat wil zeggen, 10 percentielwaarde als de ondergrens en 90th percentielwaarde als de bovengrens. Een strangeness-waarde voor de huidige gebeurtenis is als volgt berekend:  

   - 0 als event_value in normale operationele bereik is  
   - event_value/90th_percentile als event_value > 90th_percentile  
   - 10th_percentile/event_value, als de event_value < 10th_percentile  

2. **Trage positieve trend:** een trendlijn via de waarden van de gebeurtenis in het geschiedenisvenster wordt berekend en de bewerking zoekt een positieve trend in de regel. De waarde strangeness is als volgt berekend:  

   - Helling, als helling positief  
   - 0 is, anders 

3. **Trage negatieve trend:** een trendlijn via de waarden van de gebeurtenis in het geschiedenisvenster wordt berekend en de bewerking zoekt negatieve trend in de regel. De waarde strangeness is als volgt berekend: 

   - Helling, als helling negatief is  
   - 0 is, anders  

Zodra de strangeness-waarde voor de binnenkomende gebeurtenis wordt berekend, een martingale-waarde wordt berekend op basis van de waarde strangeness (Zie de [Machine Learning-blog](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) voor meer informatie over hoe de martingale-waarde wordt berekend). Deze waarde martingale is als de score afwijkingsdetectie retuned. De waarde martingale hoger langzaam in reactie op vreemd waarden, dit kan de detectie robuuste sporadisch wijzigingen blijven en vermindert false waarschuwingen. Er wordt ook een handig eigenschap: 

Kans [bestaat er t dergelijke M<sub>t</sub> > λ] < 1/λ waar M<sub>t</sub> is de waarde martingale instant t en λ een werkelijke waarde. Bijvoorbeeld, als er een waarschuwing wanneer M<sub>t</sub>> 100 en vervolgens de waarschijnlijkheid van valse positieven is kleiner dan 1/100.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>Richtlijnen voor het gebruik van het niveau van bidirectionele detectie wijzigen 

De detectie van de wijzigingen in twee richtingen kan worden gebruikt in scenario's zoals power onderbreking en recovery, of snel uitgevoerd uur verkeer, enzovoort. Dit werkt echter zodanig dat wanneer een model wordt getraind met bepaalde gegevens, een ander niveau wijziging afwijkende is als de nieuwe waarde hoger dan de vorige bovengrens is (opwaarts niveau hoofdlettergebruik) of lager is dan de vorige ondergrens (neerwaartse niveau hoofdlettergebruik). Daarom ziet een model niet gegevenswaarden in de buurt van het nieuwe (omhoog of omlaag) in een venster training voor afwijkende worden beschouwd. 

De volgende punten overwegen wanneer u met deze detectie: 

1. Wanneer de tijdreeks plotseling ziet verhogen of verwijderen in waarde, de operator AnomalyDetection wordt gedetecteerd. Maar het rendement op normaal detecteren vereist meer planning. Als een tijdreeks in de actieve status voordat de afwijkingsdetectie die kan worden bereikt door de AnomalyDetection-operator detectie venster in te stellen op maximaal helft van de lengte van de afwijkingsdetectie was. Dit scenario wordt ervan uitgegaan dat de minimale duur van de afwijkingsdetectie kan worden geschat tevoren en er onvoldoende gebeurtenissen in die periode zijn voor het model trainen voldoende (ten minste 50 gebeurtenissen). 

   Dit wordt weergegeven in de afbeeldingen 1 en 2 hieronder met behulp van een wijziging in de bovengrens (dezelfde logica van toepassing op een lagere limiet wijziging). In beide cijfers zijn de golfvormen een afwijkende niveau wijzigen. Verticale oranje lijnen duiden hop grenzen en de hopgrootte is hetzelfde als het venster detectie is opgegeven in de operator AnomalyDetection. De groene regels geven aan de grootte van het venster training. In afbeelding 1 is de hopgrootte hetzelfde als de tijd voor welke afwijkingsdetectie duurt. In afbeelding 2 is de hopgrootte helft van de tijd op waarvoor de afwijkingsdetectie duurt. In alle gevallen is een wijziging in de opwaarts omdat het model dat wordt gebruikt voor score berekenen is getraind op normale gegevens gedetecteerd. Maar op basis van hoe de detectie van de wijzigingen in twee richtingen werkt, moet deze de normale waarden uitsluiten van de training venster dat wordt gebruikt voor het model dat het retourtype op normaal scores. In afbeelding 1 bevat de score model training sommige normale gebeurtenissen, zodat terug naar normaal kan niet worden gedetecteerd. Maar in afbeelding 2 bevat de training alleen de afwijkende onderdeel zorgt ervoor dat het retourtype op normaal wordt gedetecteerd. Inhoud die kleiner is dan de helft werkt ook voor dezelfde reden dat alles groter uiteindelijk met inbegrip van een bits van de normale gebeurtenissen. 

   ![AD met de lengte van venster Grootte gelijk afwijkingsdetectie](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_anomaly_length.png)

   ![AD met venstergrootte gelijk is aan de helft van de lengte van de afwijkingsdetectie](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_half_anomaly_length.png)

2. In gevallen waarbij de lengte van de afwijkingsdetectie kan niet worden voorspeld deze detectie wordt uitgevoerd met de beste inspanning. Echter ervoor kiest dat een fijner tijdvenster beperkt de trainingsgegevens, die zou vergroot de kans dat het retourtype op normaal detecteren. 

3. In het volgende scenario is niet de afwijkingsdetectie langer als het venster training al een afwijkingsdetectie van de dezelfde hoge waarde bevat gedetecteerd. 

   ![Afwijkingen met dezelfde grootte](media/stream-analytics-machine-learning-anomaly-detection/anomalies_with_same_length.png)

## <a name="example-query-to-detect-anomalies"></a>Van de voorbeeldquery voor het detecteren van afwijkingen 

De volgende query kan worden gebruikt voor uitvoer van een waarschuwing als er een afwijkingsdetectie wordt gedetecteerd.
Wanneer de invoerstroom niet uniform, moet u de stap aggregatie kunt transformeren in een uniform tijdreeks. In het voorbeeld gebruikt AVG, maar het specifieke type van de aggregatie is afhankelijk van het gebruikersscenario. Bovendien wanneer een tijdreeks groter dan het venster aggregatie hiaten is, er zijn niet alle gebeurtenissen in de tijdreeks voor afwijkingsdetectie trigger (volgens de semantiek voor venster Verschuivend). Als gevolg hiervan is de veronderstelling van homogeniteit verbroken bij het ontvangen van de volgende gebeurtenis. In dergelijke gevallen moeten de hiaten in de tijdreeks worden ingevuld. Een mogelijke aanpak is het nemen van de laatste gebeurtenis in alle vensters hop, zoals hieronder wordt weergegeven.

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

## <a name="performance-guidance"></a>Richtlijnen voor prestaties

* Gebruik zes streaming-eenheden voor taken. 
* Verzenden van gebeurtenissen ten minste één seconde uit elkaar liggen.
* Een niet-gepartitioneerde-query is met de operator AnomalyDetection resultaten kan worden verkregen met een berekening latentie van ongeveer 25 ms gemiddeld.
* De latentie is opgetreden door een gepartitioneerde query verschilt enigszins met het aantal partities, zoals het aantal berekeningen hoger is. De latentie is echter hetzelfde als de aanvraag niet-gepartitioneerde voor een vergelijkbare totaal aantal gebeurtenissen voor alle partities.
* Tijdens het lezen van gegevens real time, een grote hoeveelheid gegevens snel ingenomen. Verwerken van deze gegevens is momenteel langzamer. De latentie bij scenario's is te verhogen Lineair met het aantal gegevenspunten in het venster in plaats van het venster grootte of gebeurtenis-interval gevonden. Als u de latentie in real time gevallen, kunt u overwegen een kleinere venster. Houd ook rekening met uw taak starten vanuit de huidige tijd. Enkele voorbeelden van latenties in een niet-gepartitioneerde query: 
    - 60 gegevenspunten in het venster detectie kunnen resulteren in een latentie van 10 seconden met een doorvoersnelheid van 3 MBps. 
    - De latentie kan ongeveer 80 seconden met een doorvoersnelheid van 0,4 MBps bereiken op 600 gegevenspunten.

## <a name="limitations-of-the-anomalydetection-operator"></a>Beperkingen van de operator AnomalyDetection 

* Deze operator ondersteunt momenteel geen piek- en dip detectie. Pieken en dips zijn eigen initiatief of tijdelijke wijzigingen in de tijdreeks. 

* Deze operator verwerkt momenteel niet periodieke patronen. Periodieke patronen zijn herhaalde patronen in de gegevens, bijvoorbeeld een ander web verkeer-gedrag tijdens weekeinden of andere webwinkel trends tijdens zwart vrijdag, dat niet afwijkingen is vereist, maar een verwachte-patroon in gedrag. 

* Deze operator invoer tijdreeks uniform worden verwacht. Een stroom gebeurtenissen kan uniform worden gemaakt door via een daling aggregeren of hopping venster. In scenario's waarin het hiaat tussen gebeurtenissen altijd kleiner zijn dan het venster aggregatie, is een tumblingvenster voldoende zijn voor het maken van de tijdreeks uniform. Wanneer de ruimte kan niet groter zijn, kunnen de hiaten door te herhalen van de laatste waarde met behulp van een hoppingvenster worden ingevuld. 

## <a name="references"></a>Verwijzingen

* [Afwijkingsdetectie – gebruiken machine learning-afwijkingen in de reeks tijdgegevens detecteren](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Machine learning-API voor afwijkingsdetectie](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Time series-afwijkingsdetectie](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

