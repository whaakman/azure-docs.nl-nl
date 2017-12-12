---
title: Stream Analytics-taken te verhogen doorvoer schalen | Microsoft Docs
description: Informatie over het configureren van invoer partities, definitie van de query te verfijnen en het instellen van taak streaming-eenheden schalen Stream Analytics-taken.
keywords: gegevens streaming afstemmen analytics streaming gegevensverwerking,
services: stream-analytics
documentationcenter: 
author: JSeb225
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeanb
ms.openlocfilehash: 781a3b71c35cb48e40202e3b1acc8edbbaf865c4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="scale-azure-stream-analytics-jobs-to-increase--throughput"></a>Azure Stream Analytics-taken voor een betere doorvoer schalen
In dit artikel leest u hoe af te stemmen een Stream Analytics query voor een betere doorvoer voor Streaming Analytics-taken. U kunt de volgende handleiding voor het schalen van uw taak voor het verwerken van de hogere belasting en te profiteren van meer systeembronnen (zoals meer bandbreedte, meer CPU-bronnen, meer geheugen).
Als een vereiste moet u mogelijk de volgende artikelen te lezen:
-   [Streaming-eenheden begrijpen en aanpassen](stream-analytics-streaming-unit-consumption.md)
-   [Worden taken maken](stream-analytics-parallelization.md)


## <a name="case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions"></a>Voorbeeld 1: uw query is inherent volledig worden meerdere partities invoer
Als uw query inherent volledig worden meerdere invoer partities is, kunt u de volgende stappen uit:
1.  Schrijven van uw query perfect parallelle worden met behulp van **PARTITION BY** sleutelwoord. Meer informatie in de sectie perfect parallelle taken [op deze pagina](stream-analytics-parallelization.md).
2.  Afhankelijk van de uitvoer die worden gebruikt in uw query, sommige uitvoer kan ofwel niet parallel lopend, of meer configuratie moet perfect parallelle nodig. SQL, SQL DW en Power BI-uitvoer zijn bijvoorbeeld niet worden. Uitvoer worden altijd vóór het verzenden naar de uitvoerlocatie samengevoegd. BLOBs, tabellen, ADLS, Service Bus en Azure-functie worden automatisch geparallelliseerde. CosmosDB en Event Hub moet beschikken over de configuratie van de PartitionKey ingesteld op overeen met de **PARTITION BY** veld (meestal PartitionId). Voor Event Hub, ook moet extra aandacht schenken aan overeen met het aantal partities voor alle in- en alle uitgangen om te voorkomen dat cross-over tussen de partities. 
3.  Voer uw query met **6 SU** (dit is de volledige capaciteit van een enkel knooppunt computing) voor het meten van maximaal haalbare doorvoer, en als u **GROUP BY**, meten hoeveel groepen (kardinaliteit) de taak kunnen verwerken. Algemene symptomen van de taak kunt u limieten voor systeem door zijn het volgende.
    - SU % gebruik meetwaarde is meer dan 80%. Hiermee wordt aangegeven geheugen gebruik is hoog. De factoren die bijdragen aan de toename van deze metrische gegevens worden beschreven [hier](stream-analytics-streaming-unit-consumption.md). 
    -   Uitvoer tijdstempel valt achter met betrekking tot kloktijd. Afhankelijk van de logische query wellicht het tijdstempel voor de uitvoer een offset van de logica van de kloktijd. Ze moeten echter de voortgang ongeveer dezelfde snelheid. Als de uitvoer-tijdstempel is dalen verdere en verdere achter, is een indicator die het systeem is overworking. Het kan zijn veroorzaakt door downstream uitvoer sink bandbreedteregeling of hoog CPU-gebruik. We bieden niet CPU-gebruik metrische gegevens op dit moment, zodat het kan lastig zijn om te onderscheiden van de twee.
        - Als het probleem veroorzaakt door sink beperking wordt, moet u mogelijk Verhoog het aantal partities uitvoer (en ook invoer partities zodat de taak volledig worden) of verhoog de hoeveelheid resources van de sink (bijvoorbeeld aantal Aanvraageenheden voor CosmosDB).
    - In het diagram van de taak, er is een per partitie achterstand gebeurtenis metrische gegevens voor elke invoer. Als de achterstand gebeurtenis metriek stijgt houdt, maar het is ook een indicatie dat de systeembron wordt begrensd (zowel vanwege uitvoer sink-beperking of intensief CPU).
