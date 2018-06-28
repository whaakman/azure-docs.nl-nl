---
title: Voorbeelden van SQL API Python voor Azure Cosmos DB | Microsoft Docs
description: Vind Python-voorbeelden op github voor veelvoorkomende taken in Azure Cosmos DB, zoals CRUD-bewerkingen.
keywords: python-voorbeelden
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: python
ms.topic: sample
ms.date: 03/14/2018
ms.author: sngun
ms.openlocfilehash: 5effc20f421e448d6d27841d2d3f02c0220af8c8
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751023"
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

- U kunt de [voordelen voor Visual Studio-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): via uw Visual Studio-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U hebt ook de [Python SDK](sql-api-sdk-python.md) nodig. 
   
   > [!NOTE]
   > Elk voorbeeld staat op zichzelf. Het stelt zichzelf in en aan het einde worden de gegevens automatisch opgeschoond. In de voorbeelden wordt [document_client.CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) meerdere keren aangeroepen. Steeds wanneer dit wordt gedaan, wordt uw abonnement gefactureerd voor het gebruik voor één uur. Zie [Azure Cosmos DB-prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie over Azure Cosmos DB-facturering.
   > 
   > 

## <a name="database-examples"></a>Voorbeelden voor databases
In het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py) van het project [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement) ziet u hoe de volgende taken worden uitgevoerd:

| Taak | API-verwijzing |
| --- | --- |
| [Een database maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client.CreateDatabase](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#createdatabase) |
| [Een database lezen op id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client.ReadDatabase](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdatabase) |
| [Databases voor een account weergeven](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client.ReadDatabases](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdatabases) |
| [Een database verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client.DeleteDatabase](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#deletedatabase) |

## <a name="collection-examples"></a>Voorbeelden voor verzamelingen
In het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py) van het project [CollectionManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement) ziet u hoe de volgende taken worden uitgevoerd:

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client.CreateCollection](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#createcollection) |
| [Een lijst van alle verzamelingen in een database lezen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L209) |[document_client.ReadCollections](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readcollections) |
| [Een verzameling ophalen op id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client.ReadCollection](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readcollection) |
| [De doorvoer van een verzameling wijzigen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L168-L172) | [document_client.ReplaceOffer](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#replaceoffer)|
| [Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client.DeleteCollection](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#deletecollection) |

## <a name="document-examples"></a>Voorbeelden voor documenten
In het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py) van het project [DocumentManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement) ziet u hoe de volgende taken worden uitgevoerd:

| Taak | API-verwijzing |
| --- | --- |
| [Een document maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[document_client.CreateDocument](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#createdocument) |
| [Een verzameling van documenten maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[document_client.CreateDocuments](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python) |
| [Een document lezen op id](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L69-L78) |[document_client.ReadDocument](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdocument) |
| [Alle documenten in een verzameling lezen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L81-L92) |[document_client.ReadDocuments](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdocuments) |

## <a name="indexing-examples"></a>Voorbeelden van indexen
In het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py) van het project [IndexManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement) ziet u hoe de volgende taken worden uitgevoerd:

| Taak | API-verwijzing |
| --- | --- |
| [Handmatige (in plaats van automatische) indexering gebruiken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L245-L246) |[IndexingPolicy.Automatic](https://docs.microsoft.com/en-us/python/api/pydocumentdb.documents?view=azure-python) |
| [Opgegeven documentpaden uitsluiten van de index ](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L294-L367) |[IndexingPolicy.ExcludedPaths](https://docs.microsoft.com/en-us/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Een document uitsluiten van de index](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L204-L210) |[documents.IndexingDirective.Exclude](https://docs.microsoft.com/en-us/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Indexeringsmodus instellen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L533) |[documents.IndexingMode](https://docs.microsoft.com/en-us/python/api/pydocumentdb.documents.indexingmode?view=azure-python) |
| [Bereikindexen voor tekenreeksen gebruiken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L440-L456) |[IndexingPolicy.IncludedPaths](https://docs.microsoft.com/en-us/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Een indextransformatie uitvoeren](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L514-L559) |[document_client.ReplaceCollection](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#replacecollection) |

## <a name="query-examples"></a>Voorbeelden van query's
In de voorbeeldprojecten ziet u ook hoe de volgende querytaken worden uitgevoerd:

| Taak | API-verwijzing |
| --- | --- |
| [Een query uitvoeren op een account voor een database](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client.QueryDatabases](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#querydatabases) |
| [Een query uitvoeren voor documenten](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L152-L169) |[document_client.QueryDocuments](https://docs.microsoft.com/en-us/python/api/pydocumentdb.document_client.documentclient?view=azure-python#querydocuments) |
| [Een bereikscanbewerking voor een met hash geïndexeerd pad afdwingen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L409-L415) |[http_constants.HttpHeaders.EnableScanInQuery](https://docs.microsoft.com/en-us/python/api/pydocumentdb.http_constants.httpheaders?view=azure-python#enablescaninquery) |
