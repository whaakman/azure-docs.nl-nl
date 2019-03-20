---
title: Converteren van Azure managed disks-opslag van Standard naar Premium- of Premium naar standaard | Microsoft Docs
description: Het converteren van Azure managed disks-opslag van Standard naar Premium- of Premium naar standaard met behulp van de Azure CLI.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 18730f662f36000e1efc826c35bebde79dbf0e79
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013611"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-or-premium-to-standard"></a>Converteren van Azure managed disks-opslag van Standard naar Premium- of Premium naar standaard

Er zijn vier [schijftypen](disks-types.md) voor Azure beheerde schijven: Azure Ultra Disk-opslag, Premium SSD, Standard-SSD en standaard harde schijven. U kunt eenvoudig schakelen tussen Premium SSD-, Standard-SSD- en standaard harde schijven op basis van de prestatiebehoeften van uw met weinig downtime. Deze functionaliteit wordt niet ondersteund voor niet-beheerde schijven of Ultra Disk-opslag. Maar u kunt eenvoudig [converteren naar beheerde schijven niet-beheerde](convert-unmanaged-to-managed-disks.md) te kunnen schakelen tussen schijftypen.

Dit artikel leest hoe u kunt beheerde schijven converteren van Standard naar Premium- of Premium naar standaard met behulp van de Azure CLI. Als u wilt installeren of upgraden van het hulpprogramma, Zie [Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Voordat u begint

* Conversie van schijven vereist een herstart van de virtuele machine (VM), dus plan de migratie van uw disk-opslag tijdens een reeds bestaande onderhoudsperiode.
* Voor niet-beheerde schijven, eerste [converteren naar managed disks](convert-unmanaged-to-managed-disks.md) , zodat u tussen opslagopties schakelen kunt.


## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Overschakelen van alle beheerde schijven van een virtuele machine tussen Premium en Standard

In dit voorbeeld laat zien hoe alle schijven van een virtuele machine converteren van Standard naar Premium storage of van Premium naar Standard-opslag. Voor het gebruik van Premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor Premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor Premium-opslag.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the SKU of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the SKU of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Overschakelen van beheerde schijven van de afzonderlijke naast standaard en Premium

Voor uw workload ontwikkelen en testen, kunt u een combinatie van standaard en Premium-schijven om uw kosten te verlagen. U kunt alleen de schijven die betere prestaties nodig een upgrade uitvoert. In dit voorbeeld laat zien hoe een enkele VM-schijf converteren van Standard naar Premium storage of van Premium naar Standard-opslag. Voor het gebruik van Premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor Premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor Premium-opslag.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --ids $vmId --size $size

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Overschakelen van beheerde schijven tussen Standard HDD- en Standard-SSD

In dit voorbeeld laat zien hoe een enkele VM-schijf converteren van Standard-SSD in plaats van Standard HDD of Standard-SSD op standaard harde schijven.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM ID 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-and-premium-in-azure-portal"></a>Overschakelen van beheerde schijven naast standaard en Premium in Azure portal

Volg deze stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de virtuele machine uit de lijst met **virtuele machines**.
3. Als de virtuele machine is niet gestopt, selecteert u **stoppen** aan de bovenkant van de virtuele machine **overzicht** deelvenster en wacht tot de virtuele machine te stoppen.
4. Selecteer in het deelvenster voor de virtuele machine, **schijven** in het menu.
5. Selecteer de schijf die u wilt converteren.
6. Selecteer **configuratie** in het menu.
7. Wijziging de **accounttype** van **standaard harde schijven** te **Premium SSD** of van **Premium SSD** te **Standard HDD**.
8. Selecteer **opslaan**, en sluit het deelvenster van de schijf.

Het bijwerken van het schijftype is onmiddellijk. U kunt uw virtuele machine opnieuw opstarten na de conversie.

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine maken met behulp van [momentopnamen](snapshot-copy-managed-disk.md).
