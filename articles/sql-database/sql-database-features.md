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
ms.custom: features
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: a1ede93b5aacf0d8a5bcf83f208f72be328ee72f
ms.lasthandoff: 04/15/2017


---
# <a name="azure-sql-database-features"></a>Azure SQL Database-functies

In de volgende tabellen worden de belangrijkste functies van Azure SQL Database en de equivalente functies van SQL Server weergegeven. Ook wordt hier informatie gegeven over of een bepaalde functie wordt ondersteund en er wordt een koppeling gegeven naar meer informatie over de functie op elk platform. Zie [Resolving Transact-SQL differences during migration to SQL Database](sql-database-transact-sql-information.md) (Transact-SQL-verschillen oplossen tijdens de migratie naar de SQL Database) voor Transact-SQL-verschillen om te overwegen wanneer u een bestaande databaseoplossing migreert.

Er worden doorlopend extra functies toegevoegd aan Azure SQL Database. Het wordt daarom aangeraden om onze Azure-webpagina Service-updates regelmatig te bezoeken en filters te gebruiken:

* Gefilterd op de [SQL Database-service](https://azure.microsoft.com/updates/?service=sql-database).
* Gefilterd op [aankondigingen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) over de algemene beschikbaarheid (GA) van SQL Database-functies.

| **Functie** | **SQL Server** | **Azure SQL Database** | 
| --- | :---: | :---: | 
| Actieve Geo-replicatie | Niet ondersteund - Zie [AlwaysOn-beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Ondersteund](sql-database-geo-replication-overview.md)
| Altijd versleuteld | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ondersteund - Zie [Cert Store](sql-database-always-encrypted.md) en [Key Vault](sql-database-always-encrypted-azure-key-vault.md)|
| AlwaysOn-beschikbaarheidsgroepen | [Ondersteund](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Niet ondersteund - Zie [Actieve geo-replicatie](sql-database-geo-replication-overview.md) |
| Een database koppelen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Niet ondersteund |
| Toepassingsrollen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) |
| Automatisch schalen | Niet ondersteund | Ondersteund - Zie [Servicelagen](sql-database-service-tiers.md) |
| Azure Active Directory | Niet ondersteund | [Ondersteund](sql-database-aad-authentication.md) |
| Azure Data Factory | [Ondersteund](../data-factory/data-factory-introduction.md) | [Ondersteund](../data-factory/data-factory-introduction.md) |
| Controleren | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ondersteund](sql-database-auditing.md) |
| BACPAC-bestand (exporteren) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | [Ondersteund](sql-database-export.md) |
| BACPAC-bestand (importeren) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | [Ondersteund](sql-database-import.md) |
| BACKUP | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Niet ondersteund |
| Ingebouwde functies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie [Afzonderlijke functies] (https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Gegevensregistratie wijzigen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Niet ondersteund |
| Wijzigingen bijhouden | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) |
| Sorteringsinstructies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/collations) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/collations) |
| Columnstore-indexen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | [Alleen de Premium Edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| Common Language Runtime (CLR) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Niet ondersteund |
| Ingesloten databases | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) |
| Ingesloten gebruikers | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | [Ondersteund](sql-database-manage-logins.md#non-administrator-users) |
| Trefwoorden voor de taal voor het bepalen van de uitvoeringsvolgorde | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) |
| Query's tussen databases | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Gedeeltelijk - Zie [Elastische query's](sql-database-elastic-query-overview.md) |
| Cursors | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | 
| Gegevenscompressie | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) |
| Databaseback-ups | [Door gebruiker beheerd](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) | [Beheerd door de SQL Database-service](sql-database-automated-backups.md) |
| Database-e-mail | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Niet ondersteund |
| Databasespiegeling | [Ondersteund](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Niet ondersteund |
| Configuratie-instellingen van de database | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Data Quality Services (DQS) | [Ondersteund](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Niet ondersteund |
| Momentopnamen van databases | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Niet ondersteund |
| Gegevenstypen | [Ondersteund](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) |  
| DBCC-instructies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | De meeste - Zie [Afzonderlijke instructies](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) |
| DDL-instructies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/statements) | De meeste - Zie [Afzonderlijke instructies](https://docs.microsoft.com/sql/t-sql/statements/statements)
| DDL-triggers | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | [Alleen de database](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) |
| Gedistribueerde transacties | [MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Alleen beperkte intra-SQL Database-scenario's |
| DML-instructies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/queries/queries) | De meeste - Zie [Afzonderlijke instructies] (https://docs.microsoft.com/sql/t-sql/queries/queries) |
| DML-triggers | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| DMV’s | [Alle](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Sommige - Zie [Afzonderlijke DMV's](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) |
| Pools voor Elastic Database | Niet ondersteund | [Ondersteund](sql-database-elastic-pool.md) |
| Elastische taken | Niet ondersteund - Zie [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | [Ondersteund](sql-database-elastic-jobs-getting-started.md) | 
| Elastische query’s | Niet ondersteund - Zie [Query’s tussen databases](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | [Ondersteund](sql-database-elastic-query-overview.md) |
| Gebeurtenismeldingen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | [Ondersteund](sql-database-insights-alerts-portal.md) |
| Expressies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |
| Uitgebreide gebeurtenissen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Sommige - Zie [Afzonderlijke gebeurtenissen](sql-database-xevent-db-diff-from-svr.md) |
| Uitgebreide opgeslagen procedures | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Niet ondersteund |
| Bestanden en bestandsgroepen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | [Alleen primair](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) |
| Bestandsstroom | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Niet ondersteund |
| Zoekopdracht in volledige tekst | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | [Niet-ondersteunde woordafbrekingen van derden](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |
| Functies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie [Afzonderlijke functies](https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Optimalisatie in het geheugen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | [Alleen de Premium Edition](sql-database-in-memory.md) |
| Taken | Zie [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Zie [Elastische taken](sql-database-elastic-jobs-getting-started.md) |
| Ondersteuning voor JSON-gegevens | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Ondersteund](sql-database-json-features.md) |
| Taalelementen | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | De meeste - Zie [Afzonderlijke elementen](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) |  
| Gekoppelde servers | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Niet ondersteund - Zie [Elastische query](sql-database-elastic-query-horizontal-partitioning.md) |
| Back-upfunctie voor logboekbestanden | [Ondersteund](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Niet ondersteund - Zie [Actieve geo-replicatie](sql-database-geo-replication-overview.md) |
| Master Data Services (MDS) | [Ondersteund](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Niet ondersteund |
| Minimale bulkimport voor aanmelden | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Niet ondersteund |
| Systeemgegevens wijzigen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Niet ondersteund |
| Online-indexbewerkingen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | [Ondersteund - De grootte van de transactie wordt beperkt door de servicelaag](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) |
| Operators | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | De meeste - Zie [Afzonderlijke operators](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) |
| Een database herstellen naar een bepaald tijdstip | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | [Ondersteund](sql-database-recovery-using-backups.md#point-in-time-restore) |
| PolyBase | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Niet ondersteund
| Op beleid gebaseerd beheer | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Niet ondersteund |
| Predicaten | [Ondersteund](https://docs.microsoft.com/sql/t-sql/queries/predicates) | De meeste - Zie [Afzonderlijke predicaten](https://docs.microsoft.com/sql/t-sql/queries/predicates)
| R Services | [Ondersteund](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services)
| Resource Governor | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Niet ondersteund |
| RESTORE-instructies | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Niet ondersteund | 
| Database terugzetten via back-up | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | [Alleen via ingebouwde back-ups](sql-database-recovery-using-backups.md) |
| Beveiliging op rijniveau | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) |
| Semantische zoekopdrachten | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Niet ondersteund |
| Volgnummers | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) |
| Service Broker | [Ondersteund](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Niet ondersteund |
| Serverconfiguratie-instellingen | [Ondersteund](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Niet ondersteund - Zie [Databaseconfiguratie-opties](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Instructies instellen | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | De meeste - Zie [Afzonderlijke instructies](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) 
| Ruimtelijk | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) |
| SQL Server Agent | [Ondersteund](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Niet ondersteund - Zie [Elastische taken](sql-database-elastic-jobs-getting-started.md) |
| SQL Server Analysis Services (SSAS) | [Ondersteund](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Niet ondersteund - Zie [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Ondersteund](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Niet ondersteund - Zie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| SQL Server PowerShell | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) |
| SQL Server Profiler | [Ondersteund](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Niet ondersteund - Zie [Uitgebreid gebeurtenissen](sql-database-xevent-db-diff-from-svr.md) |
| SQL Server-replicatie | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Alleen voor transactionele abonnees en abonnees met momentopnamereplicatie](sql-database-cloud-migrate.md) |
| SQL Server Reporting Services (SSRS) | [Ondersteund](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Niet ondersteund |
| Opgeslagen procedures | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) |
| In het systeem opgeslagen functies | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Sommige - Zie [Afzonderlijke functie](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) |
| In het systeem opgeslagen procedures | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Sommige - Zie [Afzonderlijke opgeslagen procedure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Systeemtabellen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Sommige - Zie [Afzonderlijke tabel](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Systeemcatalogusweergaven | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Sommige - Zie [Afzonderlijke weergaven](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql)
| Tabellen partitioneren | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ondersteund - [Alleen primaire bestandsgroep](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) |
| Tijdelijke tabellen | [Lokaal en wereldwijd](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | [Alleen lokaal](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) |
| Tijdelijke tabellen | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Ondersteund](sql-database-temporal-tables.md) |
| Transacties | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) |
| Variabelen | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | 
| Transparent Data Encryption (TDE)  | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | [Ondersteund](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Windows Server-failoverclusters | [Ondersteund](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Niet ondersteund - Zie [Actieve geo-replicatie](sql-database-geo-replication-overview.md) |
| XML-indexen | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | [Ondersteund](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie [Resolving Transact-SQL differences during migration to SQL Database](sql-database-transact-sql-information.md) (Transact-SQL-verschillen oplossen tijdens de migratie naar de SQL Database) voor meer informatie over Transact-SQL-ondersteuning en -verschillen.

