---
title: Converteren van Azure managed disks-opslag van standard naar premium, en vice versa | Microsoft Docs
description: Het converteren van Azure managed disks-opslag van standard naar premium, en vice versa, met behulp van Azure CLI.
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
ms.component: disks
ms.openlocfilehash: 62c9d01a4322daaa8cf7210f60c5ee42f55b093c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54476356"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Converteren van Azure managed disks-opslag van standard naar premium, en vice versa

Beheerde schijven biedt drie opties voor opslag: [Premium SSD](../windows/premium-storage.md), Standard-SSD, en [Standard HDD](../windows/standard-storage.md). Hiermee kunt u gemakkelijk schakelen tussen de opties met minimale downtime op basis van uw prestatiebehoeften. Dit wordt niet ondersteund voor niet-beheerde schijven. Maar u kunt eenvoudig [converteren naar managed disks](convert-unmanaged-to-managed-disks.md) eenvoudig schakelen tussen de schijftypen.

In dit artikel wordt beschreven hoe u beheerde schijven converteren van standard naar premium, en vice versa met behulp van Azure CLI. Als u wilt installeren of upgraden, raadpleegt [Azure CLI installeren](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Voordat u begint

* De conversie moet opnieuw worden opgestart van de virtuele machine, dus plan de migratie van uw opslag schijven tijdens een reeds bestaande onderhoudsperiode. 
* Als u niet-beheerde schijven, eerst [converteren naar managed disks](convert-unmanaged-to-managed-disks.md) in dit artikel gebruiken om over te schakelen tussen de opties voor opslag. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Alle beheerde schijven van een virtuele machine converteren van standard naar premium, en vice versa

Het volgende voorbeeld ziet hoe u wilt overschakelen van alle schijven van een virtuele machine van standaard naar premium storage. Voor het gebruik van premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor premium-opslag.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Een beheerde schijf converteren van standard naar premium, en vice versa

Voor uw workload ontwikkelen en testen, kunt u de combinatie van standard en premium-schijven om uw kosten lager zijn. U kunt dit kunt uitvoeren door te upgraden naar premium-opslag, alleen de schijven die betere prestaties nodig hebben. Het volgende voorbeeld laat zien hoe om over te schakelen van één schijf van een virtuele machine van standard naar premium storage, en vice versa. Voor het gebruik van premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor premium-opslag.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Een beheerde schijf converteren van standard HDD naar standard-SSD, en vice versa

Het volgende voorbeeld ziet het wijzigen van één schijf van een virtuele machine van de standaard harde schijf naar de standard-SSD.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-using-the-azure-portal"></a>Converteren met behulp van de Azure-portal

U kunt ook niet-beheerde schijven converteren naar managed disks met behulp van de Azure portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de virtuele machine uit de lijst met virtuele machines in de portal.
3. Selecteer in de blade voor de virtuele machine, **schijven** in het menu.
4. Aan de bovenkant van de **schijven** Selecteer **migreren naar managed disks**.
5. Als uw virtuele machine zich in een beschikbaarheidsset, worden er een waarschuwing weergegeven op de **migreren naar managed disks** blade die u moet eerst de beschikbaarheidsset converteren. De waarschuwing moet een koppeling die u kunt klikken op als u wilt converteren van de beschikbaarheidsset. Zodra de beschikbaarheidsset is geconverteerd, of als uw virtuele machine zich niet in een beschikbaarheidsset, klikt u op **migreren** om het proces van de migratie van uw schijven naar beheerde schijven te starten. 

De virtuele machine worden gestopt en opnieuw opgestart nadat de migratie is voltooid.

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine maken met behulp van [momentopnamen](snapshot-copy-managed-disk.md).