4.  Als u hebt vastgesteld dat de grenzen van wat een 6 SU-taak kan bereiken, kunt u afleiden lineair de verwerkingscapaciteit van de taak toevoegen van meer SUs, ervan uitgaande dat u geen leiden tot onjuiste gegevens waarmee bepaalde partitie 'hot'.
>[!Note]
> Kies het juiste aantal Streaming-eenheden: omdat de Stream Analytics maakt een verwerkingsknooppunt voor elke 6 SU toegevoegd, is het beste het aantal knooppunten een deler van het aantal invoer partities, zodat de partities kunnen gelijkmatig worden verdeeld over de knooppunten.
> U hebt bijvoorbeeld uw 6 gemeten SU taak kunt bereiken 4 MB/s verwerken frequentie en het aantal invoer partitie is 4. U kunt kiezen uit te voeren van uw werk met 12 SU als u de verwerkingssnelheid van ongeveer 8 MB/s of 24 SU naar het bereiken van 16 MB/s. Vervolgens kunt u bepalen wanneer SU getal voor de taak op welke waarde als een functie van de snelheid van de invoer verhogen.



## <a name="case-2---if-your-query-is-not-embarrassingly-parallel"></a>Voorbeeld 2 - als de query niet perfect parallelle.
Als de query niet perfect parallelle, kunt u de volgende stappen te volgen.
1.  Beginnen met een query zonder **PARTITION BY** eerst om te voorkomen partitioneren complexiteit en uw query uitvoeren met 6 SU voor het meten van de maximale belasting als in [geval 1](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions).
2.  Als u de verwachte belasting termijn van doorvoer bereiken kunt, bent u klaar. U kunt ook dezelfde taak uitgevoerd op 3 SU en 1 SU om te weten het minimum aantal SU die geschikt is voor uw scenario te meten.
3.  Als u de gewenste doorvoer niet kan bereiken, kunt u uw query opdelen in meerdere stappen indien mogelijk, als deze niet al hebben van meerdere stappen en maximaal 6 SU voor elke stap in de query toewijzen. Bijvoorbeeld als er 3 stappen 18 SU in de optie 'Schalen' toewijzen.
4.  Wanneer deze taak wordt uitgevoerd, worden elke stap in Stream Analytics geplaatst op een eigen knooppunt met speciale 6 SU-resources. 
5.  Als u uw doel load nog niet bereikt, kunt u proberen om te gebruiken **PARTITION BY** vanaf stap dichter naar de invoer. Voor **GROUP BY** operator die niet natuurlijk partitioneerbare zijn, kunt u het lokale/globale cumulatieve patroon om uit te voeren een gepartitioneerde **GROUP BY** gevolgd door een niet-gepartitioneerde **GROEPEREN op** . Bijvoorbeeld, als u wilt tellen kan hoeveel auto's gaan via elke tolstation stand om de 3 minuten en het volume van de gegevens is groter dan wat worden verwerkt door 6 SU.

Query:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

U bent in de bovenstaande query tellen auto's per tolstation stand per partitie en vervolgens toe te voegen de telling van alle partities samen.

Zodra het is gepartitioneerd, voor elke partitie van de stap toewijzen maximaal 6 SU, elke partitie met 6 SU is het maximum, zodat elke partitie op een eigen verwerkingsknooppunt kan worden geplaatst.

> [!Note]
> Als uw query kan niet worden gepartitioneerd, extra SU toe te voegen in een query met meerdere stappen mogelijk niet altijd verbeterd doorvoer. Er is een manier om toegang te krijgen van de prestaties te verminderen van volume op de eerste stappen met lokale/globale cumulatieve patroon, zoals hierboven is beschreven in stap 5.

