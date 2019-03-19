---
title: T-SQL-instructies - Azure SQL Data Warehouse | Microsoft Docs
description: Koppelingen naar de documentatie voor T-SQL-instructies ondersteund in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 06/13/2018
ms.author: twounder
ms.reviewer: igorstan
ms.openlocfilehash: 8922d4399f52937710f111c3c8ad4c9af91b6bb7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57890097"
---
# <a name="t-sql-statements-supported-in-azure-sql-data-warehouse"></a>T-SQL-instructies ondersteund in Azure SQL Data Warehouse
Koppelingen naar de documentatie voor T-SQL-instructies ondersteund in Azure SQL Data Warehouse.

## <a name="data-definition-language-ddl-statements"></a>Instructies voor Data Definition Language (DDL)
* [ALTER DATABASE](https://msdn.microsoft.com/library/mt204042.aspx)
* [ALTER INDEX](https://msdn.microsoft.com/library/ms188388.aspx)
* [ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
* [ALTER SCHEMA](https://msdn.microsoft.com/library/ms173423.aspx)
* [ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)
* [COLUMNSTORE-INDEX MAKEN](https://msdn.microsoft.com/library/gg492153.aspx)
* [CREATE DATABASE](https://msdn.microsoft.com/library/mt204021.aspx)
* [MAKEN VAN DATABASE-SCOPED REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
* [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
* [CREATE EXTERNAL FILE FORMAT](https://msdn.microsoft.com/library/dn935026.aspx)
* [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx)
* [FUNCTIE MAKEN](https://msdn.microsoft.com/library/mt203952.aspx)
* [INDEX MAKEN](https://msdn.microsoft.com/library/ms188783.aspx)
* [PROCEDURE MAKEN](https://msdn.microsoft.com/library/ms187926.aspx)
* [SCHEMA MAKEN](https://msdn.microsoft.com/library/ms189462.aspx)
* [CREATE STATISTICS](https://msdn.microsoft.com/library/ms188038.aspx)
* [CREATE TABLE](https://msdn.microsoft.com/library/mt203953.aspx)
* [CREATE TABLE AS SELECT](https://msdn.microsoft.com/library/mt204041.aspx)
* [WEERGAVE MAKEN](https://msdn.microsoft.com/library/ms187956.aspx)
* [MAKEN van de WERKBELASTING classificatie](/sql/t-sql/statements/create-workload-classifier-transact-sql) (Preview)
* [EXTERNE GEGEVENSBRON VERWIJDEREN](https://msdn.microsoft.com/library/mt146367.aspx)
* [VERWIJDEREN VAN DE EXTERNE BESTANDSINDELING](https://msdn.microsoft.com/library/mt146379.aspx)
* [EXTERNE TABEL VERWIJDEREN](https://msdn.microsoft.com/library/mt130698.aspx)
* [INDEX VERWIJDEREN](https://msdn.microsoft.com/library/ms176118.aspx)
* [PROCEDURE VERWIJDEREN](https://msdn.microsoft.com/library/ms174969.aspx)
* [DROP STATISTICS](https://msdn.microsoft.com/library/ms175075.aspx)
* [TABEL VERWIJDEREN](https://msdn.microsoft.com/library/ms173790.aspx)
* [SCHEMA VERWIJDEREN](https://msdn.microsoft.com/library/ms186751.aspx)
* [WEERGAVE VERWIJDEREN](https://msdn.microsoft.com/library/ms173492.aspx)
* [VERVOLGKEUZELIJST WERKBELASTING classificatie](/sql/t-sql/statements/drop-workload-classifier-transact-sql) (Preview)
* [WIJZIG DE NAAM](https://msdn.microsoft.com/library/mt631611.aspx)
* [TRUNCATE TABLE](https://msdn.microsoft.com/library/ms177570.aspx)
* [UPDATE STATISTICS](https://msdn.microsoft.com/library/ms187348.aspx)

## <a name="data-manipulation-language-dml-statements"></a>Gegevens manipuleren taal (DML)-instructies
* [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)
* [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)
* [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)

## <a name="database-console-commands"></a>Database Console Commands
* [DBCC DROPCLEANBUFFERS](https://msdn.microsoft.com/library/ms187762.aspx)
* [DBCC FREEPROCCACHE](https://msdn.microsoft.com/library/mt204018.aspx)
* [DBCC SHRINKLOG](https://msdn.microsoft.com/library/mt204020.aspx)
* [DBCC PDW_SHOWEXECUTIONPLAN](https://msdn.microsoft.com/library/mt204017.aspx)
* [DBCC PDW_SHOWPARTITIONSTATS](https://msdn.microsoft.com/library/mt204013.aspx)
* [DBCC PDW_SHOWSPACEUSED](https://msdn.microsoft.com/library/mt204028.aspx)
* [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/mt204043.aspx)

## <a name="query-statements"></a>Queryinstructies
* [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)
* [WITH common_table_expression](https://msdn.microsoft.com/library/ms175972.aspx)
* [Met uitzondering van en SNIJDEN](https://msdn.microsoft.com/library/ms188055.aspx)
* [EXPLAIN](https://msdn.microsoft.com/library/mt631615.aspx)
* [FROM](https://msdn.microsoft.com/library/ms177634.aspx)
* [Met behulp van DRAAITABELLEN en UNPIVOT](https://msdn.microsoft.com/library/ms177410.aspx)
* [GROEPEREN OP](https://msdn.microsoft.com/library/ms177673.aspx)
* [HAVING](https://msdn.microsoft.com/library/ms180199.aspx)
* [ORDER BY](https://msdn.microsoft.com/library/ms188385.aspx)
* [OPTION](https://msdn.microsoft.com/library/ms190322.aspx)
* [UNION](https://msdn.microsoft.com/library/ms180026.aspx)
* [WHERE](https://msdn.microsoft.com/library/ms188047.aspx)
* [TOP](https://msdn.microsoft.com/library/ms189463.aspx)
* [Aliasing](https://msdn.microsoft.com/library/mt631614.aspx)
* [Zoekvoorwaarde](https://msdn.microsoft.com/library/ms173545.aspx)
* [Subquery 's](https://msdn.microsoft.com/library/mt631613.aspx)

## <a name="security-statements"></a>Instructies voor beveiliging
* Machtigingen: [GRANT](https://msdn.microsoft.com/library/ms187965.aspx), [DENY](https://msdn.microsoft.com/library/ms188338.aspx), [REVOKE](https://msdn.microsoft.com/library/ms187728.aspx)
* [ALTER AUTORISATIE](https://msdn.microsoft.com/library/ms187359.aspx)
* [CERTIFICAAT WIJZIGEN](https://msdn.microsoft.com/library/ms189511.aspx)
* [ALTER DATABASE ENCRYPTION KEY](https://msdn.microsoft.com/library/bb630389.aspx)
* [ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)
* [HOOFDSLEUTEL WIJZIGEN](https://msdn.microsoft.com/library/ms186937.aspx)
* [ROL WIJZIGEN](https://msdn.microsoft.com/library/ms189775.aspx)
* [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)
* [BACK-CERTIFICAAT](https://msdn.microsoft.com/library/ms178578.aspx)
* [SLUIT DE HOOFDSLEUTEL](https://msdn.microsoft.com/library/ms188387.aspx)
* [CERTIFICAAT MAKEN](https://msdn.microsoft.com/library/ms187798.aspx)
* [DATABASE ENCRYPTION KEY MAKEN](https://msdn.microsoft.com/library/bb677241.aspx)
* [AANMELDING MAKEN](https://msdn.microsoft.com/library/ms189751.aspx)
* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
* [ROL MAKEN](https://msdn.microsoft.com/library/ms187936.aspx)
* [GEBRUIKER MAKEN](https://msdn.microsoft.com/library/ms173463.aspx)
* [CERTIFICAAT VERWIJDEREN](https://msdn.microsoft.com/library/ms179906.aspx)
* [DROP DATABASE ENCRYPTION KEY](https://msdn.microsoft.com/library/bb630256.aspx)
* [AANMELDING](https://msdn.microsoft.com/library/ms188012.aspx)
* [HOOFDSLEUTEL VERWIJDEREN](https://msdn.microsoft.com/library/ms180071.aspx)
* [ROL VERWIJDEREN](https://msdn.microsoft.com/library/ms174988.aspx)
* [GEBRUIKER VERWIJDEREN](https://msdn.microsoft.com/library/ms189438.aspx)
* [OPEN DE HOOFDSLEUTEL](https://msdn.microsoft.com/library/ms174433.aspx)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [taalelementen T-SQL in Azure SQL Data Warehouse](sql-data-warehouse-reference-tsql-language-elements.md), en [systeemweergaven in Azure SQL Data Warehouse](sql-data-warehouse-reference-tsql-system-views.md).
