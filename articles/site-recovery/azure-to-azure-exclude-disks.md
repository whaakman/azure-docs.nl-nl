---
title: Azure Site Recovery - uitsluiten schijf tijdens de replicatie van virtuele machines van Azure met behulp van Azure PowerShell | Microsoft Docs
description: Informatie over het uitsluiten van schijven voor virtuele machines van Azure met Azure Site Recovery met behulp van Azure PowerShell.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 978566eb9e0073c60046eca024e09ba63c642180
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56458012"
---
# <a name="exclude-disks-from-replication-of-azure-vms-to-azure-using-azure-powershell"></a>Schijven uitsluiten van replicatie van virtuele Azure-machines naar Azure met behulp van Azure PowerShell

In dit artikel wordt beschreven hoe u schijven uitsluiten bij het repliceren van virtuele Azure-machines. Door schijven uit te sluiten, kunt u de verbruikte replicatiebandbreedte optimaliseren of de resources aan de doelzijde waarvan deze schijven gebruikmaken, optimaliseren. Momenteel wordt deze mogelijkheid alleen weergegeven via Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Raadpleeg de [ondersteuningsvereisten](azure-to-azure-support-matrix.md) voor alle onderdelen.
- U beschikt over versie 5.7.0 of hoger van de AzureRm PowerShell-module. Als u wilt installeren of upgraden van Azure PowerShell, volgt u deze [handleiding voor het installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).
- U al een Recovery services-kluis hebt gemaakt en beveiliging van virtuele machines ten minste één keer hebt gedaan. Als geen vervolgens dit doen met behulp van de documentatie genoemde [hier](azure-to-azure-powershell.md) 

## <a name="why-exclude-disks-from-replication"></a>Waarom is het nodig om schijven uit te sluiten van replicatie?
Schijven uitsluiten van replicatie is vaak nodig, omdat:

- Uw virtuele machine heeft bereikt [tarieven van Azure Site Recovery-limieten voor het repliceren van gegevens gewijzigd](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#azure-site-recovery-limits-to-replicate-data-change-rates)

- Het gegevensverloop op de uitgesloten schijf niet belangrijk is of niet hoeft te worden gerepliceerd.

- Opslag- en netwerkresources kunnen worden uitgespaard door dit verloop niet te repliceren.


## <a name="how-to-exclude-disks-from-replication"></a>Hoe sluit ik schijven uit van replicatie?

In het voorbeeld in dit artikel wordt een virtuele machine met 1 OS en 3 gegevensschijven in de regio VS-Oost moeten worden gerepliceerd naar de regio VS-West 2. De naam van de virtuele machine die in het voorbeeld is AzureDemoVM. en we worden uitgesloten schijf 1 en 2 en 3 schijven behouden

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Details van de virtuele machines worden gerepliceerd ophalen

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

## <a name="replicate-azure-virtual-machine"></a>Virtuele Azure-machines repliceren

In het onderstaande voorbeeld hebben we uitgegaan dat u al een cache-opslagaccount, beleid voor replicatie en -toewijzingen hebben. Als geen vervolgens dit doen met behulp van de documentatie genoemde [hier](azure-to-azure-powershell.md) 


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

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded so we will provide it during the time of replication 

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Zodra de begin-replicatiebewerking is gelukt, worden gegevens van virtuele machines wordt gerepliceerd naar de herstelregio voor.

Kunt u naar de Azure-portal gaan en onder gerepliceerde items ziet u de virtuele machines die worden gerepliceerd ophalen.
Het replicatieproces wordt gestart door een kopie van de replicerende schijven van de virtuele machine in de herstelregio in eerste instantie seeding. Deze fase wordt de fase van de initiële replicatie genoemd.

Nadat de eerste replicatie is voltooid, wordt replicatie naar de differentiële synchronisatiefase verplaatst. Op dit moment is de virtuele machine beveiligd. Klik op de beveiligde virtuele machine > schijven om te zien die als de schijf is uitgesloten of niet.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.