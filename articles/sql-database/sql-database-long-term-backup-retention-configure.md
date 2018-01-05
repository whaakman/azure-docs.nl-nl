---
title: Back-up langdurig bewaren - Azure SQL-database configureren | Microsoft Docs
description: Meer informatie over het automatische back-ups opslaan in de Azure Recovery Services-kluis en te herstellen van de Azure Recovery Services-kluis
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: carlrab
ms.openlocfilehash: e75facfd77fc1cb3c23aa4e1f6f7f799620fef39
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a>En terugzetten van back-up op lange termijn bewaren van Azure SQL Database configureren

U kunt de Azure Recovery Services-kluis voor het opslaan van back-ups van Azure SQL database en vervolgens een database herstelt via back-ups behouden in de kluis die met de Azure-portal of PowerShell configureren.

## <a name="azure-portal"></a>Azure Portal

De volgende secties laten zien hoe de Azure portal gebruiken voor het configureren van de Azure Recovery Services-kluis, back-ups weergeven in de kluis en terugzetten van de kluis.

### <a name="configure-the-vault-register-the-server-and-select-databases"></a>Configureren van de kluis, de-server registreren en databases selecteren

U [configureren van een Azure Recovery Services-kluis voor automatische back-ups behouden](sql-database-long-term-retention.md) gedurende een periode langer is dan de bewaarperiode voor de servicetier. 

1. Open de **SQL Server** pagina voor uw server.

   ![pagina met SQL server](./media/sql-database-get-started-portal/sql-server-blade.png)

