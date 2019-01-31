---
title: Langetermijnretentie - Azure SQL Database configureren | Microsoft Docs
description: Leer hoe u voor het opslaan van geautomatiseerde back-ups in de Azure Recovery Services-kluis en kunt herstellen vanuit de Azure Recovery Services-kluis
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma,carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 5100ef307bc125b21e1c42c87856492a4a496065
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55455629"
---
# <a name="configure-long-term-backup-retention-using-azure-recovery-services-vault"></a>Langetermijnretentie met behulp van Azure Recovery Services-kluis configureren

U kunt de Azure Recovery Services-kluis voor het opslaan van back-ups van Azure SQL database en vervolgens met de back-ups behouden in de kluis met de Azure portal of PowerShell een database te herstellen.

> [!NOTE]
> Als onderdeel van de eerste release van de Preview-versie van langetermijnretentie in oktober 2016, zijn back-ups opgeslagen in de Recovery-Service van Azure-Services-kluis. Deze update verwijdert deze afhankelijkheid, maar voor achterwaartse compatibiliteit de oorspronkelijke API tot en met 31 mei 2018 wordt ondersteund. Als u nodig hebt om te communiceren met back-ups in de Azure-Recovery Services-kluis, Zie [langetermijnretentie met behulp van Azure Services Recovery Services-kluis](sql-database-long-term-backup-retention-configure-vault.md).

## <a name="azure-portal"></a>Azure Portal

De volgende secties laten zien hoe u de Azure portal gebruiken voor het configureren van de Azure Recovery Services-kluis, back-ups weergeven in de kluis en terugzetten van de kluis.

### <a name="configure-the-vault-register-the-server-and-select-databases"></a>Configureren van de kluis, de server te registreren en databases selecteren

Configureren van een Azure Recovery Services-kluis [automatische back-ups](sql-database-long-term-retention.md) voor een periode die langer is dan de retentietermijn van uw servicecategorie.

1. Open de **SQL Server** pagina voor uw server.

   ![pagina met SQL server](./media/sql-database-get-started-portal/sql-server-blade.png)

