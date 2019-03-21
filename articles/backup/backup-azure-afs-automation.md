---
title: Back-up en herstellen van Azure Files met Azure Backup en PowerShell
description: Back-up en herstellen van Azure Files met Azure Backup en PowerShell.
author: pvrk
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: pullabhk
ms.openlocfilehash: 83fe8d17699c19d442fd734d71d828eb9fd9d6ed
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258357"
---
# <a name="back-up-and-restore-azure-files-with-powershell"></a>Back-up en herstellen van Azure Files met PowerShell

In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken voor back-up en herstellen van een Azure Files bestand delen met een [Azure Backup](backup-overview.md) Recovery Services-kluis. 

In deze zelfstudie wordt het volgende uitgelegd:

> [!div class="checklist"]
> * PowerShell instellen en registreren van de Azure Recovery Services-Provider.
> * Maak een Recovery Services-kluis.
> * Back-up van een Azure-bestandsshare configureren.
> * Een back-uptaak uitvoeren.
> * Een back-ups van Azure-bestandsshare of een afzonderlijk bestand van een share herstellen.
> * Back-ups controleren en herstellen van taken.


## <a name="before-you-start"></a>Voordat u begint

- [Meer informatie](backup-azure-recovery-services-vault-overview.md) over Recovery Services-kluizen.
- Meer informatie over de preview-mogelijkheden voor [back-ups van Azure-bestandsshares](backup-azure-files.md).
- Bekijk de PowerShell-objecthiërarchie voor Recovery Services.


## <a name="recovery-services-object-hierarchy"></a>Recovery Services-objecthiërarchie

De objecthiërarchie worden samengevat in het volgende diagram.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Controleer de **Az.RecoveryServices** [cmdlet-verwijzing](/powershell/module/az.recoveryservices) verwijzing in de Azure-bibliotheek.


## <a name="set-up-and-install"></a>Instellen en installeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell instellen als volgt:

1. [Download de nieuwste versie van PowerShell Az](/powershell/azure/install-az-ps). De minimaal vereiste versie is 1.0.0.

