---
title: Query's uitvoeren op containers in Azure Cosmos DB
description: Leer hoe u query's kunt uitvoeren op containers in Azure Cosmos DB
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 11c68b61802f6c7b3755da71c176ea777f171e4c
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409833"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Query's uitvoeren op containers in Azure Cosmos DB

In dit artikel wordt uitgelegd hoe u query's kunt uitvoeren op containers (collecties, grafieken, tabellen) in Azure Cosmos DB.

## <a name="in-partition-query"></a>In-partitiequery

Wanneer u een query op een container uitvoert om gegevens op te vragen en voor de query een partitiesleutelfilter is opgegeven, stuurt Azure Cosmos DB de query automatisch naar de partities die overeenkomen met de partitiesleutelwaarden die zijn opgegeven in het filter. De volgende query wordt bijvoorbeeld doorgestuurd naar de partitie DeviceId die alle documenten bevat die overeenkomen met partitiesleutelwaarde 'XMS-0001'.

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Partitieoverkoepelende query

De volgende query heeft geen een filter voor de partitiesleutel (DeviceId) en wordt verspreid over alle partities waarop deze wordt uitgevoerd op basis van de index van de partitie. Als u een query wilt uitvoeren op meerdere partities, stelt u **EnableCrossPartitionQuery** in op waar (of x-ms-documentdb-query-enablecrosspartition in de REST API).

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Cosmos DB biedt ondersteuning voor de statistische functies COUNT, MIN, MAX en AVG voor containers door middel van SQL. De statistische functies voor containers beginnen vanaf SDK-versie 1.12.0. Query's moeten één samengestelde operator bevatten en één waarde in de projectie.

## <a name="parallel-cross-partition-query"></a>Parallelle partitieoverkoepelende query

De Cosmos DB-SDK's 1.9.0 en hoger bieden ondersteuning voor opties voor parallelle query-uitvoer.  Met parallelle partitieoverkoepelende query's kunt u query's met lage latentie uitvoeren op meerdere partities. De volgende query is bijvoorbeeld geconfigureerd voor parallelle uitvoering in meerdere partities.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

U kunt parallelle queryuitvoering beheren door de volgende parameters af te stemmen:

- **MaxDegreeOfParallelism**: hiermee wordt het maximum aantal simultane netwerkverbindingen met de partities van de container ingesteld. Als u deze eigenschap instelt op -1, wordt de mate van parallelle uitvoering beheerd door de SDK. Als MaxDegreeOfParallelism niet wordt opgegeven of op 0 (de standaardwaarde) ingesteld, is er één netwerkverbinding met de partities van de container.

- **MaxBufferedItemCount**: hiermee wordt de latentie van de query ingewisseld voor het geheugengebruik aan de clientzijde. Als deze optie wordt weggelaten of ingesteld op -1, wordt het aantal items dat in de buffer wordt opgeslagen tijdens parallelle queryuitvoering, beheerd door de SDK.

Met dezelfde status van de verzameling, retourneert een parallelle query resultaten in dezelfde volgorde als bij seriële uitvoering. Wanneer u een partitieoverkoepelende query uitvoert met sortering van operatoren (ORDER BY en/of TOP), wordt de query door de Azure Cosmos DB-SDK parallel in meerdere partities uitgegeven en worden gedeeltelijk gesorteerde resultaten samengevoegd aan de clientzijde om globaal geordende resultaten te produceren.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over partitionering in Cosmos DB:

- [Partitionering in Azure Cosmos DB](partitioning-overview.md)
- [Synthetic partition keys in Azure Cosmos DB](synthetic-partition-keys.md) (Synthetische partitiesleutels in Azure Cosmos DB)
