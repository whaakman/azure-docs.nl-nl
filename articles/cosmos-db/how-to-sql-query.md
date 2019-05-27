---
title: SQL-query's voor Azure Cosmos DB
description: Meer informatie over SQL-syntaxis, database-concepten en SQL-query's voor Azure Cosmos DB. Gebruik SQL als een Azure Cosmos DB-JSON-querytaal.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: bbca0239053b8f3164055a07b376abc597b0348f
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954132"
---
# <a name="sql-query-examples-for-azure-cosmos-db"></a>SQL-queryvoorbeelden voor Azure Cosmos DB

Azure Cosmos DB SQL API-accounts ondersteunen een query uitvoeren op items Structured Query Language (SQL) gebruiken als een querytaal voor JSON. De ontwerpdoelstellingen van de querytaal van Azure Cosmos DB zijn:

* Ondersteuning voor SQL, een van de meest vertrouwde en populaire querytalen, in plaats van een nieuwe querytaal vruchtbare. SQL biedt een formele programmeermodel voor uitgebreide query's via JSON-items.  

* JavaScript programmeermodel gebruiken als basis voor de querytaal. JavaScript typesysteem, evaluatie van de expressie en functieaanroepen zijn de hoofdmappen van de SQL-API. Deze hoofdmappen bieden een natuurlijk programmeermodel voor functies, zoals projecties van relationele, hiërarchische navigatie in JSON-items, zelf-joins, ruimtelijke query's, en het aanroepen van de gebruiker gedefinieerde functies (UDF's) die volledig in JavaScript geschreven.

In dit artikel doorloopt u een voorbeeld van de SQL-query's op eenvoudige JSON-objecten. Zie voor meer informatie over de syntaxis van de Azure Cosmos DB SQL-taal, [naslaginformatie over SQL-syntaxis](sql-api-query-reference.md).

## <a id="GettingStarted"></a>Aan de slag met SQL-query 's

Maak een container met de naam in uw SQL API Cosmos DB-account `Families`. Maak twee eenvoudige JSON-items in de container en een paar eenvoudige query's uitvoeren met betrekking tot deze.

### <a name="create-json-items"></a>JSON-items maken

De volgende code maakt twee eenvoudige JSON-items over families. De eenvoudige JSON-items voor de Andersen en Wakefield families bevatten ouders, kinderen en hun huisdieren, adres en registratie-informatie. Het eerste item heeft tekenreeksen, getallen, Booleaanse waarden, matrices en geneste eigenschappen.


```json
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
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Maakt gebruik van het tweede item `givenName` en `familyName` in plaats van `firstName` en `lastName`.

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
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>Query uitvoeren op de JSON-items

Probeer enkele query's op basis van de JSON-gegevens om te begrijpen van enkele van de belangrijkste aspecten van Azure Cosmos DB SQL-querytaal.

De volgende query retourneert de items waar de `id` veld komt overeen met `AndersenFamily`. Omdat het een `SELECT *` query, de uitvoer van de query is de volledige JSON-object. Zie voor meer informatie over de syntaxis van SELECT [SELECT-instructie](sql-api-query-reference.md#select-query). 

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Resultaten van de query zijn: 

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

De volgende query uit zet de JSON-uitvoer in een andere vorm. De query projecteert een nieuwe JSON `Family` object met twee geselecteerde velden, `Name` en `City`, wanneer de plaats hetzelfde als de status is. "NY, NY" komt overeen met deze aanvraag.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Resultaten van de query zijn:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

De volgende query retourneert alle opgegeven namen van kinderen in de familie waarvan `id` komt overeen met `WakefieldFamily`, geordende per stad.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

De resultaten zijn:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

De voorgaande voorbeelden tonen verschillende aspecten van de querytaal van Cosmos DB:  

* Omdat SQL API op JSON-waarden werkt, behandelt structuur vormgegeven entiteiten in plaats van rijen en kolommen. U kunt verwijzen naar de structuurknooppunten op elke willekeurige diepte zoals `Node1.Node2.Node3…..Nodem`, vergelijkbaar met de verwijzing tweedelige van `<table>.<column>` in ANSI SQL.

* Omdat de querytaal met gegevens zonder schema werkt, moet het typesysteem dynamisch zijn gebonden. Een expressie kan verschillende typen voor verschillende elementen opleveren. Het resultaat van een query is een geldige JSON-waarde, maar staat niet vast van een vast schema.  

* Azure Cosmos DB biedt alleen ondersteuning voor JSON-items. Het systeem en voor expressies zijn beperkt tot alleen bekommeren om JSON-typen. Zie voor meer informatie de [JSON-specificatie](https://www.json.org/).  

* Een Cosmos DB-container is een verzameling JSON-items zonder schema. De relaties binnen en tussen container items worden impliciet vastgelegd door containment, niet door de primaire sleutel en refererende sleutels relaties. Deze functie is belangrijk voor de intra-item wordt verderop in dit artikel besproken.

## <a id="SelectClause"></a>SELECT-component

Elke query bestaat uit een SELECT-component en een optionele FROM en WHERE-componenten, per ANSI SQL-standaarden. Normaal gesproken de bron in de component FROM is geïnventariseerd, en de component WHERE wordt een filter toegepast op de bron om op te halen van een subset van JSON-items. De component SELECT projecten vervolgens de vereiste JSON-waarden in de selectielijst. Zie voor meer informatie over de syntaxis van de [SELECT-instructie](sql-api-query-reference.md#select-query).

De volgende Selecteer voorbeeld retourneert query `address` van `Families` waarvan `id` komt overeen met `AndersenFamily`:

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

## <a id="EscapingReservedKeywords"></a>Accessor van eigenschap tussen aanhalingstekens
U kunt toegang tot eigenschappen met behulp van de eigenschap tussen aanhalingstekens operator []. Zo is `SELECT c.grade` bijvoorbeeld het equivalent van `SELECT c["grade"]`. Deze syntaxis is handig als u een eigenschap die spaties, speciale tekens bevat of heeft dezelfde naam als een SQL-trefwoord of gereserveerd woord.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

## <a name="nested-properties"></a>Geneste eigenschappen

Het volgende voorbeeld projecten twee eigenschappen van geneste `f.address.state` en `f.address.city`.

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

De resultaten zijn:

```json
    [{
      "state": "WA",
      "city": "Seattle"
    }]
```

## <a name="json-expressions"></a>JSON-expressies

Projectie biedt ook ondersteuning voor JSON-expressies, zoals wordt weergegeven in het volgende voorbeeld:

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

De resultaten zijn:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle",
        "name": "AndersenFamily"
      }
    }]
```

In het voorgaande voorbeeld de component SELECT moet maken van een JSON-object en omdat het voorbeeld geen sleutel bevat, de naam van de impliciete argument variabele maakt gebruik van de component `$1`. De volgende query retourneert twee argumentvariabelen die impliciete: `$1` en `$2`.

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

De resultaten zijn:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a id="ValueKeyword"></a>Sleutelwoord VALUE

Het sleutelwoord waarde biedt een manier om te retourneren van de JSON-waarde alleen. De onderstaande query retourneert bijvoorbeeld de scalaire expressie `"Hello World"` in plaats van `{$1: "Hello World"}`:

