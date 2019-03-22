---
title: Voorbeelden van SQL API Python voor Azure Cosmos DB
description: Vind Python-voorbeelden op GitHub voor veelvoorkomende taken in Azure Cosmos DB, zoals CRUD-bewerkingen.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: sample
ms.date: 03/14/2018
ms.author: sngun
ms.openlocfilehash: cf296d8bb494307dbb58b9de522d55a83892c6d5
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537317"
---
# <a name="azure-cosmos-db-python-examples"></a>Python-voorbeelden voor Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET-voorbeelden](sql-api-dotnet-samples.md)
> * [Java-voorbeelden](sql-api-java-samples.md)
> * [Async Java-voorbeelden](sql-api-async-java-samples.md)
> * [Node.js-voorbeelden](sql-api-nodejs-samples.md)
> * [Python-voorbeelden](sql-api-python-samples.md)
> * [Galerie met codevoorbeelden voor Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Voorbeeldoplossingen waarmee CRUD-bewerkingen en andere veelvoorkomende bewerkingen worden uitgevoerd in Azure Cosmos DB-resources zijn opgenomen in de GitHub-opslagplaats [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python). Dit artikel bevat:

* Koppelingen naar de taken in elk van de Python-voorbeeldprojectbestanden. 
* Koppelingen naar het bijbehorende API-referentiemateriaal.

**Vereisten**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- U kunt [voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Via uw Visual Studio-abonnement ontvangt u elke maand een tegoed dat u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U hebt ook de [Python SDK](sql-api-sdk-python.md) nodig. 
   
   > [!NOTE]
   > Elk voorbeeld staat op zichzelf. Het stelt zichzelf in en aan het einde worden de gegevens automatisch opgeschoond. De voorbeelden doen meerdere aanroepen aan [CosmosClient.CreateContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createcontainer-database-link--collection--options-none-). Steeds wanneer dit wordt gedaan, wordt uw abonnement gefactureerd voor het gebruik voor één uur. Zie [Azure Cosmos DB-prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie over Azure Cosmos DB-facturering.
   > 
   > 

## <a name="database-examples"></a>Voorbeelden voor databases
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py) -bestand van de [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement) project laat zien hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-databases voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een database maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L65-L76) |[CosmosClient.CreateDatabase](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createdatabase-database--options-none-) |
| [Een database lezen op id](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L79-L96) |[CosmosClient.ReadDatabase](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readdatabase-database-link--options-none-) |
| [Databases voor een account weergeven](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L99-L110) |[CosmosClient.ReadDatabases](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readdatabases-options-none-) |
| [Een database verwijderen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L113-L126) |[CosmosClient.DeleteDatabase](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#deletedatabase-database-link--options-none-) |

## <a name="collection-examples"></a>Voorbeelden voor verzamelingen
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py) -bestand van de [CollectionManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement) project laat zien hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-verzamelingen voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L84-L135) |[CosmosClient.CreateContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createcontainer-database-link--collection--options-none-) |
| [Een lijst van alle verzamelingen in een database lezen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L210-L222) |[CosmosClient.ReadContainers](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readcontainers-database-link--options-none-) |
| [Een verzameling ophalen op id](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L190-L208) |[CosmosClient.ReadContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readcontainer-collection-link--options-none-) |
| [De doorvoer van een verzameling wijzigen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L184-L188) | [CosmosClient.ReplaceOffer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#replaceoffer-offer-link--offer-)|
| [Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L224-L238) |[CosmosClient.DeleteContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#deletecontainer-collection-link--options-none-) |

## <a name="document-examples"></a>Voorbeelden voor documenten
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py) -bestand van de [DocumentManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement) project laat zien hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-documenten voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een document maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[CosmosClient.CreateItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createitem-database-or-container-link--document--options-none-) |
| [Een verzameling van documenten maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[CosmosClient.CreateItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createitem-database-or-container-link--document--options-none-) |
| [Een document lezen op id](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L69-L78) |[CosmosClient.ReadItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readitem-document-link--options-none-) |
| [Alle documenten in een verzameling lezen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L81-L92) |[CosmosClient.ReadItems](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readitems-collection-link--feed-options-none-) |
| [Document vervangen door voorwaardelijke ETag-controle](https://github.com/Azure/azure-cosmos-python/blob/a21f6fb4bad3f59909ef43558b598f9fb476b7bc/test/crud_tests.py#L1216-L1218) | [CosmosClient.ReplaceItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#replaceitem-document-link--new-document--options-none-) |

## <a name="indexing-examples"></a>Voorbeelden van indexen
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py) -bestand van de [IndexManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement) project laat zien hoe u de volgende taken uitvoeren.  Zie voor meer informatie over voordat de volgende voorbeelden wordt uitgevoerd in Azure Cosmos DB-indexering [indexeringsbeleid](index-policy.md), [indexeren typen](index-types.md), en [indexeren paden](index-paths.md) conceptuele artikelen. 

| Taak | API-verwijzing |
| --- | --- |
| [Handmatige (in plaats van automatische) indexering gebruiken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L245-L246) | Beleid voor automatisch indexeren |
| [Opgegeven documentpaden uitsluiten van de index ](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L294-L367) | Indexeringsbeleid met uitgesloten paden|
| [Een document uitsluiten van de index](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L204-L210) |[IndexingDirective.Exclude](/python/api/azure-cosmos/azure.cosmos.documents.indexingdirective#exclude) |
| [Indexeringsmodus instellen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L533) |[IndexingMode](/python/api/azure-cosmos/azure.cosmos.documents.indexingmode) |
| [Bereikindexen voor tekenreeksen gebruiken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L440-L456) | Indexeringsbeleid met opgenomen paden|
| [Een indextransformatie uitvoeren](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L514-L559) |[CosmosClient.ReplaceContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#replacecontainer-collection-link--collection--options-none-) |

## <a name="query-examples"></a>Voorbeelden van query's
De voorbeeldprojecten laten ook zien hoe u de volgende querytaken uitvoeren. Zie voor meer informatie over een verwijzing naar de SQL-query in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [SQL-queryvoorbeelden](how-to-sql-query.md) conceptueel artikel. Zie voor meer informatie over een verwijzing naar de SQL-query in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [SQL-queryvoorbeelden](how-to-sql-query.md) conceptueel artikel. 


| Taak | API-verwijzing |
| --- | --- |
| [Een query uitvoeren op een account voor een database](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L49-L62) |[CosmosClient.QueryDatabases](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#querydatabases-query--options-none-) |
| [Een query uitvoeren voor documenten](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L152-L169) |[CosmosClient.QueryItems](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#queryitems-database-or-container-link--query--options-none--partition-key-none-) |
| [Een bereikscanbewerking voor een met hash geïndexeerd pad afdwingen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L409-L415) |[HttpHeaders.EnableScanInQuery](/python/api/azure-cosmos/azure.cosmos.http_constants.httpheaders#enablescaninquery) |

