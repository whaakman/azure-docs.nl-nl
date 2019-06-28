---
title: Werken met matrices en objecten in Azure Cosmos DB
description: Meer informatie over matrix- en maken van een SQL-syntaxis voor Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 338f3b51edf38d20a963992e121b7e2dbd0c6873
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342735"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Werken met matrices en objecten in Azure Cosmos DB

Een belangrijke functie van de SQL-API van Azure Cosmos DB is een matrix en object maken.

## <a name="arrays"></a>matrices

U kunt matrices kunt maken, zoals wordt weergegeven in het volgende voorbeeld:

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

De resultaten zijn:

```json
    [
      {
        "CityState": [
          "Seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

U kunt ook de [matrixexpressie](sql-query-subquery.md#array-expression) te maken van een matrix van [subquery](sql-query-subquery.md) resultaten. Deze query haalt alle afzonderlijke opgegeven namen van kinderen in een matrix.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a id="Iteration"></a>Herhaling

De SQL-API biedt ondersteuning voor iteratie van JSON-matrices met een nieuwe constructie toegevoegd via de [trefwoord](sql-query-keywords.md#in) in de bron van. In het volgende voorbeeld:

```sql
    SELECT *
    FROM Families.children
```

De resultaten zijn:

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

De volgende query iteratie van uitvoert `children` in de `Families` container. De uitvoermatrix wijkt af van de voorgaande query. In dit voorbeeld splitst `children`, en worden de resultaten samengevoegd in één matrix:  

```sql
    SELECT *
    FROM c IN Families.children
```

De resultaten zijn:

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

U kunt filteren op elke afzonderlijke vermelding van de matrix, meer, zoals wordt weergegeven in het volgende voorbeeld:

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

De resultaten zijn:

```json
    [{
      "givenName": "Lisa"
    }]
```

U kunt ook via het resultaat van een matrix iteratie samenvoegen. Bijvoorbeeld telt de volgende query het aantal onderliggende items tussen alle families:

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

De resultaten zijn:

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Joins](sql-query-join.md)