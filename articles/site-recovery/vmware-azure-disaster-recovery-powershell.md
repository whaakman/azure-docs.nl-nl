---
title: Repliceren en de failover van virtuele VMware-machines naar Azure met PowerShell in Azure Site Recovery | Microsoft Docs
description: Informatie over het instellen van de replicatie en failover naar Azure voor virtuele VMware-machines met PowerShell in Azure Site Recovery.
services: site-recovery
author: bsiva
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: bsiva
ms.openlocfilehash: 051bc3a0e1c0126826e96b49ff0a4e8008c88006
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287851"
---
# <a name="replicate-and-fail-over-vmware-vms-to-azure-with-powershell"></a>Repliceren en de failover van virtuele VMware-machines naar Azure met PowerShell

In dit artikel ziet u hoe u voor replicatie en failover virtuele VMware-machines naar Azure met Azure PowerShell. 

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> - Maak een Recovery Services-kluis en stelt u de context van de kluis.
> - Valideren van de registratie van de server in de kluis.
> - De replicatie, met inbegrip van een beleid voor wachtwoordreplicatie instellen. De vCenter-server toevoegen en virtuele machines te detecteren. >: Een vCenter-server toevoegen en detecteren 
> - Storage-accounts voor het opslaan van gegevens van replicatie maken en de virtuele machines repliceren.
> - Voer een failover. Configureer voor de failover, voert u een e-mailinstellingen voor het repliceren van virtuele machines.

