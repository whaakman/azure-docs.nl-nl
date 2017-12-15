---
title: Hoe query SQL in Azure Cosmos DB? | Microsoft Docs
description: Informatie over query's uitvoeren met SQL in Azure Cosmos-DB
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.openlocfilehash: ffef6ec2120a80d907449470efb7b4ab6dca8037
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a>Azure Cosmos DB: Hoe kan ik query uitvoert met behulp van SQL?

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

De Azure DB die Cosmos [SQL-API](documentdb-introduction.md) ondersteunt het uitvoeren van query's documenten met behulp van SQL. Dit artikel bevat een voorbeelddocument en twee voorbeeld SQL-query's en resultaten.

In dit artikel bevat informatie over de volgende taken: 

> [!div class="checklist"]
> * Opvragen van gegevens met behulp van SQL

## <a name="sample-document"></a>Voorbeelddocument

De SQL-query's in dit artikel gebruikt het volgende voorbeelddocument.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a>Waar kan ik SQL-query's uitvoeren?

U kunt query's met de gegevensverkenner in de Azure portal via uitvoeren de [REST-API en SDK's](sql-api-sdk-dotnet.md), en zelfs de [queryspeelplaats](https://www.documentdb.com/sql/demo), die query's wordt uitgevoerd op een bestaande set met voorbeeldgegevens.

Zie voor meer informatie over SQL-query's:
* [SQL-query en SQL-syntaxis](sql-api-sql-query.md)

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt ervan uitgegaan dat u hebt een Azure DB die Cosmos-account en een verzameling. Geen van deze? Voltooi de [5 minuten Quick Start](create-mongodb-nodejs.md) of de [developer-zelfstudie](tutorial-develop-mongodb.md) voor het maken van een account en een verzameling.

## <a name="example-query-1"></a>Voorbeeldquery 1

Het voorbeeld familie document bovenstaande gegeven, volgende SQL-query retourneert de documenten waarbij het veld id overeenkomt met `WakefieldFamily`. Omdat het een `SELECT *` de uitvoer van de query-instructie is de volledige JSON-document:

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Resultaten**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a>Voorbeeldquery 2

De volgende query retourneert de opgegeven namen van onderliggende items in de familie-id die overeenkomt met `WakefieldFamily` hun hoogwaardige geordend.

**Query**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Resultaten**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Hebt geleerd hoe u een query uitvoert met behulp van SQL  

U kunt nu doorgaan met de volgende zelfstudie voor informatie over het distribueren van uw gegevens globaal.

> [!div class="nextstepaction"]
> [Uw gegevens globaal distribueren](tutorial-global-distribution-sql-api.md)

