---
title: Implementeren en beheren van back-ups voor Azure-bestandsshares met behulp van PowerShell
description: PowerShell gebruiken om te implementeren en beheren van back-ups in Azure voor Azure-bestandsshares
services: backup
author: pvrk
manager: shivamg
keywords: PowersShell; Back-up van Azure-bestanden. Herstel van de Azure-bestanden.
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 30fc36f29a7602e2bc3f192b445474bfc50e9434
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632632"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>PowerShell gebruiken om te back-up en herstellen van Azure-bestandsshares

Dit artikel leest hoe u back-up en herstellen van een Azure-bestandsshare vanaf een Recovery Services-kluis met Azure PowerShell-cmdlets. Een Recovery Services-kluis is een Azure Resource Manager-resource die is gebruikt voor het beveiligen van gegevens en assets in Azure Backup en Azure Site Recovery-services.

## <a name="concepts"></a>Concepten

Als u niet bekend met de Azure Backup-service, voor een overzicht van de service bent, Zie het artikel [wat is Azure Backup?](backup-introduction-to-azure-backup.md). Voordat u begint, zorg ervoor dat u Let op de previewmogelijkheden van het back-ups van Azure-bestandsshares beschreven [hier](backup-azure-files.md).

Voor het gebruik van PowerShell effectief, is het nodig om inzicht in de hiërarchie van objecten en van waar u wilt beginnen.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Voor de naslaginformatie over AzureRm.RecoveryServices.Backup PowerShell-cmdlets, raadpleegt u de [Azure Backup - Recovery Services-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in de Azure-bibliotheek.

## <a name="setup-and-registration"></a>Installatie en registratie

> [!NOTE]
> Zoals opgemerkt [hier](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.13.0), ondersteuning voor nieuwe functies in AzureRM-module eindigt in november 2018. Daarom bieden we ondersteuning voor back-up van Azure-bestandsshares met de nieuwe 'Az' PS-module nu beschikbaar in de algemene beschikbaarheid.

Om te beginnen met:

1. [Download de nieuwste versie van PowerShell 'Az'](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0) (de minimaal vereiste versie is: 1.0.0)

2. De beschikbare Azure Backup PowerShell-cmdlets vinden door de volgende opdracht te typen:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    De aliassen en cmdlets voor back-up van Azure, Azure Site Recovery en de Recovery Services-kluis worden weergegeven. De volgende afbeelding is een voorbeeld van wat u ziet. Het is niet de volledige lijst met cmdlets.

    ![lijst met Recovery Services](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Aanmelden bij uw Azure-account met **Connect AzAccount**. Deze cmdlet wordt een webpagina vraagt u om referenties voor uw account:

    * U kunt ook uw accountreferenties opnemen als een parameter in de **Connect AzAccount** cmdlet, met behulp van de **-referentie** parameter.
    * Als u de CSP-partner werken namens een tenant bent, geeft u de klant als een tenant met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Bijvoorbeeld: **Verbinding maken met AzAccount-Tenant "fabrikam.com"**

4. Koppel het abonnement dat u gebruiken met het account wilt, omdat een account kan meerdere abonnementen hebben:

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Als u Azure Backup voor de eerste keer gebruikt, moet u de **registreren AzResourceProvider** cmdlet voor het registreren van de Azure Recovery Service-provider met uw abonnement.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. U kunt controleren of de Providers is geregistreerd met de volgende opdrachten:
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    In de uitvoer van de opdracht, de **RegistrationState** moet wijzigen naar **geregistreerde**. Als niet het geval is, alleen wordt uitgevoerd de **registreren AzResourceProvider** cmdlet opnieuw uit.

De volgende taken kunnen worden geautomatiseerd met PowerShell:

* Een Recovery Services-kluis maken
* Back-up voor Azure-bestandsshares configureren
* Een back-uptaak activeert
* Een back-uptaak controleren
* Herstellen van een Azure-bestandsshare
* Een individuele Azure-bestand herstellen vanuit een Azure-bestandsshare

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

De volgende stappen leiden u bij het maken van een Recovery Services-kluis.

1. De Recovery Services-kluis is een Resource Manager-resource, dus u moet dit binnen een resourcegroep te plaatsen. U kunt een bestaande resourcegroep gebruiken, of maak een resourcegroep met de **New-AzResourceGroup** cmdlet. Bij het maken van een resourcegroep, geef de naam en locatie voor de resourcegroep.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Gebruik de **New-AzRecoveryServicesVault** cmdlet voor het maken van de Recovery Services-kluis. Zorg ervoor dat de dezelfde locatie voor de kluis opgeven dat werd gebruikt voor de resourcegroep.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geef het type opslagredundantie moet gebruiken. u kunt [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). Het volgende voorbeeld ziet dat de optie - BackupStorageRedundancy voor testvault is ingesteld op GeoRedundant.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven

U kunt alle kluizen weergeven in het abonnement, met **Get-AzRecoveryServicesVault**:

```powershell
Get-AzRecoveryServicesVault
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, ziet u de dat locatie en de bijbehorende ResourceGroupName worden geleverd.

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

Gebruik **Set AzRecoveryServicesVaultContext** om in te stellen van de context van de kluis. Zodra deze is ingesteld, is deze op alle navolgende cmdlets van toepassing. Het volgende voorbeeld wordt de context van de kluis voor de kluis *testvault*.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Wij van plan bent de instelling van de context van de kluis aan de hand van Azure PowerShell-richtlijnen afschaffen. In plaats daarvan wordt aangeraden gebruikers om door te geven van de kluis-ID zoals hieronder wordt beschreven

U kunt ook u kunt store/ophalen op de ID van de kluis die u wilt een PowerShell-bewerking uitvoeren en deze doorgeven aan de relevante opdracht.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Back-up van een Azure-bestandsshare configureren

### <a name="create-protection-policy"></a>Beveiligingsbeleid maken

Een back-protection-beleid is gekoppeld aan ten minste één beleid voor Gegevensretentie. Bewaarbeleid wordt gedefinieerd hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd. Gebruik **Get-AzRecoveryServicesBackupRetentionPolicyObject** om het standaardretentiebeleid weer te geven.  Op deze manier kunt u **Get-AzRecoveryServicesBackupSchedulePolicyObject** om op te halen van het standaardbeleid voor planning. De **New-AzRecoveryServicesBackupProtectionPolicy** met de cmdlet maakt een PowerShell-object dat back-upbeleid informatie bevat. De beleidsobjecten schema en de retentie worden gebruikt als invoer voor de **New-AzRecoveryServicesBackupProtectionPolicy** cmdlet. Het volgende voorbeeld wordt het beleid voor planning en het bewaarbeleid in variabelen. Het voorbeeld deze variabelen worden gebruikt voor het definiëren van de parameters bij het maken van een beveiligingsbeleid *NewPolicy*.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

De uitvoer lijkt op die in het volgende voorbeeld:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

De 'NewAFSPolicy' neemt een dagelijkse back-up en behoudt deze gedurende 30 dagen.

### <a name="enable-protection"></a>Beveiliging inschakelen

Nadat u het beveiligingsbeleid hebt gedefinieerd, kunt u de beveiliging voor de Azure-bestandsshare met dit beleid inschakelen.

Eerst ophalen van de relevante policy-object met de **Get-AzRecoveryServicesBackupProtectionPolicy** cmdlet. U kunt deze cmdlet gebruiken om op te halen van een specifiek beleid, of om de beleidsregels die zijn gekoppeld aan een type werkbelasting weer te geven.

Het volgende voorbeeld wordt een beleid voor het type werkbelasting, Azure Files.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

De uitvoer lijkt op die in het volgende voorbeeld:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> De tijdzone van het veld BackupTime in PowerShell is UTC. Echter, wanneer de back-uptijd wordt weergegeven in de Azure-portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

Het volgende beleid worden opgehaald van de back-upbeleid met de naam als "dailyafs"

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Gebruik **inschakelen AzRecoveryServicesBackupProtection** beveiliging van het item aan het opgegeven beleid in te schakelen. Nadat het beleid gekoppeld aan de kluis is, worden de back-werkstroom wordt geactiveerd op het moment dat is gedefinieerd in de planning van het beleid.

Het volgende voorbeeld wordt de beveiliging voor de Azure-bestandsshare, "testAzureFileShare", onder de storage-account "testStorageAcct" met de dailyafs beleid.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

De opdracht wacht totdat de taak van de beveiliging configureren is voltooid en een vergelijkbare uitvoer biedt zoals hieronder wordt weergegeven.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>Trigger een on-demand back-up

Gebruik **back-up-AzRecoveryServicesBackupItem** voor het activeren van een back-uptaak voor een beveiligde Azure-bestandsshare. Haal de storage-account en de bestandsshare in met behulp van de volgende opdrachten en activeren van een on-demand back-up.

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

De opdracht retourneert een taak met de ID zoals in het volgende voorbeeld worden bijgehouden.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

We zijn gebruik te maken van momentopnamen van Azure-bestandsshares bij het maken van back-ups en kan daarom meestal de taak is voltooid op het moment dat de opdracht deze uitvoer retourneert

### <a name="modify-protection-policy"></a>Beveiligingsbeleid wijzigen

Als u wilt wijzigen van het beleid met de Azure-bestandsshare is beveiligd, gebruikt u de **inschakelen AzRecoveryServicesBackupProtection** met de relevante back-upitem en het nieuwe beveiligingsbeleid.

De volgende voorbeelden wordt het beveiligingsbeleid van 'testAzureFS' uit 'dailyafs' naar 'monthlyafs' gewijzigd

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-sharesazure-files"></a>Herstellen van Azure file share (s) / Azure bestanden

U kunt een volledige bestandsshare herstellen naar de oorspronkelijke of een alternatieve locatie. Op deze manier kunnen afzonderlijke bestanden van de bestandsshare ook worden hersteld.

### <a name="fetching-recovery-points"></a>Herstelpunten ophalen

Gebruik de **Get-AzRecoveryServicesBackupRecoveryPoint** cmdlet om alle herstelpunten voor de back-upitem weer te geven. In het volgende script, de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index 0. Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt. In het voorbeeld selecteert $rp [0] het meest recente herstelpunt.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

De uitvoer lijkt op die in het volgende voorbeeld:

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

Nadat het relevante herstelpunt dat is geselecteerd, gaat u verder met de bestandsshare of het bestand naar een alternatieve/oorspronkelijke locatie herstellen, zoals hieronder wordt uitgelegd.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Azure-bestandsshares naar een alternatieve locatie herstellen

#### <a name="restoring-an-azure-file-share"></a>Herstellen van een Azure-bestandsshare

De alternatieve locatie identificeren door te geven van de volgende informatie:

* ***TargetStorageAccountName***: Storage-account waarnaar de back-ups van inhoud is hersteld. De doel-opslagaccount moet zich op dezelfde locatie als de kluis.
* ***TargetFileShareName***: Bestandsshares in het doelopslagaccount waarnaar de back-ups van inhoud is hersteld
* ***TargetFolder***: De map onder de bestandsshare waarop gegevens worden teruggezet. Als de back-ups van inhoud moet worden hersteld naar de hoofdmap, geeft u de waarden van de map doel als lege tekenreeks
* ***ResolveConflict***: De instructie als er een conflict met de herstelde gegevens. Accepteert 'Overschrijven' of 'overslaan'

Geef deze parameters op voor de opdracht restore om terug te zetten van een back-ups van de bestandsshare naar een alternatieve locatie.

````powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
````

De opdracht retourneert een taak met de ID moet worden bijgehouden, zoals wordt weergegeven in het volgende voorbeeld.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
````

#### <a name="restoring-an-azure-file"></a>Herstellen van een Azure-bestand

Voor het geval u herstellen van een afzonderlijk bestand in plaats van een volledige bestandsshare wilt, moet het afzonderlijke bestand uniek worden aangeduid met het leveren van de volgende parameters.

* ***TargetStorageAccountName***: Storage-account waarnaar de back-ups van inhoud is hersteld. De doel-opslagaccount moet zich op dezelfde locatie als de kluis.
* ***TargetFileShareName***: Bestandsshares in het doelopslagaccount waarnaar de back-ups van inhoud is hersteld
* ***TargetFolder***: De map onder de bestandsshare waarop gegevens worden teruggezet. Als de back-ups van inhoud moet worden hersteld naar de hoofdmap, geeft u de waarden van de map doel als lege tekenreeks
* ***SourceFilePath***: Het absolute pad van het bestand, kunnen worden hersteld in de bestandsshare, als een tekenreeks. Dit is de hetzelfde pad dat wordt gebruikt ```Get-AzStorageFile``` PS-cmdlet.
* ***SourceFileType***: Of een map of een bestand is geselecteerd. Accepteert 'Directory' of 'File'
* ***ResolveConflict***: De instructie als er een conflict met de herstelde gegevens. Accepteert 'Overschrijven' of 'overslaan'

Zoals u ziet, wordt de aanvullende parameters hebben alleen betrekking op de afzonderlijke bestanden kunnen worden hersteld.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Een taak met de ID worden bijgehouden, zoals hierboven wordt ook geretourneerd.

### <a name="restore-azure-file-shares-to-original-location"></a>Azure-bestandsshares op de oorspronkelijke locatie herstellen

In geval van de oorspronkelijke locatie herstellen gerelateerde alle bestemming parameters niet moeten worden opgegeven. Alleen ```ResolveConflict``` moet worden opgegeven

#### <a name="overwrite-an-azure-file-share"></a>Een Azure-bestandsshare overschrijven

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Een Azure-bestand overschrijven

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Bijhouden van back-up en herstellen van taken

On-demand back-up en herstel bewerkingen retourneren een taak, samen met een ID zoals [hierboven](#trigger-an-on-demand-backup). Gebruik de ```Get-AzRecoveryServicesBackupJobDetails``` cmdlet voor het bijhouden van de voortgang van de taak en ophalen van meer details.

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
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support
```