```sql
    SELECT VALUE "Hello World"
```

De volgende query retourneert de JSON-waarden zonder de `address` label:

```sql
    SELECT VALUE f.address
    FROM Families f
```

De resultaten zijn:

```json
    [
      {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan",
        "city": "NY"
      }
    ]
```

Het volgende voorbeeld laat zien hoe om terug te keren primitieve JSON-waarden (het knooppuntniveau van de JSON-structuur):


```sql
    SELECT VALUE f.address.state
    FROM Families f
```

De resultaten zijn:

```json
    [
      "WA",
      "NY"
    ]
```

## <a id="DistinctKeyword"></a>Sleutelwoord DISTINCT

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

## <a name="aliasing"></a>Aliasing

U kunt expliciet alias waarden in query's. Als een query twee eigenschappen met dezelfde naam heeft, gebruikt u aliasing een of beide van de eigenschappen wijzigen zodat ze in het verwachte resultaat bent disambiguated.

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

## <a id="FromClause"></a>FROM-component

De FROM (`FROM <from_specification>`)-component is optioneel, tenzij de bron is gefilterd of verderop in de query verwachte. Zie voor meer informatie over de syntaxis van de [van syntaxis](sql-api-query-reference.md#bk_from_clause). Een query zoals `SELECT * FROM Families` inventariseert de gehele `Families` container. U kunt ook de speciale ROOT-id gebruiken voor de container in plaats van de containernaam van de.

De FROM-component gebruikt de volgende regels per query:

* Voor de container kunt u een alias gebruiken, zoals `SELECT f.id FROM Families AS f` of gewoon `SELECT f.id FROM Families f`. Hier `f` is de alias voor `Families`. Als er is een optioneel trefwoord tot de alias van de id.  

* Eenmaal alias, de naam van de oorspronkelijke bron kan niet worden gekoppeld. Bijvoorbeeld, `SELECT Families.id FROM Families f` syntaxis is ongeldig omdat de id `Families` alias is en kan niet meer worden omgezet.  

* Eigenschappen van alle waarnaar wordt verwezen, moet volledig gekwalificeerd zijn, om te voorkomen dat een niet-eenduidige bindingen in de afwezigheid van strikt schema voldoet. Bijvoorbeeld, `SELECT id FROM Families f` syntaxis is ongeldig omdat de eigenschap `id` niet afhankelijk is.

### <a name="get-subitems-by-using-the-from-clause"></a>Subitems ophalen met behulp van de FROM-component

De FROM-component kan de bron naar een kleinere subset verminderen. Als u wilt inventariseren alleen een substructuur in elk item, kan de subroot de bron, vormen, zoals weergegeven in het volgende voorbeeld:

```sql
    SELECT *
    FROM Families.children
```

De resultaten zijn:

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

De voorgaande query gebruikt een matrix als de bron, maar u kunt ook een object gebruiken als de bron. De query rekening gehouden met een waarde van de JSON geldig, die is gedefinieerd in de bron voor opname in het resultaat. Het volgende voorbeeld zou uitsluiten `Families` die geen een `address.state` waarde.

```sql
    SELECT *
    FROM Families.address.state
```

De resultaten zijn:

```json
    [
      "WA",
      "NY"
    ]
```

## <a id="WhereClause"></a>WHERE-component

De optionele WHERE-component (`WHERE <filter_condition>`) voorwaarde(n) geeft aan dat de bron-JSON-items om de query voor het opnemen in resultaten voldoen moeten. Een JSON-item moet de opgegeven voorwaarden om te evalueren `true` worden overwogen voor het resultaat. De WHERE-component om te bepalen van de kleinste subset van de bronitems die deel van het resultaat uitmaken kunnen wordt gebruikt door de index-laag. Zie voor meer informatie over de syntaxis van de [waar syntaxis](sql-api-query-reference.md#bk_where_clause).

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

Het vorige voorbeeld bevatte een eenvoudige gelijkheidsquery. De SQL-API ondersteunt ook verschillende [scalaire expressies](#scalar-expressions). Binaire en unaire expressies worden het meest gebruikt. Eigenschapsverwijzingen vanaf het JSON-bronobject zijn ook geldige expressies.

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

### <a name="equality-and-comparison-operators"></a>Gelijkheids- en vergelijkingsoperatoren

In de volgende tabel ziet het resultaat van gelijkheidsvergelijkingen in de SQL-API voor elk paar JSON-typen.

| **Op** | **Undefined** | **Null** | **Boolean** | **Number** | **String** | **Object** | **Array** |
|---|---|---|---|---|---|---|---|
| **Undefined** | Niet gedefinieerd | Undefined | Undefined | Undefined | Undefined | Undefined | Niet gedefinieerd |
| **Null** | Niet gedefinieerd | **Ok** | Niet gedefinieerd | Undefined | Undefined | Undefined | Niet gedefinieerd |
| **Boolean** | Niet gedefinieerd | Niet gedefinieerd | **Ok** | Niet gedefinieerd | Undefined | Undefined | Niet gedefinieerd |
| **Number** | Niet gedefinieerd | Undefined | Niet gedefinieerd | **Ok** | Niet gedefinieerd | Undefined | Niet gedefinieerd |
| **String** | Niet gedefinieerd | Undefined | Undefined | Niet gedefinieerd | **Ok** | Niet gedefinieerd | Niet gedefinieerd |
| **Object** | Niet gedefinieerd | Undefined | Undefined | Undefined | Niet gedefinieerd | **Ok** | Niet gedefinieerd |
| **Array** | Niet gedefinieerd | Undefined | Undefined | Undefined | Undefined | Niet gedefinieerd | **Ok** |

Voor vergelijkingsoperators zoals `>`, `>=`, `!=`, `<`, en `<=`, vergelijking van de verschillende typen of tussen twee objecten of matrices produceert `Undefined`.  

Als het resultaat van de scalaire expressie `Undefined`, het item niet is opgenomen in het resultaat, omdat `Undefined` is niet gelijk aan `true`.

### <a name="logical-and-or-and-not-operators"></a>Logische operatoren (EN, OF en NIET)

Logische operatoren worden uitgevoerd op Booleaanse waarden. De volgende tabellen ziet de logische waarheid tabellen voor deze operators:

**Operator OF**

| OR | True | Onwaar | Niet gedefinieerd |
| --- | --- | --- | --- |
| True |True |True |True |
| Onwaar |True |Onwaar |Niet gedefinieerd |
| Niet gedefinieerd |True |Niet gedefinieerd |Undefined |

**Operator EN**

| EN | True | Onwaar | Niet gedefinieerd |
| --- | --- | --- | --- |
| True |True |Onwaar |Niet gedefinieerd |
| Onwaar |Onwaar |Onwaar |Onwaar |
| Niet gedefinieerd |Niet gedefinieerd |Onwaar |Niet gedefinieerd |

**Operator NIET**

| NIET |  |
| --- | --- |
| True |Onwaar |
| Onwaar |True |
| Niet gedefinieerd |Niet gedefinieerd |

## <a name="between-keyword"></a>Sleutelwoord BETWEEN

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

## <a name="in-keyword"></a>Sleutelwoord IN

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

## <a name="-operator"></a>* operator

De speciale operator * het gehele artikel projecten is. Als u deze operator gebruikt, moet dit het enige geprojecteerde veld zijn. Een query zoals `SELECT * FROM Families f` is geldig, maar `SELECT VALUE * FROM Families f` en `SELECT *, f.id FROM Families f` zijn niet geldig. De [eerst query in dit artikel](#query-the-json-items) gebruikt de * operator. 

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

## <a id="TopKeyword"></a>TOP-operator

Het sleutelwoord TOP retourneert de eerste `N` van de resultaten van de query in een niet-gedefinieerde volgorde. Als een best practice, gebruikt u boven met de component ORDER BY te beperken tot de eerste `N` aantal geordende waarden. Deze twee componenten combineren, is de enige manier om aan te geven zoals verwacht welke bovenste is van invloed op rijen.

U kunt boven gebruiken met een constante waarde, zoals in het volgende voorbeeld, of met de waarde van een variabele met geparameteriseerde query's. Zie voor meer informatie de [query's met parameters](#parameterized-queries) sectie.

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

## <a id="OrderByClause"></a>ORDER BY-component

Zoals in de ANSI SQL, kunt u een optionele ORDER BY-component opnemen in query's. Het optionele argument van de ASC of DESC geeft aan of ophalen van resultaten in oplopende of aflopende volgorde. ASC is de standaardinstelling.

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

## <a id="OffsetLimitClause"></a>De component OFFSET LIMIET

LIMIET voor de OFFSET is een optionele component overslaan en vervolgens een aantal waarden van de query uitvoeren. Het aantal OFFSET en het maximum aantal zijn vereist in de component OFFSET LIMIET.

Wanneer de LIMIET van OFFSET wordt gebruikt in combinatie met een component ORDER BY, wordt de resultatenset wordt geproduceerd door te doen overslaan en de geordende waarden ondernemen. Als er geen component ORDER BY wordt gebruikt, wordt deze resulteren in een deterministische volgorde van waarden.

Bijvoorbeeld, als volgt een query die de eerste waarde overslaat en retourneert de tweede waarde (in volgorde van de naam van het residente stad):

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

De resultaten zijn:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Hier volgt een query die de eerste waarde overslaat en de tweede waarde retourneert (zonder volgorde):

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

De resultaten zijn:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```




## <a name="scalar-expressions"></a>Scalaire expressies

De component SELECT biedt ondersteuning voor scalaire expressies zijn constanten, wiskundige expressies en logische expressies. Een scalaire expressie die u maakt gebruik van de volgende query uit:


```sql
    SELECT ((2 + 11 % 7)-2)/3
```

De resultaten zijn:

```json
    [{
      "$1": 1.33333
    }]
```

In de volgende query is het resultaat van de scalaire expressie die een Booleaanse waarde:


```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

De resultaten zijn:

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

Een belangrijke functie van de SQL-API is een matrix en object maken. Het vorige voorbeeld een nieuwe JSON-object gemaakt `AreFromSameCityState`. U kunt ook matrices, maken, zoals wordt weergegeven in het volgende voorbeeld:


```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

De resultaten zijn:

```json
    [
      {
        "CityState": [
          "Seattle",
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

De volgende SQL-query is een ander voorbeeld van het gebruik van matrix binnen in subquery's. Deze query haalt alle afzonderlijke opgegeven namen van kinderen in een matrix.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```


## <a id="Iteration"></a>Herhaling

De SQL-API biedt ondersteuning voor iteratie van JSON-matrices met een nieuwe constructie toegevoegd via het sleutelwoord in de bron van. In het volgende voorbeeld:

```sql
    SELECT *
    FROM Families.children
```

De resultaten zijn:

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

De volgende query iteratie van uitvoert `children` in de `Families` container. De uitvoermatrix wijkt af van de voorgaande query. In dit voorbeeld splitst `children`, en worden de resultaten samengevoegd in één matrix:  

```sql
    SELECT *
    FROM c IN Families.children
```

De resultaten zijn:

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

U kunt filteren op elke afzonderlijke vermelding van de matrix, meer, zoals wordt weergegeven in het volgende voorbeeld:

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

De resultaten zijn:

```json
    [{
      "givenName": "Lisa"
    }]
```

U kunt ook via het resultaat van een matrix iteratie samenvoegen. Bijvoorbeeld telt de volgende query het aantal onderliggende items tussen alle families:

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

De resultaten zijn:

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a id="Joins"></a>Samenvoegingen

In een relationele database, moeten worden samengevoegd in tabellen de logische overheidsinstelling voor het ontwerpen van genormaliseerde schema's. Daarentegen, de SQL-API maakt gebruik van het model gedenormaliseerd gegevens zonder schema items, dit de logische is equivalent van een *self-join*.

De taal die ondersteuning biedt voor de syntaxis van de `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`. Deze query retourneert een set met tuples met `N` waarden. Elke tuple heeft waarden die worden geproduceerd door alle containeraliassen te herhalen voor hun respectieve sets. Met andere woorden, deze query wordt uitgevoerd op het volledige vectorproduct van de sets die deelnemen aan de join.

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

## <a id="UserDefinedFunctions"></a>Gebruiker gedefinieerde functies (UDF's)

De SQL-API biedt ondersteuning voor de gebruiker gedefinieerde functies (UDF's). U kunt met scalaire UDF's, nul of meer argumenten worden doorgegeven en retourneert een resultaat één argument. De API wordt elk argument voor een geldige JSON-waarden worden gecontroleerd.  

De API een uitbreiding voor de SQL-syntaxis ter ondersteuning van aangepaste toepassingslogica met UDF's. U kunt UDF's registreren bij de SQL-API en ernaar te verwijzen in SQL-query's. De UDFs zijn speciaal ontworpen voor aanroepen vanuit query's. Als gevolg van hebt UDF's geen toegang tot het contextobject, zoals andere JavaScript-typen, zoals opgeslagen procedures en triggers. Query's zijn alleen-lezen en kunnen worden uitgevoerd op de primaire of secundaire replica's. UDF's, in tegenstelling tot andere typen JavaScript zijn ontworpen om uit te voeren op secundaire replica's.

Het volgende voorbeeld wordt een UDF onder de itemcontainer van een in de Cosmos DB-database geregistreerd. Het voorbeeld wordt een UDF met de naam `REGEX_MATCH`. De cmdlet accepteert twee waarden voor JSON-tekenreeks, `input` en `pattern`, en controleert of het eerste komt overeen met het patroon die zijn opgegeven in de tweede met behulp van JavaScript `string.match()` functie.

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

Gebruik deze UDF nu in de projectie van een query. Moet u in aanmerking komt UDF's met het voorvoegsel hoofdlettergevoelig `udf.` bij het aanroepen van ze van binnen query's.

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

De resultaten zijn:

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

U kunt de UDF gekwalificeerd met de `udf.` voorvoegsel binnen een filter, zoals in het volgende voorbeeld:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

De resultaten zijn:

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

UDF's zijn in wezen geldige scalaire expressies die u in zowel projecties en filters gebruiken kunt.

Als u wilt uitbreiden op de kracht van UDF's, een ander voorbeeld kijken met voorwaardelijke logica:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'Seattle':
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

De UDF gebruik maakt van het volgende voorbeeld:

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

De resultaten zijn:

```json
     [
      {
        "city": "Seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Als de eigenschappen waarnaar wordt verwezen door de UDF parameters niet beschikbaar zijn in de JSON-waarde, wordt de parameter wordt beschouwd als niet-gedefinieerde en de UDF-aanroep is overgeslagen. Op dezelfde manier als het resultaat van de UDF gedefinieerd is, het niet opgenomen in het resultaat.

Als de voorgaande voorbeelden laten zien, integreren UDF's in de kracht van JavaScript-taal de SQL-API. UDF's bieden een rijke programmeerbare interface hiervoor complexe procedurele, voorwaardelijke logica aan de hand van de ingebouwde mogelijkheden voor JavaScript-runtime. De SQL-API biedt de argumenten voor de UDF's voor elk Bronitem op de huidige locatie of de component SELECT fase van de verwerking. Het resultaat wordt naadloos opgenomen in de hele pijplijn. Kortom zijn UDF's geweldige hulpprogramma's voor complexe bedrijfslogica doen als onderdeel van query's.

## <a id="Aggregates"></a>Statistische functies

Statistische functies een berekening uitgevoerd op een set waarden in de component SELECT en één waarde retourneren. De volgende query retourneert bijvoorbeeld het aantal items in de `Families` container:

```sql
    SELECT COUNT(1)
    FROM Families f
```

De resultaten zijn:

```json
    [{
        "$1": 2
    }]
```

Ook kunt u alleen de scalaire waarde van de statistische functie met behulp van het sleutelwoord waarde retourneren. De volgende query retourneert bijvoorbeeld het aantal waarden als één getal:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

De resultaten zijn:

```json
    [ 2 ]
```

U kunt ook aggregaties combineren met filters. De volgende query retourneert bijvoorbeeld het aantal items met de adresstatus van `WA`.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

De resultaten zijn:

```json
    [ 1 ]
```

De SQL-API biedt ondersteuning voor de volgende statistische functies. SOM en gemiddelde werken op basis van numerieke waarden en het aantal, MIN en MAX werken op getallen, tekenreeksen, Booleaanse waarden en null-waarden.

| Function | Description |
|-------|-------------|
| AANTAL | Retourneert het aantal items in de expressie. |
| SUM   | Retourneert de som van alle waarden in de expressie. |
| MIN   | Retourneert de minimumwaarde in de expressie. |
| MAX   | Retourneert de maximumwaarde in de expressie. |
| GEM   | Retourneert het gemiddelde van de waarden in de expressie. |

U kunt ook via de resultaten van een matrix iteratie samenvoegen. Zie voor meer informatie de [iteratie](#Iteration) sectie.

> [!NOTE]
> In de Azure portal Data Explorer kunnen aggregatie-query's gedeeltelijke resultaten samenvoegen op slechts één query op pagina. De SDK produceert één cumulatieve waarde op alle pagina's. Als u wilt uitvoeren van aggregatie-query's met behulp van code, moet u de .NET SDK 1.12.0, .NET Core SDK 1.1.0 of Java SDK 1.9.5 of hoger.
>

## <a id="BuiltinFunctions"></a>Ingebouwde functies

Cosmos DB ondersteunt ook een aantal ingebouwde functies voor algemene bewerkingen, die u in query's zoals de gebruiker gedefinieerde functies (UDF's gebruiken kunt).

| Functiegroep | Bewerkingen |
|---------|----------|
| Wiskundige functies | ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, TAN |
| Controle van het type functies | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, IS_PRIMITIVE |
| Tekenreeksfuncties | CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, UPPER |
| Matrixfuncties | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH en ARRAY_SLICE |
| Ruimtelijke functies | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, ST_ISVALIDDETAILED |

Als u momenteel van een gebruiker gedefinieerde functie (UDF's gebruikmaakt) waarvoor een ingebouwde functie nu beschikbaar is, worden de bijbehorende ingebouwde functie om uit te voeren sneller en efficiënter.

Het belangrijkste verschil tussen de Cosmos DB en ANSI SQL-functies is dat Cosmos DB-functies zijn ontworpen om te werken goed met gegevens zonder schema en een gemengde-schema. Bijvoorbeeld, als een eigenschap ontbreekt of heeft een niet-numerieke waarde, zoals `unknown`, het item is overgeslagen in plaats van een fout geretourneerd.

### <a name="mathematical-functions"></a>Wiskundige functies

Met elke wiskundige functie wordt een berekening op basis van de opgegeven invoerwaarden uitgevoerd en een numerieke waarde geretourneerd. Dit is een tabel met ondersteunde ingebouwde wiskundige functies.

| Gebruik | Description |
|----------|--------|
| ABS (num_expr) | Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie. |
| CEILING (num_expr) | Retourneert het kleinste gehele getal dat groter is dan of gelijk is aan de opgegeven numerieke expressie. |
| FLOOR (num_expr) | Retourneert het grootste gehele getal dat kleiner is dan of gelijk is aan de opgegeven numerieke expressie. |
| EXP (num_expr) | Retourneert de exponent van de opgegeven numerieke expressie. |
| LOG (num_expr, base) | Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie, of de logaritme met behulp van het opgegeven grondtal. |
| LOG10 (num_expr) | Retourneert de waarde van de logaritme met het grondtal 10 van de opgegeven numerieke expressie. |
| ROUND (num_expr) | Retourneert een numerieke waarde, afgerond naar het dichtstbijzijnde gehele getal. |
| TRUNC (num_expr) | Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal. |
| SQRT (num_expr) | Retourneert de vierkantswortel van de opgegeven numerieke expressie. |
| SQUARE (num_expr) | Retourneert het kwadraat van de opgegeven numerieke expressie. |
| POWER (num_expr, num_expr) | Retourneert de machtsverheffing met de opgegeven waarde van de opgegeven numerieke expressie. |
| SIGN (num_expr) | Retourneert de waarde voor het teken (-1, 0, 1) van de opgegeven numerieke expressie. |
| ACOS (num_expr) | Retourneert de hoek, in radialen, waarvan de cosinus de opgegeven numerieke expressie is. Dit wordt ook wel de arccosinus genoemd. |
| ASIN (num_expr) | Retourneert de hoek, in radialen, waarvan de sinus de opgegeven numerieke expressie is. Deze functie wordt ook wel de arcsinus genoemd. |
| ATAN (num_expr) | Retourneert de hoek, in radialen, waarvan de tangens de opgegeven numerieke expressie is. Deze functie wordt ook de boogtangens genoemd. |
| ATN2 (num_expr) | Retourneert de hoek, in radialen, tussen de positieve x-as en de straal vanaf de oorsprong tot het punt (y, x), waarbij x en y de waarden van de twee opgegeven float-expressies zijn. |
| COS (num_expr) | Retourneert de trigonometrische cosinus van de opgegeven hoek, in radialen, in de opgegeven expressie. |
| COT (num_expr) | Retourneert de trigonometrische cotangens van de opgegeven hoek, in radialen, in de opgegeven expressie. |
| DEGREES (num_expr) | Retourneert de overeenkomende hoek in graden voor een hoek die is opgegeven in radialen. |
| PI () | Retourneert de constante waarde van PI. |
| RADIANS (num_expr) | Retourneert radialen als een numerieke expressie in graden wordt ingevoerd. |
| SIN (num_expr) | Retourneert de trigonometrische sinus van de opgegeven hoek, in radialen, in de opgegeven expressie. |
| TAN (num_expr) | Retourneert de tangens van de ingevoerde expressie, in de opgegeven expressie. |

U kunt query's zoals het volgende voorbeeld kunt uitvoeren:

```sql
    SELECT VALUE ABS(-4)
```

Het resultaat is:

```json
    [4]
```

### <a name="type-checking-functions"></a>Controle van het type functies

De controle van het type functies kunnen u controleert u het type van een expressie in een SQL-query. Controle van het type functies kunt u bepalen welke typen eigenschappen in de items op elk gewenst moment, wanneer de variabele of onbekend. Hier volgt een lijst met ondersteunde ingebouwde controle van het type functies:

| **Gebruik** | **Beschrijving** |
|-----------|------------|
| [IS_ARRAY (expr)](sql-api-query-reference.md#bk_is_array) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een matrix is. |
| [IS_BOOL (expr)](sql-api-query-reference.md#bk_is_bool) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een Booleaanse waarde is. |
| [IS_NULL (expr)](sql-api-query-reference.md#bk_is_null) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde null is. |
| [IS_NUMBER (expr)](sql-api-query-reference.md#bk_is_number) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een getal is. |
| [IS_OBJECT (expr)](sql-api-query-reference.md#bk_is_object) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een JSON-object is. |
| [IS_STRING (expr)](sql-api-query-reference.md#bk_is_string) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks is. |
| [IS_DEFINED (expr)](sql-api-query-reference.md#bk_is_defined) | Retourneert een Booleaanse waarde die aangeeft of aan de eigenschap een waarde is toegewezen. |
| [IS_PRIMITIVE (expr)](sql-api-query-reference.md#bk_is_primitive) | Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks, getal, Booleaanse waarde of null zijn is. |

Met behulp van deze functies, kunt u query's zoals het volgende voorbeeld uitvoert:

```sql
    SELECT VALUE IS_NUMBER(-4)
```

Het resultaat is:

```json
    [true]
```

### <a name="string-functions"></a>Tekenreeksfuncties

De volgende scalaire functies uitvoeren van een bewerking op een tekenreekswaarde voor invoer en retourneert een tekenreeks, numerieke of Booleaanse waarde. Hier volgt een tabel met ingebouwde tekenreeksfuncties:

| Gebruik | Description |
| --- | --- |
| [LENGTH (str_expr)](sql-api-query-reference.md#bk_length) | Retourneert het aantal tekens van de opgegeven tekenreeksexpressie. |
| [CONCAT (str_expr, str_expr [, str_expr])](sql-api-query-reference.md#bk_concat) | Retourneert een tekenreeks die het resultaat is van het samenvoegen van twee of meer tekenreekswaarden. |
| [SUBSTRING (str_expr, num_expr, num_expr)](sql-api-query-reference.md#bk_substring) | Retourneert een deel van een tekenreeksexpressie. |
| [STARTSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_startswith) | Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks begint met de tweede. |
| [ENDSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_endswith) | Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks eindigt met de tweede. |
| [CONTAINS (str_expr, str_expr)](sql-api-query-reference.md#bk_contains) | Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie de tweede bevat. |
| [INDEX_OF (str_expr, str_expr)](sql-api-query-reference.md#bk_index_of) | Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste expressie in de opgegeven tekenreeks, of -1 als de tekenreeks is niet gevonden. |
| [LEFT (str_expr, num_expr)](sql-api-query-reference.md#bk_left) | Retourneert het linkerdeel van een tekenreeks met het opgegeven aantal tekens. |
| [RIGHT (str_expr, num_expr)](sql-api-query-reference.md#bk_right) | Retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens. |
| [LTRIM (str_expr)](sql-api-query-reference.md#bk_ltrim) | Retourneert een tekenreeksexpressie na het verwijderen van lege voorlooptekens. |
| [RTRIM (str_expr)](sql-api-query-reference.md#bk_rtrim) | Retourneert een tekenreeksexpressie na het afkappen van alle lege volgtekens. |
| [LOWER (str_expr)](sql-api-query-reference.md#bk_lower) | Retourneert een tekenreeksexpressie na het converteren van tekens in hoofdletters naar kleine letters. |
| [UPPER (str_expr)](sql-api-query-reference.md#bk_upper) | Retourneert een tekenreeksexpressie na het converteren van tekens in kleine letters naar hoofdletters. |
| [REPLACE (str_expr, str_expr, str_expr)](sql-api-query-reference.md#bk_replace) | Vervangt alle exemplaren van een opgegeven tekenreekswaarde door een andere tekenreekswaarde. |
| [REPLICATE (str_expr, num_expr)](sql-api-query-reference.md#bk_replicate) | Herhaalt een tekenreekswaarde een opgegeven aantal keren. |
| [REVERSE (str_expr)](sql-api-query-reference.md#bk_reverse) | Retourneert een tekenreekswaarde in de omgekeerde volgorde. |

Met behulp van deze functies, kunt u query's, zoals het volgende voorbeeld, waarin de familie retourneert uitvoeren `id` in hoofdletters:

```sql
    SELECT VALUE UPPER(Families.id)
    FROM Families
```

De resultaten zijn:

```json
    [
        "WAKEFIELDFAMILY",
        "ANDERSENFAMILY"
    ]
```

Of samenvoegen van tekenreeksen, zoals in dit voorbeeld:

```sql
    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families
```

De resultaten zijn:

```json
    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "Seattle,WA"
    }]
```

U kunt ook de tekenreeksfuncties in de WHERE-component voor het filteren van, zoals in het volgende voorbeeld resultaten gebruiken:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")
```

De resultaten zijn:

```json
    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]
```

### <a name="array-functions"></a>Matrixfuncties

De volgende scalaire functies uitvoeren van een bewerking op een invoerwaarde voor de matrix en een numerieke waarde, Booleaanse waarde of matrix-waarde retourneren. Hier volgt een tabel met ingebouwde matrixfuncties:

| Gebruik | Description |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](sql-api-query-reference.md#bk_array_length) |Retourneert het aantal elementen van de opgegeven matrixexpressie. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](sql-api-query-reference.md#bk_array_concat) |Retourneert een matrix die het resultaat is van het samenvoegen van twee of meer matrixwaarden. |
| [ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](sql-api-query-reference.md#bk_array_contains) |Retourneert een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat. U kunt kiezen tussen een volledige of gedeeltelijke overeenkomst. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](sql-api-query-reference.md#bk_array_slice) |Retourneert een deel van een matrixexpressie. |

Matrixfuncties gebruiken voor het bewerken van matrices in JSON. Bijvoorbeeld, als volgt een query alle item retourneert `id`s wanneer één van de `parents` is `Robin Wakefield`: 

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })
```

Het resultaat is:

```json
    [{
      "id": "WakefieldFamily"
    }]
```

U kunt een gedeeltelijk fragment opgeven voor het vergelijken van elementen in de matrix. De volgende query worden alle `id`die `parents` met de `givenName` van `Robin`:

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)
```

Het resultaat is:

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Hier volgt een voorbeeld waarin ARRAY_LENGTH wordt gebruikt voor het aantal `children` per serie:

```sql
    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 
```

De resultaten zijn:

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

Cosmos DB ondersteunt de volgende Open georuimtelijke Consortium (OGC) ingebouwde functies voor georuimtelijke query's: 

| Gebruik | Description |
| --- | --- |
| ST_DISTANCE (point_expr, point_expr) | Retourneert de afstand tussen de twee GeoJSON `Point`, `Polygon`, of `LineString` expressies. |
| T_WITHIN (point_expr, polygon_expr) | Retourneert een Booleaanse expressie die aangeeft of het eerste GeoJSON-object (`Point`, `Polygon`, of `LineString`) is in het tweede GeoJSON-object (`Point`, `Polygon`, of `LineString`). |
| ST_INTERSECTS (spatial_expr, spatial_expr) | Retourneert een Booleaanse expressie waarmee wordt aangegeven of de twee GeoJSON-objecten opgegeven (`Point`, `Polygon`, of `LineString`) elkaar overlappen. |
| ST_ISVALID | Retourneert een Booleaanse waarde die aangeeft of de opgegeven GeoJSON `Point`, `Polygon`, of `LineString` expressie is ongeldig. |
| ST_ISVALIDDETAILED | Retourneert een JSON-waarde met een Boolean-waarde als de opgegeven GeoJSON `Point`, `Polygon`, of `LineString` -expressie is geldig, en als ongeldig, de reden als een string-waarde. |

U kunt ruimtelijke functies gebruiken om uit te voeren van de service-query's op ruimtelijke gegevens. Bijvoorbeeld, als volgt een query waarmee alle familie items die binnen 30 kilometer van een opgegeven locatie met behulp van de ingebouwde functie ST_DISTANCE geretourneerd:

```sql
    SELECT f.id
    FROM Families f
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

Het resultaat is:

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Zie [Werken met georuimtelijke gegevens in Azure Cosmos DB](geospatial.md) voor meer informatie over ondersteuning voor georuimtelijke gegevens. 

## <a name="parameterized-queries"></a>Geparameteriseerde query 's

Cosmos DB biedt ondersteuning voor query's met parameters worden uitgedrukt met de vertrouwde @ notatie. Geparameteriseerde SQL biedt robuuste verwerking en aanhalingstekens invoer van de gebruiker en wordt voorkomen dat onbedoelde blootstelling van gegevens via SQL-injectie.

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

## <a id="JavaScriptIntegration"></a>JavaScript-integratie

Azure Cosmos DB biedt een programmeermodel voor het uitvoeren van JavaScript-gebaseerde toepassingslogica rechtstreeks op containers, met behulp van opgeslagen procedures en triggers. Dit model biedt ondersteuning voor:

* Krachtige transactionele CRUD-bewerkingen en query's voor items in een container, omdat de diepe integratie van de JavaScript-runtime binnen de database-engine.
* Een natuurlijke modellering van Controlestroom, variabele scopes en -toewijzing en integratie van primitieven databasetransacties afhandeling van uitzonderingen. 

Zie voor meer informatie over de integratie van Azure Cosmos DB JavaScript de [JavaScript-API van server-side](#JavaScriptServerSideApi) sectie.

### <a name="operator-evaluation"></a>Evaluatie van operatoren

Cosmos DB, omdat een JSON-database wordt getekend parallels met JavaScript-operators en semantiek voor evaluatie. Cosmos DB probeert te behouden van JavaScript-semantiek in termen van JSON-ondersteuning, maar de evaluatie van de bewerking in sommige gevallen afwijkt.

In de SQL-API, zijn in tegenstelling tot in traditionele SQL, de typen van waarden vaak niet bekend totdat de API de waarden uit de database haalt. Om query's efficiënt te kunnen uitvoeren, gelden er strikte vereisten voor typen voor de meeste operatoren.

In tegenstelling tot JavaScript uitvoeren niet de SQL-API impliciete conversies. Bijvoorbeeld, een query zoals `SELECT * FROM Person p WHERE p.Age = 21` komt overeen met items die bevatten een `Age` waarvan de waarde is de eigenschap `21`. Deze niet overeenkomt met een ander object waarvan de `Age` eigenschap komt overeen met mogelijk oneindige variaties zoals `twenty-one`, `021`, of `21.0`. Dit in tegenstelling tot met JavaScript, waar tekenreekswaarden impliciet worden geconverteerd naar getallen op basis van de operator, bijvoorbeeld: `==`. Dit gedrag SQL-API is van cruciaal belang voor het afstemmen van efficiënte index.

## <a id="ExecutingSqlQueries"></a>De uitvoering van de SQL-query

Elke taal waarmee HTTP/HTTPS-aanvragen kan het Cosmos DB REST-API aanroepen. Cosmos DB biedt ook programmeringsbibliotheken geboden voor .NET, Node.js, JavaScript en Python programmeertalen. De REST-API en bibliotheken ondersteuning voor het uitvoeren van query's via SQL en de .NET SDK biedt ook ondersteuning voor [LINQ uitvoeren van query's](#Linq).

De volgende voorbeelden laten zien hoe u een query maakt en verzendt met een Cosmos DB-databaseaccount.

### <a id="RestAPI"></a>REST API

Cosmos DB biedt een open RESTful-programmeermodel via HTTP. Het resourcemodel dat bestaat uit een set met resources onder een databaseaccount,, die een bepaalde Azure-abonnement. Account van de database bestaat uit een reeks *databases*, die elk meerdere mag *containers*, die op zijn beurt bevatten *items*, UDF's en andere resourcetypen. Elke Cosmos DB-resource is opgevraagd met de URI van een logische en stabiel. Een set met resources heet een *feed*. 

Het model basic interactie met deze resources is via het HTTP-termen `GET`, `PUT`, `POST`, en `DELETE`, met hun standaard interpretaties. Gebruik `POST` voor het maken van een nieuwe resource, een opgeslagen procedure uitvoeren of een Cosmos DB-query. Query's zijn altijd alleen-lezen bewerkingen zonder neveneffecten.

De volgende voorbeelden ziet u een `POST` voor een SQL-API-query op de voorbeelditems. De query is een eenvoudige filter op de JSON- `name` eigenschap. De `x-ms-documentdb-isquery` en Content-Type: `application/query+json` headers duiden dat de bewerking een query wordt. Vervang `mysqlapicosmosdb.documents.azure.com:443` met de URI voor uw Cosmos DB-account.

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
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

De resultaten zijn:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

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
                "city":"Seattle"
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

De volgende complexere query retourneert verschillende resultaten van een join:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
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

De resultaten zijn: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

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

Als de resultaten van een query niet op één pagina passen, de REST-API retourneert een vervolgtoken via de `x-ms-continuation-token` response-header. Clients kunnen resultaten pagineren door de header op te nemen in de volgende resultaten. U kunt ook bepalen het aantal resultaten per pagina via de `x-ms-max-item-count` getal-header. 

Als een query een statistische functie, zoals het aantal bevat, kan de querypagina een gedeeltelijk geaggregeerde waarde via slechts één pagina met resultaten geretourneerd. Clients moeten een aggregatie op het tweede niveau uitvoeren via deze resultaten voor het produceren van de laatste resultaten. Bijvoorbeeld, som via de aantallen die worden geretourneerd in de afzonderlijke pagina's om de totale telling te retourneren.

Voor het beheren van het beleid van de consistentie van gegevens voor query's, gebruikt u de `x-ms-consistency-level` header in alle REST API-aanvragen. Sessieconsistentie is ook vereist de meest recente echo `x-ms-session-token` cookie-header in de query-aanvragen. Het indexeringsbeleid van de container waarop de query wordt uitgevoerd kan ook van invloed zijn op de consistentie van queryresultaten. Met de standaardbeleidsregels voor indexering beleidsinstellingen voor containers, de index is altijd actueel zijn met de inhoud van de items en queryresultaten overeenkomen met de consistentie gekozen voor gegevens. Zie voor meer informatie, [Azure Cosmos DB-consistentieniveaus][consistency-levels].

Als de opgegeven query kan niet worden ondersteund door het geconfigureerde indexeringsbeleid voor de container, retourneert de Azure Cosmos DB-server 400 'Ongeldige aanvraag". Dit foutbericht wordt geretourneerd voor query's met paden expliciet is uitgesloten van het indexeren. U kunt opgeven de `x-ms-documentdb-query-enable-scan` header om toe te staan van de query voor het uitvoeren van een scan wanneer een index is niet beschikbaar.

U kunt gedetailleerde metrische gegevens krijgen bij uitvoeren van query's door in te stellen de `x-ms-documentdb-populatequerymetrics` koptekst `true`. Zie [Metrische gegevens van SQL-query's voor Azure Cosmos DB](sql-api-query-metrics.md) voor meer informatie.

### <a id="DotNetSdk"></a>C# (.NET SDK)

De .NET SDK biedt ondersteuning voor het uitvoeren van zowel SQL-query's als LINQ-query's. Het volgende voorbeeld ziet hoe u de voorgaande filterquery met .NET uitvoert:

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

Het volgende voorbeeld worden twee eigenschappen voor gelijkheid binnen elk item vergeleken en anonieme projecties gebruikt.

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

Het volgende voorbeeld toont joins, uitgedrukt via LINQ `SelectMany`.

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

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

Alle pagina's van de resultaten van de query in de .NET-client automatisch doorloopt de `foreach` wordt geblokkeerd, zoals wordt weergegeven in het voorgaande voorbeeld. De opties voor query's die zijn geïntroduceerd in de [REST-API](#RestAPI) sectie zijn ook beschikbaar in de .NET SDK, met behulp van de `FeedOptions` en `FeedResponse` klassen in de `CreateDocumentQuery` methode. U kunt het aantal pagina's beheren met behulp van de `MaxItemCount` instelling.

U kunt ook expliciet paginering beheren met het maken van `IDocumentQueryable` met behulp van de `IQueryable` object, klikt u vervolgens met het lezen van de `ResponseContinuationToken` waarden en geven ze weer als `RequestContinuationToken` in `FeedOptions`. U kunt instellen `EnableScanInQuery` scans inschakelen wanneer de query wordt niet ondersteund door het geconfigureerde beleid voor indexering. Voor gepartitioneerde containers, kunt u `PartitionKey` aan de query uitvoeren op een enkele partitie, hoewel Azure Cosmos DB dit automatisch uit de tekst van de query ophalen kunt. U kunt `EnableCrossPartitionQuery` query's uitvoeren op basis van meerdere partities.

Zie voor meer voorbeelden van .NET met query's, de [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-cosmosdb-dotnet) in GitHub.

### <a id="JavaScriptServerSideApi"></a>JavaScript-API op de server

Cosmos DB biedt een programmeermodel voor het uitvoeren van op basis van JavaScript-toepassingslogica rechtstreeks op containers, met behulp van opgeslagen procedures en triggers. De JavaScript-logica die is geregistreerd op het niveau van de container kan vervolgens databasebewerkingen uitvoeren op de items van de opgegeven container, verpakt in ambient ACID-transactions.

Het volgende voorbeeld ziet u hoe u `queryDocuments` op de server in JavaScript API voor het maken van query's uit binnen opgeslagen procedures en triggers:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a id="Linq"></a>LINQ to SQL-API

LINQ is een .NET-programmeermodel die berekening als query's voor object-streams. Cosmos DB biedt een clientbibliotheek voor de interactie met LINQ door het faciliteren van een conversie tussen JSON- en .NET-objecten en het toewijzen van een subset van LINQ-query's aan Cosmos DB-query's.

Het volgende diagram toont de architectuur van het LINQ-query's met Cosmos DB ondersteunt. Met de Cosmos DB-client, kunt u een `IQueryable` -object dat rechtstreeks query de Cosmos DB-provider voor query's en zet de LINQ-query in een Cosmos DB-query. U geeft de query naar de Cosmos DB-server, die worden opgehaald van een set resultaten in JSON-indeling. De JSON-deserializer converteert de resultaten naar een stream van .NET-objecten aan de clientzijde.

![Architectuur voor ondersteunende LINQ-query's met de SQL-API - SQL-syntaxis, JSON-querytaal, databaseconcepten en SQL-query's][1]

### <a name="net-and-json-mapping"></a>.NET- en JSON-toewijzing

De toewijzing tussen .NET-objecten en JSON-items is natuurlijk. Elk gegevensveld lid worden toegewezen aan een JSON-object, waarbij de naam van het veld wordt toegewezen aan de *sleutel* onderdeel van het object en de waarde recursief wordt toegewezen aan de *waarde* deel uitmaakt van het object. De volgende code maps de `Family` klasse in een JSON-item en maakt vervolgens een `Family` object:

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

Het vorige voorbeeld maakt u de volgende JSON-object:

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

De Cosmos DB-queryprovider doet een zo goed mogelijke poging tot omzetting van een LINQ-query in een SQL-query voor Cosmos DB. De volgende beschrijving wordt ervan uitgegaan dat een enigszins bekend bent met LINQ.

De query-provider typesysteem ondersteunt alleen de JSON primitieve typen: numerieke, Boolean, string en null. 

De queryprovider ondersteunt de volgende scalaire expressies:

- Constante waarden, met inbegrip van constante waarden van de primitieve gegevenstypen op query-evaluatie.
  
- Eigenschap/matrix indexexpressies die naar de eigenschap van een object of een element van de matrix verwijzen. Bijvoorbeeld:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Wiskundige expressies, met inbegrip van veelgebruikte wiskundige expressies op basis van numerieke en Booleaanse waarden. Zie voor de volledige lijst de [Azure Cosmos DB SQL-specificatie](https://go.microsoft.com/fwlink/p/?LinkID=510612).
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Vergelijking van de expressies voor verbindingsreeksen, waaronder een string-waarde naar een constante tekenreeks-waarde te vergelijken.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Object/matrix maken van expressies, die een object van samengestelde waarde of een anoniem type of een matrix van dergelijke objecten retourneren. U kunt deze waarden nesten.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

### <a id="SupportedLinqOperators"></a>Ondersteunde LINQ-operators

Het LINQ-provider opgenomen in de SQL-SDK voor .NET ondersteunt de volgende operators:

- **Select**: Projecties vertalen naar SQL selecteert, met inbegrip van objectconstructie.
- **Where**: Filters vertalen in waar SQL en ondersteuning voor de conversie van `&&`, `||`, en `!` voor de SQL-operators
- **SelectMany**: maakt het mogelijk om matrices af te wikkelen naar de SQL-JOIN-component. Gebruiken om te koppelen of expressies te filteren op matrixelementen nesten.
- **OrderBy** en **OrderByDescending**: ORDER BY met ASC of DESC vertaald.
- Operatoren **Count**, **Sum**, **Min**, **Max** en **Average** voor statistische functies en de bijbehorende asynchrone equivalenten **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync** en **AverageAsync**.
- **CompareTo**: wordt omgezet naar bereikvergelijkingen. Doorgaans gebruikt voor tekenreeksen, omdat ze niet vergelijkbaar in .NET.
- **Take**: Wordt omgezet in het SQL-bovenaan voor het beperken van de resultaten van een query.
- **Wiskundige functies**: Biedt ondersteuning voor vertaling van .NET `Abs`, `Acos`, `Asin`, `Atan`, `Ceiling`, `Cos`, `Exp`, `Floor`, `Log`, `Log10`, `Pow`, `Round`, `Sign`, `Sin`, `Sqrt`, `Tan`, en `Truncate` naar de equivalente ingebouwde functies van SQL.
- **Functies de tekenreeks**: Biedt ondersteuning voor vertaling van .NET `Concat`, `Contains`, `Count`, `EndsWith`,`IndexOf`, `Replace`, `Reverse`, `StartsWith`, `SubString`, `ToLower`, `ToUpper`, `TrimEnd`, en `TrimStart` naar de equivalente ingebouwde functies van SQL.
- **Matrix van functies**: Biedt ondersteuning voor vertaling van .NET `Concat`, `Contains`, en `Count` naar de equivalente ingebouwde functies van SQL.
- **Extensie voor georuimtelijke functies**: Biedt ondersteuning voor vertaling van stub-methoden `Distance`, `IsValid`, `IsValidDetailed`, en `Within` naar de equivalente ingebouwde functies van SQL.
- **De gebruiker gedefinieerde functie extensie functie**: Biedt ondersteuning voor omzetting van de methode stub `UserDefinedFunctionProvider.Invoke` naar de bijbehorende door de gebruiker gedefinieerde functie.
- **Miscellaneous**: Biedt ondersteuning voor vertaling van `Coalesce` en voorwaardelijke operators. Kan vertalen `Contains` tekenreeks bevat, ARRAY_CONTAINS of SQL IN, afhankelijk van context.

### <a name="sql-query-operators"></a>Operatoren voor SQL-query's

De volgende voorbeelden laten zien hoe sommige van de standaardoperators voor LINQ-query wordt omgezet naar Cosmos DB-query's.

#### <a name="select-operator"></a>Operator selecteren

De syntaxis is `input.Select(x => f(x))`. Hierbij is `f` een scalaire expressie.

**Selecteer de operator, voorbeeld 1:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**Selecteer de operator, voorbeeld 2:** 

- **LINQ lambda-expressie**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**Selecteer de operator, voorbeeld 3:**

- **LINQ lambda-expressie**
  
  ```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE {"name":f.children[0].familyName,
                    "grade": f.children[0].grade + 3 }
      FROM Families f
  ```

#### <a name="selectmany-operator"></a>Operator SelectMany

De syntaxis is `input.SelectMany(x => f(x))`. Hierbij is `f` een scalaire expressie die een containertype retourneert.

- **LINQ lambda-expressie**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

#### <a name="where-operator"></a>Operator Where

De syntaxis is `input.Where(x => f(x))`. Hierbij is `f` een scalaire expressie die een Booleaanse waarde retourneert.

**Waar operator, voorbeeld 1:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.Where(family=> family.parents[0].familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```
  
**Waar operator, voorbeeld 2:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.Where(
          family => family.parents[0].familyName == "Wakefield" &&
          family.children[0].grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
      AND f.children[0].grade < 3
  ```

### <a name="composite-sql-queries"></a>Samengestelde SQL-query's

U kunt de voorgaande operators om krachtigere query's maken. U kunt omdat Cosmos DB biedt ondersteuning voor geneste containers, samenvoegen of nesten van de samenstelling.

#### <a name="concatenation"></a>Samenvoegen

De syntaxis is `input(.|.SelectMany())(.Select()|.Where())*`. Een samengevoegde query kunt beginnen met een optionele `SelectMany` query, gevolgd door meerdere `Select` of `Where` operators.

**Selector voorbeeld 1:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.Select(family=>family.parents[0])
          .Where(familyName == "Wakefield");
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```

**Selector voorbeeld 2:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.Where(family => family.children[0].grade > 3)
          .Select(family => family.parents[0].familyName);
  ```

- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
      WHERE f.children[0].grade > 3
  ```

**Selector voorbeeld 3:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.Select(family => new { grade=family.children[0].grade}).
          Where(anon=> anon.grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE ({grade: f.children[0].grade}.grade > 3)
  ```

**Selector voorbeeld 4:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.SelectMany(family => family.parents)
          .Where(parent => parents.familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM p IN Families.parents
      WHERE p.familyName = "Wakefield"
  ```

#### <a name="nesting"></a>Nesten

De syntaxis is `input.SelectMany(x=>x.Q())` waar `Q` is een `Select`, `SelectMany`, of `Where` operator.

Een geneste query geldt de binnenste query voor elk element van de buitenste container. Een belangrijk onderdeel is dat de binnenste query naar de velden van de elementen in de buitenste container, zoals een self-join verwijzen kan.

**Nesten, voorbeeld 1:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.SelectMany(family=>
          family.parents.Select(p => p.familyName));
  ```

- **SQL**
  
  ```sql
      SELECT VALUE p.familyName
      FROM Families f
      JOIN p IN f.parents
  ```

**Nesten, voorbeeld 2:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.SelectMany(family =>
          family.children.Where(child => child.familyName == "Jeff"));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = "Jeff"
  ```

**Nesten, voorbeeld 3:**

- **LINQ lambda-expressie**
  
  ```csharp
      input.SelectMany(family => family.children.Where(
          child => child.familyName == family.parents[0].familyName));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = f.parents[0].familyName
  ```

## <a id="References"></a>Naslaginformatie

- [SQL-specificatie voor Azure Cosmos DB](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [JSON](https://json.org/)
- [JavaScript-specificatie](https://www.ecma-international.org/publications/standards/Ecma-262.htm) 
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 
- Graefe, Goetz. [Query-evaluatietechnieken voor grote databases](https://dl.acm.org/citation.cfm?id=152611). *ACM enquêtes Computing* 25, geen. 2 (1993).
- Graefe, G. "De trapsgewijs framework voor queryoptimalisering." *IEEE-gegevens eng Bull.* 18, geen. 3 (1995).
- Lu, Ooi, Tan. "Verwerken van query's in parallelle relationele databasesystemen." *IEEE-Computer Society druk* (1994).
- Olston, Christopher Benjamin Reed, Utkarsh Srivastava, Kumar Ravi en Andrew Tomkins. "Pig Latin: Een niet-zodat-vreemde taal voor de verwerking van gegevens." *SIGMOD* (2008).

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB][introduction]
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Azure Cosmos DB-consistentieniveaus][consistency-levels]

[1]: ./media/how-to-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