2. Zoek de Azure Backup PowerShell-cmdlets met de volgende opdracht:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
3. Bekijk de aliassen en cmdlets voor back-up van Azure, Azure Site Recovery en de Recovery Services-kluis worden weergegeven. Hier volgt een voorbeeld van wat wordt weergegeven. Het is niet een volledige lijst met cmdlets.

    ![Lijst met Recovery Services-cmdlets](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Meld u aan bij uw Azure-account met **Connect AzAccount**.
4. Op de webpagina die wordt weergegeven, wordt u gevraagd voor het invoeren van referenties voor uw account.

    - U kunt ook uw accountreferenties opnemen als een parameter in de **Connect AzAccount** cmdlet met **-referentie**.
    - Als u een CSP-partner werken namens een tenant bent, geeft u de klant als een tenant, met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Een voorbeeld is **Connect AzAccount-Tenant** fabrikam.com.

4. Koppelen van het abonnement dat u gebruiken met het account, wilt omdat een account kan meerdere abonnementen hebt.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. Als u Azure Backup voor de eerste keer gebruikt, gebruikt u de **registreren AzResourceProvider** cmdlet voor het registreren van de Azure Recovery Services-provider met uw abonnement.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Controleer of de providers is geregistreerd:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
7. Controleer in de opdrachtuitvoer **RegistrationState** wordt gewijzigd in **geregistreerde**. Als dat niet zo is, wordt uitgevoerd de **registreren AzResourceProvider** cmdlet opnieuw uit.



## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Volg deze stappen voor het maken van een Recovery Services-kluis.

- De Recovery Services-kluis is een Resource Manager-resource, dus u deze in de resourcegroep plaatsen moet. U kunt een bestaande resourcegroep gebruiken of kunt u een resourcegroep met de **New-AzResourceGroup** cmdlet. Wanneer u een resourcegroep maakt, geeft u de naam en locatie voor de resourcegroep. 

1. Een kluis is in een resourcegroep geplaatst. Als u geen een bestaande resource groep, maakt u een nieuw bestand met de [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). In dit voorbeeld maken we een nieuwe resourcegroep in de regio VS-West.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```
2. Gebruik de [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) cmdlet voor het maken van de kluis. Geef dezelfde locatie voor de kluis dat werd gebruikt voor de resourcegroep.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geef het type redundantie om te gebruiken voor de opslag van de kluis.

   - U kunt [lokaal redundante opslag](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag](../storage/common/storage-redundancy-grs.md).
   - Het volgende voorbeeld wordt de **- BackupStorageRedundancy** optie voor de[Set AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperties?view=azps-1.4.0) cmd voor **testvault** ingesteld op  **GeoRedundant**.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven

U kunt alle kluizen weergeven in het abonnement, met [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

De uitvoer is vergelijkbaar met het volgende. Houd er rekening mee dat de bijbehorende resourcegroep en locatie zijn opgegeven.

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

- Veel Azure Backup-cmdlets vereist het object Recovery Services-kluis als invoer, dus is het handig om op te slaan van het object kluis in een variabele.
- De context van de kluis is het type gegevens dat in de kluis wordt beveiligd. Stelt u deze met [Set AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Nadat de context is ingesteld, is dit van toepassing op alle navolgende cmdlets.


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

- Een back-upbeleid is gekoppeld aan ten minste één beleid voor Gegevensretentie. Een bewaarbeleid wordt gedefinieerd hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd.
- Het standaard back-upbeleid bewaren met gebruik van [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
- Het standaard back-upbeleid schema met behulp van weergave [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
-  U gebruikt de [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet voor het maken van een nieuwe back-upbeleid. Ingevoerde de beleidsobjecten schema en de retentie.

Het volgende voorbeeld wordt het beleid voor planning en het bewaarbeleid in variabelen. Vervolgens wordt deze variabele als parameters voor een nieuw beleid (**NewAFSPolicy**). **NewAFSPolicy** neemt een dagelijkse back-up en worden deze 30 dagen bewaard.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

De uitvoer is vergelijkbaar met het volgende.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```



## <a name="enable-backup"></a>Back-up inschakelen

Nadat u de back-upbeleid definieert, kunt u de beveiliging voor de Azure-bestandsshare met behulp van het beleid inschakelen.

### <a name="retrieve-a-backup-policy"></a>Een back-upbeleid ophalen

Ophalen van de relevante policy-object met [Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0). Deze cmdlet gebruiken om op te halen van een specifiek beleid, of om de beleidsregels die zijn gekoppeld aan een type werkbelasting weer te geven.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Ophalen van een beleid voor een type werkbelasting

Het volgende voorbeeld wordt een beleid voor het type werkbelasting **Azure Files**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

De uitvoer is vergelijkbaar met het volgende.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```
> [!NOTE]
> De tijdzone van de **BackupTime** veld in PowerShell is Coordinated Universal Time (UTC). Wanneer de back-uptijd wordt weergegeven in de Azure-portal, wordt de tijd wordt aangepast aan uw lokale tijdzone.

### <a name="retrieve-a-specific-policy"></a>Een specifiek beleid ophalen

Het volgende beleid worden opgehaald van de back-upbeleid met de naam **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Back-up inschakelen en beleid toepassen

Schakel de beveiliging met [inschakelen AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Nadat het beleid gekoppeld aan de kluis is, worden back-ups worden geactiveerd volgens een planning voor het beleid.

Het volgende voorbeeld wordt de beveiliging voor de Azure-bestandsshare **testAzureFileShare** in storage-account **testStorageAcct**, met het beleid **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

De opdracht wordt gewacht totdat de taak van de beveiliging configureren voltooid is en een vergelijkbare uitvoer biedt, zoals wordt weergegeven.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="trigger-an-on-demand-backup"></a>Trigger een on-demand back-up

Gebruik [back-up-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) om uit te voeren van een on-demand back-up voor een beveiligde Azure-bestandsshare.

1. Het storage-account en het bestand delen van de container in de kluis waarin de back-upgegevens met ophalen [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer).
2. Voor het starten van een back-uptaak u informatie verkrijgen over de virtuele machine met [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem).
3. Uitvoeren van een on-demand back-up met[back-up-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem).

Voer de on-demand back-up als volgt uit:
    
```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

De opdracht retourneert een taak met de ID moet worden bijgehouden, zoals wordt weergegeven in het volgende voorbeeld.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Momentopnamen van Azure-bestandsshares worden gebruikt tijdens de back-ups worden genomen, dus meestal de taak is voltooid op het moment dat de opdracht deze uitvoer retourneert.

### <a name="modify-the-protection-policy"></a>Het beveiligingsbeleid wijzigen

U kunt wijzigen van het beleid dat wordt gebruikt voor back-ups van de Azure-bestandsshare met [inschakelen AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Geef de relevante back-upitem en de nieuwe back-upbeleid.

Het volgende voorbeeld van de wijzigingen de **testAzureFS** protection-beleid vanuit **dailyafs** naar **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-files"></a>Azure-bestandsshares en bestanden terugzetten

U kunt een volledige bestandsshare of specifieke bestanden op de share herstellen. U kunt naar de oorspronkelijke locatie of naar een alternatieve locatie herstellen. 

### <a name="fetch-recovery-points"></a>Herstelpunten ophalen

Gebruik [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) om alle herstelpunten voor de back-ups van item weer te geven.

In het volgende script:

- De variabele **$rp** is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen.
- De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index **0**.
- Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt.
- In het voorbeeld **$rp [0]** het meest recente herstelpunt selecteert.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

De uitvoer is vergelijkbaar met het volgende.

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```
Nadat u de relevante herstelpunt is geselecteerd, terugzetten u de bestandsshare of het bestand naar de oorspronkelijke locatie of naar een alternatieve locatie.

### <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Een Azure-bestandsshare naar een alternatieve locatie herstellen

Gebruik de [terugzetten AzRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) om naar het geselecteerde herstelpunt te herstellen. Deze parameters voor het identificeren van de alternatieve locatie opgeven: 

- **TargetStorageAccountName**: Het opslagaccount waarnaar de inhoud van een back-up is hersteld. De doel-opslagaccount moet zich op dezelfde locatie als de kluis.
- **TargetFileShareName**: De bestandsshares in de doel-opslagaccount-account aan dat de inhoud van een back-up is hersteld.
- **TargetFolder**: De map onder de bestandsshare waarop gegevens worden teruggezet. Als de inhoud van de back-up is kunnen worden hersteld naar een hoofdmap, geeft u de waarden van de map doel als een lege tekenreeks.
- **ResolveConflict**: De instructie als er een conflict met de herstelde gegevens. Accepteert **overschrijven** of **overslaan**.

Voer de cmdlet met de parameters als volgt uit:

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

De opdracht retourneert een taak met de ID moet worden bijgehouden, zoals wordt weergegeven in het volgende voorbeeld.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

### <a name="restore-an-azure-file-to-an-alternate-location"></a>Een Azure-bestand naar een alternatieve locatie herstellen

Gebruik de [terugzetten AzRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) om naar het geselecteerde herstelpunt te herstellen. Deze parameters voor het identificeren van de alternatieve locatie opgeven en als unieke identificatie van het bestand dat u wilt herstellen.

* **TargetStorageAccountName**: Het opslagaccount waarnaar de inhoud van een back-up is hersteld. De doel-opslagaccount moet zich op dezelfde locatie als de kluis.
* **TargetFileShareName**: De bestandsshares in de doel-opslagaccount-account aan dat de inhoud van een back-up is hersteld.
* **TargetFolder**: De map onder de bestandsshare waarop gegevens worden teruggezet. Als de inhoud van de back-up is kunnen worden hersteld naar een hoofdmap, geeft u de waarden van de map doel als een lege tekenreeks.
* **SourceFilePath**: Het absolute pad van het bestand, kunnen worden hersteld in de bestandsshare, als een tekenreeks. Dit pad is hetzelfde pad gebruikt in de **Get-AzStorageFile** PowerShell-cmdlet.
* **SourceFileType**: Of een map of een bestand is geselecteerd. Accepteert **Directory** of **bestand**.
* **ResolveConflict**: De instructie als er een conflict met de herstelde gegevens. Accepteert **overschrijven** of **overslaan**.

De aanvullende parameters (SourceFilePath en SourceFileType) hebben alleen naar de afzonderlijke bestand dat u wilt herstellen.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Met deze opdracht retourneert een taak met de ID moet worden bijgehouden, zoals wordt weergegeven in de vorige sectie.

### <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Azure-bestandsshares en bestanden naar de oorspronkelijke locatie herstellen

Wanneer u naar een oorspronkelijke locatie herstelt, moet u geen doel - en doel-gerelateerde parameters opgeven. Alleen **ResolveConflict** moet worden opgegeven.

#### <a name="overwrite-an-azure-file-share"></a>Een Azure-bestandsshare overschrijven

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Een Azure-bestand overschrijven

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Bijhouden van back-up en herstellen van taken

On-demand back-up en herstel bewerkingen retourneren een taak, samen met een ID, zoals wordt weergegeven wanneer u [uitgevoerd van een on-demand back-up](#trigger-an-on-demand-backup). Gebruik de [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0) cmdlet voor het bijhouden van de taak wordt uitgevoerd en de details.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```
## <a name="next-steps"></a>Volgende stappen
[Meer informatie over](backup-azure-files.md) back-ups van Azure Files in Azure portal.
