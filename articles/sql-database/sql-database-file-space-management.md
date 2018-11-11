---
title: Azure SQL Database-bestand ruimtebeheer | Microsoft Docs
description: Deze pagina wordt beschreven hoe u voor het beheren van bestandsruimte met Azure SQL Database, en bevat voorbeelden van code voor het bepalen of u moet voor het verkleinen van een database ook hoe om uit te voeren een database bewerking verkleinen.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 2de57a4ade91293fb1164815f83e87517068544e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277883"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Ruimte in Azure SQL Database beheren
Dit artikel beschrijft de verschillende typen opslagruimte in Azure SQL Database en de stappen die kunnen worden uitgevoerd wanneer de bestandsruimte voor databases toegewezen en elastische pools moet expliciet worden beheerd.

## <a name="overview"></a>Overzicht

In Azure SQL Database zijn er patronen van werkbelasting waarbij de toewijzing van onderliggende gegevensbestanden voor databases die groter is dan de hoeveelheid gebruikte gegevenspagina's kan worden. Dit probleem kan optreden wanneer ruimte toeneemt gebruikt en gegevens worden vervolgens verwijderd. De reden is dat bestandsruimte die is toegewezen niet automatisch wordt opgeëist wanneer gegevens worden verwijderd.

Gebruik van ruimte bewaking en gegevensbestanden verkleinen mogelijk in de volgende scenario's:
- Groei van gegevens in een elastische pool toestaan wanneer het toegewezen bestandsruimte voor de databases de maximale grootte van de groep bereikt.
- Toestaan dat de maximale grootte van een individuele database of elastische pool verlagen.
- Het wijzigen van een individuele database of elastische pool naar een andere servicelaag of de prestatielaag met een lagere maximale grootte toegestaan.

### <a name="monitoring-file-space-usage"></a>Gebruik van schijfruimte bewaken
De meeste storage space metrische gegevens weergegeven in de Azure-portal en de volgende API's meten alleen het formaat van pagina's van gegevens die worden gebruikt:
- Azure Resource Manager op basis van metrische gegevens over API's zoals PowerShell [get-metrische gegevens](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)

De volgende API's meten echter ook de grootte van de toegewezen ruimte voor databases en elastische pools:
- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

### <a name="shrinking-data-files"></a>Bestanden verkleinen

