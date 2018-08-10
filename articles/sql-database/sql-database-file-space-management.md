---
title: Azure SQL Database-bestand ruimtebeheer | Microsoft Docs
description: Deze pagina wordt beschreven hoe u voor het beheren van bestandsruimte met Azure SQL Database, en bevat voorbeelden van code voor het bepalen of u moet voor het verkleinen van een database ook hoe om uit te voeren een database bewerking verkleinen.
services: sql-database
author: oslake
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: moslake
ms.openlocfilehash: 5dce07996191af3df3a4bdf16b211c29d59a994f
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003855"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Ruimte in Azure SQL Database beheren
Dit artikel beschrijft de verschillende typen opslagruimte in Azure SQL Database en de stappen die kunnen worden uitgevoerd wanneer de bestandsruimte voor databases toegewezen en elastische pools moet expliciet worden beheerd.

## <a name="overview"></a>Overzicht

De meeste storage space metrische gegevens weergegeven in de Azure-portal en de volgende API's meten in Azure SQL Database, het aantal pagina's van gegevens die worden gebruikt voor databases en elastische pools:
- Azure Resource Manager op basis van metrische gegevens over API's zoals PowerShell [get-metrische gegevens](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Er zijn patronen van werkbelasting waarbij de toewijzing van onderliggende gegevensbestanden voor databases die groter is dan de hoeveelheid gebruikte gegevenspagina's kan worden.  Dit kan gebeuren wanneer de ruimte gebruikt toeneemt en gegevens worden vervolgens verwijderd.  Dit komt doordat de toegewezen ruimte niet automatisch wordt opgeëist wanneer gegevens worden verwijderd.  In dergelijke scenario's, kan de toegewezen ruimte voor een database of een groep van toepassingen groter zijn dan de ondersteunde limieten te voorkomen dat de groei van gegevens of te voorkomen dat prestaties laagwijzigingen en vereisen verkleinen gegevensbestanden te beperken.

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
|**Maximale grootte**|De maximale hoeveelheid ruimte die door de elastische groep kan worden gebruikt voor alle bijbehorende databases.|De toegewezen ruimte voor de elastische pool mag niet groter zijn dan de maximale grootte van de elastische pool.  Als dit gebeurt, kan toegewezen ruimte die wordt gebruikt door de databasebestanden te verkleinen gegevens worden vrijgemaakt.|
||||

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Een elastische pool voor storage space informatie opvragen

De volgende query's kunnen worden gebruikt om te bepalen storage space hoeveelheden voor een elastische pool.  

### <a name="elastic-pool-data-space-used"></a>Gegevensruimte op de elastische pool gebruikt
De volgende query uit om de hoeveelheid gegevensruimte op de elastische pool gebruikt te wijzigen.  Eenheden van het queryresultaat zijn in MB.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Gegevensruimte op de elastische pool toegewezen en ongebruikte toegewezen ruimte

De volgende PowerShell-script om te retourneren een tabel met de ruimte die is toegewezen en ongebruikte toegewezen ruimte voor elke database in een elastische pool wijzigen. De tabel orders databases die met de grootste hoeveelheid niet-gebruikte toegewezen ruimte aan de minimale hoeveelheid niet-gebruikte ruimte toegewezen.  Eenheden van het queryresultaat zijn in MB.  

De resultaten van de query voor het bepalen van de toegewezen ruimte voor elke database in de groep, kan samen worden toegevoegd om te bepalen van de totale ruimte voor de elastische pool toegewezen. De toegewezen ruimte voor de elastische pool mag niet groter zijn dan de maximale grootte van de elastische pool.  

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
# Requires SQL Server PowerShell module – see here to install.  
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

Zodra de databases zijn geïdentificeerd voor het vrijmaken van ongebruikte toegewezen ruimte, wijzigt u de volgende opdracht voor het verkleinen van de gegevensbestanden voor elke database.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Zie voor meer informatie over deze opdracht [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 

> [!IMPORTANT] 
> Houd rekening met herbouwen database indexen na de databasebestanden van de gegevens worden gecomprimeerd, indexen gefragmenteerd raken en hun effectiviteit van de optimalisatie van prestaties verliezen. Als dit het geval is, klikt u vervolgens de indexen moeten opnieuw worden opgebouwd. Zie voor meer informatie over fragmentatie en opnieuw opbouwen van indexen [Reorganize en indexen opnieuw samenstellen](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over de maximale grootte van database:
  - [Azure SQL Database vCore gebaseerde model limieten voor één database aanschaffen](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [Resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Azure SQL Database vCore gebaseerde model limieten voor elastische pools aanschaffen](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [Limieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel resources](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Voor meer informatie over de `SHRINKDATABASE` opdracht, Zie [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Zie voor meer informatie over fragmentatie en opnieuw opbouwen van indexen [Reorganize en indexen opnieuw samenstellen](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
