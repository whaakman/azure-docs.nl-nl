---
title: SQL-subquery's voor Azure Cosmos DB
description: Meer informatie over SQL-subquery's en hun algemene scenario's in Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: tisande
ms.openlocfilehash: 4181a44e87d59d35d424a51c8fedc89523223f90
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342910"
---
# <a name="sql-subquery-examples-for-azure-cosmos-db"></a>SQL subquery voorbeelden voor Azure Cosmos DB

Een subquery is een query genest in een andere query. Subquery's wordt ook een binnenste query of binnenste Selecteer genoemd. De instructie die een subquery bevat wordt doorgaans een outer join-query genoemd.

Dit artikel wordt beschreven voor SQL-subquery's en hun algemene scenario's in Azure Cosmos DB. Alle voorbeeldquery's in dit document kunnen worden uitgevoerd op de gegevensset over een voeding die vooraf is geladen op de [Azure Cosmos DB-Query Playground](https://www.documentdb.com/sql/demo).

## <a name="types-of-subqueries"></a>Typen van subquery 's

Er zijn twee soorten subquery's:

* **Gecorreleerde**: Een subquery die verwijst naar waarden van de buitenste query. De subquery wordt één keer geëvalueerd voor elke rij die de buitenste query verwerkt.
* **Niet-gecorreleerde**: Een subquery die onafhankelijk is van de buitenste query. Het kan worden uitgevoerd op een eigen zonder afhankelijkheid van de buitenste query.

> [!NOTE]
> Azure Cosmos DB ondersteunt alleen gecorreleerde subquery's.

Subquery's kunnen verder worden geclassificeerd op basis van het aantal rijen en kolommen die ze retourneren. Er zijn drie typen:
* **tabel**: Meerdere rijen en meerdere kolommen geretourneerd.
* **Meerdere waarden**: Meerdere rijen en één kolom geretourneerd.
* **Scalar**: Retourneert één rij en één kolom.

SQL-query's in Azure Cosmos DB retourneren altijd één kolom (een eenvoudige waarde of een complex document). Daarom zijn alleen meerdere waarden en scalaire subquery's van toepassing in Azure Cosmos DB. U kunt meerdere waarden subquery's alleen in de component FROM gebruiken als een relationele-expressie. U kunt een scalaire subquery als een scalaire expressie die u in het selecteren of een WHERE-component, of als een relationele-expressie in de component FROM.

## <a name="multi-value-subqueries"></a>Subquery's met meerdere waarden

Subquery's met meerdere waarden retourneert een set documenten en worden altijd gebruikt in de component FROM. Ze worden gebruikt voor:

* JOIN-expressies te optimaliseren. 
* Duur expressies eenmaal evalueren en verwijzen naar meerdere keren.

## <a name="optimize-join-expressions"></a>JOIN-expressies optimaliseren

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

Voor deze query de index komt overeen met elk document dat heeft een tag met de naam 'volledige formule." Het is een nutriënten item met een waarde tussen 0 en 10, en een item voor met een bedrag dat groter is dan 1. De JOIN-expressie wordt het vectorproduct van alle onderdelen van tags, nutriënten en porties matrices uitvoeren voor elke overeenkomende document voordat een filter wordt toegepast. 

De WHERE-component wordt het filter predicaat voor elke < c, t, n, s >-tuple vervolgens toepassen. Bijvoorbeeld, als er een overeenkomende document 10 items in elk van de drie matrices, deze wordt uitgebreid tot 1 x 10 x 10 x 10 (dat wil zeggen, 1000) tuples. Subquery's hier gebruik kan helpen in de is toegevoegd aan matrixitems filteren voordat hij bij met de volgende expressie.

Deze query is gelijk aan het vorige item maar subquery's gebruikt:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

Wordt ervan uitgegaan dat slechts één item in de matrix tags komt overeen met het filter, en er vijf items voor zowel voedingsstoffen en porties matrices zijn. Vouw de JOIN-expressies vervolgens 1 x 1 x 5 x 5 = 25 items, in plaats van 1000 items in de eerste query.

## <a name="evaluate-once-and-reference-many-times"></a>Eenmaal en verwijzing vaak evalueren

