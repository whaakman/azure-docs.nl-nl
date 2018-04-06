---
title: Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven | Microsoft Docs
description: Het vinden en niet-gekoppelde Azure beheerde en onbeheerde (VHD's / pagina-blobs)-schijven verwijderen met behulp van Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ramankum
ms.openlocfilehash: a8d2e017ee0b368c2d509ab4bf47ea566ee52a3b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde-schijven
Wanneer u een virtuele machine (VM) in Azure, standaard verwijdert worden niet alle schijven die zijn gekoppeld aan de virtuele machine verwijderd. Deze functie helpt voorkomen dat gegevens verloren gaan als gevolg van het onbedoeld verwijderen van virtuele machines. Nadat een virtuele machine wordt verwijderd, blijft u betaalt voor niet-gekoppelde schijven. In dit artikel laat zien hoe vinden en verwijderen van eventuele niet-gekoppelde schijven en onnodige kosten te verlagen. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Schijven die worden beheerd: zoeken en verwijderen van niet-gekoppelde schijven 

Zoekt naar het volgende script ontkoppeld [schijven die worden beheerd](managed-disks-overview.md) aan de hand van de waarde van de **door** eigenschap. Wanneer u een beheerde schijf is gekoppeld aan een VM de **door** eigenschap bevat de bron-ID van de virtuele machine. Wanneer u een beheerde schijf is ontkoppeld, de **door** eigenschap is null. Het script onderzoekt de beheerde schijven in een Azure-abonnement. Wanneer het script wordt gezocht naar een beheerde schijf met de **door** eigenschap is ingesteld op null, het script wordt bepaald of de schijf ontkoppeld is.

>[!IMPORTANT]
>Voer eerst het script door in te stellen de **deleteUnattachedDisks** variabele op 0. Deze actie kunt u zoeken en weergeven van alle niet-gekoppelde beheerde schijven.
>
>Nadat u alle niet-gekoppelde schijven, voert u het script opnieuw en stelt de **deleteUnattachedDisks** variabele op 1. Deze actie kunt u alle niet-gekoppelde beheerde schijven verwijderen.
>

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Schijven zonder begeleiding: zoeken en verwijderen van niet-gekoppelde schijven 

Niet-beheerde schijven zijn VHD-bestanden die worden opgeslagen als [pagina-blobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Het volgende script wordt gezocht naar niet-gekoppelde niet-beheerde schijven (pagina-blobs) aan de hand van de waarde van de **LeaseStatus** eigenschap. Wanneer een niet-beheerde schijf is gekoppeld aan een VM de **LeaseStatus** eigenschap is ingesteld op **vergrendeld**. Wanneer een niet-beheerde schijf ontkoppeld is, de **LeaseStatus** eigenschap is ingesteld op **ontgrendeld**. Het script onderzoekt de niet-beheerde schijven in de Azure storage-accounts in een Azure-abonnement. Wanneer het script wordt gezocht naar een niet-beheerde schijf met een **LeaseStatus** eigenschap ingesteld op **ontgrendeld**, het script bepaald of de schijf ontkoppeld is.

>[!IMPORTANT]
>Voer eerst het script door in te stellen de **deleteUnattachedVHDs** variabele op 0. Deze actie kunt u zoeken en weergeven van alle niet-gekoppelde onbeheerde VHD's.
>
>Nadat u alle niet-gekoppelde schijven, voert u het script opnieuw en stelt de **deleteUnattachedVHDs** variabele op 1. Deze actie kunt u alle niet-gekoppelde onbeheerde VHD's te verwijderen.
>

```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

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

Zie voor meer informatie [opslagaccount verwijderen](../../storage/common/storage-create-storage-account.md) en [identificeren zwevende Disks Using PowerShell](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)



