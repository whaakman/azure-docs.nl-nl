---
title: Azure Site Recovery - instellingen en testen van herstel na noodgevallen voor Azure-machines met behulp van Azure PowerShell | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen voor Azure-machines met Azure Site Recovery met behulp van Azure PowerShell.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: a37cfb19ab63335f120383a20753bf2b8333bd96
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448922"
---
# <a name="set-up-disaster-recovery-for-azure-virtual-machines-using-azure-powershell"></a>Instellen van herstel na noodgevallen voor Azure-machines met behulp van Azure PowerShell


In dit artikel ziet u hoe u kunt instellen en testen herstel na noodgevallen voor Azure-machines met behulp van Azure PowerShell.

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> - Maak een Recovery Services-kluis.
> - De kluiscontext instellen voor de PowerShell-sessie.
> - Bereid de kluis om te beginnen met het repliceren van virtuele machines van Azure.
> - Netwerktoewijzingen maken.
> - Opslagaccounts voor het repliceren van virtuele machines te maken.
> - Virtuele Azure-machines repliceren naar een regio voor herstel na noodgevallen.
> - Een testfailover uitvoeren, te valideren, en failovertest opschonen.
> - Failover naar de recovery-regio.

> [!NOTE]
> Niet alle scenario mogelijkheden die beschikbaar zijn via de portal is mogelijk beschikbaar via Azure PowerShell. Enkele van de scenario-mogelijkheden die momenteel niet ondersteund via Azure PowerShell zijn:
> - De mogelijkheid om op te geven dat alle schijven in een virtuele machine zonder expliciet opgeven van elke schijf van de virtuele machine moeten worden gerepliceerd.  

## <a name="prerequisites"></a>Vereisten

