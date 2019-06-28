---
title: Geparameteriseerde query's in Azure Cosmos DB
description: Meer informatie over SQL geparametriseerde-query 's
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 2bfc22346c1dd43d7d3c2937ffc286e48ae774d0
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342613"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Geparameteriseerde query's in Azure Cosmos DB

Cosmos DB biedt ondersteuning voor query's met parameters worden uitgedrukt met de vertrouwde @ notatie. Geparameteriseerde SQL biedt robuuste verwerking en aanhalingstekens invoer van de gebruiker en wordt voorkomen dat onbedoelde blootstelling van gegevens via SQL-injectie.

## <a name="examples"></a>Voorbeelden

U kunt bijvoorbeeld een query waarmee schrijven `lastName` en `address.state` als parameters en uit te voeren voor verschillende waarden van `lastName` en `address.state` op basis van de invoer van de gebruiker.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Vervolgens kunt u deze aanvraag verzenden naar Cosmos DB als een JSON-query met parameters als volgt uit:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

Het volgende voorbeeld wordt de bovenste argument met een query met parameters: 

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Parameterwaarden mag geldige JSON: tekenreeksen, getallen, Booleaanse waarden, null, zelfs matrices of geneste JSON. Sinds de Cosmos DB schemaloos is, worden parameters worden niet gevalideerd op basis van elk type.


## <a name="next-steps"></a>Volgende stappen

- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Documentgegevens modelleren](modeling-data.md)
