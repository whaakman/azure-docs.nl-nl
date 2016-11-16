# Overzicht
## [Wat is SQL Database?](sql-database-technical-overview.md)
## [Veelgestelde vragen over SQL Database](sql-database-faq.md)
## Functies
### [Servicelagen](sql-database-service-tiers.md)
### [Database Transaction Units](sql-database-what-is-a-dtu.md)
### [DTU-benchmarkoverzicht](sql-database-benchmark-overview.md)
### [Beheerhulpprogramma's](sql-database-manage-overview.md)
## Overweging en beperkingen
### [SQL Database versus SQL op een VM](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL-verschillen](sql-database-transact-sql-information.md)
### [Bronlimieten](sql-database-resource-limits.md)
### [Algemene beperkingen](sql-database-general-limitations.md)
### [Richtlijnen voor beveiliging](sql-database-security-guidelines.md)

## Voordelen
### [Kan leren en zich aanpassen](sql-database-learn-and-adapt.md)
### [Schaalt onmiddellijk](sql-database-scale-on-the-fly.md)
### [Bouwt apps voor meerdere tenants](sql-database-build-multi-tenant-apps.md)
### [Beveiligt en beschermt](sql-database-helps-secures-and-protects.md)
### [Werkt in uw omgeving](sql-database-works-in-your-environment.md)

## Scenario's

### Servers, pools, databases en firewalls
#### [Een pool voor Elastic Database maken](sql-database-elastic-pool-guidance.md)
#### [Pools voor Elastic Database](sql-database-elastic-pool.md)
#### [Automatisering](sql-database-manage-automation.md)

### Uitgeschaalde databases
#### [Overzicht](sql-database-elastic-scale-introduction.md)
#### [Schaalbare clouddatabases bouwen](sql-database-elastic-database-client-library.md)
#### [Taken tussen databases](sql-database-elastic-jobs-overview.md)
#### [Woordenlijst voor hulpprogramma's voor Elastic Database](sql-database-elastic-scale-glossary.md)
#### [Veelgestelde vragen](sql-database-elastic-scale-faq.md)

### Toegang en machtigingen
#### [Overzicht](sql-database-security.md)
#### [Azure Security Center voor Azure SQL Database](../security-center/security-center-sql-database.md?toc=%2fazure%2fsql-database%2ftoc.json)
#### [SQL-beveiligingscentrum](https://msdn.microsoft.com/library/azure/bb510589)

### Bedrijfscontinuïteit
#### [Overzicht](sql-database-business-continuity.md)
#### [Databaseback-ups](sql-database-automated-backups.md)
#### [Databaseherstel met behulp van back-ups](sql-database-recovery-using-backups.md)
#### [Verificatievereisten voor herstel na noodgevallen](sql-database-geo-replication-security-config.md)
#### [Ontwerpscenario's voor bedrijfscontinuïteit](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategieën voor herstel na noodgevallen met pools voor Elastic Database](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Rolling upgrades](sql-database-manage-application-rolling-upgrade.md)

### [Databaseontwikkeling](sql-database-develop-overview.md)
### [SQL Server-databases migreren](sql-database-cloud-migrate.md)

## Klantimplementaties
### [Daxko/CSI-software](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)


# Aan de slag
## [Snel starten van oplossing](sql-database-solution-quick-starts.md)
## Een SQL-database maken
### [Azure Portal](sql-database-get-started.md)
### [C#](sql-database-get-started-csharp.md)
### [PowerShell](sql-database-get-started-powershell.md)
## Uitgeschaalde databases maken en beheren
### [Elastisch schalen](sql-database-elastic-scale-get-started.md)
### [Elastische taken](sql-database-elastic-jobs-getting-started.md)
### [Databaseoverschrijdende rapporten](sql-database-elastic-query-getting-started.md)
### [Databaseoverschrijdende query's](sql-database-elastic-query-getting-started-vertical.md)
## Databases controleren en afstemmen
### [Overzicht van SQL Database Advisor](sql-database-advisor.md)
### [Inzicht in workloads in Azure Portal](sql-database-performance.md)
## [Toegang en machtigingen maken en beheren](sql-database-get-started-security.md)
## [Optimalisatie in het geheugen](sql-database-in-memory.md)
## [Gegevenssynchronisatie](sql-database-get-started-sql-data-sync.md)

# Procedures

