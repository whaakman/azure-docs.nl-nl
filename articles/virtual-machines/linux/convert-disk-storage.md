---
title: Converteren van Azure storage van de schijven van standaard beheerd naar premium, en vice versa | Microsoft Docs
description: Het converteren van Azure beheerd schijven opslag van standaard naar premium en omgekeerd, met behulp van Azure CLI.
services: virtual-machines-linux
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: c22c2c194cb839c3ec9e3e851768ca19bc6fc443
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Converteren van Azure storage van de schijven van standaard beheerd naar premium, en omgekeerd

Beheerde schijven biedt twee opties voor opslag: [Premium](../windows/premium-storage.md) (SSD-gebaseerde) en [standaard](../windows/standard-storage.md) (gebaseerd op harde schijf). Hiermee kunt u eenvoudig schakelen tussen de twee opties met minimale downtime op basis van uw prestatievereisten past. Deze mogelijkheid is niet beschikbaar voor niet-beheerde schijven. Maar u kunt eenvoudig [converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) eenvoudig schakelen tussen de twee opties.

In dit artikel leest u hoe beheerde schijven van standard converteren naar premium, en vice versa met Azure CLI. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren](/cli/azure/install-azure-cli.md). 

## <a name="before-you-begin"></a>Voordat u begint

* De conversie vereist een herstart van de virtuele machine, zodat de migratie van de opslag van uw schijven tijdens een onderhoudsvenster vooraf bestaande plannen. 
* Als u niet-beheerde schijven eerst [converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) in dit artikel gebruiken om over te schakelen tussen de twee opties voor opslag. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Standaard alle beheerde schijven van een virtuele machine converteren naar premium, en omgekeerd

In het volgende voorbeeld laten we zien hoe overschakelen van de schijven van een virtuele machine van standaard naar premium-opslag. U kunt beheerde premium-schijven, uw virtuele machine gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld wordt ook verandert in een grootte die ondersteuning biedt voor premium-opslag.

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
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Een beheerde schijf van standard converteren naar premium, en omgekeerd

Voor uw workload ontwikkelen en testen, is het raadzaam combinatie van standard en premium-schijven voor uw kosten hebben. U kunt dit doen door te upgraden naar de premium-opslag alleen op de schijven die betere prestaties zijn vereist. In het volgende voorbeeld laten we zien hoe overschakelen van één schijf van een virtuele machine van standaard naar premium-opslag, en vice versa. U kunt beheerde premium-schijven, uw virtuele machine gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld wordt ook verandert in een grootte die ondersteuning biedt voor premium-opslag.

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

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine uitvoeren met behulp van [momentopnamen](snapshot-copy-managed-disk.md).

