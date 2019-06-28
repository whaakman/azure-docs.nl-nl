---
title: SQL-trefwoorden voor Azure Cosmos DB
description: Meer informatie over SQL trefwoorden voor Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: c9024f120e0a55162a1f6dba0cd9cbda97f5eebc
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342482"
---
# <a name="keywords-in-azure-cosmos-db"></a>Trefwoorden in Azure Cosmos DB
Dit artikel wordt uitgelegd trefwoorden die kunnen worden gebruikt in Azure Cosmos DB SQL-query's.

## <a name="between"></a>TUSSEN

Zoals in de ANSI SQL, kunt u het sleutelwoord BETWEEN om query's voor het bereiken van de tekenreeks of numerieke waarden. De volgende query retourneert bijvoorbeeld alle artikelen waarin zich het eerste onderliggende niveau 1-5, inclusief.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

In tegenstelling tot in ANSI SQL, kunt u ook gebruiken de BETWEEN-component in de component FROM, zoals in het volgende voorbeeld.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

In SQL-API, in tegenstelling tot ANSI SQL, kunt u de bereik-query's op basis van eigenschappen van gemengde gegevenstypen uitdrukken. Bijvoorbeeld, `grade` mogelijk een aantal achtige `5` in sommige items en een tekenreeks, zoals `grade4` in andere gevallen. In dergelijke gevallen, zoals JavaScript, in de vergelijking tussen de twee verschillende typen resulteert in `Undefined`, zodat het item wordt overgeslagen.

> [!TIP]
> Voor snellere query uitvoeringstijden, indexering beleid die gebruikmaakt van een type bereik index op basis van numerieke eigenschappen of paden die de component BETWEEN filters te maken.

## <a name="distinct"></a>DISTINCT

Het sleutelwoord DISTINCT wordt voorkomen dat dubbele waarden in de projectie van de query.

```sql
SELECT DISTINCT VALUE f.lastName
FROM Families f
```

De query projecteert in dit voorbeeld waarden voor elke achternaam op.

De resultaten zijn:

```json
[
    "Andersen"
]
```

U kunt ook unieke objecten projecteren. In dit geval bestaat het veld lastName niet in een van de twee documenten, zodat de query een leeg object retourneert.

```sql
SELECT DISTINCT f.lastName
FROM Families f
```

De resultaten zijn:

```json
[
    {
        "lastName": "Andersen"
    },
    {}
]
```

DISTINCT kan ook worden gebruikt in de projectie in een subquery:

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

Deze query projecteert een matrix met elke kind givenName met dubbele waarden zijn verwijderd. Deze matrix heeft een alias als ChildNames en de geschatte in de buitenste query.

De resultaten zijn:

```json
[
    {
        "id": "AndersenFamily",
        "ChildNames": []
    },
    {
        "id": "WakefieldFamily",
        "ChildNames": [
            "Jesse",
            "Lisa"
        ]
    }
]
```
## <a name="in"></a> IN

Gebruik het sleutelwoord IN om te controleren of een opgegeven waarde komt overeen met een willekeurige waarde in een lijst. De volgende query retourneert bijvoorbeeld alle serie artikelen waarbij de `id` is `WakefieldFamily` of `AndersenFamily`.

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

Het volgende voorbeeld retourneert alle artikelen waar is de status van de opgegeven waarden:

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

De SQL-API biedt ondersteuning voor [iteratie van JSON-matrices](sql-query-object-array.md#Iteration), met een nieuwe constructie toegevoegd via het sleutelwoord in in de bron van. 

## <a name="top"></a>TOP

Het sleutelwoord TOP retourneert de eerste `N` van de resultaten van de query in een niet-gedefinieerde volgorde. Als een best practice, gebruikt u boven met de component ORDER BY te beperken tot de eerste `N` aantal geordende waarden. Deze twee componenten combineren, is de enige manier om aan te geven zoals verwacht welke bovenste is van invloed op rijen.

U kunt boven gebruiken met een constante waarde, zoals in het volgende voorbeeld, of met de waarde van een variabele met geparameteriseerde query's.

```sql
    SELECT TOP 1 *
    FROM Families f
```

De resultaten zijn:

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [Joins](sql-query-join.md)
- [Subquery 's](sql-query-subquery.md)