## Maken en beheren
### Servers en databases
#### [Individuele databases](sql-database-manage-portal.md)
#### [Azure Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### Pools voor Elastic Database
#### [Azure Portal](sql-database-elastic-pool-create-portal.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
### Shard-databases
#### [Shard-toewijzingsbeheer gebruiken](sql-database-elastic-scale-shard-map-management.md)
#### [Beveiligingsconfiguratie splitsen en samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
#### [Werken met Dapper](sql-database-elastic-scale-working-with-dapper.md)
#### [Entity Framework gebruiken](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)
#### [Beveiliging op rijniveau voor multitenants](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Referenties beheren](sql-database-elastic-scale-manage-credentials.md)
#### [Een service voor splitsen en samenvoegen implementeren](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Een shard toevoegen](sql-database-elastic-scale-add-a-shard.md)
#### [Problemen met shard-toewijzingen oplossen met de RecoveryManager-klasse](sql-database-elastic-database-recovery-manager.md)
###  Authentication
#### [Azure Active Directory-verificatie](sql-database-aad-authentication.md)
#### [Multi-factor authentication](sql-database-ssms-mfa-authentication.md)
### Firewall-regels
#### [Azure Portal](sql-database-configure-firewall-settings.md)
#### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [REST-API](sql-database-configure-firewall-settings-rest.md)
#### [T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Taken
#### [Service-installatie](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Upgrade van clientbibliotheek uitvoeren](sql-database-elastic-scale-upgrade-client-library.md)
### [Aanmeldingen](sql-database-manage-logins.md)

## Ontwikkelen
### [Overzicht](https://msdn.microsoft.com/library/mt763826.aspx)
### [Connectiviteitsbibliotheken](sql-database-libraries.md)
### Scenario's
#### [SaaS-toepassingen voor meerdere tenants](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [JSON-gegevens](sql-database-json-features.md)
#### [Tijdelijke tabellen](sql-database-temporal-tables.md)
#### [Bewaarbeleid](sql-database-temporal-tables-retention-policy.md)
### Een toepassing verbinden
#### [.NET](sql-database-develop-dotnet-simple.md)
#### [Java](sql-database-develop-java-simple.md)
#### [Node.js](sql-database-develop-nodejs-simple.md)
#### [PHP](sql-database-develop-php-simple.md)
#### [Python](sql-database-develop-python-simple.md)
#### [Ruby](sql-database-develop-ruby-simple.md)
#### [Excel](sql-database-connect-excel.md)
#### [Visual Studio](sql-database-connect-query.md)

### [Databases maken](sql-database-get-started-powershell.md)
### Pools voor Elastic Database beheren
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Vereiste waarden voor het verifiëren van een toepassing ophalen](sql-database-client-id-keys.md)
### [Poorten boven 1433 voor ADO.NET 4.5 gebruiken](sql-database-develop-direct-route-ports-adonet-v12.md)
### [Werken met foutberichten](sql-database-develop-error-messages.md)
### [Batchverwerking gebruiken](sql-database-use-batching-to-improve-performance.md)

### Naslaginformatie
#### [.NET (concepten)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [.NET (API-referentie)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### [Entity Framework](https://www.nuget.org/packages/EntityFramework/)
#### [Azure-SDK (download)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure-SDK (documentatie)](../dotnet-sdk.md)
#### [PowerShell Service Management-cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx)
#### REST
##### [REST (resourcebeheer)](https://msdn.microsoft.com/library/azure/mt420159)
##### [REST (servicebeheer)](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## Bedreigingen detecteren
### [Detectie van bedreigingen](sql-database-threat-detection-get-started.md)
### [Firewall](sql-database-firewall-configure.md)

## Gegevens versleutelen
### [Overzicht Always Encrypted](sql-database-always-encrypted.md)
### [Azure Key Vault Always Encrypted](sql-database-always-encrypted-azure-key-vault.md)
### [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/azure/dn948096)
### [Kolomversleuteling](https://msdn.microsoft.com/library/azure/ms179331)

## Gegevens maskeren
### [Dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started-portal.md)
### [Downlevel clients](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)

## Migreren
### Compatibiliteit bepalen
#### [SQL Package-hulpprogramma](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Compatibiliteitsproblemen oplossen
#### [SQL Server Data Tools](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [SQL Azure-migratiewizard](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [SQL Server Management Studio-migratiewizard gebruiken](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
### [Transactionele replicatie gebruiken](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
### [Bestaande uitgeschaalde databases migreren voor uitschaling](sql-database-elastic-convert-to-use-elastic-tools.md)

