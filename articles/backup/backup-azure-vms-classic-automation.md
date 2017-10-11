---
title: Implementeren en beheren van back-up voor virtuele Azure-machines met behulp van PowerShell | Microsoft Docs
description: Informatie over het implementeren en beheren van Azure Backup met behulp van PowerShell.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;trinadhk;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f0f06adb8177ce2d17aa0b40666470279c04e22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-azurermbackup-cmdlets-to-back-up-virtual-machines"></a>AzureRM.Backup-cmdlets gebruiken om back-up van virtuele machines
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Klassiek](backup-azure-vms-classic-automation.md)
>
>

Dit artikel laat zien hoe u Azure PowerShell gebruikt voor back-up en herstel van virtuele Azure-machines. In Azure zijn twee verschillende implementatiemodellen beschikbaar voor het maken van en werken met resources: Resource Manager en het klassieke model. In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel voor back-ups naar een back-upkluis. Als u niet een back-upkluis in uw abonnement hebt gemaakt, raadpleegt u de Resource Manager-versie van dit artikel [gebruik AzureRM.RecoveryServices.Backup-cmdlets voor back-up van virtuele machines](backup-azure-vms-automation.md). U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

> [!IMPORTANT]
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 15 oktober 2017 kunt u PowerShell niet meer gebruiken voor het maken van back-upkluizen. **Per 1 november 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

## <a name="concepts"></a>Concepten
In dit artikel bevat informatie die specifiek zijn voor de PowerShell-cmdlets gebruikt voor back-up van virtuele machines. Raadpleeg voor inleidende informatie over het beveiligen van Azure Virtual machines [plannen van uw back-infrastructuur van de virtuele machine in Azure](backup-azure-vms-introduction.md).

> [!NOTE]
> Lees voordat u begint, de [vereisten](backup-azure-vms-prepare.md) vereist voor het werken met Azure Backup en de [beperkingen](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) van de huidige VM-back-upoplossing.
>
>

PowerShell als effectief wilt gebruiken, neem even de tijd om te begrijpen van de hiërarchie van objecten en van waar u wilt beginnen.

