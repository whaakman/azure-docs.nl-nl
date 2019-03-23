---
title: Verschillen en overwegingen voor Managed Disks in Azure Stack | Microsoft Docs
description: Meer informatie over de verschillen en overwegingen bij het werken met beheerde schijven in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 02/26/2019
ms.openlocfilehash: 28210048cd007fc10dcd4cf5e92577cbd121e2a3
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368269"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack-beheerde schijven: de verschillen en overwegingen met betrekking tot

In dit artikel bevat een overzicht van de bekende verschillen tussen [Azure Stack Managed Disks](azure-stack-manage-vm-disks.md) en [Managed Disks voor Azure](../../virtual-machines/windows/managed-disks-overview.md). Zie voor meer informatie over belangrijke verschillen tussen Azure Stack en Azure, de [belangrijke overwegingen met betrekking tot](azure-stack-considerations.md) artikel.

Beheerde schijven vereenvoudigt u Schijfbeheer voor IaaS-VM's door het beheer van de [opslagaccounts](../azure-stack-manage-storage-accounts.md) die zijn gekoppeld aan de VM-schijven.

> [!Note]  
> Beheerde schijven in Azure Stack is beschikbaar via 1808 update. Dit is standaard ingeschakeld tijdens het maken van virtuele machines met behulp van de Azure Stack-portal van 1811 update.
  
## <a name="cheat-sheet-managed-disk-differences"></a>Overzichtskaart: Beheerde schijf verschillen

| Functie | Azure (wereldwijd) | Azure Stack |
| --- | --- | --- |
|Versleuteling van Data-at-Rest |Azure Storage Service Encryption (SSE), Azure Disk Encryption (ADE)     |BitLocker-128-bits AES-versleuteling      |
|Installatiekopie          | Ondersteuning voor beheerde aangepaste installatiekopie |Ondersteund|
|Opties voor back-up |Ondersteuning voor Azure Backup-Service |Nog niet ondersteund |
|Opties voor herstel na noodgevallen |Ondersteuning voor Azure Site Recovery |Nog niet ondersteund|
|Schijftypen     |Premium SSD, Standard-SSD (Preview) en Standard HDD |Premium SSD, Standard HDD |
|Premium-schijven  |Volledig ondersteund |Kan worden ingericht, maar er is geen prestatielimiet voor of garanderen  |
|Premium-schijven IOPs  |Afhankelijk van de schijfgrootte  |2300 IOP's per schijf |
|Doorvoer van Premium-schijven |Afhankelijk van de schijfgrootte |145 MB/s per schijf |
|Schijfgrootte  |Azure Premium Disk: P4 (32 GiB) naar P80 (32 TiB)<br>Azure Standard SSD Disk: E10 (128 GiB) naar E80 (32 TiB)<br>Azure Standard HDD Disk: S4 (32 GiB) naar S80 (32 TiB) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 GiB<br>M20: 512 GiB<br>M30: 1024 GiB |
|Exemplaar van de momentopname schijven|Momentopname maken van Azure beheerde schijven zijn gekoppeld aan een actieve virtuele machine die wordt ondersteund|Nog niet ondersteund |
|Schijven prestaties analytische |Cumulatieve metrische gegevens en per schijf ondersteunde metrische gegevens |Nog niet ondersteund |
|Migratie      |Hulpprogramma voor het migreren van bestaande niet-beheerde Azure Resource Manager VM's zonder de noodzaak om de virtuele machine opnieuw te bieden  |Nog niet ondersteund |

> [!NOTE]  
> Beheerde schijven IOPs en doorvoer in Azure Stack is een maximum aantal in plaats van een ingerichte nummer, dat mogelijk beïnvloed door de hardware- en workloads die worden uitgevoerd in Azure Stack.

## <a name="metrics"></a>Metrische gegevens

Er zijn ook verschillen met metrische gegevens over opslag:

