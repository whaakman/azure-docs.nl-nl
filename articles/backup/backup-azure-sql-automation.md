---
title: 'Azure Backup: Back-up en herstellen van SQL-Databases in virtuele Azure-machines met Azure Backup en PowerShell'
description: Back-up en herstellen van SQL-Databases in virtuele Azure-machines met Azure Backup en PowerShell.
services: backup
author: pvrk
manager: vijayts
keywords: Azure Backup; SQL;
ms.service: backup
ms.topic: conceptual
ms.date: 03/15/2019
ms.author: pullabhk
ms.assetid: 57854626-91f9-4677-b6a2-5d12b6a866e1
ms.openlocfilehash: 6469e3b35357867b6b38b00c8b11637ba30b2960
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287553"
---
# <a name="back-up-and-restore-sql-databases-in-azure--vms-with-powershell"></a>Back-up en herstellen van SQL-Databases in virtuele Azure-machines met PowerShell

In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken voor back-up en herstellen van een SQL-database binnen een Azure-VM met [Azure Backup](backup-overview.md) Recovery Services-kluis.

In deze zelfstudie wordt het volgende uitgelegd:

> [!div class="checklist"]
> * PowerShell instellen en registreren van de Azure Recovery Services-Provider.
> * Maak een Recovery Services-kluis.
> * Back-up configureren voor SQL-database binnen een Azure-VM.
> * Een back-uptaak uitvoeren.
> * Herstel een back-ups van SQL-database.
> * Back-ups controleren en herstellen van taken.

## <a name="before-you-start"></a>Voordat u begint