Subquery's kunt query's met dure expressies, zoals de gebruiker gedefinieerde functies (UDF's), complexe tekenreeksen of aritmetische expressies optimaliseren. De expressie voor één keer geëvalueerd, maar verwijzen we hiernaar vaak kunt u een subquery samen met een JOIN-expressie.

De volgende query wordt uitgevoerd voor de UDF `GetMaxNutritionValue` tweemaal:

```sql
SELECT c.id, udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue
FROM c
WHERE udf.GetMaxNutritionValue(c.nutrients) > 100
```

Hier volgt een gelijkwaardige query die de UDF slechts één keer wordt uitgevoerd:

```sql
SELECT TOP 1000 c.id, MaxNutritionValue
FROM c
JOIN (SELECT VALUE udf.GetMaxNutritionValue(c.nutrients)) MaxNutritionValue
WHERE MaxNutritionValue > 100
``` 

> [!NOTE] 
> Houd rekening met het vectorproduct gedrag van JOIN-expressies. Als de UDF-expressie niet-gedefinieerde evalueren kan, moet u ervoor zorgen dat de JOIN-expressie altijd één rij produceert door rechtstreeks een object uit de subquery in plaats van de waarde retourneren.
>

Hier volgt een vergelijkbare voorbeeld waarmee een object in plaats van een waarde geretourneerd:

```sql
SELECT TOP 1000 c.id, m.MaxNutritionValue
FROM c
JOIN (SELECT udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue) m
WHERE m.MaxNutritionValue > 100
```

De methode is niet beperkt tot UDF's. Dit geldt voor een potentieel dure expressie. Bijvoorbeeld, kunt u de dezelfde methode met de functie wiskundige uitvoeren `avg`:

```sql
SELECT TOP 1000 c.id, AvgNutritionValue
FROM c
JOIN (SELECT VALUE avg(n.nutritionValue) FROM n IN c.nutrients) AvgNutritionValue
WHERE AvgNutritionValue > 80
```

## <a name="mimic-join-with-external-reference-data"></a>Koppelen met externe referentiegegevens nabootsen

Mogelijk moet u vaak verwijzen naar statische gegevens die zelden worden gewijzigd, zoals eenheden van de meting of landcodes. Is het beter niet te dupliceren van dergelijke gegevens voor elk document. Deze duplicatie vermijden besparen op opslag en schrijven prestaties verbeteren door de grootte van het document kleinere houden. U kunt een subquery gebruiken om na te bootsen semantiek voor inner join met een verzameling van referentiegegevens.

Bijvoorbeeld, houd rekening met deze reeks referentiegegevens:

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
| J        | Joule               | 1.00E + 00       | Joule         |
| kJ       | Kilojoule           | 1.00E + 03.       | Joule         |
| MJ       | Megajoule           | 1.00E + 06       | Joule         |
| GJ       | Gigajoule           | 1.00E + 09       | Joule         |
| CAL      | Calorie             | 1.00E + 00       | Calorie       |
| kcal     | Calorie             | 1.00E + 03.       | Calorie       |
| IU       | Internationale eenheden |                |               |


De volgende query imiteert worden samengevoegd met deze gegevens, zodat u de naam van de eenheid aan de uitvoer toevoegen:

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

Een scalaire subquery-expressie is een subquery die in een enkele waarde resulteert. De waarde van de expressie scalaire subquery is de waarde van de projectie (SELECT-component) van de subquery.  U kunt een scalaire subquery-expressie gebruiken op talloze plaatsen waar een scalaire expressie die geldig is. U kunt bijvoorbeeld een subquery scalaire expressie in zowel de selecteren en WHERE-componenten.

Met behulp van een scalaire subquery te niet altijd optimaliseren, hoewel. Bijvoorbeeld, biedt een scalaire subquery als een argument doorgegeven aan een systeem of de gebruiker gedefinieerde functies geen voordelen in resourceverbruik eenheid (RU) of latentie.

Scalaire subquery's kunnen verder worden geclassificeerd als:
* Eenvoudige expressie scalaire subquery 's
* Cumulatieve scalaire subquery 's

## <a name="simple-expression-scalar-subqueries"></a>Eenvoudige expressie scalaire subquery 's

Een eenvoudige expressie scalaire subquery is gecorreleerde subquery's met een component SELECT die niet alle statistische expressies bevatten. Deze subquery's, wordt er geen optimalisatie voordelen bieden omdat de compiler in een grotere eenvoudige expressie omgezet. Er is geen gecorreleerde context tussen de binnenste en buitenste query's.

Hier volgen enkele voorbeelden:

**Voorbeeld 1**

```sql
SELECT 1 AS a, 2 AS b
```

U kunt deze query met behulp van een eenvoudige expressie scalaire subquery, te herschrijven:

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

U kunt deze query met behulp van een eenvoudige expressie scalaire subquery, te herschrijven:

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

U kunt deze query met behulp van een eenvoudige expressie scalaire subquery, te herschrijven:

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

### <a name="aggregate-scalar-subqueries"></a>Cumulatieve scalaire subquery 's

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

Hier volgt een subquery met meerdere statistische functie-expressies:

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

Hier volgt een query met een statistische subquery in de projectie en het filter:

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

Een meer optimale manier om te schrijven van deze query is te koppelen op de subquery en verwijzen naar de subquery alias in zowel het selecteren en de WHERE-componenten. Deze query is efficiënter omdat u nodig hebt voor het uitvoeren van de subquery alleen in de join-instructie, en niet in de projectie en het filter.

```sql
SELECT TOP 5 f.id, count_mg
FROM food f
JOIN (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
WHERE count_mg > 20
```

## <a name="exists-expression"></a>EXISTS-expressie

Azure Cosmos DB biedt ondersteuning voor expressies met EXISTS. Dit is een cumulatieve scalaire subquery die is ingebouwd in de Azure Cosmos DB SQL API. EXISTS is een Booleaanse expressie die u neemt een subquery-expressie en retourneert ' True ' als de subquery alle rijen retourneert. Deze retourneert anders false.

Omdat de Azure Cosmos DB SQL API geen onderscheid tussen Booleaanse expressies en eventuele andere scalaire expressies, kunt u EXISTS in beide selecteren en WHERE-componenten. Dit is in tegenstelling tot T-SQL, waar een Booleaanse expressie (bijvoorbeeld EXISTS, tussen, en IN) beperkt tot het filter is.

Als de subquery EXISTS één waarde die niet is gedefinieerd retourneert, bestaat wordt geëvalueerd als onwaar. Bijvoorbeeld, houd rekening met de volgende query die wordt geëvalueerd als onwaar:
```sql
SELECT EXISTS (SELECT VALUE undefined)
```   


Als het sleutelwoord waarde in de voorgaande subquery wordt weggelaten, wordt de query wordt geëvalueerd op ' True ':
```sql
SELECT EXISTS (SELECT undefined) 
```

De subquery plaatst u de lijst met waarden in de geselecteerde lijst in een object. Als de geselecteerde lijst geen waarden heeft, retourneert de subquery de enkele waarde '{}'. Deze waarde is gedefinieerd, zodat EXISTS in waar resulteert.

### <a name="example-rewriting-arraycontains-and-join-as-exists"></a>Voorbeeld: Herschrijven ARRAY_CONTAINS en neem deel aan als EXISTS

Er is een gebruikelijk van ARRAY_CONTAINS voor het filteren van een document door het bestaan van een item in een matrix. In dit geval controleren we om te zien als de matrix labels bevat een item met de naam "oranje."

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE ARRAY_CONTAINS(f.tags, {name: 'orange'})
```

U kunt dezelfde query voor het gebruik van EXISTS herschrijven:

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE EXISTS(SELECT VALUE t FROM t IN f.tags WHERE t.name = 'orange')
```

ARRAY_CONTAINS kan bovendien alleen controleren als een waarde gelijk aan elk element in een matrix is. Als u complexere filters op de Matrixeigenschappen van de moet, gebruikt u de JOIN.

Houd rekening met de volgende query waarmee gefilterd op basis van de eenheden en `nutritionValue` eigenschappen in de matrix: 

```sql
SELECT VALUE c.description
FROM c
JOIN n IN c.nutrients
WHERE n.units= "mg" AND n.nutritionValue > 0
```

Voor elk van de documenten in de verzameling is wordt een cross-product uitgevoerd met de matrixelementen. Deze JOIN-bewerking maakt het mogelijk om te filteren op eigenschappen binnen de matrix. RU-verbruik van deze query wordt echter aanzienlijk zijn. Bijvoorbeeld, als 1000 documenten 100 items in de matrix heeft, deze wordt uitgebreid tot en met 1000 x 100 (dat wil zeggen, 100.000) tuples.

Met behulp van EXISTS kan helpen om te voorkomen dat deze duur vectorproduct:

```sql
SELECT VALUE c.description
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0
)
```

In dit geval filteren u op matrixelementen binnen de subquery EXISTS. Als een matrixelement overeenkomt met het filter, klikt u vervolgens u het project en EXISTS resulteert in waar.

U kunt ook een alias bestaat en verwijs ernaar in de projectie:

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

## <a name="array-expression"></a>Matrixexpressie

U kunt de matrix-expressie gebruiken om de resultaten van een query als een matrix. U kunt deze expressie alleen in de component SELECT van de query.

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

Net als bij andere subquery's, zijn filters met de matrixexpressie mogelijk.

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

- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Documentgegevens modelleren](modeling-data.md)
