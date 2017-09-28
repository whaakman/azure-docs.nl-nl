# Overzicht

## [Wat is SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)
## [Datawarehouse-workload](sql-data-warehouse-overview-workload.md)
## [Gedistribueerde gegevens](sql-data-warehouse-distributed-data.md)
## [Veelgestelde vragen](sql-data-warehouse-overview-faq.md)

# Aan de slag

## [Zelfstudie voor beginners](sql-data-warehouse-get-started-tutorial.md)
## [Elastische query-zelfstudie](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)
## [Aanbevolen procedures](sql-data-warehouse-best-practices.md)
## [Beheren](sql-data-warehouse-overview-manage.md)



# Procedures

## Back-ups en herstellen

### [Overzicht van back-up](sql-data-warehouse-backups.md)
### [Overzicht van herstel](sql-data-warehouse-restore-database-overview.md)
#### [Azure Portal](sql-data-warehouse-restore-database-portal.md)
#### [Azure PowerShell](sql-data-warehouse-restore-database-powershell.md)
#### [REST](sql-data-warehouse-restore-database-rest-api.md)

## Verbinding maken

### [Overzicht](sql-data-warehouse-connect-overview.md)
### [SSMS](sql-data-warehouse-query-ssms.md)
### [Visual Studio](sql-data-warehouse-query-visual-studio.md)
### [Visual Studio installeren](sql-data-warehouse-install-visual-studio.md)
### [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
### [Verbindingsreeksen](sql-data-warehouse-connection-strings.md)

## Maken
### [Azure Portal](sql-data-warehouse-get-started-provision.md)
### [Azure PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
### [T-SQL](sql-data-warehouse-get-started-create-database-tsql.md)

## Ontwikkelen

### [Overzicht](sql-data-warehouse-overview-develop.md)

### Tabellen

#### [Overzicht](sql-data-warehouse-tables-overview.md)
#### [CTAS](sql-data-warehouse-develop-ctas.md)
#### [Gegevenstypen](sql-data-warehouse-tables-data-types.md)
#### [Gedistribueerde tabellen](sql-data-warehouse-tables-distribute.md)
#### [Indexen](sql-data-warehouse-tables-index.md)
#### [Identity](sql-data-warehouse-tables-identity.md)
#### [Partities](sql-data-warehouse-tables-partition.md)
#### [Gerepliceerde tabellen](design-guidance-for-replicated-tables.md)
#### [Statistieken](sql-data-warehouse-tables-statistics.md)
#### [Tijdelijk](sql-data-warehouse-tables-temporary.md)

### Query's

#### [Dynamic SQL](sql-data-warehouse-develop-dynamic-sql.md)
#### [Groeperen op opties](sql-data-warehouse-develop-group-by-options.md)
#### [Labels](sql-data-warehouse-develop-label.md)

### Taalelementen T-SQL

#### [Lussen](sql-data-warehouse-develop-loops.md)
#### [Opgeslagen procedures](sql-data-warehouse-develop-stored-procedures.md)
#### [Transacties](sql-data-warehouse-develop-transactions.md)
#### [Aanbevolen procedures voor transacties](sql-data-warehouse-develop-best-practices-transactions.md)
#### [Door de gebruiker gedefinieerde schema's](sql-data-warehouse-develop-user-defined-schemas.md)
#### [Variabele toewijzing](sql-data-warehouse-develop-variable-assignment.md)
#### [Weergaven](sql-data-warehouse-develop-views.md)

## Integreren

### [Overzicht](sql-data-warehouse-overview-integrate.md)
### [Data Factory](sql-data-warehouse-integrate-azure-data-factory.md)
### [Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md)
### [Zelfstudie over Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
### [Power BI](sql-data-warehouse-integrate-power-bi.md)
### [Power BI-visualisatie](sql-data-warehouse-get-started-visualize-with-power-bi.md)
### [Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md)

