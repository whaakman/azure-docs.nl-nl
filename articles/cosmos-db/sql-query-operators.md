---
title: SQL-query-operators voor Azure Cosmos DB
description: Meer informatie over SQL-operators voor Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: eecc1522f8c260286c7dd7fc4c2e58d5d8caa8fb
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342450"
---
# <a name="operators-in-azure-cosmos-db"></a>Operators in Azure Cosmos DB

Dit artikel worden de verschillende operators ondersteund door Azure Cosmos DB.

## <a name="equality-and-comparison-operators"></a>Gelijkheid en vergelijkingsoperators

In de volgende tabel ziet het resultaat van gelijkheidsvergelijkingen in de SQL-API voor elk paar JSON-typen.

| **Op** | **Undefined** | **Null** | **Boolean** | **Number** | **String** | **Object** | **Array** |
|---|---|---|---|---|---|---|---|
| **Undefined** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Null** | Undefined | **Ok** | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Boolean** | Undefined | Undefined | **Ok** | Undefined | Undefined | Undefined | Undefined |
| **Number** | Undefined | Undefined | Undefined | **Ok** | Undefined | Undefined | Undefined |
| **String** | Undefined | Undefined | Undefined | Undefined | **Ok** | Undefined | Undefined |
| **Object** | Undefined | Undefined | Undefined | Undefined | Undefined | **Ok** | Undefined |
| **Array** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | **Ok** |

Voor vergelijkingsoperators zoals `>`, `>=`, `!=`, `<`, en `<=`, vergelijking van de verschillende typen of tussen twee objecten of matrices produceert `Undefined`.  

Als het resultaat van de scalaire expressie `Undefined`, het item niet is opgenomen in het resultaat, omdat `Undefined` is niet gelijk aan `true`.

## <a name="logical-and-or-and-not-operators"></a>Logische operatoren (EN, OF en NIET)

Logische operatoren worden uitgevoerd op Booleaanse waarden. De volgende tabellen ziet de logische waarheid tabellen voor deze operators:

**Operator OF**

| OF | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Undefined |
| Undefined |True |Undefined |Undefined |

**Operator EN**

| EN | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |False |Undefined |
| False |False |False |False |
| Undefined |Undefined |False |Undefined |

**Operator NIET**

| NIET |  |
| --- | --- |
| True |False |
| False |True |
| Undefined |Undefined |


## <a name="-operator"></a>* operator

De speciale operator * het gehele artikel projecten is. Als u deze operator gebruikt, moet dit het enige geprojecteerde veld zijn. Een query zoals `SELECT * FROM Families f` is geldig, maar `SELECT VALUE * FROM Families f` en `SELECT *, f.id FROM Families f` zijn niet geldig.

## <a name="-and--operators"></a>? en? Operators

U kunt de Ternair (?) en operators (?) voor het bouwen van voorwaardelijke expressies, zoals in programmeertalen, zoals samenvoegen C# en JavaScript. 

U kunt de? de operator op die nieuwe JSON-eigenschappen op elk gewenst moment te maken. Bijvoorbeeld, de volgende query worden geclassificeerd geavanceerde niveaus in `elementary` of `other`:

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

U kunt ook nesten aanroepen naar de? operator, zoals in de volgende query uit: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Net als bij andere standaardoperators voor query's, de? operator niet van toepassing op items als de waarnaar wordt verwezen, eigenschappen ontbreken of de typen dat wordt vergeleken verschillend zijn.

Gebruik de? de operator op die efficiënt controleren voor een eigenschap van een item wanneer een query op gemengde type of semi-gestructureerde gegevens. Bijvoorbeeld, de volgende query retourneert `lastName` indien aanwezig, of `surname` als `lastName` niet aanwezig is.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Volgende stappen

- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [trefwoorden](sql-query-keywords.md)
- [SELECT-component](sql-query-select.md)
