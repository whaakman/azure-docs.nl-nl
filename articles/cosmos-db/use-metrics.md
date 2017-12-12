---
title: Bewaking en foutopsporing met metrische gegevens in Azure Cosmos DB | Microsoft Docs
description: Gebruik metrische gegevens in Azure Cosmos DB opsporen veelvoorkomende problemen en de database bewaken.
keywords: metrics
services: cosmos-db
author: gnot
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: govindk
ms.openlocfilehash: 3b3de91c3850071d7c3fbff1faccde6c17a606e3
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="monitoring-and-debugging-with-metrics-in-azure-cosmos-db"></a>Bewaking en foutopsporing met metrische gegevens in Azure Cosmos-DB

Azure Cosmos DB voorziet in maatstaven voor doorvoer, opslag, consistentie, beschikbaarheid en latentie. De [Azure-portal](https://portal.azure.com) bevat een samengevoegde weergave van deze metrische gegevens; voor gedetailleerdere metrische gegevens, zowel de client-SDK en de [diagnostische logboeken](./logging.md) beschikbaar zijn.

Om een overzicht van de nieuwe metrische gegevens en informatie over zoeken hot partities in de database, moet u de volgende Azure vrijdag-video bekijken:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Cosmos-DB-Get-the-Most-Out-of-Provisioned-Throughput/player]
> 

Dit artikel begeleidt bij algemene gebruiksvoorbeelden en hoe Azure Cosmos DB metrische gegevens te analyseren en foutopsporing van deze problemen kunnen worden gebruikt. Metrische gegevens worden verzameld om de vijf minuten en zeven dagen worden bewaard.

## <a name="understanding-how-many-requests-are-succeeding-or-causing-errors"></a>Informatie over hoeveel aanvragen zijn slaagt of fouten veroorzaken

Om te beginnen, Ga naar de [Azure-portal](https://portal.azure.com) en navigeer naar de **metrische gegevens** blade. In de blade ziet u de **aantal aanvragen overschreden capaciteit per 1 minuut** grafiek. Dit diagram toont een minuut door minuut totaal aantal aanvragen dat gesegmenteerd op de statuscode. Zie voor meer informatie over HTTP-statuscodes [HTTP-statuscodes voor Azure Cosmos DB](https://docs.microsoft.com/rest/api/documentdb/http-status-codes-for-documentdb).

De meest voorkomende foutstatuscode is 429 (beperking), wat betekent dat aanvragen bij Azure Cosmos DB de ingerichte doorvoer overschrijdt. De meest voorkomende oplossing voor dit is [de RUs opschalen](./set-throughput.md) voor de opgegeven verzameling.

![Het aantal aanvragen per minuut](media/use-metrics/metrics-12.png)

## <a name="determining-the-throughput-distribution-across-partitions"></a>Bepalen van de distributie van doorvoer meerdere partities

Met een goede kardinaliteit van uw partitiesleutels is essentieel voor alle schaalbare toepassingen. Om te bepalen van de distributie van de doorvoer van een gepartitioneerde verzameling uitgesplitst partities, gaat u naar de **metrische gegevens blade** in de [Azure-portal](https://portal.azure.com). In de **doorvoer** tabblad uitsplitsing van de opslag wordt weergegeven in de **Max RU/seconde verbruikt door elke fysieke partitie** grafiek. De volgende afbeelding ziet u een voorbeeld van een slechte verdeling van gegevens als blijkt uit de vertekende partitie aan de linkerkant. 

![Één partitie intensief gebruik zien op 15:05:00](media/use-metrics/metrics-17.png)

Een doorvoer ongelijke verdeling kan ertoe leiden dat *hot* partities die kunnen leiden tot beperkte aanvragen en is mogelijk opnieuw partitioneren. Zie voor meer informatie over partitioneren in Azure Cosmos DB [partitie en schalen in Azure Cosmos DB](./partition-data.md).

## <a name="determining-the-storage-distribution-across-partitions"></a>Bepalen van de distributie opslag meerdere partities

Met een goede kardinaliteit van de partitie is essentieel voor alle schaalbare toepassingen. Om te bepalen van de distributie van de doorvoer van een gepartitioneerde verzameling uitgesplitst partities, Ga naar de blade metrische gegevens in de [Azure-portal](https://portal.azure.com). In het tabblad doorvoer uitsplitsing van de opslag weergegeven in het veld Max RU/seconde verbruikt door elke grafiek fysieke partitie. De volgende afbeelding ziet u een slechte verdeling van gegevens als blijkt uit de vertekende partitie aan de linkerkant. 

![Voorbeeld van slechte gegevensdistributie](media/use-metrics/metrics-07.png)

U kunt hoofdoorzaak met het welke partitiesleutel de distributie scheeftrekken is door te klikken op de partitie in de grafiek. 

![De partitiesleutel is de distributie scheeftrekken](media/use-metrics/metrics-05.png)

Nadat u te identificeren welke partitiesleutel veroorzaakt de tijdverschil in distributie, moet u wellicht uw verzameling met een meer gedistribueerde partitiesleutel partitioneren. Zie voor meer informatie over partitioneren in Azure Cosmos DB [partitie en schalen in Azure Cosmos DB](./partition-data.md).

## <a name="comparing-data-size-against-index-size"></a>Gegevensgrootte op basis van de indexgrootte van de vergelijken

In Azure Cosmos DB is de totale verbruikte opslag de combinatie van de omvang van gegevens en de grootte van de Index. De indexgrootte van de is meestal een fractie van de omvang van gegevens. In de blade metrische gegevens in de [Azure-portal](https://portal.azure.com), het tabblad opslag gepresenteerd de uitsplitsing van opslagverbruik op basis van gegevens en de index. Afbeelding (mogelijk) ook van de SDK vindt u het huidige opslaggebruik via een verzameling lezen.
```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll")); 
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
``` 
Als u besparen index wilt, kunt u aanpassen de [indexeren beleid](./indexing-policies.md).

## <a name="debugging-why-queries-are-running-slow"></a>Foutopsporing waarom query's worden langzaam uitgevoerd

In de SQL-SDK-API's biedt Azure Cosmos DB Uitvoeringsstatistieken query. 

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
 “SELECT * FROM c WHERE c.city = ‘Seattle’”, 
 new FeedOptions 
 { 
 PopulateQueryMetrics = true, 
 MaxItemCount = -1, 
 MaxDegreeOfParallelism = -1, 
 EnableCrossPartitionQuery = true 
 }).AsDocumentQuery();
FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id 
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;
```

*QueryMetrics* biedt details over hoe lang elk onderdeel van de query om te worden uitgevoerd duurde. De meest voorkomende hoofdoorzaak voor lange actieve query's zijn scans (van de query is gebruikmaken van de indexen), die omgezet met een betere filtervoorwaarde worden kunnen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u kunt bewaken en foutopsporing problemen met de metrische gegevens opgegeven in de Azure portal, wilt u mogelijk meer informatie over het verbeteren van de prestaties van de database door te lezen van de volgende artikelen:

* [Prestaties en schaal testen met Azure Cosmos-DB](performance-testing.md)
* [Tips voor betere prestaties voor Azure Cosmos-DB](performance-tips.md)
