---
title: Controle en foutopsporing met metrische gegevens in Azure Cosmos DB | Microsoft Docs
description: Metrische gegevens in Azure Cosmos DB gebruiken voor veelvoorkomende problemen kunt verhelpen en controleren van de database.
keywords: metrics
services: cosmos-db
author: kanshiG
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: govindk
ms.openlocfilehash: 9b9f72812b1a1f0e30379c32e10d316fcbf71d3b
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345586"
---
# <a name="monitoring-and-debugging-with-metrics-in-azure-cosmos-db"></a>Controle en foutopsporing met metrische gegevens in Azure Cosmos DB

Azure Cosmos DB biedt metrische gegevens voor doorvoer, opslag, consistentie, beschikbaarheid en latentie. De [Azure-portal](https://portal.azure.com) biedt een samengevoegde weergave van deze metrische gegevens; voor gedetailleerdere metrische gegevens, zowel de client-SDK en de [diagnostische logboeken](./logging.md) beschikbaar zijn.

Voor een overzicht van de nieuwe metrische gegevens en meer zoeken naar hot-partities in de database, de volgende Azure Friday-video te bekijken:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Cosmos-DB-Get-the-Most-Out-of-Provisioned-Throughput/player]
> 

Dit artikel begeleidt veelvoorkomende use cases en hoe de Azure Cosmos DB metrische gegevens te analyseren en foutopsporing van deze problemen kunnen worden gebruikt. Metrische gegevens worden verzameld om de vijf minuten en gedurende zeven dagen worden bewaard.

## <a name="understanding-how-many-requests-are-succeeding-or-causing-errors"></a>Inzicht krijgen in hoe veel aanvragen zijn slaagt of fouten veroorzaken

Als u wilt beginnen, Ga naar de [Azure-portal](https://portal.azure.com) en navigeer naar de **metrische gegevens** blade. Zoek in de blade de **aantal aanvragen heeft capaciteit per één minuut overschreden** grafiek. In deze grafiek toont een minuut door minuut totaal aantal aanvragen dat gesegmenteerd op de statuscode. Zie voor meer informatie over HTTP-statuscodes [HTTP-statuscodes voor Azure Cosmos DB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

De meest voorkomende foutstatuscode wordt 429 (snelheid beperken/beperking), wat betekent dat de ingerichte doorvoer zijn meer dan aanvragen met Azure Cosmos DB. De meest voorkomende oplossing hiervoor is het [de RU's opschalen](./set-throughput.md) voor de desbetreffende verzameling.

![Aantal aanvragen per minuut](media/use-metrics/metrics-12.png)

## <a name="determining-the-throughput-distribution-across-partitions"></a>Bepalen van de doorvoer-distributie over meerdere partities

Met een goede kardinaliteit van de partitiesleutels is essentieel voor elke schaalbare toepassing. Om te bepalen van de distributie van de doorvoer van een gepartitioneerde verzameling onderverdeeld op basis van partities, gaat u naar de **blade met metrische gegevens** in de [Azure-portal](https://portal.azure.com). In de **doorvoer** tabblad de uitsplitsing van de opslag wordt weergegeven in de **maximaal verbruikte ru's / seconde per elke fysieke partitie** grafiek. De volgende afbeelding toont een voorbeeld van een slechte verdeling van gegevens als blijkt uit de ongelijke partitie aan de linkerkant. 

![Één partitie intensief gebruik zien om 3:05 uur](media/use-metrics/metrics-17.png)

Een doorvoer ongelijke verdeling kan ertoe leiden dat *hot* partities, die kunnen leiden tot beperkte aanvragen en is mogelijk opnieuw partitioneren. Zie voor meer informatie over partitionering in Azure Cosmos DB, [partitioneren en schalen in Azure Cosmos DB](./partition-data.md).

## <a name="determining-the-storage-distribution-across-partitions"></a>Bepalen van de opslag-distributie over meerdere partities

Met een goede kardinaliteit van de partitie is essentieel voor elke schaalbare toepassing. Om te bepalen van de distributie van de doorvoer van een gepartitioneerde verzameling onderverdeeld op basis van partities, Ga naar de blade met metrische gegevens in de [Azure-portal](https://portal.azure.com). Op het tabblad doorvoer wordt de uitsplitsing van de opslag weergegeven in het maximum aantal verbruikte RU/seconde per elke fysieke partitie-grafiek. De volgende afbeelding ziet u een slechte verdeling van gegevens als blijkt uit de ongelijke partitie aan de linkerkant. 

![Voorbeeld van slechte gegevensdistributie](media/use-metrics/metrics-07.png)

U kunt de hoofd-oorzaak welke partitiesleutel de distributie scheeftrekken is door te klikken op de partitie in de grafiek. 

![Partitiesleutel is de distributie scheeftrekken](media/use-metrics/metrics-05.png)

Nadat identificeren welke partitiesleutel het scheeftrekken in distributie veroorzaakt, moet u wellicht uw verzameling met een meer gedistribueerde partitiesleutel partitioneren. Zie voor meer informatie over partitionering in Azure Cosmos DB, [partitioneren en schalen in Azure Cosmos DB](./partition-data.md).

## <a name="comparing-data-size-against-index-size"></a>Vergelijking van de grootte van de gegevens op basis van de indexgrootte van de

In Azure Cosmos DB is de totale verbruikte opslag de combinatie van de grootte en de grootte van de Index. De indexgrootte van de is meestal een fractie van de grootte van de gegevens. In de blade met metrische gegevens in de [Azure-portal](https://portal.azure.com), het tabblad opslag brengt de uitsplitsing van het gebruik van opslag op basis van gegevens en index. Afbeelding (misschien) ook van de SDK, u vindt het huidige gebruik van opslag via een verzameling lezen.
```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll")); 
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
``` 
Als u wilt de index ruimte besparen, kunt u de [indexeringsbeleid](./indexing-policies.md).

## <a name="debugging-why-queries-are-running-slow"></a>Foutopsporing waarom query's worden traag uitgevoerd

In de SDK's van SQL API biedt Azure Cosmos DB Uitvoeringsstatistieken query. 

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

*QueryMetrics* vindt u informatie over hoe lang elk onderdeel van de query om te worden uitgevoerd duurde. De meest voorkomende hoofdoorzaak voor het uitvoeren van lange query's zijn scans (de query is gebruikmaken van de indexen), die opgelost met een betere filtervoorwaarde worden kunnen.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe om te controleren en problemen met fouten opsporen met behulp van de metrische gegevens die in Azure portal, kunt u meer informatie over het verbeteren van de prestaties van de database door te lezen van de volgende artikelen:

* [Prestaties en schaal testen met Azure Cosmos DB](performance-testing.md)
* [Tips voor betere prestaties voor Azure Cosmos DB](performance-tips.md)
