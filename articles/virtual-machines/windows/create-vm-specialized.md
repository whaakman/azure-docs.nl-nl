---
title: Een Windows-virtuele machine maken vanaf een speciale VHD in Azure | Microsoft Docs
description: Maak een nieuwe Windows VM door het koppelen van een gespecialiseerde beheerde schijf als de OS-schijf met het implementatiemodel van Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: cynthn
ms.openlocfilehash: be7933b038fb5a648249e9b0c73415bff778930b
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012781"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-using-powershell"></a>Een Windows-virtuele machine maken vanaf een speciale schijf met behulp van PowerShell

Maak een nieuwe virtuele machine door het koppelen van een gespecialiseerde beheerde schijf als de besturingssysteemschijf. Een speciale schijf is een kopie van de virtuele harde schijf (VHD) van een bestaande virtuele machine die de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM onderhoudt. 

Wanneer u een nieuwe virtuele machine maken met een gespecialiseerde VHD, behoudt de nieuwe virtuele machine de naam van de computer van de oorspronkelijke virtuele machine. Andere computer-specifieke informatie wordt ook opgeslagen en in sommige gevallen kan deze dubbele gegevens problemen kan veroorzaken. Zich bewust zijn van welke typen computerspecifieke informatie uw toepassingen afhankelijk zijn van bij het kopiëren van een virtuele machine.

