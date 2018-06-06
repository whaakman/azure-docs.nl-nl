---
title: Azure SQL Data Warehouse releaseopmerkingen April 2018 | Microsoft Docs
description: Releaseopmerkingen voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 49ffc3ddfd586964ae8a9688aeb48fffdd327b45
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738933"
---
# <a name="whats-new-in-azure-sql-data-warehouse-april-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse (April 2018)?
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. In dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in April 2018.

## <a name="features"></a>Functies

### <a name="ability-to-truncate-a-partition-before-a-switch"></a>Mogelijkheid een partitie voordat een Switch worden afgekapt
Klanten partitie overschakelen als patroon vaak gebruikt om gegevens te laden van de ene tabel naar een andere door het wijzigen van de metagegevens van de tabel via de `ALTER TABLE SourceTable SWITCH PARTITION X TO TargetTable PARTITION X` syntaxis. SQL Data Warehouse biedt geen ondersteuning voor partitie overschakelen wanneer de doelpartitie gegevens bevat. Als de doelpartitie al gegevens bevat, moet de klant de doelpartitie worden afgekapt en klik vervolgens op de switch.

SQL Data Warehouse biedt nu ondersteuning voor deze bewerking in een enkele T-SQL-instructie.

```sql
ALTER TABLE SourceTable 
    SWITCH PARTITION X TO TargetTable PARTITION X
    WITH (TRUNCATE_TARGET_PARTITION = ON)
```
Zie voor meer informatie de [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) artikel.

### <a name="improved-query-compilation-performance"></a>Compilatie van verbeterde queryprestaties
SQL Data Warehouse geïntroduceerd om een set van wijzigingen voor het verbeteren van de query compilatiestap van gedistribueerde query's. Deze verbeteringen komen query compilatie tijden tot **10 x** verminderen uitvoering runtimes algemene query. Deze wijzigingen zijn duidelijker op datawarehouses met een groot aantal objecten (tabellen, functies, weergaven, procedures).

## <a name="behavior-changes"></a>Gedragswijzigingen

### <a name="dbcc-commands-do-not-consume-concurrency-slots"></a>DBCC-opdrachten veel niet gelijktijdigheid sleuven
SQL Data Warehouse ondersteunt een subset van de T-SQL [DBCC-opdrachten](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) zoals [DBCC DROPCLEANBUFFERS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql). Voorheen deze opdrachten zou gebruiken een [gelijktijdigheid sleuf](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#concurrency-slots) minder belasting/gebruikersquery die kan worden uitgevoerd. De `DBCC` opdrachten nu worden uitgevoerd in een lokale wachtrij die niet veel een resource sleuf verbeteren de algehele prestaties van query's worden uitgevoerd.

### <a name="updated-error-message-for-excessive-literals"></a>Bijgewerkte foutbericht voor overmatige Literals
Voorheen SQL Data Warehouse omvat een *geschatte* tellen wanneer een query bevat te veel letterlijke waarden.
```
Msg 100086
Cannot have more than 20,000 literals in the query. The query contains [n] literals.
```

Het foutbericht is bijgewerkt om aan te geven alleen dat u de letterlijke limiet hebt bereikt.
```
Msg 100086
The number of literals in the query is beyond the limit. Please rewrite your query.
```

Zie voor meer informatie de [query's](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#queries) sectie van de [Capaciteitslimieten](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits) artikel voor meer informatie over maximaal limieten.

### <a name="removed-the-syspdwdatabasemappings-view"></a>De SYS verwijderd. PDW_DATABASE_MAPPINGS weergeven
Dit `sys.pdw_database_mappings` weergave kan niet worden gebruikt in SQL Data Warehouse. Voorheen zou een selectie van deze weergave geen resultaten geretourneerd. De weergave is verwijderd. 