## <a name="case-3---you-are-running-lots-of-independent-queries-in-a-job"></a>Voorbeeld 3 - hoeveelheden onafhankelijke query's in een taak wordt uitgevoerd.
Voor bepaalde ISV gebruiksvoorbeelden, waarbij is het kostenefficiënte om gegevens van meerdere tenants in een enkele taak te verwerken, met afzonderlijke invoer en uitvoer voor elke tenant, zou u uiteindelijk zeer enkele (bijvoorbeeld 20) uitgevoerd onafhankelijke query's in een enkele taak. De veronderstelling is dat elke dergelijke subquery belasting relatief klein is. In dit geval kunt u de volgende stappen volgen.
1.  In dit geval gebruik geen **PARTITION BY** in de query
2.  Verminder het aantal invoer partities aan de laagst mogelijke waarde van 2 als u van Event Hub gebruikmaakt.
3.  Voer de query met 6 SU. Toevoegen met de verwachte belasting voor elke subquery zoveel dergelijke subquery's mogelijk, totdat de taak is roept limieten voor systeem. Raadpleeg [geval 1](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions) voor de symptomen als dit gebeurt.
4.  Wanneer u de subquery limiet gemeten hierboven zijn roept, beginnen met de subquery toe te voegen aan een nieuwe taak. Het aantal taken uit te voeren als een functie van het aantal onafhankelijke query's moet redelijk lineaire, ervan uitgaande dat u hebt geen belasting leiden tot onjuiste. U kunt vervolgens een prognose hoeveel 6 SU taken u moet uitvoeren als een functie van het aantal tenants dat u wilt leveren.
5.  Als u de verwijzing naar gegevens join met dergelijke query's, moet u union die invoerwaarden samen voordat het lid met dezelfde verwijzingsgegevens, klikt u vervolgens splitsen de gebeurtenissen indien nodig. Elke verwijzing gegevens join, anders wordt een kopie van referentiegegevens blijft in geheugen, die waarschijnlijk van het geheugengebruik onnodig.

> [!Note] 
> Hoeveel tenants in elke taak te plaatsen?
> Dit patroon query vaak een groot aantal subquery's en resulteert in zeer grote en complexe topologie. De domeincontroller van de taak is mogelijk niet kunnen verwerken van een grote topologie. Als een vuistregel blijven onder 40 tenants voor SU-taak 1 en 60 tenants voor SU 3 en 6 SU-taken. Wanneer u de capaciteit van de controller overschrijdt, wordt de taak niet starten.


## <a name="an-example-of-stream-analytics-throughput-at-scale"></a>Een voorbeeld van een Stream Analytics-doorvoer op grote schaal
Om te begrijpen hoe de Stream Analytics-taken schalen, hebben we een experiment op basis van de invoer van een apparaat frambozen Pi uitgevoerd. Dit experiment laat ons zien van het effect op de doorvoer van meerdere streaming-eenheden en partities.

In dit scenario wordt verzendt het apparaat sensorgegevens (clients) naar een event hub. Streaming Analytics verwerkt de gegevens en een waarschuwing of -statistieken als uitvoer naar een andere event hub verzendt. 

De client stuurt sensorgegevens in JSON-indeling. De gegevensuitvoer van de is ook in JSON-indeling. De gegevens ziet er als volgt:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

De volgende query wordt gebruikt voor het verzenden van een waarschuwing wanneer een licht wordt uitgeschakeld:

    SELECT AVG(lght), "LightOff" as AlertText
    FROM input TIMESTAMP BY devicetime 
    PARTITION BY PartitionID
    WHERE lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Meting doorvoer

In deze context is doorvoer de hoeveelheid invoergegevens verwerkt door de Stream Analytics in een vaste hoeveelheid tijd. (We gemeten voor 10 minuten.) Als u wilt de beste verwerking doorvoer voor de invoergegevens bereiken, zijn zowel de gegevensstroominvoer als de query gepartitioneerd. We opgenomen **COUNT()** in de query om te meten hoeveel invoervelden zijn verwerkt. Elke partitie van de invoer event hub is om te zorgen dat de taak is gewoon wacht niet invoervelden te komen, vooraf geladen met ongeveer 300 MB met invoergegevens.

De volgende tabel toont de resultaten die we zagen we het aantal streaming-eenheden verhoogd als de overeenkomstige partitie telt in event hubs.  

<table border="1">
<tr><th>Invoer partities</th><th>Uitvoer partities</th><th>Streamingeenheden</th><th>Volgehouden doorvoer
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

En de volgende grafiek ziet een visualisatie van de relatie tussen SUs en doorvoer.

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

