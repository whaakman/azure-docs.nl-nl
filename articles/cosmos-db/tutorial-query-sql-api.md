---
title: Query's uitvoeren met SQL in Azure Cosmos DB? | Microsoft Docs
description: Leren hoe u query's uitvoert met SQL in Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: rafats
manager: kfile
editor: ''
tags: ''
ms.assetid: ''
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/10/2017
ms.author: rafats
ms.openlocfilehash: 74ea4753e36da7686ac88d4426391a38dac0b436
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-sql-api"></a>Zelfstudie: Query's uitvoeren in Azure Cosmos DB met behulp van de SQL-API

De [SQL-API](documentdb-introduction.md) van Azure Cosmos DB ondersteunt het opvragen van documenten met behulp van SQL. Dit artikel bevat een voorbeelddocument en twee voorbeelden van SQL-query's en de resultaten daarvan.

Dit artikel behandelt de volgende taken: 

> [!div class="checklist"]
> * Gegevens opvragen met behulp van SQL

## <a name="sample-document"></a>Voorbeelddocument

In de SQL-query's in dit artikel wordt het volgende voorbeelddocument gebruikt.

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

U kunt query's uitvoeren met de Data Explorer in de Azure-portal, via de [REST API en SDK's](sql-api-sdk-dotnet.md) en zelfs de [query-speelplaats](https://www.documentdb.com/sql/demo), waar query's worden uitgevoerd voor een bestaande reeks voorbeeldgegevens.

Meer informatie over SQL-query's vindt u in:
* [SQL-query en SQL-syntaxis](sql-api-sql-query.md)

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u een Azure Cosmos DB-account en een verzameling hebt. Hebt u geen van beide? Voltooi de [Quickstart van 5 minuten](create-mongodb-nodejs.md) of de [zelfstudie voor ontwikkelaars](tutorial-develop-mongodb.md) om een account en een verzameling te maken.

## <a name="example-query-1"></a>Voorbeeldquery 1

Op basis van het bovenstaand voorbeelddocument van een familie retourneert de volgende SQL-query de documenten waarin het id-veld gelijk is aan `WakefieldFamily`. Omdat het een `SELECT *`-instructie is, is de uitvoer van de query het volledige JSON-document:

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Results**

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

De volgende query retourneert alle opgegeven namen van kinderen in de familie waarvan de id overeenkomt met `WakefieldFamily`, gesorteerd op leerjaar.

**Query**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Results**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * U hebt geleerd hoe u een query uitvoert met behulp van SQL  

U kunt nu doorgaan met de volgende zelfstudie, waarin u leert hoe u uw gegevens globaal distribueert.

> [!div class="nextstepaction"]
> [Uw gegevens globaal distribueren](tutorial-global-distribution-sql-api.md)

