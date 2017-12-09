---
title: Overzicht van Azure SQL Database-functies | Microsoft Docs
description: Deze pagina biedt een overzicht van de logische servers en databases van Azure SQL Database en omvat een functieondersteuningsmatrix met een koppeling naar elke ondersteunde functie.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 10/25/2017
ms.author: carlrab
ms.openlocfilehash: a15c23683a3334328720e8ae4369b2495f9613de
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-sql-database-features"></a>Azure SQL Database-functies

Azure SQL Database deelt een algemene codebasis met SQL Server en ondersteunt de meeste functies van de op het databaseniveau van de. De belangrijke functionele verschillen tussen Azure SQL Database en SQL Server zijn op het niveau van het exemplaar. 

Er worden doorlopend extra functies toegevoegd aan Azure SQL Database. Het wordt daarom aangeraden om onze Azure-webpagina Service-updates regelmatig te bezoeken en filters te gebruiken:

* Gefilterd op de [SQL Database-service](https://azure.microsoft.com/updates/?service=sql-database).
* Gefilterd op [aankondigingen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) over de algemene beschikbaarheid (GA) van SQL Database-functies.

## <a name="sql-server-and-sql-database-feature-support"></a>Ondersteuning voor de functie SQL Server en SQL-Database

De volgende tabel bevat de belangrijkste functies van SQL Server en bevat informatie over of elke bepaalde functie wordt ondersteund en een koppeling naar meer informatie over de functie. Zie [Resolving Transact-SQL differences during migration to SQL Database](sql-database-transact-sql-information.md) (Transact-SQL-verschillen oplossen tijdens de migratie naar de SQL Database) voor Transact-SQL-verschillen om te overwegen wanneer u een bestaande databaseoplossing migreert.


| **SQL Server-functie** | **Ondersteund in Azure SQL Database** | 
| --- | --- |  
| [Altijd versleuteld.](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja - Zie [certificaatarchief](sql-database-always-encrypted.md) en [sleutelkluis](sql-database-always-encrypted-azure-key-vault.md)|
| [AlwaysOn-beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Hoge beschikbaarheid is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-geo-replication-overview.md) |
| [Een database koppelen](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nee |
| [Toepassingsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja |
|[Controle](/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ja](sql-database-auditing.md)|
| [Automatische afstemming](/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)|
| [Bacpac-bestand (exporteren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja - Zie [SQL-Database wordt geëxporteerd](sql-database-export.md) |
| [Bacpac-bestand (importeren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja - Zie [SQL-Database importeren](sql-database-import.md) |
| [Back-opdracht](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Niet - Zie [geautomatiseerde back-ups](sql-database-automated-backups.md) |
| [Ingebouwde functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie afzonderlijke functies |
| [Gegevensregistratie wijzigen](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nee |
| [Wijzigingen bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |
| [Sortering instructies](https://docs.microsoft.com/sql/t-sql/statements/collations) | Ja |
| [Columnstore-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja - [alleen Premium-editie](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| [De common language runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nee |
| [Ingesloten databases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja |
| [Ingesloten gebruikers](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja |
| [Beheer van stroom trefwoorden](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja |
| [Cross-databasequery 's](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Gedeeltelijk - Zie [Elastische query's](sql-database-elastic-query-overview.md) |
| [Cursors](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja | 
| [Compressie van gegevens](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |
| [Database mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nee |
| [Databasespiegeling](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nee |
| [Databaseconfiguratie-instellingen](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nee |
| [Momentopnamen van databases](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nee |
| [Gegevenstypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |  
| [DBCC-instructies](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | De meeste - Zie afzonderlijke instructies |
| [DDL-componenten](https://docs.microsoft.com/sql/t-sql/statements/statements) | Ja |
| [DDL-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Alleen de database |
| [Gedistribueerde transacties - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Niet - Zie [elastische transacties](sql-database-elastic-transactions-overview.md) |
| [DML-instructies](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ja |
| [DML-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | De meeste - Zie afzonderlijke instructies | 
| [DMV's](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Sommige - Zie afzonderlijke DMV 's |
|[Dynamische gegevensmaskering](/sql/relational-databases/security/dynamic-data-masking)|[Ja](sql-database-dynamic-data-masking-get-started.md)|
| [Gebeurtenismeldingen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Niet - Zie [waarschuwingen](sql-database-insights-alerts-portal.md) |
| [Expressies](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja |
| [Uitgebreide gebeurtenissen](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Sommige - [uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-db-diff-from-svr.md) |
| [Uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nee |
| [Bestanden en bestandsgroepen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Primaire bestandsgroep |
| [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nee |
| [Zoekopdracht in volledige tekst](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | Woordafbreking van derden worden niet ondersteund. |
| [Functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie afzonderlijke functies |
| [Verwerking van de grafiek](/sql/relational-databases/graphs/sql-graph-overview) | Ja |
| [Optimalisatie in het geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja - [alleen Premium-editie](sql-database-in-memory.md) |
| [Ondersteuning voor JSON-gegevens](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | Ja |
| [Elementen van de taal](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | De meeste - Zie afzonderlijke elementen |  
| [Gekoppelde servers](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Niet - Zie [elastische query](sql-database-elastic-query-horizontal-partitioning.md) |
| [Back-upfunctie voor logboekbestanden](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Hoge beschikbaarheid is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-geo-replication-overview.md) |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nee |
| [Minimale bulkimport aanmelden](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nee |
| [Systeemgegevens wijzigen](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nee |
| [Online-indexbewerkingen](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ja |
| [Operators](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | De meeste - Zie afzonderlijke operators |
| [Punt in tijd database terugzetten](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja - Zie [SQL-Database herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nee |
| [Beheer op basis van beleid](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nee |
| [Predikaten](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ja |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Preview-versie; Zie [wat is er nieuw in machine learning](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  |
| [Resourceregeling](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nee |
| [Instructies herstellen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nee | 
| [Database herstellen vanuit back-up](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Zie van alleen - de ingebouwde back-ups [SQL-Database herstellen](sql-database-recovery-using-backups.md) |
| [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja |
| [Semantische zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nee |
| [Volgnummers](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nee |
| [Server-configuratie-instellingen](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nee |
| [Set-instructies](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | De meeste - Zie afzonderlijke instructies 
| [Ruimtelijk](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Niet - Zie [elastische taken](sql-database-elastic-jobs-getting-started.md) |
| [SQL Server analyseservices (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Zie [Azure analyseservices](https://azure.microsoft.com/services/analysis-services/) |
| [Controle van SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Niet - Zie [SQL Database auditing](sql-database-auditing.md) |
| [SQL Server integratieservices (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ja - Zie [Lift en shift SQL Server integratieservices-werkbelastingen naar de cloud](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview) |
| [SQL Server-PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Niet - Zie [uitgebreide gebeurtenissen](sql-database-xevent-db-diff-from-svr.md) |
| [SQL Server-replicatie](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Alleen voor transactionele abonnees en abonnees met momentopnamereplicatie](sql-database-cloud-migrate.md) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nee |
| [Opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja |
| [Systeem opgeslagen functies](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Sommige - Zie afzonderlijke functies |
| [Systeem opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Sommige - Zie afzonderlijke opgeslagen procedures |
| [Systeemtabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Sommige - Zie afzonderlijke tabellen |
| [Systeemweergaven catalogus](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Sommige - Zie afzonderlijke weergaven |
| [Tabellen partitioneren](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja - alleen de primaire bestandsgroep |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | Lokale en database-scoped globale tijdelijke tabellen alleen |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | Ja |
| [Variabelen](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | 
| [Transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja |
| [Windows Serverfailover Clustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Hoge beschikbaarheid is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van zakelijke continuïteit met Azure SQL Database](sql-database-geo-replication-overview.md) |
| [XML-indexen](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie [Resolving Transact-SQL differences during migration to SQL Database](sql-database-transact-sql-information.md) (Transact-SQL-verschillen oplossen tijdens de migratie naar de SQL Database) voor meer informatie over Transact-SQL-ondersteuning en -verschillen.
