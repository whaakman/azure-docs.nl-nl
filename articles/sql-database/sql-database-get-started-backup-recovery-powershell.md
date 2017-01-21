---
title: Aan de slag met back-ups en herstel van Azure SQL-databases voor gegevensbescherming en -herstel met behulp van Azure PowerShell | Microsoft Docs
description: In deze zelfstudie leert u hoe u via geautomatiseerde back-ups kunt herstellen naar een eerder tijdstip, geautomatiseerde back-ups kunt opslaan in de Azure Recovery Services-kluis en kunt herstellen vanuit de Azure Recovery Services-kluis met behulp van PowerShell
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 12/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 68a4ed7aad946dda644a0f085c48fd33f453e018
ms.openlocfilehash: 15d5cb803332133c8015a8ba23ca5751b8abc29a


---


# <a name="get-started-with-backup-and-restore-for-data-protection-and-recovery-using-powershell"></a>Aan de slag met back-ups en herstel voor gegevensbescherming en -herstel met PowerShell

In deze Aan de slag-zelfstudie leert u hoe u Azure PowerShell kunt gebruiken voor het volgende:

- Bestaande back-ups van een database weergeven
- Een database herstellen naar een eerder tijdstip
- Een langetermijnretentie voor een databaseback-upbestand configureren in de Azure Recovery Services-kluis
- Een database herstellen vanuit de Azure Recovery Services-kluis

**Geschatte tijd**: deze zelfstudie duurt circa 30 minuten (mits u al aan de vereisten voldoet).


## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* U moet verbinding maken met Azure met een account dat eigenaar is van het abonnement of de rol Inzender heeft. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* U moet de nieuwste versie van Azure PowerShell installeren en uitvoeren. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

* U hebt de zelfstudie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure Portal en SQL Server Management Studio](sql-database-get-started.md) of de equivalente [PowerShell-versie](sql-database-get-started-powershell.md) voltooid. Als u dit niet hebt gedaan, voltooit u eerst deze vereiste zelfstudie of voert u het PowerShell-script aan het einde van de [PowerShell-versie](sql-database-get-started-powershell.md) voordat u doorgaat.

