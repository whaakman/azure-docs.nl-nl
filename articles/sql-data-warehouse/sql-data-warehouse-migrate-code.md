---
title: Uw SQL-code migreren naar SQL Data Warehouse | Microsoft Docs
description: Tips voor het migreren van uw SQL-code naar Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: f36ee3ce6a8d678cdd846474576fc049df0b4f88
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472300"
---
# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Uw SQL-code migreren naar SQL Data Warehouse
In dit artikel wordt uitgelegd codewijzigingen u waarschijnlijk moet bij het migreren van uw code van een andere database met SQL Data Warehouse. Sommige functies van SQL Data Warehouse kunnen de prestaties aanzienlijk verbeteren als ze zijn ontworpen om te werken in een gedistribueerde manier. Echter, als u wilt behouden prestaties en schaalbaarheid, sommige functies zijn ook niet beschikbaar.

## <a name="common-t-sql-limitations"></a>Algemene beperkingen voor T-SQL
De volgende lijst bevat een overzicht van de meest voorkomende functies die geen ondersteuning biedt voor SQL Data Warehouse. De koppelingen verwijzen naar tijdelijke oplossingen voor de niet-ondersteunde functies:

* [ANSI-join op updates][ANSI joins on updates]
* [ANSI-join op verwijderen][ANSI joins on deletes]
* [instructie Merge][merge statement]
* verbindingen tussen meerdere databases
* [cursors][cursors]
* [INSERT..EXEC][INSERT..EXEC]
* OUTPUT-component
* inline gebruiker gedefinieerde functies
* meerdere instructies functies
* [algemene tabelexpressies](#Common-table-expressions)
* [recursieve algemene tabelexpressies (CTE)] (#Recursive-common-table-expressions-(CTE)
* CLR-functies en procedures
* de functie $partition
* tabelvariabelen voor de
* de parameters voor waarde
* Gedistribueerde transacties
* Commit / rollback werken
* transactie opslaan
* contexten worden uitgevoerd (EXECUTE AS)
* [groeperen op component met updatepakket / kubus / sets opties voor groeperen][group by clause with rollup / cube / grouping sets options]
* [geneste niveaus dan 8][nesting levels beyond 8]
* [bijwerken via weergaven][updating through views]
* [gebruik van selecteren voor toewijzing van variabele][use of select for variable assignment]
* [Er is geen MAX-gegevenstype voor de dynamische SQL-tekenreeksen][no MAX data type for dynamic SQL strings]

De meeste van deze beperkingen kunt gelukkig rond worden gewerkt. Uitleg over vindt u in de ontwikkeling van relevante artikelen waarnaar hierboven wordt verwezen.

## <a name="supported-cte-features"></a>Ondersteunde CTE-functies
Algemene tabelexpressies (CTE's) worden gedeeltelijk ondersteund in SQL Data Warehouse.  De volgende CTE-functies worden momenteel ondersteund:

* Een CTE kan worden opgegeven in een instructie SELECT.
* Een CTE kan worden opgegeven in een instructie CREATE VIEW.
* Een CTE kan worden opgegeven in een instructie CREATE TABLE AS SELECT (CTAS).
* Een CTE kan worden opgegeven in een instructie maken externe tabel als selecteren (CRTAS).
* Een CTE kan worden opgegeven in een instructie maken externe tabel als selecteren (CETAS).
* Een externe tabel kan worden verwezen vanuit een CTE.
* Een externe tabel kan worden verwezen vanuit een CTE.
* Definities van de query meerdere CTE kunnen worden gedefinieerd in een CTE.

## <a name="cte-limitations"></a>CTE-beperkingen
Algemene tabelexpressies hebben enkele beperkingen in SQL Data Warehouse, waaronder:

* Een CTE moet worden gevolgd door één SELECT-instructie. INSERT, UPDATE, DELETE, en de instructies voor samenvoegen worden niet ondersteund.
* Een algemene tabelexpressie die verwijzingen naar zichzelf (een recursieve algemene tabelexpressie bevat) wordt niet ondersteund (Zie volgende sectie).
* Meer dan één met de component opgeven in een CTE is niet toegestaan. Bijvoorbeeld, als een CTE_query_definition een subquery bevat, mag die subquery niet een geneste WITH-component die een andere CTE definieert.
* Een component ORDER BY kan niet worden gebruikt in de CTE_query_definition, behalve wanneer een TOP-component is opgegeven.
* Wanneer een CTE wordt gebruikt in een instructie die deel uitmaakt van een batch, moet de instructie voordat deze worden gevolgd door een puntkomma.
* Wanneer gebruikt in instructies voorbereid door sp_prepare, gedragen CTE's dezelfde manier als andere SELECT-instructies in PDW. Echter als CTE's worden gebruikt als onderdeel van CETAS voorbereid door sp_prepare, kan het gedrag uitstellen van SQL Server en andere instructies PDW vanwege de manier waarop de binding voor sp_prepare is geïmplementeerd. Als de optie verwijzingen die CTE met behulp van een verkeerde kolom die niet in CTE bestaat, de sp_prepare zonder het detecteren van de fout wordt doorgegeven, maar de fout gegenereerd tijdens sp_execute in plaats daarvan.

## <a name="recursive-ctes"></a>Recursieve CTE 's
Recursieve CTE's worden niet ondersteund in SQL Data Warehouse.  De migratie van recursieve CTE kan redelijk complex en de aanbevolen procedure is opsplitsen in meerdere stappen. Doorgaans kunt u een lus gebruiken en vullen van een tijdelijke tabel als u de tijdelijke recursieve query's herhalen. Zodra de tijdelijke tabel wordt ingevuld kunt u de gegevens vervolgens als een set één resultaat retourneren. Een soortgelijke benadering is gebruikt om op te lossen `GROUP BY WITH CUBE` in de [groeperen op component met updatepakket / kubus / sets opties voor groeperen] [ group by clause with rollup / cube / grouping sets options] artikel.

## <a name="unsupported-system-functions"></a>Niet-ondersteunde opgeslagen functies
Er zijn ook enkele systeemfuncties die worden niet ondersteund. Enkele van de belangrijkste adressen die u doorgaans kunt vinden in de gegevensopslag gebruikt, zijn:

* NEWSEQUENTIALID()
* @@NESTLEVEL()
* @@IDENTITY()
* @@ROWCOUNT()
* ROWCOUNT_BIG
* ERROR_LINE()

Sommige van deze problemen kunnen worden uitgevoerd om.

## <a name="rowcount-workaround"></a>@@ROWCOUNT tijdelijke oplossing
Geen ondersteuning voor @ omzeilen@ROWCOUNT, maak een opgeslagen procedure waarmee wordt het laatste aantal rijen opgehaald uit sys.dm_pdw_request_steps en voer vervolgens `EXEC LastRowCount` na een DML-instructie.

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
Zie voor een volledige lijst van alle ondersteunde T-SQL-instructies, [Transact-SQL-onderwerpen][Transact-SQL topics].

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
