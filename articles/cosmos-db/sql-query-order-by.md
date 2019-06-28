---
title: ORDER BY-component in Azure Cosmos DB
description: Meer informatie over SQL ORDER BY-component voor Azure Cosmos DB. Gebruik SQL als een Azure Cosmos DB-JSON-querytaal.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: d0a1ed33d5848c3ed8d5f83af8b320d77fe0dc65
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342953"
---
# <a name="order-by-clause"></a>ORDER BY clause

De optionele ORDER BY-component Hiermee geeft u de sorteervolgorde voor de resultaten geretourneerd door de query.

## <a name="syntax"></a>Syntaxis
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Argumenten
  
- `<sort_specification>`  
  
   Hiermee geeft u een eigenschap of een expressie waarop u wilt sorteren van de queryresultaatset. Een sorteerkolom kan worden opgegeven als een alias voor de naam of de eigenschap.  
  
   Meerdere eigenschappen kunnen worden opgegeven. Namen van eigenschappen moeten uniek zijn. De volgorde van de eigenschappen van de sortering in de component ORDER BY definieert de organisatie van de gesorteerde resultatenset. Dat wil zeggen, de resultatenset is gesorteerd op de eerste eigenschap en vervolgens die geordende lijst is gesorteerd op de tweede eigenschap, enzovoort.  
  
   De namen van eigenschappen waarnaar wordt verwezen in de component ORDER BY moeten overeenkomen met ofwel een eigenschap in de lijst selecteren of een eigenschap die is gedefinieerd in de verzameling die is opgegeven in de component FROM zonder eventuele dubbelzinnigheden.  
  
- `<sort_expression>`  
  
   Hiermee geeft u een of meer eigenschappen of expressies waarop u wilt sorteren van de queryresultaatset.  
  
- `<scalar_expression>`  
  
   Zie de [scalaire expressies](sql-query-scalar-expressions.md) sectie voor meer informatie.  
  
- `ASC | DESC`  
  
   Hiermee geeft u op dat de waarden in de opgegeven kolom moeten worden gesorteerd in oplopende of aflopende volgorde. ASC sorteren van de laagste waarde naar hoogste waarde. DESC sorteren van hoogste waarde naar laagste waarde. ASC is de standaardsorteervolgorde. Null-waarden worden behandeld als de laagste mogelijke waarden.  
  
## <a name="remarks"></a>Opmerkingen  
  
   De component ORDER BY is vereist dat het indexeringsbeleid bevatten een index voor de velden worden gesorteerd. De runtime van de query Azure Cosmos DB biedt ondersteuning voor sorteren op basis van een eigenschapsnaam en niet op basis van de berekende eigenschappen. Azure Cosmos DB biedt ondersteuning voor meerdere ORDER BY-eigenschappen. Als u wilt een query uitvoert met meerdere ORDER BY-eigenschappen, moet u definiëren een [samengestelde index](index-policy.md#composite-indexes) op de velden worden gesorteerd.

## <a name="examples"></a>Voorbeelden

Bijvoorbeeld, als volgt een query waarmee families in oplopende volgorde van de residente plaatsnaam opgehaald:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

De resultaten zijn:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

De volgende query haalt familie `id`s in de volgorde van de datum waarop de item maken. Item `creationDate` een getal vertegenwoordigt de *epoche-tijd*, of de tijd verstreken sinds 1 januari 1970 in seconden.

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

De resultaten zijn:

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

U kunt ook sorteren op meerdere eigenschappen. Een query die door meerdere eigenschappen orders vereist een [samengestelde index](index-policy.md#composite-indexes). Houd rekening met de volgende query uit:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Deze query haalt de familie `id` in oplopende volgorde van de naam van de stad. Als meerdere items dezelfde naam hebben, de query worden gesorteerd door de `creationDate` in aflopende volgorde.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [SELECT-component](sql-query-select.md)
- [De component OFFSET LIMIET](sql-query-offset-limit.md)
