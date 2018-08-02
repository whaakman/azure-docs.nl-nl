---
title: Azure SQL Database-bestand ruimtebeheer | Microsoft Docs
description: Deze pagina wordt beschreven hoe u voor het beheren van bestandsruimte met Azure SQL Database, en bevat voorbeelden van code voor het bepalen of u moet voor het verkleinen van een database ook hoe om uit te voeren een database bewerking verkleinen.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 1ecc0ce08ef42f5f5935bca29e8269be2ea142f0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415146"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Ruimte in Azure SQL Database beheren

Dit artikel beschrijft de verschillende typen opslagruimte in Azure SQL Database en de stappen die kunnen worden uitgevoerd wanneer de bestandsruimte voor databases toegewezen en elastische pools moet worden beheerd door de klant.

## <a name="overview"></a>Overzicht

In Azure SQL Database meten weergegeven in de Azure-portal en de volgende API's voor metrische opslaggegevens grootte het aantal pagina's van gegevens die worden gebruikt voor databases en elastische pools.
- Azure Resource Manager op basis van metrische gegevens over API's zoals PowerShell [get-metrische gegevens](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Er zijn patronen van werkbelasting waarin de toewijzing van ruimte in de onderliggende gegevensbestanden voor databases die groter zijn dan het aantal pagina's van gegevens die worden gebruikt in de gegevensbestanden wordt. Dit kan gebeuren wanneer ruimte toeneemt gebruikt en gegevens worden vervolgens verwijderd. Wanneer de gegevens wordt verwijderd, wordt de toegewezen bestandsruimte niet automatisch vrijgemaakt als de gegevens worden verwijderd. In dergelijke scenario's, de toegewezen ruimte voor een database of een groep van toepassingen kan groter zijn dan de ondersteunde maximale limieten instellen (of ondersteund) voor de database en, als gevolg hiervan, te voorkomen dat de groei van gegevens of te voorkomen dat prestaties laagwijzigingen, zelfs als de daadwerkelijk gebruikte ruimte in database is kleiner dan de maximale waarde de limiet. Om te beperken, moet u mogelijk de database voor het verminderen van toegewezen, maar niet-gebruikte ruimte in de database te verkleinen.