![Objecthiërarchie](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

De twee belangrijkste stromen u beveiliging inschakelt voor een virtuele machine en herstellen van gegevens vanaf een herstelpunt. De focus van dit artikel is om te helpen u bijzonder geschikt voor het werken met de PowerShell-cmdlets voor het inschakelen van deze twee scenario's.

## <a name="setup-and-registration"></a>De installatie en registratie
Om te beginnen met:

1. [Download de meest recente PowerShell](https://github.com/Azure/azure-powershell/releases) (minimaal vereiste versie is: 1.0.0)
2. De beschikbare Azure back-up PowerShell-cmdlets vinden door de volgende opdracht te typen:

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

De volgende taken voor de installatie en registratie kunnen worden geautomatiseerd met PowerShell:

* Een back-upkluis maken
* Registreren van de virtuele machines met de Azure Backup-service

### <a name="create-a-backup-vault"></a>Een back-upkluis maken
> [!WARNING]
> Voor klanten met Azure Backup voor de eerste keer, moet u de Azure Backup-provider die moet worden gebruikt met uw abonnement te registreren. Dit kan worden gedaan door de volgende opdracht uit te voeren: Register AzureRmResourceProvider - ProviderNamespace 'Microsoft.Backup'
>
>

U kunt maken met een nieuwe back-upkluis met de **nieuw AzureRmBackupVault** cmdlet. De back-upkluis is een ARM-bron, dus u moet deze binnen een resourcegroep te plaatsen. Voer de volgende opdrachten in een verhoogde Azure PowerShell-console:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

U krijgt een lijst met alle back-upkluizen in een bepaald abonnement met de **Get-AzureRmBackupVault** cmdlet.

> [!NOTE]
> Is het handig voor het opslaan van het back-upkluis-object in een variabele. Het object van de kluis is nodig als invoer voor veel Azure Backup-cmdlets.
>
>

### <a name="registering-the-vms"></a>Registreren van de virtuele machines
De eerste stap naar de back-up configureren met Azure Backup is uw computer of virtuele machine met een Azure Backup-kluis registreren. De **registreren AzureRmBackupContainer** cmdlet neemt de invoergegevens van een virtuele machine van Azure IaaS en registreert deze bij de opgegeven kluis. De registerbewerking wordt gekoppeld aan virtuele machine van Azure met de back-upkluis en volgt de virtuele machine via de levenscyclus van de back-up.

Registreren van uw virtuele machine met de Azure Backup-service, maakt een site op het hoogste containerobject. Een container bevat doorgaans meerdere items die u kunnen een back-up, maar in het geval van virtuele machines wordt er slechts één back-item voor de container.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Back-ups maken van virtuele Azure-machines
### <a name="create-a-protection-policy"></a>Een beveiligingsbeleid maken
Het is niet verplicht te maken van een nieuw beveiligingsbeleid back-up van uw virtuele machines starten. De kluis wordt geleverd met een 'standaardbeleid' die kunnen worden gebruikt voor het snel geschikt maken van beveiliging en later bewerkt met de juiste gegevens. U kunt een lijst van de beleidsregels die beschikbaar zijn in de kluis ophalen met behulp van de **Get-AzureRmBackupProtectionPolicy** cmdlet:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> De tijdzone van het veld BackupTime in PowerShell is UTC. Echter, wanneer de back-uptijd wordt weergegeven in de Azure portal, de tijdzone wordt uitgelijnd met het lokale systeem samen met de UTC-offset.
>
>

Er is een back-upbeleid gekoppeld aan ten minste één bewaarbeleid. Het bewaarbeleid definieert hoe lang een herstelpunt wordt gehouden met Azure Backup. De **nieuw AzureRmBackupRetentionPolicy** cmdlet maakt een PowerShell-objecten die informatie over bewaarbeleid bevatten. Deze bewaren beleidsobjecten worden gebruikt als invoer voor de *nieuw AzureRmBackupProtectionPolicy* cmdlet, of rechtstreeks met de *inschakelen AzureRmBackupProtection* cmdlet.

Een back-upbeleid definieert wanneer en hoe vaak de back-up van een item wordt uitgevoerd. De **nieuw AzureRmBackupProtectionPolicy** cmdlet maakt een PowerShell-object dat back-upbeleid informatie bevat. Het back-upbeleid wordt gebruikt als invoer voor de *inschakelen AzureRmBackupProtection* cmdlet.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Beveiliging inschakelen
Twee objecten - het Item en het beleid voor het inschakelen van beveiliging omvat en beide moeten deel uitmaken van dezelfde kluis. Als het beleid is gekoppeld aan het item, wordt de back-werkstroom starten op het opgegeven schema.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>Eerste back-up
Het back-upschema zorgt voor de volgende back-ups en de incrementele kopie van de volledige initiële kopie voor het item te doen. Echter, als u wilt zorgen dat de eerste back-up naar gebeuren op een bepaalde tijd of zelfs onmiddellijk gebruikt u de **back-up AzureRmBackupItem** cmdlet:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> De tijdzone van de StartTime en EndTime velden wordt weergegeven in PowerShell is UTC. Wanneer de dezelfde informatie wordt weergegeven in de Azure portal, is echter de tijdzone op de lokale klok uitgelijnd.
>
>

### <a name="monitoring-a-backup-job"></a>Bewaking van een back-uptaak
De meeste langlopende bewerkingen in Azure Backup gemodelleerd zijn als een taak. Hiermee kunt u gemakkelijk voortgang volgen zonder te houden van de Azure-portal openen te allen tijde.

Als u de meest recente status van een taak wordt uitgevoerd, gebruikt de **Get-AzureRmBackupJob** cmdlet.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

In plaats van deze taken voor voltooiing - dit is niet nodig, aanvullende code - polling is het eenvoudiger om te gebruiken de **wacht AzureRmBackupJob** cmdlet. Als in een script gebruikt, wordt de cmdlet de uitvoering onderbroken totdat de taak is voltooid of de opgegeven time-outwaarde is bereikt.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Een Azure-virtuele machine herstellen
Als u wilt back-upgegevens herstellen, moet u identificeren van het artikel back-up en het herstelpunt dat de gegevens van de punt in tijd bevat. Deze informatie wordt verstrekt aan de cmdlet terugzetten AzureRmBackupItem terugzetten van gegevens uit de kluis om het account van de klant te initiëren.

### <a name="select-the-vm"></a>Selecteer de virtuele machine
Als u het PowerShell-object waarmee de juiste back-artikel, moet u starten vanuit de Container in de kluis en boven naar beneden objecthiërarchie werken. Gebruik om te selecteren in de container waarin de VM vertegenwoordigt, de **Get-AzureRmBackupContainer** cmdlet en doorsluizen die u wilt de **Get-AzureRmBackupItem** cmdlet.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>Kies een herstelpunt
U kunt nu aanbieden alle herstelpunten voor de back-item met de **Get-AzureRmBackupRecoveryPoint** cmdlet, en kies het herstelpunt te herstellen. Meestal gebruikers het meest recente Kies *AppConsistent* wijst u in de lijst.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

De variabele ```$rp``` is een matrix van herstelpunten voor de geselecteerde back-up artikel, in omgekeerde volgorde gesorteerd tijd - het laatste herstelpunt bij index 0 is. Gebruik standaard PowerShell matrix indexeren om op te halen van het herstelpunt. Bijvoorbeeld: ```$rp[0]``` wordt het meest recente herstelpunt selecteren.

### <a name="restoring-disks"></a>Herstellen van schijven
Er is een belangrijk verschil tussen de herstelbewerkingen die zijn gedaan via de Azure-portal en via Azure PowerShell. Met PowerShell stopt de herstelbewerking bij het herstellen van de schijven en configuratie-informatie van het herstelpunt. Er wordt een virtuele machine niet gemaakt.

> [!WARNING]
> De Restore-AzureRmBackupItem maakt een virtuele machine niet. De schijven wordt alleen hersteld naar het opgegeven opslagaccount. Dit is niet hetzelfde gedrag die treden er al in de Azure portal.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

U kunt de details ophalen van de restore-bewerking via de **Get-AzureRmBackupJobDetails** cmdlet wanneer de hersteltaak is voltooid. De *ErrorDetails* eigenschap heeft de benodigde informatie om het opnieuw opbouwen van de virtuele machine.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-the-vm"></a>De virtuele machine maken
Het bouwen van de virtuele machine buiten de herstelde schijven kan worden gedaan met behulp van de oudere Azure Service Management PowerShell-cmdlets, de nieuwe Azure Resource Manager-sjablonen of zelfs via de Azure-portal. We tonen hoe u er met de Azure Service Management-cmdlets in een kort voorbeeld.

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

Lees voor meer informatie over het bouwen van een virtuele machine van de herstelde schijven over de volgende cmdlets:

* [Voeg AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [Nieuwe AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Nieuwe-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Codevoorbeelden
### <a name="1-get-the-completion-status-of-job-sub-tasks"></a>1. De voltooiingsstatus van onderliggende taken ophalen
Om bij te houden de voltooiingsstatus van afzonderlijke onderliggende taken, kunt u de **Get-AzureRmBackupJobDetails** cmdlet:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data to Backup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. Maak een rapport dagelijks/wekelijks back-uptaken
Beheerders willen doorgaans weten back-uptaken uitgevoerd in de afgelopen 24 uur de status van deze back-uptaken. Bovendien geeft de hoeveelheid gegevens overgedragen beheerders een manier om in te schatten van de maandelijkse gebruiksgegevens van de gegevens. Het onderstaande script de ruwe gegevens ophaalt uit de Azure Backup-service en de informatie in de PowerShell-console weergegeven.

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -To $enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for the last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract the information for the reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

Als u wilt de mogelijkheden voor grafieken toevoegen aan de rapportuitvoer van dit, leren van de TechNet-blogbericht [voor grafieken met PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Volgende stappen
Als u liever met PowerShell koppelen aan uw Azure-resources, controleert u het PowerShell-artikel voor het beveiligen van Windows Server [implementeren en beheren van back-up voor Windows Server](backup-client-automation-classic.md). Er is ook een PowerShell-artikel voor het beheren van back-ups van DPM [implementeren en beheren van Backup voor DPM](backup-dpm-automation-classic.md). Hebben beide van deze artikelen een versie voor implementaties van Resource Manager, evenals klassieke implementaties.
