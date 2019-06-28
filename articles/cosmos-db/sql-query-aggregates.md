---
title: Statistische functies in Azure Cosmos DB
description: Meer informatie over SQL-syntaxis voor statistische functie voor Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: a6937e9e811ea8e44eda6f2bcb5d2c7d78db4934
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342570"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Statistische functies in Azure Cosmos DB

Statistische functies een berekening uitgevoerd op een set waarden in de component SELECT en één waarde retourneren. De volgende query retourneert bijvoorbeeld het aantal items in de `Families` container:

## <a name="examples"></a>Voorbeelden

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

## <a name="types-of-aggregate-functions"></a>Typen statistische functies

De SQL-API biedt ondersteuning voor de volgende statistische functies. SOM en gemiddelde werken op basis van numerieke waarden en het aantal, MIN en MAX werken op getallen, tekenreeksen, Booleaanse waarden en null-waarden.

| Function | Description |
|-------|-------------|
| COUNT | Retourneert het aantal items in de expressie. |
| SUM   | Retourneert de som van alle waarden in de expressie. |
| MIN   | Retourneert de minimumwaarde in de expressie. |
| MAX   | Retourneert de maximumwaarde in de expressie. |
| AVG   | Retourneert het gemiddelde van de waarden in de expressie. |

U kunt ook via de resultaten van een matrix iteratie samenvoegen.

> [!NOTE]
> In de Azure portal Data Explorer kunnen aggregatie-query's gedeeltelijke resultaten samenvoegen op slechts één query op pagina. De SDK produceert één cumulatieve waarde op alle pagina's. Als u wilt uitvoeren van aggregatie-query's met behulp van code, moet u de .NET SDK 1.12.0, .NET Core SDK 1.1.0 of Java SDK 1.9.5 of hoger.

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB](introduction.md)
- [Systeemfuncties](sql-query-system-functions.md)
- [Door gebruikers gedefinieerde functies](sql-query-udfs.md)