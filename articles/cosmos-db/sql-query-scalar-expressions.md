---
title: Scalaire expressies in Azure Cosmos DB SQL-query 's
description: Meer informatie over scalaire expressie SQL-syntaxis voor Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 4464c39a45c47c680a13f3ebc34841b47ee0d7c6
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342880"
---
# <a name="scalar-expressions-in-azure-cosmos-db-sql-queries"></a>Scalaire expressies in Azure Cosmos DB SQL-query 's

De [component SELECT](sql-query-select.md) biedt ondersteuning voor scalaire expressies. Een scalaire expressie die is een combinatie van tekens en operators die kunnen worden geëvalueerd om te verkrijgen van een enkele waarde. Voorbeelden van scalaire expressies zijn onder meer: constanten, verwijzen naar eigenschappen, matrix-element verwijst naar, alias-verwijzingen of functieaanroepen. Scalaire expressies kunnen worden gecombineerd tot complexe expressies operators gebruiken.

## <a name="syntax"></a>Syntaxis
  
```sql  
<scalar_expression> ::=  
       <constant>
     | input_alias
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>
     | <create_array_expression>  
     | (<scalar_expression>)
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```

## <a name="arguments"></a>Argumenten
  
- `<constant>`  
  
   Hiermee geeft u een constante waarde. Zie [constanten](sql-query-constants.md) sectie voor meer informatie.  
  
- `input_alias`  
  
   Hiermee geeft u een waarde die is gedefinieerd door de `input_alias` die is geïntroduceerd in de `FROM` component.  
  Deze waarde kan niet worden gegarandeerd **niet-gedefinieerde** –**niet-gedefinieerde** waarden in de invoer worden overgeslagen.  
  
- `<scalar_expression>.property_name`  
  
   Hiermee geeft u een waarde van de eigenschap van een object. Als de eigenschap niet bestaat of de eigenschap wordt verwezen op een waarde die niet een object, en de expressie wordt geëvalueerd als **niet-gedefinieerde** waarde.  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   Hiermee geeft u een waarde van de eigenschap met de naam `property_name` of matrixelement met index `array_index` van een matrix. Als de eigenschap/matrixindex niet bestaat of index van de eigenschap/array wordt verwezen op een waarde die is geen object/matrix, wordt de expressie wordt geëvalueerd als niet-gedefinieerde waarde.  
  
- `unary_operator <scalar_expression>`  
  
   Hiermee geeft u een operator die wordt toegepast op een enkele waarde. Zie [Operators](sql-query-operators.md) sectie voor meer informatie.  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   Een operator die wordt toegepast op de twee waarden vertegenwoordigt. Zie [Operators](sql-query-operators.md) sectie voor meer informatie.  
  
- `<scalar_function_expression>`  
  
   Hiermee geeft u een waarde die is gedefinieerd door een resultaat van een aanroep van de functie.  
  
- `udf_scalar_function`  
  
   De naam van de gebruiker gedefinieerde scalaire functie.  
  
- `builtin_scalar_function`  
  
   Naam van de ingebouwde scalaire functie.  
  
- `<create_object_expression>`  
  
   Hiermee geeft u een waarde die is verkregen door het maken van een nieuw object met de opgegeven eigenschappen en hun waarden.  
  
- `<create_array_expression>`  
  
   Hiermee geeft u een waarde die is verkregen door het maken van een nieuwe matrix met de opgegeven waarden als elementen  
  
- `parameter_name`  
  
   Vertegenwoordigt een waarde van de opgegeven parameternaam. Namen van parameters ze beschikken over één \@ als het eerste teken.  
  
## <a name="remarks"></a>Opmerkingen
  
  Wanneer u een ingebouwde of door de gebruiker gedefinieerde scalaire functie aanroept, kunnen alle argumenten moeten worden gedefinieerd. Als een van de argumenten niet is gedefinieerd, wordt de functie niet wordt aangeroepen en wordt het resultaat is niet gedefinieerd.  
  
  Bij het maken van een object, wordt elke eigenschap die niet-gedefinieerde waarde is toegewezen overgeslagen en niet opgenomen in het gemaakte object.  
  
  Wanneer het maken van een matrix, een elementwaarde die is toegewezen **niet-gedefinieerde** waarde wordt overgeslagen en niet opgenomen in het gemaakte object. Dit zorgt ervoor dat de volgende gedefinieerde element moet de plaatsvinden zodanig dat de gemaakte matrix wordt niet hebt overgeslagen indexen.  

## <a name="examples"></a>Voorbeelden

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

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB](introduction.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Subquery 's](sql-query-subquery.md)