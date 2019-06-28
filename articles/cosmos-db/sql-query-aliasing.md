---
title: Aliasing in Azure Cosmos DB
description: Meer informatie over aliasing waarden in Azure Cosmos DB SQL-query 's
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: e532fb7180af8a21de6ae9a2e4d798abd9e93e7b
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342930"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Aliasing in Azure Cosmos DB

U kunt expliciet alias waarden in query's. Als een query twee eigenschappen met dezelfde naam heeft, gebruikt u aliasing een of beide van de eigenschappen wijzigen zodat ze in het verwachte resultaat bent disambiguated.

## <a name="examples"></a>Voorbeelden

Het AS-trefwoord gebruikt voor aliasing is optioneel, zoals wordt weergegeven in het volgende voorbeeld bij de projectie van de tweede waarde als `NameInfo`:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

De resultaten zijn:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Volgende stappen

- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [SELECT-component](sql-query-select.md)
- [FROM-component](sql-query-from.md)
