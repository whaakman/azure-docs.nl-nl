---
title: Gebruikmaken van de query garandeert in Azure Stream Analytics | Microsoft Docs
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
ms.openlocfilehash: dd60026cad9246da8eba141125aebf061ecf7e9d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Gebruik de query garandeert in Azure Stream Analytics
In dit artikel leest u hoe om te profiteren van garandeert in Azure Stream Analytics. U leert hoe Stream Analytics-taken schalen door invoer partities configureren en de definitie van de analytics query afstemmen.
Als een vereiste kunt u bekend bent met het begrip van de Streaming-eenheid die wordt beschreven in [begrijpen en aanpassen van Streaming-eenheden](stream-analytics-streaming-unit-consumption.md).

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Wat zijn de onderdelen van een Stream Analytics-taak?
De definitie van een Stream Analytics-taak bevat invoer, een query- en uitvoer. Invoer is waar de taak leest de gegevensstroom uit. De query wordt gebruikt voor het transformeren van de invoerstroom van gegevens en de uitvoer is waar de taak verzendt de resultaten van de taak aan.  

Een taak vereist ten minste één invoerbron voor het streamen van gegevens. De invoerbron van gegevensstroom kan worden opgeslagen in een Azure event hub of in Azure blob-opslag. Zie voor meer informatie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md) en [aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-sources-and-sinks"></a>Partities in de bronnen en put
Schalen van een Stream Analytics-taak maakt gebruik van partities in de invoer of uitvoer. Hiermee kunt die u gegevens in op basis van een partitiesleutel subsets verdelen partitioneren. Een proces waarbij de gegevens (zoals een Streaming Analytics-taak) kunt gebruiken en het schrijven van verschillende partities parallel verhoogt de doorvoer. 

### <a name="inputs"></a>Invoer
Alle Azure Stream Analytics-invoer kan profiteren van het partitioneren van:
-   EventHub (u moet de partitiesleutel expliciet instellen)
-   IoT Hub (u moet de partitiesleutel expliciet instellen)
-   Blob Storage

### <a name="outputs"></a>Uitvoer

Wanneer u met Stream Analytics werkt, kunt u profiteren van het partitioneren van in de uitvoer:
-   Azure Data Lake Storage
-   Azure Functions
-   Azure-tabel
-   Blob Storage
-   CosmosDB (u moet de partitiesleutel expliciet instellen)
-   EventHub (u moet de partitiesleutel expliciet instellen)
-   IoT Hub (u moet de partitiesleutel expliciet instellen)
-   Service Bus

Power BI, SQL en SQL-datawarehouse uitvoer ondersteuning geen voor partitioneren. Maar u kunt nog steeds partitioneren de invoer zoals beschreven in [in deze sectie](#multi-step-query-with-a-grouping-key) 

Zie voor meer informatie over de partities, de volgende artikelen:

* [Overzicht van Event Hubs-functies](../event-hubs/event-hubs-features.md#partitions)
* [Partitioneren van gegevens](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="embarrassingly-parallel-jobs"></a>Perfect parallelle taken
Een *perfect parallelle* taak is het meest schaalbare scenario dat we in Azure Stream Analytics hebben. Een partitie van de invoer van één exemplaar van de query verbindt met een partitie van de uitvoer. Deze parallelle uitvoering heeft de volgende vereisten:

1. Als uw query logica is afhankelijk van dezelfde sleutel door hetzelfde exemplaar van de query wordt verwerkt, moet u ervoor zorgen dat de gebeurtenissen gaat u naar dezelfde partitie van uw invoer. Voor event hubs, dit betekent dat gegevens van de gebeurtenis moet de **PartitionKey** waarde ingesteld. U kunt ook de gepartitioneerde afzenders gebruiken. Dit betekent dat de gebeurtenissen worden verzonden naar de map met dezelfde partitie voor blob storage. Als uw query logica dezelfde sleutel moet worden verwerkt door de dezelfde query-instantie niet vereist, kunt u deze vereiste negeren. Een voorbeeld van deze logica is een eenvoudige select-project-filter-query.  

2. Nadat de gegevens worden verspreid de invoer-zijde, moet u ervoor zorgen dat uw query is gepartitioneerd. Hiervoor moet u gebruikmaken van **PARTITION BY** in alle stappen. Meerdere stappen zijn toegestaan, maar ze alle moeten worden gepartitioneerd met dezelfde sleutel. Op dit moment wordt de te nemen partitionerende sleutel moet worden ingesteld op **PartitionId** in volgorde van de taak moet worden volledig parallelle.  

3. De meeste van onze uitvoer kunt profiteren van partitioneren, maar als u een uitvoertype die biedt geen ondersteuning voor partitioneren uw taak niet volledig parallelle. Raadpleeg de [uitvoer sectie](#Outputs) voor meer informatie.

4. Het aantal invoer partities moet gelijk zijn aan het aantal partities van de uitvoer. BLOB storage uitvoer ondersteunt momenteel geen partities. Maar dat is geen probleem, omdat deze het partitieschema van de upstream-query. Hier volgen voorbeelden van waarden van partitie waarmee een volledig parallelle taak:  

   * 8 event hub invoer partities en 8 event hub uitvoer partities
   * 8 event hub invoer partities en uitvoer van blob-opslag  
   * 8 blob storage invoer partities en uitvoer van blob-opslag  
   * 8 blob-opslag invoer partities en 8 uitvoer partities van de event hub  

De volgende secties worden enkele voorbeeldscenario's die perfect parallelle zijn besproken.

### <a name="simple-query"></a>Eenvoudige query

* Invoer: Event hub, met 8 partities
* Uitvoer: Event hub, met 8 partities

Query:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Deze query is een eenvoudige filter. We hebben daarom te hoeven maken over het partitioneren van de invoer die worden verzonden naar de event hub. U ziet dat de query bevat een **partitie door PartitionId**, zodat deze voldoet aan vereiste #2 van eerder. De uitvoer moet voor het configureren van de event hub-uitvoer in de taak is het belangrijkste partitie worden ingesteld op **PartitionId**. Er is een laatste controle om ervoor te zorgen dat het aantal invoer partities gelijk aan het aantal partities van de uitvoer is.

### <a name="query-with-a-grouping-key"></a>Query's uitvoeren met een groeperingssleutel

* Invoer: Event hub, met 8 partities
* Uitvoer: Blob-opslag

Query:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Deze query heeft een groeperingssleutel. Daarom moeten de gebeurtenissen die gegroepeerd worden verzonden naar dezelfde Event Hub-partitie. Aangezien we in dit voorbeeld op TollBoothID groeperen, moet we zorgen dat TollBoothID als de partitiesleutel wordt gebruikt wanneer de gebeurtenissen naar een Event Hub worden verzonden. Klik in de ASA, we gebruiken **partitie door PartitionId** overnemen van deze partitieschema en volledige garandeert inschakelen. Omdat de uitvoer van de blob-opslag is, moeten we geen zorgen over het configureren van een waarde voor de partitiesleutel, volgens de vereiste #4.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Voorbeeld van scenario's die zijn *niet* perfect parallelle

In de vorige sectie bleek we enkele perfect parallelle scenario's. Scenario's die niet voldoen aan alle vereisten voor het perfect parallelle worden besproken in deze sectie. 

### <a name="mismatched-partition-count"></a>Aantal niet-overeenkomende partities
* Invoer: Event hub, met 8 partities
* Uitvoer: Event hub met 32 partities

In dit geval wordt het maakt niet uit wat de query is. Als het aantal partities invoer komt niet overeen met het aantal van de partities uitvoer, de topologie is niet perfect parallel + maar we kunnen nog steeds sommige niveau of garandeert.

### <a name="query-using-non-partitioned-output"></a>Query uitvoert met behulp van niet-gepartitioneerde uitvoer
* Invoer: Event hub, met 8 partities
* Uitvoer: Power BI

Power BI-uitvoer ondersteunt momenteel geen partitioneren. Dit scenario is daarom niet perfect parallelle.

### <a name="multi-step-query-with-different-partition-by-values"></a>De query meerdere stappen met verschillende waarden voor PARTITION BY
* Invoer: Event hub, met 8 partities
* Uitvoer: Event hub, met 8 partities

Query:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Zoals u ziet, gebruikmaakt van de tweede stap **TollBoothId** als de te nemen partitionerende sleutel. Deze stap is niet hetzelfde zijn als de eerste stap en daarom vereist we een willekeurige volgorde. 

De voorgaande voorbeelden worden sommige Stream Analytics-taken die voldoen aan (of niet) op een perfect parallelle topologie. Als die in overeenstemming zijn, hebben ze de mogelijkheden voor maximale schaal. Voor taken die niet voldoen aan een van deze profielen schalen richtlijnen beschikbaar in toekomstige zijn updates. Gebruik de algemene richtlijnen in de volgende secties voor nu.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>De maximale streaming-eenheden van een taak berekenen
Het totale aantal streaming-eenheden die kunnen worden gebruikt door een Stream Analytics-taak is afhankelijk van het aantal stappen in de query is opgegeven voor de taak en het aantal partities voor elke stap.

### <a name="steps-in-a-query"></a>De stappen in een query
Een query kan één of meerdere stappen hebben. Elke stap bestaat uit een subquery die zijn gedefinieerd door de **WITH** sleutelwoord. De query die buiten de **WITH** sleutelwoord (slechts één query) is ook beschouwd als een stap, zoals de **Selecteer** -instructie in de volgende query:

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
> Deze query wordt later in het artikel nader besproken.
>  

### <a name="partition-a-step"></a>Een stap partitioneren
Een stap partitioneren, moet de volgende voorwaarden:

* De invoerbron moet worden gepartitioneerd. 
* De **Selecteer** instructie van de query moet lezen van een gepartitioneerde invoerbron.
* De query in de stap moet hebben de **PARTITION BY** sleutelwoord.

Wanneer een query is gepartitioneerd, de invoer gebeurtenissen worden verwerkt en geaggregeerde in afzonderlijke partitiegroepen en uitvoer gebeurtenissen worden gegenereerd voor elk van de groepen. Als u een gecombineerde aggregaat wilt, moet u een tweede stap van niet-gepartitioneerde kunnen worden geaggregeerd.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Het maximum aantal streaming-eenheden voor een taak berekenen
Alle niet-gepartitioneerde stappen kunnen samen maximaal zes streaming-eenheden (SUs) voor een Stream Analytics-taak schalen. Daarnaast kunt u 6 SUs voor elke partitie in een gepartitioneerde stap toevoegen.
U ziet enkele **voorbeelden** in de onderstaande tabel.

| Query’s uitvoeren                                               | Maximale SUs voor de taak |
| --------------------------------------------------- | ------------------- |
| <ul><li>De query bevat één stap.</li><li>De stap niet is gepartitioneerd.</li></ul> | 6 |
| <ul><li>De invoer gegevensstroom is gepartitioneerd door 16.</li><li>De query bevat één stap.</li><li>De stap is gepartitioneerd.</li></ul> | 96 (6 * 16 partities) |
| <ul><li>De query bevat twee stappen.</li><li>Geen van de stappen is gepartitioneerd.</li></ul> | 6 |
| <ul><li>De invoer gegevensstroom is gepartitioneerd met 3.</li><li>De query bevat twee stappen. De invoer stap is gepartitioneerd en de tweede stap bestaat niet.</li><li>De <strong>Selecteer</strong> instructie leest uit de gepartitioneerde invoer.</li></ul> | 24 (18 voor gepartitioneerde stappen + 6 voor niet-gepartitioneerde stappen |

### <a name="examples-of-scaling"></a>Voorbeelden van schaal

De volgende query berekent het aantal auto's binnen een drie minuten een gratis-station met drie tollbooths doorlopen. Deze query kan worden uitgebreid tot maximaal zes SUs.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Als u wilt meer SUs gebruiken voor de query, moeten zowel de invoergegevens stroom en de query worden gepartitioneerd. Omdat de partitie van de gegevensstroom is ingesteld op 3, kunnen de volgende gewijzigde query kan worden uitgebreid tot 18 SUs:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wanneer een query is gepartitioneerd, worden de invoervelden verwerkt en geaggregeerd in een afzonderlijke partitiegroepen. Uitvoergebeurtenissen worden ook gegenereerd voor elk van de groepen. Partitioneren, kan sommige onverwachte resultaten opleveren wanneer de **GROUP BY** veld is niet de partitiesleutel in de stroom invoergegevens. Bijvoorbeeld, de **TollBoothId** veld in de vorige query is niet de partitiesleutel van **Input1**. Het resultaat is dat de gegevens van tolhuisje #1 in meerdere partities kunnen worden verspreid.

Elk van de **Input1** partities afzonderlijk wordt verwerkt door de Stream Analytics. Hierdoor kunnen wordt meerdere records van het aantal auto voor de dezelfde tolhuisje in de dezelfde tumblingvenster gemaakt. Als de invoer partitiesleutel kan niet worden gewijzigd, kan dit probleem worden opgelost door de stap van een niet-partitie op geaggregeerde waarden toe te voegen meerdere partities, zoals in het volgende voorbeeld:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Deze query kan worden geschaald naar 24 SUs.

> [!NOTE]
> Als u van twee streams samenvoegen, ervoor zorgen dat de stromen worden gepartitioneerd met de partitiesleutel van de kolom die u kunt de joins maken. Zorg ook dat u hetzelfde aantal partities in beide stromen hebben.
> 
> 





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

