---
title: Gebruik van een Windows VM maken met Azure PowerShell probleemoplossing | Microsoft Docs
description: Informatie over het oplossen van problemen met de Windows-VM in Azure door verbinding te maken van de besturingssysteemschijf aan een virtuele machine met behulp van Azure PowerShell voor herstel
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: 27409de144274cde4201937c47df0fd2bbfd788a
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984440"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine met behulp van Azure PowerShell voor herstel
Als uw Windows virtuele machine (VM) in Azure een opstart- of schijffout-fout optreedt, moet u mogelijk de stappen voor probleemoplossing uitvoeren op de schijf zelf. Een veelvoorkomend voorbeeld is een mislukte toepassingsupdate die verhindert de virtuele machine dat wordt het opstarten. Dit artikel wordt uitgelegd hoe u verbinding maken met de schijf naar een andere Windows-virtuele machine om eventuele fouten te corrigeren en vervolgens de oorspronkelijke virtuele machine herstellen met Azure PowerShell. 

> [!Important]
> De scripts in dit artikel zijn alleen van toepassing op de virtuele machines die gebruikmaken van [beheerde schijf](../windows/managed-disks-overview.md). 

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
We kunnen nu Azure PowerShell gebruiken voor het wijzigen van de besturingssysteemschijf voor een virtuele machine. We hoeft niet meer te verwijderen en opnieuw maken van de virtuele machine.

Het probleemoplossingsproces is als volgt:

1. De betrokken virtuele machine stoppen.
2. Een momentopname maken van de Besturingssysteemschijf van de virtuele machine.
3. Maak een schijf van de momentopname van de OS-schijf.
4. De schijf als een gegevensschijf koppelen aan een virtuele machine voor herstel.
5. Verbinding maken met de virtuele machine voor herstel. Bewerk bestanden of alle hulpmiddelen voor het oplossen van problemen op de gekopieerde schijf met het besturingssysteem worden uitgevoerd.
6. Ontkoppel en schijf loskoppelen van de virtuele machine voor herstel.
7. De besturingssysteemschijf voor de betreffende virtuele machine wijzigen.

U kunt de scripts voor het herstel van virtuele machine gebruiken voor het automatiseren van stap 1, 2, 3, 4, 6 en 7. Zie voor meer informatie over documentatie en instructies [Scripts voor het herstel van virtuele machine voor Resource Manager-VM](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager).

Zorg ervoor dat u hebt [de nieuwste Azure PowerShell](/powershell/azure/overview) geïnstalleerd en aangemeld bij uw abonnement:

```powershell
Connect-AzAccount
```

In de volgende voorbeelden kunt u de parameternamen van de vervangen door uw eigen waarden. 

## <a name="determine-boot-issues"></a>Opstartproblemen met bepalen
U kunt een schermopname van de virtuele machine in Azure om op te lossen opstartproblemen met weergeven. Deze schermopname kunt identificeren waarom een virtuele machine is mislukt om op te starten. Het volgende voorbeeld wordt de schermafbeelding van de Windows-VM met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Bekijk de schermafbeelding om te bepalen waarom de virtuele machine is mislukt om op te starten. Houd er rekening mee specifieke foutberichten of foutcodes die zijn opgegeven.

## <a name="stop-the-vm"></a>De virtuele machine stoppen

Het volgende voorbeeld wordt de VM met de naam gestopt `myVM` uit de resourcegroep met de naam `myResourceGroup`:

```powershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Wacht totdat de virtuele machine verwijderd is voordat u met de volgende stap verwerkt.


## <a name="create-a-snapshot-from-the-os-disk-of-the-vm"></a>Een momentopname maken van de Besturingssysteemschijf van de virtuele machine

Het volgende voorbeeld wordt een momentopname met de naam `mySnapshot` van het besturingssysteem schijf van de virtuele machine met de naam 'myVM'. 

```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  

#Get the VM
$vm = get-azvm `
-ResourceGroupName $resourceGroupName `
-Name $vmName

#Create the snapshot configuration for the OS disk
$snapshot =  New-AzSnapshotConfig `
-SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
-Location $location `
-CreateOption copy

#Take the snapshot
New-AzSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```

Een momentopname is een volledige, alleen-lezen kopie van een VHD. Het kan niet worden gekoppeld aan een virtuele machine. In de volgende stap maken we een schijf van deze momentopname.

## <a name="create-a-disk-from-the-snapshot"></a>Maak een schijf van de momentopname

Dit script maakt u een beheerde schijf met de naam `newOSDisk` vanuit de momentopname met de naam `mysnapshot`.  

```powershell
#Set the context to the subscription Id where Managed Disk will be created
#You can skip this step if the subscription is already selected

$subscriptionId = 'yourSubscriptionId'

Select-AzSubscription -SubscriptionId $SubscriptionId

#Provide the name of your resource group
$resourceGroupName ='myResourceGroup'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'mySnapshot' 

