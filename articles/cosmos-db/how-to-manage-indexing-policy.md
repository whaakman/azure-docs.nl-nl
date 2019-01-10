---
title: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
description: Informatie over het beheren van databaseaccounts in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: c27cee4842c0e65e1737f100a215cff82a0fd439
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033091"
---
# <a name="manage-indexing-in-azure-cosmos-db"></a>Indexering beheren in Azure Cosmos DB

Indexeren Azure Cosmos DB kunt u kiezen of u wilt dat alle items in een container automatisch moeten worden geïndexeerd of niet. Standaard worden alle items in een Azure Cosmos-container automatisch geïndexeerd, maar u kunt automatisch indexeren uitschakelen. Als indexeren is uitgeschakeld, kunnen items worden geopend via de bijbehorende interne koppelingen of via de query's met behulp van de item-id, zoals de document-id. U kunt er expliciet om vragen om de resultaten te beheren zonder de index te gebruiken, door de header **x-ms-documentdb-enable-scan**  in REST API door te geven, of via de aanvraagoptie **EnableScanInQuery** met behulp van de .NET SDK.

Als automatisch indexeren is uitgeschakeld, kunt u nog steeds selectief bepaalde items toevoegen aan de index. Daarentegen kunt u automatisch indexeren ingeschakeld laten en er selectief voor kiezen om bepaalde items uit te sluiten. Configuratie voor indexeren aan/uit zijn nuttig wanneer u een subset items hebt die moeten worden doorzocht.  

Schrijfdoorvoer en aanvraageenheden zijn in verhouding met het aantal waarden dat moet worden geïndexeerd, wat wordt opgegeven via de opgenomen set in het indexeringsbeleid. Als u een goed begrip hebt van querypatronen, kunt u expliciet de subset paden kiezen die moeten worden opgenomen/uitgesloten om de schrijfdoorvoer te verbeteren.

## <a name="manage-indexing-using-azure-portal"></a>Indexeren beheren met behulp van de Azure-portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Maak een nieuw Azure Cosmos DB-account of selecteer een bestaand account.

3. Open het deelvenster **Data Explorer**.

4. Selecteer een bestaande container, vouw deze uit en wijzig de volgende waarden:

   * Open het venster **Schaal en instellingen**.
   * Wijzig **indexingMode** van *consistent* in *geen*. U kunt ook bepaalde paden opnemen/uitsluiten voor indexeren.
   * Klik op **OK** om de wijzigingen op te slaan.

   ![Indexeren beheren met behulp van de Azure-portal](./media/how-to-manage-indexing/how-to-manage-indexing-portal.png)

## <a name="manage-indexing-using-azure-sdks"></a>Indexeren beheren met behulp van Azure-SDK's

### <a id="dotnet"></a>.NET SDK

In het volgende voorbeeld ziet u hoe u een item expliciet kunt opnemen door de [SQL API .NET SDK](sql-api-sdk-dotnet.md) en de eigenschap [RequestOptions.IndexingDirective](/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective) te gebruiken.

```csharp
// To override the default behavior to exclude (or include) a document in indexing,
// use the RequestOptions.IndexingDirective property.
client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new { id = "myDocumentId", isRegistered = true },
    new RequestOptions { IndexingDirective = IndexingDirective.Include });
```

## <a name="next-steps"></a>Volgende stappen

Lees meer over indexeren in de volgende artikelen:

* [Overzicht van indexeren](index-overview.md)
* [Indexeringsbeleid](index-policy.md)
* [Indextypen](index-types.md)
* [Indexpaden](index-paths.md)
