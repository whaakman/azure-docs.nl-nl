---
title: SELECT-component in Azure Cosmos DB
description: Meer informatie over SQL SELECT-component voor Azure Cosmos DB. Gebruik SQL als een Azure Cosmos DB-JSON-querytaal.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: 84d0212f7f212b4554b506726e027fe51f795eea
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342860"
---
# <a name="select-clause"></a>SELECT-component

Elke query bestaat uit een SELECT-component en optionele [FROM](sql-query-from.md) en [waar](sql-query-where.md) van de EU, per ANSI SQL-standaarden. Normaal gesproken de bron in de component FROM is geïnventariseerd, en de component WHERE wordt een filter toegepast op de bron om op te halen van een subset van JSON-items. De component SELECT projecten vervolgens de vereiste JSON-waarden in de selectielijst.

## <a name="syntax"></a>Syntaxis

```sql
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | [DISTINCT] <object_property_list>   
      | [DISTINCT] VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
## <a name="arguments"></a>Argumenten
  
- `<select_specification>`  

  De eigenschappen of de waarde die moet worden geselecteerd voor de resultatenset.  
  
- `'*'`  

  Hiermee geeft u op dat de waarde zonder wijzigingen moet worden opgehaald. Specifiek als de verwerkte waarde een object is, worden alle eigenschappen worden opgehaald.  
  
- `<object_property_list>`  
  
  Hiermee geeft u de lijst met eigenschappen die moeten worden opgehaald. Elke geretourneerde waarde is een object met de eigenschappen die zijn opgegeven.  
  
- `VALUE`  

  Hiermee geeft u op dat de JSON-waarde moet worden opgehaald in plaats van de volledige JSON-object. Dit, in tegenstelling tot `<property_list>` loopt niet de verwachte waarde in een object.  
 
- `DISTINCT`
  
  Hiermee geeft u op dat de kopieën van de verwachte eigenschappen moeten worden verwijderd.  

- `<scalar_expression>`  

  Expressie voor de waarde die moet worden berekend. Zie [scalaire expressies](sql-query-scalar-expressions.md) sectie voor meer informatie.  

## <a name="remarks"></a>Opmerkingen

De `SELECT *` syntaxis is alleen geldig als FROM-component precies één alias is gedeclareerd. `SELECT *` biedt een identity-projectie handig is als er geen projectie is vereist. Selecteer * is alleen geldig als FROM-component is opgegeven en slechts één invoer bron geïntroduceerd.  
  
Beide `SELECT <select_list>` en `SELECT *` 'syntactische suiker' zijn en kan ook worden uitgedrukt met behulp van eenvoudige SELECT-instructies zoals hieronder wordt weergegeven.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   is gelijk aan:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   is gelijk aan:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Voorbeelden

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

### <a name="quoted-property-accessor"></a>Tussen aanhalingstekens eigenschapsaccessor
U kunt toegang tot eigenschappen met behulp van de eigenschap tussen aanhalingstekens operator []. Zo is `SELECT c.grade` bijvoorbeeld het equivalent van `SELECT c["grade"]`. Deze syntaxis is handig als u een eigenschap die spaties, speciale tekens bevat of heeft dezelfde naam als een SQL-trefwoord of gereserveerd woord.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

### <a name="nested-properties"></a>Geneste eigenschappen

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
### <a name="json-expressions"></a>JSON-expressies

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

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [WHERE-component](sql-query-where.md)