# Overzicht
## [Wat is SQL Database?](sql-database-technical-overview.md)
### [Servicelagen](sql-database-service-tiers.md)
### [DTU's en eDTU's](sql-database-what-is-a-dtu.md)
### [DTU-benchmarkoverzicht](sql-database-benchmark-overview.md)
### [Bronlimieten](sql-database-resource-limits.md)
### [Functies](sql-database-features.md)
### [Veelgestelde vragen over SQL Database](sql-database-faq.md)
## Vergelijkingen
### [SQL Database versus SQL op een VM](sql-database-paas-vs-sql-server-iaas.md)
### [T-SQL-verschillen](sql-database-transact-sql-information.md)
### [SQL versus NoSQL](../documentdb/documentdb-nosql-vs-sql.md)
## [Hulpmiddelen voor SQL Database](sql-database-manage-overview.md)
## [Zelfstudies voor SQL Database](sql-database-explore-tutorials.md)
## Beveiliging
### [Beveiligingsoverzicht](sql-database-security-overview.md)
### [Azure Security Center voor Azure SQL Database](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)
# Aan de slag
## Databases en servers
### Leren
#### [Servers](sql-database-server-overview.md)
#### [Individuele databases](sql-database-overview.md)
#### [Meerdere databases](sql-database-elastic-scale-introduction.md)
##### Tenants toewijzen
###### [Elastische clientbibliotheek](sql-database-elastic-database-client-library.md)
###### [Shard-toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md)
###### [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)
###### [Referenties beheren](sql-database-elastic-scale-manage-credentials.md)
###### [Multishard-query's uitvoeren](sql-database-elastic-scale-multishard-querying.md)
##### Elastische pools (resourcepools)
###### [Wat is een elastische pool?](sql-database-elastic-pool.md)
###### [Wanneer gebruik ik een elastische pool?](sql-database-elastic-pool-guidance.md)
###### [Prijzen voor Elastic-pools](sql-database-elastic-pool-price.md)
##### Shard-databases
###### [Woordenlijst voor elastische pools](sql-database-elastic-scale-glossary.md)
###### [Gegevens verplaatsen tussen shards](sql-database-elastic-scale-overview-split-and-merge.md)
###### [Veelgestelde vragen over Elastic-hulpprogramma's](sql-database-elastic-scale-faq.md)
##### Elastische query's (query’s tussen databases)
###### [Wat is een elastische query?](sql-database-elastic-query-overview.md)
##### Elastische transacties (gedistribueerde transacties)
###### [Transacties tussen clouddatabases](sql-database-elastic-transactions-overview.md)
##### Elastische taken (taken tussen databases)
###### [Wat is een elastische taak?](sql-database-elastic-jobs-overview.md)
#### [Azure RemoteApp gebruiken om verbinding te maken met SQL Database](sql-database-ssms-remoteapp.md)
#### [Azure SQL-databases beheren via de Azure Automation-service](sql-database-manage-automation.md)
### Wel doen
#### [Een individuele database maken met Azure Portal](sql-database-get-started.md)
#### [Een individuele database maken met PowerShell](sql-database-get-started-powershell.md)
#### [Een individuele database maken met C#](sql-database-get-started-csharp.md)
#### [Een shard-toepassing maken](sql-database-elastic-scale-get-started.md)
#### [Gegevens verplaatsen tussen shards](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Aan de slag met elastische taken](sql-database-elastic-jobs-getting-started.md)
#### [Aan de slag met elastische query’s](sql-database-elastic-query-getting-started-vertical.md)
#### [Rapporten maken met elastische query’s](sql-database-elastic-query-getting-started.md)
#### [Query's uitvoeren voor databases met verschillende schema's](sql-database-elastic-query-vertical-partitioning.md)
#### [Rapportage over uitgeschaalde databases](sql-database-elastic-query-horizontal-partitioning.md)
## Gegevens migreren en verplaatsen
### Leren
#### [Een database migreren](sql-database-cloud-migrate.md)
#### [Een database exporteren](sql-database-export.md)
#### [Gegevenssynchronisatie](sql-database-get-started-sql-data-sync.md)
#### [Een SQL-database kopiëren](sql-database-copy.md)
## Firewallregels, -verificatie en -autorisatie
### Leren
#### [Toegangsbeheer](sql-database-control-access.md)
#### [Firewallregels](sql-database-firewall-configure.md)
#### [Aanmeldingen beheren](sql-database-manage-logins.md)
#### [Azure Active Directory-verificatie](sql-database-aad-authentication.md)
#### [Multi-factor authentication](sql-database-ssms-mfa-authentication.md)
### Wel doen
#### [SQL-verificatie en -autorisatie](sql-database-control-access-sql-authentication-get-started.md)
#### [Azure AD-verificatie en -autorisatie](sql-database-control-access-aad-authentication-get-started.md)
## Gegevens beveiligen en beschermen
### Leren
#### Controleren
##### [Controleren](sql-database-auditing.md)
##### [Ondersteuning voor downlevel-clients en wijzigingen in IP-eindpunten voor controle](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Detectie van bedreigingen](sql-database-threat-detection.md)
#### Gegevens versleutelen
##### [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/azure/dn948096)
##### [Kolomversleuteling](https://msdn.microsoft.com/library/azure/ms179331)
#### Gegevens maskeren
##### [Dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md)
### Wel doen
#### [Dynamische gegevensmaskering met Azure Portal](sql-database-dynamic-data-masking-get-started-portal.md)
#### [Controles configureren met behulp van Azure Portal](sql-database-auditing-portal.md)
#### [Controles configureren met behulp van PowerShell](sql-database-auditing-powershell.md)
#### [Controles configureren met behulp van de REST-API](sql-database-auditing-rest.md)
#### [Altijd versleuteld met behulp van een Windows-certificaat](sql-database-always-encrypted.md)
#### [Altijd versleuteld met behulp van Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)
## Bedrijfscontinuïteit
### Leren
#### [Overzicht](sql-database-business-continuity.md)
#### [Databaseback-ups](sql-database-automated-backups.md)
#### [Langetermijnretentie](sql-database-long-term-retention.md)
#### [Databaseherstel met behulp van back-ups](sql-database-recovery-using-backups.md)
#### [Een individuele tabel herstellen](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Herstellen van een datacenterstoring](sql-database-disaster-recovery.md)
#### [Verificatievereisten voor herstel na noodgevallen](sql-database-geo-replication-security-config.md)
#### [Ontwerpscenario's voor bedrijfscontinuïteit](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategieën voor herstel na noodgevallen met pools voor Elastic Database](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Rolling upgrades](sql-database-manage-application-rolling-upgrade.md)
#### [DR-herstelanalyses uitvoeren](sql-database-disaster-recovery-drills.md)
#### [Overzicht actieve geo-replicatie](sql-database-geo-replication-overview.md)
### Wel doen
#### [Azure Portal: back-up en herstel](sql-database-get-started-backup-recovery-portal.md)
#### [PowerShell: back-up en herstel](sql-database-get-started-backup-recovery-powershell.md)
## Ontwikkeling van apps
### Leren
#### [Overzicht van ontwikkeling van databasetoepassingen](sql-database-develop-overview.md)
#### [Connectiviteitsbibliotheken](sql-database-libraries.md)
#### [SaaS-toepassingen voor meerdere tenants](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [SaaS-toepassingen voor meerdere tenants schalen met beveiliging op rijniveau](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Poorten boven 1433 voor ADO.NET 4.5 gebruiken](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Vereiste waarden voor het verifiëren van een toepassing ophalen](sql-database-client-id-keys.md)
### Wel doen
#### Een toepassing verbinden
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [C en C++](sql-database-develop-cplusplus-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Verbinden met Visual Studio](sql-database-connect-query.md)
#### [Een clienttoepassing bouwen](https://www.microsoft.com/sql-server/developer-get-started)
#### [Werken met foutberichten](sql-database-develop-error-messages.md)
#### [Entity Framework gebruiken](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Clientbibliotheek gebruiken met Dapper](sql-database-elastic-scale-working-with-dapper.md)
### Klantimplementaties
#### [Daxko/CSI-software](sql-database-implementation-daxko.md)
#### [GEP](sql-database-implementation-gep.md)
#### [SnelStart](sql-database-implementation-snelstart.md)
#### [Umbraco](sql-database-implementation-umbraco.md)
## Databaseontwikkeling
### Leren
#### Tijdelijke tabellen
##### [Tijdelijke tabellen](sql-database-temporal-tables.md)
##### [Bewaarbeleid](sql-database-temporal-tables-retention-policy.md)
#### [JSON-gegevens](sql-database-json-features.md)
#### [Optimalisatie in het geheugen](sql-database-in-memory.md)
### Wel doen
#### [SQL Server-ontwikkeling](https://msdn.microsoft.com/library/ms179422.aspx)
#### [OLTP in het geheugen gebruiken](sql-database-in-memory-oltp-migration.md)
## Controleren en afstemmen
### Leren
#### [Individuele databases](sql-database-single-database-monitor.md)
#### [Overzicht van SQL Database Advisor](sql-database-advisor.md)
#### [Hulp bij individuele database](sql-database-performance-guidance.md)
#### [Prestatie-inzichten: Azure Portal](sql-database-performance.md)
#### [Batchverwerking gebruiken](sql-database-use-batching-to-improve-performance.md)
#### [Uitgebreide gebeurtenissen](sql-database-xevent-db-diff-from-svr.md)
## SQL Database V11
### [Oude Web- en Business-edities](sql-database-web-business-sunset-faq.md)
### [Service Tier Advisor](sql-database-service-tier-advisor.md)
### [Beoordelingshulpprogramma voor elastische pool](sql-database-elastic-pool-database-assessment-powershell.md)
### [Upgraden naar V12](sql-database-v12-plan-prepare-upgrade.md)
#### [Upgraden via Azure Portal](sql-database-upgrade-server-portal.md)
#### [Upgraden met PowerShell](sql-database-upgrade-server-powershell.md)
# Procedures
## Maken en beheren
### [Beheren met Azure Portal](sql-database-manage-portal.md)
### [Beheren met PowerShell](sql-database-manage-powershell.md)
### [Beheren met SSMS](sql-database-manage-azure-ssms.md)
### Servers
#### [Beheren met Azure Portal](sql-database-manage-servers-portal.md)
#### [Beheren met PowerShell](sql-database-manage-servers-powershell.md)
### Individuele databases
#### [Beheren met Azure Portal](sql-database-manage-single-databases-portal.md)
#### [Beheren met PowerShell](sql-database-manage-single-databases-powershell.md)
#### [Beheren met Transact-SQL](sql-database-manage-single-databases-tsql.md)
### Firewall-regels
#### [Maken met Azure Portal](sql-database-configure-firewall-settings.md)
#### [Maken met PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [Maken met REST-API](sql-database-configure-firewall-settings-rest.md)
#### [Maken met T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Meerdere databases
#### [Clientbibliotheek upgraden in clienttoepassingen](sql-database-elastic-scale-upgrade-client-library.md)
#### Shard-databases
##### [Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Een shard toevoegen](sql-database-elastic-scale-add-a-shard.md)
##### [Shard-toewijzingsproblemen oplossen](sql-database-elastic-database-recovery-manager.md)
##### [Migreren naar Shard-databases](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Prestatiemeteritems maken voor shard-toewijzingsbeheer](sql-database-elastic-database-perf-counters.md)
#### Elastische taken
##### [Hoe voer ik de installatie uit?](sql-database-elastic-jobs-service-installation.md)
##### [Beheren met PowerShell](sql-database-elastic-jobs-powershell.md) 
##### [Beheren met Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
##### [Hoe verwijder ik de installatie?](sql-database-elastic-jobs-uninstall.md)
#### Pools voor Elastic Database
##### [Beheren met Azure Portal](sql-database-elastic-pool-manage-portal.md)
##### [Beheren met PowerShell](sql-database-elastic-pool-manage-powershell.md)
##### [Beheren met C#](sql-database-elastic-pool-manage-csharp.md)
##### [Beheren met T-SQL](sql-database-elastic-pool-manage-tsql.md)
## Verifiëren en autoriseren
### [Azure Active Directory-verificatie instellen](sql-database-aad-authentication-configure.md)
### [Multi-Factor Authentication instellen](sql-database-ssms-mfa-authentication-configure.md)
## Detectie van bedreigingen instellen
### [Detectie van bedreigingen instellen met behulp van Azure Portal](sql-database-threat-detection-portal.md)
## Gegevens versleutelen
### [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/azure/dn948096)
### [Kolomversleuteling](https://msdn.microsoft.com/library/azure/ms179331)
## Controleren en afstemmen
### [Inzicht in queryprestaties](sql-database-query-performance.md)
### [SQL Database Advisor](sql-database-advisor-portal.md)
### [DMV's](sql-database-monitoring-with-dmvs.md)
### [Compatibiliteitsniveaus](sql-database-compatibility-level-query-performance-130.md)
### [Tips voor het afstemmen van prestaties](sql-database-troubleshoot-performance.md)
### [Waarschuwingen maken](sql-database-insights-alerts-portal.md)
### [OLTP-opslag in het geheugen controleren](sql-database-in-memory-oltp-monitoring.md)
### Query Store
#### [Prestaties controleren met de Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Query Store-gebruiksscenario's](https://msdn.microsoft.com/library/mt614796.aspx)
#### [De Query Store gebruiken](sql-database-operate-query-store.md)
### Uitgebreide gebeurtenissen
#### [Doelcode gebeurtenisbestand](sql-database-xevent-code-event-file.md)
#### [Doelcode ringbuffer](sql-database-xevent-code-ring-buffer.md)
## Gegevens verplaatsen
### Een SQL-database kopiëren
#### [Kopiëren met Azure Portal](sql-database-copy-portal.md)
#### [Kopiëren met PowerShell](sql-database-copy-powershell.md)
#### [Kopiëren met T-SQL](sql-database-copy-transact-sql.md)
### Database naar een BACPAC-bestand exporteren
#### [Exporteren met behulp van Azure Portal](sql-database-export-portal.md)
#### [Exporteren met behulp van het hulpprogramma SQL Package](sql-database-export-sqlpackage.md)
#### [Exporteren met behulp van PowerShell](sql-database-export-powershell.md)
#### [Exporteren met behulp van SSMS](sql-database-export-ssms.md)
## Gegevens importeren
### [Importeren met behulp van Azure Portal](sql-database-import-portal.md)
### [Importeren met behulp van PowerShell](sql-database-import-powershell.md)
### [Importeren met behulp van SQLPackage](sql-database-import-sqlpackage.md)
### [Laden uit CSV-bestand met BCP](sql-database-load-from-csv-with-bcp.md)
## Query's uitvoeren
### [Query’s uitvoeren met behulp van SSMS](sql-database-connect-query-ssms.md)
## Back-ups en herstellen
### Langetermijnretentie van back-ups
#### [Langetermijnretentie van back-ups beheren met behulp van Azure Portal](sql-database-manage-long-term-backup-retention-portal.md)
#### [Langetermijnretentie van back-ups beheren met behulp van PowerShell](sql-database-manage-long-term-backup-retention-powershell.md)
### Verwijderde database herstellen
#### [Verwijderde items herstellen met behulp van Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [Verwijderde items herstellen met behulp van PowerShell](sql-database-restore-deleted-database-powershell.md)
### Een database herstellen naar een bepaald tijdstip
#### [Herstel naar een bepaald tijdstip met behulp van Azure Portal](sql-database-point-in-time-restore-portal.md)
#### [Database herstellen naar een bepaald tijdstip met behulp van PowerShell](sql-database-point-in-time-restore-powershell.md)
### Terugzetten vanuit geografisch redundante back-up
### [Geo-herstellen met behulp van Azure Portal](sql-database-geo-restore-portal.md)
### [Geo-herstellen met behulp van PowerShell](sql-database-geo-restore-powershell.md)
## Actieve Geo-replicatie
### [Configureren met behulp van Azure Portal](sql-database-geo-replication-portal.md)
### [Configureren met behulp van PowerShell](sql-database-geo-replication-powershell.md)
### [Configureren met behulp van T-SQL](sql-database-geo-replication-transact-sql.md)
### [Failover met behulp van Azure Portal](sql-database-geo-replication-failover-portal.md)
### [Failover met behulp van PowerShell](sql-database-geo-replication-failover-powershell.md)
### [Failover met behulp van T-SQL](sql-database-geo-replication-failover-transact-sql.md)
## Problemen oplossen
### [Verbindingsproblemen](sql-database-troubleshoot-common-connection-issues.md)
### [Tijdelijke verbindingsfout](sql-database-troubleshoot-connection.md)
### [Diagnosticeren en voorkomen](sql-database-connectivity-issues.md)
### [Machtigingen](sql-database-troubleshoot-permissions.md)
### [Databases verplaatsen](sql-database-troubleshoot-moving-data.md)
# Naslaginformatie
## [PowerShell](/powershell/resourcemanager/azurerm.sql/v2.3.0/azurerm.sql)
## [PowerShell (elastische database)](/powershell/elasticdatabasejobs/v0.8.33/elasticdatabasejobs)
## [Azure CLI 2.0](/cli/azure/appservice)
## [.NET](/dotnet/api/microsoft.azure.management.sql.models)
## [Java](/java/api/com.microsoft.azure.management.sql)
## [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
## [Python](https://msdn.microsoft.com/library/mt652092.aspx)
## [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
## [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
## [T-SQL](https://msdn.microsoft.com/library/bb510741.aspx)
## [REST](https://msdn.microsoft.com/library/azure/mt163571.aspx)

# Verwant
## SQL Database-beheerbibliotheek
### [Het pakket met de SQL Database-beheerbibliotheek downloaden](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [SQL Server-stuurprogramma's](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)

# Resources
## [Prijzen](https://azure.microsoft.com/pricing/details/sql-database/)
## [MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-azure)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)
## [Service-updates](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Server-hulpprogramma's](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
