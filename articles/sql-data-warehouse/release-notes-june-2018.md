---
title: Azure SQL Data Warehouse release opmerkingen 2018 juni | Microsoft Docs
description: Release opmerkingen bij Azure SQL Data Warehouse.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/23/2018
ms.author: anjangsh
ms.reviewer: jrasnick
ms.openlocfilehash: 4348a634fd5b2b33f36d8e79f28caf659b82ccf4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "67626154"
---
# <a name="whats-new-in-azure-sql-data-warehouse-june-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Juni 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. In dit artikel worden de nieuwe functies en wijzigingen beschreven die zijn geïntroduceerd in juni 2018. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="user-defined-restore-points"></a>Door de gebruiker gedefinieerde herstel punten
SQL Data Warehouse maakt automatisch moment opnamen van uw data warehouse om de acht uur dat een RPO-Recovery Point Objective (8 uur) wordt gegarandeerd. Hoewel deze automatische moment opnamen de beheer last van het uitvoeren van uw data warehouse vereenvoudigen, is het nood zakelijk om moment opnamen te maken op kritieke tijdstippen op basis van uw bedrijfs behoeften. U kunt bijvoorbeeld een moment opname maken voor een belang rijke gegevens belasting of de implementatie van nieuwe scripts in het Data Warehouse om een herstel punt voor de bewerking in te scha kelen. 

SQL Data Warehouse ondersteunt nu door de [gebruiker gedefinieerde herstel punten](https://azure.microsoft.com/blog/quick-recovery-time-with-sql-data-warehouse-using-user-defined-restore-points/) via de cmdlet [New-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint) .

```powershell
New-AzSqlDatabaseRestorePoint
    -ResourceGroupName $ResourceGroupName
    -ServerName $ServerName
    -DatabaseName $DatabaseName
    -RestorePointLabel $RestorePointName
```

## <a name="column-level-security"></a>Beveiliging op kolom niveau
Het beheren van de toegang en beveiliging van gegevens in uw data warehouse is van cruciaal belang voor het ontwikkelen van vertrouwen met uw klanten en partners. SQL Data Warehouse [ondersteunt nu beveiliging op kolom niveau (CLS)](https://azure.microsoft.com/blog/column-level-security-is-now-supported-in-azure-sql-data-warehouse/) waarmee u machtigingen kunt aanpassen voor het weer geven van gevoelige gegevens door de gebruikers toegang tot specifieke kolommen in uw tabellen te beperken zonder dat u uw data warehouse opnieuw hoeft te ontwerpen.

Met CLS kunt u de toegang tot tabel kolommen beheren op basis van de uitvoerings context van de gebruiker of het groepslid maatschap met [de Standard-](https://docs.microsoft.com/azure/sql-data-warehouse/column-level-security) T-SQL-instructie. De logica van de toegangs beperking bevindt zich in de database tier zelf in plaats van de gegevens in een andere toepassing, waardoor de algemene beveiligings implementatie wordt vereenvoudigd.


```sql
CREATE USER Nurse WITHOUT LOGIN;   
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO Nurse;   
EXECUTE AS USER = 'Nurse';
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12 
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="objectschemaname"></a>OBJECT_SCHEMA_NAME
De functie [OBJECT_SCHEMA_NAME ()](https://docs.microsoft.com/sql/t-sql/functions/object-schema-name-transact-sql) retourneert de naam van het database schema voor objecten met een schema bereik. Deze functie is gebruikelijk geworden in ETL-hulpprogram ma's wanneer validatie van object schema wordt uitgevoerd. 

```sql
SELECT
    OBJECT_SCHEMA_NAME([object_id]) [table_schema]
    , [name]                        [table_name]
FROM
    [sys].[tables];
```

**Voorbeeld resultaten**
```
table_schema    | table_name
-----------------------------
dbo               customer
dbo               lineitem
dbo               nation
dbo               orders
```

## <a name="support-for-the-systimezoneinfo-view"></a>Ondersteuning voor de weer gave sys. time_zone_info
De [sys. time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) -weer gave retourneert informatie over de ondersteunde tijd zones in Azure SQL Data Warehouse.

```sql
SELECT * FROM [sys].[time_zone_info];
```

**Voorbeeld resultaten**
```
name                            | current_utc_offset | is_currently_dst
-------------------------------------------------------------------------
Pacific Standard Time (Mexico)    -07:00               1
US Mountain Standard Time         -07:00               0
Mountain Standard Time (Mexico)   -06:00               1
Central Standard Time             -05:00               1
```

## <a name="auto-stats-operations-appear-in-sysdmpdwexecrequests-behavior-change"></a>De bewerkingen voor automatische statistieken worden weer gegeven in sys. DM _pdw_exec_requests (gedrags wijziging)

Met de introductie van [automatisch gemaakte statistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistic)genereert Azure SQL Data Warehouse statistieken voor het optimaliseren van de uitvoering van query's. De release van juni 2018 voegt de mogelijkheid toe om te controleren wanneer de statistieken automatisch worden gegenereerd door een record toe te voegen aan de _pdw_exec_requests-weer gave [sys. DM](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) wanneer een bewerking voor het [maken van statistieken](https://docs.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql) wordt uitgevoerd.

```sql
SELECT
    [start_time]
    , [end_time]
    , [command]
FROM
    [sys].[dm_pdw_exec_requests]
WHERE
    [command] LIKE 'CREATE STATISTICS _WA_Sys%';
```
**Voorbeeld resultaten**
```
start_time                | end_time                | command
------------------------------------------------------------------------------------------------------------------------------
2018-06-06 19:06:26.173    2018-06-06 19:06:26.173    CREATE STATISTICS _WA_Sys_00000001_63D998CC ON dbo.LineItem(l_orderkey);
```

## <a name="next-steps"></a>Volgende stappen
Nu u een beetje weet over SQL Data Warehouse, kunt u leren hoe u snel [een SQL Data Warehouse maakt][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