Gegevensbestanden als u wilt vrijmaken van ongebruikte toegewezen ruimte vanwege de mogelijke impact op de prestaties van de database niet automatisch wordt verkleind door de SQL-database-service.  Klanten kunnen echter gegevensbestanden via de self-service-verkleinen op een tijdstip van hun keuze met de volgende stappen wordt beschreven in [Reclaim ongebruikte ruimte toegewezen](#reclaim-unused-allocated-space). 

> [!NOTE]
> In tegenstelling tot gegevensbestanden en verkleint de service SQL Database automatisch logboekbestanden omdat die bewerking is niet van invloed op de prestaties van de database. 

## <a name="understanding-types-of-storage-space-for-a-database"></a>Typen van de opslagruimte voor een database

Inzicht krijgen in de volgende storage space aantallen zijn belangrijk voor het beheren van de bestandsruimte van een database.

|Aantal van de database|Definitie|Opmerkingen|
|---|---|---|
|**Gebruikte gegevensruimte**|De hoeveelheid ruimte die wordt gebruikt voor het opslaan van gegevens uit een database op pagina's van 8 KB.|Over het algemeen gebruikt ruimte toeneemt (afname) op voegt (verwijderen). In sommige gevallen de ruimte die wordt gebruikt niet wordt gewijzigd op ingevoegd of wordt verwijderd, afhankelijk van de hoeveelheid en het patroon van de gegevens die betrokken zijn bij de bewerking en eventuele fragmentatie van gegevens. Bijvoorbeeld, komt verwijderen van één rij uit elke gegevenspagina niet noodzakelijkerwijs verkleinen de ruimte die wordt gebruikt.|
|**Toegewezen gegevensruimte**|De hoeveelheid opgemaakt bestandsruimte beschikbaar is voor het opslaan van gegevens uit een database gemaakt.|De hoeveelheid ruimte die is toegewezen automatisch vergroot, maar nooit afneemt na het verwijderen. Dit gedrag zorgt ervoor dat toekomstige voegt sneller zijn sinds de ruimte hoeft niet opnieuw worden geformatteerd.|
|**Gegevensruimte op de niet-gebruikte toegewezen**|Het verschil tussen de hoeveelheid toegewezen gegevensruimte en gegevensruimte die wordt gebruikt.|Dit aantal geeft de maximale hoeveelheid ruimte die door de databasebestanden te verkleinen gegevens kan worden vrijgemaakt.|
|**Maximale grootte**|De maximale hoeveelheid ruimte die kan worden gebruikt voor het opslaan van gegevens uit een database.|De hoeveelheid toegewezen gegevensruimte kan niet groter wordt dan de maximale grootte van gegevens.|
||||

Het volgende diagram illustreert de relatie tussen de verschillende typen opslagruimte voor een database.

![ruimte opslagtypen en relaties](./media/sql-database-file-space-management/storage-types.png) 

## <a name="query-a-database-for-storage-space-information"></a>Query uitvoeren op een database voor storage space informatie

De volgende query's kunnen worden gebruikt om te bepalen storage space hoeveelheden voor een database.  

### <a name="database-data-space-used"></a>Databaseruimte voor gegevens die worden gebruikt
De volgende query uit om de hoeveelheid ruimte in database gegevens gebruikt te wijzigen.  Eenheden van het queryresultaat zijn in MB.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Gegevens uit de databaseruimte toegewezen en ongebruikte toegewezen ruimte
Gebruik de volgende query retourneert de hoeveelheid ruimte in database gegevens toegewezen en de hoeveelheid niet-gebruikte ruimte die is toegewezen.  Eenheden van het queryresultaat zijn in MB.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-data-max-size"></a>Maximale grootte van database
De volgende query uit om de maximale grootte van de database gegevens te wijzigen.  Eenheden van het queryresultaat zijn in bytes.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Typen van de opslagruimte voor een elastische pool

Inzicht krijgen in de volgende storage space aantallen zijn belangrijk voor het beheren van de bestandsruimte van een elastische pool.

|Elastische pool hoeveelheid|Definitie|Opmerkingen|
|---|---|---|
|**Gebruikte gegevensruimte**|De som van de ruimte die wordt gebruikt door alle databases in de elastische pool.||
|**Toegewezen gegevensruimte**|De som van de ruimte die door alle databases in de elastische pool toegewezen.||
|**Gegevensruimte op de niet-gebruikte toegewezen**|Het verschil tussen de hoeveelheid toegewezen gegevensruimte en gegevensruimte die wordt gebruikt door alle databases in de elastische pool.|Dit aantal geeft de maximale hoeveelheid toegewezen ruimte voor de elastische pool die kan worden vrijgemaakt door gegevens van databasebestanden verkleinen.|
|**Maximale grootte**|De maximale hoeveelheid ruimte die door de elastische groep kan worden gebruikt voor alle bijbehorende databases.|De toegewezen ruimte voor de elastische pool mag niet groter zijn dan de maximale grootte van de elastische pool.  Als deze fout optreedt, kan toegewezen ruimte die wordt gebruikt door de databasebestanden te verkleinen gegevens worden vrijgemaakt.|
||||

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Een elastische pool voor storage space informatie opvragen

De volgende query's kunnen worden gebruikt om te bepalen storage space hoeveelheden voor een elastische pool.  

### <a name="elastic-pool-data-space-used"></a>Gegevensruimte op de elastische pool gebruikt
De volgende query uit om de hoeveelheid gegevensruimte op de elastische pool gebruikt te wijzigen.  Eenheden van het queryresultaat zijn in MB.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Gegevensruimte op de elastische pool toegewezen en ongebruikte toegewezen ruimte

De volgende PowerShell-script om te retourneren een tabel met de ruimte die is toegewezen en ongebruikte toegewezen ruimte voor elke database in een elastische pool wijzigen. De tabel orders databases van deze databases met de grootste hoeveelheid niet-gebruikte toegewezen ruimte aan de minimale hoeveelheid niet-gebruikte ruimte toegewezen.  Eenheden van het queryresultaat zijn in MB.  

De resultaten van de query voor het bepalen van de toegewezen ruimte voor elke database in de groep, kan samen worden toegevoegd om te bepalen van de totale ruimte voor de elastische pool toegewezen. De toegewezen ruimte voor de elastische pool mag niet groter zijn dan de maximale grootte van de elastische pool.  

Het PowerShell-script is vereist voor SQL Server PowerShell-module: Zie [downloaden PowerShell-module](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module?view=sql-server-2017) te installeren.

```powershell
# Resource group name
$resourceGroupName = "rg1" 
# Server name
$serverName = "ls2" 
# Elastic pool name
$poolName = "ep1"
# User name for server
$userName = "name"
# Password for server
$password = "password"

# Get list of databases in elastic pool
$databasesInPool = Get-AzureRmSqlElasticPoolDatabase `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# For each database in the elastic pool,
# get its space allocated in MB and space allocated unused in MB.
  
foreach ($database in $databasesInPool)
{
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn `
        -Database $database.DatabaseName `
        -Username $userName `
        -Password $password `
        -Query $sqlCommand)
}
# Display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort `
    -Property DatabaseDataSpaceAllocatedUnusedInMB `
    -Descending | Format-Table
```

