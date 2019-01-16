---
title: Een klassieke virtuele machine migreren naar een beheerde schijf van ARM VM | Microsoft Docs
description: Een enkele Azure-virtuele machine van het klassieke implementatiemodel migreren naar Managed Disks in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: a662a61d737dbb620d07fa6d114649e70c082796
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54329766"
---
# <a name="migrate-a-classic-vm-to-use-a-managed-disk"></a>Migreren van een klassieke virtuele machine voor het gebruik van een beheerde schijf maken 


In deze sectie helpt u bij het migreren van uw bestaande Azure-VM's van het klassieke implementatiemodel naar [Managed Disks](managed-disks-overview.md) in het Resource Manager-implementatiemodel.


## <a name="plan-for-the-migration-to-managed-disks"></a>Plan voor de migratie naar Managed Disks

In deze sectie helpt u bij het maken van de beste beslissing op schijf en VM-typen.


### <a name="location"></a>Locatie

Kies een locatie waar Managed Disks beschikbaar zijn. Als u naar beheerde schijven die worden ondersteund door Premium-opslag migreert, zorg er ook voor Premium storage is beschikbaar in deze regio. Zie [Azure Services byRegion](https://azure.microsoft.com/regions/#services) voor actuele informatie over de beschikbare locaties.

### <a name="vm-sizes"></a>Formaten van virtuele machines

Als u naar een beheerde schijven met behulp van Premium-opslag migreert, hebt u de grootte van de virtuele machine bijwerken naar Premium Storage kunnen grootte beschikbaar in de regio waar de virtuele machine zich bevindt. Bekijk de VM-grootten die Premium-opslag die geschikt zijn. De specificaties van de grootte van virtuele Azure-machine vindt u in [grootten voor virtuele machines](sizes.md).
Bekijk de prestatiekenmerken van virtuele machines die werken met Premium Storage en kiest u de meest geschikte VM-grootte die het beste bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het schijfverkeer te stimuleren.

### <a name="disk-sizes"></a>Schijfformaten

**Premium**

Er zijn zeven typen Premium-opslag die kunnen worden gebruikt met de virtuele machine en elk heeft specifieke IOPs en doorvoer limieten. Houd rekening met deze limieten bij het kiezen van het schijftype voor Premium voor uw virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek wordt geladen.

| Schijftype voor Premium-schijven  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde | 

**Standard**

Er zijn zeven typen standaardschijven die kunnen worden gebruikt met de virtuele machine. Elk van deze andere capaciteit hebben maar dezelfde IOPS en doorvoerlimieten. Kies het type Standard beheerde schijven op basis van de capaciteitsbehoeften van uw toepassing.

| Standard-schijftype  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Schijfgrootte           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| IOP's per schijf       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Doorvoer per schijf | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 


### <a name="disk-caching-policy"></a>Beleid voor caching schijf 

**Premium Managed Disks**

Beleid voor caching schijf is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen / schrijven* voor de Premium-schijf die is gekoppeld aan de virtuele machine. Deze configuratieinstelling wordt aanbevolen om de optimale prestaties voor IOs van uw toepassing. Voor schijven schrijfintensief of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden) uitschakelen in schijfcache zodat u betere prestaties van toepassingen kunt bereiken.

### <a name="pricing"></a>Prijzen

Controleer de [prijzen voor Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/). Prijzen van beheerde Premium-schijven is hetzelfde als de niet-beheerde premium-schijven. Maar de prijzen voor beheerde standaardschijven komt niet overeen met niet-beheerde standaardschijven.


## <a name="checklist"></a>Controlelijst

1.  Als u naar Premium Managed Disks migreert, zorg er dan voor dat deze beschikbaar is in de regio die u naar migreert.

2.  Bepaal de nieuwe VM-serie die u wilt gebruiken. Het moet een Premium Storage kunnen zijn als u naar Premium Managed Disks migreren wilt.

3.  De exacte VM-grootte u wilt gebruiken die beschikbaar zijn in de regio die u migreert om te bepalen. VM-grootte moet groot genoeg is voor het aantal gegevensschijven hebt ondersteunen. Als u vier gegevensschijven hebt, moet de virtuele machine bijvoorbeeld twee of meer kernen hebben. Overweeg ook verwerkingskracht, geheugen, en bandbreedte netwerkbehoeften.