Voordat u begint:
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](azure-to-azure-support-matrix.md) voor alle onderdelen.
- U beschikt over versie 5.7.0 of hoger van de AzureRm PowerShell-module. Als u wilt installeren of upgraden van Azure PowerShell, volgt u deze [handleiding voor het installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Meld u aan bij uw Microsoft Azure-abonnement

Meld u aan bij uw Azure-abonnement met de cmdlet Connect-AzureRmAccount

```azurepowershell
Connect-AzureRmAccount
```
Selecteer uw Azure-abonnement. Gebruik de cmdlet Get-AzureRmSubscription om te profiteren van de lijst met Azure-abonnementen hebt. Selecteer het Azure-abonnement om te werken met behulp van de Select-AzureRmSubscription-cmdlet.

```azurepowershell
Select-AzureRmSubscription -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Details van de virtuele machines worden gerepliceerd ophalen

In het voorbeeld in dit artikel wordt een virtuele machine in de regio VS-Oost worden gerepliceerd naar en hersteld in de regio VS-West 2. De virtuele machine wordt gerepliceerd, is een virtuele machine met een besturingssysteemschijf en één gegevensschijf. De naam van de virtuele machine die in het voorbeeld is AzureDemoVM.

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzureRmVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

Write-Output $VM     
```

```
ResourceGroupName  : A2AdemoRG
Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/A2AdemoRG/providers/Microsoft.Compute/virtualMachines/AzureDemoVM
VmId               : 1b864902-c7ea-499a-ad0f-65da2930b81b
Name               : AzureDemoVM
Type               : Microsoft.Compute/virtualMachines
Location           : eastus
Tags               : {}
DiagnosticsProfile : {BootDiagnostics}
HardwareProfile    : {VmSize}
NetworkProfile     : {NetworkInterfaces}
OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState  : Succeeded
StorageProfile     : {ImageReference, OsDisk, DataDisks}
```

Schijfdetails van de ophalen voor de schijven van de virtuele machine. Details van schijf wordt gebruikt later bij het starten van replicatie voor de virtuele machine.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Maak een resourcegroep waarin u wilt maken van de Recovery Services-kluis.

> [!IMPORTANT]
> * De Recovery services-kluis en de virtuele machines die wordt beveiligd, moeten zich in verschillende Azure-locaties.
> * De resourcegroep van de Recovery services-kluis en de virtuele machines die wordt beveiligd, moet zich in verschillende Azure-locaties.
> * De Recovery services-kluis en de resourcegroep waartoe deze behoort, kunnen zich in dezelfde Azure-locatie.

In het voorbeeld in dit artikel is de virtuele machine die wordt beveiligd in de regio VS-Oost. De herstelregio geselecteerd voor herstel na noodgevallen is de regio VS-West 2. De recovery services-kluis en de resourcegroep van de kluis zijn beide in de herstelregio (VS-West 2)

```azurepowershell
#Create a resource group for the recovery services vault in the recovery Azure region
New-AzureRmResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"
```
```
ResourceGroupName : a2ademorecoveryrg
Location          : westus2
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg
```

Maak een Recovery services-kluis. In het voorbeeld hieronder een Recovery Services-kluis met de naam a2aDemoRecoveryVault wordt gemaakt in de regio VS-West 2.

```azurepowershell
#Create a new Recovery services vault in the recovery region
$vault = New-AzureRmRecoveryServicesVault -Name "a2aDemoRecoveryVault" -ResourceGroupName "a2ademorecoveryrg" -Location "West US 2"

Write-Output $vault
```
```
Name              : a2aDemoRecoveryVault
ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg/providers/Microsoft.RecoveryServices/vaults/a2aDemoRecoveryVault
Type              : Microsoft.RecoveryServices/vaults
Location          : westus2
ResourceGroupName : a2ademorecoveryrg
SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```
## <a name="set-the-vault-context"></a>De kluiscontext instellen

> [!TIP]
> De Azure Site Recovery PowerShell-module (AzureRm.RecoveryServices.SiteRecovery-module) wordt geleverd met eenvoudig te gebruiken-aliassen voor de meeste cmdlets. De cmdlets in de module verstaan het  *\<bewerking >-**AzureRmRecoveryServicesAsr**\<Object >* en gelijkwaardige aliassen die de vorm hebben  *\<Bewerking >-**ASR**\<Object >*. In dit artikel wordt de cmdlet-aliassen voor betere leesbaarheid.

Stel de context van de kluis voor gebruik in de PowerShell-sessie. Om dit te doen, download de kluis settings-bestand op en importeer het gedownloade bestand in de PowerShell-sessie om in te stellen van de context van de kluis.

Nadat deze is ingesteld, de volgende Azure Site Recovery-bewerkingen in de PowerShell-sessie worden uitgevoerd in de context van de geselecteerde kluis.

 ```azurepowershell
#Download the vault settings file for the vault.
$Vaultsettingsfile = Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteRecovery -Path C:\users\user\Documents\

#Import the downloaded vault settings file to set the vault context for the PowerShell session.
Import-AzureRmRecoveryServicesAsrVaultSettingsFile -Path $Vaultsettingsfile.FilePath

```
```
ResourceName         ResourceGroupName ResourceNamespace          ResourceType
------------         ----------------- -----------------          -----------
a2aDemoRecoveryVault a2ademorecoveryrg Microsoft.RecoveryServices Vaults     
```

```azurepowershell
#Delete the downloaded vault settings file
Remove-Item -Path $Vaultsettingsfile.FilePath
```
## <a name="prepare-the-vault-to-start-replicating-azure-virtual-machines"></a>De kluis om te beginnen met het repliceren van virtuele Azure-machines voorbereiden

### <a name="create-a-site-recovery-fabric-object-to-represent-the-primary-source-region"></a>Een Site Recovery-infrastructuur-object om weer te geven van de regio van de primaire (bron) maken

De fabric-object in de kluis vertegenwoordigt een Azure-regio. Het primaire fabric-object is gemaakt om weer te geven van de Azure-regio die deel uitmaken van virtuele machines die wordt beveiligd op de kluis op. In het voorbeeld in dit artikel is de virtuele machine die wordt beveiligd in de regio VS-Oost.

- Slechts één fabric object kan per regio worden gemaakt. 
- Als u Site Recovery-replicatie voor een virtuele machine in Azure portal eerder hebt ingeschakeld, maakt Site Recovery automatisch een fabric-object. Als een object fabric voor een regio bestaat, kunt u een nieuw wachtwoord kan niet maken.


Voordat u begint, houd er rekening mee dat Site Recovery-bewerkingen asynchroon worden uitgevoerd. Wanneer u een bewerking hebt gestart, wordt een Azure Site Recovery-taak wordt verzonden en wordt een taak voor het bijhouden van object wordt geretourneerd. De taak voor het bijhouden van object gebruiken om op te halen van de meest recente status voor de taak (Get-ASRJob) en het controleren van de status van de bewerking.

```azurepowershell
#Create Primary ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'East US'  -Name "A2Ademo-EastUS"

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$PrimaryFabric = Get-AsrFabric -Name "A2Ademo-EastUS"
```
Als u virtuele machines van Azure-regio's bij dezelfde kluis worden beveiligd, moet u één fabric-object voor elke bron Azure-regio maken.

### <a name="create-a-site-recovery-fabric-object-to-represent-the-recovery-region"></a>Maken van een Site Recovery-infrastructuur-object om weer te geven van de herstelregio

De recovery-infrastructuur-object vertegenwoordigt het herstel van Azure-locatie. Virtuele machines worden gerepliceerd naar en hersteld (in het geval van een failover) de herstelregio vertegenwoordigd door de recovery-infrastructuur. Het herstel van Azure-regio die wordt gebruikt in dit voorbeeld is VS-West 2.

```azurepowershell
#Create Recovery ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'West US 2'  -Name "A2Ademo-WestUS"

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$RecoveryFabric = Get-AsrFabric -Name "A2Ademo-WestUS"

```

### <a name="create-a-site-recovery-protection-container-in-the-primary-fabric"></a>Een Site Recovery-beveiliging-container maken in de primaire-infrastructuur

De beveiligingscontainer is een container die wordt gebruikt om gerepliceerde items binnen een infrastructuur te groeperen.

```azurepowershell
#Create a Protection container in the primary Azure region (within the Primary fabric)
$TempASRJob = New-AzureRmRecoveryServicesAsrProtectionContainer -InputObject $PrimaryFabric -Name "A2AEastUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

Write-Output $TempASRJob.State

$PrimaryProtContainer = Get-ASRProtectionContainer -Fabric $PrimaryFabric -Name "A2AEastUSProtectionContainer"
```
### <a name="create-a-site-recovery-protection-container-in-the-recovery-fabric"></a>Een Site Recovery-beveiliging-container maken in de recovery-infrastructuur

```azurepowershell
#Create a Protection container in the recovery Azure region (within the Recovery fabric)
$TempASRJob = New-AzureRmRecoveryServicesAsrProtectionContainer -InputObject $RecoveryFabric -Name "A2AWestUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"

Write-Output $TempASRJob.State

$RecoveryProtContainer = Get-ASRProtectionContainer -Fabric $RecoveryFabric -Name "A2AWestUSProtectionContainer"
```

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

```azurepowershell
#Create replication policy
$TempASRJob = New-ASRPolicy -AzureToAzure -Name "A2APolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$ReplicationPolicy = Get-ASRPolicy -Name "A2APolicy"
```
### <a name="create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container"></a>Een toewijzing van beveiligingscontainer tussen de primaire en beveiligingscontainer maken

Een toewijzing van beveiligingscontainer wijst de primaire beveiliging-container met een beveiligingscontainer herstel en beleid voor replicatie. Maak een toewijzing voor elke replicatiebeleid dat u gebruikt voor het repliceren van virtuele machines tussen twee container beveiliging.

```azurepowershell
#Create Protection container mapping between the Primary and Recovery Protection Containers with the Replication policy
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2APrimaryToRecovery" -Policy $ReplicationPolicy -PrimaryProtectionContainer $PrimaryProtContainer -RecoveryProtectionContainer $RecoveryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$EusToWusPCMapping = Get-ASRProtectionContainerMapping -ProtectionContainer $PrimaryProtContainer -Name "A2APrimaryToRecovery"
```

### <a name="create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover"></a>Maken van een toewijzing van beveiligingscontainer voor failback (omgekeerde replicatie na een failover)

Na een failover, wanneer u klaar bent om te worden overgebracht naar de virtuele machine terug naar de oorspronkelijke Azure-regio u failback. Failback-proces, de virtuele machine is omgekeerde gerepliceerd van de mislukte via regio naar de oorspronkelijke regio. Omgekeerde replicatie overschakelen de rollen van de oorspronkelijke regio en de herstelregio. De oorspronkelijke regio wordt nu de nieuwe herstelregio en wat oorspronkelijk was de herstelregio nu wordt de primaire regio. De toewijzing van beveiligingscontainer voor omgekeerde replicatie geeft de geschakelde rollen van de oorspronkelijke en recovery-regio's.

```azurepowershell
#Create Protection container mapping (for failback) between the Recovery and Primary Protection Containers with the Replication policy
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2ARecoveryToPrimary" -Policy $ReplicationPolicy -PrimaryProtectionContainer $RecoveryProtContainer -RecoveryProtectionContainer $PrimaryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

## <a name="create-cache-storage-accounts-and-target-storage-accounts"></a>Cache-opslagaccount (s) en doel-opslagaccount (s) maken

Een cache-opslagaccount is een standaard opslagaccount in dezelfde Azure-regio als de virtuele machine wordt gerepliceerd. De cache-opslagaccount wordt gebruikt voor het opslaan van replicatiewijzigingen tijdelijk, voordat de wijzigingen worden verplaatst naar de Azure-regio van het herstel. (Maar hoeven niet) kunt u verschillende cacheopslagaccounts voor de verschillende schijven van een virtuele machine opgeven.

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$EastUSCacheStorageAccount = New-AzureRmStorageAccount -Name "a2acachestorage" -ResourceGroupName "A2AdemoRG" -Location 'East US' -SkuName Standard_LRS -Kind Storage
```

Voor virtuele machines **niet met behulp van beheerde schijven**, het doelopslagaccount is het opslagaccount (s) in de herstelregio waarnaar de schijven van de virtuele machine worden gerepliceerd. Het doelopslagaccount is een standard storage-account of een premium storage-account. Selecteer het type opslagaccount dat vereist is op basis van de veranderingssnelheid van gegevens (i/o schrijven tarief) voor de schijven en de Azure Site Recovery ondersteunde verloop limieten voor het opslagtype.

```azurepowershell
#Create Target storage account in the recovery region. In this case a Standard Storage account
$WestUSTargetStorageAccount = New-AzureRmStorageAccount -Name "a2atargetstorage" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -SkuName Standard_LRS -Kind Storage

```

## <a name="create-network-mappings"></a>Netwerktoewijzingen maken

Virtuele netwerken in de primaire regio een netwerktoewijzing toegewezen aan virtuele netwerken in de herstelregio voor. De netwerktoewijzing Hiermee geeft u het Azure-netwerk in de herstelregio, die een virtuele machine in het primaire virtuele netwerk failover naar moeten. Een virtueel Azure-netwerk kan worden toegewezen aan slechts één Azure virtueel netwerk in een herstelregio voor.

- Een Azure-netwerk maken in de herstelregio voor de failover

   ```azurepowershell
    #Create a Recovery Network in the recovery region
    $WestUSRecoveryVnet = New-AzureRmVirtualNetwork -Name "a2arecoveryvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.0.0.0/16"

    Add-AzureRmVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $WestUSRecoveryVnet -AddressPrefix "10.0.0.0/20" | Set-AzureRmVirtualNetwork

    $WestUSRecoveryNetwork = $WestUSRecoveryVnet.Id
   ```
- Ophalen van de primaire virtuele netwerk (het vnet waarmee de virtuele machine is verbonden met)
   ```azurepowershell
    #Retrieve the virtual network that the virtual machine is connected to

    #Get first network interface card(nic) of the virtual machine
    $SplitNicArmId = $VM.NetworkProfile.NetworkInterfaces[0].Id.split("/")

    #Extract resource group name from the ResourceId of the nic
    $NICRG = $SplitNicArmId[4]

    #Extract resource name from the ResourceId of the nic
    $NICname = $SplitNicArmId[-1]

    #Get network interface details using the extracted resource group name and resource name
    $NIC = Get-AzureRmNetworkInterface -ResourceGroupName $NICRG -Name $NICname

    #Get the subnet ID of the subnet that the nic is connected to
    $PrimarySubnet = $NIC.IpConfigurations[0].Subnet

    # Extract the resource ID of the Azure virtual network the nic is connected to from the subnet ID
    $EastUSPrimaryNetwork = (Split-Path(Split-Path($PrimarySubnet.Id))).Replace("\","/")  
   ```
- De netwerktoewijzing tussen de primaire virtuele netwerk en het virtuele herstelnetwerk maken
   ```azurepowershell
    #Create an ASR network mapping between the primary Azure virtual network and the recovery Azure virtual network
    $TempASRJob = New-ASRNetworkMapping -AzureToAzure -Name "A2AEusToWusNWMapping" -PrimaryFabric $PrimaryFabric -PrimaryAzureNetworkId $EastUSPrimaryNetwork -RecoveryFabric $RecoveryFabric -RecoveryAzureNetworkId $WestUSRecoveryNetwork

    #Track Job status to check for completion
    while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
            sleep 10;
            $TempASRJob = Get-ASRJob -Job $TempASRJob
    }

    #Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
    Write-Output $TempASRJob.State

   ```
- Netwerktoewijzing voor de omgekeerde richting (failback) maken
    ```azurepowershell
    #Create an ASR network mapping for failback between the recovery Azure virtual network and the primary Azure virtual network
    $TempASRJob = New-ASRNetworkMapping -AzureToAzure -Name "A2AWusToEusNWMapping" -PrimaryFabric $RecoveryFabric -PrimaryAzureNetworkId $WestUSRecoveryNetwork -RecoveryFabric $PrimaryFabric -RecoveryAzureNetworkId $EastUSPrimaryNetwork

    #Track Job status to check for completion
    while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
            sleep 10;
            $TempASRJob = Get-ASRJob -Job $TempASRJob
    }

    #Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
    Write-Output $TempASRJob.State
    ```

## <a name="replicate-azure-virtual-machine"></a>Virtuele Azure-machines repliceren

Virtuele machine van Azure met repliceren **beheerde schijven**.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzureRmResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data disk
$datadiskId1  = $vm.StorageProfile.DataDisks[0].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[0]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[0]. StorageAccountType

$DataDisk1ReplicationConfig  = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId1 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig


#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId

```

Virtuele machine van Azure met repliceren **niet-beheerde schijven**.

```azurepowershell
#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#Disk replication configuration for the OS disk
$OSDiskReplicationConfig = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -VhdUri $OSDiskVhdURI.Uri -LogStorageAccountId $EastUSCacheStorageAccount.Id -RecoveryAzureStorageAccountId $WestUSTargetStorageAccount.Id

#Disk replication configuration for data disk
$DataDisk1ReplicationConfig = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -VhdUri $DataDisk1VhdURI.Uri -LogStorageAccountId $EastUSCacheStorageAccount.Id -RecoveryAzureStorageAccountId $WestUSTargetStorageAccount.Id

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzureRmResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.  
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}


