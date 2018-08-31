---
title: Azure SQL Data Warehouse opmerkingen bij de Release van April 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: f1a3b99d40435bf2b7e33f81a43372f304fbb856
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287000"
---
# <a name="whats-new-in-azure-sql-data-warehouse-april-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? April 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in April 2018.

## <a name="ability-to-truncate-a-partition-before-a-switch"></a>Mogelijkheid om een partitie voordat u een Switch afkappen
Schakelen tussen als patroon partities klanten vaak gebruiken om gegevens te laden van de ene tabel naar een andere door het veranderen van de metagegevens van de tabel via de `ALTER TABLE SourceTable SWITCH PARTITION X TO TargetTable PARTITION X` syntaxis. SQL Data Warehouse biedt geen ondersteuning voor partitie schakelen tussen bij de doelpartitie gegevens bevat. Als de doelpartitie al gegevens bevat, moet de klant op de doelpartitie worden afgekapt en klik vervolgens op de switch.

SQL Data Warehouse biedt nu ondersteuning voor deze bewerking in één T-SQL-instructie.

```sql
ALTER TABLE SourceTable 
    SWITCH PARTITION X TO TargetTable PARTITION X
    WITH (TRUNCATE_TARGET_PARTITION = ON)
```
Zie voor meer informatie de [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) artikel.

## <a name="improved-query-compilation-performance"></a>Compilatie van betere queryprestaties
SQL Data Warehouse is een set wijzigingen voor het verbeteren van de query-compilatiestap van gedistribueerde query's geïntroduceerd. Deze wijzigingen sneller query compilatie tot **10 x** beperken tot uitvoering van runtimes algemene query. Deze wijzigingen zijn duidelijker op datawarehouses met een groot aantal objecten (tabellen, functies, weergaven, procedures).

## <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>DBCC-opdrachten veel niet Gelijktijdigheidssleuven (gedrag wijzigen)
SQL Data Warehouse ondersteunt een subset van de T-SQL [DBCC-opdrachten](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) zoals [DBCC DROPCLEANBUFFERS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql). Eerder deze opdrachten wilt gebruiken een [gelijktijdigheid van taken sleuf](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#concurrency-slots) minder belasting/query's van gebruikers die kan worden uitgevoerd. De `DBCC` opdrachten nu worden uitgevoerd in een lokale wachtrij die niet meetellen als een resource-sleuf verbeteren de algehele prestaties van query's worden uitgevoerd.

## <a name="updated-error-message-for-excessive-literals-behavior-change"></a>Bijgewerkte foutbericht overmatige letterlijke (gedrag wijzigen)
Voorheen SQL Data Warehouse omvat een *geschatte* tellen wanneer een query bevat te veel letterlijke tekenreeksen zijn.
```
Msg 100086
Cannot have more than 20,000 literals in the query. The query contains [n] literals.
```

Het foutbericht is bijgewerkt om aan te geven alleen dat u de letterlijke limiet hebt bereikt.
```
Msg 100086
The number of literals in the query is beyond the limit. Please rewrite your query.
```

Zie voor meer informatie de [query's](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#queries) sectie van de [Capaciteitslimieten](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits) artikel voor meer informatie over maximumlimieten.

## <a name="removed-the-syspdwdatabasemappings-view-behavior-change"></a>De SYS verwijderd. PDW_DATABASE_MAPPINGS weergeven (gedrag wijzigen)
Dit `sys.pdw_database_mappings` weergave wordt gebruikt in SQL Data Warehouse. Voorheen zou een selectie van deze weergave geen resultaten geretourneerd. De weergave is verwijderd. 

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