---
title: Azure Site Recovery - instellingen en herstel na noodgevallen voor virtuele machines in Azure met Azure PowerShell testen | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen voor virtuele machines in Azure met Azure Site Recovery met Azure PowerShell.
services: site-recovery
author: bsiva
manager: abhemraj
editor: raynew
ms.service: site-recovery
ms.topic: article
ms.date: 04/25/2018
ms.author: bsiva
ms.openlocfilehash: 83f167b047fa9c5df7c04304c7955ae838ae8373
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="set-up-disaster-recovery-for-azure-virtual-machines-using-azure-powershell"></a>Instellen van herstel na noodgevallen voor virtuele machines in Azure met Azure PowerShell

In dit artikel leert u zien hoe instellen en testen herstel na noodgevallen voor virtuele machines in Azure met Azure PowerShell. 

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> - Maak een Recovery Services-kluis.
> - De context van de kluis ingesteld voor de PowerShell-sessie.
> - Bereid de kluis om te repliceren van virtuele machines in Azure.
> - Maak-netwerkkoppelingen.
> - Opslagaccounts voor het repliceren van virtuele machines te maken.
> - Azure virtuele machines repliceren naar een regio herstelpunt voor herstel na noodgevallen.
> - Een testfailover uitvoeren, te valideren, en het opruimen van de testfailover.
> - Failover naar de herstel-regio.

> [!NOTE]
> Azure Site Recovery PowerShell-ondersteuning voor de '*herstel na noodgevallen voor Azure virtual machines*' scenario is momenteel in preview. Niet alle scenario mogelijkheden zijn beschikbaar via de portal mogelijk niet beschikbaar is via Azure PowerShell. Enkele van de scenario-mogelijkheden die momenteel niet ondersteund via Azure PowerShell zijn:
> - De mogelijkheid om Azure virtuele machines die gebruikmaken van beheerde schijven repliceren.
> - De mogelijkheid om op te geven dat alle schijven in een virtuele machine zonder expliciet opgeven van elke schijf van de virtuele machine moeten worden gerepliceerd.  

## <a name="prerequisites"></a>Vereisten

