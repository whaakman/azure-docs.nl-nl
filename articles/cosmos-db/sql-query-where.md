---
title: WHERE-component in Azure Cosmos DB
description: Meer informatie over SQL WHERE-component voor Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 6a942e48ffea7785fe971cc2f8fa66e8569ed672
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342541"
---
# <a name="where-clause"></a>WHERE-component

De optionele WHERE-component (`WHERE <filter_condition>`) voorwaarde(n) geeft aan dat de bron-JSON-items om de query voor het opnemen in resultaten voldoen moeten. Een JSON-item moet de opgegeven voorwaarden om te evalueren `true` worden overwogen voor het resultaat. De WHERE-component om te bepalen van de kleinste subset van de bronitems die deel van het resultaat uitmaken kunnen wordt gebruikt door de index-laag.
  
## <a name="syntax"></a>Syntaxis
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Argumenten

- `<filter_condition>`  
  
   Hiermee geeft u de voorwaarde moet worden voldaan voor de documenten die moeten worden geretourneerd.  
  
- `<scalar_expression>`  
  
   Expressie voor de waarde die moet worden berekend. Zie [scalaire expressies](sql-query-scalar-expressions.md) voor meer informatie.  
  

## <a name="remarks"></a>Opmerkingen
  
  Opdat het document dat moet worden geretourneerd van een expressie die is opgegeven als filter moet voorwaarde resulteren in waar. Alleen Booleaanse waarde ' True ', voldoen aan de voorwaarde, een andere waarde: niet-gedefinieerde, null, ingesteld op false, getal, matrix of Object zal voldoen niet aan de voorwaarde. 

## <a name="examples"></a>Voorbeelden

De volgende query aanvragen items die bevatten een `id` waarvan de waarde is de eigenschap `AndersenFamily`. Deze niet van toepassing op elk item dat niet beschikt over een `id` eigenschap of waarvan de waarde komt niet overeen met `AndersenFamily`.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

De resultaten zijn:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>Scalaire expressies in de WHERE-component

Het vorige voorbeeld bevatte een eenvoudige gelijkheidsquery. De SQL-API ondersteunt ook verschillende [scalaire expressies](sql-query-scalar-expressions.md). Binaire en unaire expressies worden het meest gebruikt. Eigenschapsverwijzingen vanaf het JSON-bronobject zijn ook geldige expressies.

U kunt de volgende ondersteunde binaire operators gebruiken:  

|**Operatortype**  | **Waarden** |
|---------|---------|
|Rekenkundig | +, -, *, /, % |
|Bitsgewijs    | \|, &, ^, <<, >>, >>> (opvulling met nullen shift-rechts) |
|Logisch    | EN, OF, NIET      |
|Vergelijking | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|String     |  \|\| (samenvoegen) |

De volgende query's gebruiken binaire operators:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

U kunt ook de unitaire operators +,-, ~, en niet in query's, zoals wordt weergegeven in de volgende voorbeelden:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

U kunt ook de eigenschap verwijzingen gebruiken in query's. Bijvoorbeeld, `SELECT * FROM Families f WHERE f.isRegistered` retourneert het JSON-item met de eigenschap `isRegistered` met de waarde gelijk is aan `true`. Een andere waarde, zoals `false`, `null`, `Undefined`, `<number>`, `<string>`, `<object>`, of `<array>`, sluit u het item van het resultaat. 

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [FROM-component](sql-query-from.md)