---
title: Gebruikmaken van T-SQL-lussen in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor de Transact-SQL-lussen en vervang cursors in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 40a872ff310f48bfd543ac184fe7301b85b50258
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="loops-in-sql-data-warehouse"></a>Lussen in SQL datawarehouse
SQL Data Warehouse ondersteunt de [terwijl][terwijl] lus voor herhaaldelijk blokken instructie uitvoeren. Dit wordt voortgezet voor als de opgegeven voorwaarden voldaan wordt, of tot de code specifiek de lus met behulp van beëindigt de `BREAK` sleutelwoord. Lussen zijn bijzonder nuttig voor het vervangen van cursors die zijn gedefinieerd in SQL-code. Gelukkig bijna alle cursors die zijn geschreven in SQL-code zijn van de snel vooruit-lezen alleen groot. Daarom [terwijl] lussen vormen een geweldige alternatief als u dat merkt wilt vervangen.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Gebruik van lussen en vervangt cursors in SQL Data Warehouse
Echter eerst in head wilt voordat u moet zelf de volgende vraag stellen: 'Kan deze cursor worden opnieuw geschreven naar set op basis van bewerkingen gebruiken?'. In veel gevallen is het antwoord Ja zijn, en is vaak de aanbevolen aanpak. Een set op basis van de bewerking vaak voert aanzienlijk sneller dan een benadering iteratieve, per rij.

Vooruit cursors voor alleen-lezen kunnen eenvoudig worden vervangen door een samenvoegartikel constructie. Hieronder vindt u een eenvoudig voorbeeld. Dit codevoorbeeld werkt de statistieken voor elke tabel in de database. Door iteratie van de tabellen in de lus kunnen we elke opdracht wordt uitgevoerd in de reeks.

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


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[terwijl]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
