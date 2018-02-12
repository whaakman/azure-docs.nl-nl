---
title: 'Azure Cosmos DB: Java-voorbeelden voor de SQL-API | Microsoft Docs'
description: Java-voorbeelden op GitHub voor algemene taken met de Azure Cosmos DB SQL API, inclusief CRUD-bewerkingen niet vinden.
keywords: NoSQL-voorbeeld
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: java
ms.assetid: d824d517-903e-4d82-ab0a-09fc3b984c84
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 02/08/2018
ms.author: mimig
ms.openlocfilehash: a46b6e4d64b44c07fe7a2aa63bc49bea59ad5b53
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2018
---
# <a name="azure-cosmos-db-java-examples-for-the-sql-api"></a>Azure Cosmos DB: Java-voorbeelden voor de SQL-API

> [!div class="op_single_selector"]
> * [.NET-voorbeelden](sql-api-dotnet-samples.md)
> * [Java-voorbeelden](sql-api-java-samples.md)
> * [Node.js-voorbeelden](sql-api-nodejs-samples.md)
> * [Python-voorbeelden](sql-api-python-samples.md)
> * [Voorbeeld van Azure Codegalerie](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

De meest recente voorbeeldtoepassingen uitvoeren CRUD-bewerkingen en andere veelvoorkomende bewerkingen op Azure DB die Cosmos-bronnen zijn opgenomen in de [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java) GitHub-opslagplaats. Dit artikel bevat:

* Koppelingen naar de taken in elk van de voorbeeld-Java-projectbestanden. 
* Koppelingen naar de bijbehorende API verwijst naar inhoud.

**Vereisten**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- U kunt [voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): uw Visual Studio-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U moet het volgende dit voorbeeldtoepassing uit te voeren:

* Java Development Kit 7
* Microsoft Azure DocumentDB Java SDK

U kunt eventueel Maven gebruiken om op te halen van de meest recente Microsoft Azure DocumentDB Java SDK binaire bestanden voor gebruik in uw project. Maven automatisch eventuele vereiste afhankelijkheden toegevoegd. Anders kunt u rechtstreeks de afhankelijkheden die worden vermeld in het bestand pom.xml downloaden en toe te voegen aan uw build-pad.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-documentdb</artifactId>
    <version>LATEST</version>
</dependency>
```

**De voorbeeldtoepassingen uitvoeren**

Kloon de opslagplaats voorbeeld:
```bash
$ git clone https://github.com/Azure/azure-documentdb-java.git

$ cd azure-documentdb-java
```

U kunt de voorbeelden uitvoeren ofwel met behulp van Eclipse of vanaf de opdrachtregel met behulp van Maven.

Vanaf Eclipse uitvoeren:
* Laden van de belangrijkste bovenliggende pom.xml projectbestand in Eclipse; het documentdb-voorbeelden moet automatisch worden geladen.
* Voor het uitvoeren van de voorbeelden, moet u een geldige Azure Cosmos DB-eindpunt. De eindpunten worden gelezen uit `src/test/java/com/microsoft/azure/documentdb/examples/AccountCredentials.java`.
* U kunt de referenties van uw eindpunt als VM-argumenten in Eclipse JUnit uitvoeren Config doorgeven of kunt u de referenties van uw eindpunt in AccountCredentials.java plaatsen.
    ```bash
    -DACCOUNT_HOST="https://REPLACE_THIS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS"
    ```
* U kunt nu de voorbeelden uitvoeren als de tests JUnit in Eclipse.

Uitvoeren vanaf de opdrachtregel:
* De andere manier voor het uitvoeren van voorbeelden is maven gebruiken:
* Voer Maven en de referenties van uw Azure Cosmos DB eindpunt doorgeven:
    ```bash
    mvn test -DACCOUNT_HOST="https://REPLACE_THIS_WITH_YOURS.documents.azure.com:443/" -DACCOUNT_KEY="REPLACE_THIS_WITH_YOURS"
    ```

   > [!NOTE]
   > Elk voorbeeld is ingesloten; het zelf wordt ingesteld en opgeschoond. De voorbeelden geven meerdere aanroepen naar [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection). Telkens wanneer die dit wordt gedaan, wordt uw abonnement gefactureerd voor gebruik voor de prestatielaag van de verzameling gemaakt 1 uur. 
   > 
   > 

## <a name="database-examples"></a>Database-voorbeelden
De [DatabaseCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Maken en een database te lezen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L64-L79) | [DocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdatabase)<br>[DocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdatabase)<br>[Resource.setId](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._resource.setid) |
| [Maken en verwijderen van een database](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L82-L93) | [DocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletedatabase) |
| [Maken en een database](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DatabaseCrudSamples.java#L96-L111) | [DocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydatabases) |

## <a name="collection-examples"></a>Voorbeelden van de verzameling
De [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling van één partitie maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L74-L84) | [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection) |
| [Een aangepaste verzameling van meerdere partitie maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L103-L155) | [Documentcollection maakt](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_collection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._partition_key_definition)<br>[RequestOptions](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._request_options) |
| [Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L97-L99) | [DocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.deletecollection) |

## <a name="document-examples"></a>Voorbeelden van document
De [DocumentCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Maken, lezen en een document verwijderen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L84-L122) | [DocumentClient.createDocument](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createdocument)<br>[DocumentClient.readDocument](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.readdocument)<br>[DocumentClient.deleteDocument](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) |
| [Een document met de documentdefinitie van een programmeerbare maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentCrudSamples.java#L126-L147) | [Document](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document)<br>[Resource.setId](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._resource.setid) |

## <a name="indexing-examples"></a>Indexeren van voorbeelden
De [CollectionCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een index en een set indexeren beleid maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/CollectionCrudSamples.java#L125-L141) | [Index](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._index)<br>[IndexingPolicy](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._indexing_policy) |

Zie voor meer informatie over het indexeren [Azure Cosmos DB indexeren beleid](indexing-policies.md).

## <a name="query-examples"></a>Query-voorbeelden
De [DocumentQuerySamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een eenvoudig cross-partitie document-query uitvoeren](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L108-L129) | [DocumentClient.queryDocuments](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.querydocuments)<br>[FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._feed_options.setenablecrosspartitionquery) |
| [Order by, query](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/DocumentQuerySamples.java#L132-L154) | [FeedResponse<T>.getQueryIterator](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._feed_response.getqueryiterator) |

Zie voor meer informatie over het schrijven van query's [SQL-query in Azure Cosmos DB](sql-api-sql-query.md).

## <a name="offer-examples"></a>Voorbeelden van aanbieding
De [OfferCrudSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Maken van een verzameling en doorvoer](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L76-L102) | [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection)<br>[RequestOptions.setOfferThroughput ](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._request_options.setofferthroughput) |
| [Een verzameling om te zoeken naar de bijbehorende aanbieding lezen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java#L108-L132) | [Offer.getContent](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._offer.getcontent)<br>[DocumentClient.replaceOffer](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.replaceoffer)<br>[DocumentClient.readCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.readcollection)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.queryoffers) |

## <a name="partition-key-examples"></a>De belangrijkste voorbeelden partitie
De [SinglePartitionCollectionDocumentCrudSample](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling van één partitie maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L164-L207) | [DocumentClient.createCollection](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createcollection) |
| [De doorvoer-aanbieding voor een verzameling van één partitie wijzigen](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/SinglePartitionCollectionDocumentCrudSample.java#L209-L223) | [DocumentClient.replaceOffer](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.replaceoffer) |

## <a name="stored-procedure-examples"></a>Voorbeelden van de opgeslagen procedure
De [StoredProcedureSamples](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java) bestand ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een opgeslagen procedure maken](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L85-L118) | [StoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._stored_procedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.createstoredprocedure) |
| [Voer een opgeslagen procedure met argumenten](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L121-L144) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.executestoredprocedure) |
| [Een opgeslagen procedure uitvoeren met een argument van het object](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/StoredProcedureSamples.java#L147-L177) | [DocumentClient.executeStoredProcedure](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.documentdb._document_client.executestoredprocedure) |
