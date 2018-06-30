---
title: Back-up op lange termijn bewaren van Azure SQL Database beheren | Microsoft Docs
description: Meer informatie over het automatische back-ups opslaan in de SQL Azure-opslag en deze herstellen
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: f7125a18aa2496ebe8367443a67502a7a7dbac02
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128510"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Back-up op lange termijn bewaren van Azure SQL Database beheren

U kunt Azure SQL database met een [lange bewaartermijn van de back-](sql-database-long-term-retention.md) beleid (LTR) voor het bewaren van automatisch back-ups in Azure blob-opslag voor maximaal tien jaar. U kunt een database met behulp van deze back-ups met de Azure-portal of PowerShell vervolgens herstellen.

> [!NOTE]
> Als onderdeel van de initiële versie van de evaluatieversie van dit onderdeel in oktober 2016, zijn back-ups opgeslagen in de kluis van Azure Services Recovery Service. Deze update verwijdert u deze afhankelijkheid, maar voor achterwaartse compatibiliteit de oorspronkelijke API tot en met 31 mei 2018 wordt ondersteund. Als u nodig hebt om te communiceren met back-ups in de Azure-Services Recovery-kluis, Zie [langdurig back-up bewaren met behulp van Azure Services Recovery Service kluis](sql-database-long-term-backup-retention-configure-vault.md). 

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>De Azure portal gebruiken voor en terugzetten van back-ups op lange termijn bewaarbeleidsregels configureren

De volgende secties laten zien hoe de Azure portal gebruiken voor de lange bewaartermijn configureren, back-ups in lange bewaartermijn weergeven en terugzetten van back-up van lange bewaartermijn.

### <a name="configure-long-term-retention-policies"></a>Op lange termijn bewaarbeleidsregels configureren

U kunt SQL Database configureren [automatische back-ups behouden](sql-database-long-term-retention.md) gedurende een periode langer is dan de bewaarperiode voor de servicetier. 

1. In de Azure portal, selecteer uw SQL-server en klik vervolgens op **lange bewaartermijn van de back-**.

   ![koppeling langetermijnretentie](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Op de **-beleid configureren** tabblad, selecteert u de database die u wilt instellen of wijzigen op lange termijn bewaarbeleid voor back-up.

   ![database selecteren](./media/sql-database-long-term-retention/ltr-configure-select-database.png)

3. In de **-beleid configureren** deelvenster, selecteer deze optie indien wilt behouden wekelijkse, maandelijkse of jaarlijkse back-ups en de bewaarperiode voor elke opgeven. 

   ![-beleid configureren](./media/sql-database-long-term-retention/ltr-configure-policies.png)

4. Wanneer voltooid, klikt u op **toepassen**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Weergeven van de back-ups en terugzetten vanuit een back-up met Azure portal

De back-ups die worden bewaard voor een specifieke database met een beleid van links naar rechts en herstel van deze back-ups weergeven. 

1. In de Azure portal, selecteer uw SQL-server en klik vervolgens op **lange bewaartermijn van de back-**.

   ![koppeling langetermijnretentie](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Op de **beschikbare back-ups** tabblad, selecteert u de database waarvan u wilt zien van de beschikbare back-ups.

   ![database selecteren](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. In de **beschikbare back-ups** deelvenster Bekijk de beschikbare back-ups. 

   ![back-ups weergeven](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Selecteer de back-up van waaruit u wilt herstellen en geef vervolgens de naam van de nieuwe database.

   ![De pagina Restore](./media/sql-database-long-term-retention/ltr-restore.png)

5. Klik op **OK** de database te herstellen van de back-up in Azure SQL-opslag naar de nieuwe database.

6. Klik op de werkbalk op het meldingspictogram om de status van de hersteltaak weer te geven.

   ![taakvoortgang herstellen vanuit kluis](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Als de hersteltaak is voltooid, opent u de **SQL-databases** pagina om de herstelde database weer te geven.

> [!NOTE]
> Hier kunt u verbinding maken met de herstelde database met behulp van SQL Server Management Studio om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>PowerShell gebruiken om te en terugzetten van back-ups op lange termijn bewaarbeleidsregels configureren

De volgende secties laten zien hoe PowerShell gebruiken voor het configureren van de lange bewaartermijn voor de back-up, back-ups weergeven in Azure SQL-opslag- en herstel van een back-up in Azure SQL-opslag.

> [!IMPORTANT]
> LTR V2 API wordt ondersteund in de volgende PowerShell-versies:
- [AzureRM.Sql 4.5.0](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0) of hoger
- [AzureRM 6.1.0](https://www.powershellgallery.com/packages/AzureRM/6.1.0) of hoger
> 

### <a name="create-an-ltr-policy"></a>Een beleid LTR maken

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>LTR-beleid weergeven
Dit voorbeeld ziet u hoe u het beleid LTR in een server

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Een beleid LTR wissen
Dit voorbeeld ziet u hoe u een beleid LTR uit een database

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>LTR back-ups weergeven

Dit voorbeeld toont hoe u de LTR back-ups binnen een server. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Back-ups LTR verwijderen

In dit voorbeeld laat zien hoe een LTR verwijderen uit de lijst met back-ups back-up.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>Herstellen vanuit back-ups van links naar rechts
In dit voorbeeld laat zien hoe een LTR back-up terugzetten. Houd er rekening mee deze interface is niet veranderd, maar de resource-id-parameter nu de LTR back-resource-id vereist. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Hier kunt u verbinding kunt maken met de herstelde database met SQL Server Management Studio benodigde taken uit te voeren, zoals het ophalen van een bits van gegevens uit de herstelde database kopiëren naar de bestaande database of verwijder de bestaande database en de naam van de herstelde de database is de naam van de bestaande database. Zie [punt in tijd terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Volgende stappen

- Zie [Automatic backups](sql-database-automated-backups.md) (Automatische back-ups) voor meer informatie over door de service gegenereerde automatische back-ups.
- Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over back-ups met langetermijnretentie.
