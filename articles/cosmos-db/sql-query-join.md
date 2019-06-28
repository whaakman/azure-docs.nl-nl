---
title: SQL-JOIN-query's voor Azure Cosmos DB
description: Meer informatie over het deelnemen aan SQL-syntaxis voor Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 408ee11b318143b3128833a741e04dd68f3816ed
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342477"
---
# <a name="joins-in-azure-cosmos-db"></a>Joins in Azure Cosmos DB

In een relationele database, moeten worden samengevoegd in tabellen de logische overheidsinstelling voor het ontwerpen van genormaliseerde schema's. Daarentegen, de SQL-API maakt gebruik van het model gedenormaliseerd gegevens zonder schema items, dit de logische is equivalent van een *self-join*.

Inner joins leiden tot een volledige vectorproduct van de sets die deel uitmaken van de join. Het resultaat van een join N manier is een set met tuples van de N-element, waarbij elke waarde in de tuple is gekoppeld aan de alias instellen die deel uitmaken van de join en kan worden geopend door te verwijzen naar deze alias in andere componenten.

## <a name="syntax"></a>Syntaxis

De taal die ondersteuning biedt voor de syntaxis van de `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`. Deze query retourneert een set met tuples met `N` waarden. Elke tuple heeft waarden die worden geproduceerd door alle containeraliassen te herhalen voor hun respectieve sets. 

Bekijk de volgende FROM-component: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Elke bron definiëren, kunnen `input_alias1, input_alias2, …, input_aliasN`. Deze component FROM retourneert een set met N-tuples (tuple met N-waarden). Elke tuple heeft waarden die worden geproduceerd door alle containeraliassen te herhalen voor hun respectieve sets.  
  
**Voorbeeld 1** -2 bronnen  
  
- Laat `<from_source1>` container bereik en set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source2>` worden document binnen het bereik van verwijst naar een input_alias1 en sets vertegenwoordigen:  
  
    {1, 2} voor `input_alias1 = A,`  
  
    {3} voor `input_alias1 = B,`  
  
    {4, 5} voor `input_alias1 = C,`  
  
- De component FROM `<from_source1> JOIN <from_source2>` resulteert in de volgende tuples:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Voorbeeld 2** -3-bronnen  
  
- Laat `<from_source1>` container bereik en set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source2>` verwijst naar een document binnen het bereik worden `input_alias1` en sets vertegenwoordigen:  
  
    {1, 2} voor `input_alias1 = A,`  
  
    {3} voor `input_alias1 = B,`  
  
    {4, 5} voor `input_alias1 = C,`  
  
- Laat `<from_source3>` verwijst naar een document binnen het bereik worden `input_alias2` en sets vertegenwoordigen:  
  
    {100, 200} voor `input_alias2 = 1,`  
  
    {300} voor `input_alias2 = 3,`  
  
- De component FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulteert in de volgende tuples:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Gebrek aan tuples voor andere waarden van `input_alias1`, `input_alias2`, waarvoor de `<from_source3>` heeft geen waarden geretourneerd.  
  
**Voorbeeld 3** -3-bronnen  
  
- Laat < from_source1 > worden binnen het bereik van container en set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source1>` container bereik en set {A, B, C} vertegenwoordigen.  
  
- < From_source2 > worden verwijzende input_alias1 document binnen het bereik en sets vertegenwoordigen, kunnen:  
  
    {1, 2} voor `input_alias1 = A,`  
  
    {3} voor `input_alias1 = B,`  
  
    {4, 5} voor `input_alias1 = C,`  
  
- Laat `<from_source3>` worden afgestemd op `input_alias1` en sets vertegenwoordigen:  
  
    {100, 200} voor `input_alias2 = A,`  
  
    {300} voor `input_alias2 = C,`  
  
- De component FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulteert in de volgende tuples:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), C, 4, 300, (C, 5, 300)  
  
  > [!NOTE]
  > Dit heeft geresulteerd in vectorproduct tussen `<from_source2>` en `<from_source3>` omdat beide zijn gericht op hetzelfde `<from_source1>`.  Dit leidde tot 4 (2 x 2) tuples die waarde A of 0 tuples waarde B (1, 0) en 2 (2 x 1) tuples waarde C.  
  
## <a name="examples"></a>Voorbeelden

De volgende voorbeelden laten zien hoe de JOIN-component werkt. In het volgende voorbeeld wordt het resultaat is leeg, omdat het vectorproduct van elk item uit de bron en een lege verzameling leeg is:

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

Het resultaat is:

```json
    [{
    }]
```

In het volgende voorbeeld wordt de join is een vectorproduct tussen twee JSON-objecten, de hoofdmap van het item `id` en de `children` subroot. Het feit dat `children` is een matrix is geschikt voor de join, niet omdat het omgaat met een hoofdmap die is de `children` matrix. Het resultaat bevat slechts twee resultaten, omdat het vectorproduct van elk item met de matrix precies slechts één item levert.

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

De resultaten zijn:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

In het volgende voorbeeld ziet u een conventionelere join:

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

De resultaten zijn:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

De bron van de JOIN-component is een iterator. De stroom in het voorgaande voorbeeld is dus:  

1. Vouw elk onderliggend element `c` in de matrix.
2. Een cross-product met de hoofdmap van het item toepassen `f` met elk onderliggend element `c` die de eerste stap afgevlakt.
3. Ten slotte het hoofdobject van project `f` `id` alleen de eigenschap.

Het eerste item `AndersenFamily`, bevat slechts één `children` -element, zodat de resultatenset alleen een enkel object bevat. Het tweede item `WakefieldFamily`, bevat twee `children`, zodat het vectorproduct twee objecten, één voor elk produceert `children` element. De hoofdvelden in beide items zijn hetzelfde, zoals u zou verwachten bij een vectorproduct.

De echte hulpprogramma van de JOIN-component is het formulier tuples uit het vectorproduct in een vorm die anders moeilijk te project. Het voorbeeld hieronder filters op de combinatie van een tuple waarmee de gebruiker de keuze van een voorwaarde is voldaan door de algehele tuples.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
```

De resultaten zijn:

```json
    [
      {
        "familyName": "AndersenFamily",
        "childFirstName": "Henriette Thaulow",
        "petName": "Fluffy"
      },
      {
        "familyName": "WakefieldFamily",
        "childGivenName": "Jesse",
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

De volgende uitbreiding van het vorige voorbeeld voert een dubbele koppeling. U kunt het vectorproduct kan bekijken als de volgende pseudo-code:

```
    for-each(Family f in Families)
    {
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName,
                  c.givenName AS childGivenName,
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily` een onderliggende die één huisdier heeft, zodat het vectorproduct resulteert in een rij is (1\*1\*1) van deze serie. `WakefieldFamily` heeft twee kinderen, slechts één van wie huisdieren heeft, maar het kind heeft twee huisdieren. Het vectorproduct voor deze serie levert 1\*1\*2 = 2 rijen.

In het volgende voorbeeld wordt er is een extra filter op `pet`, die niet van toepassing op alle tuples waar de huisdier-naam geen is `Shadow`. U kunt tuples van matrices, filter op een van de elementen van de tuple, het bouwen en een combinatie van de elementen van project.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

De resultaten zijn:

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Subquery 's](sql-query-subquery.md)