> [!TIP]
> U kunt dezelfde taken uitvoeren in een Aan de slag-zelfstudie met [Azure Portal](sql-database-get-started-backup-recovery.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Het oudste herstelpunt van de door de service gegenereerde back-ups van een database weergeven

In deze sectie van de zelfstudie kunt u informatie bekijken over het oudste herstelpunt van de [door de service gegenereerde automatische back-ups](sql-database-automated-backups.md) van uw database. 

U kunt een database herstellen naar een bepaald tijdstip tussen het eerste herstelpunt en de nieuwste back-up (6 minuten vóór de huidige tijd). 

Voor het volgende fragment wordt de cmdlet [Get-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabase) gebruikt om het eerste herstelpunt op te halen van de database die u wilt herstellen. De tijd wordt geretourneerd met UTC-indeling, maar in de volgende fragmenten ziet u hoe u in uw lokale tijd kunt werken. Het nieuwste beschikbare herstelpunt van een actieve database is meestal ongeveer zes minuten oud. Daarom wordt het nieuwste herstelpunt eenvoudigweg ingesteld op de huidige tijd min zes minuten. 

```
# Get available restore points

$resourceGroupName = "{resource-group-name}"
$serverName = "{server-name}"
$databaseName = "{database-name}"

$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"
```



## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip

In deze sectie van de zelfstudie herstelt u de database naar een nieuwe database vanaf een bepaald punt in de tijd. 

>[!NOTE]
>U kunt de server waarop u naar een bepaald punt in de tijd wilt herstellen niet wijzigen. Als u wilt herstellen naar een andere server, gebruikt u [Geo-herstel](sql-database-disaster-recovery.md#recover-using-geo-restore). U kunt herstellen naar een [elastische databasepool](sql-database-elastic-jobs-overview.md) of naar een andere prijscategorie. 

In het volgende fragment wordt de cmdlet [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase) gebruikt om de $databaseToRestore terug te zetten die u in het vorige fragment hebt ingesteld. Voor de parameter **-PointInTime** is een UTC-tijdwaarde nodig die lijkt op: *12/09/2016 20:00:00*. Het fragment converteert de lokale tijd voor u.

```
# Restore a database to a previous point in time

#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}
$newRestoredDatabaseName = "{new-database-name}"
$localTimeToRestoreTo = "{12/9/2016 12:00:00 PM}" # change to a valid restore point for your database
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb
```

> [!NOTE]
> Hier kunt u verbinding maken met de herstelde database met behulp van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Langetermijnretentie van automatische back-ups configureren in een Azure Recovery Services-kluis 

In deze sectie van de zelfstudie configureert u [een Azure Recovery Services-kluis voor de retentie van automatische back-ups](sql-database-long-term-retention.md) gedurende een periode die langer is dan de retentietermijn van uw servicecategorie (maximaal 10 jaar). 


> [!TIP]
> Zie [Delete long-term retention backups](sql-database-long-term-retention-delete.md) (Back-ups voor langetermijnretentie verwijderen) als u back-ups voor langetermijnretentie wilt verwijderen.


### <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

> [!IMPORTANT]
> De kluis moet zich bevinden in dezelfde regio als de logische Azure SQL-server en moet dezelfde resourcegroep gebruiken als de logische server.

Voor het volgende fragment wordt gebruikgemaakt van de cmdlets [New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/new-azurermrecoveryservicesvault) en [Set-AzureRmRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesbackupproperties) om een nieuwe Recovery Services-kluis te maken en om een redundantieniveau voor back-ups in te stellen in de kluis. 

```
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-you-just-created-for-its-long-term-retention-backups"></a>Stel uw server zodanig in dat de Recovery-kluis die u zojuist hebt gemaakt, wordt gebruikt voor het langdurig bewaren van back-ups

In het volgende fragment wordt de cmdlet [Set-AzureRmSqlServerBackupLongTermRetentionVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverbackuplongtermretentionvault) gebruikt om de eerder gemaakte Recovery Services-kluis te koppelen aan een specifieke Azure SQL-server.

```
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Retentiebeleid maken

In een retentiebeleid stelt u in hoe lang een databaseback-up moet worden bewaard. 

In het volgende fragment wordt [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) gebruikt om het standaardretentiebeleid op te halen dat u gaat gebruiken als sjabloon voor het maken van nieuw beleid. Het sjabloon wordt zodanig ingesteld dat back-ups gedurende twee jaar worden bewaard. Voer vervolgens [New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/new-azurermrecoveryservicesbackupprotectionpolicy) uit om het nieuwe beleid te maken. 

Voor sommige cmdlets moet u de kluiscontext instellen vóór het uitvoeren ([Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesvaultcontext)) zodat u de cmdlet ook in enkele gerelateerde fragmenten te zien krijgt. De context wordt ingesteld omdat het beleid deel uitmaakt van de kluis. U kunt meerdere beleidsregels voor retentie maken voor elke kluis en vervolgens het gewenste beleid toepassen op specifieke databases. 


```
# Retrieve the default retention policy for the AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention value to two years (you can set to any time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-to-use-the-previously-defined-retention-policy"></a>Een database configureren voor gebruik van het eerder gedefinieerde retentiebeleid

In het volgende fragment wordt de cmdlet [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqldatabasebackuplongtermretentionpolicy) gebruikt om het nieuwe beleid toe te passen op een specifieke database.

```
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

> [!IMPORTANT]
> Na de configuratie worden back-ups in de kluis binnen de komende zeven dagen weergegeven. Ga pas verder met deze zelfstudie als er back-ups in de kluis worden weergegeven.

## <a name="view-backup-info-and-backups-in-long-term-retention"></a>Back-upinformatie en back-ups met langetermijnretentie weergeven

In deze sectie van de zelfstudie kunt u informatie bekijken over uw databaseback-ups met [langetermijnretentie](sql-database-long-term-retention.md). 

In de volgende fragmenten wordt met een query informatie opgevraagd uit de kluis. Hiervoor worden de cmdlets [Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupcontainer), [Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupitem) en [Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackuprecoverypoint) gebruikt.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set the vault context to the vault we want to restore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore


# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```


## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Een database herstellen vanuit een back-up met langetermijnretentie

In deze sectie van de zelfstudie herstelt u de database naar een nieuwe database vanuit een back-up in de Azure Recovery Services-kluis.

Net als bij de tijdsgebonden fragmenten eerder in deze zelfstudie wordt voor het herstellen van langdurig bewaarde back-ups de cmdlet [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase) gebruikt, maar deze keer met de parameter **-FromLongTermRetentionBackup** (in plaats van de eerder gebruikte parameter **-FromPointInTimeBackup**).

```
# Restore the most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> Hier kunt u verbinding maken met de herstelde database met behulp van SQL Server Management Studio om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="complete-azure-powershell-script-to-restore-from-a-previous-point-in-time-and-to-configure-and-restore-from-long-term-backup-retention-using-powershell"></a>Volledig Azure PowerShell-script voor het herstellen vanaf een eerder moment, om langdurige back-upretentie met PowerShell te configureren en om dergelijke back-ups terug te zetten

> [!NOTE]
> Als u probeert te herstellen met de meest recente back-up aan het einde van dit script, wordt de uitzondering *Kan niet indexeren in een lege matrix* weergegeven als er geen back-ups in de kluis staan.

```
# Sign in to Azure and set the subscription to work with
########################################################

$subscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId


# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myServerName = "{server-name}"
$myDatabaseToRestoreFromPointInTime = "{database-name}"
$myLocalTimeToRestoreTo = "{12/08/2016 16:00:00}" # change to a valid restore point for your database
$myNewDatabaseRestoredFromPointInTime = "{new-database-name}"

$myRecoveryServiceVaultName = "{vault-name}"
$myRetentionPolicyDurationType = "{duration-period}" # set to Years, Months, or Weeks
$myRetentionPolicyDuration = {2}
$myRetentionPolicyName = "{retention-policy-name}"
$myDatabaseToRestoreFromLTR = "{database-name}"
$myNewDatabaseRestoredFromLTR = "{new-database-name}"


# Get available restore points for a specific database
######################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseToRestoreFromPointInTime


$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"


# Restore a database to a previous point in time
################################################

$newRestoredDatabaseName = $myNewDatabaseRestoredFromPointInTime
$localTimeToRestoreTo = $myLocalTimeToRestoreTo
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb



# CONFIGURE LONG-TERM RETENTION

# Create a recovery services vault
##################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = $myRecoveryServiceVaultName

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $resourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
$vault

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id

# Retrieve the default retention policy for the AzureSQLDatabase workload type
##############################################################################

$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention values
$retentionPolicy.RetentionDurationType = $myRetentionPolicyDurationType
$retentionPolicy.RetentionCount = $myRetentionPolicyDuration

$retentionPolicyName = $myRetentionPolicyName

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy

$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id


$databaseNeedingRestore = $myDatabaseToRestoreFromLTR

# Set the vault context to the vault we want to restore from
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Get the container associated with your vault
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore


# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
###

# This command restores the most recent backup: $availableBackups[0]
$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$restoredDatabaseName = $myNewDatabaseRestoredFromLTR
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDbFromLtr = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel

$restoredDbFromLtr
```

## <a name="next-steps"></a>Volgende stappen

- Zie [Automatic backups](sql-database-automated-backups.md) (Automatische back-ups) voor meer informatie over door de service gegenereerde automatische back-ups.
- Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over back-ups met langetermijnretentie.
- Zie [Herstellen vanuit back-up](sql-database-recovery-using-backups.md) voor meer informatie over het herstellen van back-ups.


<!--HONumber=Dec16_HO4-->


