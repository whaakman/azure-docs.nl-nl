---
title: Voorbeelden voor Azure Cosmos DB SQL API Python | Microsoft Docs
description: Python-voorbeelden op github voor algemene taken in Azure Cosmos DB, inclusief CRUD-bewerkingen niet vinden.
keywords: Python-voorbeelden
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: python
ms.assetid: 7f4f8db3-e9db-4645-92ef-7819d486a349
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 03/14/2018
ms.author: mimig
ms.openlocfilehash: 869c38c1f3dbdb21c3ecc21141341b1ae9dbaad6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-cosmos-db-python-examples"></a>Azure Cosmos DB Python-voorbeelden

> [!div class="op_single_selector"]
> * [.NET-voorbeelden](sql-api-dotnet-samples.md)
> * [Java-voorbeelden](sql-api-java-samples.md)
> * [Node.js-voorbeelden](sql-api-nodejs-samples.md)
> * [Python-voorbeelden](sql-api-python-samples.md)
> * [Voorbeeld van Azure Codegalerie](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Voorbeeldoplossingen die worden uitgevoerd CRUD-bewerkingen en andere veelvoorkomende bewerkingen op Azure DB die Cosmos-bronnen zijn opgenomen in de [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python) GitHub-opslagplaats. Dit artikel bevat:

* Koppelingen naar de taken in elk van de projectbestanden voor Python-voorbeeld. 
* Koppelingen naar de bijbehorende API verwijst naar inhoud.

**Vereisten**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- U kunt [voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): uw Visual Studio-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U moet ook de [Python SDK](sql-api-sdk-python.md). 
   
   > [!NOTE]
   > Elk voorbeeld is ingesloten; het zelf wordt ingesteld en opgeschoond. De voorbeelden geven meerdere aanroepen naar [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Telkens wanneer die dit wordt gedaan, wordt uw abonnement gefactureerd voor gebruik van één uur. Zie voor meer informatie over Azure Cosmos DB facturering [prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).
   > 
   > 

## <a name="database-examples"></a>Database-voorbeelden
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py) -bestand van de [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement) project ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een database maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client.CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateDatabase) |
| [Lezen van een database met ID](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client.ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#ReadDatabase) |
| [Lijst met databases voor een account](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client.ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Een database verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client.DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |

## <a name="collection-examples"></a>Voorbeelden van de verzameling
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py) -bestand van de [CollectionManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement) project ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client.CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Een lijst van alle verzamelingen in een database lezen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L209) |[document_client.ReadCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#ReadCollections) |
| [Een verzameling door-ID ophalen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client.ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#ReadCollection) |
| [De doorvoer van een verzameling wijzigen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L168-L172) | [document_client.ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)|
| [Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client.DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#DeleteCollection) |

## <a name="document-examples"></a>Voorbeelden van document
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py) -bestand van de [DocumentManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement) project ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Een document maken](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[document_client.CreateDocument](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateDocument) |
| [Maken van een verzameling van documenten](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[document_client.CreateDocuments](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateDocuments) |
| [Een document door ID lezen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L69-L78) |[document_client.ReadDocument](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#ReadDocument) |
| [Lees de documenten in een verzameling](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L81-L92) |[document_client.ReadDocuments](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#ReadDocuments) |

## <a name="indexing-examples"></a>Indexeren van voorbeelden
De [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py) -bestand van de [IndexManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement) project ziet u hoe u de volgende taken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Gebruik indexeren handmatig (in plaats van automatisch)](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L245-L246) |[IndexingPolicy.Automatic](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Opgegeven document paden uitsluiten van de index](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L294-L367) |[IndexingPolicy.ExcludedPaths](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Uitsluiten van een document van de index](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L204-L210) |[documents.IndexingDirective.Exclude](http://azure.github.io/azure-documentdb-python/_modules/pydocumentdb/documents.html#IndexingDirective) |
| [Modus instellen voor indexering](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L533) |[documenten. IndexingMode](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.documents.html#IndexingMode) |
| [Gebruik bereik indexen op tekenreeksen](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L440-L456) |[IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Uitvoeren van een index transformatie](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L514-L559) |[document_client.ReplaceCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#ReplaceCollection) |

## <a name="query-examples"></a>Query-voorbeelden
De voorbeeldprojecten ook ziet u hoe de volgende querytaken uitvoeren:

| Taak | API-verwijzing |
| --- | --- |
| [Query uitvoeren op een account voor een database](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client.QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Query voor documenten](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L152-L169) |[document_client.QueryDocuments](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#QueryDocuments) |
| [Afdwingen van een scanbewerking bereik op een geïndexeerde hash-pad](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L409-L415) |[http_constants.HttpHeaders.EnableScanInQuery](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.http_constants.html#pydocumentdb.http_constants.HttpHeaders.EnableScanInQuery) |