De volgende schermafbeelding is een voorbeeld van de uitvoer van het script:

![elastische pool toegewezen ruimte en voorbeeld van de niet-gebruikte toegewezen ruimte](./media/sql-database-file-space-management/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Maximale grootte van elastische pool

De volgende T-SQL-query uit om maximale grootte van de elastische pool te wijzigen.  Eenheden van het queryresultaat zijn in MB.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Niet-gebruikte toegewezen ruimte vrijmaken

### <a name="dbcc-shrink"></a>DBCC verkleinen

Zodra de databases zijn geïdentificeerd voor het vrijmaken van ongebruikte toegewezen ruimte, wijzigt u de naam van de database in de volgende opdracht voor het verkleinen van de gegevensbestanden voor elke database.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Met deze opdracht kan invloed hebben op prestaties van de database terwijl deze wordt uitgevoerd, en indien mogelijk moet worden uitgevoerd tijdens perioden met een laag gebruik.  

Zie voor meer informatie over deze opdracht [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 

### <a name="auto-shrink"></a>Automatisch verkleind

U kunt ook kan automatisch verkleind worden ingeschakeld voor een database.  Automatisch verkleind vermindert de complexiteit van de bestand-beheer en is minder impact hebben op prestaties van de database dan SHRINKDATABASE of SHRINKFILE.  Automatische verkleining kan met name handig voor het beheren van elastische pools met veel databases worden gemaakt.  Automatische verkleining kan echter wel minder effectief in het vrijmaken van ruimte dan SHRINKDATABASE en SHRINKFILE.
Wijzig de naam van de database in de volgende opdracht uit zodat automatisch verkleind.


```sql
-- Enable auto-shrink for the database.
ALTER DATABASE [db1] SET AUTO_SHRINK ON
```

Zie voor meer informatie over deze opdracht [DATABASE ingesteld](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=sql-server-2017) opties. 

### <a name="rebuild-indexes"></a>Indexen opnieuw samenstellen

Nadat de databasebestanden van de gegevens worden gecomprimeerd, worden de indexen gefragmenteerd raken en verliezen hun effectiviteit van de optimalisatie van prestaties. Als verminderde prestaties optreedt, overweeg dan om database-indexen herbouwen. Zie voor meer informatie over fragmentatie en opnieuw opbouwen van indexen [Reorganize en indexen opnieuw samenstellen](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over de maximale grootte van database:
  - [Azure SQL Database vCore gebaseerde model limieten voor één database aanschaffen](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [Resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Azure SQL Database vCore gebaseerde model limieten voor elastische pools aanschaffen](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [Limieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel resources](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Voor meer informatie over de `SHRINKDATABASE` opdracht, Zie [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Zie voor meer informatie over fragmentatie en opnieuw opbouwen van indexen [Reorganize en indexen opnieuw samenstellen](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