2. Klik op **Langetermijnretentie**.

   ![koppeling langetermijnretentie](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Op de **langetermijnretentie** pagina voor uw server, lees en accepteer onder de preview-voorwaarden (tenzij u nog hebt gedaan, - of deze functie niet langer in de Preview-versie is).

   ![voorwaarden voor weergeven van een voorbeeld accepteren](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Langetermijnretentie van back-up configureren, selecteert u die database in het raster en klik vervolgens op **configureren** op de werkbalk.

   ![database selecteren voor langetermijnretentie](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Op de **configureren** pagina, klikt u op **vereiste instellingen configureren** onder **Recovery Services-kluis**.

   ![koppeling kluis](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Op de **Recovery services-kluis** pagina, selecteert u een bestaande kluis, indien van toepassing. Als er voor uw abonnement geen Recovery Services-kluis is gevonden, klikt u om de werkstroom af te sluiten en een Recovery Services-kluis te maken.

   ![koppeling kluis maken](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Op de **Recovery Services-kluizen** pagina, klikt u op **toevoegen**.

   ![koppeling kluis toevoegen](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)

8. Op de **Recovery Services-kluis** pagina, Geef een geldige naam voor de Recovery Services-kluis.

   ![naam nieuwe kluis](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Selecteer uw abonnement en resourcegroep. Selecteer vervolgens de locatie voor de kluis. Klik op **Maken** als u klaar bent.

   ![kluis maken](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > De kluis moet zich bevinden in dezelfde regio als de SQL Database-server en dezelfde resourcegroep bevinden als de SQL-Database-server moet gebruiken.

10. Nadat de nieuwe kluis is gemaakt, uitvoeren van de benodigde stappen om terug te keren naar de **Recovery services-kluis** pagina.

11. Op de **Recovery services-kluis** pagina, klikt u op de kluis en klik vervolgens op **Selecteer**.

   ![bestaande kluis selecteren](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Op de **configureren** pagina, Geef een geldige naam voor het nieuwe retentiebeleid, het standaardretentiebeleid desgewenst wijzigen en klik vervolgens op **OK**.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

   > [!NOTE]
   > Retentie beleidsnamen toestaan niet van bepaalde tekens, inclusief spaties.

13. Op de **langetermijnretentie** pagina voor uw database, klikt u op **opslaan** en klik vervolgens op **OK** de lange termijn back-upretentie beleid toepassen op alle geselecteerde databases.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klik op **Opslaan** om langetermijnretentie voor back-ups in te schakelen met dit nieuwe beleid voor de Azure Recovery Services-kluis die u hebt geconfigureerd.

   ![retentiebeleid definiëren](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Na de configuratie worden back-ups in de kluis binnen de komende zeven dagen weergegeven. Ga pas verder met deze zelfstudie als er back-ups in de kluis worden weergegeven.

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Back-ups weergeven in met behulp van Azure portal met een langetermijnbewaarperiode

Informatie weergeven over uw databaseback-ups in [langetermijnretentie](sql-database-long-term-retention.md).

1. Open in de Azure-portal, de Azure Recovery Services-kluis voor uw databaseback-ups (Ga naar **alle resources** en selecteer deze in de lijst met resources voor uw abonnement) om weer te geven van de hoeveelheid opslag die wordt gebruikt door uw databaseback-ups in de kluis.

   ![recovery services-kluis weergeven met back-ups](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Open de **SQL-database** pagina voor uw database.

   ![nieuwe db voorbeeldpagina](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Klik op de werkbalk op **Herstellen**.

   ![werkbalk herstellen](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Klik op de gelijknamige pagina **op lange termijn**.

5. Klik onder de back-ups van de Azure kluis op **Een back-up selecteren** om de beschikbare databaseback-ups met langetermijnretentie weer te geven.

   ![back-ups in kluis](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-the-azure-portal"></a>Een database herstellen vanuit een back-ups uit langetermijnretentie met behulp van de Azure portal

U kunt de database herstellen naar een nieuwe database vanuit een back-up in de Azure Recovery Services-kluis.

1. Op de **back-ups Azure kluis** pagina, klikt u op de back-up om te herstellen en klik vervolgens op **Selecteer**.

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

De volgende secties laten zien hoe u PowerShell gebruiken voor het configureren van de Azure Recovery Services-kluis, back-ups weergeven in de kluis en terugzetten van de kluis.

### <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Gebruik de [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) naar een recovery services-kluis maken.

> [!IMPORTANT]
> De kluis moet zich bevinden in dezelfde regio als de SQL Database-server en dezelfde resourcegroep bevinden als de SQL-Database-server moet gebruiken.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-for-its-long-term-retention-backups"></a>Stel uw server met de recovery-kluis voor de lange termijn bewaren back-ups

Gebruik de [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet naar een eerder gemaakte recovery services-kluis koppelen aan een specifieke Azure SQL-server.

```PowerShell
# Set your server to use the vault to for long-term backup retention

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Retentiebeleid maken

In een retentiebeleid stelt u in hoe lang een databaseback-up moet worden bewaard. Gebruik de [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet om op te halen van het standaardretentiebeleid moet worden gebruikt als de sjabloon voor het maken van beleid. In deze sjabloon is de bewaarperiode ingesteld gedurende twee jaar. Voer vervolgens de [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) ten slotte het beleid te maken.

> [!NOTE]
> Sommige cmdlets moet u voordat u de context van de kluis in te stellen ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) zodat u deze cmdlet in enkele gerelateerde fragmenten te zien. U stelt de context omdat het beleid deel uit van de kluis maakt. U kunt meerdere beleidsregels voor retentie maken voor elke kluis en vervolgens het gewenste beleid toepassen op specifieke databases.

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

Gebruik de [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet om het nieuwe beleid toepassen op een specifieke database.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Back-upinformatie en back-ups met langetermijnretentie weergeven

Informatie weergeven over uw databaseback-ups in [langetermijnretentie](sql-database-long-term-retention.md).

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

Herstellen vanuit een langetermijnretentie voor back-up gebruikt de [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.

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
> Hier kunt u verbinding kunt maken met de herstelde database met behulp van SQL Server Management Studio om uit te voeren van de noodzakelijke taken, zoals het ophalen van een deel van de gegevens uit de herstelde database te kopiëren naar de bestaande database of te verwijderen van de bestaande database en wijzig de naam van de herstelde de database de naam van de bestaande database. Zie [punt in tijd herstel](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="how-to-cleanup-backups-in-recovery-services-vault"></a>Het opruimen back-ups in Recovery Services-kluis

Vanaf 1 juli 2018 de LTR-V1-API is gedeprecieerd en alle uw bestaande back-ups in Recovery Services-kluizen zijn gemigreerd naar de LTR storage-containers die worden beheerd door de SQL-Database. Om ervoor te zorgen dat u niet meer in rekening voor de oorspronkelijke back-ups gebracht bent, zijn ze verwijderd uit de kluizen na de migratie. Echter, als u een vergrendeling voor uw kluis hebt geplaatst de back-ups blijven er. Als u wilt de overbodige kosten voorkomen, kunt u handmatig de oude back-ups verwijderen uit de Recovery Services-kluis met het volgende script.

```PowerShell
<#
.EXAMPLE
    .\Drop-LtrV1Backup.ps1 -SubscriptionId “{vault_sub_id}” -ResourceGroup “{vault_resource_group}” -VaultName “{vault_name}”
#>
[CmdletBinding()]
Param (
    [Parameter(Mandatory = $true, HelpMessage="The vault subscription ID")]
    $SubscriptionId,

    [Parameter(Mandatory = $true, HelpMessage="The vault resource group name")]
    $ResourceGroup,

    [Parameter(Mandatory = $true, HelpMessage="The vault name")]
    $VaultName
)

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$vaults = Get-AzureRmRecoveryServicesVault
$vault = $vaults | where { $_.Name -eq $VaultName }

Set-AzureRmRecoveryServicesVaultContext -Vault $vault

$containers = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL

ForEach ($container in $containers)
{
   $canDeleteContainer = $true
   $ItemCount = 0
   Write-Host "Working on container" $container.Name
   $items = Get-AzureRmRecoveryServicesBackupItem -container $container -WorkloadType AzureSQLDatabase
   ForEach ($item in $items)
   {
    write-host "Deleting item" $item.name
    Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints -item $item -Force
   }

   Write-Host "Deleting container" $container.Name
   Unregister-AzureRmRecoveryServicesBackupContainer -Container $container
}
```

## <a name="next-steps"></a>Volgende stappen

- Zie [Automatic backups](sql-database-automated-backups.md) (Automatische back-ups) voor meer informatie over door de service gegenereerde automatische back-ups.
- Zie [Langetermijnretentie](sql-database-long-term-retention.md) voor meer informatie over back-ups met langetermijnretentie.
- Zie [Herstellen vanuit back-up](sql-database-recovery-using-backups.md) voor meer informatie over het herstellen van back-ups.