- Met Azure Stack, de transactiegegevens in metrische opslaggegevens geen onderscheid tussen interne of externe netwerkbandbreedte.
- Azure Stack-transactiegegevens in metrische gegevens van storage bevat geen virtuele machine toegang tot de gekoppelde schijven.

## <a name="api-versions"></a>API-versies

Azure Stack Managed Disks ondersteunt de volgende API-versies:

- 2017-03-30
- 2017-12-01

## <a name="convert-to-managed-disks"></a>Converteren naar managed disks

U kunt het volgende script een momenteel ingerichte virtuele machine niet converteren naar beheerde schijven gebruiken. Vervang de tijdelijke aanduidingen door uw eigen waarden:

```powershell
$subscriptionId = 'subid'

# The name of your resource group
$resourceGroupName ='rgmgd'

# The name of the managed disk
$diskName = 'unmgdvm'

# The size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '50'

# The URI of the VHD file that will be used to create the managed disk.
# The VHD file can be deleted as soon as the managed disk is created.
$vhdUri = 'https://rgmgddisks347.blob.local.azurestack.external/vhds/unmgdvm20181109013817.vhd' 

# The storage type for the managed disk; PremiumLRS or StandardLRS.
$accountType = 'StandardLRS'

# The Azure Stack location where the managed disk is located.
# The location should be the same as the location of the storage account in which VHD file is stored.
$location = 'local'
$virtualMachineName = 'mgdvm'
$virtualMachineSize = 'Standard_D1'
$pipname = 'unmgdvm-ip'
$virtualNetworkName = 'rgmgd-vnet'
$nicname = 'unmgdvm295'

# Set the context to the subscription ID in which the managed disk will be created
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$diskConfig = New-AzureRmDiskConfig -AccountType $accountType  -Location $location -DiskSizeGB $diskSize -SourceUri $vhdUri -CreateOption Import

# Create managed disk
New-AzureRmDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
$disk = get-azurermdisk -DiskName $diskName -ResourceGroupName $resourceGroupName
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize

# Use the managed disk resource ID to attach it to the virtual machine.
# Change the OS type to Linux if the OS disk has Linux OS.
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $disk.Id -CreateOption Attach -Linux

# Create a public IP for the VM
$publicIp = Get-AzureRmPublicIpAddress -Name $pipname -ResourceGroupName $resourceGroupName 

# Get the virtual network where the virtual machine will be hosted
$vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName

# Create NIC in the first subnet of the virtual network
$nic = Get-AzureRmNetworkInterface -Name $nicname -ResourceGroupName $resourceGroupName

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id

# Create the virtual machine with managed disk
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
```

## <a name="managed-images"></a>Beheerde installatiekopieën

Azure Stack ondersteunt *beheerde installatiekopieën*, waardoor u een beheerde installatiekopie-object maken op een gegeneraliseerde VM (zowel niet-beheerde en beheerde) die u alleen van beheerde maken kunt schijf voortaan VM's. Beheerde installatiekopieën inschakelen voor de volgende twee scenario's:

- U hebt niet-beheerde virtuele machines gegeneraliseerd en wilt gebruikmaken van beheerde schijven voortaan.
- U hebt een gegeneraliseerde beheerde virtuele machine en wilt maken van meerdere, vergelijkbare beheerde virtuele machines.

### <a name="step-1-generalize-the-vm"></a>Stap 1: De virtuele machine generaliseren
Voor Windows, volgt u de sectie 'De Windows-VM generaliseren met behulp van Sysprep' hier: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep Voor Linux, volgt u stap 1 hier: https://docs.microsoft.com/en-us/azure/virtual-machines/linux/capture-image#step-1-deprovision-the-vm 

Opmerking: Vergeet niet om te generaliseren van uw virtuele machine. Het maken van een virtuele machine van een installatiekopie die niet goed is gegeneraliseerd zal leiden tot een fout VMProvisioningTimeout.

