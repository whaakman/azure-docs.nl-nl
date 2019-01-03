---
title: Schalen en uitbreiden in Azure Stream Analytics-taken
description: In dit artikel wordt beschreven hoe u een Stream Analytics-taak door het partitioneren van invoergegevens, het afstemmen van de query en het instellen van de taak streaming-eenheden schalen.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/22/2017
ms.openlocfilehash: f4307da2e74846507cafb9f767a6ccae855e42a2
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554670"
---
# <a name="scale-an-azure-stream-analytics-job-to-increase-throughput"></a>Een Azure Stream Analytics-taak voor een betere doorvoer schalen
Dit artikel ziet u hoe u een Stream Analytics-query voor een betere doorvoer voor Streaming Analytics-taken af te stemmen. U kunt de volgende procedure gebruiken voor het schalen van uw taak voor het verwerken van de hogere load en profiteer van meer systeembronnen (zoals meer bandbreedte, meer CPU-resources, meer geheugen).
Als een vereiste moet u mogelijk om de volgende artikelen te lezen:
-   [Streaming-eenheden begrijpen en aanpassen](stream-analytics-streaming-unit-consumption.md)
-   [Worden opgestart taken maken](stream-analytics-parallelization.md)

## <a name="case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions"></a>Geval 1: de query is inherent volledig worden opgestart voor de invoer partities
Als uw query inherent volledig worden opgestart voor invoer partities is, kunt u de volgende stappen volgen:
1.  Uw query te perfect parallel met behulp van maken **PARTITION BY** trefwoord. Zie voor meer informatie de sectie perfect parallelle taken [op deze pagina](stream-analytics-parallelization.md).
2.  Afhankelijk van de uitvoertypen die in de query wordt gebruikt, enkele uitvoer een mogelijk niet worden opgestart, of meer configuratie perfect parallelle nodig hebt. SQL-, SQL DW- en Power BI-uitvoer zijn bijvoorbeeld niet worden opgestart. Uitvoer worden altijd samengevoegd voordat ze worden verzonden naar het uitvoereindpunt worden verplaatst. BLOBs, tabellen, ADLS, Service Bus en Azure-functie worden automatisch laat parallelliseren. Cosmos DB en Event Hub moet de PartitionKey configuratieset moet overeenkomen met de **PARTITION BY** (meestal PartitionId) veld. Voor Event Hub, betaalt u ook extra aandacht aan het aantal partities voor alle invoer en alle uitvoer om te voorkomen dat cross-over tussen partities. 
3.  Uitvoeren van uw query's uitvoeren met **6 SU** (dit is de volledige capaciteit van een enkel knooppunt computing) voor het meten van maximale haalbare doorvoer, en als u **GROUP BY**, meet het aantal groepen (kardinaliteit) van de taak kunnen de ingang. Algemene problemen van de taak te maken met system resourcelimieten zijn als volgt.
    - SU % gebruik metrische gegevens is meer dan 80%. Hiermee wordt aangegeven geheugengebruik hoog is. De factoren die bijdragen aan de toename van deze metrische gegevens worden beschreven [hier](stream-analytics-streaming-unit-consumption.md). 
    -   Tijdstempel van de uitvoer valt achter met betrekking tot de kloktijd. Afhankelijk van uw logische query mogelijk de tijdstempel van de uitvoer een verschuiving van de logica van de kloktijd. Ze moeten echter de voortgang ongeveer hetzelfde tarief. Als de tijdstempel van de uitvoer valt verdere en verdere achter, is een indicator die het systeem is overworking. Het kan een gevolg zijn van de downstream-uitvoer-sink aanvragen worden beperkt of hoge CPU-gebruik zijn. Bieden we geen CPU-gebruik metrische gegevens op dit moment, zodat het kan lastig zijn om te onderscheiden van de twee.
        - Als het probleem vanwege een beperking van sink is, moet u mogelijk verhogen van het aantal partities van de uitvoer (en ook invoer partities om te voorkomen dat de taak die volledig worden opgestart), of vergroot de hoeveelheid resources van de sink (bijvoorbeeld aantal Aanvraageenheden voor cosmos DB).
    - Taakdiagram, er is een per partitie achterstand gebeurtenis metrische gegevens voor elke invoer. Als de achterstand gebeurtenis metrische gegevens stijgen blijft, maar het is ook een indicatie dat de bron van het systeem is beperkt (hetzij vanwege een beperking van de uitvoer-sink of hoge CPU).
4.  Nadat u hebt vastgesteld dat de grenzen van wat een 6 SU-taak kan bereiken, kunt u afleiden lineair de verwerkingscapaciteit van de taak als u meer su's toevoegen, ervan uitgaande dat u geen gegevens scheeftrekken waarmee bepaalde partitie 'hot'.

> [!NOTE]
> Kies het juiste aantal Streaming-eenheden: Omdat de Stream Analytics maakt een verwerkingsknooppunt voor elke 6 SU toegevoegd, is het aanbevolen om het aantal knooppunten een deler van het aantal invoer-partities, zodat de partities kunnen evenredig worden verdeeld over de knooppunten.
> Bijvoorbeeld, hebt u uw 6 gemeten SU taak kan maar liefst 4 MB/s verwerken van snelheid en het aantal invoerpartitie is 4. U kunt uw taak uitvoeren met 12 SU naar ongeveer 8 MB/s-verwerkingssnelheid bereiken of 24 SU naar het bereiken van 16 MB/s. Vervolgens kunt u bepalen wanneer u SU getal voor de taak op welke waarde als een functie van de snelheid van de invoer verhogen.


