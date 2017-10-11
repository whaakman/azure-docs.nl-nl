---
title: Migreren van uw SQL-code naar SQL Data Warehouse | Microsoft Docs
description: Tips voor het migreren van uw SQL-code naar Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 19c252a3-0e41-4eec-9d3e-09a68c7e7add
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/23/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: c6e6b890f5e2d0e31b10bbb6803adad02bf60248
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migreren van uw SQL-code naar SQL Data Warehouse
Dit artikel wordt uitgelegd codewijzigingen u waarschijnlijk moet maken wanneer u uw code uit een andere database migreren naar SQL Data Warehouse. Sommige functies van SQL Data Warehouse kunnen prestaties aanzienlijk verbeteren omdat ze zijn ontworpen om te werken in een gedistribueerde wijze. Echter, om te blijven de prestaties en schaalbaarheid, sommige functies zijn ook niet beschikbaar.

## <a name="common-t-sql-limitations"></a>Algemene T-SQL-beperkingen
De volgende lijst bevat een overzicht van de meest gebruikte functies die geen ondersteuning biedt voor SQL Data Warehouse. Koppelingen gaat u naar de tijdelijke oplossingen voor de niet-ondersteunde functies:

* [ANSI joins op updates][ANSI joins on updates]
* [ANSI-verbindingen in verwijderen][ANSI joins on deletes]
* [Merge-instructie][merge statement]
* meerdere databases joins
* [cursors][cursors]
* [INSERT... EXEC][INSERT..EXEC]
* OUTPUT-component
* de gebruiker gedefinieerde inlinefuncties
* meerdere instructies functies
* [algemene tabelexpressies](#Common-table-expressions)
* [recursieve algemene tabelexpressies (CTE)] (#Recursive-common-table-expressions-(CTE)
* CLR-functies en procedures
* de functie $partition
* tabelvariabelen voor de
* de parameters voor waarde
* gedistribueerde transacties
* Commit / rollback werken
* transactie opslaan
* uitvoering contexten (EXECUTE AS)
* [Group by, component met rollup / kubus / sets opties voor groeperen][group by clause with rollup / cube / grouping sets options]
* [geneste niveaus dan 8][nesting levels beyond 8]
* [door weergaven bijwerken][updating through views]
* [gebruik van selecteren voor de toewijzing van variabele][use of select for variable assignment]
* [Er is geen MAX-gegevenstype voor dynamische SQL-tekenreeksen][no MAX data type for dynamic SQL strings]

De meeste van deze beperkingen kunt gelukkig rond worden gewerkt. Uitleg vindt u in de hierboven vermelde relevante ontwikkeling-artikelen.

## <a name="supported-cte-features"></a>Ondersteunde CTE-functies
Algemene tabelexpressies (CTE's) worden gedeeltelijk ondersteund in SQL Data Warehouse.  De volgende CTE-functies worden momenteel ondersteund:

* Een CTE kan worden opgegeven in een SELECT-instructie.
* Een CTE kan worden opgegeven in een instructie CREATE VIEW.
* Een CTE kan worden opgegeven in een instructie maken tabel AS selecteren (CTAS).
* Een CTE kan worden opgegeven in een instructie maken externe tabel AS selecteren (CRTAS).
* Een CTE kan worden opgegeven in een instructie maken externe tabel AS selecteren (CETAS).
* Een externe tabel kan worden verwezen vanuit een CTE.
* Een externe tabel kan worden verwezen vanuit een CTE.
* Meerdere definities voor CTE-query kunnen worden gedefinieerd in een CTE.

## <a name="cte-limitations"></a>CTE-beperkingen
Algemene tabelexpressies hebben enkele beperkingen in SQL Data Warehouse, waaronder:

* Een CTE moet worden gevolgd door één SELECT-instructie. INSERT, UPDATE, DELETE, en MERGE-instructies worden niet ondersteund.
* Een algemene tabelexpressie die verwijzingen naar zichzelf (een recursieve algemene tabelexpressie bevat) wordt niet ondersteund (Zie onderstaande sectie).
* Het opgeven van meer dan één met de component in een CTE is niet toegestaan. Bijvoorbeeld, als een CTE_query_definition een subquery bevat, kan geen die subquery bevatten een geneste met component die een andere CTE definieert.
* Een component ORDER BY kan niet worden gebruikt in de CTE_query_definition, behalve wanneer de component TOP is opgegeven.
* Wanneer een CTE wordt gebruikt in een instructie die deel uitmaakt van een batch, moet de instructie voordat deze worden gevolgd door een puntkomma.
* Wanneer gebruikt in instructies sp_prepare voorbereid, wordt dezelfde manier als andere SELECT-instructies in PDW gedragen zich CTE's. Echter als CTE's worden gebruikt als onderdeel van CETAS sp_prepare voorbereid, kan het gedrag uitstellen van SQL Server en andere instructies PDW vanwege de manier waarop binding voor sp_prepare is geïmplementeerd. Als SELECT verwijzingen die CTE met behulp van een verkeerde kolom niet in CTE bestaat, geeft de sp_prepare zonder de fout te detecteren, maar de fout gegenereerd tijdens sp_execute in plaats daarvan.

## <a name="recursive-ctes"></a>Recursieve CTE 's
Recursieve CTE's worden niet ondersteund in SQL Data Warehouse.  De migratie van recursieve CTE kan erg complex en de aanbevolen procedure is dit in meerdere stappen te verdelen. U kunt gewoonlijk een lus gebruiken en vul een tijdelijke tabel waar u de tussentijdse recursieve query's doorlopen. Zodra de tijdelijke tabel wordt ingevuld kunt u de gegevens als een enkelvoudig resultaat wordt verkregen set terugkeren. Een soortgelijke benadering is gebruikt voor het oplossen van `GROUP BY WITH CUBE` in de [group by, component met rollup / kubus / sets opties voor groeperen] [ group by clause with rollup / cube / grouping sets options] artikel.

## <a name="unsupported-system-functions"></a>Niet-ondersteunde systeemfuncties
Er zijn ook enkele systeemfuncties die worden niet ondersteund. Enkele van de belangrijkste zijn die doorgaans wellicht gebruikt in de gegevensopslag zijn:

* NEWSEQUENTIALID()
* @@NESTLEVEL()
* @@IDENTITY()
* @@ROWCOUNT()
* ROWCOUNT_BIG
* ERROR_LINE()

Sommige van deze problemen kunnen worden uitgevoerd om.

## <a name="rowcount-workaround"></a>@@ROWCOUNT tijdelijke oplossing
Onvoldoende ondersteuning voor @ omzeilen@ROWCOUNT, maakt u een opgeslagen procedure die wordt opgehaald van het laatste aantal rijen uit sys.dm_pdw_request_steps en wordt vervolgens uitgevoerd `EXEC LastRowCount` na een DML-instructie.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een volledige lijst met alle ondersteunde T-SQL-instructies, [Transact-SQL-onderwerpen][Transact-SQL topics].

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI joins on deletes]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[merge statement]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INSERT..EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL topics]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[group by clause with rollup / cube / grouping sets options]: ./sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: ./sql-data-warehouse-develop-transactions.md
[updating through views]: ./sql-data-warehouse-develop-views.md
[use of select for variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
