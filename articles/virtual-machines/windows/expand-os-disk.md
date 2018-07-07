---
title: Vouw de besturingssysteemschijf van een Windows-VM in Azure | Microsoft Docs
description: Vouw de grootte van de besturingssysteemschijf van een virtuele machine met behulp van Azure Powershell in het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: kirpasingh
manager: roshar
editor: ''
tags: azure-resource-manager
ms.assetid: d9edfd9f-482f-4c0b-956c-0d2c2c30026c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: kirpas
ms.openlocfilehash: f7ff1f2f27806c92c1ac887ce3c3343b96339745
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888946"
---
# <a name="how-to-expand-the-os-drive-of-a-virtual-machine"></a>Over het uitbreiden van de besturingssysteemschijf van een virtuele machine

Wanneer u een nieuwe virtuele machine (VM) maakt in een resourcegroep gemaakt met het implementeren van een installatiekopie van [Azure Marketplace](https://azure.microsoft.com/marketplace/), de standaard OS-station is vaak 127 GB (sommige afbeeldingen hebben kleinere OS-schijfformaten standaard). Hoewel het mogelijk is om gegevensschijven toe te voegen aan de VM (het aantal is afhankelijk van de SKU die u hebt gekozen) en het bovendien wordt aangeraden om toepassingen en CPU-intensieve werkbelastingen te installeren op deze aanvullende schijven, moeten klanten vaak ook de besturingssysteemschijf uitbreiden om bepaalde scenario's te ondersteunen, zoals de volgende:

- Ondersteuning voor oudere toepassingen die onderdelen op de besturingssysteemschijf installeren.
- Een fysieke computer of virtuele machine migreren van on-premises met een grotere besturingssysteemschijf.


> [!IMPORTANT]
> Formaat van de OS-schijf van een Azure-Machine zorgt ervoor dat deze opnieuw te starten.
>
> Na het uitbreiden van de schijven, moet u [Vouw het schijfvolume binnen het besturingssysteem](#expand-the-volume-within-the-os) om te profiteren van de grotere schijf.
> 

## <a name="resize-a-managed-disk"></a>Grootte van een beheerde schijf wijzigen

Open de Powershell ISE of het Powershell-venster in de beheerdersmodus en voer de volgende stappen uit:

1. Meld u aan met uw Microsoft Azure-account in de modus voor resourcebeheer en selecteer uw abonnement als volgt:
   
   ```powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Stel de naam van de resourcegroep en de VM als volgt in:
   
   ```powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Vraag als volgt een verwijzing op naar uw VM:
   
   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Ga als volgt te werk om de VM te stoppen voordat u de grootte van de schijf aanpast:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Verkrijg een verwijzing naar de beheerde OS-schijf. De grootte van de beheerde besturingssysteemschijf instellen op de gewenste waarde en de schijf als volgt te werk:
   
   ```Powershell
   $disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > De nieuwe grootte moet groter zijn dan de bestaande schijfgrootte. Het maximaal toegestane aantal is 2048 GB voor OS-schijven. (Het is mogelijk om uit te breiden de VHD-blob die grootte overschrijdt, maar het besturingssysteem is alleen mogelijk om te werken met de eerste 2048 GB aan ruimte.)
   > 
   > 
6. Het bijwerken van de VM kan een paar seconden duren. Zodra de opdracht is voltooid, start u de VM als volgt opnieuw op:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Dat is alles. Ga nu met RDP naar de VM, open Computerbeheer (of Schijfbeheer) en vouw het station met de zojuist toegewezen ruimte uit.

## <a name="resize-an-unmanaged-disk"></a>Grootte van een niet-beheerde schijf wijzigen

Open de Powershell ISE of het Powershell-venster in de beheerdersmodus en voer de volgende stappen uit:

1. Meld u aan met uw Microsoft Azure-account in de modus voor resourcebeheer en selecteer uw abonnement als volgt:
   
   ```Powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Stel de naam van de resourcegroep en de VM als volgt in:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Vraag als volgt een verwijzing op naar uw VM:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Ga als volgt te werk om de VM te stoppen voordat u de grootte van de schijf aanpast:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. De grootte van de niet-beheerde besturingssysteemschijf instellen op de gewenste waarde en de virtuele machine als volgt te werk:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > De nieuwe grootte moet groter zijn dan de bestaande schijfgrootte. Het maximaal toegestane aantal is 2048 GB voor OS-schijven. (Het is mogelijk om uit te breiden de VHD-blob die grootte overschrijdt, maar het besturingssysteem is alleen mogelijk om te werken met de eerste 2048 GB aan ruimte.)
   > 
   > 
   
6. Het bijwerken van de VM kan een paar seconden duren. Zodra de opdracht is voltooid, start u de VM als volgt opnieuw op:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```


## <a name="scripts-for-os-disk"></a>Scripts voor de besturingssysteemschijf

Hieronder vindt u het volledige script ter referentie voor zowel beheerde als niet-beheerde schijven:


**Beheerde schijven**

```Powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRMVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

**Niet-beheerde schijven**

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="resizing-data-disks"></a>Gegevensschijven vergroten of verkleinen

In dit artikel primair gericht op het uitbreiden van de besturingssysteemschijf van de virtuele machine, maar het script kan ook worden gebruikt voor het uitbreiden van de gegevensschijven die zijn gekoppeld aan de virtuele machine. Als u bijvoorbeeld de eerste gegevensschijf die is gekoppeld aan de VM wilt uitbreiden, vervangt u het object `OSDisk` van `StorageProfile` door de matrix `DataDisks` en gebruikt u een numerieke index om een verwijzing naar de eerste gekoppelde schijf te verkrijgen, zoals hieronder wordt weergegeven:

**Beheerde schijf**

```powershell
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```


**Niet-beheerde schijf**

```powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```



Op dezelfde manier u kan verwijzen naar andere gegevensschijven die zijn gekoppeld aan de virtuele machine, hetzij met behulp van een index, zoals hierboven of de **naam** van de schijf:


**Beheerde schijf**

```powershell
(Get-AzureRmDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

**Niet-beheerde schijf**

```powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```

## <a name="expand-the-volume-within-the-os"></a>Vouw het schijfvolume binnen het besturingssysteem

Nadat u de schijf hebt uitgebreid voor de virtuele machine, moet u het besturingssysteem en vouw het volume voor de nieuwe ruimte bevat. Er zijn verschillende methoden voor het uitbreiden van een partitie. In deze sectie bevat informatie over verbinding te maken van de virtuele machine met behulp van een RDP-verbinding om uit te breiden de partitie met behulp **DiskPart**.

1. Open een RDP-verbinding met uw virtuele machine.

2.  Open een opdrachtprompt en typ **diskpart**.

2.  Op de **DISKPART** vragen, typ `list volume`. Noteer het volume dat u wilt uitbreiden.

3.  Op de **DISKPART** vragen, typ `select volume <volumenumber>`. Hiermee wordt het volume *volumenummer* die u wilt uitbreiden naar aaneengesloten vrije ruimte op dezelfde schijf.

4.  Op de **DISKPART** vragen, typ `extend [size=<size>]`. Hiermee wordt het geselecteerde volume door *grootte* in megabytes (MB).


##<a name="next-steps"></a>Volgende stappen

U kunt ook Koppel de schijven met behulp van de [Azure-portal](attach-managed-disk-portal.md).