#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

Zodra de begin-replicatiebewerking is gelukt, worden gegevens van virtuele machines wordt gerepliceerd naar de herstelregio voor.

Het replicatieproces wordt gestart door een kopie van de replicerende schijven van de virtuele machine in de herstelregio in eerste instantie seeding. Deze fase wordt de fase van de initiële replicatie genoemd.

Nadat de eerste replicatie is voltooid, wordt replicatie naar de differentiële synchronisatiefase verplaatst. Op dit moment de virtuele machine is beveiligd en een testfailover kan erop worden uitgevoerd. De replicatiestatus van het gerepliceerde item voor de virtuele machine gaat naar de status 'Beveiligd' nadat de initiële replicatie is voltooid.

De replicatiestatus en de replicatiestatus voor de virtuele machine met het ophalen van details van de replicatie wordt beveiligd item overeenkomt met het bewaken.
```azurepowershell
 Get-ASRReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState ReplicationHealth
------------ --------------- -----------------
AzureDemoVM  Protected       Normal           
```

## <a name="perform-a-test-failover-validate-and-cleanup-test-failover"></a>Een testfailover uitvoeren, te valideren, en failovertest opschonen

Wanneer replicatie voor de virtuele machine een beveiligde status bereikt heeft, kan een testfailover worden uitgevoerd op de virtuele machine (op het item beveiligd replicatie van de virtuele machine.)

