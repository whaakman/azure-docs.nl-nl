---
title: Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven | Microsoft Docs
description: Het vinden en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde (VHD's / pagina-blobs)-schijven, met behulp van Azure PowerShell.
services: virtual-machines-windows
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: a846d3578d40b19762f185381c92bdf8e225b185
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde-schijven
Wanneer u een virtuele machine in Azure verwijdert, worden de schijven die zijn gekoppeld aan het niet standaard verwijderd. Deze voorkomt dat gegevensverlies als gevolg van virtuele machines per ongeluk hebt verwijderd, maar u betaalt voor de niet-gekoppelde schijven onnodig blijven. Gebruik dit artikel om te zoeken en verwijderen van de niet-gekoppelde schijven en kosten opslaan. 


## <a name="find-and-delete-unattached-managed-disks"></a>Zoeken en verwijderen van niet-gekoppelde beheerde schijven 

Het volgende script toont u het zoeken van niet-gekoppelde [schijven beheerd](managed-disks-overview.md) met behulp van *door* eigenschap. Alle schijven beheerd in een abonnement en controles doorlopen de *door* eigenschap is null niet-gekoppelde schijven beheerd vinden. *Door* eigenschap slaat de resource-ID van de virtuele machine waarop een beheerd schijf is gekoppeld.

We raden u aan de eerste uitvoering van het script door in te stellen de *deleteUnattachedDisks* variabele op 0 om weer te geven van alle niet-gekoppelde schijven. Bekijk de niet-gekoppelde schijven en voer het script uit door *deleteUnattachedDisks* op 1 om te verwijderen van de niet-gekoppelde schijven.

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
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde niet-beheerde schijven 

Niet-beheerde schijven zijn VHD-bestanden opgeslagen als [pagina-blobs] (/ rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure Storage-accounts](../../storage/common/storage-create-storage-account.md). Het volgende script toont u hoe niet-gekoppelde niet-beheerde schijven (pagina-blobs) vinden met behulp van de *LeaseStatus* eigenschap. Het doorlopen van de niet-beheerde schijven in alle de storage-accounts in een abonnement en controleert u of de *LeaseStatus* eigenschap worden ontgrendeld zodat niet-gekoppelde niet-beheerde schijven vinden. *LeaseStatus* eigenschap is ingesteld op vergrendeld als een niet-beheerde schijf is gekoppeld aan een virtuele machine.

We raden u aan de eerste uitvoering van het script door in te stellen de *deleteUnattachedVHDs* variabele op 0 om weer te geven van alle niet-gekoppelde schijven. Bekijk de niet-gekoppelde schijven en voer het script uit door *deleteUnattachedVHDs* op 1 om te verwijderen van de niet-gekoppelde schijven.


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

[Storage-account verwijderen](../../storage/common/storage-create-storage-account.md)




