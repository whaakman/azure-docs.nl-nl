---
title: Gebruiker gedefinieerde functies (UDF's) in Azure Cosmos DB
description: Meer informatie over de gebruiker gedefinieerde functies in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: e168e450230720f4ad78516e6edcdc3aa08ba3e1
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342585"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Gebruiker gedefinieerde functies (UDF's) in Azure Cosmos DB

De SQL-API biedt ondersteuning voor de gebruiker gedefinieerde functies (UDF's). U kunt met scalaire UDF's, nul of meer argumenten worden doorgegeven en retourneert een resultaat één argument. De API wordt elk argument voor een geldige JSON-waarden worden gecontroleerd.  

De API een uitbreiding voor de SQL-syntaxis ter ondersteuning van aangepaste toepassingslogica met UDF's. U kunt UDF's registreren bij de SQL-API en ernaar te verwijzen in SQL-query's. De UDFs zijn speciaal ontworpen voor aanroepen vanuit query's. Als gevolg van hebt UDF's geen toegang tot het contextobject, zoals andere JavaScript-typen, zoals opgeslagen procedures en triggers. Query's zijn alleen-lezen en kunnen worden uitgevoerd op de primaire of secundaire replica's. UDF's, in tegenstelling tot andere typen JavaScript zijn ontworpen om uit te voeren op secundaire replica's.

Het volgende voorbeeld wordt een UDF onder de itemcontainer van een in de Cosmos DB-database geregistreerd. Het voorbeeld wordt een UDF met de naam `REGEX_MATCH`. De cmdlet accepteert twee waarden voor JSON-tekenreeks, `input` en `pattern`, en controleert of het eerste komt overeen met het patroon die zijn opgegeven in de tweede met behulp van JavaScript `string.match()` functie.

## <a name="examples"></a>Voorbeelden

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

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB](introduction.md)
- [Systeemfuncties](sql-query-system-functions.md)
- [Aggregates](sql-query-aggregates.md)