---
title: Azure SQL Data Warehouse opmerkingen bij de Release van juni 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: cae02627c539e543d27ea188d521605f187ea8a0
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475343"
---
# <a name="whats-new-in-azure-sql-data-warehouse-june-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Juni 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in juni 2018. 

## <a name="user-defined-restore-points"></a>Herstelpunten gedefinieerd door de gebruiker
SQL Data Warehouse maakt automatisch momentopnamen van uw datawarehouse om de 8 uur een acht uur beoogd herstelpunt (RPO) te garanderen. Deze momentopnamen eenvoudig op het werk te verrichten van het uitvoeren van uw datawarehouse geautomatiseerd, maar er is behoefte aan momentopnamen op kritieke perioden, op basis van uw zakelijke behoeften. Bijvoorbeeld: maken van een momentopname aan vóór de belasting van een aanzienlijke hoeveelheid gegevens of de implementatie van nieuwe scripts in de datawarehouse om in te schakelen van een herstelpunt aan vóór het opnieuw. 

SQL Data Warehouse nu ondersteunt [herstelpunten die door de gebruiker gedefinieerde](https://azure.microsoft.com/blog/quick-recovery-time-with-sql-data-warehouse-using-user-defined-restore-points/) via de [New-AzureRmSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabaserestorepoint) cmdlet.

```PowerShell
New-AzureRmSqlDatabaseRestorePoint
    -ResourceGroupName $ResourceGroupName
    -ServerName $ServerName
    -DatabaseName $DatabaseName
    -RestorePointLabel $RestorePointName
```

## <a name="column-level-security"></a>Beveiliging op kolom
Het beheren van toegang tot gegevens en beveiliging in uw datawarehouse is essentieel is voor het bouwen van de vertrouwensrelatie met uw klanten en partners. SQL Data Warehouse [biedt nu ondersteuning voor beveiliging op kolomniveau (CLS)](https://azure.microsoft.com/blog/column-level-security-is-now-supported-in-azure-sql-data-warehouse/) waarmee u machtigingen voor het weergeven van gevoelige gegevens, door het beperken van toegang tot specifieke kolommen in de tabellen voor gebruikers zonder te hoeven ontwerpen van uw datawarehouse aanpassen.

CLS kunt u voor het beheren van toegang tot de kolommen in de tabel op basis van de uitvoeringscontext van de gebruiker of hun groepslidmaatschap met behulp van standaard [verlenen](https://docs.microsoft.com/azure/sql-data-warehouse/column-level-security) T-SQL-instructie. De logica van de beperking van toegang bevindt zich in de databaselaag zelf in plaats van weg van de gegevens in een andere toepassing, de implementatie van de algehele beveiliging te vereenvoudigen.


```sql
CREATE USER Nurse WITHOUT LOGIN;   
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO Nurse;   
EXECUTE AS USER = 'Nurse';
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12 
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="objectschemaname"></a>OBJECT_SCHEMA_NAME
De [OBJECT_SCHEMA_NAME()](https://docs.microsoft.com/sql/t-sql/functions/object-schema-name-transact-sql) functie retourneert de naam van de database-schema voor schema-objecten. Deze functie is gebruikelijk in ETL-hulpprogramma's geworden als schemavalidatie-object. 

```sql
SELECT
    OBJECT_SCHEMA_NAME([object_id]) [table_schema]
    , [name]                        [table_name]
FROM
    [sys].[tables];
```

**Van voorbeeldresultaten**
```
table_schema    | table_name
-----------------------------
dbo               customer
dbo               lineitem
dbo               nation
dbo               orders
```

## <a name="support-for-the-systimezoneinfo-view"></a>Ondersteuning voor de weergave sys.time_zone_info
De [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) weergave retourneert informatie over de ondersteunde tijdzones in Azure SQL Data Warehouse.

```sql
SELECT * FROM [sys].[time_zone_info];
```

**Van voorbeeldresultaten**
```
name                            | current_utc_offset | is_currently_dst
-------------------------------------------------------------------------
Pacific Standard Time (Mexico)    -07:00               1
US Mountain Standard Time         -07:00               0
Mountain Standard Time (Mexico)   -06:00               1
Central Standard Time             -05:00               1
```

## <a name="auto-stats-operations-appear-in-sysdmpdwexecrequests-behavior-change"></a>Automatische statistieken bewerkingen worden weergegeven in sys.dm_pdw_exec_requests (gedrag wijzigen)

Dankzij de introductie van [automatisch Create Statistics](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistics), Azure SQL Data Warehouse genereert statistieken voor het uitvoeren van query's optimaliseren. De release van juni 2018 voegt de mogelijkheid om te controleren wanneer statistieken automatisch gegenereerd worden door het toevoegen van een record in de [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) weergeven als een [CREATE STATISTICS](https://docs.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql) bewerking wordt uitgevoerd.

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
**Van voorbeeldresultaten**
```
start_time                | end_time                | command
------------------------------------------------------------------------------------------------------------------------------
2018-06-06 19:06:26.173    2018-06-06 19:06:26.173    CREATE STATISTICS _WA_Sys_00000001_63D998CC ON dbo.LineItem(l_orderkey);
```

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

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
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
