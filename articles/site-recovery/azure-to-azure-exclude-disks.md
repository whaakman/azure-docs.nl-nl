---
title: Azure Site Recovery - schijven uitsluiten tijdens de replicatie van virtuele Azure-machines met behulp van Azure PowerShell | Microsoft Docs
description: Informatie over het uitsluiten van schijven van virtuele machines van Azure tijdens de Azure Site Recovery met behulp van Azure PowerShell.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 54a32d7f7aa4bcab73f5828da3e7eba9d25276be
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160299"
---
# <a name="exclude-disks-from-powershell-replication-of-azure-vms"></a>Schijven uitsluiten van replicatie van virtuele machines van Azure PowerShell

In dit artikel wordt beschreven hoe u schijven uitsluiten wanneer u virtuele Azure-machines repliceren. U kunt schijven voor het optimaliseren van de verbruikte replicatiebandbreedte of de resources aan die gebruikmaken van deze schijven uitsluiten. Deze mogelijkheid is momenteel alleen beschikbaar via Azure PowerShell.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat u begrijpt de [herstel na noodgevallen architectuur en onderdelen](azure-to-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](azure-to-azure-support-matrix.md) voor alle onderdelen.
- Zorg ervoor dat u AzureRm PowerShell 'Az' module. Als u wilt installeren of bijwerken van PowerShell, Zie [installeren van de Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Zorg ervoor dat u hebt gemaakt van een recovery services-kluis en virtuele machines ten minste één keer beveiligde. Als u dit allemaal nog niet hebt gedaan, gaat u als volgt het proces op [instellen van herstel na noodgevallen voor Azure-machines met behulp van Azure PowerShell](azure-to-azure-powershell.md).

## <a name="why-exclude-disks-from-replication"></a>Waarom schijven uitsluiten van replicatie
U moet mogelijk schijven uitsluiten van replicatie, omdat:

- Uw virtuele machine heeft bereikt [tarieven van Azure Site Recovery-limieten voor het repliceren van gegevens gewijzigd](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix).

- De gegevens die gegevensverloop op de uitgesloten schijf niet belangrijk is of niet hoeft te worden gerepliceerd.

- Wilt u opslag- en netwerkresources opslaan door de gegevens niet te repliceren.

## <a name="how-to-exclude-disks-from-replication"></a>Schijven uitsluiten van replicatie

In ons voorbeeld repliceren we een virtuele machine waarvoor een besturingssysteem en de drie gegevensschijven die zich in de regio VS-Oost naar de regio VS-West 2. De naam van de virtuele machine is *AzureDemoVM*. We houden schijven 2 en 3 en schijf 1 uitsluiten.

## <a name="get-details-of-the-virtual-machines-to-replicate"></a>Details van de virtuele machines te repliceren ophalen

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

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

Meer informatie over de schijven van de virtuele machine. Deze gegevens worden later gebruikt bij het starten van replicatie van de virtuele machine.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-an-azure-virtual-machine"></a>Een Azure-machines repliceren

Voor het volgende voorbeeld veronderstellen we dat u al een cache-opslagaccount, beleid voor replicatie en -toewijzingen. Als u dit allemaal hebt, volgt u het proces op [instellen van herstel na noodgevallen voor Azure-machines met behulp van Azure PowerShell](azure-to-azure-powershell.md).

Een Azure-machine met repliceren *beheerde schijven*.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration).

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded, so we will provide it during the time of replication. 

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating a replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Wanneer de begin-replicatiebewerking is gelukt, wordt de VM-gegevens gerepliceerd naar de herstelregio voor.

U kunt gaat u naar de Azure-portal en de gerepliceerde VM's onder "gerepliceerde items."

Het replicatieproces wordt gestart door een kopie van de replicerende schijven van de virtuele machine in de herstelregio seeding. Deze fase wordt de fase van de initiële replicatie genoemd.

Nadat de initiële replicatie is voltooid, wordt de status van replicatie verplaatst op met de fase differentiële-synchronisatie. Op dit moment is de virtuele machine beveiligd. Selecteer de beveiligde virtuele machine om te zien als alle schijven die zijn uitgesloten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [uitvoeren van een testfailover](site-recovery-test-failover-to-azure.md).
