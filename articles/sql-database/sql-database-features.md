---
title: Vergelijking van Azure SQL Database | Microsoft Docs
description: In dit artikel vergelijkt de functies van SQL Server die beschikbaar in verschillende versies van Azure SQL Database zijn.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 7445496154d9fc2ca59e19059a87a794a1361b14
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747404"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Vergelijking van functies: Azure SQL Database versus SQL Server

Azure SQL Database deelt een algemene codebasis met SQL Server. De functies van SQL Server wordt ondersteund door Azure SQL Database, is afhankelijk van het type van Azure SQL-database die u maakt. Met Azure SQL Database, kunt u een database maken als onderdeel van een [beheerd exemplaar](sql-database-managed-instance.md), als een individuele database, of als onderdeel van een elastische pool.

Microsoft blijft functies toevoegen aan Azure SQL Database. Bezoek de webpagina van de Service-Updates voor Azure voor de nieuwste updates met behulp van deze filters:

- Gefilterd op de [SQL Database-service](https://azure.microsoft.com/updates/?service=sql-database).
- Gefilterd op [aankondigingen](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) over de algemene beschikbaarheid (GA) van SQL Database-functies.

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Ondersteuning van SQL Server-functies in Azure SQL Database

De volgende tabel staan de belangrijkste functies van SQL Server en bevat informatie over of de functie wordt volledig of gedeeltelijk ondersteund en een koppeling naar meer informatie over de functie.

| **SQL-functie** | **Ondersteund in één databases/elastische pools** | **Ondersteund in Azure SQL Database of worden beheerd exemplaar** |
| --- | --- | --- |
| [Actieve Geo-replicatie](sql-database-active-geo-replication.md) | Ja, algemeen gebruik en bedrijfskritiek-service alleen lagen| Nee, Zie [automatische failover-groepen](sql-database-auto-failover-group.md) |
| [Groepen voor automatische failover](sql-database-auto-failover-group.md) | Ja, algemeen gebruik en bedrijfskritiek-service alleen lagen| [Ja-preview](sql-database-auto-failover-group.md)|
| [Altijd versleuteld](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja, Zie [certificatenstore](sql-database-always-encrypted.md) en [Key vault](sql-database-always-encrypted-azure-key-vault.md) | Ja, Zie [certificatenstore](sql-database-always-encrypted.md) en [Key vault](sql-database-always-encrypted-azure-key-vault.md) |
| [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |
| [Een database koppelen](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nee | Nee |
| [Toepassingsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja | Ja |
|[Controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ja](sql-database-auditing.md)| [Ja](sql-database-managed-instance-auditing.md) |
| [Automatische back-ups](sql-database-automated-backups.md) | Ja | Ja |
| [Automatisch afstemmen (plan forceren)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| [Ja](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automatisch afstemmen (indexen)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| Nee |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Ja | Ja |
| [BACPAC-bestand (exporteren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja, Zie [SQL-Database exporteren](sql-database-export.md) | Ja |
| [BACPAC-bestand (importeren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja, Zie [SQL-Database importeren](sql-database-import.md) | Ja |
| [Back-opdracht](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nee, alleen systeem gestarte automatische back-ups - Zie [geautomatiseerde back-ups](sql-database-automated-backups.md) | Systeem gestarte automatische back-ups en de gebruiker geïnitieerd kopie-alleen back-ups - Zie [back-up van verschillen](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Ingebouwde functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Gegevensregistratie wijzigen](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nee | Ja |
| [Wijzigingen bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |Ja |
| [Sortering - database](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Ja | Ja |
| [Sortering - server /-exemplaar](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | Nee | [Ja (preview)](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)|
| [Columnstore-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja, [Premium-laag, Standard-laag - S3 en hoger wordt de categorie Algemeen gebruik en bedrijfskritiek lagen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Ja |
| [Common language runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nee | Ja, Zie [CLR-verschillen](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Ingesloten databases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja | Ja |
| [Ingesloten gebruikers](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja | Ja |
| [Beheer van flow taaltrefwoorden](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja | Ja |
| [Query's tussen databases](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee, Zie [elastische query's](sql-database-elastic-query-overview.md) | Ja, plus [elastische query's](sql-database-elastic-query-overview.md) |
| [Transacties tussen databases](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee | Ja, Zie [verschillen van de server is gekoppeld](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [Cursors](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja |Ja |
| [Compressie van gegevens](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |Ja |
| [Database mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nee | Ja |
| [Data Migration Service (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Ja | Ja |
| [Databasespiegeling](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nee | Nee |
| [Databaseconfiguratie-instellingen](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja | Ja |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nee | Nee |
| [Momentopnamen van databases](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nee | Nee |
| [Gegevenstypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |Ja |
| [DBCC-instructies](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | De meeste - Zie afzonderlijke instructies | Ja, Zie [DBCC verschillen](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL-componenten](https://docs.microsoft.com/sql/t-sql/statements/statements) | De meeste - Zie afzonderlijke instructies | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [DDL-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Alleen de database |  Ja |
| [Partitie gedistribueerde weergaven](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Nee | Ja |
| [Gedistribueerde transacties: MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nee, Zie [elastische transacties](sql-database-elastic-transactions-overview.md) |  Nee, Zie [elastische transacties](sql-database-elastic-transactions-overview.md) |
| [DML-instructies](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ja | Ja |
| [DML-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | De meeste - Zie afzonderlijke instructies |  Ja |
| [DMV's](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | De meeste - Zie afzonderlijke DMV 's |  Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
|[Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Ja](sql-database-dynamic-data-masking-get-started.md)| [Ja](sql-database-dynamic-data-masking-get-started.md) |
| [Elastische pools](sql-database-elastic-pool.md) | Ja | Ingebouwde-één beheerde instantie kan meerdere databases die delen van dezelfde groep resources bevatten. |
| [Meldingen van gebeurtenissen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Nee, Zie [waarschuwingen](sql-database-insights-alerts-portal.md) | Nee |
| [Expressies](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja | Ja |
| [Uitgebreide gebeurtenissen](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Sommige - Zie [Extended events in SQL-Database](sql-database-xevent-db-diff-from-svr.md) | Ja, Zie [uitgebreide gebeurtenissen-verschillen](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nee | Nee |
[Bestanden en bestandsgroepen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Alleen primaire bestandsgroep | Ja |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nee | Nee |
| [Zoeken in volledige tekst](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Woordafbrekingen van derden worden niet ondersteund. |Woordafbrekingen van derden worden niet ondersteund. |
| [Functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Geo-restore](sql-database-recovery-using-backups.md#geo-restore) | Ja, algemeen gebruik en bedrijfskritiek-service alleen lagen | Nee – u kunt herstellen dabtabaseback volledige back-ups die u regelmatig - Zie [back-up van verschillen](sql-database-managed-instance-transact-sql-information.md#backup) en [herstellen verschillen](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Grafische verwerking](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Ja | Ja |
| [Optimalisatie in het geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja, [Premium en bedrijfskritiek-lagen](sql-database-in-memory.md) | Ja, [alleen de bedrijfskritieke laag Business](sql-database-managed-instance.md) |
| [Ondersteuning voor JSON-gegevens](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Ja](sql-database-json-features.md) | [Ja](sql-database-json-features.md) |
| [Taalelementen](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | De meeste - Zie afzonderlijke elementen |  Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Gekoppelde servers](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee, Zie [elastische query's](sql-database-elastic-query-horizontal-partitioning.md) | Alleen voor SQL Server en SQL-Database |
| [Back-upfunctie](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |[Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nee | Nee |
| [Minimale bulkimport aanmelden](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nee | Nee |
| [Systeemgegevens wijzigen](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nee | Ja |
| [Online-indexbewerkingen](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ja | Ja |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Nee|Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Ja|Ja|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Nee|Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Nee|Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Ja|Ja|
| [Operators](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | De meeste - Zie afzonderlijke operators |Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Partitioneren](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja | Ja |
| [Punt in tijd database terugzetten](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja, voor algemeen gebruik en bedrijfskritiek Servicelagen alleen - Zie [SQL-Database herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) | Ja, Zie [SQL-Database herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nee | Nee |
| [Beheer op basis van beleid](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nee | Nee |
| [Predicaten](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ja | Ja |
| [Querymeldingen](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | Nee | Ja |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Preview-versie; Zie [wat is er nieuw in machine learning](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Nee |
| [Resourceregeling](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nee | Ja |
| [RESTORE-instructies](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nee | Ja, Zie [verschillen herstellen](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Database herstellen vanuit back-up](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | In de alleen - de geautomatiseerde back-ups zien [SQL-Database herstellen](sql-database-recovery-using-backups.md) | Automatische back-ups - Zie [SQL-databaseherstel](sql-database-recovery-using-backups.md) en volledige back-ups - Zie [back-up van verschillen](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja | Ja |
| [Semantisch zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nee | Nee |
| [Volgnummers](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nee | Ja, Zie [verschillen van de Service Broker](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Server-configuratie-instellingen](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nee | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Instructies instellen](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | De meeste - Zie afzonderlijke instructies | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Ja | Ja |
| [Ruimtelijk](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja | Ja |
| [SQL Data Sync](sql-database-get-started-sql-data-sync.md) | Ja | Nee |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nee, Zie [elastische taken](sql-database-elastic-jobs-getting-started.md) | Ja, Zie [verschillen van de SQL Server Agent](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nee, Zie [Azure analyseservices](https://azure.microsoft.com/services/analysis-services/) | Nee, Zie [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [Controleren voor SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Nee, Zie [SQL Database auditing](sql-database-auditing.md) | Ja, Zie [verschillen controleren](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Ja | Ja |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ja, met een beheerde SSIS in Azure Data Factory (ADF)-omgeving, waarin de pakketten worden opgeslagen in SSISDB wordt gehost door Azure SQL Database en worden uitgevoerd op Azure SSIS Integration Runtime (IR), Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Als u wilt vergelijken van de SSIS-functies in SQL Database-server en de Managed Instance, Zie [vergelijken Azure SQL Database single databases/elastische pools en Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). | Ja, met een beheerde SSIS in Azure Data Factory (ADF)-omgeving, waarin de pakketten worden opgeslagen in SSISDB wordt gehost door de Managed Instance en worden uitgevoerd op Azure SSIS Integration Runtime (IR), Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Als u wilt vergelijken van de SSIS-functies in SQL-Database en de Managed Instance, Zie [vergelijken Azure SQL Database single databases/elastische pools en Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Ja | Ja |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja | Ja |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nee, Zie [Extended events](sql-database-xevent-db-diff-from-svr.md) | Ja |
| [SQL Server-replicatie](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Alleen voor transactionele abonnees en abonnees met momentopnamereplicatie](sql-database-cloud-migrate.md) | Ja (openbare preview) - [replicatie met SQL Database Managed Instance](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Niet - [Zie Power BI](https://docs.microsoft.com/power-bi/) | Niet - [Zie Power BI](https://docs.microsoft.com/power-bi/) |
| [Opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja | Ja |
| [Het systeem opgeslagen functies](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | De meeste - Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [In het systeem opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Sommige - Zie afzonderlijke opgeslagen procedures | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Systeemtabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Sommige - Zie afzonderlijke tabellen | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Systeemcatalogusweergaven](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Sommige - Zie afzonderlijke weergaven | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Lokale en database-scoped globale tijdelijke tabellen | Lokale en exemplaar scoped globale tijdelijke tabellen |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Ja](sql-database-temporal-tables.md) | [Ja](sql-database-temporal-tables.md) |
|Detectie van bedreigingen|  [Ja](sql-database-threat-detection.md)|[Ja](sql-database-managed-instance-threat-detection.md)|
| [Traceervlaggen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Nee | Nee |
| [Variabelen](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | Ja |
| [TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja, algemeen gebruik en bedrijfskritiek-service alleen lagen| [Ja](transparent-data-encryption-azure-sql.md) |
[VNet](../virtual-network/virtual-networks-overview.md) | Gedeeltelijk - Zie [VNet-eindpunten](sql-database-vnet-service-endpoint-rule-overview.md) | Ja, alleen Resource Manager-model |
| [Windows Server Failover Clustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |
| [XML-indexen](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja | Ja |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie voor meer informatie over een beheerd exemplaar [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
