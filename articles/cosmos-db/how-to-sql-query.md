---
title: SQL-query's voor Azure Cosmos DB
description: Meer informatie over SQL-syntaxis, database-concepten en SQL-query's voor Azure Cosmos DB. SQL kan worden gebruikt als een JSON-query-taal in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: mjbrown
ms.openlocfilehash: f2ad46e7738582f82edcef6b54ac8234901c887d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885329"
---
# <a name="sql-query-examples-for-azure-cosmos-db"></a>SQL-queryvoorbeelden voor Azure Cosmos DB

Azure Cosmos DB biedt ondersteuning voor het uitvoeren van query's op items met behulp van SQL (Structured Query Language) als een JSON-querytaal in SQL-API-accounts. Het ontwerp van de querytaal voor Azure Cosmos DB is gebaseerd op de volgende twee doelen:

* In plaats van het wiel opnieuw uit te vinden, is Azure Cosmos DB zo gemaakt dat het ondersteuning biedt voor SQL, een algemeen bekende en zeer populaire querytaal. Met de Azure Cosmos DB-SQL beschikt u over een formeel programmeermodel voor het uitvoeren van uitgebreide query's op JSON-items.  

* De querytaal in Azure Cosmos DB is gebaseerd op het JavaScript-programmeermodel. De SQL-API maakt gebruik van het JavaScript-typesysteem, evaluatie van expressies, en functieaanroepen. Dat biedt op zijn beurt weer een natuurlijk programmeermodel voor relationele projecties, hiërarchische navigatie voor JSON-items, self-joins, ruimtelijke query's, en het aanroepen van door de gebruiker gedefinieerde functies (UDF's) die zijn geschreven in JavaScript, en nog veel meer.

Dit artikel bevat enkele voorbeelden van SQL-query's met eenvoudige JSON-items. Zie voor meer informatie over de syntaxis van de SQL-taal in Azure Cosmos DB het artikel met [naslaginformatie over SQL-syntaxis](sql-api-query-reference.md).

## <a id="GettingStarted"></a>Aan de slag met SQL-opdrachten

We gaan twee eenvoudige JSON-items maken en een query op die gegevens uitvoeren. We gebruiken twee JSON-items over families, voegen deze JSON-items in een container in, en voeren vervolgens een query op de gegevens uit. Hier hebben we een eenvoudig JSON-item voor de familie Andersen en de familie Wakefield, de ouders, de kinderen (en hun huisdieren) en hun adres- en registratiegegevens. Het item bevat tekenreeksen, getallen, Booleaanse waarden, matrices en geneste eigenschappen.

**Item1**

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Hier volgt een tweede item met één subtiel verschil: in plaats van `firstName` en `lastName` worden `givenName` en `familyName` gebruikt.

**Item 2**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Nu we gaan enkele query's op deze gegevens uitvoeren zodat u enkele belangrijke aspecten van de SQL-querytaal van Azure Cosmos DB beter begrijpt.

**Query1**: met de volgende query worden bijvoorbeeld de items geretourneerd waarvan het id-veld overeenkomt met `AndersenFamily`. Omdat het een `SELECT *` is, is de uitvoer van de query het volledige JSON-item. Zie [SELECT-instructie](sql-api-query-reference.md#select-query) voor meer informatie over de syntaxis:

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

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
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

**Query2:** stel nu dat we de JSON-uitvoer moeten wijzigen in een andere vorm. Met deze query wordt een nieuw JSON-object met twee geselecteerde velden, Naam en Plaats, geprojecteerd als de plaatsnaam van het adres overeenkomt met de naam van de staat. In dit geval is dat "NY, NY".

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

**Resultaten**

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

**Query3**: deze query retourneert alle opgegeven namen van de kinderen in de familie waarvan de id overeenkomt met `WakefieldFamily`, gesorteerd op woonplaats.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

**Resultaten**

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

Hier volgen enkele aspecten van de Cosmos DB-querytaal in de voorbeelden die u tot dusver hebt gezien:  

* Omdat de SQL-API werkt op basis van JSON-waarden, worden er entiteiten in de vorm van een boomstructuur gebruikt in plaats van rijen en kolommen. Daarom kunt u in de taal verwijzen naar knooppunten van de structuur op elke willekeurige diepte, zoals `Node1.Node2.Node3…..Nodem`, net als bij relationele SQL die verwijst naar de tweedelige verwijzing van `<table>.<column>`.

* De gestructureerde querytaal werkt met gegevens zonder schema. Daarom moet het typesysteem dynamisch worden gebonden. Een expressie kan verschillende typen voor verschillende elementen opleveren. Het resultaat van een query is een geldige JSON-waarde, maar niet per se van een vast schema.  

* Azure Cosmos DB biedt alleen ondersteuning voor JSON-items. Dit betekent dat het typesysteem en expressies alleen geschikt zijn voor JSON-typen. Raadpleeg de [JSON-specificatie](https://www.json.org/) voor meer informatie.  

* Een Cosmos DB-container is een verzameling JSON-items zonder schema. De relaties in gegevensentiteiten binnen en tussen items in een container worden impliciet vastgelegd door insluiting en niet door de relaties tussen de primaire sleutel en de refererende sleutel. Dit is een belangrijk aspect met betrekking tot de joins tussen items die verderop in dit artikel worden besproken.

## <a id="SelectClause"></a>SELECT-component

Elke query bestaat uit een SELECT-component en optionele FROM- en WHERE-componenten conform ANSI-SQL-standaarden. Normaal gesproken wordt de bron in de FROM-component voor elke query geïnventariseerd. Vervolgens wordt het filter in de WHERE-component op de bron toegepast om een subset van JSON-items op te halen. Ten slotte wordt de SELECT-component gebruikt om de vereiste JSON-waarden in de SELECT-lijst te projecten. Zie [SELECT-syntaxis](sql-api-query-reference.md#bk_select_query) voor meer informatie over de syntaxis.

In het volgende voorbeeld ziet u een typische SELECT-query.

**Query’s uitvoeren**

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "seattle"
      }
    }]
```

### <a name="nested-properties"></a>Geneste eigenschappen

In het volgende voorbeeld projecteren we twee geneste eigenschappen: `f.address.state` en `f.address.city`.

**Query’s uitvoeren**

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

```json
    [{
      "state": "WA",
      "city": "seattle"
    }]
```

Projectie biedt ook ondersteuning voor JSON-expressies, zoals wordt weergegeven in het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

```json
    [{
      "$1": {
        "state": "WA",
        "city": "seattle",
        "name": "AndersenFamily"
      }
    }]