2. Klik op **Langetermijnretentie**.

   ![koppeling langetermijnretentie](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Op de **lange bewaartermijn van de back-** pagina voor uw server, lees en accepteer de preview-voorwaarden (tenzij u hebben gedaan - of deze functie niet langer in preview is).

   ![voorwaarden voor weergeven van een voorbeeld accepteren](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Lange bewaartermijn van de back-up configureren, dat de database in het raster selecteren en klik vervolgens op **configureren** op de werkbalk.

   ![database selecteren voor langetermijnretentie](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Op de **configureren** pagina, klikt u op **vereiste instellingen configureren** onder **Recovery-kluis service**.

   ![koppeling kluis](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Op de **Recovery services-kluis** pagina, selecteert u een bestaande kluis, indien van toepassing. Als er voor uw abonnement geen Recovery Services-kluis is gevonden, klikt u om de werkstroom af te sluiten en een Recovery Services-kluis te maken.

   ![koppeling van de kluis maken](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Op de **Recovery Services-kluizen** pagina, klikt u op **toevoegen**.

   ![kluis koppeling toevoegen](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Op de **Recovery Services-kluis** pagina, Geef een geldige naam voor de Recovery Services-kluis.

   ![naam nieuwe kluis](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Selecteer uw abonnement en resourcegroep. Selecteer vervolgens de locatie voor de kluis. Klik op **Maken** als u klaar bent.

   ![Kluis maken](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > De kluis moet zich bevinden in dezelfde regio als de logische Azure SQL-server en moet dezelfde resourcegroep gebruiken als de logische server.
   >

10. Nadat de nieuwe kluis is gemaakt, uitvoeren van de benodigde stappen om terug te keren naar de **Recovery services-kluis** pagina.

11. Op de **Recovery services-kluis** pagina, klikt u op de kluis en klik vervolgens op **Selecteer**.

   ![bestaande kluis selecteren](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Op de **configureren** pagina, Geef een geldige naam voor de nieuw bewaarbeleid, het bewaarbeleid standaard zo nodig wijzigen en klik vervolgens op **OK**.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)
   
   >[!NOTE]
   >Bewaarperiode beleidsnamen zijn een aantal tekens, inclusief spaties niet toegestaan.

13. Op de **lange bewaartermijn van de back-** pagina voor uw database, klikt u op **opslaan** en klik vervolgens op **OK** de lange termijn bewaren van back-beleid toepassen op alle geselecteerde databases.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klik op **Opslaan** om langetermijnretentie voor back-ups in te schakelen met dit nieuwe beleid voor de Azure Recovery Services-kluis die u hebt geconfigureerd.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Na de configuratie worden back-ups in de kluis binnen de komende zeven dagen weergegeven. Ga pas verder met deze zelfstudie als er back-ups in de kluis worden weergegeven.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Back-ups weergeven in lange bewaartermijn met Azure portal

Informatie weergeven over uw databaseback-ups in [lange bewaartermijn van de back-](sql-database-long-term-retention.md). 

1. Open uw Azure Recovery Services-kluis voor back-ups van uw database in de Azure-portal (Ga naar **alle resources** en selecteert u deze in de lijst met resources voor uw abonnement) om de hoeveelheid opslagruimte die wordt gebruikt door uw back-ups in de kluis weer te geven.

   ![recovery services-kluis weergeven met back-ups](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Open de **SQL-database** pagina voor uw database.

   ![nieuwe db voorbeeldpagina](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Klik op de werkbalk op **Herstellen**.

   ![werkbalk herstellen](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Klik op de pagina terugzetten **op lange termijn**.

5. Klik onder de back-ups van de Azure kluis op **Een back-up selecteren** om de beschikbare databaseback-ups met langetermijnretentie weer te geven.

   ![back-ups in kluis](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-the-azure-portal"></a>Een database herstellen vanaf een back-up in lange Backup-bewaartermijn met de Azure portal

U kunt de database herstellen naar een nieuwe database vanuit een back-up in de Azure Recovery Services-kluis.

1. Op de **Azure kluis back-ups** pagina, klikt u op de back-up herstellen en klik vervolgens op **Selecteer**.

   ![back-up in kluis selecteren](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. Geef in het tekstvak **Databasenaam** de naam voor de herstelde database op.

   ![nieuwe databasenaam](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Klik op **OK** om de database vanuit de back-up in de kluis naar de nieuwe database te herstellen.

4. Klik op de werkbalk op het meldingspictogram om de status van de hersteltaak weer te geven.

   ![taakvoortgang herstellen vanuit kluis](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Als de hersteltaak is voltooid, opent u de **SQL-databases** pagina om de herstelde database weer te geven.

   ![uit kluis herstelde database](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Hier kunt u verbinding maken met de herstelde database met behulp van SQL Server Management Studio om noodzakelijke taken uit te voeren, zoals [een deel van de gegevens uit de herstelde database extraheren om naar de bestaande database te kopiëren, of de bestaande database verwijderen en de naam van de herstelde database wijzigen in de naam van de bestaande database](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="powershell"></a>PowerShell

De volgende secties laten zien hoe PowerShell gebruiken voor het configureren van de Azure Recovery Services-kluis, back-ups weergeven in de kluis en terugzetten van de kluis.

### <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Gebruik de [nieuw AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) voor het maken van een recovery services-kluis.

> [!IMPORTANT]
> De kluis moet zich bevinden in dezelfde regio als de logische Azure SQL-server en moet dezelfde resourcegroep gebruiken als de logische server.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-for-its-long-term-retention-backups"></a>Stel uw server met de recovery-kluis voor de lange termijn bewaren van back-ups

Gebruik de [Set AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet een eerder gemaakte recovery services-kluis koppelen aan een specifieke Azure SQL-server.

```PowerShell
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Retentiebeleid maken

In een retentiebeleid stelt u in hoe lang een databaseback-up moet worden bewaard. Gebruik de [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet ophalen van het bewaarbeleid standaard moet worden gebruikt als de sjabloon voor het maken van beleid. In deze sjabloon is de bewaarperiode ingesteld voor 2 jaar. Voer vervolgens de [nieuw AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) ten slotte het beleid te maken. 

> [!NOTE]
> Sommige cmdlets vereisen dat u de context van de kluis voordat u ingesteld ([Set AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) zodat u deze cmdlet in enkele verwante codefragmenten zien. U stelt u de context omdat het beleid deel uit van de kluis maakt. U kunt meerdere beleidsregels voor retentie maken voor elke kluis en vervolgens het gewenste beleid toepassen op specifieke databases. 


```PowerShell
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

Gebruik de [Set AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet naar het nieuwe beleid toepassen op een specifieke database.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Back-upinformatie en back-ups met langetermijnretentie weergeven

Informatie weergeven over uw databaseback-ups in [lange bewaartermijn van de back-](sql-database-long-term-retention.md). 

De volgende cmdlets gebruiken om back-informatie weer te geven:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
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

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Een database herstellen vanuit een back-up met langetermijnretentie

Terugzetten vanaf een lange bewaartermijn van de back-up gebruikt de [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.

```PowerShell
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
> Hier kunt u verbinding kunt maken met de herstelde database met SQL Server Management Studio benodigde taken uit te voeren, zoals het ophalen van een bits van gegevens uit de herstelde database kopiëren naar de bestaande database of verwijder de bestaande database en de naam van de herstelde database op de naam van de bestaande database. Zie [punt in tijd terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Volgende stappen

- Zie [Automatic backups](sql-database-automated-backups.md) (Automatische back-ups) voor meer informatie over door de service gegenereerde automatische back-ups.
- Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over back-ups met langetermijnretentie.
- Zie [Herstellen vanuit back-up](sql-database-recovery-using-backups.md) voor meer informatie over het herstellen van back-ups.
