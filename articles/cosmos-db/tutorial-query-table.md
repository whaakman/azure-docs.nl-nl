---
title: Tabelgegevens opvragen in Azure Cosmos DB
description: Tabelgegevens opvragen in Azure Cosmos DB
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 05/21/2019
ms.reviewer: sngun
ms.openlocfilehash: 161b424c5c89d34eaa55181c0d6ca0515b376168
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978772"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-table-api"></a>Zelfstudie: query's uitvoeren in Azure Cosmos DB met behulp van de Table-API

De [tabel-API](table-introduction.md) van Azure Cosmos DB ondersteunt de uitvoering van OData- en [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service)-query's op gegevens van de sleutelwaarde (tabel).  

Dit artikel behandelt de volgende taken: 

> [!div class="checklist"]
> * Gegevens opvragen met de tabel-API

In de query's in dit artikel wordt de volgende voorbeeldtabel `People` gebruikt:

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Zie [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) (Tabellen en entiteiten opvragen) voor meer informatie over het uitvoeren van query's met behulp van de tabel-API. 

Meer informatie over de premium-mogelijkheden van Azure Cosmos DB vindt u in [De tabel-API van Azure Cosmos DB](table-introduction.md) en [Ontwikkelen met de tabel-API in .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Vereisten

Deze query's werken alleen als u een Azure DB Cosmos DB-account hebt en een container met entiteitsgegevens. Hebt u geen van beide? Voltooi de [Quickstart van 5 minuten](create-table-dotnet.md) of de [zelfstudie voor ontwikkelaars](tutorial-develop-table-dotnet.md) om een account te maken en uw database te vullen.

## <a name="query-on-partitionkey-and-rowkey"></a>Query op PartitionKey en RowKey
Omdat de eigenschappen PartitionKey en RowKey de primaire sleutel van een entiteit vormen, kunt u de volgende specifieke syntaxis gebruiken voor het identificeren van de entiteit: 

**Query**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Results**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

U kunt deze eigenschappen ook opgeven als onderdeel van de optie `$filter`, zoals wordt weergegeven in de volgende sectie. Houd er rekening mee dat de namen van sleuteleigenschappen en constante waarden hoofdlettergevoelig zijn. De eigenschappen PartitionKey en RowKey zijn van het type String. 

## <a name="query-by-using-an-odata-filter"></a>Query's uitvoeren met behulp van een OData-filter
Houd bij het samenstellen van een filtertekenreeks rekening met deze regels: 

* Gebruik de logische operators die door de specificatie van het OData-protocol zijn gedefinieerd om een eigenschap te vergelijken met een waarde. U kunt een eigenschap niet vergelijken met een dynamische waarde. Eén kant van de expressie moet een constante zijn. 
* De eigenschapsnaam, operator en constante waarde moeten worden gescheiden door in URL gecodeerde spaties. Een spatie wordt in URL gecodeerd als `%20`. 
* Alle onderdelen van de filtertekenreeks zijn hoofdlettergevoelig. 
* Het filter retourneert alleen geldige resultaten als de constante waarde van hetzelfde gegevenstype is als de eigenschap. Zie [Understanding the Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model) (Het gegevensmodel van de tabelservice) voor meer informatie over ondersteunde eigenschapstypen. 

Hier volgt een voorbeeldquery die laat zien hoe u de eigenschappen PartitionKey en Email filtert met behulp van een OData-`$filter`.

**Query**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Zie [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities) (Tabellen en entiteiten opvragen) voor meer informatie over het samenstellen van filterexpressies voor verschillende soorten gegevens.

**Results**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Smith |Ben | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Query’s uitvoeren met LINQ 
U kunt ook query's uitvoeren met behulp van LINQ, die deze vertaalt naar de bijbehorende OData-query-expressies. Hier volgt een voorbeeld van hoe u query's opbouwt met behulp van de .NET-SDK:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("People");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * U hebt geleerd hoe u een query maakt met behulp van de tabel-API

U kunt nu doorgaan met de volgende zelfstudie, waarin u leert hoe u uw gegevens globaal distribueert.

> [!div class="nextstepaction"]
> [Uw gegevens globaal distribueren](tutorial-global-distribution-table.md)
