---
title: Monitor en fouten opsporen met metrische gegevens in Azure Cosmos DB
description: Metrische gegevens in Azure Cosmos DB gebruiken voor veelvoorkomende problemen kunt verhelpen en controleren van de database.
ms.service: cosmos-db
author: kanshiG
ms.author: sngun
ms.topic: conceptual
ms.date: 06/18/2019
ms.reviewer: sngun
ms.openlocfilehash: ef457fe8c21bc7e62f910a78913069df32bea1a3
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275672"
---
# <a name="monitor-and-debug-with-metrics-in-azure-cosmos-db"></a>Monitor en fouten opsporen met metrische gegevens in Azure Cosmos DB

Azure Cosmos DB biedt metrische gegevens voor doorvoer, opslag, consistentie, beschikbaarheid en latentie. De Azure-portal biedt een samengevoegde weergave van deze metrische gegevens. U kunt ook Azure Cosmos DB-metrische gegevens van Azure Monitor API weergeven. Zie voor meer informatie over het weergeven van metrische gegevens van Azure monitor, de [metrische gegevens ophalen uit Azure Monitor](cosmos-db-azure-monitor-metrics.md) artikel. 

Dit artikel begeleidt veelvoorkomende use cases en hoe de Azure Cosmos DB metrische gegevens te analyseren en foutopsporing van deze problemen kunnen worden gebruikt. Metrische gegevens worden verzameld om de vijf minuten en gedurende zeven dagen worden bewaard.

## <a name="view-metrics-from-azure-portal"></a>Metrische gegevens weergeven vanuit Azure portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/)

1. Open de **metrische gegevens** deelvenster. Standaard, het deelvenster met de metrische gegevens geeft de opslag, indexeren, aanvraag eenheden metrische gegevens voor alle databases in uw Azure Cosmos-account. U kunt deze metrische gegevens per database, container of een regio filteren. U kunt ook de metrische gegevens in een specifieke tijdgranulatie filteren. Meer informatie over de doorvoer, opslag, beschikbaarheid, latentie en consistentie van metrische gegevens vindt u op een afzonderlijk tabblad. 

   ![Cosmos DB-maatstaven voor prestaties in Azure portal](./media/use-metrics/performance-metrics.png)

De volgende metrische gegevens zijn beschikbaar via de **metrische gegevens** deelvenster: 

* **Metrische gegevens over doorvoer** -met deze metriek wordt het aantal aanvragen die worden gebruikt of is mislukt (fout 429 code), omdat de doorvoer of opslagcapaciteit ingericht voor de container heeft overschreden.

* **Metrische opslaggegevens** -met deze metriek wordt de grootte van het gebruik van gegevens en index.

* **Metrische gegevens over beschikbaarheid** -met deze metriek wordt het percentage van geslaagde aanvragen ten opzichte van het totale aantal aanvragen per uur. Het slagingspercentage wordt gedefinieerd door de SLA's voor Azure Cosmos DB.

* **Metrieken voor latentie bij** -met deze metriek wordt het lezen en schrijven latentie waargenomen door Azure Cosmos DB in de regio waar uw account wordt uitgevoerd. Meer regio's voor een geo-replicatie-account, kunt u visualiseren van latentie. Met deze metriek staan niet voor de voor end-to-end-aanvraaglatentie.

* **Consistentie van metrische gegevens** -met deze metriek wordt aangegeven hoe uiteindelijke consistentie voor de consistentiemodel dat u kiest. Voor accounts voor meerdere regio's, ook ziet u de replicatielatentie tussen de regio's die u hebt geselecteerd.

* **Systeemmeetgegevens** -met deze metriek wordt het aantal aanvragen voor metagegevens worden aangeboden door de master-partitie. Het helpt tevens te identificeren van de beperkte aanvragen.

De volgende secties worden algemene scenario's waar u metrische gegevens van Azure Cosmos DB kunt gebruiken. 

## <a name="understand-how-many-requests-are-succeeding-or-causing-errors"></a>Begrijpen hoe veel aanvragen zijn slaagt of fouten veroorzaken