#Provide the name of the Managed Disk
$diskName = 'newOSDisk'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. PremiumLRS or StandardLRS.
$storageType = 'StandardLRS'

#Provide the Azure region (e.g. westus) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'eastus'

$snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
 
$diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
 
New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
```
U hebt nu een kopie van de schijf met het oorspronkelijke besturingssysteem. U kunt deze schijf aan een andere Windows-virtuele machine koppelen voor het oplossen van problemen.

## <a name="attach-the-disk-to-another-windows-vm-for-troubleshooting"></a>De schijf koppelen aan een andere Windows-VM voor probleemoplossing

Nu koppelen we de kopie van de schijf met het oorspronkelijke besturingssysteem aan een virtuele machine als gegevensschijf. Dit proces kunt u voor het corrigeren van fouten in de configuratie of extra toepassing of een systeem-logboekbestanden op de schijf controleren. Het volgende voorbeeld wordt de schijf met de naam `newOSDisk` aan de virtuele machine met de naam `RecoveryVM`.

> [!NOTE]
> Als u wilt de schijf koppelen, moet de kopie van de schijf met het oorspronkelijke besturingssysteem en de virtuele machine voor herstel op dezelfde locatie.

```powershell
$rgName = "myResourceGroup"
$vmName = "RecoveryVM"
$location = "eastus" 
$dataDiskName = "newOSDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="connect-to-the-recovery-vm-and-fix-issues-on-the-attached-disk"></a>Verbinding maken met de virtuele machine voor herstel en los problemen op de gekoppelde schijf

1. RDP-verbinding de herstel-VM met behulp van de juiste referenties. In het volgende voorbeeld wordt het RDP-verbinding-bestand gedownload voor de virtuele machine met de naam `RecoveryVM` in de resourcegroep met de naam `myResourceGroup`, en downloadt deze `C:\Users\ops\Documents`"

    ```powershell
    Get-AzRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "RecoveryVM" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. De gegevensschijf moet automatisch worden gedetecteerd en die is gekoppeld. De lijst met gekoppelde volumes om te bepalen van de stationsletter als volgt bekijken:

    ```powershell
    Get-Disk
    ```

    De volgende voorbeelduitvoer ziet u de schijf een schijf verbonden **2**. (U kunt ook `Get-Volume` om weer te geven van de stationsletter):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        newOSDisk                                  Healthy             Online       127 GB MBR
    ```

Nadat de kopie van de oorspronkelijke OS-schijf is gekoppeld, kunt u onderhouds- en stappen voor probleemoplossing naar behoefte kunt uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.

## <a name="unmount-and-detach-original-os-disk"></a>Oorspronkelijke OS-schijf loskoppelen
Zodra de fouten opgelost zijn, kunt u ontkoppelen en de bestaande schijf loskoppelen van uw virtuele machine voor herstel. U kunt de schijf met andere VM's niet gebruiken totdat de lease die de schijf koppelen aan de virtuele machine voor herstel is uitgebracht.

1. Ontkoppel de gegevensschijf binnen uw RDP-sessie op de virtuele machine voor herstel. U moet het nummer van de schijf van de vorige `Get-Disk` cmdlet. Vervolgens gebruikt u `Set-Disk` om in te stellen van de schijf als offline:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Controleer of de schijf is nu ingesteld als het gebruik van offline `Get-Disk` opnieuw. De volgende voorbeelduitvoer ziet u dat de schijf is nu ingesteld als offline:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Sluit uw RDP-sessie. Vanuit de Azure PowerShell-sessie, verwijdert u de schijf met de naam `newOSDisk` van de virtuele machine met de naam 'RecoveryVM'.

    ```powershell
    $myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "RecoveryVM"
    Remove-AzVMDataDisk -VM $myVM -Name "newOSDisk"
    Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>De besturingssysteemschijf voor de betreffende virtuele machine wijzigen

U kunt Azure PowerShell gebruiken om te vervangen door de OS-schijven. U hoeft niet te verwijderen en opnieuw maken van de virtuele machine.

In dit voorbeeld wordt de VM met de naam gestopt `myVM` en wijst de schijf met de naam `newOSDisk` als een schijf met het nieuwe besturingssysteem. 

```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name  -sto

# Update the VM with the new OS disk. Possible values of StorageAccountType include: 'Standard_LRS' and 'Premium_LRS'
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -StorageAccountType <Type of the storage account >

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```

## <a name="verify-and-enable-boot-diagnostics"></a>Controleer en diagnostische gegevens over opstarten inschakelen

Het volgende voorbeeld wordt de diagnostische extensie op de virtuele machine met de naam `myVMDeployed` in de resourcegroep met de naam `myResourceGroup`:

```powershell
$myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, Zie [oplossen RDP-verbindingen met een Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zie voor problemen met toegang tot toepassingen die worden uitgevoerd op de virtuele machine, [verbindingsproblemen van toepassing op een Windows-VM oplossen](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
