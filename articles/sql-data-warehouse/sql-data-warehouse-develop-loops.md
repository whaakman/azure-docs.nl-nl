---
title: Met T-SQL-lussen in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van T-SQL-lussen en vervangen cursors in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: ''
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 2e49a0de0e4a6aba6639f7f3100f41c8db254220
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>Met behulp van T-SQL-lussen in SQL Data Warehouse
Tips voor het gebruik van T-SQL-lussen en vervangen cursors in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="purpose-of-while-loops"></a>Doel van tijdens het lussen

SQL Data Warehouse ondersteunt de [terwijl](/sql/t-sql/language-elements/while-transact-sql) lus voor herhaaldelijk blokken instructie uitvoeren. Deze lus tijdens geldt voor zolang de opgegeven voorwaarden waar of totdat de code specifiek zijn beëindigt de lus met het sleutelwoord BREAK. Lussen zijn nuttig voor het vervangen van cursors die zijn gedefinieerd in SQL-code. Gelukkig zijn bijna alle cursors die zijn geschreven in SQL-code van de verschillende vooruit, alleen-lezen. Daarom [terwijl] lussen zijn een uitstekend alternatief voor het vervangen van cursors.

## <a name="replacing-cursors-in-sql-data-warehouse"></a>Vervangen van cursors in SQL Data Warehouse
Echter eerst in head wilt voordat u moet zelf de volgende vraag stellen: ' kan deze cursor worden herschreven om te gebruiken op basis van een set operations?. " In veel gevallen kan het antwoord Ja is en is vaak de aanbevolen aanpak. Een bewerking op basis van een set vaak sneller dan een benadering iteratieve, per rij wordt uitgevoerd.

Vooruit cursors voor alleen-lezen kunnen eenvoudig worden vervangen door een samenvoegartikel constructie. Hier volgt een voorbeeld van een eenvoudige. Dit codevoorbeeld werkt de statistieken voor elke tabel in de database. Elke opdracht wordt uitgevoerd door iteratie van de tabellen in de lus, in de reeks.

Maak eerst een tijdelijke tabel die een unieke rij-getal gebruikt voor het identificeren van de afzonderlijke instructies:

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

Ten tweede initialiseren van de variabelen die zijn vereist voor het uitvoeren van de lus:

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
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