```azurepowershell
#Create a separate network for test failover (not connected to my DR network)
$TFOVnet = New-AzureRmVirtualNetwork -Name "a2aTFOvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.3.0.0/16"

Add-AzureRmVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $TFOVnet -AddressPrefix "10.3.0.0/20" | Set-AzureRmVirtualNetwork

$TFONetwork= $TFOVnet.Id
```

Testfailover uitvoeren.
```azurepowershell
$ReplicationProtectedItem = Get-ASRReplicationProtectedItem -FriendlyName "AzureDemoVM" -ProtectionContainer $PrimaryProtContainer

$TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem -AzureVMNetworkId $TFONetwork -Direction PrimaryToRecovery


```

Wachten op voor de testfailover te voltooien.
```azurepowershell
Get-ASRJob -Job $TFOJob
```

```
Name             : 3dcb043e-3c6d-4e0e-a42e-8d4245668547
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg/providers/Microsoft.RecoveryServices/vaults/a2aDemoR
                   ecoveryVault/replicationJobs/3dcb043e-3c6d-4e0e-a42e-8d4245668547
Type             : Microsoft.RecoveryServices/vaults/replicationJobs
JobType          : TestFailover
DisplayName      : Test failover
ClientRequestId  : 1ef8515b-b130-4452-a44d-91aaf071931c ActivityId: 907bb2bc-ebe6-4732-8b66-77d0546eaba8
State            : Succeeded
StateDescription : Completed
StartTime        : 4/25/2018 4:29:43 AM
EndTime          : 4/25/2018 4:33:06 AM
TargetObjectId   : ce86206c-bd78-53b4-b004-39b722c1ac3a
TargetObjectType : ProtectionEntity
TargetObjectName : azuredemovm
AllowedActions   :
Tasks            : {Prerequisites check for test failover, Create test virtual machine, Preparing the virtual machine, Start the virtual machine}
Errors           : {}
```
Zodra de test-failover-taak voltooid is, kunt u verbinding maken met de virtuele machine failover testen en valideren van de testfailover.

