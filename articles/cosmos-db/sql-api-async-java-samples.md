---
title: 'Azure Cosmos DB: Async Java-voorbeelden voor de SQL API'
description: Vind Async Java-voorbeelden op GitHub voor veelvoorkomende taken met de SQL API in Azure Cosmos DB, zoals CRUD-bewerkingen.
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: sample
ms.date: 06/18/2018
ms.author: sngun
ms.openlocfilehash: 34551dcc64e6add2baa1ec94067037c98de67abe
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917937"
---
# <a name="azure-cosmos-db-async-java-examples-for-the-sql-api"></a>Azure Cosmos DB: Async Java-voorbeelden voor de SQL API

> [!div class="op_single_selector"]
> * [.NET-voorbeelden](sql-api-dotnet-samples.md)
> * [Java-voorbeelden](sql-api-java-samples.md)
> * [Async Java-voorbeelden](sql-api-async-java-samples.md)
> * [Node.js-voorbeelden](sql-api-nodejs-samples.md)
> * [Voorbeelden van Python](sql-api-python-samples.md)
> * [Galerie met voorbeelden van Azure-Code](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

De nieuwste voorbeeldtoepassingen waarmee CRUD-bewerkingen en andere veelvoorkomende bewerkingen in Azure Cosmos DB-resources worden uitgevoerd, zijn opgenomen in de GitHub-opslagplaats [azure-cosmosdb-java](https://github.com/Azure/azure-cosmosdb-java). Dit artikel bevat:

* Koppelingen naar de taken in elk van de Async Java-voorbeeldprojectbestanden. 
* Koppelingen naar het bijbehorende API-referentiemateriaal.

**Vereisten**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- U kunt [voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Via uw Visual Studio-abonnement ontvangt u elke maand een tegoed dat u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U hebt het volgende nodig om deze voorbeeldtoepassing uit te voeren:

* Java Development Kit 8
* Microsoft Azure Cosmos DB Java SDK

U kunt eventueel Maven gebruiken om de recentste binaire Microsoft Azure Cosmos DB Java SDK-bestanden op te halen voor gebruik in uw project. U kunt [hier](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) de recentste versie kiezen. Maven voegt automatisch eventuele vereiste afhankelijkheden toe. Anders kunt u de afhankelijkheden die worden vermeld in het bestand pom.xml, rechtstreeks downloaden en toevoegen aan uw build-pad.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-cosmosdb</artifactId>
    <version>${LATEST}</version>
</dependency>
```

**Uitvoeren van de voorbeeldtoepassingen**

De voorbeeldopslagplaats klonen:
```bash
$ git clone https://github.com/Azure/azure-cosmosdb-java.git

$ cd azure-cosmosdb-java
```

U kunt de voorbeelden uitvoeren met behulp van Eclipse of vanaf de opdrachtregel met behulp van Maven.

Vanaf Eclipse uitvoeren:
* Laad het bovenliggende hoofdprojectbestand pom.xml in Eclipse. De azure-cosmosdb-voorbeelden moeten automatisch worden geladen.
* Voor het uitvoeren van de voorbeelden hebt u een geldig Azure Cosmos DB-eindpunt nodig. De eindpunten worden gelezen uit `src/test/java/com/microsoft/azure/cosmosdb/rx/examples/TestConfigurations.java`.
* U kunt de eindpuntreferenties als VM-argumenten in Eclipse JUnit Run Config doorgeven of u kunt de eindpuntreferenties in TestConfigurations.java plaatsen.
    ```bash
    -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```
* U kunt nu de voorbeelden uitvoeren als JUnit-tests in Eclipse.

Uitvoeren vanaf de opdrachtregel:
* U kunt voorbeelden ook uitvoeren met behulp van Maven:
* Voer Maven uit en geef uw Azure Cosmos DB-eindpuntreferenties door:
    ```bash
    mvn test -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```

   > [!NOTE]
   > Elk voorbeeld staat op zichzelf. Het stelt zichzelf in en aan het einde worden de gegevens automatisch opgeschoond. In de voorbeelden wordt [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createcollection) meerdere keren aangeroepen. Telkens wanneer dit wordt gedaan, wordt uw abonnement gefactureerd voor 1 uur gebruik voor de prestatielaag van de gemaakte verzameling. 
   > 
   > 

## <a name="database-examples"></a>Voorbeelden voor databases
De [DatabaseCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-databases voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een database maken](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L115-L132) | [AsyncDocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createdatabase) |
| [Kan niet maken van een database die al bestaat](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L189-L204) | |
| [Maken en lezen van een database](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L228-L249) | [AsyncDocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.readdatabase) |
| [Maken en een database verwijderen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L255-L276) | [AsyncDocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.deletedatabase) |
| [Maken en het opvragen van een database](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L282-L312) | [AsyncDocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.querydatabases) |

## <a name="collection-examples"></a>Voorbeelden voor verzamelingen
De [CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-verzamelingen voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling met één partitie maken](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L134-L153) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createcollection) |
| [Een aangepaste verzameling van meerdere partitie maken](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L164-L184) | [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.documentcollection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.partitionkeydefinition)<br>[RequestOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.requestoptions) |
| [Failover naar een verzameling maken die er al bestaat](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L241-L256) | |
| [Maken en lezen van een verzameling](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L281-L304) | [AsyncDocumentClient.readCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.readcollection) |
| [Maken en een verzameling verwijderen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L310-L333) | [AsyncDocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.deletecollection) |
| [Maken en het opvragen van een verzameling](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L339-L372) | [AsyncDocumentClient.queryCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.querycollections) |

## <a name="document-examples"></a>Voorbeelden voor documenten
De [DocumentCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de Azure Cosmos-documenten voordat de volgende voorbeelden wordt uitgevoerd, [werken met databases, containers en objecten](databases-containers-items.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een document maken](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L137-L156) | [AsyncDocumentClient.createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createdocument) |
| [Een document met de documentdefinitie van een programmeerbare maken](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L213-L242) | [Document](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.document)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.resource.setid) |
| [Maken van documenten en vinden van de totale kosten van de RU](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L248-L280) | [ResourceResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.resourceresponse.getrequestcharge) |
| [Kan geen maken van een document dat al bestaat](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L316-L336) | |
| [Maken en een document vervangen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L342-L365) | [AsyncDocumentClient.replaceDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.replacedocument) |
| [Maken en een document upsert](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L371-L393) | [AsyncDocumentClient.upsertDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.upsertdocument) |
| [Maken en een document verwijderen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L399-L431) | [AsyncDocumentClient.deleteDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.deletedocument) |
| [Maken en een document lezen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L437-L458) | [AsyncDocumentClient.readDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.readdocument) |

## <a name="indexing-examples"></a>Voorbeelden van indexen
De [CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren.  Zie voor meer informatie over voordat de volgende voorbeelden wordt uitgevoerd in Azure Cosmos DB-indexering [indexeringsbeleid](index-policy.md), [indexeren typen](index-types.md), en [indexeren paden](index-paths.md) conceptuele artikelen. 

| Taak | API-verwijzing |
| --- | --- |
| [Maak een index en een set indexeringsbeleid](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L394-L410) | [Index](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.index)<br>[IndexingPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.indexingpolicy) |

Zie [Azure Cosmos DB-indexeringsbeleid](index-policy.md) voor meer informatie over indexering.

## <a name="query-examples"></a>Voorbeelden van query's
De [DocumentQuerySamples](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over een verwijzing naar de SQL-query in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [SQL-queryvoorbeelden](how-to-sql-query.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een eenvoudig document-query uitvoeren](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L154-L190) | [AsyncDocumentClient.queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.querydocuments) |
| [Een eenvoudig document-query uitvoeren en totaal aantal RU kosten zoeken](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L249-L268) | [FeedResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.feedresponse.getrequestcharge) |
| [Een eenvoudig document-query uitvoeren, lezen van één pagina en afmelden voor de geretourneerde zichtbaar zijn](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L274-L312) | |
| [Een eenvoudig document-query uitvoeren en de resultaten te filteren](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L318-L368) | |
| [Een partitie-overkoepelende document volgorde door query uitvoeren](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L410-L457) | [FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.feedoptions.setenablecrosspartitionquery) |

Zie [SQL-query in Azure Cosmos DB](how-to-sql-query.md) voor meer informatie over het schrijven van query's.

## <a name="offer-examples"></a>Voorbeelden van aanbieding
Het bestand [OfferCRUDAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java) laat u zien hoe u de volgende taken uitvoert:

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maakt en doorvoer instellen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L106-L113) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createcollection)<br>[RequestOptions.setOfferThroughput](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.requestoptions.setofferthroughput) |
| [Een verzameling om te vinden van de aanbieding van de bijbehorende lezen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L118-L130) | [Offer.getContent](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.offer.getContent)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.queryoffers) |
| [Doorvoer van een verzameling bijwerken door het vervangen van de aanbieding](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L101-L153) | [AsyncDocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.replaceoffer) |

## <a name="stored-procedure-examples"></a>Voorbeelden van opgeslagen procedures
De [StoredProcedureAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over programmeren-Server in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [opgeslagen procedures, triggers en gebruikersgedefinieerde functies](stored-procedures-triggers-udfs.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Maken en uitvoeren van een opgeslagen procedure](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L108-L155) | [StoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.storedprocedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createstoredprocedure)<br>[AsyncDocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.executestoredprocedure) |
| [Maken en uitvoeren van een opgeslagen procedure met argumenten](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L161-L195) | |
| [Maken en uitvoeren van een opgeslagen procedure met het ongeldige objectargument](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L201-L241) | |

## <a name="unique-key"></a>Unieke sleutel
De [UniqueIndexAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java) bestand ziet u hoe u de volgende taken uitvoeren. Zie voor meer informatie over de unieke sleutels in Azure Cosmos DB voordat de volgende voorbeelden wordt uitgevoerd, [Unique key-beperkingen](unique-keys.md) conceptueel artikel. 

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maken met een unieke sleutel](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java#L65-L97) | [UniqueKey](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.uniquekey)<br>[UniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.uniquekeypolicy)<br>[DocumentCollection.setUniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.documentcollection.setuniquekeypolicy) |
