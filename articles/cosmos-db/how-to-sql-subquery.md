---
title: SQL-subquery's voor Azure Cosmos DB
description: Meer informatie over SQL-subquery's en hun algemene scenario's in Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: tisande
ms.openlocfilehash: 3ba547aea9034777fe76f3c911efd2648f6184fa
ms.sourcegitcommit: e729629331ae10097a081a03029398525f4147a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64514797"
---
# <a name="sql-subquery-examples-for-azure-cosmos-db"></a>SQL subquery voorbeelden voor Azure Cosmos DB

Een subquery is een query genest in een andere query. Subquery's wordt ook wel een binnenste query of binnenste selecteren en de instructie een subquery met wordt doorgaans een outer join-query genoemd.

Er zijn twee soorten subquery's:

* Gecorreleerde - gecorreleerde subquery is een subquery die verwijst naar waarden van de buitenste query. De subquery wordt één keer geëvalueerd voor elke rij die is verwerkt door de buitenste query.

* Niet-gecorreleerde - een niet-gecorreleerde subquery is een subquery die onafhankelijk is van de buitenste query en deze kan worden uitgevoerd op een eigen zonder afhankelijkheid van de buitenste query.

> [!NOTE]
> Azure Cosmos DB ondersteunt gecorreleerde subquery's.

## <a name="types-of-subqueries"></a>Typen van subquery 's

Subquery's kunnen verder worden geclassificeerd op basis van het aantal rijen en kolommen die ze retourneren. Er zijn drie verschillende typen:
1.  **tabel**: Retourneert meerdere rijen en meerdere kolommen
2.  **Meerdere waarden**: Meerdere rijen en één kolom geretourneerd
3.  **Scalar**: Retourneert een enkele rij en één kolom

> [!NOTE]
> Azure Cosmos DB biedt ondersteuning voor meerdere waarden en scalaire subquery 's

Azure Cosmos DB SQL-query's retourneren altijd één kolom (een eenvoudige waarde of een complex document). Daarom zijn alleen meerdere waarden en scalaire subquery's hierboven van toepassing in Azure Cosmos DB. Een subquery met meerdere waarden kan alleen worden gebruikt in de component FROM als een relationele-expressie, terwijl een scalaire subquery kan worden gebruikt als een scalaire expressie die u in het selecteren of een WHERE-component of als een relationele-expressie in de component FROM.


## <a name="multi-value-subqueries"></a>Subquery's met meerdere waarden

Subquery's met meerdere waarden retourneert een set documenten en worden altijd gebruikt in de component FROM. Ze worden gebruikt voor:

* JOIN-expressies te optimaliseren 
* Duur expressies eenmaal evalueren en verwijzen naar meerdere keren

### <a name="optimize-join-expressions"></a>JOIN-expressies optimaliseren

Subquery's met meerdere waarden kunnen JOIN expressies optimaliseren door te pushen predicaten na elke select-veel-expressie in plaats van nadat alle cross-joins in de component WHERE.

