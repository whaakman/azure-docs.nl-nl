---
title: Aan de slag met SQL-query's in Azure Cosmos DB
description: Inleiding tot SQL-query 's
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 87b275806c06443e37e9e92c35a38b4cde378322
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342535"
---
# <a name="getting-started-with-sql-queries"></a>Aan de slag met SQL-query 's

Azure Cosmos DB SQL API-accounts ondersteunen een query uitvoeren op items Structured Query Language (SQL) gebruiken als een querytaal voor JSON. De ontwerpdoelstellingen van de querytaal van Azure Cosmos DB zijn:

* Ondersteuning voor SQL, een van de meest vertrouwde en populaire querytalen, in plaats van een nieuwe querytaal vruchtbare. SQL biedt een formele programmeermodel voor uitgebreide query's via JSON-items.  

* JavaScript programmeermodel gebruiken als basis voor de querytaal. JavaScript typesysteem, evaluatie van de expressie en functieaanroepen zijn de hoofdmappen van de SQL-API. Deze hoofdmappen bieden een natuurlijk programmeermodel voor functies, zoals projecties van relationele, hiërarchische navigatie in JSON-items, zelf-joins, ruimtelijke query's, en het aanroepen van de gebruiker gedefinieerde functies (UDF's) die volledig in JavaScript geschreven.

## <a name="upload-sample-data"></a>Voorbeeldgegevens uploaden

Maak een container met de naam in uw SQL API Cosmos DB-account `Families`. Maak twee eenvoudige JSON-items in de container. U kunt de meeste van de voorbeeldquery's uitvoeren in de Azure Cosmos DB-query-documenten met behulp van deze gegevensset.

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

De volgende query retourneert de items waar de `id` veld komt overeen met `AndersenFamily`. Omdat het een `SELECT *` query, de uitvoer van de query is de volledige JSON-object. Zie voor meer informatie over de syntaxis van SELECT [SELECT-instructie](sql-query-select.md). 

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

## <a name="remarks"></a>Opmerkingen

De voorgaande voorbeelden tonen verschillende aspecten van de querytaal van Cosmos DB:  

* Omdat SQL API op JSON-waarden werkt, behandelt structuur vormgegeven entiteiten in plaats van rijen en kolommen. U kunt verwijzen naar de structuurknooppunten op elke willekeurige diepte zoals `Node1.Node2.Node3…..Nodem`, vergelijkbaar met de verwijzing tweedelige van `<table>.<column>` in ANSI SQL.

* Omdat de querytaal met gegevens zonder schema werkt, moet het typesysteem dynamisch zijn gebonden. Een expressie kan verschillende typen voor verschillende elementen opleveren. Het resultaat van een query is een geldige JSON-waarde, maar staat niet vast van een vast schema.  

* Azure Cosmos DB biedt alleen ondersteuning voor JSON-items. Het systeem en voor expressies zijn beperkt tot alleen bekommeren om JSON-typen. Zie voor meer informatie de [JSON-specificatie](https://www.json.org/).  

* Een Cosmos DB-container is een verzameling JSON-items zonder schema. De relaties binnen en tussen container items worden impliciet vastgelegd door containment, niet door de primaire sleutel en refererende sleutels relaties. Deze functie is belangrijk voor de intra-item wordt verderop in dit artikel besproken.

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB](introduction.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [SELECT-component](sql-query-select.md)