Voordat u begint:
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](azure-to-azure-support-matrix.md) voor alle onderdelen.
- U beschikt over versie 5.7.0 of hoger van de AzureRm PowerShell-module. Als u wilt installeren of upgraden van Azure PowerShell, voert u [handleiding voor het installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Aanmelden bij uw Microsoft Azure-abonnement

Aanmelden bij uw Azure-abonnement met de cmdlet Connect-AzureRmAccount

```azurepowershell
Connect-AzureRmAccount
```
Selecteer uw Azure-abonnement. Gebruik de cmdlet Get-AzureRmSubscription voor de lijst met Azure-abonnementen hebt u toegang tot. Selecteer het Azure-abonnement werken met behulp van de Select-AzureRmSubscription-cmdlet.

```azurepowershell
Select-AzureRmSubscription -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Details van de virtuele machine worden gerepliceerd ophalen

In het voorbeeld in dit artikel wordt wordt een virtuele machine in de regio VS-Oost gerepliceerd naar en in de regio VS-West 2 hersteld. De virtuele machine wordt gerepliceerd is een virtuele machine met een besturingssysteemschijf en een enkele gegevensschijf. De naam van de virtuele machine die in het voorbeeld is AzureDemoVM.

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

Details van de schijf ophalen voor de schijven van de virtuele machine. Details van de schijf wordt later bij het starten van replicatie voor de virtuele machine worden gebruikt.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Maak een resourcegroep in te maken van de Recovery Services-kluis. 

> [!IMPORTANT]
> * De Recovery services-kluis en de virtuele machines die worden beveiligd, moet zich in verschillende Azure-locaties.
> * De resourcegroep van de Recovery services-kluis en de virtuele machines die worden beveiligd, moet zich in verschillende Azure-locaties.
> * De Recovery services-kluis en de resourcegroep waartoe deze behoort, kunnen zich in dezelfde Azure-locatie.
 
In het voorbeeld in dit artikel wordt de virtuele machine wordt beveiligd in de regio VS-Oost. De regio herstel is geselecteerd voor herstel na noodgevallen, de regio VS-West 2 is. De recovery services-kluis en de resourcegroep van de kluis zijn beide in de regio herstel (VS-West 2)

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
   
Maak een Recovery services-kluis. In het voorbeeld hieronder een Recovery Services-kluis a2aDemoRecoveryVault met de naam wordt gemaakt in de regio VS-West 2.

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
## <a name="set-the-vault-context"></a>De context van de kluis instellen

> [!TIP]
> De Azure Site Recovery PowerShell-module (AzureRm.RecoveryServices.SiteRecovery module) wordt geleverd met eenvoudig te gebruiken voor de meeste cmdlets-aliassen. De cmdlets in de module de indeling nemen  *\<bewerking >-**AzureRmRecoveryServicesAsr**\<Object >* en gelijkwaardige aliassen die de vorm hebben  *\<Bewerking >-**ASR**\<Object >*. In dit artikel gebruikt de cmdlet-aliassen voor betere leesbaarheid.

Stel de context van de kluis voor gebruik in de PowerShell-sessie. U doet dit door het instellingenbestand kluis te downloaden en het gedownloade bestand importeren in de PowerShell-sessie in te stellen de context van de kluis. 

Nadat deze is ingesteld, worden verdere Azure Site Recovery-bewerkingen in de PowerShell-sessie uitgevoerd in de context van de geselecteerde kluis. 

 ```azurepowershell
#Download the vault settings file for the vault.
$Vaultsettingsfile = Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteRecovery -Path C:\users\user\Documents\

#Import the downloaded vault settings file to set the vault context for the PowerShell session.
Import-AzureRmRecoveryServicesAsrVaultSettingsFile -Path $Vaultsettingsfile.FilePath

```
```
ResourceName         ResourceGroupName ResourceNamespace          ResouceType
------------         ----------------- -----------------          -----------
a2aDemoRecoveryVault a2ademorecoveryrg Microsoft.RecoveryServices Vaults     
```

```azurepowershell
#Delete the downloaded vault settings file
Remove-Item -Path $Vaultsettingsfile.FilePath
```
## <a name="prepare-the-vault-to-start-replicating-azure-virtual-machines"></a>Voorbereiden van de kluis om te repliceren van virtuele machines in Azure

####<a name="1-create-a-site-recovery-fabric-object-to-represent-the-primarysource-region"></a>1. Een object van de Site Recovery fabric staat voor de regio primary(source) maken

De fabric-object in de kluis vertegenwoordigt een Azure-regio. Het primaire fabric-object, is het fabric-object voor de Azure-regio die virtuele machines die worden beveiligd in de kluis die deel uitmaken van hebt gemaakt. In het voorbeeld in dit artikel wordt de virtuele machine wordt beveiligd in de regio VS-Oost.

> [!NOTE]
> Azure Site Recovery-bewerkingen worden asynchroon uitgevoerd. Wanneer u een bewerking initieert, wordt een Azure Site Recovery-taak wordt verzonden en wordt een taak voor het bijhouden van object wordt geretourneerd. Gebruik de taak voor het bijhouden van object naar de meest recente status ophalen voor de job (Get-ASRJob) en voor het bewaken van de status van de bewerking.

```azurepowershell
#Create Primary ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'East US'  -Name "A2Ademo-EastUS" 

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$PrimaryFabric = Get-AsrFabric -Name "A2Ademo-EastUS"
```
Als u virtuele machines van meerdere Azure-regio's bij dezelfde kluis worden beveiligd, maakt u één fabric-object voor elke bron Azure-regio.

####<a name="2-create-a-site-recovery-fabric-object-to-represent-the-recovery-region"></a>2. Een object van de Site Recovery fabric staat voor de regio herstel maken

Het herstel fabric-object vertegenwoordigt de Azure-locatie van het herstel. Virtuele machines worden gerepliceerd naar en hersteld (in het geval van een failover) het herstel de regio dat wordt vertegenwoordigd door de herstel-infrastructuur. Het herstel van Azure-regio die wordt gebruikt in dit voorbeeld is VS-West 2.

```azurepowershell
#Create Recovery ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'West US 2'  -Name "A2Ademo-WestUS" 

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$RecoveryFabric = Get-AsrFabric -Name "A2Ademo-WestUS"

```

####<a name="3-create-a-site-recovery-protection-container-in-the-primary-fabric"></a>3. Een Site Recovery-beveiligingscontainer maken in de primaire fabric

De beveiligingscontainer is een container gebruikt om gerepliceerde items binnen een structuur te groeperen.

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
####<a name="4-create-a-site-recovery-protection-container-in-the-recovery-fabric"></a>4. Een Site Recovery-beveiligingscontainer maken in de fabric herstel

```azurepowershell
#Create a Protection container in the recovery Azure region (within the Recovery fabric)
$TempASRJob = New-AzureRmRecoveryServicesAsrProtectionContainer -InputObject $RecoveryFabric -Name "A2AWestUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"

Write-Output $TempASRJob.State

$RecoveryProtContainer = Get-ASRProtectionContainer -Fabric $RecoveryFabric -Name "A2AWestUSProtectionContainer"
```

####<a name="5-create-a-replication-policy"></a>5. Een replicatiebeleid maken

```azurepowershell
#Create replication policy
$TempASRJob = New-ASRPolicy -AzureToAzure -Name "A2APolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$ReplicationPolicy = Get-ASRPolicy -Name "A2APolicy"
```
####<a name="6-create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container"></a>6. Maak een toewijzing van de container beveiliging tussen de primaire en herstelsites beveiligingscontainer

Een toewijzing van de container beveiliging wordt de primaire beveiligingscontainer met een container recovery-beveiliging en een beleid voor wachtwoordreplicatie toegewezen. Maak een toewijzing voor elke replicatiebeleid u virtuele machines repliceren tussen een paar van de container beveiliging.

```azurepowershell
#Create Protection container mapping between the Primary and Recovery Protection Containers with the Replication policy
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2APrimaryToRecovery" -Policy $ReplicationPolicy -PrimaryProtectionContainer $PrimaryProtContainer -RecoveryProtectionContainer $RecoveryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$EusToWusPCMapping = Get-ASRProtectionContainerMapping -ProtectionContainer $PrimaryProtContainer -Name "A2APrimaryToRecovery"
```

####<a name="7-create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover"></a>7. Maak een toewijzing van de container beveiliging voor failback (omgekeerde replicatie na een failover)

Na een failover, wanneer u klaar bent om te worden overgebracht naar de virtuele machine terug naar de oorspronkelijke Azure-regio u failback. Aan de mislukte failback via de virtuele machine is reverse gerepliceerd van de mislukte via regio naar de oorspronkelijke regio. De functies van de oorspronkelijke regio en de regio herstel-switch voor omgekeerde replicatie. De oorspronkelijke regio wordt nu het nieuwe herstel de regio en wat oorspronkelijk was de regio van het herstel nu wordt de primaire regio. De toewijzing van de container beveiliging voor omgekeerde replicatie vertegenwoordigt de geschakelde rollen van de oorspronkelijke en herstel regio's.

```azurepowershell
#Create Protection container mapping (for failback) between the Recovery and Primary Protection Containers with the Replication policy 
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2ARecoveryToPrimary" -Policy $ReplicationPolicy -PrimaryProtectionContainer $RecoveryProtContainer -RecoveryProtectionContainer $PrimaryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

## <a name="create-cache-storage-accounts-and-target-storage-accounts"></a>Cache-opslagaccounts en opslagaccounts doel maken

Een opslagaccount van de cache is een standaard opslagaccount in dezelfde Azure-regio als de virtuele machine wordt gerepliceerd. Het opslagaccount van de cache wordt gebruikt om replicatie worden wijzigingen tijdelijk voordat de wijzigingen worden verplaatst naar de Azure-regio van het herstel. (Maar hoeven niet) kunt u verschillende cache storage-accounts voor de verschillende schijven van een virtuele machine opgeven.

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$EastUSCacheStorageAccount = New-AzureRmStorageAccount -Name "a2acachestorage" -ResourceGroupName "A2AdemoRG" -Location 'East US' -SkuName Standard_LRS -Kind Storage
```

Voor virtuele machines niet met beheerde schijven, is het doel storage-account de opslagaccounts in het herstel de regio waarnaar de schijven van de virtuele machine worden gerepliceerd. Het doel storage-account mag een standard-opslag-account of een premium storage-account. Selecteer het type van de storage-account vereist op basis van de gegevenswijzigingssnelheid (i/o schrijven snelheid) voor de schijven en de Azure Site Recovery ondersteund verloop limieten voor het opslagtype.

```azurepowershell
#Create Target storage account in the recovery region. In this case a Standard Storage account
$WestUSTargetStorageAccount = New-AzureRmStorageAccount -Name "a2atargetstorage" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -SkuName Standard_LRS -Kind Storage

```

## <a name="create-network-mappings"></a>Netwerktoewijzingen maken

Virtuele netwerken in de primaire regio een netwerktoewijzing toegewezen aan virtuele netwerken in de regio van het herstel. De netwerktoewijzing geeft de virtuele Azure-netwerk in het herstel de regio die failover naar een virtuele machine in de primaire virtuele netwerk moet worden. Een virtuele Azure-netwerk kan worden toegewezen aan slechts één Azure virtueel netwerk in een regio herstel.

- Een Azure-netwerk maken in de regio herstel failover

   ```azurepowershell
    #Create a Recovery Network in the recovery region
    $WestUSRecoveryVnet = New-AzureRmVirtualNetwork -Name "a2arecoveryvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.0.0.0/16"

    Add-AzureRmVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $WestUSRecoveryVnet -AddressPrefix "10.0.0.0/20" | Set-AzureRmVirtualNetwork

    $WestUSRecoveryNetwork = $WestUSRecoveryVnet.Id
   ```
- Ophalen van de primaire virtuele netwerk (het vnet dat de virtuele machine is verbonden met)
   ```azurepowershell
    #Retrieve the virtual network that the virtual machine is connected to
    
    #Get first network interface card(nic) of the virtual machine
    $SplitNicArmId = $VM.NetworkProfile.NetworkInterfaces[0].Id.split("/")

    #Extract resource group name from the ResourceId of the nic
    $NICRG = $SplitNicArmId[4]

    #Extract resource name from the ResourceId of the nic
    $NICname = $SplitNicArmId[-1]

    #Get network interface details using the extracted resource group name and resourec name
    $NIC = Get-AzureRmNetworkInterface -ResourceGroupName $NICRG -Name $NICname

    #Get the subnet ID of the subnet that the nic is connected to
    $PrimarySubnet = $NIC.IpConfigurations[0].Subnet

    # Extract the resource ID of the Azure virtual network the nic is connected to from the subnet ID
    $EastUSPrimaryNetwork = (Split-Path(Split-Path($PrimarySubnet.Id))).Replace("\","/")  
   ```
- De netwerkkoppeling tussen de primaire virtuele netwerk en het herstel virtuele netwerk maken
   ```azurepowershell
    #Create an ASR network mapping between the primary Azure virtual network and the recovery Azure virtual network
    $TempASRJob = New-ASRNetworkMapping -AzureToAzure -Name "A2AEusToWusNWMapping" -PrimaryFabric $PrimaryFabric -PrimaryAzureNetworkId $EastUSPrimaryNetwork -RecoveryFabric $RecoveryFabric -RecoveryAzureNetworkId $WestUSRecoveryNetwork
    
    #Track Job status to check for completion
    while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
            sleep 10; 
            $TempASRJob = Get-ASRJob -Job $TempASRJob
    }
    
    #Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
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
        
    #Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
    Write-Output $TempASRJob.State
    ```

## <a name="replicate-azure-virtual-machine"></a>Virtuele machine van Azure repliceren

Virtuele machine van Azure repliceren.

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


#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

Zodra de replicatie start bewerking is geslaagd, wordt virtuele machinegegevens gerepliceerd naar de herstel-regio. 

Het replicatieproces wordt gestart door seeding in eerste instantie een kopie van de replicerende schijven van de virtuele machine in de regio van het herstel. In deze fase wordt de initiële replicatiefase genoemd. 

Wanneer de initiële replicatie is voltooid, wordt de replicatie verplaatst naar de differentiële synchronisatiefase. Op dit punt wordt de virtuele machine is beveiligd en een testfailover kan worden uitgevoerd. De replicatiestatus van de gerepliceerde onderdeel staat voor de virtuele machine gaat u naar de status 'Beveiligde' nadat de initiële replicatie is voltooid.

De replicatiestatus en de replicatiestatus voor de virtuele machine met het ophalen van informatie van de replicatie wordt beveiligd item overeenkomt met het bewaken.
```azurepowershell
 Get-ASRReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState ReplicationHealth
------------ --------------- -----------------
AzureDemoVM  Protected       Normal           
```

## <a name="perform-a-test-failover-validate-and-cleanup-test-failover"></a>Een testfailover uitvoeren, te valideren, en testfailover opschonen

Zodra de replicatie voor de virtuele machine een beveiligde status bereikt heeft, kan een testfailover worden uitgevoerd op de virtuele machine (op het item beveiligd replicatie van de virtuele machine.)

```azurepowershell
#Create a seperate network for test failover (not connected to my DR network)
$TFOVnet = New-AzureRmVirtualNetwork -Name "a2aTFOvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.3.0.0/16"

Add-AzureRmVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $TFOVnet -AddressPrefix "10.3.0.0/20" | Set-AzureRmVirtualNetwork

$TFONetwork= $TFOVnet.Id
```

Testfailover uitvoeren.
```azurepowershell
$ReplicationProtectedItem = Get-ASRReplicationProtectedItem -FriendlyName "AzureDemoVM" -ProtectionContainer $PrimaryProtContainer

$TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem -AzureVMNetworkId $TFONetwork -Direction PrimaryToRecovery


```

Wacht totdat de testfailover te voltooien.
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
Zodra de test failover-taak voltooid is, kunt u verbinding maken met de virtuele machine failover testen en valideren van de testfailover.

Zodra de test is voltooid op de virtuele machine failover testen, test u opruimen van de kopie van de test op basis van het opruimen van de failoverbewerking. Deze bewerking worden verwijderd van de test-kopie van de virtuele machine die is gemaakt door de testfailover.

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

De virtuele machine naar een specifiek herstelpunt failover.

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

Zodra de failover is mislukt, kunt u de failoverbewerking doorvoeren.
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

Wanneer u klaar bent om terug te keren naar de oorspronkelijke regio beveiligd start omgekeerde replicatie voor de replicatie na een failover, item met de cmdlet Update-AzureRmRecoveryServicesAsrProtectionDirection.

## <a name="next-steps"></a>Volgende stappen
Weergave de [Azure Site Recovery PowerShell-referentie ](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery) voor meer informatie over hoe u kunt andere taken uitvoert zoals herstelplannen maken en testen van failover van via PowerShell-herstelplannen.
