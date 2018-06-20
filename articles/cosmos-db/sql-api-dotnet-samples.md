---
title: 'Azure Cosmos DB: .NET-voorbeelden voor de SQL API | Microsoft Docs'
description: Vind C# .NET-voorbeelden op github voor veelvoorkomende taken met de SQL API in Azure Cosmos DB, zoals CRUD-bewerkingen.
keywords: Voorbeeld van NoSQL
services: cosmos-db
author: rnagpal
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: sample
ms.date: 02/22/2017
ms.author: rnagpal
ms.openlocfilehash: d5b57f0bb7a9d539be821c2f93cddfb8c7e29f5f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831866"
---
# <a name="azure-cosmos-db-net-examples-for-the-sql-api"></a>Azure Cosmos DB: .NET-voorbeelden voor de SQL API
> [!div class="op_single_selector"]
> * [.NET-voorbeelden](sql-api-dotnet-samples.md)
> * [Java-voorbeelden](sql-api-java-samples.md)
> * [Async Java-voorbeelden](sql-api-async-java-samples.md)
> * [Node.js-voorbeelden](sql-api-nodejs-samples.md)
> * [Python-voorbeelden](sql-api-python-samples.md)
> * [Galerie met codevoorbeelden voor Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

De nieuwste voorbeeldoplossingen waarmee CRUD-bewerkingen en andere veelvoorkomende bewerkingen in Azure Cosmos DB-resources worden uitgevoerd, zijn opgenomen in de GitHub-opslagplaats [azure-documentdb-dotnet](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples). Dit artikel bevat:

* Koppelingen naar de taken in elk van de C#-voorbeeldprojectbestanden. 
* Koppelingen naar het bijbehorende API-referentiemateriaal.

**Vereisten**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- U kunt de [voordelen voor Visual Studio-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): via uw Visual Studio-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

U hebt ook het pakket [Microsoft.Azure.DocumentDB NuGet](http://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) nodig. 

> [!NOTE]
> Elk voorbeeld staat op zichzelf. Het stelt zichzelf in en aan het einde worden de gegevens automatisch opgeschoond. Als zodanig, wordt in de voorbeelden CreateDocumentCollectionAsync() meerdere keren aangeroepen. Telkens wanneer dit wordt gedaan, wordt uw abonnement gefactureerd voor 1 uur gebruik per prestatielaag van de verzameling die wordt gemaakt. 
> 
> 

## <a name="database-examples"></a>Voorbeelden voor databases
In de methode [RunDatabaseDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L72-L121) van het voorbeeldproject DatabaseManagement ziet u hoe de volgende taken worden uitgevoerd.

| Taak | API-verwijzing |
| --- | --- |
| [Een database maken](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L90) |[DocumentClient.CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) |
| [Een query in een database uitvoeren](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L81) |[DocumentQueryable.CreateDatabaseQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdatabasequery.aspx) |
| [Een database lezen op id](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L102) |[DocumentClient.ReadDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdatabaseasync.aspx) |
| [Alle databases lezen](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L108-L113) |[DocumentClient.ReadDatabaseFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdatabasefeedasync.aspx) |
| [Een database verwijderen](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L118) |[DocumentClient.DeleteDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.deletedatabaseasync.aspx) |

## <a name="collection-examples"></a>Voorbeelden voor verzamelingen
In de methode [RunCollectionDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/CollectionManagement/Program.cs#L96-L185) van het voorbeeldproject CollectionManagement ziet u hoe de volgende taken worden uitgevoerd.

| Taak | API-verwijzing |
| --- | --- |
| [Een verzameling maken](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L101) |[DocumentClient.CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) |
| [Geconfigureerde prestaties van een verzameling ophalen](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L198) |[DocumentQueryable.CreateOfferQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx) |
| [Geconfigureerde prestaties van een verzameling wijzigen](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L207) |[DocumentClient.ReplaceOfferAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx) |
| [Een verzameling ophalen op id](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L153) |[DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) |
| [Alle verzamelingen in een database lezen](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L162) |[DocumentClient.ReadDocumentCollectionFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdocumentcollectionfeedasync.aspx) |
| [Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L175) |[DocumentClient.DeleteDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.deletedocumentcollectionasync.aspx) |

## <a name="document-examples"></a>Voorbeelden voor documenten
In de methode [RunDocumentsDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L97-L102) van het voorbeeldproject DocumentManagement ziet u hoe de volgende taken worden uitgevoerd.

| Taak | API-verwijzing |
| --- | --- |
| [Een document maken](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L198) |[DocumentClient.CreateDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) |
| [Een document lezen op id](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L211) |[DocumentClient.ReadDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdocumentasync.aspx) |
| [Alle documenten in een verzameling lezen](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L227) |[DocumentClient.ReadDocumentFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdocumentfeedasync.aspx) |
| [Een query uitvoeren voor documenten](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L248-L251) |[DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Een document vervangen](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L263) |[DocumentClient.ReplaceDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replacedocumentasync.aspx) |
| [Een document invoegen of updaten](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L300) |[DocumentClient.UpsertDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.upsertdocumentasync.aspx) |
| [Document verwijderen](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L322) |[DocumentClient.DeleteDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.deletedocumentasync.aspx) |
| [Werken met dynamische .NET-objecten](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L331-L380) |[DocumentClient.CreateDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx)<br>[DocumentClient.ReadDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdocumentasync.aspx)<br>[DocumentClient.ReplaceDocumentAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replacedocumentasync.aspx) |
| [Document vervangen door voorwaardelijke ETag-controle](https://github.com/Azure/azure-documentdb-dotnet/blob/f2b11dec45a195ddeed333560ebba63055f5ed09/samples/code-samples/DocumentManagement/Program.cs#L398-L440) |[DocumentClient.AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx)<br>[Documents.Client.AccessConditionType](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accessconditiontype.aspx) |
| [Document alleen lezen als het is gewijzigd](https://github.com/Azure/azure-documentdb-dotnet/blob/f2b11dec45a195ddeed333560ebba63055f5ed09/samples/code-samples/DocumentManagement/Program.cs#L442-L470) |[DocumentClient.AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx)<br>[Documents.Client.AccessConditionType](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accessconditiontype.aspx) |

## <a name="indexing-examples"></a>Voorbeelden van indexen
In de methode [RunIndexDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/ea8c977b9c2f37ddc2894911ec239907ab60e40a/samples/code-samples/IndexManagement/Program.cs#L89-L117) van het voorbeeldproject IndexManagement ziet u hoe de volgende taken worden uitgevoerd.

| Taak | API-verwijzing |
| --- | --- |
| [Een document uitsluiten van de index](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L125-L163) |[IndexingDirective.Exclude](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexingdirective.aspx) |
| [Handmatige (in plaats van automatische) indexering gebruiken](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L171-L209) |[IndexingPolicy.Automatic](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexingpolicy.automatic.aspx) |
| [Vertraagde indexering (in plaats van consistente) gebruiken](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L221-L238) |[IndexingMode.Lazy](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexingpolicy.indexingmode.aspx#P:Microsoft.Azure.Documents.IndexingPolicy.IndexingMode) |
| [Opgegeven documentpaden uitsluiten van de index ](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L248-L297) |[IndexingPolicy.ExcludedPaths](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexingpolicy.excludedpaths.aspx) |
| [Een bereikscanbewerking voor een met hash geïndexeerd pad afdwingen](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L305-L340) |[FeedOptions.EnableScanInQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx) |
| [Bereikindexen voor tekenreeksen gebruiken](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L342-L405) |[IndexingPolicy.IncludedPaths](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexingpolicy.includedpaths.aspx)<br>[RangeIndex](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.rangeindex.aspx) |
| [Een indextransformatie uitvoeren](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L407-L464) |[ReplaceDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replacedocumentcollectionasync.aspx) |

Zie [Azure Cosmos DB-indexeringsbeleid](indexing-policies.md) voor meer informatie over indexering.

## <a name="geospatial-examples"></a>Georuimtelijke voorbeelden
In het bestand met georuimtelijke voorbeelden [azure-documentdb-dotnet/samples/code-samples/Geospatial/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) ziet u hoe de volgende taken worden uitgevoerd.  

| Taak | API-verwijzing |
| --- | --- |
| [Georuimtelijke indexering op een nieuwe verzameling inschakelen](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L45-L63) |[IndexingPolicy](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexingpolicy.aspx) <br> [IndexKind.Spatial](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.indexkind.aspx) <br>[DataType.Point](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.datatype.aspx) |
| [Documenten met GeoJSON-punten invoegen](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L116-L126) |[DocumentClient.CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync) </br> [DataType.Point](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.datatype.aspx) |
| [Punten zoeken binnen een opgegeven afstand](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L152-L194) |[ST_DISTANCE](sql-api-sql-query.md#BuiltinFunctions) </br> [GeometryOperationExtensions.Distance](/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.distance) |
| [Punten in een polygoon vinden](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L196-L221) |[ST_WITHIN](sql-api-sql-query.md#BuiltinFunctions) </br> [GeometryOperationExtensions.Within](/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.distance) en </br>[Polygoon](/dotnet/api/microsoft.azure.documents.spatial.polygon) |
| [Georuimtelijke indexering op een bestaande verzameling inschakelen](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L312-L336) |[DocumentClient.ReplaceDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replacedocumentcollectionasync.aspx)<br>[DocumentCollection.IndexingPolicy](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.documentcollection.indexingpolicy.aspx#P:Microsoft.Azure.Documents.DocumentCollection.IndexingPolicy) |
| [Point- en polygoongegevens valideren](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L223-L265) |[ST_ISVALID](sql-api-sql-query.md#BuiltinFunctions)<br>[ST_ISVALIDDETAILED](sql-api-sql-query.md#BuiltinFunctions)<br>[GeometryOperationExtensions.IsValid](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.spatial.geometryoperationextensions.isvalid.aspx)<br>[GeometryOperationExtensions.IsValidDetailed](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.spatial.geometryoperationextensions.isvaliddetailed.aspx) |

Zie [Werken met georuimtelijke gegevens in Azure Cosmos DB](geospatial.md) voor meer informatie over het werken met georuimtelijke gegevens.  

## <a name="query-examples"></a>Voorbeelden van query's
In het querydocumentbestand [azure-documentdb-dotnet/samples/code-samples/Queries/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) ziet u hoe u elk van de volgende taken wordt uitgevoerd met behulp van de SQL-query-grammatica, de LINQ-provider met query, en met Lambda.

| Taak | API-verwijzing |
| --- | --- |
| [Een query uitvoeren voor alle documenten](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L122-L138) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren voor gelijkheid met behulp van ==](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L251-L268) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren voor ongelijkheid met behulp van != en NOT](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L270-L276) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met behulp van bereikoperators zoals >, <, >=, <=](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L305-L325) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met behulp van bereikoperators op tekenreeksen](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L337-L346) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met Sorteren op](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L370-L392) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met statistische functies](https://github.com/arramac/azure-documentdb-dotnet/blob/198bed2865e54af6681fc96b3ca253d31b113b9a/samples/code-samples/Queries/Program.cs#L451-L455) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Werken met subdocumenten](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L394-L419) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met Joins tussen documenten](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L421-L435) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met de tekenreeks-, berekening- en matrixoperators](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L527-L552) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met SQL met parameters met behulp van SqlQuerySpec](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L140-L174) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx)<br>[SqlQuerySpec](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.sqlqueryspec.aspx) |
| [Query uitvoeren met expliciete paginering](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L554-L576) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query gelijktijdig uitvoeren op gepartitioneerde verzamelingen](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs#L664-L734) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [Query uitvoeren met Sorteren op voor gepartitioneerde verzamelingen](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs#L737-L810) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |

Zie [SQL-query in Azure Cosmos DB](sql-api-sql-query.md) voor meer informatie over het schrijven van query's.

## <a name="change-feed-examples"></a>Voorbeelden van wijzigingenfeeds 
In het voorbeeld van wijzigingenfeeds [azure-documentdb-dotnet/samples/code-samples/ChangeFeed/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ServerSideScripts/Program.cs) ziet u hoe de volgende taken worden uitgevoerd. 

| Taak | API-verwijzing |
| --- | --- |
| [Wijzigingenfeed lezen](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ChangeFeed/Program.cs#L132) |[DocumentClient.CreateDocumentChangeFeedQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) | 
| [Partitiesleutelbereiken lezen](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ChangeFeed/Program.cs#L118) |[DocumentClient.ReadPartitionKeyRangeFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync.aspx) | 

In het voorbeeld van wijzigingenfeedprocessor: [Migratiehulpmiddel wijzigingenfeeds](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedMigrationTool) ziet u hoe u de wijzigingenfeed-processorbibliotheek gebruikt om gegevens te repliceren naar een andere Cosmos DB-verzameling.   

## <a name="server-side-programming-examples"></a>Voorbeelden van programmering op de server
In het bestand voor programmering op de server[azure-documentdb-dotnet/samples/code-samples/ServerSideScripts/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ServerSideScripts/Program.cs), ziet u hoe de volgende taken worden uitgevoerd.

| Taak | API-verwijzing |
| --- | --- |
| [Een opgeslagen procedure maken](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L112) |[DocumentClient.CreateStoredProcedureAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createstoredprocedureasync.aspx) |
| [Een opgeslagen procedure uitvoeren](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L127) |[DocumentClient.ExecuteStoredProcedureAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.executestoredprocedureasync.aspx) |
| [Een documentfeed voor een opgeslagen procedure document lezen](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L194) |[DocumentClient.ReadDocumentFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readdocumentfeedasync.aspx) |
| [Een opgeslagen procedure maken met Sorteren op](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L219) |[DocumentClient.CreateStoredProcedureAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createstoredprocedureasync.aspx) |
| [Een voorafgaande trigger maken](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L264) |[DocumentClient.CreateTriggerAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createtriggerasync.aspx) |
| [Een navolgende trigger maken](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L329) |[DocumentClient.CreateTriggerAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createtriggerasync.aspx) |
| [Een door gebruiker gedefinieerde functie (UDF) maken](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L389) |[DocumentClient.CreateUserDefinedFunctionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createuserdefinedfunctionasync.aspx) |

Zie voor meer informatie over het programmeren op de server [Programmeren op de server met Azure Cosmos DB: opgeslagen procedures, databasetriggers en UDF's](programming.md).

## <a name="user-management-examples"></a>Voorbeelden van gebruikersbeheer
In het gebruikersbeheerbestand [azure-documentdb-dotnet/samples/code-samples/UserManagement/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/UserManagement/Program.cs) ziet u hoe de volgende taken worden uitgevoerd.

| Taak | API-verwijzing |
| --- | --- |
| [Een gebruiker maken](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L81) |[DocumentClient.CreateUserAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createuserasync.aspx) |
| [Machtigingen instellen voor een verzameling of document](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L85) |[DocumentClient.CreatePermissionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createpermissionasync.aspx) |
| [Een lijst met machtigingen van een gebruiker ophalen](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L218) |[DocumentClient.ReadUserAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readuserasync.aspx)<br>[DocumentClient.ReadPermissionFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readpermissionfeedasync.aspx) |