Ongebruikte toegewezen ruimte vanwege de mogelijke impact op de prestaties van de database-databasebestanden niet automatisch wordt verkleind door de service SQL Database. U kunt het bestand in een database echter verkleinen op een tijdstip van uw keuze met de volgende stappen wordt beschreven in [Reclaim ongebruikte ruimte toegewezen](#reclaim-unused-allocated-space). 

> [!NOTE]
> In tegenstelling tot gegevensbestanden en verkleint de service SQL Database automatisch logboekbestanden omdat die bewerking is niet van invloed op de prestaties van de database.

## <a name="understanding-the-types-of-storage-space-for-a-database"></a>Informatie over de typen van de opslagruimte voor een database

Voor het beheren van de ruimte, moet u inzicht in de volgende voorwaarden met betrekking tot de database-opslag voor een individuele database en voor een elastische pool.

|Storage space term|Definitie|Opmerkingen|
|---|---|---|
|**Gebruikte gegevensruimte**|De hoeveelheid ruimte die wordt gebruikt voor het opslaan van gegevens uit een database op pagina's van 8 KB.|Deze ruimte gebruikt in het algemeen toeneemt (afname) op voegt (verwijderen). In sommige gevallen de ruimte die wordt gebruikt niet wordt gewijzigd op ingevoegd of wordt verwijderd, afhankelijk van de hoeveelheid en het patroon van de gegevens die betrokken zijn bij de bewerking en eventuele fragmentatie van gegevens. Bijvoorbeeld, komt verwijderen van één rij uit elke gegevenspagina niet noodzakelijkerwijs verkleinen de ruimte die wordt gebruikt.|
|**Toegewezen ruimte**|De hoeveelheid opgemaakt bestandsruimte beschikbaar is voor het opslaan van gegevens uit een database gemaakt|De toegewezen ruimte automatisch vergroot, maar nooit afneemt na het verwijderen. Dit gedrag zorgt ervoor dat toekomstige voegt sneller zijn sinds de ruimte hoeft niet opnieuw worden geformatteerd.|
|**Toegewezen maar niet-gebruikte ruimte**|De hoeveelheid niet-gebruikte gegevens bestand toegewezen ruimte voor de database.|Dit aantal is het verschil tussen de hoeveelheid ruimte die is toegewezen en de gebruikte ruimte, en vertegenwoordigt de maximale hoeveelheid ruimte die kan worden vrijgemaakt door databasebestanden te verkleinen.|
|**Maximale grootte**|De maximale hoeveelheid ruimte die kan worden gebruikt door de database.|De toegewezen gegevensruimte kan niet groter wordt dan de maximale grootte van gegevens.|
||||

Het volgende diagram illustreert de relatie tussen de typen van opslagruimte.

![ruimte opslagtypen en relaties](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-database-for-storage-space-information"></a>Query uitvoeren op een database voor storage space informatie

Om te bepalen als u hebt toegewezen, maar ongebruikt gegevensruimte voor een individuele database dat u wilt vrijmaken, gebruik de volgende query's:

### <a name="database-data-space-used"></a>Databaseruimte voor gegevens die worden gebruikt
De volgende query uit om de hoeveelheid ruimte in database gegevens gebruikt in MB te wijzigen.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-allocated-and-allocated-space-unused"></a>Databasegegevens toegewezen en ongebruikte ruimte toegewezen
De volgende query om de hoeveelheid gegevens toegewezen en ongebruikte ruimte toegewezen terug te wijzigen.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-max-size"></a>Maximale databasegrootte
De volgende query om de maximale grootte van de database terug in bytes te wijzigen.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Een elastische pool voor storage space informatie opvragen

Om te bepalen als u hebt toegewezen, maar ongebruikt gegevensruimte in een elastische pool en voor elke gepoolde database dat u wilt vrijmaken, gebruik de volgende query's:

### <a name="elastic-pool-data-space-used"></a>Gegevensruimte op de elastische pool gebruikt
De volgende query uit om de hoeveelheid elastische pool gegevens schijfruimte in MB te wijzigen.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-allocated-and-allocated-space-unused"></a>Gegevens van de elastische pool toegewezen en ongebruikte ruimte toegewezen

Wijzig de volgende PowerShell-script om te retourneren een tabel met de totale ruimte die is toegewezen en ongebruikte ruimte die is toegewezen voor elke database in een elastische pool. De tabel orders databases die met de grootste hoeveelheid ruimte die is toegewezen aan de minimale hoeveelheid toegewezen ruimte niet-gebruikte niet-gebruikte.  

De resultaten van de query voor het bepalen van de toegewezen ruimte voor elke database in de pool kan samen worden toegevoegd aan de Bepaal de elastische pool-ruimte toegewezen. De toegewezen ruimte voor de elastische pool mag niet groter zijn dan de maximale grootte van de elastische pool.  

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

### <a name="elastic-pool-max-size"></a>Maximale grootte van elastische pool

Gebruik de volgende T-SQL-query om terug te keren van de elastische database maximale grootte in MB.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Niet-gebruikte toegewezen ruimte vrijmaken

Nadat u hebt vastgesteld dat u hebt niet-gebruikte toegewezen ruimte die u wilt vrijmaken, gebruik de volgende opdracht voor het verkleinen van de toegewezen databaseruimte. 

> [!IMPORTANT]
> Databases met de meeste toegewezen ruimte voor de databases in een elastische pool en niet-gebruikte moet eerst worden gecomprimeerd bestand om vrij te maken is de snelste.  

Gebruik de volgende opdracht voor het verkleinen van alle van de bestanden in de opgegeven database:

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'<database_name>')
```

Zie voor meer informatie over deze opdracht [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

> [!IMPORTANT] 
> Houd rekening met herbouwen database indexen na de databasebestanden van de gegevens worden gecomprimeerd, indexen gefragmenteerd raken en hun effectiviteit van de optimalisatie van prestaties verliezen. Als dit het geval is, klikt u vervolgens de indexen moeten opnieuw worden opgebouwd. Zie voor meer informatie over fragmentatie en opnieuw opbouwen van indexen [Reorganize en indexen opnieuw samenstellen](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over de grootte van maximaal database:
  - [Azure SQL Database vCore gebaseerde model limieten voor één database aanschaffen](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [Resourcelimieten voor individuele databases met behulp van het op DTU gebaseerde aankoopmodel](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Azure SQL Database vCore gebaseerde model limieten voor elastische pools aanschaffen](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [Limieten voor elastische pools met behulp van het op DTU gebaseerde aankoopmodel resources](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Voor meer informatie over de `SHRINKDATABASE` opdracht, Zie [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Zie voor meer informatie over fragmentatie en opnieuw opbouwen van indexen [Reorganize en indexen opnieuw samenstellen](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).