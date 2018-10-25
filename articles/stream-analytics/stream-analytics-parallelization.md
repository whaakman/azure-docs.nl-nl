---
title: Query-parallellisatie en schalen in Azure Stream Analytics gebruiken
description: In dit artikel wordt beschreven hoe u Stream Analytics-taken schalen door het configureren van invoer partities, afstemmen van de definitie van de query en het instellen van de taak streaming-eenheden.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 83fbebc07be3a61d7fd54953f842a320a537a7ac
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985009"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Maak gebruik van query-parallellisatie in Azure Stream Analytics
Dit artikel ziet u hoe u kunt profiteren van parallelle uitvoering in Azure Stream Analytics. Leert u hoe u Stream Analytics-taken schalen door invoer partities configureren en afstemmen van de definitie van de analytics-query.
Als een vereiste, kunt u om vertrouwd te raken met het concept van Streaming-eenheid die worden beschreven in [begrijpen en aanpassen van Streaming-eenheden](stream-analytics-streaming-unit-consumption.md).

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Wat zijn de onderdelen van een Stream Analytics-taak?
De definitie van een Stream Analytics-taak bevat invoer, een query- en uitvoer. Invoer is waar de taak leest de gegevensstroom uit. De query wordt gebruikt om te zetten van de invoerstroom van gegevens en de uitvoer is waar de taak de resultaten van de taak te verzenden.  