### <a name="step-2-create-the-managed-image"></a>Stap 2: De beheerde installatiekopie maken
U kunt de portal, powershell of cli om dit te doen. Ga als volgt hier document van Azure: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource

### <a name="step-3-choose-the-use-case"></a>Stap 3: Kies de use-case:
#### <a name="case-1-migrate-unmanaged-vms-to-managed-disks"></a>Geval 1: Niet-beheerde virtuele machines migreren naar beheerde schijven
Vergeet niet om te generaliseren van uw virtuele machine goed voordat u deze stap uitvoert. Post mag generalisatie, deze virtuele machine niet meer gebruikt. Het maken van een virtuele machine van een installatiekopie die niet goed is gegeneraliseerd zal leiden tot een fout VMProvisioningTimeout. 

Volg de instructies [hier](../../virtual-machines/windows/capture-image-resource.md#create-an-image-from-a-vhd-in-a-storage-account) naar een beheerde installatiekopie maken vanaf een gegeneraliseerde VHD in een storage-account. Deze installatiekopie kan worden gebruikt om beheerde virtuele machines gaan maken.

#### <a name="case-2-create-managed-vm-from-managed-image-using-powershell"></a>Geval 2: Beheerde virtuele machine maken van beheerde installatiekopie met behulp van Powershell

Na het maken van een installatiekopie van een bestaande beheerde schijf-VM met behulp van het script [hier](../../virtual-machines/windows/capture-image-resource.md#create-an-image-from-a-managed-disk-using-powershell) , het volgende voorbeeldscript maakt u een dergelijke Linux-VM van een bestaande installatiekopie-object:

Azure Stack powershell-module 1.7.0 of hoger: Volg de instructies [hier](../../virtual-machines/windows/create-vm-generalized-managed.md) 

Azure Stack powershell-module 1.6.0 of onderstaande:

```powershell
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "redmond"
$vmName = "myVM"
$imagerg = "managedlinuxrg"
$imagename = "simplelinuxvmm-image-2019122"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

$image = get-azurermimage -ResourceGroupName $imagerg -ImageName $imagename
# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

U kunt ook de portal een virtuele machine van een beheerde installatiekopie maken. Zie voor meer informatie, de door Azure beheerde installatiekopie artikelen [maken van een beheerde installatiekopie van een gegeneraliseerde VM in Azure](../../virtual-machines/windows/capture-image-resource.md) en [een virtuele machine maken vanaf een beheerde installatiekopie](../../virtual-machines/windows/create-vm-generalized-managed.md).

## <a name="configuration"></a>Configuratie

Na het toepassen van de 1808 bijwerken of hoger, moet u de volgende configuratie voordat u Managed Disks uitvoeren:

- Als een abonnement is gemaakt vóór de update 1808, volg de onderstaande stappen voor het bijwerken van het abonnement. Anders wordt kan het implementeren van virtuele machines in dit abonnement mislukken met een foutbericht "Interne fout in Schijfbeheer."
   1. Ga in de tenantportal naar **abonnementen** en zoek het abonnement. Klik op **Resourceproviders**, klikt u vervolgens op **Microsoft.Compute**, en klik vervolgens op **opnieuw registreren**.
   2. Onder hetzelfde abonnement, gaat u naar **Access Control (IAM)**, en Controleer **Azure Stack – beheerde schijf** wordt vermeld.
- Als u een omgeving met meerdere tenants gebruikt, vraagt u uw cloud-operator (die mogelijk in uw eigen organisatie of van de serviceprovider) om te configureren op elk van de Gast-mappen volgens de stappen in [in dit artikel](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory). Implementatie van VM's in een abonnement dat is gekoppeld aan die directory Gast anders kan mislukken met een foutbericht "Interne fout in Schijfbeheer."

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over virtuele machines van Azure Stack](azure-stack-compute-overview.md)