Houd rekening met de volgende query uit:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0 
AND n.nutritionValue < 10) AND s.amount > 1
```

De index komt overeen met elk document dat een tag met de naam 'volledige formule', een nutriënten item met een waarde tussen 0 en 10 en een item voor met een groter is dan 1 bedrag heeft voor deze query. De JOIN-expressie wordt echter het vectorproduct van alle onderdelen van tags, nutriënten en porties matrices voor elke overeenkomende document uitvoeren voordat een filter wordt toegepast. De WHERE-component wordt het filter predicaat voor elke < c, t, n, s >-tuple vervolgens toepassen. Bijvoorbeeld, als er een overeenkomende document 10 items in elk van de drie matrices, deze wordt uitgebreid tot 1 x 10 x 10 x 10 (dat wil zeggen 1000) tuples. Subquery's hier gebruik, kan u helpen samengevoegde matrixitems filteren voordat hij bij met de volgende expressie.

Deze query is gelijk aan het hierboven, maar maakt gebruik van subquery's:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

Ervan uitgaande dat slechts één item in de matrix tags overeenkomt met het filter en vijf items voor zowel voedingsstoffen en porties matrices, de expressies JOIN wordt uitgebreid tot en met 1 x 1 x 5 x 5 = 25-items in plaats van 1000 items in de eerste query.

### <a name="evaluate-once-and-reference-many-times"></a>Eenmaal en verwijzing vaak evalueren

Subquery's kunt query's met dure expressies, zoals de gebruiker gedefinieerde functies (UDF's) of complexe tekenreeks of aritmetische expressies optimaliseren. De expressie voor één keer geëvalueerd, maar verwijzen we hiernaar vaak kunt u een subquery samen met een JOIN-expressie.

De volgende query wordt de UDF-GetMaxNutritionValue twee keer uitgevoerd:

```sql
SELECT c.id, udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue
FROM c
WHERE udf.GetMaxNutritionValue(c.nutrients) > 100
```

Hier volgt een gelijkwaardige query die wordt de UDF slechts één keer uitgevoerd:

```sql
SELECT TOP 1000 c.id, MaxNutritionValue
FROM c
JOIN (SELECT VALUE udf.GetMaxNutritionValue(c.nutrients)) MaxNutritionValue
WHERE MaxNutritionValue > 100
``` 

> [!NOTE] 
> Het vectorproduct gedrag van JOIN-expressies, opgegeven als de UDF-expressie in niet-gedefinieerde resulteren kan, moet u ervoor zorgen dat de JOIN-expressie altijd één rij produceert door rechtstreeks een object uit de subquery in plaats van de waarde retourneren.
>

Hier volgt een vergelijkbare voorbeeld waarmee een object in plaats van een waarde geretourneerd:

```sql
SELECT TOP 1000 c.id, m.MaxNutritionValue
FROM c
JOIN (SELECT udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue) m
WHERE m.MaxNutritionValue > 100
```

De methode is niet beperkt tot UDF's, maar in plaats daarvan op elke potentieel dure expressie. We kan bijvoorbeeld de dezelfde methode met de avg wiskundige functie nemen:

```sql
SELECT TOP 1000 c.id, AvgNutritionValue
FROM c
JOIN (SELECT VALUE avg(n.nutritionValue) FROM n IN c.nutrients) AvgNutritionValue
WHERE AvgNutritionValue > 80
```

### <a name="mimic-join-with-external-reference-data"></a>Koppelen met externe referentiegegevens nabootsen

We moeten vaak verwijst naar statische gegevens die zelden worden gewijzigd, zoals eenheden van metingen of landcodes. Voor dergelijke gegevens is het beter niet te dupliceren voor elk document. Deze duplicatie vermijden besparen op opslag en schrijfprestaties verbeteren doordat documentgrootte kleiner. Subquery's kan hier worden gebruikt om na te bootsen semantiek voor inner join met een verzameling van referentiegegevens.
Bijvoorbeeld, kunt u overwegen deze set van referentiegegevens.

| **Eenheid** | **Naam**            | **Vermenigvuldiger** | **Basiseenheid** |
| -------- | ------------------- | -------------- | ------------- |
| ng       | Nanogram            | 1.00E-09       | Gram          |
| µg       | Microgram           | 1.00E-06       | Gram          |
| mg       | Milligram           | 1.00E-03       | Gram          |
| g        | Gram                | 1.00E + 00       | Gram          |
| kg       | Kilogram            | 1.00E + 03.       | Gram          |
| Mg       | Megagram            | 1.00E + 06       | Gram          |
| Gg       | Gigagram            | 1.00E + 09       | Gram          |
| nJ       | Nanojoule           | 1.00E-09       | Joule         |
| µJ       | Microjoule          | 1.00E-06       | Joule         |
| mJ       | Millijoule          | 1.00E-03       | Joule         |
| D        | Joule               | 1.00E + 00       | Joule         |
| kJ       | Kilojoule           | 1.00E + 03.       | Joule         |
| MJ       | Megajoule           | 1.00E + 06       | Joule         |
| GJ       | Gigajoule           | 1.00E + 09       | Joule         |
| CAL      | Calorie             | 1.00E + 00       | calorie       |
| kcal     | Calorie             | 1.00E + 03.       | calorie       |
| IU       | Internationale eenheden |                |               |


De volgende query imiteert worden samengevoegd met deze gegevens, zodat we de naam van de eenheid aan de uitvoer toevoegen:

```sql
SELECT TOP 10 n.id, n.description, n.nutritionValue, n.units, r.name
FROM food
JOIN n IN food.nutrients
JOIN r IN (
    SELECT VALUE [
        {unit: 'ng', name: 'nanogram', multiplier: 0.000000001, baseUnit: 'gram'},
        {unit: 'µg', name: 'microgram', multiplier: 0.000001, baseUnit: 'gram'},
        {unit: 'mg', name: 'milligram', multiplier: 0.001, baseUnit: 'gram'},
        {unit: 'g', name: 'gram', multiplier: 1, baseUnit: 'gram'},
        {unit: 'kg', name: 'kilogram', multiplier: 1000, baseUnit: 'gram'},
        {unit: 'Mg', name: 'megagram', multiplier: 1000000, baseUnit: 'gram'},
        {unit: 'Gg', name: 'gigagram', multiplier: 1000000000, baseUnit: 'gram'},
        {unit: 'nJ', name: 'nanojoule', multiplier: 0.000000001, baseUnit: 'joule'},
        {unit: 'µJ', name: 'microjoule', multiplier: 0.000001, baseUnit: 'joule'},
        {unit: 'mJ', name: 'millijoule', multiplier: 0.001, baseUnit: 'joule'},
        {unit: 'J', name: 'joule', multiplier: 1, baseUnit: 'joule'},
        {unit: 'kJ', name: 'kilojoule', multiplier: 1000, baseUnit: 'joule'},
        {unit: 'MJ', name: 'megajoule', multiplier: 1000000, baseUnit: 'joule'},
        {unit: 'GJ', name: 'gigajoule', multiplier: 1000000000, baseUnit: 'joule'},
        {unit: 'cal', name: 'calorie', multiplier: 1, baseUnit: 'calorie'},
        {unit: 'kcal', name: 'Calorie', multiplier: 1000, baseUnit: 'calorie'},
        {unit: 'IU', name: 'International units'}
    ]
)
WHERE n.units = r.unit
```

## <a name="scalar-subqueries"></a>Scalaire subquery 's

Een scalaire subquery-expressie is een subquery die in een enkele waarde resulteert. De waarde van de expressie scalaire subquery is de waarde van de projectie (SELECT-component) van de subquery.  Een scalaire subquery-expressie kan worden gebruikt op talloze plaatsen een scalaire expressie die geldig is. Een scalaire subquery kan bijvoorbeeld worden gebruikt in een expressie in zowel de selecteren en WHERE-componenten.
Echter is met behulp van een scalaire subquery niet altijd te optimaliseren. Bijvoorbeeld, biedt een scalaire subquery als een argument doorgegeven aan een systeem of de gebruiker gedefinieerde functies geen voordelen in RU-verbruik of latentie.

Scalaire subquery's kunnen verder worden geclassificeerd als:
* Simple-Expression Scalar Subqueries
* Aggregate Scalar Subqueries

### <a name="simple-expression-scalar-subqueries"></a>Eenvoudige expressie scalaire subquery 's

Een eenvoudige expressie scalaire subquery is gecorreleerde subquery's met een component SELECT die niet alle statistische expressies bevat. Deze subquery's, wordt er geen optimalisatie voordelen bieden omdat de compiler in een grotere eenvoudige expressie omgezet. Er is geen gecorreleerde context tussen de binnenste en buitenste query.

Hier volgen enkele voorbeelden:

**Voorbeeld 1**

```sql
SELECT 1 AS a, 2 AS b
```

Deze query kan worden herschreven, met behulp van een eenvoudige expressie scalaire subquery op:

```sql
SELECT (SELECT VALUE 1) AS a, (SELECT VALUE 2) AS b
```

Deze uitvoer produceren van beide query's:

```json
[
  { "a": 1, "b": 2 }
]
```

**Voorbeeld 2**

```sql
SELECT TOP 5 Concat('id_', f.id) AS id
FROM food f
```

Deze query kan worden herschreven, met behulp van een eenvoudige expressie scalaire subquery op:

```sql
SELECT TOP 5 (SELECT VALUE Concat('id_', f.id)) AS id
FROM food f
```

Query-uitvoer:

```json
[
  { "id": "id_03226" },
  { "id": "id_03227" },
  { "id": "id_03228" },
  { "id": "id_03229" },
  { "id": "id_03230" }
]
```

**Voorbeeld 3**

```sql
SELECT TOP 5 f.id, Contains(f.description, 'fruit') = true ? f.description : undefined
FROM food f
```

Deze query kan worden herschreven, met behulp van een eenvoudige expressie scalaire subquery op:

```sql
SELECT TOP 10 f.id, (SELECT f.description WHERE Contains(f.description, 'fruit')).description
FROM food f
```

Query-uitvoer:

```json
[
  { "id": "03230" },
  { "id": "03238", "description":"Babyfood, dessert, tropical fruit, junior" },
  { "id": "03229" },
  { "id": "03226", "description":"Babyfood, dessert, fruit pudding, orange, strained" },
  { "id": "03227" }
]
```

## <a name="aggregate-scalar-subqueries"></a>Cumulatieve scalaire subquery 's

Een statistische scalaire subquery is een subquery die een statistische functie in de projectie of filter die in een enkele waarde resulteert heeft.

**Voorbeeld 1:**

Hier volgt een subquery met de expressie voor een enkele statistische functie in de projectie:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg'
) AS count_mg
FROM food f
```