U hebt verschillende mogelijkheden:
* [Gebruik een bestaande beheerde schijf](#option-1-use-an-existing-disk). Dit is handig als u hebt een virtuele machine die niet goed werkt. U kunt de VM opnieuw gebruiken de beheerde schijf te maken van een nieuwe virtuele machine verwijderen. 
* [Een VHD uploaden](#option-2-upload-a-specialized-vhd) 
* [Kopiëren van een bestaande virtuele machine van Azure met momentopnamen](#option-3-copy-an-existing-azure-vm)

U kunt ook de Azure portal om te gebruiken [maken van een nieuwe virtuele machine vanaf een speciale VHD](create-vm-specialized-portal.md).

Dit onderwerp leest u hoe u beheerde schijven te gebruiken. Als u een verouderde implementatie hebt waarvoor een opslagaccount gebruikt, Zie [een virtuele machine vanaf een speciale VHD in een opslagaccount maken](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Voordat u begint
Als u PowerShell gebruikt, zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module hebt. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Zie voor meer informatie [Azure PowerShell Versioning](/powershell/azure/overview).

## <a name="option-1-use-an-existing-disk"></a>Optie 1: Een bestaande schijf gebruiken

Als u had een virtuele machine die u hebt verwijderd en u gebruiken als de OS-schijf wilt naar een nieuwe virtuele machine maken, gebruikt u [Get-AzureRmDisk](/azure/powershell/get-azurermdisk).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzureRmDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Nu kunt u deze schijf koppelen als de besturingssysteemschijf voor een [nieuwe virtuele machine](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>Optie 2: Een gespecialiseerde VHD uploaden

U kunt de VHD van een speciale virtuele machine gemaakt met een lokale virtualisatie hulpprogramma, zoals Hyper-V of een virtuele machine die zijn geëxporteerd uit een andere cloud uploaden.

### <a name="prepare-the-vm"></a>De virtuele machine voorbereiden
Als u wilt gebruiken van de VHD-is een nieuwe virtuele machine maken, controleert u de volgende stappen zijn voltooid. 
  
  * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Geen** generalize van de virtuele machine met behulp van Sysprep.
  * Verwijder eventuele Gast virtualisatie-hulpprogramma's en de agents die zijn geïnstalleerd op de virtuele machine (zoals VMware tools).
  * Zorg ervoor dat de virtuele machine is geconfigureerd om op te halen van de IP-adres en DNS-instellingen via DHCP. Dit zorgt ervoor dat de server een IP-adres binnen het VNet verkrijgt wanneer deze opgestart wordt. 


### <a name="get-the-storage-account"></a>Het storage-account ophalen
U moet een opslagaccount in Azure voor het opslaan van de geüploade VHD. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken. 

Als u wilt de beschikbare opslagruimte accounts weergeven, typt u:

```powershell
Get-AzureRmStorageAccount
```

Als u een bestaand opslagaccount gebruiken wilt, gaat u verder met de [uploaden van de VHD](#upload-the-vhd-to-your-storage-account) sectie.

Als u wilt maken van een opslagaccount, als volgt te werk:

1. U moet de naam van de resourcegroep waar het storage-account moet worden gemaakt. Voor meer informatie over de resourcegroepen die zich in uw abonnement, typt u:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Maken van een resourcegroep met de naam *myResourceGroup* in de *VS-West* regio, type:

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Maken van een opslagaccount met de naam *mystorageaccount* in deze resourcegroep met behulp van de [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>De VHD te uploaden naar uw opslagaccount 
Gebruik de [toevoegen AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet voor het uploaden van de VHD naar een container in uw opslagaccount. In dit voorbeeld wordt het bestand geüpload *myVHD.vhd* van `"C:\Users\Public\Documents\Virtual hard disks\"` om een opslagaccount met de naam *mystorageaccount* in de *myResourceGroup* resourcegroep. Het bestand is opgeslagen in de container met de naam *mycontainer* en worden de nieuwe bestandsnaam *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Als dit lukt, kunt u krijgen een antwoord dat ziet er ongeveer als volgt:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Afhankelijk van uw netwerkverbinding en de grootte van de VHD-bestand met deze opdracht kan even duren om te voltooien

### <a name="create-a-managed-disk-from-the-vhd"></a>Een beheerde schijf van de VHD maken

Maak een beheerde schijf van de gespecialiseerde VHD in uw storage-account met [nieuw AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). In dit voorbeeld wordt **myOSDisk1** voor de naam van de schijf, plaatst u de schijf in *Standard_LRS* opslag en maakt gebruik van *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* als de URI voor de bron-VHD.

Een nieuwe resourcegroep maken voor de nieuwe virtuele machine.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Nieuwe schijf met het besturingssysteem van de geüploade VHD maken. 

```powershell
$sourceUri = (https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Optie 3: Een bestaande virtuele machine van Azure kopiëren

U kunt een kopie van een virtuele machine dat maakt gebruik van schijven die worden beheerd door het maken van een momentopname van de virtuele machine en vervolgens met die momentopname maken van een nieuwe schijf- en een nieuwe virtuele machine beheerde maken.


### <a name="take-a-snapshot-of-the-os-disk"></a>Een momentopname van de besturingssysteemschijf

U kunt nemen een momentopname van en volledige virtuele machine (inclusief alle schijven) of van één schijf. De volgende stappen ziet u hoe u een momentopname van de besturingssysteemschijf van uw virtuele machine met de [nieuw AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Sommige parameters instellen. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Haal het VM-object.

```powershell
$vm = Get-AzureRmVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
Haal de naam van de OS-schijf.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

De configuratie van de momentopname maken. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

De momentopname.

```powershell
$snapShot = New-AzureRmSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Als u van plan bent om de momentopname te maken van een virtuele machine die moet worden hoge prestaties, gebruikt u de parameter `-AccountType Premium_LRS` met de opdracht New-AzureRmSnapshot. De parameter wordt de momentopname gemaakt zodat deze wordt opgeslagen als een schijf Premium beheerd. Premium-schijven beheerd zijn duurder dan de standaard. Daarom moet u dat wat u moet Premium voordat u de parameter.

### <a name="create-a-new-disk-from-the-snapshot"></a>Een nieuwe schijf van de momentopname maken

Maak een beheerde schijf van de momentopname met behulp [nieuw AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). In dit voorbeeld wordt *myOSDisk* voor naam van de schijf.

Een nieuwe resourcegroep maken voor de nieuwe virtuele machine.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

De naam Besturingssysteemschijf ingesteld. 

```powershell
$osDiskName = 'myOsDisk'
```

De beheerde schijf maken.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>De nieuwe virtuele machine maken 

Maak de netwerken en andere VM-netwerkbronnen kunnen worden gebruikt door de nieuwe virtuele machine.

### <a name="create-the-subnet-and-vnet"></a>Het subNet en een vNet maken

Maken van het vNet en het subNet van de [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

Het subNet maken. In dit voorbeeld wordt een subnet met de naam **mySubNet**, in de resourcegroep **myDestinationResourceGroup**, en stelt u het adres subnetvoorvoegsel op **10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
   -Name $subnetName `
   -AddressPrefix 10.0.0.0/24
```

Het vNet maken. Dit voorbeeld wordt de naam van het virtuele netwerk moet **myVnetName**, de locatie voor het **VS-West**, en het adresvoorvoegsel voor het virtuele netwerk naar **10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork `
   -Name $vnetName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>De netwerkbeveiligingsgroep en een RDP-regel maken
Als u zich aanmelden bij uw virtuele machine met RDP, moet u een regel waarmee op poort 3389 van RDP-toegang hebben. Omdat de VHD voor de nieuwe virtuele machine is gemaakt vanuit een bestaande gespecialiseerde VM, kunt u een account van de virtuele bronmachine voor RDP.

In het volgende voorbeeld wordt de naam van de NSG op **myNsg** en de naam van de RDP-regel aan **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Zie voor meer informatie over eindpunten en NSG-regels [openen van poorten voor een virtuele machine in Azure met behulp van PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Een openbaar IP-adres en NIC maken
Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, hebt u een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface nodig.

Het openbare IP-adres maken. In dit voorbeeld wordt de openbare naam van de IP-adres is ingesteld op **myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress `
   -Name $ipName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AllocationMethod Dynamic
```       

Maken van de NIC. In dit voorbeeld wordt de naam van de NIC is ingesteld op **myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location -SubnetId $vnet.Subnets[0].Id `
   -PublicIpAddressId $pip.Id `
   -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a>VM-naam en de grootte instellen

Dit voorbeeld wordt de naam van de VM op *myVM* en de VM-grootte naar *Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>De NIC toevoegen
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>De OS-schijf toevoegen 

De OS-schijf toevoegen aan de configuratie met behulp van [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). In dit voorbeeld wordt de grootte van de schijf moet ingesteld *128 GB* en koppelt u de beheerde schijf als een *Windows* besturingssysteemschijf.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>De virtuele machine voltooien 

Maak de virtuele machine met [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)de configuraties die we zojuist hebben gemaakt.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Als deze opdracht voltooid is, ziet u uitvoer als volgt:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Controleren of de virtuele machine is gemaakt
U ziet de zojuist gemaakte virtuele machine ofwel in de [Azure-portal](https://portal.azure.com)onder **Bladeren** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Volgende stappen
Meld u aan de nieuwe virtuele machine. Zie voor meer informatie [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](connect-logon.md).