## <a name="case-2---if-your-query-is-not-embarrassingly-parallel"></a>Geval 2: als uw query niet perfect parallel is.
Als uw query niet perfect parallel is, kunt u de volgende stappen volgen.
1.  Begin met een query zonder **PARTITION BY** om te voorkomen dat het partitioneren van complexiteit en de query uitvoert met 6 SU voor het meten van maximale belasting als in [voorbeeld1](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions).
2.  Als u de verwachte belasting op termijn van doorvoer bereiken kunt, bent u klaar. U kunt ook kiezen voor het meten van dezelfde taak uitgevoerd op 3 SU en 1 SU om erachter te komen het minimum aantal SU die geschikt is voor uw scenario.
3.  Als u de gewenste doorvoer niet kan bereiken, kunt u proberen om uw query in meerdere stappen indien mogelijk, als deze niet al meerdere stappen en maximaal 6 SU voor elke stap in de query toewijzen. Bijvoorbeeld als u in 3 stappen, 18 SU in de optie 'Schaal' toewijzen.
4.  Wanneer een dergelijke taak wordt uitgevoerd, wordt elke stap in Stream Analytics geplaatst op een eigen knooppunt met speciale 6 SU-resources. 
5.  Als u uw doel belasting nog niet hebt bereikt, kunt u proberen te gebruiken **PARTITION BY** vanaf stap dichter naar de invoer. Voor **GROUP BY** operator die mogelijk niet op een natuurlijke manier partitioneerbare, kunt u het lokale/globale cumulatieve patroon om uit te voeren een gepartitioneerde **GROUP BY** gevolgd door een niet-gepartitioneerde **GROUP BY** . Bijvoorbeeld, als u wilt tellen kan hoeveel auto's elke stand nummer te doorlopen om de 3 minuten, en het volume van de gegevens is groter dan wat worden verwerkt door 6 SU.

Query:

 ```SQL
 WITH Step1 AS (
 SELECT COUNT(*) AS Count, TollBoothId, PartitionId
 FROM Input1 Partition By PartitionId
 GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
 )
 SELECT SUM(Count) AS Count, TollBoothId
 FROM Step1
 GROUP BY TumblingWindow(minute, 3), TollBoothId
 ```
U bent in de bovenstaande query auto's per nummer tolloket per partitie tellen en vervolgens toe te voegen de telling van alle partities samen.

Nadat is gepartitioneerd, voor elke partitie van de stap toewijzen maximaal 6 SU, elke partitie met 6 SU is het maximum, zodat elke partitie kan worden geplaatst op een eigen verwerkingsknooppunt.

> [!Note]
> Als uw query kan niet worden gepartitioneerd, extra SU toe te voegen in een query met meerdere stappen mogelijk niet altijd de doorvoer verbeteren. Er is één manier om toegang te krijgen van de prestaties te verminderen van het volume op de eerste stappen met behulp van lokale/globale cumulatieve patroon, zoals hierboven is beschreven in stap 5.

## <a name="case-3---you-are-running-lots-of-independent-queries-in-a-job"></a>Casus 3: veel onafhankelijke query's in een taak wordt uitgevoerd.
Voor bepaalde ISV use-cases, waarbij is het meer betaalbare om gegevens van meerdere tenants in één taak te verwerken, met behulp van afzonderlijke invoer en uitvoer voor elke tenant, zou u uiteindelijk een behoorlijke enkele (bijvoorbeeld 20) uitgevoerd onafhankelijke query's in één taak. De veronderstelling is dat elke dergelijke subquery laden is relatief klein. In dit geval kunt u de volgende stappen volgen.
1.  In dit geval, gebruik geen **PARTITION BY** in de query
2.  Het aantal invoer partities beperken tot de laagst mogelijke waarde van 2, als u van Event Hub gebruikmaakt.
3.  Voer de query uit met 6 SU. Met de verwachte belasting voor elke subquery toevoegen u zoveel dergelijke subquery's mogelijk, totdat de taak wordt de system resource limiet hebt bereikt. Raadpleeg [voorbeeld1](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions) voor de symptomen als dit gebeurt.
4.  Nadat u de limiet van de subquery gemeten hierboven hebt bereikt, start u de subquery toe te voegen aan een nieuwe taak. Het aantal taken uit te voeren als een functie van het aantal onafhankelijke query's moet vrij lineaire, ervan uitgaande dat u hebt een elke belasting door scheeftrekken. U kunt vervolgens een prognose hoeveel 6 SU taken moet u uitvoeren als een functie van het aantal tenants dat u wilt leveren.
5.  Wanneer u gegevens lid worden van verwijzing met dergelijke query's, union de samen voordat hij bij met dezelfde invoer verwijzen naar gegevens. Opgesplitst vervolgens de gebeurtenissen indien nodig. Anders houdt elke verwijzing gegevens join een kopie van referentiegegevens in het geheugen, die waarschijnlijk van het geheugengebruik onnodig.

> [!Note] 
> Hoeveel tenants in elke taak te plaatsen?
> Dit patroon query vaak een groot aantal subquery's en resulteert in zeer grote en complexe topologie. De controller van de taak niet mogelijk om af te handelen van een grote topologie. Als vuistregel, blijven onder 40 tenants voor 1 SU-taak en 60 tenants voor SU 3 en 6 SU-taken. Wanneer u de capaciteit van de controller overschrijden, wordt de taak niet starten.



## <a name="get-help"></a>Help opvragen
Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

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

[microsoft.support]: https://support.microsoft.com
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