```

Bekijk de rol van `$1`. De component `SELECT` moet een JSON-object maken maar omdat er geen sleutel is opgegeven, gebruiken we namen van impliciete argumentvariabelen die beginnen met `$1`. Deze query retourneert bijvoorbeeld twee impliciete argumentvariabelen met de labels `$1` en `$2`.

**Query’s uitvoeren**

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

```json
    [{
      "$1": {
        "state": "WA",
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a id="FromClause"></a>FROM-component

De component <from_specification> is optioneel, tenzij de bron verderop in de query wordt gefilterd of geprojecteerd. Zie [FROM-syntaxis](sql-api-query-reference.md#bk_from_clause) voor meer informatie over de syntaxis. Een query zoals `SELECT * FROM Families` geeft aan dat de hele container Families de bron is die u wilt inventariseren. Een speciale ROOT-id kan worden gebruikt om de container aan te duiden in plaats van de containernaam te gebruiken.
De volgende lijst bevat de regels die voor elke query worden afgedwongen:

* Voor de container kunt u een alias gebruiken, zoals `SELECT f.id FROM Families AS f` of gewoon `SELECT f.id FROM Families f`. Hier is `f` het equivalent van `Families`. `AS` is een optioneel trefwoord tot de alias van de id.  

* Als de alias eenmaal is gebruikt, kan de oorspronkelijke bron niet worden gebonden. Zo is `SELECT Families.id FROM Families f` syntactisch ongeldig omdat de id 'Families' niet meer kan worden opgelost.  

* Alle eigenschappen waarnaar moet worden verwezen, moeten volledig gekwalificeerd zijn. Bij afwezigheid van strikte schemanaleving wordt dit afgedwongen ter voorkoming van niet-eenduidige bindingen. Daarom is `SELECT id FROM Families f` syntactisch ongeldig, omdat de eigenschap `id` niet is gebonden.

### <a name="get-subitems-using-from-clause"></a>Subitems ophalen met behulp van een FROM-component

De bron kan ook worden teruggebracht tot een kleinere subset. Als u bijvoorbeeld alleen een substructuur in elk item wilt inventariseren, kan de submap van de hoofdmap de bron worden, zoals wordt weergegeven in het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT *
    FROM Families.children
```

**Resultaten**

```json
    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
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

In het bovenstaande voorbeeld wordt een matrix als bron gebruikt. U kunt ook een object als bron gebruiken. Dit wordt in het volgende voorbeeld getoond: Elke geldige JSON-waarde (niet ongedefinieerd) die in de bron kan worden gevonden, wordt beschouwd voor opname in het resultaat van de query. Families die geen waarde voor `address.state` bevatten, worden uitgesloten van het queryresultaat.

**Query’s uitvoeren**

```sql
    SELECT *
    FROM Families.address.state
```

**Resultaten**

```json
    [
      "WA",
      "NY"
    ]
```

## <a id="WhereClause"></a>WHERE-component

De WHERE-component (**`WHERE <filter_condition>`**) is optioneel. Hiermee geeft u de voorwaarde(n) op waaraan de JSON-items in de bron moet(en) voldoen om te worden opgenomen in het resultaat. Elk JSON-item wordt pas opgenomen in het resultaat als het evaluatieresultaat van de opgegeven voorwaarden "true" is. De WHERE-component wordt door de indexlaag gebruikt om de kleinste absolute subset van bronitems die deel kunnen uitmaken van het resultaat te bepalen. Zie [WHERE-syntaxis](sql-api-query-reference.md#bk_where_clause) voor meer informatie over de syntaxis.

Met de volgende query worden de items opgevraagd die een naameigenschap met de waarde `AndersenFamily` bevatten. Elk ander item dat geen naameigenschap bevat, of waarvan de waarde niet overeenkomt met `AndersenFamily`, worden uitgesloten.

**Query’s uitvoeren**

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "seattle"
      }
    }]
```

Het vorige voorbeeld bevatte een eenvoudige gelijkheidsquery. De SQL-API biedt ook ondersteuning voor tal van scalaire expressies. Binaire en unaire expressies worden het meest gebruikt. Eigenschapsverwijzingen vanaf het JSON-bronobject zijn ook geldige expressies.

De volgende binaire operatoren worden momenteel ondersteund en kunnen worden gebruikt in query's zoals wordt weergegeven in de volgende voorbeelden:  

|**Operatortype**  | **Waarden** |
|---------|---------|
|Rekenkundig | +, -, *, /, % |
|Bitsgewijs    | \|, &, ^, <<, >>, >>> (opvulling met nullen shift-rechts) |
|Logisch    | EN, OF, NIET      |
|Vergelijking | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|String     |  \|\| (samenvoegen) |

Laten we eens enkele query's met binaire operatoren bekijken.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5
```

De unaire operatoren +, -, ~ en NIET worden ook ondersteund, en kunnen worden gebruikt in query's zoals wordt weergegeven in de volgende voorbeelden:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Naast de binaire en unaire operatoren zijn ook verwijzingen naar eigenschappen toegestaan. Zo retourneert `SELECT * FROM Families f WHERE f.isRegistered` het JSON-item met de eigenschap `isRegistered` waarvan de waarde van de eigenschap gelijk is aan de JSON-waarde `true`. Bij elke andere waarde (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, enzovoort) wordt het bronitem uitgesloten van het resultaat. 

### <a name="equality-and-comparison-operators"></a>Gelijkheids- en vergelijkingsoperatoren

In de volgende tabel ziet het resultaat van gelijkheidsvergelijkingen in de SQL-API voor elk paar JSON-typen.

| **Op** | **Undefined** | **Null** | **Booleaans** | **Aantal** | **String** | **Object** | **Matrix** |
|---|---|---|---|---|---|---|---|
| **Undefined** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Null** | Undefined | **OK** | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Booleaans** | Undefined | Undefined | **OK** | Undefined | Undefined | Undefined | Undefined |
| **Aantal** | Undefined | Undefined | Undefined | **OK** | Undefined | Undefined | Undefined |
| **String** | Undefined | Undefined | Undefined | Undefined | **OK** | Undefined | Undefined |
| **Object** | Undefined | Undefined | Undefined | Undefined | Undefined | **OK** | Undefined |
| **Matrix** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | **OK** |

Voor andere vergelijkingsoperatoren zoals >, >=, !=, < en <= gelden de volgende regels:

* Vergelijking van verschillende typen resulteert in Undefined.  
* Vergelijking van twee objecten of twee matrices resulteert in Undefined.

Als het resultaat van de scalaire expressie in het filter Undefined is, wordt het bijbehorende item niet opgenomen in het resultaat, omdat Undefined logisch niet overeenkomt met "true".

## <a name="between-keyword"></a>Sleutelwoord BETWEEN
U kunt ook het sleutelwoord BETWEEN gebruiken om query's uit te voeren op een bereik van waarden, zoals in ANSI SQL. BETWEEN kan worden gebruikt voor tekenreeksen of getallen.

Deze query retourneert bijvoorbeeld alle familie-items waarvan de cijferwaarde (grade) van het eerste onderliggende item tussen 1 en 5 (inclusief 1 en 5) ligt.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

In tegenstelling tot in ANSI SQL, kunt u de BETWEEN-component ook in de FROM-component gebruiken, zoals het volgende voorbeeld laat zien.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

Het belangrijkste verschil tussen het gebruik van BETWEEN in de SQL-API en ANSI SQL is dat u bereikquery's kunt uitvoeren op eigenschappen van verschillende typen. Zo kan "grade" bijvoorbeeld een getal (5) zijn in sommige items en tekenreeksen in andere items ("grade4"). In dergelijke gevallen, zoals in JavaScript, resulteert een vergelijking tussen twee verschillende typen in 'Undefined' en wordt het item overgeslagen.

> [!NOTE]
> Als u snellere query-uitvoeringstijden wilt, moet u een indexeringsbeleid maken dat gebruikmaakt van een type bereikindex op basis van numerieke eigenschappen/paden die zijn gefilterd in de BETWEEN-component.

### <a name="logical-and-or-and-not-operators"></a>Logische operatoren (EN, OF en NIET)
Logische operatoren worden uitgevoerd op Booleaanse waarden. De logische waarheidstabellen voor deze operatoren worden weergegeven in de volgende tabellen.

**OF een operator**

| OF | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Undefined |
| Undefined |True |Undefined |Undefined |

**EN de operator**

| EN | True | False | Undefined |
| --- | --- | --- | --- |
| True |True |False |Undefined |
| False |False |False |False |
| Undefined |Undefined |False |Undefined |

**NIET-operator**

| NIET |  |
| --- | --- |
| True |False |
| False |True |
| Undefined |Undefined |

## <a name="in-keyword"></a>Sleutelwoord IN

Het sleutelwoord IN kan worden gebruikt om te controleren of een opgegeven waarde overeenkomt met een willekeurige waarde in een lijst. Deze query retourneert bijvoorbeeld alle familie-items waarvan de id overeenkomt met 'WakefieldFamily' of 'AndersenFamily'.

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

In dit voorbeeld worden alle items geretourneerd waarvan de staat overeenkomt met een van de opgegeven waarden.

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

## <a name="ternary--and-coalesce--operators"></a>Ternaire operatoren (?) en samenvoegingsoperatoren (??)

De ternaire en samenvoegingsoperatoren kunnen worden gebruikt om voorwaardelijke expressies te bouwen, vergelijkbaar met populaire programmeertalen zoals C# en JavaScript. De ternaire operator (?) kan handig zijn bij het maken van nieuwe JSON-eigenschappen op elk gewenst moment. Zo kunt u nu query's voor het classificeren van de klassenniveaus schrijven in een voor mensen leesbare vorm, bijvoorbeeld Beginner/Gemiddeld/Geavanceerd, zoals hieronder wordt weergegeven.

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

U kunt ook de aanroepen van de operator nesten, zoals in de onderstaande query.

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel
    FROM Families.children[0] c
```

Net zoals bij andere operatoren voor query's geldt hier ook: als de eigenschappen waarnaar wordt verwezen in de voorwaardelijke expressie in een item ontbreken, of als de typen die worden vergeleken niet overeenkomen, worden die items niet in de queryresultaten opgenomen.

De samenvoegingsoperator (??) kan worden gebruikt om efficiënt te controleren of een eigenschap aanwezig is in een item. Deze operator is handig bij het uitvoeren van query's op semi-gestructureerde gegevens of gegevens van verschillende typen. Deze query retourneert bijvoorbeeld de eigenschap "lastName" (achternaam) als deze aanwezig is, of de eigenschap "surname" (achternaam) als deze niet aanwezig is.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a id="EscapingReservedKeywords"></a>Accessor van eigenschap tussen aanhalingstekens
U kunt ook toegang tot eigenschappen krijgen met behulp van de operator voor eigenschappen tussen aanhalingstekens `[]`. Zo is `SELECT c.grade` bijvoorbeeld het equivalent van `SELECT c["grade"]`. Deze syntaxis is handig wanneer u een eigenschap die spaties of speciale tekens bevat, of dezelfde naam heeft als een SQL-sleutelwoord of een gereserveerd woord, wilt voorzien van escapetekens.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

## <a name="aliasing"></a>Aliasing

Nu gaan we het voorbeeld hierboven uitbreiden met expliciete aliasing van waarden. AS is het sleutelwoord dat wordt gebruikt voor aliasing. Het is optioneel, zoals wordt weergegeven bij het projecteren van de tweede waarde als `NameInfo`.

Als een query twee eigenschappen met dezelfde naam bevat, moet aliasing worden gebruikt om de naam van een van beide eigenschappen of de naam van beide eigenschappen zo te wijzigen dat het geprojecteerde resultaat niet meer ambigu kan zijn.

**Query’s uitvoeren**

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="scalar-expressions"></a>Scalaire expressies

Naast verwijzingen naar eigenschappen biedt de SELECT-component ook ondersteuning voor scalaire expressies, zoals constanten, rekenkundige expressies, logische expressies, enzovoort. Dit is bijvoorbeeld een eenvoudige 'Hallo wereld'-query.

**Query’s uitvoeren**

```sql
    SELECT "Hello World"
```

**Resultaten**

```json
    [{
      "$1": "Hello World"
    }]
```

Dit is een iets ingewikkelder voorbeeld met een scalaire expressie.

**Query’s uitvoeren**

```sql
    SELECT ((2 + 11 % 7)-2)/3
```

**Resultaten**

```json
    [{
      "$1": 1.33333
    }]
```

In het volgende voorbeeld is het resultaat van de scalaire expressie een Booleaanse waarde.

**Query’s uitvoeren**

```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

**Resultaten**

```json
    [
      {
        "AreFromSameCityState": false
      },
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="object-and-array-creation"></a>Objecten en matrices maken

Een andere belangrijke functie van de SQL-API is de functie voor het maken van objecten/matrices. In het vorige voorbeeld hebt u een nieuw JSON-object gemaakt. Op deze manier kan men ook matrices bouwen, zoals wordt weergegeven in de volgende voorbeelden:

**Query’s uitvoeren**

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

**Resultaten**

```json
    [
      {
        "CityState": [
          "seattle",
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

## <a id="ValueKeyword"></a>Sleutelwoord VALUE

Het sleutelwoord **VALUE** (waarde) biedt de mogelijkheid om een JSON-waarde te retourneren. De onderstaande query retourneert bijvoorbeeld de scalaire waarde `"Hello World"` in plaats van `{$1: "Hello World"}`.

**Query’s uitvoeren**

```sql
    SELECT VALUE "Hello World"
```

**Resultaten**

```json
    [
      "Hello World"
    ]
```

De volgende query retourneert de JSON-waarde zonder het label `"address"` in de resultaten.

**Query’s uitvoeren**

```sql
    SELECT VALUE f.address
    FROM Families f
```

**Resultaten**

```json
    [
      {
        "state": "WA",
        "county": "King",
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan",
        "city": "NY"
      }
    ]
```

Het volgende uitgebreide voorbeeld laat zien hoe u primitieve JSON-waarden (het knooppuntniveau van de JSON-structuur) kunt retourneren.

**Query’s uitvoeren**

```sql
    SELECT VALUE f.address.state
    FROM Families f
```

**Resultaten**

```json
    [
      "WA",
      "NY"
    ]
```

## <a name="-operator"></a>Operator *
De speciale operator (*) wordt ondersteund om het item ongewijzigd te projecteren. Als u deze operator gebruikt, moet dit het enige geprojecteerde veld zijn. Een query zoals `SELECT * FROM Families f` is wel geldig, maar `SELECT VALUE * FROM Families f` en `SELECT *, f.id FROM Families f` zijn niet geldig.

**Query’s uitvoeren**

```sql
    SELECT * 
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

**Resultaten**

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
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a id="TopKeyword">Operator TOP</a>

Het sleutelwoord TOP kan worden gebruikt om het aantal resultaatwaarden van een query te beperken. Als u TOP gebruikt in combinatie met de ORDER BY-component, wordt de resultatenset beperkt tot de eerste N geordende waarden. Anders worden de eerste N resultaten in niet-gedefinieerde volgorde geretourneerd. Het is raadzaam in een SELECT-instructie altijd een ORDER BY-component te gebruiken in combinatie met de TOP-component. Het combineren van deze twee componenten is de enige manier om te voorspellen welke rijen worden beïnvloed door TOP. 

**Query’s uitvoeren**

```sql
    SELECT TOP 1 *
    FROM Families f
```

**Resultaten**

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
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

TOP kan worden gebruikt in combinatie met een constante waarde (zoals hierboven) of een variabele waarde met behulp van geparameteriseerde query's. Zie Geparameteriseerde query's verderop voor meer informatie.

## <a id="Aggregates"></a>Statistische functies

U kunt ook statistische functies uitvoeren in de `SELECT`-component. Met statistische functies wordt er een berekening uitgevoerd op een set met waarden en één waarde geretourneerd. De volgende query retourneert bijvoorbeeld het aantal familie-items in de container.

**Query’s uitvoeren**

```sql
    SELECT COUNT(1)
    FROM Families f
```

**Resultaten**

```json
    [{
        "$1": 2
    }]
```

U kunt ook de scalaire waarde van de statistische functie retourneren met behulp van het sleutelwoord `VALUE`. De volgende query retourneert bijvoorbeeld het aantal waarden als één getal:

**Query’s uitvoeren**

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

**Resultaten**

```json
    [ 2 ]
```

U kunt ook statistische functies uitvoeren in combinatie met filters. De volgende query retourneert bijvoorbeeld het aantal items met een adres in de staat Washington.

**Query’s uitvoeren**

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

**Resultaten**

```json
    [ 1 ]
```

De volgende tabel bevat de lijst met ondersteunde statistische functies in de SQL-API. `SUM` en `AVG` worden uitgevoerd over numerieke waarden, terwijl `COUNT`, `MIN`, en `MAX` in getallen, tekenreeksen, Booleaanse waarden en een null-waarden kunnen worden uitgevoerd.

| Gebruik | Description |
|-------|-------------|
| COUNT | Retourneert het aantal items in de expressie. |
| SUM   | Retourneert de som van alle waarden in de expressie. |
| MIN   | Retourneert de minimumwaarde in de expressie. |
| MAX   | Retourneert de maximumwaarde in de expressie. |
| AVG   | Retourneert het gemiddelde van de waarden in de expressie. |

Statistische functies kunnen ook worden uitgevoerd op de resultaten van een herhaling van de matrix. Zie [Herhaling van de matrix in query's](#Iteration) voor meer informatie.

> [!NOTE]
> Wanneer u Data Explorer van Azure Portal gebruikt, moet u er rekening mee houden dat met statistische functies mogelijk slechts een gedeelte van de resultaten van een querypagina wordt geretourneerd. De SDK's produceren één cumulatieve waarde, die is berekend over alle pagina's.
>
> Als u statistische query's wilt uitvoeren met behulp van code, hebt u .NET SDK 1.12.0, .NET Core SDK 1.1.0 of Java SDK 1.9.5 of hoger nodig.
>

## <a id="OrderByClause"></a>ORDER BY-component

Net zoals in ANSI SQL, kunt u een optionele ORDER BY-component opnemen in uw query's. Desgewenst kunt u het optionele argument ASC/DESC in de component gebruiken om op te geven in welke volgorde de resultaten moeten worden opgehaald.

Dit is een query waarmee families worden opgehaald op volgorde van de naam van de woonplaats.

**Query’s uitvoeren**

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

**Resultaten**

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

En dit is een query waarmee families worden opgehaald op volgorde van de aanmaakdatum, die is opgeslagen als een getal dat de Epoch-tijd aangeeft (de verstreken tijd sinds 1 januari 1970, in seconden).

**Query’s uitvoeren**

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

**Resultaten**

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

## <a id="Advanced"></a>Geavanceerde databaseconcepten en SQL-query's

### <a id="Iteration"></a>Herhaling

Er is een nieuwe constructie toegevoegd via het sleutelwoord **IN** in de SQL-API om ondersteuning te bieden voor herhaling voor JSON-matrices. De FROM-bron biedt ondersteuning voor herhaling. Laten we eens beginnen met het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT *
    FROM Families.children
```

**Resultaten**

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

Nu bekijken we een andere query die herhaling uitvoert op onderliggende items in de container. Let op het verschil in de uitvoermatrix. In dit voorbeeld wordt `children` gesplitst en worden de resultaten plat gemaakt in één matrix.  

**Query’s uitvoeren**

```sql
    SELECT *
    FROM c IN Families.children
```

**Resultaten**

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

Hiermee kunt u ook filteren op elke afzonderlijke vermelding van de matrix, zoals wordt weergegeven in het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

**Resultaten**

```json
    [{
      "givenName": "Lisa"
    }]
```

U kunt ook statistische functies uitvoeren op het resultaat van de herhaling van de matrix. Met de volgende query wordt bijvoorbeeld het aantal kinderen in alle gezinnen geteld.

**Query’s uitvoeren**

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

**Resultaten**

```json
    [
      {
        "$1": 3
      }
    ]
```

### <a id="Joins"></a>Samenvoegingen

In een relationele database is het belangrijk om tabellen te kunnen samenvoegen. Dat is het logische gevolg van het ontwerpen van genormaliseerde schema's. De SQL-API is daarentegen bedoeld voor een gedenormaliseerd gegevensmodel met items zonder schema, wat het logische equivalent is van een 'self-join'.

De syntaxis die voor de taal wordt ondersteund is `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`. Deze query retourneert een set met **N**-tuples (tuple met **N** waarden). Elke tuple heeft waarden die worden geproduceerd door alle containeraliassen te herhalen voor hun respectieve sets. Met andere woorden, deze query wordt uitgevoerd op het volledige vectorproduct van de sets die deelnemen aan de join.

De volgende voorbeelden laten zien hoe de JOIN-component werkt. In het volgende voorbeeld is het resultaat leeg omdat het vectorproduct van elk item uit de bron en een lege set leeg is.

**Query’s uitvoeren**

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

**Resultaten**

```json
    [{
    }]
```

In het volgende voorbeeld wordt de join uitgevoerd op de hoofdmap van het item en de submap `children`. Het is een vectorproduct tussen twee JSON-objecten. Het feit dat 'children' een matrix is geldt niet in de JOIN omdat we te maken hebben met één hoofdmap: de matrix 'children'. De resultatenset bevat daarom slechts twee resultaten, omdat het vectorproduct van elk item met de matrix slechts één item retourneert.

**Query’s uitvoeren**

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

**Resultaten**

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

**Query’s uitvoeren**

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

**Resultaten**

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

Merk op dat de `from_source` van de **JOIN**-component een iterator is. De stroom ziet er in dit geval als volgt uit:  

* Vouw elk onderliggend element **c** in de matrix uit.
* Pas een vectorproduct toe met de hoofdmap van het item **f** met elk onderliggend element **c** dat in de eerste stap plat is gemaakt.
* Projecteer ten slotte alleen de naameigenschap van het hoofdobject **f**.

Het eerste item (`AndersenFamily`) bevat slechts één onderliggend element. Daarom bevat de resultatenset slechts één object dat met dit item overeenkomt. Het tweede item (`WakefieldFamily`) bevat twee onderliggende elementen. Het vectorproduct produceert dan ook een afzonderlijk object voor elk onderliggend element. Dit resulteert in twee objecten, één voor elk onderliggend element dat met dit item overeenkomt. De hoofdvelden in beide items zijn hetzelfde, zoals u zou verwachten bij een vectorproduct.

Het grote voordeel van de JOIN is dat u er tuples van het vectorproduct mee kunt maken in een vorm die anders moeilijk te projecteren is. Zoals we in het volgende voorbeeld zien, kunt u filteren op de combinatie van een tuple zodat de gebruiker een algemene voorwaarde kan kiezen waaraan de tuples moeten voldoen.

**Query’s uitvoeren**

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

**Resultaten**

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

Dit voorbeeld is een natuurlijke uitbreiding van het vorige voorbeeld, en voert een dubbele join uit. Het vectorproduct kan dus worden gezien als de volgende pseudocode:

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

`AndersenFamily` heeft één onderliggende die één huisdier heeft. Het vectorproduct van deze familie is één rij (1\*1\*1). WakefieldFamily heeft twee kinderen, maar alleen Jesse heeft huisdieren. Jesse heeft echter twee huisdieren. Het vectorproduct van deze familie is twee rijen (1\*1\*2).

In het volgende voorbeeld wordt een extra filter op `pet` toegepast, dat tuples uitsluit als de naam van het huisdier niet 'Shadow' is. We kunnen tuples bouwen op basis van matrices, filteren op een of meer elementen van de tuple, en elke gewenste combinatie van de elementen projecteren.

**Query’s uitvoeren**

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

**Resultaten**

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a id="JavaScriptIntegration"></a>JavaScript-integratie

Azure Cosmos DB biedt een programmeermodel voor het rechtstreeks uitvoeren van JavaScript-toepassingslogica op de containers via opgeslagen procedures en triggers. Deze methode biedt ondersteuning voor het volgende:

* Mogelijkheid om krachtige transactionele CRUD-bewerkingen en query's uit te voeren op items in een container dankzij de diepe integratie van JavaScript-runtime in de database-engine.
* Natuurlijke modellering van controlestroom, variabele scopes en toewijzing en integratie van primitieven voor afhandeling van uitzonderingen bij databasetransacties. Zie de documentatie over de programmeerbaarheid van de JavaScript-server voor meer informatie over Azure Cosmos DB-ondersteuning voor JavaScript-integratie.

### <a id="UserDefinedFunctions"></a>Door de gebruiker gedefinieerde functies (UDF's)

Naast de typen die al in dit artikel gedefinieerd zijn, biedt de SQL-API ook ondersteuning voor door de gebruiker gedefinieerde functies (UDF's). In het bijzonder worden scalaire UDF's ondersteund. Hiermee kunnen de ontwikkelaars nul of meerdere argumenten doorgeven en de resultaten als één resultaat retourneren. Voor alle argumenten wordt gecontroleerd of ze geldige JSON-waarden zijn.  

De SQL-syntaxis is uitgebreid ter ondersteuning van aangepaste toepassingslogica met behulp van deze door de gebruiker gedefinieerde functies. UDF's kunnen worden geregistreerd met de SQL-API en vervolgens kan ernaar worden verwezen in een SQL-query. De UDFs zijn speciaal ontworpen voor aanroepen vanuit query's. Als gevolg van deze keuze hebben UDF's geen toegang tot het contextobject, terwijl de andere JavaScript-typen (opgeslagen procedures en triggers) dat wel hebben. Omdat query's worden uitgevoerd als alleen-lezen, kunnen ze zowel op primaire als secundaire replica's worden uitgevoerd. UDF's zijn echter ontworpen om te worden uitgevoerd op secundaire replica's, in tegenstelling tot andere JavaScript-typen.

Hier volgt een voorbeeld van hoe een UDF kan worden geregistreerd in de Cosmos DB-database, en in het bijzonder onder een itemcontainer.

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) {
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
           regexMatchUdf).Result;  
```

In het bovenstaande voorbeeld wordt een UDF met de naam `REGEX_MATCH` gemaakt. Deze accepteert twee JSON-tekenreekswaarden, `input` en `pattern`, en controleert of de eerste overeenkomt met het patroon dat is opgegeven in de tweede met behulp van de JavaScript-functie string.match().

We kunnen deze UDF nu gebruiken in een query in een projectie. UDF's moeten worden gekwalificeerd met het hoofdlettergevoelige voorvoegsel 'udf.' wanneer ze worden aangeroepen vanuit query's.

> [!NOTE]
> Vóór 17-3-2015 ondersteunde Cosmos DB nog UDF-aanroepen zonder het voorvoegsel 'udf.' zoals SELECT REGEX_MATCH(). Dit aanroeppatroon is inmiddels afgeschaft.  
>

**Query’s uitvoeren**

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

**Resultaten**

```json
    [
      {
        "$1": true
      },
      {
        "$1": false
      }
    ]
```

De UDF kan ook worden gebruikt in een filter, zoals wordt weergegeven in het onderstaande voorbeeld, waarin de UDF ook is gekwalificeerd met 'udf.' als voorvoegsel:

**Query’s uitvoeren**

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

**Resultaten**

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

In wezen zijn UDF's geldige scalaire expressies en kunnen ze worden gebruikt in projecties en filters.

UDF's bieden echter meer krachtige mogelijkheden, die we zullen illustreren met een ander voorbeeld met voorwaardelijke logica:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
                seaLevelUdf);
```

Hier volgt een voorbeeld met de UDF.

**Query’s uitvoeren**

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

**Resultaten**

```json
     [
      {
        "city": "seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Zoals de voorgaande voorbeelden laten zien, bieden UDF's de mogelijkheid om de kracht van JavaScript-taal te combineren met de uitgebreide programmeerbare interface van de SQL-API. Zo kunt u complexe procedurele, voorwaardelijke logica uitvoeren met behulp van de geïntegreerde functionaliteit van JavaScript-runtime.

De SQL-API levert de argumenten aan de UDF's voor elk item in de bron in de huidige verwerkingsfase (WHERE- of SELECT-component) van de UDF. Het resultaat wordt naadloos opgenomen in de algehele uitvoeringspijplijn. Als de eigenschappen waarnaar wordt verwezen door de UDF parameters niet beschikbaar zijn in de JSON-waarde, wordt de parameter beschouwd als niet-gedefinieerd en wordt de hele UDF-aanroep overgeslagen. Ook als het resultaat van de UDF-aanroep niet-gedefinieerd is, wordt het niet opgenomen in het eindresultaat.

Samenvattend: UDF's zijn erg handig voor het uitvoeren van complexe bedrijfslogica als onderdeel van een query.

### <a name="operator-evaluation"></a>Evaluatie van operatoren

Omdat Cosmos DB feitelijk een JSON-database is, zijn er parallellen met JavaScript-operatoren en de bijbehorende evaluatiesemantiek. Hoewel de JSON-ondersteuning voor JavaScript-semantiek over het algemeen hetzelfde is in Cosmos DB, wijkt de evaluatie van bewerkingen in sommige gevallen af.

In de SQL-API zijn de typen waarden vaak pas bekend als de waarden uit de database zijn opgehaald. Bij traditionele SQL is dat niet het geval. Om query's efficiënt te kunnen uitvoeren, gelden er strikte vereisten voor typen voor de meeste operatoren.

In de SQL-API worden er geen impliciete conversies uitgevoerd, terwijl dat in JavaScript wel het geval is. Met een query zoals `SELECT * FROM Person p WHERE p.Age = 21` worden items geretourneerd waarvan de waarde van de eigenschap 'Age' (leeftijd) gelijk is aan 21. Elk ander item waarvan de eigenschap voor de leeftijd overeenkomt met de tekenreeks '21' of andere mogelijk oneindige variaties zoals '021', '21,0', '0021' of '00021', wordt niet geretourneerd als overeenkomst. Dat is wel het geval bij JavaScript, waar tekenreekswaarden impliciet via een cast-conversie worden omgezet naar getallen (op basis van de operator, ex: ==). Deze keuze is van cruciaal belang voor efficiënte indexvergelijking in de SQL-API.

## <a name="parameterized-sql-queries"></a>Geparameteriseerde SQL-query's

Cosmos DB biedt ondersteuning voor query's met parameters die worden uitgedrukt in de vertrouwde notatie \@. Geparameteriseerde SQL biedt mogelijkheden om gebruikersinvoer te verwerken en van aanhalingstekens te voorzien. Dit voorkomt onbedoelde blootstelling van gegevens via SQL-injectie.

U kunt bijvoorbeeld een query schrijven met de achternaam en de staat uit de adresgegevens als parameters, en de query vervolgens uitvoeren met verschillende waarden voor de achternaam en de staat op basis van gebruikersinvoer.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Deze aanvraag kan vervolgens naar Cosmos DB worden verzonden als geparameteriseerde JSON-query, zoals hieronder wordt weergegeven.

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

Het argument voor TOP kan worden ingesteld met geparameteriseerde query's, zoals hieronder wordt weergegeven.

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Parameterwaarden kunnen elke geldige JSON zijn (tekenreeksen, getallen, Booleaanse waarden, null of zelfs matrices of geneste JSON). Omdat Cosmos DB schemaloos is, worden parameters ook niet gevalideerd op een type.

## <a id="BuiltinFunctions"></a>Ingebouwde functies

Cosmos DB ondersteunt ook een aantal ingebouwde functies voor algemene bewerkingen die kunnen worden gebruikt in query's, zoals door de gebruiker gedefinieerde functies (UDF's).

| Functiegroep | Bewerkingen |
|---------|----------|
| Wiskundige functies | ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, TAN |
| Functies voor controle van het type | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, IS_PRIMITIVE |
| Tekenreeksfuncties | CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, UPPER |
| Matrixfuncties | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH en ARRAY_SLICE |
| Ruimtelijke functies | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, ST_ISVALIDDETAILED |

Als u momenteel een door de gebruiker gedefinieerde functie (UDF) gebruikt waarvoor nu een ingebouwde functie beschikbaar is, kunt u beter de ingebouwde functie gebruiken omdat die vaak sneller en efficiënter wordt uitgevoerd.

### <a name="mathematical-functions"></a>Wiskundige functies

Met elke wiskundige functie wordt een berekening op basis van de opgegeven invoerwaarden uitgevoerd en een numerieke waarde geretourneerd. Dit is een tabel met ondersteunde ingebouwde wiskundige functies.

| Gebruik | Description |
|----------|--------|
| [ABS (num_expr) | Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie. |
| CEILING (num_expr) | Retourneert het kleinste gehele getal dat groter is dan of gelijk is aan de opgegeven numerieke expressie. |
| FLOOR (num_expr) | Retourneert het grootste gehele getal dat kleiner is dan of gelijk is aan de opgegeven numerieke expressie. |
| EXP (num_expr) | Retourneert de exponent van de opgegeven numerieke expressie. |
| LOG (num_expr ,base) | Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie, of de logaritme met behulp van het opgegeven grondtal |
| LOG10 (num_expr) | Retourneert de waarde van de logaritme met het grondtal 10 van de opgegeven numerieke expressie. |
| ROUND (num_expr) | Retourneert een numerieke waarde, afgerond naar het dichtstbijzijnde gehele getal. |
| TRUNC (num_expr) | Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal. |
| SQRT (num_expr) | Retourneert de vierkantswortel van de opgegeven numerieke expressie. |
| SQUARE (num_expr) | Retourneert het kwadraat van de opgegeven numerieke expressie. |
| POWER (num_expr, num_expr) | Retourneert de machtsverheffing met de opgegeven waarde van de opgegeven numerieke expressie. |
| SIGN (num_expr) | Retourneert de waarde voor het teken (-1, 0, 1) van de opgegeven numerieke expressie. |
| ACOS (num_expr) | Retourneert de hoek, in radialen, waarvan de cosinus de opgegeven numerieke expressie is. Dit wordt ook wel de arccosinus genoemd. |
| ASIN (num_expr) | Retourneert de hoek, in radialen, waarvan de sinus de opgegeven numerieke expressie is. Deze functie wordt ook wel de arcsinus genoemd. |
| ATAN (num_expr) | Retourneert de hoek, in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook wel de arctangens genoemd. |
| ATN2 (num_expr) | Retourneert de hoek, in radialen, tussen de positieve x-as en de straal vanaf de oorsprong tot het punt (y, x), waarbij x en y de waarden van de twee opgegeven float-expressies zijn. |
| COS (num_expr) | Retourneert de trigonometrische cosinus van de opgegeven hoek, in radialen, in de opgegeven expressie. |
| COT (num_expr) | Retourneert de trigonometrische cotangens van de opgegeven hoek, in radialen, in de opgegeven expressie. |
| DEGREES (num_expr) | Retourneert de overeenkomende hoek in graden voor een hoek die is opgegeven in radialen. |
| PI () | Retourneert de constante waarde van PI. |
| RADIANS (num_expr) | Retourneert radialen als een numerieke expressie in graden wordt ingevoerd. |
| SIN (num_expr) | Retourneert de trigonometrische sinus van de opgegeven hoek, in radialen, in de opgegeven expressie. |
| TAN (num_expr) | Retourneert de tangens van de ingevoerde expressie, in de opgegeven expressie. |

U kunt nu bijvoorbeeld query's uitvoeren zoals wordt weergegeven in het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT VALUE ABS(-4)
```

**Resultaten**

```json
    [4]
```

Het belangrijkste verschil tussen de Cosmos DB-functies en ANSI SQL is dat ze bij uitstek geschikt zijn voor gegevens zonder schema en gegevens met een gemengd schema. Als bijvoorbeeld de eigenschap voor de grootte van een item ontbreekt, of een niet-numerieke waarde zoals 'opbekend' heeft, wordt het item overgeslagen in plaats van dat er een foutmelding wordt weergegeven.

### <a name="type-checking-functions"></a>Functies voor controle van het type

Met de typecontrolefuncties kunt u het type van een expressie in SQL-query's controleren. Typecontrolefuncties kunnen op elk gewenst moment worden gebruikt om het type van eigenschappen in items te bepalen wanneer het type variabel of niet bekend is. Dit is een tabel met ondersteunde ingebouwde typecontrolefuncties.

| **Gebruik** | **Description** |
|-----------|------------|
| [IS_ARRAY (markering)](sql-api-query-reference.md#bk_is_array) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een matrix is. |
| [IS_BOOL (expr)](sql-api-query-reference.md#bk_is_bool) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een Booleaanse waarde is. |
| [IS_NULL (expr)](sql-api-query-reference.md#bk_is_null) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde null is. |
| [IS_NUMBER (markering)](sql-api-query-reference.md#bk_is_number) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een getal is. |
| [IS_OBJECT (expr)](sql-api-query-reference.md#bk_is_object) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een JSON-object is. |
| [IS_STRING (markering)](sql-api-query-reference.md#bk_is_string) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks is. |
| [IS_DEFINED (expr)](sql-api-query-reference.md#bk_is_defined) | Retourneert een Booleaanse waarde die aangeeft of aan de eigenschap een waarde is toegewezen. |
| [IS_PRIMITIVE (markering)](sql-api-query-reference.md#bk_is_primitive) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks, getal, Booleaanse waarde of null is. |

Met deze functies kunt u nu bijvoorbeeld query's uitvoeren zoals die in het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT VALUE IS_NUMBER(-4)
```

**Resultaten**

```json
    [true]
```

### <a name="string-functions"></a>Tekenreeksfuncties

Met de volgende scalaire functies wordt een bewerking op een tekenreeksinvoerwaarde uitgevoerd en een tekenreeks, numerieke waarde of Booleaanse waarde geretourneerd. Hier volgt een tabel met ingebouwde tekenreeksfuncties:

| Gebruik | Beschrijving |
| --- | --- |
| [LENGTH (str_expr)](sql-api-query-reference.md#bk_length) | Retourneert het aantal tekens van de opgegeven tekenreeksexpressie |
| [CONCAT (str_expr, str_expr [, str_expr])](sql-api-query-reference.md#bk_concat) | Retourneert een tekenreeks die het resultaat is van het samenvoegen van twee of meer tekenreekswaarden. |
| [SUBSTRING (str_expr, num_expr, num_expr)](sql-api-query-reference.md#bk_substring) | Retourneert een deel van een tekenreeksexpressie. |
| [STARTSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_startswith) | Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie begint met de tweede |
| [ENDSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_endswith) | Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie eindigt met de tweede |
| [CONTAINS (str_expr, str_expr)](sql-api-query-reference.md#bk_contains) | Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie de tweede bevat. |
| [INDEX_OF (str_expr, str_expr)](sql-api-query-reference.md#bk_index_of) | Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste opgegeven tekenreeksexpressie, of -1 als de tekenreeks niet is gevonden. |
| [LEFT (str_expr, num_expr)](sql-api-query-reference.md#bk_left) | Retourneert het linkerdeel van een tekenreeks met het opgegeven aantal tekens. |
| [RIGHT (str_expr, num_expr)](sql-api-query-reference.md#bk_right) | Retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens. |
| [LTRIM (str_expr)](sql-api-query-reference.md#bk_ltrim) | Retourneert een tekenreeksexpressie na het verwijderen van lege voorlooptekens. |
| [RTRIM (str_expr)](sql-api-query-reference.md#bk_rtrim) | Retourneert een tekenreeksexpressie na het afkappen van alle lege volgtekens. |
| [LOWER (str_expr)](sql-api-query-reference.md#bk_lower) | Retourneert een tekenreeksexpressie na het converteren van tekens in hoofdletters naar kleine letters. |
| [UPPER (str_expr)](sql-api-query-reference.md#bk_upper) | Retourneert een tekenreeksexpressie na het converteren van tekens in kleine letters naar hoofdletters. |
| [REPLACE (str_expr, str_expr, str_expr)](sql-api-query-reference.md#bk_replace) | Vervangt alle exemplaren van een opgegeven tekenreekswaarde door een andere tekenreekswaarde. |
| [REPLICATE (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/sql-api-sql-query-reference#bk_replicate) | Herhaalt een tekenreekswaarde een opgegeven aantal keren. |
| [REVERSE (str_expr)](sql-api-query-reference.md#bk_reverse) | Retourneert een tekenreekswaarde in de omgekeerde volgorde. |

Met deze functies kunt u nu query's uitvoeren zoals die in het volgende voorbeeld. Zo kunt u bijvoorbeeld de familienaam als volgt retourneren in hoofdletters:

**Query’s uitvoeren**

```sql
    SELECT VALUE UPPER(Families.id)
    FROM Families
```

**Resultaten**

```json
    [
        "WAKEFIELDFAMILY",
        "ANDERSENFAMILY"
    ]
```

U kunt ook tekenreeksen samenvoegen, zoals in dit voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families
```

**Resultaten**

```json
    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]
```

Tekenreeksfuncties kunnen ook in de WHERE-component worden gebruikt om resultaten te filteren, zoals in het volgende voorbeeld:

**Query’s uitvoeren**

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")
```

**Resultaten**

```json
    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]
```

### <a name="array-functions"></a>Matrixfuncties

Met de volgende scalaire functies wordt een bewerking op een matrixinvoerwaarde uitgevoerd en een numerieke waarde, Booleaanse waarde of matrixwaarde geretourneerd. Hier volgt een tabel met ingebouwde matrixfuncties:

| Gebruik | Beschrijving |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](sql-api-query-reference.md#bk_array_length) |Retourneert het aantal elementen van de opgegeven matrixexpressie. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](sql-api-query-reference.md#bk_array_concat) |Retourneert een matrix die het resultaat is van het samenvoegen van twee of meer matrixwaarden. |
| [ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](sql-api-query-reference.md#bk_array_contains) |Retourneert een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat. U kunt kiezen tussen een volledige of gedeeltelijke overeenkomst. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](sql-api-query-reference.md#bk_array_slice) |Retourneert een deel van een matrixexpressie. |

Matrixfuncties kunnen worden gebruikt voor het bewerken van matrices in JSON. Met de volgende query worden bijvoorbeeld alle items geretourneerd waarbij een van de ouders 'Robin Wakefield' is. 

**Query’s uitvoeren**

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })
```

**Resultaten**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

U kunt een gedeeltelijk fragment opgeven voor het vergelijken van elementen in de matrix. De volgende query retourneert alle ouders met de `givenName` van `Robin`.

**Query’s uitvoeren**

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)
```

**Resultaten**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Hier volgt nog een voorbeeld waarin ARRAY_LENGTH wordt gebruikt om het aantal kinderen per familie op te halen.

**Query’s uitvoeren**

```sql
    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 
```

**Resultaten**

```json
    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]
```

### <a name="spatial-functions"></a>Ruimtelijke functies

Cosmos DB biedt ondersteuning voor de volgende ingebouwde OGC-functies (Open Geospatial Consortium) voor georuimtelijke query's. 

| Gebruik | Beschrijving |
| --- | --- |
| ST_DISTANCE (point_expr, point_expr) | Retourneert de afstand tussen de twee GeoJSON Point-, Polygon- of LineString-expressies. |
| T_WITHIN (point_expr, polygon_expr) | Retourneert een Booleaanse expressie die aangeeft of het eerste GeoJSON-object (Point, Polygon of LineString) zich bevindt in het tweede GeoJSON-object (punt, Polygon of LineString). |
| ST_INTERSECTS (spatial_expr, spatial_expr) | Retourneert een Booleaanse expressie die aangeeft of de twee opgegeven GeoJSON-objecten (Point, Polygon of LineString) elkaar snijden. |
| ST_ISVALID | Retourneert een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is. |
| ST_ISVALIDDETAILED | Retourneert een JSON-waarde met een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is. Als de expressie ongeldig is, worden ook de reden daarvoor en een tekenreekswaarde geretourneerd. |

Ruimtelijke functies kunnen worden gebruikt om nabijheidsquery's uit te voeren op ruimtelijke gegevens. Zo retourneert de volgende query alle familie-items binnen 30 kilometer van de opgegeven locatie met behulp van de ingebouwde functie ST_DISTANCE.

**Query’s uitvoeren**

```sql
    SELECT f.id
    FROM Families f
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

**Resultaten**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Zie [Werken met georuimtelijke gegevens in Azure Cosmos DB](geospatial.md) voor meer informatie over ondersteuning voor georuimtelijke gegevens. Hiermee zijn we aan het einde gekomen van de ruimtelijke functies en de SQL-syntaxis voor Cosmos DB. Laten we nu eens kijken hoe LINQ-query's werken ten opzichte van de syntaxis die we tot nu toe hebben gezien.

## <a id="Linq"></a>LINQ to SQL-API

LINQ is een .NET-programmeermodel waarin berekeningen worden uitgedrukt als query's op objectstromen. Cosmos DB biedt een clientbibliotheek voor de interactie met LINQ door het faciliteren van een conversie tussen JSON- en .NET-objecten en het toewijzen van een subset van LINQ-query's aan Cosmos DB-query's.

In de volgende afbeelding ziet u de architectuur voor ondersteunende LINQ-query's met Cosmos DB.  Met de Cosmos DB-client kunnen ontwikkelaars een **IQueryable**-object maken waarmee query's rechtstreeks worden uitgevoerd op de Cosmos DB-queryprovider, die de LINQ-query vervolgens omzet in een Cosmos DB-query. De query wordt vervolgens doorgegeven aan de Cosmos DB-server om een resultatenset in JSON-indeling op te halen. De geretourneerde resultaten worden op de client gedeserialiseerd in een stroom .NET-objecten.

![Architectuur voor ondersteunende LINQ-query's met de SQL-API - SQL-syntaxis, JSON-querytaal, databaseconcepten en SQL-query's][1]

### <a name="net-and-json-mapping"></a>.NET- en JSON-toewijzing

De toewijzing tussen .NET-objecten en JSON-items spreekt voor zich. Elk gegevenslidveld wordt toegewezen aan een JSON-object, waarbij de naam van het veld wordt toegewezen aan het sleutelgedeelte van het object en het waardegedeelte recursief wordt toegewezen aan het waardegedeelte van het object. Kijk een naar het volgende voorbeeld: het gemaakte object Family wordt toegewezen aan het JSON-object, zoals hieronder wordt weergegeven. Het JSON-item wordt op zijn beurt ook weer toegewezen aan een .NET-object.

**C#-klasse**

```csharp
    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };
```

**JSON**

```json
    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                "givenName": "Jesse",
                "gender": "female",
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller",
              "givenName": "Lisa",
              "gender": "female",
              "grade": 8
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };
```


### <a name="linq-to-sql-translation"></a>LINQ to SQL-omzetting

De Cosmos DB-queryprovider doet een zo goed mogelijke poging tot omzetting van een LINQ-query in een SQL-query voor Cosmos DB. In de volgende beschrijving gaan we ervan uit dat de lezer redelijk bekend is met LINQ.

Ten eerste, voor het typensysteem ondersteunen we alle typen JSON-primitieven: numerieke typen, booleaanse typen, tekenreekstypen en null-typen. Alleen deze JSON-typen worden ondersteund. De volgende scalaire expressies worden ondersteund.

* Constante waarden: deze omvatten constante waarden van de primitieve gegevenstypen op het moment dat de query wordt geëvalueerd.
* Eigenschap/matrix-indexexpressies: deze expressies verwijzen naar de eigenschap van een object of een element van de matrix.
  
     family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n is een int-variabele
* Wiskundige expressies: deze omvatten veelgebruikte rekenkundige expressies op basis van numerieke en Booleaanse waarden. Raadpleeg de SQL-specificatie voor de volledige lijst.
  
     2 * family.children[0].grade;    x + y;
* Tekenreeksvergelijkingsexpressie: deze omvat de vergelijking van een tekenreekswaarde met een constante tekenreekswaarde.  
  
     mother.familyName == "Smith";    child.givenName == s; //s is een tekenreeksvariabele
* Expressie voor het maken van een object/matrix: deze expressies retourneren een object van een samengestelde-waardetype of een anoniem type of een matrix met dergelijke objecten. Deze waarden kunnen worden genest.
  
     new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //een anoniem type met twee velden              
     new int[] { 3, child.grade, 5 };

### <a id="SupportedLinqOperators"></a>Lijst met ondersteunde LINQ-operatoren

Hier volgt een lijst met ondersteunde LINQ-operatoren in de LINQ-provider die bij de SQL .NET SDK wordt meegeleverd.

* **Select**: projecties worden omgezet naar de SQL-instructie SELECT inclusief objectconstructie
* **Where**: filters worden omgezet naar de SQL-instructie WHERE en bieden ondersteuning voor omzetting van &&, || en ! naar de SQL-operatoren
* **SelectMany**: maakt het mogelijk om matrices af te wikkelen naar de SQL-JOIN-component. Kan worden gebruikt om keten- of geneste expressies te maken om te filteren op matrixelementen
* **OrderBy en OrderByDescending**: wordt omgezet naar ORDER BY oplopen/aflopend
* Operatoren **Count**, **Sum**, **Min**, **Max** en **Average** voor statistische functies en de bijbehorende asynchrone equivalenten **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync** en **AverageAsync**.
* **CompareTo**: wordt omgezet naar bereikvergelijkingen. Dit wordt vaak gebruikt voor tekenreeksen, omdat deze kunnen worden vergeleken in .NET
* **Take**: wordt omgezet naar de SQL-instructie TOP om het aantal resultaten van een query te beperken
* **Wiskundige functies**: bieden ondersteuning voor het omzetten van de .NET-functies Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan en Truncate naar de equivalente ingebouwde SQL-functies.
* **Tekenreeksfuncties**: bieden ondersteuning voor het omzetten van de .NET-functies Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString en ToUpper naar de equivalente ingebouwde SQL-functies.
* **Matrixfuncties**: bieden ondersteuning voor het omzetten van de .NET-functies Concat, Contains en Count naar de equivalente ingebouwde SQL-functies.
* **Georuimtelijke extensiefuncties**: bieden ondersteuning voor het omzetten van de stubmethoden Distance, Within, IsValid en IsValidDetailed naar de equivalente ingebouwde SQL-functies.
* **Door de gebruiker gedefinieerde extensiefunctie**: biedt ondersteuning voor het omzetten van de stubmethode UserDefinedFunctionProvider.Invoke naar de corresponderende door de gebruiker gedefinieerde functie.
* **Miscellaneous**: biedt ondersteuning voor het omzetten van de samenvoegings- en voorwaardelijke operatoren. Kan Contains omzetten naar STRING_CONTAINS, ARRAY_CONTAINS of de SQL-instructie IN, afhankelijk van de context.

### <a name="sql-query-operators"></a>Operatoren voor SQL-query's

Hier volgen enkele voorbeelden die laten zien hoe u sommige standaardoperatoren voor LINQ-query's kunt omzetten naar Cosmos DB-query's.

#### <a name="select-operator"></a>Operator Select

De syntaxis is `input.Select(x => f(x))`. Hierbij is `f` een scalaire expressie.

**LINQ lambda-expressie**

```csharp
    input.Select(family => family.parents[0].familyName);
```

**SQL** 

```sql
    SELECT VALUE f.parents[0].familyName
    FROM Families f
```

**LINQ lambda-expressie**

```csharp
    input.Select(family => family.children[0].grade + c); // c is an int variable
```

**SQL**

```sql
    SELECT VALUE f.children[0].grade + c
    FROM Families f
```

**LINQ lambda-expressie**

```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
```

**SQL** 

```sql
    SELECT VALUE {"name":f.children[0].familyName,
                  "grade": f.children[0].grade + 3 }
    FROM Families f
```


#### <a name="selectmany-operator"></a>Operator SelectMany

De syntaxis is `input.SelectMany(x => f(x))`. Hierbij is `f` een scalaire expressie die een containertype retourneert.

**LINQ lambda-expressie**

```csharp
    input.SelectMany(family => family.children);
```

**SQL**

```sql
    SELECT VALUE child
    FROM child IN Families.children
```

#### <a name="where-operator"></a>Operator Where

De syntaxis is `input.Where(x => f(x))`. Hierbij is `f` een scalaire expressie die een Booleaanse waarde retourneert.

**LINQ lambda-expressie**

```csharp
    input.Where(family=> family.parents[0].familyName == "Smith");
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
```

**LINQ lambda-expressie**

```csharp
    input.Where(
        family => family.parents[0].familyName == "Smith" &&
        family.children[0].grade < 3);
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3
```

### <a name="composite-sql-queries"></a>Samengestelde SQL-query's

De bovenstaande operatoren kunnen worden gecombineerd tot krachtige samengestelde query's. Omdat Cosmos DB ondersteuning biedt voor geneste containers, kan de samenstelling worden samengevoegd of genest.

#### <a name="concatenation"></a>Samenvoegen

De syntaxis is `input(.|.SelectMany())(.Select()|.Where())*`. Een samengevoegde query kan beginnen met een optionele `SelectMany`-query, gevolgd door meerdere `Select`- of `Where`-operatoren.

**LINQ lambda-expressie**

```csharp
    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
```

**LINQ lambda-expressie**

```csharp
    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);
```

**SQL**

```sql
    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3
```

**LINQ lambda-expressie**

```csharp
    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
```

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)
```

**LINQ lambda-expressie**

```csharp
    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");
```

**SQL**

```sql
    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"
```

#### <a name="nesting"></a>Nesten

De syntaxis is `input.SelectMany(x=>x.Q())`. Hierbij is Q een `Select`-, `SelectMany`- of `Where`-operator.

In een geneste query, wordt de inner query toegepast op elk element van de outer container. Het is belangrijk te weten dat in de inner query kan worden verwezen naar de velden van de elementen in de outer container, zoals bij self-joins.

**LINQ lambda-expressie**

```csharp
    input.SelectMany(family=>
        family.parents.Select(p => p.familyName));
```

**SQL**

```sql
    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents
```

**LINQ lambda-expressie**

```csharp
    input.SelectMany(family =>
        family.children.Where(child => child.familyName == "Jeff"));
```

**SQL**

```sql
    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"
```

**LINQ lambda-expressie**

```csharp
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));
```

**SQL**

```sql
    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName
```

## <a id="ExecutingSqlQueries"></a>SQL-query's uitvoeren

Cosmos DB ontsluit resources via een REST API die kan worden aangeroepen via elke taal waarmee HTTP-/HTTPS-aanvragen kunnen worden gemaakt. Cosmos DB biedt bovendien programmeringsbibliotheken voor verschillende veelgebruikte talen zoals .NET, Node.js, JavaScript en Python. De REST API en de verschillende bibliotheken bieden alle ondersteuning voor het uitvoeren van query's via SQL. De .NET SDK biedt de mogelijkheid om zowel SQL-query's als LINQ-query's uit te voeren.

De volgende voorbeelden laten zien hoe u een query maakt en verzendt met een Cosmos DB-databaseaccount.

### <a id="RestAPI"></a>REST API

Cosmos DB biedt een open RESTful-programmeermodel via HTTP. Databaseaccounts kunnen worden ingericht met behulp van een Azure-abonnement. Het Cosmos DB-resourcemodel bestaat uit een set met resources onder een databaseaccount, waarbij elke resource kan worden geadresseerd via een logische en stabiele URI. Een set met resources wordt in dit artikel aangeduid als een feed. Een databaseaccount bestaat uit een set met databases, die elk meerdere containers bevatten die op hun beurt weer items, UDF's en andere resourcetypen bevatten.

Als basismodel voor interactie met deze resources gebruikt u de HTTP-woorden GET, PUT, POST en DELETE met hun standaardfunctie. De POST-bewerking wordt gebruikt voor het maken van een nieuwe resource, voor het uitvoeren van een opgeslagen procedure of voor het uitgeven van een Cosmos DB-query. Query's zijn altijd alleen-lezen bewerkingen zonder neveneffecten.

In de volgende voorbeelden ziet u een POST-bewerking voor een SQL-API die wordt uitgevoerd op een container met de twee voorbeelditems die we al eerder hebben gezien. De query bevat een eenvoudig filter op de JSON-naameigenschap. Let op het gebruik van de headers `x-ms-documentdb-isquery` en Content-Type: `application/query+json` om aan te duiden dat de bewerking een query is.

**Aanvraag**
```
    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

**Resultaten**

```
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

In het tweede voorbeeld ziet u een complexere query die meerdere resultaten uit de join retourneert.

**Aanvraag**
```
    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": [] 
    }
```

**Resultaten**

```
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Als de resultaten van een query niet op één pagina passen, retourneert de REST API een vervolgtoken via de antwoordheader `x-ms-continuation-token`. Clients kunnen resultaten pagineren door de header op te nemen in de volgende resultaten. Het aantal resultaten per pagina kan ook worden beheerd via de nummerheader `x-ms-max-item-count`. Als de opgegeven query een statistische functie zoals `COUNT` bevat, kan een gedeeltelijk geaggregeerde waarde worden geretourneerd op de pagina met resultaten. De clients moeten een tweede statische functie op deze resultaten uitvoeren om het gewenste eindresultaat te verkrijgen, bijvoorbeeld door de som te berekenen van de aantallen die zijn geretourneerd in de afzonderlijke pagina's om het totaalaantal te retourneren.

Voor het beheren van het beleid voor gegevensconsistentie voor query's, gebruikt u de header `x-ms-consistency-level` zoals in alle REST API-aanvragen. Voor sessieconsistentie moet u ook de echo van de meest recente cookieheader `x-ms-session-token` toevoegen aan de queryaanvraag. Het indexeringsbeleid van de container waarop de query wordt uitgevoerd kan ook van invloed zijn op de consistentie van queryresultaten. Bij de standaardinstellingen voor het indexeringsbeleid voor containers wordt de index altijd bijgewerkt met de iteminhoud en zullen de queryresultaten overeenkomen met de consistentie die voor de gegevens is gekozen. Zie de [consistentieniveaus van Azure Cosmos DB][consistency-levels] voor meer informatie.

Als de opgegeven query niet wordt ondersteund door het geconfigureerde indexeringsbeleid voor de container, retourneert de Azure Cosmos DB-server het foutbericht '400 - Ongeldige aanvraag'. Dit foutbericht wordt geretourneerd voor query's met paden expliciet is uitgesloten van het indexeren. U kunt de header `x-ms-documentdb-query-enable-scan` opgeven om een scanbewerking door de query te laten uitvoeren als een index niet beschikbaar is.

U kunt gedetailleerde metrische gegevens ophalen bij het uitvoeren van query's door de header `x-ms-documentdb-populatequerymetrics` in te stellen op `True`. Zie [Metrische gegevens van SQL-query's voor Azure Cosmos DB](sql-api-query-metrics.md) voor meer informatie.

### <a id="DotNetSdk"></a>C# (.NET) SDK

De .NET SDK biedt ondersteuning voor het uitvoeren van zowel SQL-query's als LINQ-query's. Het volgende voorbeeld laat zien hoe u de eerder in dit item geïntroduceerde filterquery uitvoert.
```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

In dit voorbeeld worden twee eigenschappen op gelijkheid binnen elk item vergeleken en anonieme projecties gebruikt.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Het volgende voorbeeld worden joins weergegeven, uitgedrukt via een LINQ-query met SelectMany.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

De .NET-client herhaalt de query automatisch voor alle pagina's met queryresultaten in de foreach-blokken, zoals hierboven wordt weergegeven. De queryopties die zijn geïntroduceerd in de sectie REST API zijn ook beschikbaar in de .NET-SDK via de klassen `FeedOptions` en `FeedResponse` in de methode CreateDocumentQuery. Het aantal pagina's kan worden beheerd met behulp van de instelling `MaxItemCount`.

U kunt ook expliciet paginering beheren met het maken van `IDocumentQueryable` met behulp van de `IQueryable` object, klikt u vervolgens met het lezen van de `ResponseContinuationToken` waarden en geven ze weer als `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery` kan worden ingesteld op scans in te schakelen wanneer de query kan niet worden ondersteund door het geconfigureerde beleid voor indexering. Voor gepartitioneerde containers kunt u `PartitionKey` gebruiken om de query uit te voeren op één partitie (ook al kan Azure Cosmos DB deze automatisch extraheren uit de querytekst), en `EnableCrossPartitionQuery` om query's uit te voeren die mogelijk op meerdere partities moeten worden uitgevoerd.

Raadpleeg de [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet) voor meer voorbeelden met query's.

### <a id="JavaScriptServerSideApi"></a>JavaScript-API op de server

Cosmos DB biedt een programmeermodel voor het rechtstreeks uitvoeren van JavaScript-toepassingslogica op de containers via opgeslagen procedures en triggers. De JavaScript-logica die is geregistreerd op containerniveau kan vervolgens databasebewerkingen uitvoeren op de bewerkingen voor de items van de opgegeven container. Deze bewerkingen worden verpakt in ambient ACID-transacties.

Het volgende voorbeeld laat zien hoe u queryDocuments in de JavaScript-server-API gebruikt om query's te maken op basis van opgeslagen procedures en triggers.

```javascript
    function businessLogic(name, author) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a id="References"></a>Naslaginformatie

1. [Inleiding tot Azure Cosmos DB][introduction]
2. [Azure Cosmos DB SQL-specificatie](https://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-cosmosdb-dotnet)
4. [Consistentieniveaus van Azure Cosmos DB][consistency-levels]
5. ANSI SQL 2011 [https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [https://json.org/](https://json.org/)
7. JavaScript-specificatie [https://www.ecma-international.org/publications/standards/Ecma-262.htm](https://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [https://msdn.microsoft.com/library/bb308959.aspx](https://msdn.microsoft.com/library/bb308959.aspx) 
9. Query-evaluatiemethoden voor grote databases [https://dl.acm.org/citation.cfm?id=152611](https://dl.acm.org/citation.cfm?id=152611)
10. Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.
13. G. Graefe. The Cascades framework for query optimization. IEEE Data Eng. Bull., 18(3): 1995.

[1]: ./media/how-to-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