## <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](vmware-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-to-azure.md) voor alle onderdelen.
- U beschikt over versie 5.0.1 of hoger van de AzureRm PowerShell-module. Als u wilt installeren of upgraden van Azure PowerShell, voert u [handleiding voor het installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-into-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de cmdlet Connect-AzureRmAccount:

```azurepowershell
Connect-AzureRmAccount
```
Selecteer het Azure-abonnement dat u wilt uw virtuele VMware-machines te repliceren. Gebruik de cmdlet Get-AzureRmSubscription voor de lijst met Azure-abonnementen hebt u toegang tot. Selecteer het Azure-abonnement werken met behulp van de Select-AzureRmSubscription-cmdlet.

```azurepowershell
Select-AzureRmSubscription -SubscriptionName "ASR Test Subscription"
```
## <a name="set-up-a-recovery-services-vault"></a>Een Recovery Services-kluis instellen

1. Maak een resourcegroep in te maken van de Recovery Services-kluis. In het onderstaande voorbeeld de resourcegroep met de naam VMwareDRtoAzurePS en wordt gemaakt in de regio Oost-Azië.

   ```azurepowershell
   New-AzureRmResourceGroup -Name "VMwareDRtoAzurePS" -Location "East Asia"
   ```
   ```
   ResourceGroupName : VMwareDRtoAzurePS
   Location          : eastasia
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRtoAzurePS
```
   
2. Maak een Recovery services-kluis. In het onderstaande voorbeeld de Recovery services-kluis VMwareDRToAzurePs heet en wordt gemaakt in dezelfde regio Oost-Azië, en in de resourcegroep in de vorige stap hebt gemaakt.

   ```azurepowershell
   New-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePs" -Location "East Asia" -ResourceGroupName "VMwareDRToAzurePs"
   ```
   ```
   Name              : VMwareDRToAzurePs
   ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs
   Type              : Microsoft.RecoveryServices/vaults
   Location          : eastasia
   ResourceGroupName : VMwareDRToAzurePs
   SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
   ``` 

3. Download de registratiecode kluis voor de kluis. De kluisregistratiesleutel wordt gebruikt om de lokale configuratie-Server in deze kluis te registreren. Registratie is onderdeel van een software-installatieproces van de configuratieserver.

   ```azurepowershell
   #Get the vault object by name and resource group and save it to the $vault PowerShell variable 
   $vault = Get-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePS" -ResourceGroupName "VMwareDRToAzurePS"

   #Download vault registration key to the path C:\Work
   Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   ```
   ```
   FilePath
   --------
   C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials
   ```

4. Gebruik de gedownloade kluisregistratiesleutel en volg de stappen in de artikelen hieronder volledige installatie en registratie van de configuratieserver.
   - [Uw beveiligingsdoelstellingen kiezen](vmware-azure-set-up-source.md#choose-your-protection-goals)
   - [De bronomgeving instellen](vmware-azure-set-up-source.md#set-up-the-configuration-server) 

### <a name="set-the-vault-context"></a>De context van de kluis instellen

De context van de kluis met de cmdlet Set-ASRVaultContext instellen. Nadat deze is ingesteld, worden verdere Azure Site Recovery-bewerkingen in de PowerShell-sessie uitgevoerd in de context van de geselecteerde kluis.

> [!TIP]
> De Azure Site Recovery PowerShell-module (AzureRm.RecoveryServices.SiteRecovery module) wordt geleverd met eenvoudig te gebruiken voor de meeste cmdlets-aliassen. De cmdlets in de module de indeling nemen  *\<bewerking >-**AzureRmRecoveryServicesAsr**\<Object >* en gelijkwaardige aliassen die de vorm hebben  *\<Bewerking >-**ASR**\<Object >*. In dit artikel gebruikt de cmdlet-aliassen voor betere leesbaarheid.

In het onderstaande voorbeeld de details van de kluis van de $vault wordt variabele gebruikt om op te geven van de kluis context voor de PowerShell-sessie.

   ```azurepowershell
   Set-ASRVaultContext -Vault $vault
   ```
   ```
   ResourceName      ResourceGroupName ResourceNamespace          ResouceType
   ------------      ----------------- -----------------          -----------
   VMwareDRToAzurePs VMwareDRToAzurePs Microsoft.RecoveryServices vaults
   ```

Als alternatief voor de cmdlet Set-ASRVaultContext kunt een ook de cmdlet Import-AzureRmRecoveryServicesAsrVaultSettingsFile gebruiken om in te stellen de context van de kluis. Geef het pad waar de kluis registratiesleutelbestand zich bevindt als de parameter - path aan de cmdlet Import-AzureRmRecoveryServicesAsrVaultSettingsFile. Bijvoorbeeld:

   ```azurepowershell
   Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   Import-AzureRmRecoveryServicesAsrVaultSettingsFile -Path "C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials"
   ```
Latere secties van dit artikel wordt ervan uitgegaan dat de context van de kluis voor Azure Site Recovery-bewerkingen is ingesteld.

## <a name="validate-vault-registration"></a>Registratie van de kluis valideren

In dit voorbeeld hebben we het volgende:

- Een configuratieserver (**ConfigurationServer**) in deze kluis is geregistreerd.
- Een extra processerver (**ScaleOut surrogaatbestand**) is geregistreerd bij de *ConfigurationServer*
- Accounts (**vCenter_account**, **WindowsAccount**, **LinuxAccount**) zijn ingesteld op de configuratieserver. Deze accounts worden gebruikt voor het toevoegen van de vCenter-server voor het detecteren van virtuele machines en voor push-installatie van de software voor de mobility-service op Windows- en Linux-servers die moeten worden gerepliceerd.

1. De van de geregistreerde configuratieservers worden vertegenwoordigd door een fabric-object in Site Recovery. De lijst met fabric objecten ophalen in de kluis en identificeren van de configuratieserver.

   ```azurepowershell
   # Verify that the Configuration server is successfully registered to the vault
   $ASRFabrics = Get-ASRFabric
   $ASRFabrics.count
   ```
   ```
   1
   ```
   ```azurepowershell
   #Print details of the Configuration Server
   $ASRFabrics[0]
   ```
   ```
   Name                  : 2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   FriendlyName          : ConfigurationServer
   ID                    : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationFabrics
                           /2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   Type                  : Microsoft.RecoveryServices/vaults/replicationFabrics
   FabricType            : VMware
   SiteIdentifier        : ef7a1580-f356-4a00-aa30-7bf80f952510
   FabricSpecificDetails : Microsoft.Azure.Commands.RecoveryServices.SiteRecovery.ASRVMWareSpecificDetails
   ```

2. De processervers die kunnen worden gebruikt voor replicatie van machines identificeren.

   ```azurepowershell
   $ProcessServers = $ASRFabrics[0].FabricSpecificDetails.ProcessServers
   for($i=0; $i -lt $ProcessServers.count; $i++) {
    "{0,-5} {1}" -f $i, $ProcessServers[$i].FriendlyName
   }
   ```
   ```
   0     ScaleOut-ProcessServer
   1     ConfigurationServer
   ```

   Uit de bovenstaande uitvoer ***$ProcessServers [0]*** komt overeen met *ScaleOut surrogaatbestand* en ***$ProcessServers [1]*** komt overeen met de proces-serverrol op *ConfigurationServer*

3. Identificeer de accounts die zijn ingesteld op de configuratieserver.

   ```azurepowershell
   $AccountHandles = $ASRFabrics[0].FabricSpecificDetails.RunAsAccounts
   #Print the account details
   $AccountHandles
   ```
   ```
   AccountId AccountName
   --------- -----------
   1         vCenter_account
   2         WindowsAccount
   3         LinuxAccount
   ```

   Uit de bovenstaande uitvoer ***$AccountHandles [0]*** komt overeen met het account *vCenter_account*, ***$AccountHandles [1]*** account *WindowsAccount*, en ***$AccountHandles [2]*** account *LinuxAccount*

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

In deze stap zijn twee beleidsregels voor replicatie gemaakt. Failover van één beleid virtuele VMware-machines repliceren naar Azure en de andere voor replicatie van virtuele machines worden uitgevoerd in Azure weer naar de lokale VMware-site.

> [!NOTE]
> De meeste Azure Site Recovery-bewerkingen worden asynchroon uitgevoerd. Wanneer u een bewerking initieert, wordt een Azure Site Recovery-taak wordt verzonden en wordt een taak voor het bijhouden van object wordt geretourneerd. Deze taak voor het bijhouden van object kan worden gebruikt voor het bewaken van de status van de bewerking.

1. Maak een replicatiebeleid voor met de naam *ReplicationPolicy* virtuele VMware-machines repliceren naar Azure met de opgegeven eigenschappen.

   ```azurepowershell
   $Job_PolicyCreate = New-ASRPolicy -VMwareToAzure -Name "ReplicationPolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60

   # Track Job status to check for completion
   while (($Job_PolicyCreate.State -eq "InProgress") -or ($Job_PolicyCreate.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_PolicyCreate = Get-ASRJob -Job $Job_PolicyCreate
   }

   #Display job status
   $Job_PolicyCreate
   ```
   ```
   Name             : 8d18e2d9-479f-430d-b76b-6bc7eb2d0b3e
   ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/8d18e2d
                      9-479f-430d-b76b-6bc7eb2d0b3e
   Type             :
   JobType          : AddProtectionProfile
   DisplayName      : Create replication policy
   ClientRequestId  : a162b233-55d7-4852-abac-3d595a1faac2 ActivityId: 9895234a-90ea-4c1a-83b5-1f2c6586252a
   State            : Succeeded
   StateDescription : Completed
   StartTime        : 11/24/2017 2:49:24 AM
   EndTime          : 11/24/2017 2:49:23 AM
   TargetObjectId   : ab31026e-4866-5440-969a-8ebcb13a372f
   TargetObjectType : ProtectionProfile
   TargetObjectName : ReplicationPolicy
   AllowedActions   :
   Tasks            : {Prerequisites check for creating the replication policy, Creating the replication policy}
   Errors           : {}
   ```

2. Maak een beleid voor wachtwoordreplicatie wilt gebruiken voor failback vanuit Azure naar de lokale VMware-site.

   ```azurepowershell
   $Job_FailbackPolicyCreate = New-ASRPolicy -AzureToVMware -Name "ReplicationPolicy-Failback" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60
   ```

   Gebruik de taakgegevens in *$Job_FailbackPolicyCreate* voor het bijhouden van de bewerking te voltooien.

   * Maak een toewijzing van de container beveiliging als u wilt toewijzen, beleidsregels voor replicatie met de configuratieserver.

   ```azurepowershell
   #Get the protection container corresponding to the Configuration Server
   $ProtectionContainer = Get-ASRProtectionContainer -Fabric $ASRFabrics[0]

   #Get the replication policies to map by name.
   $ReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy"
   $FailbackReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy-Failback"

   # Associate the replication policies to the protection container corresponding to the Configuration Server. 

   $Job_AssociatePolicy = New-ASRProtectionContainerMapping -Name "PolicyAssociation1" -PrimaryProtectionContainer $ProtectionContainer -Policy $ReplicationPolicy

   # Check the job status
   while (($Job_AssociatePolicy.State -eq "InProgress") -or ($Job_AssociatePolicy.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_AssociatePolicy = Get-ASRJob -Job $Job_AssociatePolicy
   }
   $Job_AssociatePolicy.State

   <# In the protection container mapping used for failback (replicating failed over virtual machines 
      running in Azure, to the primary VMware site.) the protection container corresponding to the 
      Configuration server acts as both the Primary protection container and the recovery protection
      container
   #>
    $Job_AssociateFailbackPolicy = New-ASRProtectionContainerMapping -Name "FailbackPolicyAssociation" -PrimaryProtectionContainer $ProtectionContainer -RecoveryProtectionContainer $ProtectionContainer -Policy $FailbackReplicationPolicy

   # Check the job status
   while (($Job_AssociateFailbackPolicy.State -eq "InProgress") -or ($Job_AssociateFailbackPolicy.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_AssociateFailbackPolicy = Get-ASRJob -Job $Job_AssociateFailbackPolicy
   }
   $Job_AssociateFailbackPolicy.State

   ```

## <a name="add-a-vcenter-server-and-discover-vms"></a>Een vCenter-server toevoegen en virtuele machines te detecteren

Voeg een vCenter-Server op IP-adres of hostnaam. De **-poort** parameter wordt de poort op de vCenter-server verbinding maken met, **-naam** parameter geeft u een beschrijvende naam moet worden gebruikt voor de vCenter-server en de **-Account** parameter geeft u de account-ingang op de configuratieserver moet worden gebruikt voor het detecteren van virtuele machines die worden beheerd door de vCenter-server.

```azurepowershell
# The $AccountHandles[0] variable holds details of vCenter_account

$Job_AddvCenterServer = New-ASRvCenter -Fabric $ASRFabrics[0] -Name "MyvCenterServer" -IpOrHostName "10.150.24.63" -Account $AccountHandles[0] -Port 443

#Wait for the job to complete and ensure it completed successfully

while (($Job_AddvCenterServer.State -eq "InProgress") -or ($Job_AddvCenterServer.State -eq "NotStarted")) { 
        sleep 30; 
        $Job_AddvCenterServer = Get-ASRJob -Job $Job_AddvCenterServer
}
$Job_AddvCenterServer
```
```
Name             : 0f76f937-f9cf-4e0e-bf27-10c9d1c252a4
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/0f76f93
                   7-f9cf-4e0e-bf27-10c9d1c252a4
Type             :
JobType          : DiscoverVCenter
DisplayName      : Add vCenter server
ClientRequestId  : a2af8892-5686-4d64-a528-10445bc2f698 ActivityId: 7ec05aad-002e-4da0-991f-95d0de7a9f3a
State            : Succeeded
StateDescription : Completed
StartTime        : 11/24/2017 2:41:47 AM
EndTime          : 11/24/2017 2:44:37 AM
TargetObjectId   : 10.150.24.63
TargetObjectType : VCenter
TargetObjectName : MyvCenterServer
AllowedActions   :
Tasks            : {Adding vCenter server}
Errors           : {}
```

## <a name="create-storage-accounts-for-replication"></a>Storage-accounts voor replicatie maken

In deze stap worden moet worden gebruikt voor replicatie de storage-accounts gemaakt. Deze opslagaccounts worden later gebruikt voor het repliceren van virtuele machines. Zorg ervoor dat de storage-accounts worden gemaakt in dezelfde Azure-regio als de kluis. U kunt deze stap overslaan als u wilt een bestaand opslagaccount gebruiken voor replicatie.

> [!NOTE]
> Tijdens het on-premises virtuele machines repliceren naar een premium storage-account, moet u een extra standard-opslagaccount (logboek storage-account) opgeven. Het opslagaccount van het logboek bevat replicatielogboeken als een tussenliggende archief totdat de logboeken kunnen worden toegepast op het doel van de premium-opslag.
>

```azurepowershell

$PremiumStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "premiumstorageaccount1" -Location "East Asia" -SkuName Premium_LRS

$LogStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "logstorageaccount1" -Location "East Asia" -SkuName Standard_LRS

$ReplicationStdStorageAccount= New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "replicationstdstorageaccount1" -Location "East Asia" -SkuName Standard_LRS
```

## <a name="replicate-vmware-vms"></a>Virtuele VMware-machines repliceren

Het duurt ongeveer 15-20 minuten voor virtuele machines moeten worden gedetecteerd van de vCenter-server. Wanneer wordt gedetecteerd, wordt een object van het beveiligbare object gemaakt in Azure Site Recovery voor elke gedetecteerde virtuele machine. In deze stap worden drie van de gedetecteerde virtuele machines gerepliceerd naar de Azure Storage-accounts in de vorige stap hebt gemaakt.

U moet de volgende informatie om een gedetecteerde virtuele machine te beveiligen:

* De beveiligbare item worden gerepliceerd.
* Het opslagaccount voor replicatie van de virtuele machine. Bovendien is een logboek-opslag vereist voor het beveiligen van virtuele machines naar een premium-opslagaccount.
* De processerver moet worden gebruikt voor replicatie. De lijst met beschikbare processervers zijn opgehaald en opgeslagen in de ***$ProcessServers [0]****(ScaleOut surrogaatbestand)* en ***$ProcessServers [1]*** *(ConfigurationServer)* variabelen.
* Het account moet worden gebruikt voor de software voor de Mobility-service op de machines push-installatie. De lijst met beschikbare accounts is opgehaald en opgeslagen in de ***$AccountHandles*** variabele.
* De toewijzing van de container beveiliging voor het replicatiebeleid moet worden gebruikt voor replicatie.
* De resourcegroep waarin virtuele machines op failover moet worden gemaakt.
* Optioneel, de virtuele Azure-netwerk en het subnet waarop de mislukte via de virtuele machine moet worden verbonden.

Nu de volgende virtuele machines met behulp van de instellingen die zijn opgegeven in deze tabel repliceren


|Virtuele machine  |Processerver        |Opslagaccount              |Meld u Storage-account  |Beleid           |Account voor de installatie van de Mobility-service|Doelresourcegroep  | Doel virtueel netwerk  |Doelsubnet  |
|-----------------|----------------------|-----------------------------|---------------------|-----------------|-----------------------------------------|-----------------------|-------------------------|---------------|
|Win2K12VM1       |ScaleOut-ProcessServer|premiumstorageaccount1       |logstorageaccount1   |ReplicationPolicy|WindowsAccount                           |VMwareDRToAzurePs      |ASR-vnet                 |Subnet-1       |
|CentOSVM1       |ConfigurationServer   |replicationstdstorageaccount1| N/A                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR-vnet                 |Subnet-1       |   
|CentOSVM2       |ConfigurationServer   |replicationstdstorageaccount1| N/A                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR-vnet                 |Subnet-1       |   

 
```azurepowershell

#Get the target resource group to be used
$ResourceGroup = Get-AzureRmResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzureRmVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap  = Get-ASRProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine Win2K12VM1
$VM1 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "Win2K12VM1"

# Enable replication for virtual machine Win2K12VM1
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableRepication1 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $PremiumStorageAccount.Id -LogStorageAccountId $LogStorageAccount.Id -ProcessServer $ProcessServers[0] -Account $AccountHandles[1] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM2 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1
$Job_EnableRepication2 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM2 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine CentOSVM2
$VM3 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM2"

# Enable replication for virtual machine CentOSVM2
$Job_EnableRepication3 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM3 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

```

Zodra de taak van de replicatie inschakelen voltooid is, wordt de initiële replicatie gestart voor de virtuele machines. Initiële replicatie kan duren, afhankelijk van de hoeveelheid gegevens worden gerepliceerd en de bandbreedte die beschikbaar voor replicatie. Nadat de initiële replicatie is voltooid, wordt de virtuele machine verplaatst naar een beveiligde status. Zodra de virtuele machine bereikt een beveiligde status die u kunt uitvoeren met een testfailover voor de virtuele machine, deze toevoegen aan herstelplannen enzovoort.

U kunt de replicatiestatus en de replicatiestatus van de virtuele machine met de cmdlet Get-ASRReplicationProtectedItem controleren.

```azurepowershell
Get-ASRReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState                 ReplicationHealth
------------ ---------------                 -----------------
CentOSVM1    Protected                       Normal
CentOSVM2    InitialReplicationInProgress    Normal
Win2K12VM1   Protected                       Normal
```

## <a name="configure-failover-settings"></a>Failover-instellingen configureren

Failover-instellingen voor beveiligde machines kunnen worden bijgewerkt met de cmdlet Set-ASRReplicationProtectedItem. Enkele van de instellingen die kunnen worden bijgewerkt via deze cmdlet zijn:
* Naam van de virtuele machine wilt maken op failover
* VM-grootte van de virtuele machine wilt maken op failover
* Virtuele Azure-netwerk en subnet die de NIC's van de virtuele machine moeten worden verbonden met failover
* Failover naar beheerde schijven
* Azure hybride gebruik voordeel toepassen
* Wijs een statisch IP-adres van het virtuele doelnetwerk moet worden toegewezen aan de virtuele machine op failover.

In dit voorbeeld gaan we de VM-grootte van de virtuele machine wilt maken op failover voor de virtuele machine bijwerken *Win2K12VM1* en opgeven dat wordt beheerd door het gebruik van de virtuele machine schijven op failover.

```azurepowershell
$ReplicatedVM1 = Get-ASRReplicationProtectedItem -FriendlyName "Win2K12VM1" -ProtectionContainer $ProtectionContainer

Set-ASRReplicationProtectedItem -InputObject $ReplicatedVM1 -Size "Standard_DS11" -UseManagedDisk True
```
```
Name             : cafa459c-44a7-45b0-9de9-3d925b0e7db9
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/cafa459
                   c-44a7-45b0-9de9-3d925b0e7db9
Type             :
JobType          : UpdateVmProperties
DisplayName      : Update the virtual machine
ClientRequestId  : b0b51b2a-f151-4e9a-a98e-064a5b5131f3 ActivityId: ac2ba316-be7b-4c94-a053-5363f683d38f
State            : InProgress
StateDescription : InProgress
StartTime        : 11/24/2017 2:04:26 PM
EndTime          :
TargetObjectId   : 88bc391e-d091-11e7-9484-000c2955bb50
TargetObjectType : ProtectionEntity
TargetObjectName : Win2K12VM1
AllowedActions   :
Tasks            : {Update the virtual machine properties}
Errors           : {}
```

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. Voer een DR-herstelanalyse (testfailover) als volgt:

   ```azurepowershell
   #Test failover of Win2K12VM1 to the test virtual network "V2TestNetwork"

   #Get details of the test failover virtual network to be used
   TestFailovervnet = Get-AzureRmVirtualNetwork -Name "V2TestNetwork" -ResourceGroupName "asrrg" 

   #Start the test failover operation
   $TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -AzureVMNetworkId $TestFailovervnet.Id -Direction PrimaryToRecovery
   ```
2. Zodra de test failover-taak voltooid is, ziet u dat een virtuele machine moet worden voorafgegaan door *'-Test "* (Win2K12VM1-Test in dit geval) de naam is gemaakt in Azure.
3. U kunt nu verbinding maken met de virtuele machine failover testen en valideren van de testfailover.
4. De testfailover met de cmdlet Start-ASRTestFailoverCleanupJob opschonen. Deze bewerking worden verwijderd van de virtuele machine gemaakt als onderdeel van de testfailover.

   ```azurepowershell
   $Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicatedVM1
   ```

## <a name="fail-over-to-azure"></a>Failover naar Azure

In deze stap failover wordt de virtuele machine Win2K12VM1 een specifiek herstelpunt.

1. Een lijst met beschikbare herstelpunten te gebruiken voor de failover ophalen:
   ```azurepowershell
   # Get the list of available recovery points for Win2K12VM1
   $RecoveryPoints = Get-ASRRecoveryPoint -ReplicationProtectedItem $ReplicatedVM1
   "{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[0].RecoveryPointTime
   ```
   ```
   CrashConsistent 11/24/2017 5:28:25 PM
   ```
   ```azurepowershell

   #Start the failover job
   $Job_Failover = Start-ASRUnplannedFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[0]
   do {
           $Job_Failover = Get-ASRJob -Job $Job_Failover;
           sleep 60;
   } while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))
   $Job_Failover.State
   ```
   ```
   Succeeded
   ```

2. Zodra de failover is, u kunt de failoverbewerking doorvoeren en stelt u de omgekeerde replicatie van Azure weer naar de lokale VMware site.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het automatiseren van meer taken met behulp van de [Azure Site Recovery PowerShell-referentie ](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery).