### [Elastische query met SQL Database en SQL Data Warehouse](how-to-use-elastic-query-with-sql-data-warehouse.md)

## Belasting

### Concepten
#### [Overzicht](sql-data-warehouse-overview-load.md)
#### [Hulp bij PolyBase](sql-data-warehouse-load-polybase-guide.md)

### Zelfstudies
#### [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)

### Handleidingen
#### [Voorbeeldgegevens](sql-data-warehouse-load-sample-databases.md)
#### [Azure Data Lake Store](sql-data-warehouse-load-from-azure-data-lake-store.md)
#### [BCP](sql-data-warehouse-load-with-bcp.md)
#### [Data Factory](sql-data-warehouse-load-with-data-factory.md)
#### [PolyBase uit Blob Storage](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
#### [PolyBase uit SQL Server](sql-data-warehouse-load-from-sql-server-with-polybase.md)
#### [RedGate](sql-data-warehouse-load-with-redgate.md)
#### [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)

## Migreren

### [Overzicht](sql-data-warehouse-overview-migrate.md)
### [Migratiehulpprogramma](sql-data-warehouse-migrate-migration-utility.md)
### [Schema migreren](sql-data-warehouse-migrate-schema.md)
### [Code migreren](sql-data-warehouse-migrate-code.md)
### [Gegevens migreren](sql-data-warehouse-migrate-data.md)
### [Migreren naar Premium-opslag](sql-data-warehouse-migrate-to-premium-storage.md)

## Compute beheren

### [Overzicht](sql-data-warehouse-manage-compute-overview.md)
### [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
### [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
### [REST-API](sql-data-warehouse-manage-compute-rest-api.md)
### [T-SQL](sql-data-warehouse-manage-compute-tsql.md)

## Prestaties

### [Overzicht](sql-data-warehouse-overview-manage-user-queries.md)
### [Columnstore-compressie](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)
### [Controle](sql-data-warehouse-manage-monitor.md)
### [Workload](sql-data-warehouse-develop-concurrency.md)

## Beveiliging

### [Overzicht](sql-data-warehouse-overview-manage-security.md)
### [Controle](sql-data-warehouse-auditing-overview.md)
### [Controle voor downlevel-clients](sql-data-warehouse-auditing-downlevel-clients.md)
### [Verificatie](sql-data-warehouse-authentication.md)
### [Versleuteling](sql-data-warehouse-encryption-tde.md)
### [Versleuteling met T-SQL](sql-data-warehouse-encryption-tde-tsql.md)
### [Detectie van bedreigingen](sql-data-warehouse-security-threat-detection.md)

## Problemen oplossen
### [Problemen oplossen](sql-data-warehouse-troubleshoot.md)

# Naslaginformatie

## [Capaciteitslimieten](sql-data-warehouse-service-capacity-limits.md)
## T-SQL
### [Volledige naslaginformatie](https://docs.microsoft.com/sql/t-sql/language-reference/)
### [SQL DW-taalelementen](sql-data-warehouse-reference-tsql-language-elements.md)
### [SQL DW-instructies](sql-data-warehouse-reference-tsql-statements.md)
## [Systeemweergaven](sql-data-warehouse-reference-tsql-system-views.md)
## [PowerShell-cmdlets](sql-data-warehouse-reference-powershell-cmdlets.md)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=databases)
## [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse)
## [Prijzen](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Functieverzoeken](https://feedback.azure.com/forums/307516-sql-data-warehouse/)
## [Service-updates](https://azure.microsoft.com/updates/?product=sql-data-warehouse)
## [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw/)
## [Ondersteuning](sql-data-warehouse-get-started-create-support-ticket.md)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)

## Partners
### [Business Intelligence](sql-data-warehouse-partner-business-intelligence.md)
### [Gegevensintegratie](sql-data-warehouse-partner-data-integration.md)
### [Gegevensbeheer](sql-data-warehouse-partner-data-management.md)