Als u wilt beginnen, Ga naar de [Azure-portal](https://portal.azure.com) en navigeer naar de **metrische gegevens** blade. Zoek in de blade de ** aantal aanvragen heeft capaciteit per 1 minuut grafiek overschreden. In deze grafiek toont een minuut door minuut totaal aantal aanvragen dat gesegmenteerd op de statuscode. Zie voor meer informatie over HTTP-statuscodes [HTTP-statuscodes voor Azure Cosmos DB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

De meest voorkomende foutcode voor de status is 429 (snelheid beperken/beperking). Deze fout betekent dat aanvragen voor Azure Cosmos DB meer dan de ingerichte doorvoer zijn. De meest voorkomende oplossing voor dit probleem is [de RU's opschalen](./set-throughput.md) voor de desbetreffende verzameling.

![Aantal aanvragen per minuut](media/use-metrics/metrics-12.png)

## <a name="determine-the-throughput-distribution-across-partitions"></a>Bepalen van de doorvoer-distributie over meerdere partities

Met een goede kardinaliteit van de partitiesleutels is essentieel voor elke schaalbare toepassing. Om te bepalen van de distributie van de doorvoer van een gepartitioneerde container onderverdeeld op basis van partities, gaat u naar de **blade met metrische gegevens** in de [Azure-portal](https://portal.azure.com). In de **doorvoer** tabblad de uitsplitsing van de opslag wordt weergegeven in de **maximaal verbruikte ru's / seconde per elke fysieke partitie** grafiek. De volgende afbeelding toont een voorbeeld van een slechte verdeling van gegevens, zoals aangegeven in de ongelijke partitie aan de linkerkant.

![Één partitie intensief gebruik zien om 3:05 uur](media/use-metrics/metrics-17.png)

Een doorvoer ongelijke verdeling kan ertoe leiden dat *hot* partities, die kunnen leiden tot beperkte aanvragen en is mogelijk opnieuw partitioneren. Zie voor meer informatie over partitionering in Azure Cosmos DB, [partitioneren en schalen in Azure Cosmos DB](./partition-data.md).

## <a name="determine-the-storage-distribution-across-partitions"></a>Bepalen van de opslag-distributie over meerdere partities

Met een goede kardinaliteit van de partitie is essentieel voor elke schaalbare toepassing. Om te bepalen van de distributie van de opslag van een gepartitioneerde container onderverdeeld op basis van partities, Ga naar de blade met metrische gegevens in de [Azure-portal](https://portal.azure.com). In het tabblad opslag de uitsplitsing van de opslag wordt weergegeven in de gegevens + opslag die door de bovenste partitie sleutels grafiek Index. De volgende afbeelding ziet u een slechte verdeling van de gegevensopslag zoals aangegeven in de ongelijke partitie aan de linkerkant.

![Voorbeeld van slechte gegevensdistributie](media/use-metrics/metrics-07.png)

U kunt de hoofd-oorzaak welke partitiesleutel de distributie scheeftrekken is door te klikken op de partitie in de grafiek.

![Partitiesleutel is de distributie scheeftrekken](media/use-metrics/metrics-05.png)

Nadat het identificeren van welke partitiesleutel veroorzaakt het scheeftrekken in distributie, moet u wellicht opnieuw partitioneren van de container met een meer gedistribueerde partitiesleutel. Zie voor meer informatie over partitionering in Azure Cosmos DB, [partitioneren en schalen in Azure Cosmos DB](./partition-data.md).

## <a name="compare-data-size-against-index-size"></a>Vergelijk de grootte op basis van de indexgrootte van de

In Azure Cosmos DB is de totale verbruikte opslag de combinatie van de grootte en de grootte van de Index. De indexgrootte van de is meestal een fractie van de grootte van de gegevens. In de blade met metrische gegevens in de [Azure-portal](https://portal.azure.com), het tabblad opslag brengt de uitsplitsing van het gebruik van opslag op basis van gegevens en index.

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

Als u wilt de index ruimte besparen, kunt u de [indexeringsbeleid](index-policy.md).

## <a name="debug-why-queries-are-running-slow"></a>Fouten opsporen in waarom query's worden traag uitgevoerd

In de SDK's van SQL API biedt Azure Cosmos DB Uitvoeringsstatistieken query.

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName),
 "SELECT * FROM c WHERE c.city = 'Seattle'",
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

*QueryMetrics* vindt u informatie over hoe lang elk onderdeel van de query om te worden uitgevoerd duurde. De meest voorkomende oorzaak voor langlopende query's scans is, wat betekent dat de query kan niet gebruikmaken van de indexen is. Dit probleem kan worden opgelost met een betere filtervoorwaarde.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe u voor het bewaken van en foutopsporing problemen met de metrische gegevens die in Azure portal. U kunt voor meer informatie over het verbeteren van de prestaties van de database door te lezen van de volgende artikelen:

* Zie voor meer informatie over het weergeven van metrische gegevens van Azure monitor, de [metrische gegevens ophalen uit Azure Monitor](cosmos-db-azure-monitor-metrics.md) artikel. 
* [Prestaties en schaal testen met Azure Cosmos DB](performance-testing.md)
* [Tips voor betere prestaties voor Azure Cosmos DB](performance-tips.md)