## Controleren en afstemmen
### [Individuele databases](sql-database-performance-guidance.md)
### [Inzicht in queryprestaties](sql-database-query-performance.md)
### [SQL Database Advisor](sql-database-advisor-portal.md)
### [Databaseprestaties](sql-database-single-database-monitor.md)
### [DMV’s](sql-database-monitoring-with-dmvs.md)
### [Compatibiliteitsniveaus](sql-database-compatibility-level-query-performance-130.md)
### [Gebeurteniscontrole](sql-database-auditing-get-started.md)
### [Prestatiemeteritems voor shard-toewijzingsbeheer](sql-database-elastic-database-perf-counters.md)
### [Tips voor het afstemmen van prestaties](sql-database-troubleshoot-performance.md)
### Servicelagen en prestatieniveaus wijzigen
#### [Azure Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### OLTP in het geheugen
#### [OLTP in het geheugen gebruiken](sql-database-in-memory-oltp-migration.md)
#### [OLTP-opslag in het geheugen controleren](sql-database-in-memory-oltp-monitoring.md)
### Query Store
#### [Prestaties controleren via de Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Query Store-gebruiksscenario's](https://msdn.microsoft.com/library/mt614796.aspx)
#### [De Query Store gebruiken](sql-database-operate-query-store.md)
### Uitgebreide gebeurtenissen
#### [Uitgebreide gebeurtenissen](sql-database-xevent-db-diff-from-svr.md)
#### [Doelcode gebeurtenisbestand](sql-database-xevent-code-event-file.md)
#### [Doelcode ringbuffer](sql-database-xevent-code-ring-buffer.md)

## Gegevens verplaatsen
### [Een SQL-database kopiëren](sql-database-copy.md)
#### [Azure Portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Database naar een BACPAC-bestand exporteren
#### [Azure Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [SQL Package-hulpprogramma](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Database uit een BACPAC-bestand importeren
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [SQL Package-hulpprogramma](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [Laden uit CSV-bestand met BCP](sql-database-load-from-csv-with-bcp.md)
### [Gegevens verplaatsen tussen uitgeschaalde clouddatabases](sql-database-elastic-scale-overview-split-and-merge.md)

## Query’s uitvoeren
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Muiltishard-query’s uitvoeren](sql-database-elastic-scale-multishard-querying.md)
### Query’s tussen databases
#### [Overzicht](sql-database-elastic-query-overview.md)
#### [Databaseoverschrijdende query’s uitvoeren op basis van verschillende schema's](sql-database-elastic-query-vertical-partitioning.md)
#### [Databaseoverschrijdende rapportage](sql-database-elastic-query-horizontal-partitioning.md)
#### [Over clouddatabases gedistribueerde transacties](sql-database-elastic-transactions-overview.md)

## Herstellen
### Verwijderde database herstellen
#### [Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [PowerShell](sql-database-restore-deleted-database-powershell.md)
### Herstel naar een bepaald tijdstip
#### [Azure Portal](sql-database-point-in-time-restore-portal.md)
#### [PowerShell](sql-database-point-in-time-restore-powershell.md)
### Geo-herstel
#### [Azure Portal](sql-database-geo-restore-portal.md)
#### [PowerShell](sql-database-geo-restore-powershell.md)
#### [Eén tabel](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
### [Herstellen van een datacenterstoring](sql-database-disaster-recovery.md)
### [DR-herstelanalyses uitvoeren](sql-database-disaster-recovery-drills.md)

## Repliceren
### [Overzicht actieve geo-replicatie](sql-database-geo-replication-overview.md)
### Configureren
#### [PowerShell](sql-database-geo-replication-powershell.md)
#### [T-SQL](sql-database-geo-replication-transact-sql.md)
### Failover
#### [Azure Portal](sql-database-geo-replication-failover-portal.md)
#### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)

## Problemen oplossen
### [Verbindingsproblemen](sql-database-troubleshoot-common-connection-issues.md)
### [Tijdelijke verbindingsfout](sql-database-troubleshoot-connection.md)
### [Diagnosticeren en voorkomen](sql-database-connectivity-issues.md)
### [Machtigingen](sql-database-troubleshoot-permissions.md)
### [Databases verplaatsen](sql-database-troubleshoot-moving-data.md)

# Naslaginformatie
## [PowerShell](/powershell/azureps-cmdlets-docs/)
## [PowerShell klassiek](/powershell/servicemanagement/)
## [Java](/java/api/)
## [.NET](/dotnet/api/)
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## [Azure SQL Database Cmdlets](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
## [SQL Server-cmdlets](https://msdn.microsoft.com/library/mt740629.aspx)
## [REST](/rest/api/sql/)

## SQL Database-beheerbibliotheek
### [Naslaginformatie SQL Database-beheerbibliotheek](https://msdn.microsoft.com/library/azure/mt349017.aspx)
### [Het pakket met de SQL Database-beheerbibliotheek downloaden](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [SQL Server-stuurprogramma's](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)

# Bronnen
## [Prijzen](https://azure.microsoft.com/pricing/details/sql-database/)
## [Service-updates](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Server-hulpprogramma's](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## [Forum](https://social.msdn.microsoft.com/Forums/home?forum=ssdsgetstarted)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)


<!--HONumber=Nov16_HO2-->