Query-uitvoer:

```json
[
  { "id": "03230", "count_mg": 13 },
  { "id": "03238", "count_mg": 14 },
  { "id": "03229", "count_mg": 13 },
  { "id": "03226", "count_mg": 15 },
  { "id": "03227", "count_mg": 19 }
]
```

**Voorbeeld 2**

Subquery's met meerdere statistische functie-expressies:

```sql
SELECT TOP 5 f.id, (
    SELECT Count(1) AS count, Sum(n.nutritionValue) AS sum 
    FROM n IN f.nutrients 
    WHERE n.units = 'mg'
) AS unit_mg
FROM food f
```

Query-uitvoer:

```json
[
  { "id": "03230","unit_mg": { "count": 13,"sum": 147.072 } },
  { "id": "03238","unit_mg": { "count": 14,"sum": 107.385 } },
  { "id": "03229","unit_mg": { "count": 13,"sum": 141.579 } },
  { "id": "03226","unit_mg": { "count": 15,"sum": 183.91399999999996 } },
  { "id": "03227","unit_mg": { "count": 19,"sum": 94.788999999999987 } }
]
```

**Voorbeeld 3**

Een query met een statistische subquery in de projectie en het filter:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
FROM food f
WHERE (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') > 20
```

Query-uitvoer:

```json
[
  { "id": "03235", "count_mg": 27 },
  { "id": "03246", "count_mg": 21 },
  { "id": "03267", "count_mg": 21 },
  { "id": "03269", "count_mg": 21 },
  { "id": "03274", "count_mg": 21 }
]
```

Een meer optimale manier om te schrijven van deze query is te koppelen op de subquery en verwijzen naar de subquery alias in zowel het selecteren en de WHERE-componenten. Deze query is efficiënter omdat we moeten voor het uitvoeren van de subquery alleen in de join-instructie en niet in de projectie en het filter.

```sql
SELECT TOP 5 f.id, count_mg
FROM food f
JOIN (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
WHERE count_mg > 20
```

### <a name="exists-expression"></a>EXISTS-expressie

Azure Cosmos DB biedt ondersteuning voor expressies met EXISTS. Dit is een cumulatieve scalaire subquery die is ingebouwd in de Azure Cosmos DB SQL API. EXISTS wordt een Booleaanse expressie die u neemt een subquery-expressie en retourneert ' True ' als de subquery alle rijen retourneert. Deze retourneert anders false.
Aangezien Azure Cosmos DB SQL API geen onderscheid tussen Booleaanse expressies en eventuele andere scalaire expressies maken biedt, EXISTS kan worden gebruikt in beide selecteren en WHERE-componenten. Dit is in tegenstelling tot T-SQL, waar een Booleaanse expressie (bijvoorbeeld EXISTS, tussen, en IN) beperkt tot het filter is.

Als de subquery EXISTS één waarde die is gedefinieerd retourneert, wordt en EXISTS geëvalueerd op false. Bijvoorbeeld, houd rekening met de volgende query die wordt geëvalueerd als onwaar:
```sql
SELECT EXISTS (SELECT VALUE undefined)
```   


Echter, als het sleutelwoord waarde in de bovenstaande subquery wordt weggelaten en de query wordt geëvalueerd op ' True ':
```sql
SELECT EXISTS (SELECT undefined) 
```

De subquery plaatst u de lijst met waarden in de select-lijst in een object. Als de geselecteerde lijst geen waarden heeft, retourneert de subquery de enkele waarde '{}' die is gedefinieerd en daarom bestaat resulteert in waar.

### <a name="example-rewriting-arraycontains-and-join-as-exists"></a>Voorbeeld: Herschrijven ARRAY_CONTAINS en neem deel aan als EXISTS

Er is een gebruikelijk van ARRAY_CONTAINS voor het filteren van een document door het bestaan van een item in een matrix. In dit geval wordt gecontroleerd om te zien als de matrix labels een item met de naam oranje bevat.

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE ARRAY_CONTAINS(f.tags, {name: 'orange'})
```

Voor het gebruik van EXISTS kan dezelfde query worden herschreven:

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE EXISTS(SELECT VALUE t FROM t IN f.tags WHERE t.name = 'orange')
```

Daarnaast is er alleen ARRAY_CONTAINS controleren als een waarde gelijk aan elk element in een matrix is. Als u complexere filters in de Matrixeigenschappen nodig zijn, is een JOIN vereist.

Houd rekening met de volgende query dat filters op basis van de eenheden en nutritionValue eigenschappen in de matrix: 

```sql
SELECT VALUE c.description
FROM c
JOIN n IN c.nutrients
WHERE n.units= "mg" AND n.nutritionValue > 0
```

Voor elk van de documenten in de verzameling is wordt een cross-product uitgevoerd met de matrixelementen. Deze JOIN-bewerking maakt het mogelijk om te filteren op eigenschappen binnen de matrix. RU-verbruik van deze query wordt echter aanzienlijk zijn. Bijvoorbeeld, als 1000 documenten 100 items in de matrix heeft, deze wordt uitgebreid tot en met 1000 x 100 (dat wil zeggen 100.000) tuples.

Met behulp van `EXISTS` kan helpen voorkomen dat deze duur vectorproduct:

```sql
SELECT VALUE c.description
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0
)
```

In dit geval filteren we op matrixelementen binnen de subquery EXISTS. Als een element van de matrix komt overeen met het filter, wordt dat we het project en `EXISTS` resulteert in waar.

We kunnen ook alias bestaat en verwijs ernaar in de projectie:

```sql
SELECT TOP 1 c.description, EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0) as a
FROM c
```

Query-uitvoer:

```json
[
    {
        "description": "Babyfood, dessert, fruit pudding, orange, strained",
        "a": true
    }
]
```

### <a name="array-expression"></a>Matrixexpressie

De `ARRAY` expressie kan worden gebruikt om de resultaten van een query als een matrix. Deze expressie kan alleen worden gebruikt in de component SELECT van de query.

```sql
SELECT TOP 1   f.id, ARRAY(SELECT VALUE t.name FROM t in f.tags) AS tagNames
FROM  food f
```

Query-uitvoer:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

Net als bij andere subquery's, filters met de `ARRAY` expressie mogelijk zijn.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t FROM t in c.tags WHERE t.name != 'infant formula') AS tagNames
FROM c
```

Query-uitvoer:

```json
[
    {
        "id": "03226",
        "tagNames": [
            {
                "name": "babyfood"
            },
            {
                "name": "dessert"
            },
            {
                "name": "fruit pudding"
            },
            {
                "name": "orange"
            },
            {
                "name": "strained"
            }
        ]
    }
]
```

Matrix-expressies kunnen ook afkomstig zijn na de FROM-component in subquery's.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t.name FROM t in c.tags) as tagNames
FROM c
JOIN n IN (SELECT VALUE ARRAY(SELECT t FROM t in c.tags WHERE t.name != 'infant formula'))
```

Query-uitvoer:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

## <a name="next-steps"></a>Volgende stappen

- [Voorbeelden van SQL-Query](how-to-sql-query.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Documentgegevens modelleren](modeling-data.md)