Zodra het testen is voltooid op de virtuele machine failover testen, test u opschonen van de testexemplaar op basis van het opruimen van de failoverbewerking. Deze bewerking wordt de test-kopie van de virtuele machine die is gemaakt door de testfailover verwijderd.

```azurepowershell
$Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicationProtectedItem

Get-ASRJob -Job $Job_TFOCleanup | Select State
```
```
State    
-----    
Succeeded
```

## <a name="failover-to-azure"></a>Failover naar Azure

Failover uit van de virtuele machine naar een specifiek herstelpunt.

```azurepowershell
$RecoveryPoints = Get-ASRRecoveryPoint -ReplicationProtectedItem $ReplicationProtectedItem

#The list of recovery points returned may not be sorted chronologically and will need to be sorted first, in order to be able to find the oldest or the latest recovery points for the virtual machine.
"{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[-1].RecoveryPointTime
```
```
CrashConsistent 4/24/2018 11:10:25 PM
```


```azurepowershell
#Start the failover job
$Job_Failover = Start-ASRUnplannedFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[-1]

do {
        $Job_Failover = Get-ASRJob -Job $Job_Failover;
        sleep 30;
} while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))

$Job_Failover.State
```
```
Succeeded
```

Nadat de failover is mislukt, kunt u de failoverbewerking doorvoeren.
```azurepowershell
$CommitFailoverJOb = Start-ASRCommitFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem

Get-ASRJOb -Job $CommitFailoverJOb
```

