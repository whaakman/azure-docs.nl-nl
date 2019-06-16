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
ms.reviewer: bonova, sstein
manager: craigg
ms.date: 05/10/2019
ms.openlocfilehash: 4d8d2fd9a7408bb77939c9a1c8fdd67251282f49
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479212"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Vergelijking van functies: Azure SQL Database versus SQL Server

Azure SQL Database deelt een algemene codebasis met SQL Server. De functies van SQL Server wordt ondersteund door Azure SQL Database, is afhankelijk van het type van Azure SQL-database die u maakt. Met Azure SQL Database, kunt u een database maken als onderdeel van een [beheerd exemplaar](sql-database-managed-instance.md), als een individuele database, of als onderdeel van een elastische pool.

Microsoft blijft functies toevoegen aan Azure SQL Database. Bezoek de webpagina van de Service-Updates voor Azure voor de nieuwste updates met behulp van deze filters:

- Gefilterd op de [SQL Database-service](https://azure.microsoft.com/updates/?service=sql-database).
- Gefilterd op [aankondigingen](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) over de algemene beschikbaarheid (GA) van SQL Database-functies.

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Ondersteuning van SQL Server-functies in Azure SQL Database

De volgende tabel staan de belangrijkste functies van SQL Server en bevat informatie over of de functie wordt volledig of gedeeltelijk ondersteund en een koppeling naar meer informatie over de functie.

| **SQL-functie** | **Ondersteund door individuele databases en elastische pools** | **Ondersteund door beheerde exemplaren** |
| --- | --- | --- |
| [Actieve Geo-replicatie](sql-database-active-geo-replication.md) | Ja, alle lagen dan grootschalige service | Nee, Zie [automatische failover groups(preview)](sql-database-auto-failover-group.md) als alternatief |
| [Groepen voor automatische failover](sql-database-auto-failover-group.md) | Ja, alle lagen dan grootschalige service | Ja, in [openbare preview](sql-database-auto-failover-group.md)|
| [Altijd versleuteld](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja, Zie [certificatenstore](sql-database-always-encrypted.md) en [Key vault](sql-database-always-encrypted-azure-key-vault.md) | Ja, Zie [certificatenstore](sql-database-always-encrypted.md) en [Key vault](sql-database-always-encrypted-azure-key-vault.md) |
| [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database en [kan niet worden beheerd door de gebruiker](sql-database-managed-instance-transact-sql-information.md#always-on-availability). Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |
| [Een database koppelen](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nee | Nee |
| [Toepassingsrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja | Ja |
| [Controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ja](sql-database-auditing.md)| [Ja](sql-database-managed-instance-auditing.md), met enkele [verschillen](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [Automatische back-ups](sql-database-automated-backups.md) | Ja. Volledige back-ups gaat elke zeven dagen, differentiële 12 uur en logboekback-ups elke 5-10 minuten. | Ja. Volledige back-ups gaat elke zeven dagen, differentiële 12 uur en logboekback-ups elke 5-10 minuten. |
| [Automatisch afstemmen (plan forceren)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| [Ja](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automatisch afstemmen (indexen)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ja](sql-database-automatic-tuning.md)| Nee |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Ja | Ja |
| [BACPAC-bestand (exporteren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja, Zie [SQL-Database exporteren](sql-database-export.md) | Ja, Zie [SQL-Database exporteren](sql-database-export.md) |
| [BACPAC-bestand (importeren)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja, Zie [SQL-Database importeren](sql-database-import.md) | Ja, Zie [SQL-Database importeren](sql-database-import.md) |
| [Back-opdracht](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nee, alleen systeem gestarte automatische back-ups - Zie [geautomatiseerde back-ups](sql-database-automated-backups.md) | Ja, door de gebruiker geïnitieerde kopie-alleen back-ups naar Azure Blob-opslag (automatische back-ups kunnen niet worden gestart door gebruiker) - Zie [back-up van verschillen](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Ingebouwde functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) | 
| [BULK INSERT-instructie](https://docs.microsoft.com/sql/relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server) | Ja, maar alleen vanuit Azure Blob-opslag als een bron. | Ja, maar alleen in Azure Blob-opslag als een bron - zien [verschillen](sql-database-managed-instance-transact-sql-information.md#bulk-insert--openrowset). |
| [Certificaten en asymmetrische sleutels](https://docs.microsoft.com/sql/relational-databases/security/sql-server-certificates-and-asymmetric-keys) | Ja, zonder toegang tot het bestandssysteem voor `BACKUP` en `CREATE` bewerkingen. | Ja, zonder toegang tot het bestandssysteem voor `BACKUP` en `CREATE` bewerkingen - Zie [verschillen van het certificaat](sql-database-managed-instance-transact-sql-information.md#certificates). | 
| [Gegevensregistratie wijzigen](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nee | Ja |
| [Wijzigingen bijhouden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |Ja |
| [Sortering - database](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Ja | Ja |
| [Sortering - server /-exemplaar](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | Nee, logische server standaardsortering `SQL_Latin1_General_CP1_CI_AS` wordt altijd gebruikt. | Ja, kunnen worden ingesteld wanneer de [exemplaar gemaakt](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) en later kan niet worden bijgewerkt. |
| [Columnstore-indexen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja, [Premium-laag, Standard-laag - S3 en hoger wordt de categorie Algemeen gebruik en bedrijfskritiek lagen](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Ja |
| [Common language runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nee | Ja, maar geen toegang tot bestandssysteem in `CREATE ASSEMBLY` statement - Zie [CLR-verschillen](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Ingesloten databases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja | Op dit moment geen [vanwege een fout in de HERSTELOPDRACHT met inbegrip van punt-in-time RESTORE](sql-database-managed-instance-transact-sql-information.md#cant-restore-contained-database). Dit is een fout die snel worden opgelost. |
| [Ingesloten gebruikers](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja | Ja |
| [Beheer van flow taaltrefwoorden](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja | Ja |
| [Referenties](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/credentials-database-engine) | Ja, maar alleen [referenties in het databasebereik](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql). | Ja, maar alleen **Azure Key Vault** en `SHARED ACCESS SIGNATURE` worden ondersteund Zie [details](sql-database-managed-instance-transact-sql-information.md#credential) |
| [Query's tussen databases](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee, Zie [elastische query's](sql-database-elastic-query-overview.md) | Ja, plus [elastische query's](sql-database-elastic-query-overview.md) |
| [Transacties tussen databases](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee | Ja, in de instantie. Zie [gekoppelde server verschillen](sql-database-managed-instance-transact-sql-information.md#linked-servers) voor cross-instance-query's. |
| [Cursors](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja |Ja |
| [Compressie van gegevens](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |Ja |
| [Database mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nee | Ja |
| [Data Migration Service (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Ja | Ja |
| [Databasespiegeling](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nee | [Nee](sql-database-managed-instance-transact-sql-information.md#database-mirroring) |
| [Databaseconfiguratie-instellingen](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja | Ja |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nee | Nee |
| [Momentopnamen van databases](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nee | Nee |
| [Gegevenstypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |Ja |
| [DBCC-instructies](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | De meeste - Zie afzonderlijke instructies | Ja, Zie [DBCC verschillen](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL-componenten](https://docs.microsoft.com/sql/t-sql/statements/statements) | De meeste - Zie afzonderlijke instructies | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [DDL-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Alleen de database |  Ja |
| [Partitie gedistribueerde weergaven](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Nee | Ja |
| [Gedistribueerde transacties: MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nee, Zie [elastische transacties](sql-database-elastic-transactions-overview.md) |  Nee, Zie [verschillen van de server is gekoppeld](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [DML-instructies](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ja | Ja |
| [DML-triggers](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | De meeste - Zie afzonderlijke instructies |  Ja |
| [DMV's](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | De meeste - Zie afzonderlijke DMV 's |  Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Dynamische gegevensmaskering](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Ja](sql-database-dynamic-data-masking-get-started.md)| [Ja](sql-database-dynamic-data-masking-get-started.md) |
| [Elastische pools](sql-database-elastic-pool.md) | Ja | Ingebouwde-één beheerde instantie kan meerdere databases die delen van dezelfde groep resources bevatten. |
| [Meldingen van gebeurtenissen](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Nee, Zie [waarschuwingen](sql-database-insights-alerts-portal.md) | Nee |
| [Expressies](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja | Ja |
| [Uitgebreide gebeurtenissen](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Sommige - Zie [Extended events in SQL-Database](sql-database-xevent-db-diff-from-svr.md) | Ja, Zie [uitgebreide gebeurtenissen-verschillen](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nee | Nee |
| [Bestanden en bestandsgroepen](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Alleen primaire bestandsgroep | Ja. Bestandspaden worden automatisch toegewezen en de bestandslocatie kan niet worden opgegeven `ALTER DATABASE ADD FILE` [instructie](sql-database-managed-instance-transact-sql-information.md#alter-database-statement).  |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nee | [Nee](sql-database-managed-instance-transact-sql-information.md#filestream-and-filetable) |
| [Zoeken in volledige tekst](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Ja, maar woordafbrekingen van derden worden niet ondersteund. | Ja, maar [woordafbrekingen van derden worden niet ondersteund.](sql-database-managed-instance-transact-sql-information.md#full-text-semantic-search) |
| [Functies](https://docs.microsoft.com/sql/t-sql/functions/functions) | De meeste - Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Geo-restore](sql-database-recovery-using-backups.md#geo-restore) | Ja, alle lagen dan grootschalige service | Ja - met behulp van [Azure PowerShell](https://medium.com/azure-sqldb-managed-instance/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa). |
| [Grafische verwerking](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Ja | Ja |
| [Optimalisatie in het geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja, [Premium en bedrijfskritiek-lagen](sql-database-in-memory.md) | Ja, [alleen de bedrijfskritieke laag Business](sql-database-managed-instance.md) |
| [Ondersteuning voor JSON-gegevens](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Ja](sql-database-json-features.md) | [Ja](sql-database-json-features.md) |
| [Taalelementen](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | De meeste - Zie afzonderlijke elementen |  Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Gekoppelde servers](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nee, Zie [elastische query's](sql-database-elastic-query-horizontal-partitioning.md) | Alleen [SQL Server en SQL-Database](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [Back-upfunctie](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) | Systeemeigen ingebouwde als onderdeel van de DMS-migratieproces. [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database en niet-back-upfunctie gebruiken als alternatief voor HA wordt aanbevolen. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |
| [Aanmeldingen en gebruikers](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) | Ja, maar `CREATE` en `ALTER` aanmelding instructies bieden alle opties (geen Windows en Azure Active Directory-aanmelding op serverniveau). `EXECUTE AS LOGIN` wordt niet ondersteund - gebruiken `EXECUTE AS USER` in plaats daarvan.  | Ja, met enkele [verschillen](sql-database-managed-instance-transact-sql-information.md#logins-and-users). Windows-aanmeldingen worden niet ondersteund en ze moeten worden vervangen door Azure Active Directory-aanmeldingen. |
| [Langetermijnretentie - LTR](sql-database-long-term-retention.md) | Ja, blijf automatisch genomen back-ups op 10 jaar. | Nog niet. Gebruik `COPY_ONLY` [handmatige back-ups](sql-database-managed-instance-transact-sql-information.md#backup) als tijdelijke oplossing. |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nee | Nee |
| [Minimale bulkimport aanmelden](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nee | Nee |
| [Systeemgegevens wijzigen](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nee | Ja |
| [OLE Automation](https://docs.microsoft.com/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option) | Nee | Nee |
| [Online-indexbewerkingen](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ja | Ja |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Nee|Ja, alleen voor andere Azure SQL-Databases en SQL-Servers. Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Ja|Ja|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Nee|Ja, alleen voor andere Azure SQL-Databases en SQL-Servers. Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Ja, alleen als u wilt importeren uit Azure Blob storage. |Ja, alleen aan andere Azure SQL-Databases en SQL-Servers, en om te importeren uit Azure Blob-opslag. Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Ja|Ja|
| [Operators](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | De meeste - Zie afzonderlijke operators |Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Partitioneren](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ja | Ja |
| Openbaar IP-adres | Ja. De toegang kan worden beperkt met behulp van de firewall of service-eindpunten.  | Ja. Moet expliciet zijn ingeschakeld en poort 3342 moet zijn ingeschakeld in de NSG-regels. Openbaar IP-adres kan worden uitgeschakeld, indien nodig. Zie [openbaar eindpunt](sql-database-managed-instance-public-endpoint-securely.md) voor meer informatie. | 
| [Punt in tijd database terugzetten](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja, alle service-lagen dan grootschalige - Zie [SQL-Database herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) | Ja, Zie [SQL-Database herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nee | Nee |
| [Beheer op basis van beleid](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nee | Nee |
| [Predicaten](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ja | Ja |
| [Querymeldingen](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | Nee | Ja |
| [Inzicht in queryprestaties](sql-database-query-performance.md) | Ja | Nee |
| [R Services](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Ja, in [openbare preview](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Nee |
| [Resourceregeling](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nee | Ja |
| [RESTORE-instructies](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nee | Ja, met verplicht `FROM URL` opties voor de back-ups van bestanden in Azure Blob-opslag worden geplaatst. Zie [verschillen herstellen](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Database herstellen vanuit back-up](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | In de alleen - de geautomatiseerde back-ups zien [SQL-Database herstellen](sql-database-recovery-using-backups.md) | Automatische back-ups - Zie [SQL-databaseherstel](sql-database-recovery-using-backups.md) en volledige back-ups geplaatst in Azure Blob Storage - Zie [back-up van verschillen](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja | Ja |
| [Semantisch zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nee | Nee |
| [Volgnummers](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nee | Ja, maar alleen in de instantie. Zie [verschillen van de Service Broker](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Server-configuratie-instellingen](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nee | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Instructies instellen](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | De meeste - Zie afzonderlijke instructies | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | [Ja](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) | Ja [versie 150](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) |
| [Ruimtelijk](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja | Ja |
| [SQL Analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) | Ja | Ja |
| [SQL Data Sync](sql-database-get-started-sql-data-sync.md) | Ja | Nee |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nee, Zie [elastische taken](elastic-jobs-overview.md) | Ja, Zie [verschillen van de SQL Server Agent](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nee, [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) is een afzonderlijke Azure-cloud-service. | Nee, [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) is een afzonderlijke Azure-cloud-service. |
| [Controleren voor SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Nee, Zie [SQL Database auditing](sql-database-auditing.md) | Ja, Zie [verschillen controleren](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Ja | Ja |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ja, met een beheerde SSIS in Azure Data Factory (ADF)-omgeving, waarin de pakketten worden opgeslagen in SSISDB wordt gehost door Azure SQL Database en worden uitgevoerd op Azure SSIS Integration Runtime (IR), Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Als u wilt vergelijken van de SSIS-functies in SQL Database-server en de Managed Instance, Zie [vergelijken Azure SQL Database single databases/elastische pools en Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). | Ja, met een beheerde SSIS in Azure Data Factory (ADF)-omgeving, waarin de pakketten worden opgeslagen in SSISDB wordt gehost door de Managed Instance en worden uitgevoerd op Azure SSIS Integration Runtime (IR), Zie [Azure-SSIS IR maken in ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Als u wilt vergelijken van de SSIS-functies in SQL-Database en de Managed Instance, Zie [vergelijken Azure SQL Database single databases/elastische pools en Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Ja | Ja [versie 18,0 en hoger](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja | Ja |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nee, Zie [Extended events](sql-database-xevent-db-diff-from-svr.md) | Ja |
| [SQL Server-replicatie](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Alleen voor transactionele abonnees en abonnees met momentopnamereplicatie](sql-database-single-database-migrate.md) | Ja, in [openbare preview](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Niet - [Zie Power BI](https://docs.microsoft.com/power-bi/) | Niet - [Zie Power BI](https://docs.microsoft.com/power-bi/) |
| [Opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja | Ja |
| [Het systeem opgeslagen functies](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | De meeste - Zie afzonderlijke functies | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [In het systeem opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Sommige - Zie afzonderlijke opgeslagen procedures | Ja, Zie [opgeslagen procedures, functies, triggers verschillen](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-and-triggers) |
| [Systeemtabellen](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Sommige - Zie afzonderlijke tabellen | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Systeemcatalogusweergaven](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Sommige - Zie afzonderlijke weergaven | Ja, Zie [T-SQL-verschillen](sql-database-managed-instance-transact-sql-information.md) |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Lokale en database-scoped globale tijdelijke tabellen | Lokale en exemplaar scoped globale tijdelijke tabellen |
| [Tijdelijke tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Ja](sql-database-temporal-tables.md) | [Ja](sql-database-temporal-tables.md) |
| Tijdzone keuze | Nee | [Yes(preview)](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-timezone) |
| Detectie van bedreigingen|  [Ja](sql-database-threat-detection.md)|[Ja](sql-database-managed-instance-threat-detection.md)|
| [Traceervlaggen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Nee | Nee |
| [Variabelen](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | Ja |
| [TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja, algemeen gebruik en bedrijfskritiek-service alleen lagen| [Ja](transparent-data-encryption-azure-sql.md) |
| [VNet](../virtual-network/virtual-networks-overview.md) | Gedeeltelijk - Zie [VNet-eindpunten](sql-database-vnet-service-endpoint-rule-overview.md) | Ja, alleen Resource Manager-model |
| [Windows Server Failover Clustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) | [Hoge beschikbaarheid](sql-database-high-availability.md) is opgenomen in elke database. Herstel na noodgevallen wordt besproken in [overzicht van bedrijfscontinuïteit met Azure SQL Database](sql-database-business-continuity.md) |
| [XML-indexen](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja | Ja |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service Azure SQL Database
- Zie voor meer informatie over een beheerd exemplaar [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
