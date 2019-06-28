---
title: LINQ tot vertaling van SQL in Azure Cosmos DB
description: LINQ-query's toewijzen aan Azure Cosmos DB SQL-query's.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 057614da8fd29e1208c2788049c5d6d1a985eed5
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342855"
---
# <a name="linq-to-sql-translation"></a>LINQ to SQL-omzetting

De Azure Cosmos DB-provider query voert een aanbevolen inspanning toewijzing van een LINQ-query in een Cosmos DB SQL-query. De volgende beschrijving wordt ervan uitgegaan dat een enigszins bekend bent met LINQ.

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

## <a id="SupportedLinqOperators"></a>Ondersteunde LINQ-operators

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

## <a name="examples"></a>Voorbeelden

De volgende voorbeelden laten zien hoe sommige van de standaardoperators voor LINQ-query wordt omgezet naar Cosmos DB-query's.

### <a name="select-operator"></a>Operator selecteren

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

### <a name="selectmany-operator"></a>Operator SelectMany

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

### <a name="where-operator"></a>Operator Where

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

## <a name="composite-sql-queries"></a>Samengestelde SQL-query's

U kunt de voorgaande operators om krachtigere query's maken. U kunt omdat Cosmos DB biedt ondersteuning voor geneste containers, samenvoegen of nesten van de samenstelling.

### <a name="concatenation"></a>Samenvoegen

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

### <a name="nesting"></a>Nesten

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


## <a name="next-steps"></a>Volgende stappen

- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Documentgegevens modelleren](modeling-data.md)