* [Meer informatie](backup-azure-recovery-services-vault-overview.md) over Recovery Services-kluizen.
* Meer informatie over de functiemogelijkheden voor [back-ups van SQL-databases in virtuele Azure-machines](backup-azure-sql-database.md#before-you-start).
* Bekijk de PowerShell-objecthiërarchie voor Recovery Services.

### <a name="recovery-services-object-hierarchy"></a>Recovery Services-objecthiërarchie

De objecthiërarchie worden samengevat in het volgende diagram.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Controleer de **Az.RecoveryServices** [cmdlet-verwijzing](/powershell/module/az.recoveryservices) verwijzing in de Azure-bibliotheek.

### <a name="set-up-and-install"></a>Instellen en installeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell instellen als volgt:

1. [Download de nieuwste versie van PowerShell Az](/powershell/azure/install-az-ps). De minimaal vereiste versie is 1.5.0.

2. Zoek de Azure Backup PowerShell-cmdlets met de volgende opdracht:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Bekijk de aliassen en cmdlets voor Azure Backup en Recovery Services-kluis. Hier volgt een voorbeeld van wat wordt weergegeven. Het is niet een volledige lijst met cmdlets.

    ![Lijst met Recovery Services-cmdlets](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. Meld u aan bij uw Azure-account met **Connect AzAccount**.
5. Op de webpagina die wordt weergegeven, wordt u gevraagd voor het invoeren van referenties voor uw account.

    * U kunt ook uw accountreferenties opnemen als een parameter in de **Connect AzAccount** cmdlet met **-referentie**.
    * Als u een CSP-partner werken voor een tenant bent, geeft u de klant als een tenant, met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Een voorbeeld is **Connect AzAccount-Tenant** fabrikam.com.

6. Koppelen van het abonnement dat u gebruiken met het account, wilt omdat een account kan meerdere abonnementen hebt.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Als u Azure Backup voor de eerste keer gebruikt, gebruikt u de **registreren AzResourceProvider** cmdlet voor het registreren van de Azure Recovery Services-provider met uw abonnement.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Controleer of de providers is geregistreerd:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Controleer in de opdrachtuitvoer **RegistrationState** wordt gewijzigd in **geregistreerde**. Als dat niet zo is, wordt uitgevoerd de **registreren AzResourceProvider** cmdlet opnieuw uit.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Volg deze stappen voor het maken van een Recovery Services-kluis.

De Recovery Services-kluis is een Resource Manager-resource, dus u deze in de resourcegroep plaatsen moet. U kunt een bestaande resourcegroep gebruiken of kunt u een resourcegroep met de **New-AzResourceGroup** cmdlet. Wanneer u een resourcegroep maakt, geeft u de naam en locatie voor de resourcegroep.

1. Een kluis is in een resourcegroep geplaatst. Als u geen een bestaande resource groep, maakt u een nieuw bestand met de [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). In dit voorbeeld maken we een nieuwe resourcegroep in de regio VS-West.

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```

2. Gebruik de [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) cmdlet voor het maken van de kluis. Geef dezelfde locatie voor de kluis dat werd gebruikt voor de resourcegroep.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Geef het type redundantie om te gebruiken voor de opslag van de kluis.

    * U kunt [lokaal redundante opslag](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag](../storage/common/storage-redundancy-grs.md).
    * Het volgende voorbeeld wordt de **- BackupStorageRedundancy** optie voor de[Set AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperties?view=azps-1.4.0) cmd voor **testvault** ingesteld op  **GeoRedundant**.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

### <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven

U kunt alle kluizen weergeven in het abonnement, met [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

De uitvoer is vergelijkbaar met het volgende. De gekoppelde resourcegroep en locatie zijn opgegeven.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>De kluiscontext instellen

Store van het object kluis in een variabele en stelt u de context van de kluis.

* Veel Azure Backup-cmdlets vereist het object Recovery Services-kluis als invoer, dus is het handig om op te slaan van het object kluis in een variabele.
* De context van de kluis is het type gegevens dat in de kluis wordt beveiligd. Stelt u deze met [Set AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Nadat de context is ingesteld, is dit van toepassing op alle navolgende cmdlets.

Het volgende voorbeeld wordt de context van de kluis voor **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>De kluis-ID ophalen

Wij van plan bent de context van de kluis instellen in overeenstemming met de Azure PowerShell-richtlijnen beëindigde. In plaats daarvan kunt u opslaan of de kluis-ID ophalen en deze als volgt naar relevante opdrachten doorgeven:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-a-backup-policy"></a>Een back-upbeleid configureren

Een back-upbeleid Hiermee geeft u het schema voor back-ups en hoelang back-up herstelpunten moet worden opgeslagen:

* Een back-upbeleid is gekoppeld aan ten minste één beleid voor Gegevensretentie. Een bewaarbeleid wordt gedefinieerd hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd.
* Het standaard back-upbeleid bewaren met gebruik van [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
* Het standaard back-upbeleid schema met behulp van weergave [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
* U gebruikt de [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet voor het maken van een nieuwe back-upbeleid. Ingevoerde de beleidsobjecten schema en de retentie.

Het volgende voorbeeld wordt het beleid voor planning en het bewaarbeleid in variabelen. Vervolgens wordt deze variabelen als parameters voor een nieuw beleid (**NewSQLPolicy**). **NewSQLPolicy** duurt een dagelijkse 'volledig' back-up, behoudt deze gedurende 180 dagen en wordt een logboekback-up elke 2 uur

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "MSSQL"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "MSSQL"
$NewSQLPolicy = New-AzRecoveryServicesBackupProtectionPolicy -Name "NewSQLPolicy" -WorkloadType "MSSQL" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

De uitvoer is vergelijkbaar met het volgende.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                Frequency                                IsDifferentialBackup IsLogBackupEnabled
                                                                                                                                Enabled
----                 ------------       -------------------- ----------                ---------                                -------------------- ------------------
NewSQLPolicy         MSSQL              AzureWorkload        3/15/2019 9:00:00 PM      Daily                                    False                True
```

## <a name="enable-backup"></a>Back-up inschakelen

### <a name="registering-the-sql-vm"></a>Registreren van de SQL-VM

Voor virtuele Azure-machines en Azure-bestandsshares, Backup-service verbinding maken met deze Azure Resource Manager-resources en de relevante gegevens worden opgehaald. Aangezien SQL een toepassing in een Azure-VM is, Backup-service heeft toestemming nodig voor toegang tot de toepassing en de benodigde informatie ophalen. Als u wilt doen, moet u *'registreren'* de Azure-VM met de SQL-toepassing met een Recovery services-kluis. Wanneer u een SQL-VM met een kluis registreert, kunt u de SQL-databases naar alleen die kluis beveiligen. Gebruik [registreren AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0) PS-cmdlet voor het registreren van de virtuele machine.

````powershell
 $myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
````

De opdracht retourneert een 'back-upcontainer' van deze resource en de status 'geregistreerd'

> [!NOTE]
> Als de parameter force niet is opgegeven, wordt gebruikers gevraagd te bevestigen met een SMS-bericht 'wilt u Schakel de beveiliging voor deze container'. Negeer deze tekst en zeg 'Y' om te bevestigen. Dit is een bekend probleem en we werken als u wilt verwijderen van de tekst en de vereiste voor de parameter force

### <a name="fetching-sql-dbs"></a>Ophalen van de SQL-databases

Zodra de registratie is voltooid, zich Backup-service om alle beschikbare SQL-onderdelen in de VM weer te geven. Om weer te geven van alle SQL-onderdelen die nog worden back-ups moeten op het gebruik van deze kluis [Get-AzRecoveryServicesBackupProtectableItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS-cmdlet

````powershell
Get-AzRecoveryServicesBackupProtectableItem -WorkloadType MSSQL -VaultId $targetVault.ID
````

De uitvoer ziet u alle niet-beveiligde SQL-onderdelen voor alle SQL virtuele machines die worden geregistreerd bij deze kluis met itemtype en servernaam. U kunt verder filteren voor een bepaalde SQL VM door door te geven de '-Container' parameter of gebruik de combinatie van 'Name' en 'Servernaam' samen met ItemType markering om aan te komen tot een unieke SQL-item.

````powershell
$SQLDB = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLDataBase -VaultId $targetVault.ID -Name "<Item Name>" -ServerName "<Server Name>"
````

### <a name="configuring-backup"></a>Back-up configureren

Nu dat we hebben de vereiste SQL-database en het beleid met waarin it moet een back-up, gebruiken we de [inschakelen AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) cmdlet back-up configureren voor deze SQL-database.

````powershell
Enable-AzRecoveryServicesBackupProtection -ProtectableItem $SQLDB -Policy $NewSQLPolicy
````

De opdracht wordt er gewacht tot de configureren back-up is voltooid en de volgende uitvoer retourneert.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
master           ConfigureBackup      Completed            3/18/2019 6:00:21 PM      3/18/2019 6:01:35 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="fetching-new-sql-dbs"></a>Ophalen van de nieuwe SQL-databases

Zodra de computer is geregistreerd, worden de details van de databases vervolgens beschikbaar door Backup-service worden opgehaald. Als de gebruiker later SQL DB's / SQL-exemplaren aan de geregistreerde machine toevoegt, moet één voor het activeren van de Backup-service om uit te voeren van een nieuwe 'query' om op te halen van alle niet-beveiligde databases (met inbegrip van de toegevoegde velden) handmatig opnieuw. Gebruik de [initialiseren AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Initialize-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS-cmdlet op de SQL-VM om uit te voeren van een nieuwe query. De opdracht wacht totdat de bewerking is voltooid. Later gebruiken de [Get-AzRecoveryServicesBackupProtectableItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS-cmdlet om op te halen van de lijst met meest recente niet-beveiligde SQL-onderdelen

````powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
Initialize-AzRecoveryServicesBackupProtectableItem -Container $SQLContainer -WorkloadType MSSQL -VaultId $targetvault.ID
Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLDataBase -VaultId $targetVault.ID
````

Zodra de relevante beveiligbare items worden opgehaald, schakel de back-ups volgens de instructies in de [boven de sectie](#configuring-backup).
Als een niet wilt dat voor het handmatig detecteren van nieuwe databases, ze kunnen kiezen voor autoprotection, zoals wordt beschreven [hieronder](#enable-autoprotection).

## <a name="enable-autoprotection"></a>AutoProtection inschakelen

Een gebruiker kan back-up configureren zodat alle databases in de toekomst toegevoegd worden automatisch beveiligd met een bepaalde beleid. Gebruiken om in te schakelen autoprotection, [inschakelen AzRecoveryServicesBackupAutoProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupAutoProtection?view=azps-1.5.0) PS-cmdlet.

Aangezien de instructie is om back-up van alle toekomstige DB's, de bewerking wordt uitgevoerd op een SQLInstance niveau.

```powershell
$SQLInstance = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLInstance -VaultId $targetVault.ID -Name "<Protectable Item name>" -ServerName "<Server Name>"
Enable-AzRecoveryServicesBackupAutoProtection -InputItem $SQLInstance -BackupManagementType AzureWorkload -WorkloadType MSSQL -Policy $targetPolicy -VaultId $targetvault.ID
```

Zodra de bedoeling autoprotection is opgegeven, wordt de informatie bij de computer voor het ophalen van nieuwe toegevoegd databases plaatsvindt als achtergrondtaak gepland elke 8 uur.

## <a name="restore-sql-dbs"></a>SQL-databases herstellen

Azure Backup kunt SQL Server-databases die worden uitgevoerd op virtuele Azure-machines als volgt herstellen:

1. Met transactielogboekback-ups voor het herstellen naar een specifieke datum of tijd (voor de tweede). Azure Backup bepaalt automatisch de juiste volledige differentiële back-up en de keten van logboekback-ups die nodig zijn om terug te zetten op basis van de geselecteerde tijd.
2. Een specifieke volledige of differentiële back-up te herstellen naar een specifiek herstelpunt herstellen.

Controleer de vereisten die worden vermeld [hier](restore-sql-database-azure-vm.md#prerequisites) vóór het herstellen van SQL-databases.

Eerst ophalen van de relevante back-ups van SQL-database met de [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS-cmdlet.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
````

### <a name="fetch-the-relevant-restore-time"></a>Ophalen van de relevante hersteltijd

Zoals hierboven is beschreven, gebruiker de back-up SQL-database kunt herstellen naar een kopie van de volledige/differentiële **of** naar een logboekbestand point-in-time.

#### <a name="fetch-distinct-recovery-points"></a>Afzonderlijke herstelpunten ophalen

Gebruik [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupRecoveryPoint?view=azps-1.5.0) distinct (volledige/differentiële) herstelpunten ophalen voor een SQL-database waarvan een back-up is gemaakt.

````powershell
$startDate = (Get-Date).AddDays(-7).ToUniversalTime()
$endDate = Get-Date.ToUniversalTime()
Get-AzRecoveryServicesBackupRecoveryPoint -Item $bkpItem -VaultId $targetVault.ID -StartDate $startdate -EndDate $endDate
````

De uitvoer is vergelijkbaar met het volgende voorbeeld

````powershell
RecoveryPointId    RecoveryPointType  RecoveryPointTime      ItemName                             BackupManagemen
                                                                                                  tType
---------------    -----------------  -----------------      --------                             ---------------
6660368097802      Full               3/18/2019 8:09:35 PM   MSSQLSERVER;model             AzureWorkload
````

Gebruik het filter 'RecoveryPointId' of een matrix-filter voor het ophalen van de relevante herstelpunt.

````powershell
$FullRP = Get-AzRecoveryServicesBackupRecoveryPoint -Item $bkpItem -VaultId $targetVault.ID -RecoveryPointId "6660368097802"
````

#### <a name="fetch-point-in-time-recovery-point"></a>Point-in-time-herstelpunt ophalen

Als de gebruiker wil dat de database herstellen naar een bepaalde point-in-time, gebruikt u [Get-AzRecoveryServicesBackupRecoveryLogChain](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupRecoveryLogChain?view=azps-1.5.0) PS-cmdlet. De cmdlet retourneert een lijst met datums die begin- en eindtijden van een ononderbroken en continu logboekback-upketen voor deze SQL-back-upitem vertegenwoordigen. De gewenste punt in tijd moet binnen dit bereik.

```powershell
Get-AzRecoveryServicesBackupRecoveryLogChain -Item $bkpItem -Item -VaultId $targetVault.ID
```

De uitvoer is vergelijkbaar met het volgende voorbeeld.

````powershell
ItemName                       StartTime                      EndTime
--------                       ---------                      -------
SQLDataBase;MSSQLSERVER;azu... 3/18/2019 8:09:35 PM           3/19/2019 12:08:32 PM
````

De bovenstaande uitvoer betekent dat de gebruiker kunt herstellen naar een punt-in-time tussen de weergegeven begintijd en eindtijd. De tijden zijn in UTC. Maken van een point-in-time in PS dat zich binnen het bereik dat hierboven wordt weergegeven.

> [!NOTE]
> Wanneer een logboek point-in-time die voor herstel zijn geselecteerd, worden de gebruiker moet niet opgeven het beginpunt dat wil zeggen, de volledige back-up van waaruit de database wordt hersteld. Azure Backup-service zorgt voor het hele herstelplan dat wil zeggen, welke volledige back-up om te kiezen, wat logboekback-ups om toe te passen, enzovoort.

### <a name="determine-recovery-configuration"></a>Herstelconfiguratie bepalen

In het geval van SQL-database terugzetten, worden de volgende scenario's voor herstellen ondersteund.

1. De SQL-database waarvan een back-up is gemaakt met gegevens uit een ander herstelpunt - OriginalWorkloadRestore overschrijven
2. Herstellen van de SQL-database als een nieuwe database in het hetzelfde exemplaar van SQL - AlternateWorkloadRestore
3. Herstellen van de SQL-database als een nieuwe database in een andere SQL-exemplaar in een andere SQL-VM - AlternateWorkloadRestore

Na het ophalen van de relevante herstelpunt (afzonderlijke of meld u point-in-time), gebruikt u [Get-AzRecoveryServicesBackupWorkloadRecoveryConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupWorkloadRecoveryConfig?view=azps-1.5.0) PS-cmdlet voor het ophalen van het object recovery config aan de hand van het gewenste herstelplan te gaan.

#### <a name="original-workload-restore"></a>Workload herstellen

Als u wilt overschrijven van de database waarvan een back-up is gemaakt met gegevens van het herstelpunt dat, net opgeven de juiste vlag en de relevante herstelpunt zoals wordt weergegeven in de volgende voorbeelden.

##### <a name="original-restore-with-distinct-recovery-point"></a>Met verschillende herstelpunt herstellen

````powershell
$OverwriteWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -OriginalWorkloadRestore -VaultId $targetVault.ID
````

##### <a name="original-restore-with-log-point-in-time"></a>Herstellen met log point-in-time

```powershell
$OverwriteWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -Item $bkpItem  -OriginalWorkloadRestore -VaultId $targetVault.ID
```

#### <a name="alternate-workload-restore"></a>Alternatieve werkbelasting herstellen

> [!IMPORTANT]
> Een back-ups van SQL-database kan worden hersteld als een nieuwe database naar een andere SQLInstance alleen in een Azure-VM die is geregistreerd bij deze kluis.

Als de die hierboven worden beschreven, als het doel SQLInstance binnen een andere Azure-VM ligt, zorgt ervoor dat deze [geregistreerd bij deze kluis](#registering-the-sql-vm) en de relevante SQLInstance wordt weergegeven als een beveiligbaar item.

````powershell
$TargetInstance = Get-AzRecoveryServicesBackupProtectableItem -WorkloadType MSSQL -ItemType SQLInstance -Name "<SQLInstance Name>" -ServerName "<SQL VM name>" -VaultId $targetVault.ID
````

Vervolgens geeft de relevante herstelpunt, doel-SQL-exemplaar met de juiste vlag hieronder weergegeven.

##### <a name="alternate-restore-with-distinct-recovery-point"></a>Alternatieve terugzetten met afzonderlijke herstelpunt

````powershell
$AnotherInstanceWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -TargetItem $TargetInstance -AlternateWorkloadRestore -VaultId $targetVault.ID
````

##### <a name="alternate-restore-with-log-point-in-time"></a>Alternatieve terugzetten met log point-in-time

```powershell
$AnotherInstanceWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -Item $bkpItem -AlternateWorkloadRestore -VaultId $targetVault.ID
```

Het uiteindelijke hersteldoel punt configuration-object verkregen uit [Get-AzRecoveryServicesBackupWorkloadRecoveryConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupWorkloadRecoveryConfig?view=azps-1.5.0) PS-cmdlet heeft alle relevante informatie om te herstellen en zoals hieronder wordt weergegeven.

````powershell
TargetServer         : <SQL server name>
TargetInstance       : <Target Instance name>
RestoredDBName       : <Target Instance name>/azurebackup1_restored_3_19_2019_1850
OverwriteWLIfpresent : No
NoRecoveryMode       : Disabled
targetPhysicalPath   : {azurebackup1, azurebackup1_log}
ContainerId          : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/vmappcontainer;compute;computeRG;SQLVMName
SourceResourceId     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/computeRG/VMAppContainer/SQLVMName
RestoreRequestType   : Alternate WL Restore
RecoveryPoint        : Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureWorkloadRecoveryPoint
PointInTime          : 1/1/0001 12:00:00 AM
````

U kunt de herstelde DB-naam, OverwriteWLIfpresent NoRecoveryMode en targetPhysicalPath velden bewerken. Meer informatie ophalen voor de doelbestandspaden zoals hieronder wordt weergegeven.

````powershell
$AnotherInstanceWithFullConfig.targetPhysicalPath

MappingType SourceLogicalName SourcePath                  TargetPath
----------- ----------------- ----------                  ----------
Data        azurebackup1      F:\Data\azurebackup1.mdf    F:\Data\azurebackup1_1553001753.mdf
Log         azurebackup1_log  F:\Log\azurebackup1_log.ldf F:\Log\azurebackup1_log_1553001753.ldf
````

De relevante PS-eigenschappen instellen als tekenreekswaarden zoals hieronder wordt weergegeven.

````powershell
$AnotherInstanceWithFullConfig.OverwriteWLIfpresent = "Yes"
$AnotherInstanceWithFullConfig | fl

TargetServer         : <SQL server name>
TargetInstance       : <Target Instance name>
RestoredDBName       : <Target Instance name>/azurebackup1_restored_3_19_2019_1850
OverwriteWLIfpresent : Yes
NoRecoveryMode       : Disabled
targetPhysicalPath   : {azurebackup1, azurebackup1_log}
ContainerId          : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/vmappcontainer;compute;computeRG;SQLVMName
SourceResourceId     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/computeRG/VMAppContainer/SQLVMName
RestoreRequestType   : Alternate WL Restore
RecoveryPoint        : Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureWorkloadRecoveryPoint
PointInTime          : 1/1/0001 12:00:00 AM
````

> [!IMPORTANT]
> Zorg ervoor dat het uiteindelijke hersteldoel AppConfig-object alle die nodig zijn en de juiste waarden heeft, omdat de herstelbewerking wordt gebaseerd op het AppConfig-object.

### <a name="restore-with-relevant-configuration"></a>Herstellen met de desbetreffende configuratie

Zodra de relevante recovery AppConfig-object is opgehaald en geverifieerd, gebruikt u de [terugzetten AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Restore-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS-cmdlet om het herstelproces te starten.

````powershell
Restore-AzRecoveryServicesBackupItem -WLRecoveryConfig $AnotherInstanceWithLogConfig -VaultId $targetVault.ID
````

De herstelbewerking retourneert een taak moet worden bijgehouden.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
MSSQLSERVER/m... Restore              InProgress           3/17/2019 10:02:45 AM                                3274xg2b-e4fg-5952-89b4-8cb566gc1748
````

## <a name="manage-sql-backups"></a>SQL-back-ups beheren

### <a name="on-demand-backup"></a>Back-up op aanvraag

Zodra de back-up is ingeschakeld voor een database, gebruiker ook een on-demand back-up voor het gebruik van de DB kunt activeren [back-up-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Backup-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS-cmdlet. Het volgende voorbeeld wordt een volledige back-up op een SQL-database met gebruik van compressie geactiveerd en de volledige back-up moet worden bewaard gedurende 60 dagen.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
$endDate = (Get-Date).AddDays(60).ToUniversalTime()
Backup-AzRecoveryServicesBackupItem -Item $bkpItem -BackupType Full -EnableCompression -VaultId $targetVault.ID -ExpiryDateTimeUTC $endDate
````

De ad-hoc back-opdracht retourneert een taak moet worden bijgehouden.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
MSSQLSERVER/m... Backup               InProgress           3/18/2019 8:41:27 PM                                2516bb1a-d3ef-4841-97a3-9ba455fb0637
````

Als de uitvoer verloren is of als u wilt ophalen van de desbetreffende taak-ID, [lijst van alle taken](#track-azure-backup-jobs) van Azure Backup-service en te volgen en de bijbehorende details.

### <a name="change-policy-for-backup-items"></a>Beleid voor back-upitems wijzigen

Gebruiker kan bestaande beleid wijzigen of wijzigen van het beleid van het item waarvan een back-up is gemaakt van Policy1 in Policy2. Als u wilt overschakelen van beleid voor een item waarvan een back-up is gemaakt, gewoon ophalen van het desbetreffende beleid en back-up item maken en gebruiken de [inschakelen AzRecoveryServices](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) opdracht met de back-upitem als de parameter.

````powershell
$TargetPol1 = Get-AzRecoveryServicesBackupProtectionPolicy -Name <PolicyName>
$anotherBkpItem = Get-AzRecoveryServicesBackupItem -WorkloadType MSSQL -BackupManagementType AzureWorkload -Name "<BackupItemName>"
Enable-AzRecoveryServicesBackupProtection -Item $anotherBkpItem -Policy $TargetPol1
````

De opdracht wordt er gewacht tot de configureren back-up is voltooid en de volgende uitvoer retourneert.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
master           ConfigureBackup      Completed            3/18/2019 8:00:21 PM      3/18/2019 8:02:16 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="stop-protection"></a>Beveiliging stoppen

#### <a name="retain-data"></a>Gegevens behouden

Als de gebruiker wil stop de beveiliging, kunnen ze gebruiken de [uitschakelen AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS-cmdlet. Hiermee stopt u de geplande back-ups maar de back omhoog totdat nu altijd wordt bewaard.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID
````

#### <a name="delete-backup-data"></a>back-upgegevens verwijderen

Om volledig verwijdert de opgeslagen back-upgegevens in de kluis, gewoon toevoegen '-'RemoveRecoveryPoints vlag/overschakelen naar de [opdracht Beveiliging uitschakelen](#retain-data).

````powershell
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID -RemoveRecoveryPoints
````

#### <a name="disable-auto-protection"></a>Automatische beveiliging uitschakelen

Als autoprotection is geconfigureerd op een SQLInstance, gebruiker kunt uitschakelen met behulp van de [uitschakelen AzRecoveryServicesBackupAutoProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupAutoProtection?view=azps-1.5.0) PS-cmdlet.

````powershell
$SQLInstance = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLInstance -VaultId $targetVault.ID -Name "<Protectable Item name>" -ServerName "<Server Name>"
Disable-AzRecoveryServicesBackupAutoProtection -InputItem $SQLInstance -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetvault.ID
````

#### <a name="unregister-sql-vm"></a>Registratie van de SQL-VM

Als alle databases van een SQL server [zijn niet langer beveiligd is en geen back-gegevens bestaan](#delete-backup-data), gebruiker kan registratie van de SQL-VM van deze kluis. Gebruiker kan databases vervolgens alleen beveiligen naar een andere kluis. Gebruik [Unregister-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/Unregister-AzRecoveryServicesBackupContainer?view=azps-1.5.0) PS-cmdlet om de registratie van de SQL-VM te.

````powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
 Unregister-AzRecoveryServicesBackupContainer -Container $SQLContainer -VaultId $targetvault.ID
````

### <a name="track-azure-backup-jobs"></a>Azure back-uptaken bijhouden

Het is belangrijk te weten dat Azure Backup alleen gebruikerstaken geactiveerd in SQL-back-up houdt. Geplande back-ups (met inbegrip van logboekback-ups) zijn niet zichtbaar in de portal/powershell. Als een geplande wordt echter taken mislukken, een [back-waarschuwing](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) wordt gegenereerd en weergegeven in de portal. [Azure Monitor gebruiken](backup-azure-monitoring-use-azuremonitor.md) om alle geplande taken en andere relevante informatie te volgen.

Gebruikers kunnen bijhouden ad-hoc/gebruiker geactiveerd bewerkingen met de taak-id die wordt geretourneerd in de [uitvoer](#on-demand-backup) van asynchrone taken zoals back-ups. Gebruik [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupJobDetails?view=azps-1.5.0) PS-cmdlet voor het bijhouden van de taak en de bijbehorende details.

````powershell
 Get-AzRecoveryServicesBackupJobDetails -JobId 2516bb1a-d3ef-4841-97a3-9ba455fb0637 -VaultId $targetVault.ID
````

Als u de lijst met ad-hoc taken en de status van Azure Backup-service, gebruikt [Get-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupJob?view=azps-1.5.0) PS-cmdlet. Het volgende voorbeeld retourneert de taken voor de SQL wordt uitgevoerd.

```powershell
Get-AzRecoveryServicesBackupJob -Status InProgress -BackupManagementType AzureWorkload
```

Als u wilt annuleren van een taak wordt uitgevoerd, gebruikt u de [Stop-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/Stop-AzRecoveryServicesBackupJob?view=azps-1.5.0) PS-cmdlet.

## <a name="managing-sql-always-on-availability-groups"></a>SQL Always On Availability groups beheren

Zorg ervoor dat u voor SQL Always On Availability Groups, [registreren van alle knooppunten](#registering-the-sql-vm) van de beschikbaarheidsgroep (AG). Zodra inschrijving is voltooid voor alle knooppunten, wordt een SQL-object voor beschikbaarheid van groep logisch onder beveiligbare items gemaakt. De databases onder de SQL-Beschikbaarheidsgroep worden vermeld als "SQLDatabase". De knooppunten wordt weergegeven als zelfstandige exemplaren en de standaard SQL-databases onder deze zullen worden weergegeven als de SQL-databases.

Bijvoorbeeld, gaan we ervan uit een SQL-Beschikbaarheidsgroep heeft twee knooppunten: 'sql-server-0' en 'sql-server-1' en 1 SQL AG-database. Nadat de beide deze knooppunten zijn geregistreerd, als gebruiker [bevat de beveiligbare items](#fetching-sql-dbs), geeft een lijst van de volgende onderdelen

1. Een SQL-AG-object - beveiligbare itemtype als SQLAvailabilityGroup
2. Een SQL-Beschikbaarheidsgroep DB - beveiligbaar itemtype als SQLDatabase
3. SQL-server-0 - beveiligbaar item type als SQLInstance
4. SQL-server-1 - beveiligbaar item type als SQLInstance
5. Een standaard SQL-databases (master, model, msdb) onder sql-server-0 - beveiligbaar item type als SQLDatabase
6. Een standaard SQL-databases (master, model, msdb) onder sql-server-1 - beveiligbaar item type als SQLDatabase

SQL-server-0, sql-server-1 wordt ook vermeld als "AzureVMAppContainer" wanneer [back-containers worden vermeld](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupContainer?view=azps-1.5.0).

Ophalen van alleen de relevante SQL-database naar [back-up inschakelen](#configuring-backup) en de [ad-hoc back-up](#on-demand-backup) en [herstellen PS-cmdlets](#restore-sql-dbs) identiek zijn.
