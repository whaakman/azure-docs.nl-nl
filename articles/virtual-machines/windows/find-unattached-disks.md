---
title: Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven | Microsoft Docs
description: Over het zoeken en niet-gekoppelde Azure beheerde en onbeheerde (VHD's / pagina-blobs) schijven verwijderen met behulp van Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: cb52956afe085c076f0a9a7c2d6810f3def32e3f
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57568954"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven

Wanneer u een virtuele machine (VM) in Azure, standaard verwijdert worden niet alle schijven die zijn gekoppeld aan de virtuele machine verwijderd. Deze functie helpt voorkomen dat gegevens verloren gaan vanwege het onbedoeld verwijderen van virtuele machines. Nadat een virtuele machine is verwijderd, blijft u betalen voor niet-gekoppelde schijven. Dit artikel leest u hoe om te zoeken en verwijderen van een niet-gekoppelde schijven en onnodige kosten reduceren.

## <a name="managed-disks-find-and-delete-unattached-disks"></a>Beheerde schijven: Zoeken en niet-gekoppelde schijven verwijderen

Het volgende script zoekt naar niet-gekoppelde [beheerde schijven](managed-disks-overview.md) door te controleren van de waarde van de **ManagedBy** eigenschap. Wanneer een beheerde schijf is gekoppeld aan een virtuele machine, de **ManagedBy** eigenschap bevat de resource-ID van de virtuele machine. Wanneer een beheerde schijf niet-gekoppeld, is de **ManagedBy** eigenschap null is. Het script onderzoekt alle beheerde schijven in een Azure-abonnement. Wanneer het script zoekt naar een beheerde schijf met de **ManagedBy** eigenschap is ingesteld op null, het script wordt bepaald dat de schijf niet-gekoppeld is.

>[!IMPORTANT]
>Voer het script eerst uit door in te stellen de **deleteUnattachedDisks** in op 0. Deze actie kunt u vinden en weergeven van alle niet-gekoppelde beheerde schijven.
>
>Nadat u alle niet-gekoppelde schijven bekijkt, voer het script opnieuw uit en stel de **deleteUnattachedDisks** in op 1. Deze actie kunt u alle niet-gekoppelde beheerde schijven verwijderen.

```azurepowershell-interactive
# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0
$managedDisks = Get-AzDisk
foreach ($md in $managedDisks) {
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){
        if($deleteUnattachedDisks -eq 1){
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"
            $md | Remove-AzDisk -Force
            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "
        }else{
            $md.Id
        }
    }
 }
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Niet-beheerde schijven: Zoeken en niet-gekoppelde schijven verwijderen

Niet-beheerde schijven worden VHD-bestanden die zijn opgeslagen als [pagina-blobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Het volgende script zoekt naar niet-gekoppelde niet-beheerde schijven (pagina-blobs) door te controleren van de waarde van de **LeaseStatus** eigenschap. Wanneer een niet-beheerde schijf is gekoppeld aan een virtuele machine, de **LeaseStatus** eigenschap is ingesteld op **vergrendeld**. Wanneer een niet-beheerde schijf niet-gekoppeld, is de **LeaseStatus** eigenschap is ingesteld op **ontgrendeld**. Het script onderzoekt alle niet-beheerde schijven in de Azure storage-accounts in een Azure-abonnement. Wanneer het script zoekt naar een niet-beheerde schijf met een **LeaseStatus** eigenschap ingesteld op **ontgrendeld**, het script wordt bepaald dat de schijf niet-gekoppeld is.

>[!IMPORTANT]
>Voer het script eerst uit door in te stellen de **deleteUnattachedVHDs** in op 0. Deze actie kunt u vinden en weergeven van alle niet-gekoppelde niet-beheerde VHD's.
>
>Nadat u alle niet-gekoppelde schijven bekijkt, voer het script opnieuw uit en stel de **deleteUnattachedVHDs** in op 1. Deze actie kunt u alle niet-gekoppelde niet-beheerde VHD's te verwijderen.

```azurepowershell-interactive
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=0
$storageAccounts = Get-AzStorageAccount
foreach($storageAccount in $storageAccounts){
    $storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value
    $context = New-AzStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey
    $containers = Get-AzStorageContainer -Context $context
    foreach($container in $containers){
        $blobs = Get-AzStorageBlob -Container $container.Name -Context $context
        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
                    if($deleteUnattachedVHDs -eq 1){
                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                        $_ | Remove-AzStorageBlob -Force
                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                    }
                    else{
                        $_.ICloudBlob.Uri.AbsoluteUri
                    }
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [opslagaccount verwijderen](../../storage/common/storage-create-storage-account.md) en [identificeren zwevende schijven met behulp van PowerShell](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)