4.  De huidige details op de virtuele machine bij de hand hebt, met inbegrip van de lijst van schijven en de bijbehorende VHD-blobs zijn.

Uw toepassing voorbereiden voor uitvaltijd. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan kunt u deze ophalen voor een consistente status, die u naar het nieuwe platform migreren kunt. Duur van uitvaltijd is afhankelijk van de hoeveelheid gegevens in de schijven om te migreren.


## <a name="migrate-the-vm"></a>De virtuele machine migreren

Uw toepassing voorbereiden voor uitvaltijd. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan kunt u deze ophalen naar consistente staat die u naar het nieuwe platform migreren kunt. Duur van uitvaltijd is afhankelijk van de hoeveelheid gegevens in de schijven om te migreren.

Dit onderdeel is vereist voor de Azure PowerShell-moduleversie 6.0.0 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). U moet ook `Connect-AzureRmAccount` uitvoeren om een verbinding met Azure tot stand te brengen.


Variabelen voor de algemene parameters maken.

```powershell
$resourceGroupName = 'yourResourceGroupName'

$location = 'your location' 

$virtualNetworkName = 'yourExistingVirtualNetworkName'

$virtualMachineName = 'yourVMName'

$virtualMachineSize = 'Standard_DS3'

$adminUserName = "youradminusername"

$adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force

$imageName = 'yourImageName'

$osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'

$dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'

$dataDiskName = 'dataDisk1'
```

Maak een beheerde OS-schijf met de virtuele harde schijf van de klassieke virtuele machine. Zorg ervoor dat u de volledige URI van de VHD met het besturingssysteem naar de parameter $osVhdUri hebt opgegeven. Typ ook **- AccountType** als **Premium_LRS** of **Standard_LRS** op basis van type schijven (premium of standard) u migreert.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreateOption Import '
   -SourceUri $osVhdUri) '
   -ResourceGroupName $resourceGroupName
```

De besturingssysteemschijf koppelen aan de nieuwe virtuele machine.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
$VirtualMachine = Set-AzureRmVMOSDisk '
   -VM $VirtualMachine '
   -ManagedDiskId $osDisk.Id '
   -StorageAccountType Premium_LRS '
   -DiskSizeInGB 128 '
   -CreateOption Attach -Windows
```

Een beheerde gegevensschijf van de gegevens-VHD-bestand maken en toe te voegen aan de nieuwe virtuele machine.

```powershell
$dataDisk1 = New-AzureRmDisk '
   -DiskName $dataDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreationOption Import '
   -SourceUri $dataVhdUri ) '
   -ResourceGroupName $resourceGroupName
    
$VirtualMachine = Add-AzureRmVMDataDisk '
   -VM $VirtualMachine '
   -Name $dataDiskName '
   -CreateOption Attach '
   -ManagedDiskId $dataDisk1.Id '
   -Lun 1
```

De nieuwe virtuele machine maken door in te stellen van openbaar IP-adres, virtueel netwerk en NIC.

```powershell
$publicIp = New-AzureRmPublicIpAddress '
   -Name ($VirtualMachineName.ToLower()+'_ip') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -AllocationMethod Dynamic
    
$vnet = Get-AzureRmVirtualNetwork '
   -Name $virtualNetworkName '
   -ResourceGroupName $resourceGroupName
    
$nic = New-AzureRmNetworkInterface '
   -Name ($VirtualMachineName.ToLower()+'_nic') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -SubnetId $vnet.Subnets[0].Id '
   -PublicIpAddressId $publicIp.Id
    
$VirtualMachine = Add-AzureRmVMNetworkInterface '
   -VM $VirtualMachine '
   -Id $nic.Id
    
New-AzureRmVM -VM $VirtualMachine '
   -ResourceGroupName $resourceGroupName '
   -Location $location
```

> [!NOTE]
>Mogelijk zijn er extra stappen die nodig is voor de ondersteuning van uw toepassing die niet worden behandeld in deze handleiding.
>
>

## <a name="next-steps"></a>Volgende stappen

- Maak verbinding met de virtuele machine. Zie voor instructies [hoe u verbinding maken met en meld u aan een virtuele Azure-machine waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

