---
title: Implementeren en beheren van back-ups voor Azure-bestandsshares met behulp van PowerShell
description: PowerShell gebruiken om te implementeren en beheren van back-ups in Azure voor Azure-bestandsshares
services: backup
author: pvrk
manager: shivamg
keywords: PowerShell; Azure bestanden back-up. Azure Files herstellen;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: bbeccd03fffb699c95d52d50ec5c45e38b43ef51
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430395"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>PowerShell gebruiken om te back-up en herstellen van Azure-bestandsshares

Dit artikel leest hoe u back-up en herstellen van een Azure-bestandsshare vanaf een Recovery Services-kluis met Azure PowerShell-cmdlets. Een Recovery Services-kluis is een Azure Resource Manager-resource die wordt gebruikt voor het beveiligen van gegevens en assets in Azure Backup en Azure Site Recovery.

## <a name="concepts"></a>Concepten

Als u niet bekend met Azure Backup, voor een overzicht van de service bent, raadpleegt u [wat is Azure Backup?](backup-introduction-to-azure-backup.md). Voordat u begint, raadpleegt u de preview-mogelijkheden die worden gebruikt voor back-up van Azure-bestandsshares in [maakt u een Back-up van Azure-bestandsshares](backup-azure-files.md).

Voor het gebruik van PowerShell effectief, is het nodig om inzicht in de hiërarchie van objecten en waar u wilt beginnen uit.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Om weer te geven de **AzureRm.RecoveryServices.Backup** naslagdocumentatie voor PowerShell-cmdlet, Zie [Azure Backup - cmdlets van de Recovery Services](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in de Azure-bibliotheek.

## <a name="setup-and-registration"></a>Installatie en registratie

> [!NOTE]
> Zoals vermeld in [installeren van de Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.13.0), ondersteuning voor nieuwe functies in de module in November 2018 eindigt AzureRM. Er wordt ondersteuning geboden voor back-up van Azure-bestandsshares met de nieuwe Az PowerShell-module die is nu algemeen beschikbaar.

Volg deze stappen om te beginnen.

1. [Download de nieuwste versie van PowerShell Az](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0). De minimaal vereiste versie is 1.0.0.

2. Zoek de **Azure back-up PowerShell** cmdlets beschikbaar zijn met de volgende opdracht.

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    De aliassen en cmdlets voor back-up van Azure, Azure Site Recovery en de Recovery Services-kluis worden weergegeven. De volgende afbeelding is een voorbeeld van wat u ziet. Het is niet de volledige lijst met cmdlets.

    ![Lijst met Recovery Services-cmdlets](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Aanmelden bij uw Azure-account met behulp van **Connect AzAccount**. Deze cmdlet wordt een webpagina die u om de referenties van uw account vraagt:

    * U kunt ook uw accountreferenties opnemen als een parameter in de **Connect AzAccount** cmdlet met behulp van de **-referentie** parameter.
    * Als u een CSP-partner werken namens een tenant bent, geeft u de klant als een tenant met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Een voorbeeld is **Connect AzAccount-Tenant** fabrikam.com.

4. Koppelen van het abonnement dat u gebruiken met het account wilt, omdat een account kan meerdere abonnementen hebt.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Als u Azure Backup voor de eerste keer gebruikt, gebruikt u de **registreren AzResourceProvider** cmdlet voor het registreren van de Azure Recovery Services-provider met uw abonnement.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Controleer of de providers is geregistreerd met behulp van de volgende opdracht uit.
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    In de opdrachtuitvoer **RegistrationState** wordt gewijzigd in **geregistreerde**. Als u deze wijziging niet ziet, voert u de **registreren AzResourceProvider** cmdlet opnieuw uit.

De volgende taken kunnen worden geautomatiseerd met PowerShell:

* Maak een Recovery Services-kluis.
* Back-up voor Azure-bestandsshares configureren.
* Een back-uptaak activeert.
* Een back-uptaak bewaken.
* Herstel een Azure-bestandsshare.
* Een individuele Azure-bestand herstellen vanuit een Azure-bestandsshare.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Volg deze stappen voor het maken van een Recovery Services-kluis.

1. De Recovery Services-kluis is een Resource Manager-resource, dus u deze in de resourcegroep plaatsen moet. U kunt een bestaande resourcegroep gebruiken of kunt u een resourcegroep met de **New-AzResourceGroup** cmdlet. Wanneer u een resourcegroep maakt, geeft u de naam en locatie voor de resourcegroep.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Gebruik de **New-AzRecoveryServicesVault** cmdlet voor het maken van de Recovery Services-kluis. Geef dezelfde locatie voor de kluis dat werd gebruikt voor de resourcegroep.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geef het type opslagredundantie te gebruiken. U kunt [lokaal redundante opslag](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag](../storage/common/storage-redundancy-grs.md). Het volgende voorbeeld wordt de **- BackupStorageRedundancy** optie voor **testvault** ingesteld op **GeoRedundant**.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven

U kunt alle kluizen weergeven in het abonnement, met **Get-AzRecoveryServicesVault**.

```powershell
Get-AzRecoveryServicesVault
```

De uitvoer lijkt op die in het volgende voorbeeld. U ziet dat de bijbehorende **ResourceGroupName** en **locatie** worden geleverd.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

Voor veel Azure Backup-cmdlets is het object Recovery Services-kluis als invoer vereist.

Gebruik **Set AzRecoveryServicesVaultContext** om in te stellen van de context van de kluis. Nadat de context van de kluis is ingesteld, is dit van toepassing op alle navolgende cmdlets. Het volgende voorbeeld wordt de context van de kluis voor **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> We willen afschaffen van de instelling van de context kluis op basis van Azure PowerShell-richtlijnen. In plaats daarvan is het raadzaam dat gebruikers de kluis-ID zoals vermeld in de volgende instructies doorgeven.

U kunt ook opslaan of de ID van de kluis die u wilt een PowerShell-bewerking uitvoeren en deze doorgeven aan de relevante opdracht ophalen.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Back-up van een Azure-bestandsshare configureren

### <a name="create-a-protection-policy"></a>Een beveiligingsbeleid maken

Een back-protection-beleid is gekoppeld aan ten minste één beleid voor Gegevensretentie. Een bewaarbeleid wordt gedefinieerd hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd. Gebruik **Get-AzRecoveryServicesBackupRetentionPolicyObject** om het standaardretentiebeleid weer te geven. 

Op deze manier kunt u **Get-AzRecoveryServicesBackupSchedulePolicyObject** om op te halen van het standaardbeleid voor planning. De **New-AzRecoveryServicesBackupProtectionPolicy** met de cmdlet maakt een PowerShell-object dat back-upbeleid informatie bevat. De beleidsobjecten schema en de retentie worden gebruikt als invoer voor de **New-AzRecoveryServicesBackupProtectionPolicy** cmdlet. 

Het volgende voorbeeld wordt het beleid voor planning en het bewaarbeleid in variabelen. Het voorbeeld maakt gebruik van deze variabelen om de parameters te definiëren wanneer de **NewPolicy** protection-beleid is gemaakt.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

De uitvoer lijkt op die in het volgende voorbeeld.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

**NewAFSPolicy** neemt een dagelijkse back-up en worden deze 30 dagen bewaard.

### <a name="enable-protection"></a>Beveiliging inschakelen

Nadat u het beveiligingsbeleid hebt gedefinieerd, kunt u de beveiliging voor de Azure-bestandsshare met dit beleid kunt inschakelen.

Eerst ophalen van de relevante policy-object met de **Get-AzRecoveryServicesBackupProtectionPolicy** cmdlet. Deze cmdlet gebruiken om op te halen van een specifiek beleid of om de beleidsregels die zijn gekoppeld aan een type werkbelasting weer te geven.

Het volgende voorbeeld wordt een beleid voor het type werkbelasting **Azure Files**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

De uitvoer lijkt op die in het volgende voorbeeld.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> De tijdzone van de **BackupTime** veld in PowerShell is Coordinated Universal Time (UTC). Wanneer de back-uptijd wordt weergegeven in de Azure-portal, wordt de tijd wordt aangepast aan uw lokale tijdzone.
>
>

Het volgende beleid worden opgehaald van de back-upbeleid met de naam **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Gebruik **inschakelen AzRecoveryServicesBackupProtection** beveiliging van het item aan het opgegeven beleid in te schakelen. Nadat het beleid gekoppeld aan de kluis is, worden de back-werkstroom wordt geactiveerd op het moment dat is gedefinieerd in de planning van het beleid.

Het volgende voorbeeld wordt de beveiliging voor de Azure-bestandsshare **testAzureFileShare** onder het opslagaccount **testStorageAcct** aan het beleid **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

De opdracht wordt gewacht totdat de taak van de beveiliging configureren voltooid is en een vergelijkbare uitvoer biedt, zoals wordt weergegeven.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>Trigger een on-demand back-up

Gebruik **back-up-AzRecoveryServicesBackupItem** voor het activeren van een back-uptaak voor een beveiligde Azure-bestandsshare. Ophalen van de storage-account en de bestandsshare in deze met behulp van de volgende opdrachten en activeren van een on-demand back-up.

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

U kunt wijzigen van het beleid waarmee de Azure-bestandsshare is beveiligd met **inschakelen AzRecoveryServicesBackupProtection** met de relevante back-upitem en het nieuwe beveiligingsbeleid.

Het volgende voorbeeld van de wijzigingen de **testAzureFS** protection-beleid vanuit **dailyafs** naar **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-azure-files"></a>Herstellen van Azure-bestandsshares en Azure files

U kunt een volledige bestandsshare herstellen naar de oorspronkelijke locatie of een alternatieve locatie. Op deze manier kunnen afzonderlijke bestanden van de bestandsshare worden hersteld, te.

### <a name="fetch-recovery-points"></a>Herstelpunten ophalen

Gebruik de **Get-AzRecoveryServicesBackupRecoveryPoint** cmdlet om alle herstelpunten voor de back-upitem weer te geven. In het volgende script, de variabele **$rp** is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index **0**. Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt. In het voorbeeld **$rp [0]** het meest recente herstelpunt selecteert.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

De uitvoer lijkt op die in het volgende voorbeeld.

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

Nadat u de relevante herstelpunt is geselecteerd, herstellen van de bestandsshare of het bestand naar een alternatieve locatie of de oorspronkelijke locatie, zoals hier wordt beschreven.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Azure-bestandsshares naar een alternatieve locatie herstellen

#### <a name="restore-an-azure-file-share"></a>Herstellen van een Azure-bestandsshare

De alternatieve locatie identificeren door te geven van de volgende informatie:

* **TargetStorageAccountName**: Het opslagaccount waarnaar de inhoud van een back-up is hersteld. De doel-opslagaccount moet zich op dezelfde locatie als de kluis.
* **TargetFileShareName**: De bestandsshares in de doel-opslagaccount-account aan dat de inhoud van een back-up is hersteld.
* **TargetFolder**: De map onder de bestandsshare waarop gegevens worden teruggezet. Als de inhoud van de back-up is kunnen worden hersteld naar een hoofdmap, geeft u de waarden van de map doel als een lege tekenreeks.
* **ResolveConflict**: De instructie als er een conflict met de herstelde gegevens. Accepteert **overschrijven** of **overslaan**.

Geef deze parameters op voor de opdracht herstellen naar een bestandsshare van een back-up naar een alternatieve locatie herstellen.

````powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
````

De opdracht retourneert een taak met de ID moet worden bijgehouden, zoals wordt weergegeven in het volgende voorbeeld.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
````

#### <a name="restore-an-azure-file"></a>Herstel een Azure-bestand

Als u een afzonderlijk bestand in plaats van een volledige bestandsshare herstellen, unieke identificatie van de afzonderlijke bestanden door te geven van de volgende parameters:

* **TargetStorageAccountName**: Het opslagaccount waarnaar de inhoud van een back-up is hersteld. De doel-opslagaccount moet zich op dezelfde locatie als de kluis.
* **TargetFileShareName**: De bestandsshares in de doel-opslagaccount-account aan dat de inhoud van een back-up is hersteld.
* **TargetFolder**: De map onder de bestandsshare waarop gegevens worden teruggezet. Als de inhoud van de back-up is kunnen worden hersteld naar een hoofdmap, geeft u de waarden van de map doel als een lege tekenreeks.
* **SourceFilePath**: Het absolute pad van het bestand, kunnen worden hersteld in de bestandsshare, als een tekenreeks. Dit pad is hetzelfde pad gebruikt in de **Get-AzStorageFile** PowerShell-cmdlet.
* **SourceFileType**: Of een map of een bestand is geselecteerd. Accepteert **Directory** of **bestand**.
* **ResolveConflict**: De instructie als er een conflict met de herstelde gegevens. Accepteert **overschrijven** of **overslaan**.

De aanvullende parameters gerelateerd zijn alleen voor de afzonderlijke bestand dat moet worden hersteld.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Met deze opdracht retourneert ook een taak met de ID moet worden bijgehouden, zoals eerder is getoond.

### <a name="restore-azure-file-shares-to-the-original-location"></a>Azure-bestandsshares naar de oorspronkelijke locatie herstellen

Wanneer u naar een oorspronkelijke locatie herstelt, wordt alle parameters van de doel - en doel-gerelateerde hoeft te worden opgegeven. Alleen **ResolveConflict** moet worden opgegeven.

#### <a name="overwrite-an-azure-file-share"></a>Een Azure-bestandsshare overschrijven

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Een Azure-bestand overschrijven

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Bijhouden van back-up en herstellen van taken

On-demand back-up en herstel bewerkingen retourneren een taak, samen met een ID, zoals wordt weergegeven in de vorige sectie ["Activeren een on-demand back-up."](#trigger-an-on-demand-backup) Gebruik de **Get-AzRecoveryServicesBackupJobDetails** cmdlet voor het bijhouden van de voortgang van de taak en ophalen van meer details.

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