Een taak moet ten minste één invoerbron voor het streamen van gegevens. De stream-invoerbron gegevens kan worden opgeslagen in een Azure event hub of Azure blob-opslag. Zie voor meer informatie, [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md) en [aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-sources-and-sinks"></a>Partities in de bronnen en sinks
Schalen van een Stream Analytics-taak maakt gebruik van partities in de invoer of uitvoer. Hiermee kunt die u gegevens in subsets op basis van een partitiesleutel onderverdelen partitioneren. Een proces waarbij de gegevens (zoals een stream Analytics-taak) kunt gebruiken en het schrijven van verschillende partities parallel verhoogt de doorvoer. 

### <a name="inputs"></a>Invoer
Alle Azure Stream Analytics-invoer kan profiteren van het partitioneren van:
-   Event hub (u moet de partitiesleutel expliciet met PARTITION BY trefwoord instellen)
-   IoT Hub (u moet de partitiesleutel expliciet met PARTITION BY trefwoord instellen)
-   Blob Storage

### <a name="outputs"></a>Uitvoer

Wanneer u met Stream Analytics werkt, kunt u profiteren van partitionering in de uitvoer:
-   Azure Data Lake Storage
-   Azure Functions
-   Azure Table
-   BLOB-opslag (kan de partitiesleutel expliciet ingesteld)
-   Cosmos DB (u moet expliciet de partitiesleutel instellen)
-   Event hub (u moet expliciet de partitiesleutel instellen)
-   IoT Hub (u moet expliciet de partitiesleutel instellen)
-   Service Bus

Power BI, SQL en SQL Data Warehouse uitvoer bieden geen ondersteuning voor partitioneren. Maar u kunt nog steeds partitioneren de invoer zoals beschreven in [in deze sectie](#multi-step-query-with-different-partition-by-values) 

Zie voor meer informatie over partities, de volgende artikelen:

* [Overzicht van functies van Event Hubs](../event-hubs/event-hubs-features.md#partitions)
* [Gegevenspartitionering](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="embarrassingly-parallel-jobs"></a>Perfect parallelle taken
Een *perfect parallelle* taak is het meest schaalbare scenario we in Azure Stream Analytics hebben. Deze verbinding één partitie van de invoer naar één exemplaar van de query maakt met één partitie van de uitvoer. Deze parallelle uitvoering heeft de volgende vereisten:

1. Als uw querylogica is afhankelijk van dezelfde sleutel door hetzelfde exemplaar van de query wordt verwerkt, moet u ervoor zorgen dat de gebeurtenissen gaat u naar dezelfde partitie van uw invoer. Voor Event Hubs of IoT-Hub, dit betekent dat de gebeurtenisgegevens moet hebben de **PartitionKey** waarde ingesteld. U kunt ook gepartitioneerde afzenders gebruiken. Voor blob storage betekent dit dat de gebeurtenissen worden verzonden naar de map met dezelfde partitie. Als uw querylogica dezelfde sleutel kan worden verwerkt door de dezelfde query-instantie niet vereist, kunt u deze vereiste negeren. Een voorbeeld van deze logica is een eenvoudige select-project-filter-query.  

2. Zodra de gegevens wordt weergegeven op de invoer-zijde, moet u ervoor zorgen dat uw query is gepartitioneerd. Hiervoor moet u gebruikmaken van **PARTITION BY** in alle de stappen. Meerdere stappen zijn toegestaan, maar ze allemaal moeten worden gepartitioneerd op basis van dezelfde sleutel. Op dit moment de te nemen partitionerende sleutel moet worden ingesteld op **PartitionId** in volgorde voor de taak moet volledig parallelle.  

3. De meeste van onze uitvoer kunt profiteren van het partitioneren, maar als u een uitvoertype die biedt geen ondersteuning voor partitioneren uw taak geen volledig parallelle. Raadpleeg de [sectie uitvoer](#outputs) voor meer informatie.

4. Het aantal invoer partities moet gelijk zijn aan het aantal partities van de uitvoer. BLOB storage-uitvoer partities kan ondersteunen en neemt het partitieschema van de upstream-query. Wanneer een partitiesleutel voor Blob storage is opgegeven, gegevens zijn gepartitioneerd per invoerpartitie waardoor is het resultaat nog steeds volledig parallelle. Hier volgen enkele voorbeelden van waarden van partitie die een volledig parallelle taak toestaan:

   * 8 event hub-invoer partities en 8 event hub-partities uitvoer
   * 8 event hub-invoer partities en blob storage-uitvoer
   * 8 invoer event hub-partities en blob storage-uitvoer gepartitioneerd op basis van een aangepast veld met een willekeurige kardinaliteit
   * 8 blob storage-invoer partities en blob storage-uitvoer
   * 8 blob storage invoer partities en 8 event hub-uitvoer-partities

De volgende secties worden enkele voorbeeldscenario's die perfect parallelle zijn besproken.

### <a name="simple-query"></a>Eenvoudige query

* Invoer: Event hub met 8 partities
* De uitvoer: Event hub met 8 partities

Query:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Deze query is een eenvoudige filter. Daarom moet we geen zorgen over het partitioneren van de invoer die worden verzonden naar de event hub. U ziet dat de query bevat **partitie door PartitionId**, zodat deze voldoen aan de vereiste #2 van eerder. Voor de uitvoer moet voor het configureren van de event hub-uitvoer in de taak is het belangrijkste partitie worden ingesteld op **PartitionId**. Er is één sinds laatste controle om ervoor te zorgen dat het aantal invoer partities gelijk aan het aantal partities van de uitvoer is.

### <a name="query-with-a-grouping-key"></a>Query's uitvoeren met een groeperingssleutel

* Invoer: Event hub met 8 partities
* De uitvoer: Blob storage

Query:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Deze query heeft een groeperingssleutel. Daarom moeten de gebeurtenissen gegroepeerd worden verzonden naar dezelfde Event Hub-partitie. Omdat in dit voorbeeld we op TollBoothID groeperen, moeten we ervoor dat TollBoothID als de partitiesleutel wordt gebruikt als de gebeurtenissen naar Event Hub worden verzonden worden. Klik in de ASA, we gebruiken **partitie door PartitionId** overnemen van dit partitieschema en volledige parallellisering inschakelen. Omdat de uitvoer is een blob-opslag, moet we geen zorgen over het configureren van een waarde voor de partitiesleutel, aan de hand van vereiste #4.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Voorbeeld van scenario's die zijn *niet* perfect parallelle

In de vorige sectie hebt gezien hoe sommige perfect parallelle scenario's. In deze sectie wordt besproken hoe scenario's die niet voldoen aan alle vereisten voor het perfect parallelle worden. 

### <a name="mismatched-partition-count"></a>Aantal niet-overeenkomende partities
* Invoer: Event hub met 8 partities
* De uitvoer: Event hub met 32 partities

In dit geval het maakt niet uit wat de query is. Als het aantal partities invoer komt niet overeen met het aantal uitvoer, de topologie is niet perfect parallel. + echter we nog steeds bepaalde niveau of parallellisering krijgen.

### <a name="query-using-non-partitioned-output"></a>Query's uitvoeren met behulp van niet-gepartitioneerde uitvoer
* Invoer: Event hub met 8 partities
* Uitvoer: Power BI

Uitvoer van de Power BI ondersteunt momenteel geen partitioneren. In dit scenario is daarom niet perfect parallelle.

### <a name="multi-step-query-with-different-partition-by-values"></a>WebTest met meerdere stappen query's uitvoeren met verschillende waarden PARTITION BY
* Invoer: Event hub met 8 partities
* De uitvoer: Event hub met 8 partities

Query:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Zoals u zien kunt, gebruik van de tweede stap **TollBoothId** als de te nemen partitionerende sleutel. Deze stap is niet hetzelfde als de eerste stap en daarom moeten we doen een willekeurige volgorde. 

In de voorgaande voorbeelden sommige Stream Analytics-taken die voldoen aan (of niet) op een perfect parallelle topologie. Als die in overeenstemming zijn, hebben ze de mogelijkheden voor maximale schaal. Voor taken die niet zijn voor een van deze profielen geschikt, schalen richtlijnen beschikbaar in toekomstige zijn updates. Gebruik nu de algemene richtlijnen in de volgende secties.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>De maximale streaming-eenheden van een taak berekenen
Het totale aantal streamingeenheden dat kan worden gebruikt door een Stream Analytics-taak is afhankelijk van het aantal stappen in de query die is gedefinieerd voor de taak en het aantal partities voor elke stap.

### <a name="steps-in-a-query"></a>Stappen in een query
Een query kan een of meer stappen bevatten. Elke stap wordt een subquery die zijn gedefinieerd door de **WITH** trefwoord. De query die zich buiten de **WITH** sleutelwoord (slechts één query) is ook geteld als een stap, zoals de **Selecteer** -instructie in de volgende query uit:

Query:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Deze query bestaat uit twee stappen.

> [!NOTE]
> Deze query wordt later in het artikel in meer detail besproken.
>  

### <a name="partition-a-step"></a>Een stap partitioneren
Een stap partitioneren, moet de volgende voorwaarden:

* De invoerbron moet worden gepartitioneerd. 
* De **Selecteer** instructie van de query van een gepartitioneerde invoerbron moet lezen.
* De query in de stap moet hebben de **PARTITION BY** trefwoord.

Wanneer een query is gepartitioneerd, de invoer gebeurtenissen worden verwerkt en samengevoegd in afzonderlijke partitiegroepen en uitvoer gebeurtenissen worden gegenereerd voor elk van de groepen. Als u wilt dat een gecombineerde statistische functie, moet u een tweede stap van niet-gepartitioneerde maken om samen te voegen.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Het maximum aantal streaming-eenheden voor een taak berekenen
Alle niet-gepartitioneerde stappen kunnen samen worden geschaald tot zes streaming-eenheden (su's) voor een Stream Analytics-taak. Daarnaast kunt u 6 su's voor elke partitie in een gepartitioneerde stap toevoegen.
Vindt u enkele **voorbeelden** in de onderstaande tabel.

| Query’s uitvoeren                                               | Max su's voor de taak |
| --------------------------------------------------- | ------------------- |
| <ul><li>De query bevat één stap.</li><li>De stap niet is gepartitioneerd.</li></ul> | 6 |
| <ul><li>De invoer van de gegevensstroom is gepartitioneerd op basis van 16.</li><li>De query bevat één stap.</li><li>De stap is gepartitioneerd.</li></ul> | 96 (6 * 16 partities) |
| <ul><li>De query bevat twee stappen.</li><li>Geen van de stappen is gepartitioneerd.</li></ul> | 6 |
| <ul><li>De stroom inkomende gegevens zijn gepartitioneerd met 3.</li><li>De query bevat twee stappen. De invoer stap is gepartitioneerd en de tweede stap is het niet.</li><li>De <strong>Selecteer</strong> instructie van de gepartitioneerde invoer leest.</li></ul> | 24 uur per dag (18 voor gepartitioneerde stappen + 6 voor niet-gepartitioneerde stappen |

### <a name="examples-of-scaling"></a>Voorbeelden van schaal

De volgende query berekent het aantal auto's binnen een drie minuten een gratis-station met drie tollbooths te doorlopen. Deze query kan worden geschaald tot maximaal zes su's.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Voor het gebruik van meer su's voor de query, moeten zowel de invoer gegevensstroom als de query worden gepartitioneerd. Omdat de partitie van de stroom is ingesteld op 3, kan de volgende gewijzigde query worden geschaald tot maximaal 18 su's:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wanneer een query is gepartitioneerd, worden de invoergebeurtenissen verwerkt en samengevoegd in een afzonderlijke partitiegroepen. Uitvoergebeurtenissen worden ook gegenereerd voor elk van de groepen. Partitioneren kan leiden tot enige onverwachte resultaten wanneer de **GROUP BY** veld is niet de partitiesleutel in de stroom inkomende gegevens. Bijvoorbeeld, de **TollBoothId** veld in de vorige query is niet de partitiesleutel van **Input1**. Het resultaat is dat de gegevens van stencil #1 in meerdere partities kunnen worden verspreid.

Elk van de **Input1** partities worden afzonderlijk verwerkt door Stream Analytics. Als gevolg hiervan wordt meerdere records van het aantal auto's voor de dezelfde stencil in de dezelfde tumblingvenster gemaakt. Als de invoer van de partitiesleutel kan niet worden gewijzigd, kan dit probleem worden opgelost door een niet-partitie stap toe te voegen aan samengevoegde waarden over meerdere partities, zoals in het volgende voorbeeld:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Deze query kan worden geschaald tot 24 su's.

> [!NOTE]
> Als u van twee streams samenvoegen, ervoor zorgen dat de stromen worden gepartitioneerd op basis van de partitiesleutel van de kolom die u kunt de joins maken. Zorg ook dat u hetzelfde aantal partities in beide stromen hebben.
> 
> 





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

