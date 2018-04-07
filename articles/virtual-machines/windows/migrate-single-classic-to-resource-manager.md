---
title: Een klassieke virtuele machine migreren naar een beheerde schijf-ARM VM | Microsoft Docs
description: Een enkele virtuele machine van Azure vanuit het klassieke implementatiemodel migreren naar schijven beheerd in het Resource Manager-implementatiemodel.
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
ms.openlocfilehash: 1241f893ca69e3ddaf464e66943caa2697e6d8e7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="manually-migrate-a-classic-vm-to-a-new-arm-managed-disk-vm-from-the-vhd"></a>Handmatig een klassieke virtuele machine migreren naar een nieuwe ARM beheerd schijf virtuele machine van de VHD 


Deze sectie helpt u bij het migreren van uw bestaande Azure VM's van het klassieke implementatiemodel naar [schijven beheerd](managed-disks-overview.md) in het Resource Manager-implementatiemodel.


## <a name="plan-for-the-migration-to-managed-disks"></a>Plannen voor de migratie naar schijven beheerd

Deze sectie helpt u bij het maken van de beste beslissing op schijf en VM-typen.


### <a name="location"></a>Locatie

Kies een locatie waar Azure beheerd schijven beschikbaar zijn. Als u naar Premium-schijven worden beheerd migreert, ook voor zorgen dat de Premium-opslag beschikbaar is in de regio waar u van plan bent om naar te migreren. Zie [Azure Services byRegion](https://azure.microsoft.com/regions/#services) voor actuele informatie over beschikbare locaties.

### <a name="vm-sizes"></a>Formaten van virtuele machines

Als u naar Premium-schijven worden beheerd migreert, hebt u voor het bijwerken van de grootte van de virtuele machine naar de Premium-opslag kunnen grootte beschikbaar in de regio waar de VM zich bevindt. Bekijk de VM-grootten Premium-opslag die geschikt zijn. De Azure VM-grootte specificaties worden vermeld in [grootten voor virtuele machines](sizes.md).
Bekijk de prestatiekenmerken van virtuele machines die geschikt is voor Premium-opslag en kiest u de meest geschikte VM-grootte die het beste past bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het verkeer van de schijf.

### <a name="disk-sizes"></a>Schijfformaten

**Premium beheerde schijven**

Er zijn zeven typen premium-beheerde schijven die kunnen worden gebruikt met uw virtuele machine en elke principal heeft bepaalde IOPs en doorvoerlimieten limieten. U kunt deze limieten bij het kiezen van het type Premium-schijf voor de virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek wordt geladen.

| Premium-schijven Type  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde | 

**Beheerde standaardschijven**

Er zijn zeven soorten Standard-beheerde schijven die kunnen worden gebruikt met uw virtuele machine. Elk van deze andere capaciteit hebben maar dezelfde IOPS en doorvoerlimieten hebben. Kies het type van de Standard-beheerde schijven op basis van de capaciteitsbehoeften van uw toepassing.

| Standard-schijftype  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Schijfgrootte           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOP's per schijf       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Doorvoer per schijf | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 


### <a name="disk-caching-policy"></a>Het beleid voor schijf 

**Premium beheerde schijven**

Beleid voor de schijfcache is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen-schrijven* voor de Premium-schijf is gekoppeld aan de VM. Deze configuratieinstelling wordt aanbevolen de optimale prestaties voor uw toepassing IOs bereiken. Voor schijven schrijven zware of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden), uitschakelen schijfcache, zodat u kunt betere prestaties bereiken.

### <a name="pricing"></a>Prijzen

Controleer de [prijzen voor schijven beheerd](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Prijzen van beheerde Premium-schijven is hetzelfde als het niet-beheerde Premium-schijven. Maar prijzen voor beheerde standaardschijven is anders dan standaardschijven zonder begeleiding.


## <a name="checklist"></a>Controlelijst

1.  Als u naar Premium-schijven worden beheerd migreert, zorg er dan voor dat is beschikbaar in de regio die u naar migreert.

2.  Bepaal de nieuwe VM-reeks die u wilt gebruiken. Moet een Premium-opslag geschikt als u naar Premium-schijven worden beheerd migreert.

3.  Bepaal de exacte VM-grootte u wilt gebruiken die beschikbaar zijn in de regio die u migreert naar. VM-grootte moet groot genoeg zijn om het aantal gegevensschijven dat u hebt ondersteunen. Als u vier gegevensschijven hebt, moet de virtuele machine bijvoorbeeld twee of meer kernen hebben. Houd ook rekening met verwerkingskracht, geheugen en netwerkbandbreedte moet.

4.  De huidige VM details die handig zijn, met inbegrip van de lijst van schijven en de bijbehorende VHD-blobs hebben.

Bereid uw toepassing uitvaltijd. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan kunt u dit downloaden naar consistente status die u naar het nieuwe platform migreren kunt. Duur van uitvaltijd is afhankelijk van de hoeveelheid gegevens op de schijven om te migreren.


## <a name="migrate-the-vm"></a>De virtuele machine migreren

Bereid uw toepassing uitvaltijd. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan kunt u dit downloaden naar consistente status die u naar het nieuwe platform migreren kunt. Duur van uitvaltijd is afhankelijk van de hoeveelheid gegevens op de schijven om te migreren.


1.  Stel eerst de algemene parameters:

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

2.  Een beheerde OS-schijf met behulp van de VHD van de klassieke VM maken.

    Zorg ervoor dat u de volledige URI van de OS-VHD naar de parameter $osVhdUri hebt opgegeven. Typ ook **- AccountType** als **PremiumLRS** of **StandardLRS** op basis van het type schijven (Premium of Standard) u migreert naar.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  De OS-schijf koppelen aan de nieuwe virtuele machine.

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  Een beheerde gegevensschijf uit het gegevensbestand van de VHD maken en toe te voegen aan de nieuwe virtuele machine.

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  De nieuwe virtuele machine maken met het instellen van openbare IP-, virtueel netwerk en NIC.

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
>Mogelijk zijn er extra stappen die nodig zijn voor ondersteuning van uw toepassing die niet worden gedekt door deze handleiding.
>
>

## <a name="next-steps"></a>Volgende stappen

- Verbinding maken met de virtuele machine. Zie voor instructies [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