```
Name             : 58afc2b7-5cfe-4da9-83b2-6df358c6e4ff
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg/providers/Microsoft.RecoveryServices/vaults/a2aDemoR
                   ecoveryVault/replicationJobs/58afc2b7-5cfe-4da9-83b2-6df358c6e4ff
Type             : Microsoft.RecoveryServices/vaults/replicationJobs
JobType          : CommitFailover
DisplayName      : Commit
ClientRequestId  : 10a95d6c-359e-4603-b7d9-b7ee3317ce94 ActivityId: 8751ada4-fc42-4238-8de6-a82618408fcf
State            : Succeeded
StateDescription : Completed
StartTime        : 4/25/2018 4:50:58 AM
EndTime          : 4/25/2018 4:51:01 AM
TargetObjectId   : ce86206c-bd78-53b4-b004-39b722c1ac3a
TargetObjectType : ProtectionEntity
TargetObjectName : azuredemovm
AllowedActions   :
Tasks            : {Prerequisite check, Commit}
Errors           : {}
```

Wanneer u bent klaar om terug te gaan naar de oorspronkelijke regio beveiligd omgekeerde replicatie starten voor de replicatie na een failover item met de cmdlet Update-AzureRmRecoveryServicesAsrProtectionDirection.

## <a name="next-steps"></a>Volgende stappen
Weergave de [Azure Site Recovery PowerShell-referentie ](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery) voor meer informatie over hoe u kunt andere taken uitvoert zoals herstelplannen maken en testen van failover van plannen voor herstel via PowerShell.
