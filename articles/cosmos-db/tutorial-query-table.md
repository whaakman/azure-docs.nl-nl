---
title: Hoe kan ik een query over tabelgegevens in Azure Cosmos DB? | Microsoft Docs
description: Informatie over het query-tabelgegevens in Azure Cosmos-DB
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.openlocfilehash: e59cfa85c6bf584e44bdc6e88cc19d67df390041
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api-preview"></a>Azure Cosmos DB: Hoe een query over tabelgegevens met behulp van de tabel-API (preview)?

De Azure DB die Cosmos [tabel API](table-introduction.md) (preview) biedt ondersteuning voor OData en [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) een query uitgevoerd op gegevens van de sleutelwaarde (tabel).  

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Een query met de tabel-API

De query's in dit artikel gebruik het volgende voorbeeld `People` tabel:

| PartitionKey | RowKey | E-mail | Telefoonnummer |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Omdat Azure Cosmos DB compatibel met de Azure Table storage-API's is, Zie [opvragen van tabellen en entiteiten] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) voor meer informatie over de query met behulp van de tabel API. 

Zie voor meer informatie over de premium-mogelijkheden die Azure Cosmos DB biedt [Azure Cosmos DB: tabel API](table-introduction.md) en [ontwikkelen met de API van de tabel in .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Vereisten

Voor deze query's werken, moet u een Azure DB die Cosmos-account hebt en entiteitsgegevens in de container hebt. Geen van deze? Voltooi de [vijf minuten Quick Start](https://aka.ms/acdbtnetqs) of de [developer-zelfstudie](https://aka.ms/acdbtabletut) voor het maken van een account en vul uw database.

## <a name="query-on-partitionkey-and-rowkey"></a>Query op PartitionKey en RowKey
Omdat de eigenschappen PartitionKey en RowKey primaire sleutel van een entiteit vormen, kunt u de volgende specifieke syntaxis voor het identificeren van de entiteit: 

**Query**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Resultaten**

| PartitionKey | RowKey | E-mail | Telefoonnummer |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

U kunt deze eigenschappen ook opgeven als onderdeel van de `$filter` optie, zoals wordt weergegeven in de volgende sectie. Houd er rekening mee dat de namen van de sleuteleigenschap en constante waarden hoofdlettergevoelig zijn. De PartitionKey en de RowKey eigenschappen zijn van het type String. 

## <a name="query-by-using-an-odata-filter"></a>Query uitvoeren met behulp van een OData-filter
Wanneer u een filtertekenreeks construeren bent, houd er rekening mee deze regels: 

* De logische operators gedefinieerd door de specificatie van de OData-Protocol gebruiken om te vergelijken van een eigenschap een waarde. Houd er rekening mee dat u een eigenschap aan een dynamische waarde kan niet worden vergeleken. Een-zijde van de expressie moet een constante zijn. 
* De eigenschapsnaam, een operator en een constante waarde moeten worden gescheiden door spaties URL-codering. Een spatie is het URL-codering als `%20`. 
* Alle onderdelen van de filtertekenreeks zijn hoofdlettergevoelig. 
* De constante waarde moet van hetzelfde gegevenstype als de eigenschap om het filter geldige resultaten retourneren. Zie voor meer informatie over ondersteunde eigenschaptypen [inzicht in de tabel Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Hier volgt een voorbeeldquery die laat hoe u de eigenschappen PartitionKey en e-mail filteren zien met behulp van een OData `$filter`.

**Query**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Zie voor meer informatie over het samenstellen van Filterexpressies voor verschillende soorten gegevens [opvragen van tabellen en entiteiten](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Resultaten**

| PartitionKey | RowKey | E-mail | Telefoonnummer |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Query uitvoeren met behulp van LINQ 
U kunt ook een query met behulp van LINQ, die wordt vertaald naar de bijbehorende OData-query-expressies. Hier volgt een voorbeeld van hoe u query's opbouwen met behulp van de .NET SDK:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Hebt geleerd hoe u een query met behulp van de tabel-API (preview) 

U kunt nu doorgaan met de volgende zelfstudie voor informatie over het distribueren van uw gegevens globaal.

> [!div class="nextstepaction"]
> [Uw gegevens globaal distribueren](tutorial-global-distribution-documentdb.md)
