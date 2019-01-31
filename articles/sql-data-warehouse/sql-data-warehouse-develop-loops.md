---
title: Met behulp van T-SQL-lussen in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van T-SQL-lussen en vervangen van cursors in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 5aa26aeb27d962e6e6289a754ef57b49158b68db
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456286"
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>Met behulp van T-SQL-lussen in SQL Data Warehouse
Tips voor het gebruik van T-SQL-lussen en vervangen van cursors in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="purpose-of-while-loops"></a>Doel van de WHILE-lus

SQL Data Warehouse ondersteunt de [terwijl](/sql/t-sql/language-elements/while-transact-sql) lus voor instructie-blokken herhaaldelijk uitvoeren. Deze WHILE-lus geldt voor zolang de opgegeven voorwaarden waar of totdat de code specifiek zijn beëindigt de lus met het sleutelwoord BREAK. Lussen zijn nuttig voor het vervangen van cursors die zijn gedefinieerd in SQL-code. Gelukkig zijn bijna alle cursors die zijn geschreven in SQL-code van de verschillende vooruit, alleen-lezen. Daarom [terwijl] lussen zijn een goed alternatief voor het vervangen van cursors.

## <a name="replacing-cursors-in-sql-data-warehouse"></a>Vervangen van cursors in SQL Data Warehouse
Echter eerst in kop wilt voordat moet u Vraag uzelf de volgende: "Kan deze cursor worden herschreven als u wilt gebruiken op basis van een set bewerkingen?." In veel gevallen is het antwoord is ingesteld op Ja, en is vaak de beste benadering. Een bewerking op basis van een set vaak sneller dan een iteratieve, per rij benadering wordt uitgevoerd.

Vooruitspoelen cursors voor alleen-lezen kunnen eenvoudig worden vervangen door een cyklu. Het volgende is een eenvoudig voorbeeld. Dit codevoorbeeld werkt de statistieken voor elke tabel in de database. Elke opdracht wordt uitgevoerd door een iteratie van de tabellen in de lus, in de reeks.

Maak eerst een tijdelijke tabel met een unieke rij-getal dat is gebruikt voor het identificeren van de afzonderlijke instructies:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Ten tweede worden de variabelen die zijn vereist om uit te voeren van de lus geïnitialiseerd:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nu op een uitvoeren-instructies in een lus tegelijk:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Ten slotte de tijdelijke tabel die is gemaakt in de eerste stap verwijderen

```
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

