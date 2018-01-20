---
title: Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven | Microsoft Docs
description: Het vinden en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde (VHD's / pagina-blobs)-schijven, met behulp van Azure CLI
services: virtual-machines-linux
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: 9ada768cd4128b9dd6949b5a96c557496c6bb11c
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde-schijven
Wanneer u een virtuele machine in Azure verwijdert, worden de schijven die zijn gekoppeld aan het niet standaard verwijderd. Deze voorkomt dat gegevensverlies als gevolg van virtuele machines per ongeluk hebt verwijderd, maar u betaalt voor de niet-gekoppelde schijven onnodig blijven. Gebruik dit artikel om te zoeken en verwijderen van de niet-gekoppelde schijven en kosten opslaan. 


## <a name="find-and-delete-unattached-managed-disks"></a>Zoeken en verwijderen van niet-gekoppelde beheerde schijven 

Het volgende script toont hoe niet-gekoppelde schijven die worden beheerd met behulp van de eigenschap door vinden.  Het doorlopen van alle schijven beheerd in een abonnement en controleert u of de *door* eigenschap is null niet-gekoppelde schijven beheerd vinden. *Door* eigenschap slaat de resource-ID van de virtuele machine waarop een beheerd schijf is gekoppeld. 

We raden u aan de eerste uitvoering van het script door in te stellen de *deleteUnattachedDisks* variabele op 0 om weer te geven van alle niet-gekoppelde schijven. Bekijk de niet-gekoppelde schijven en voer het script uit door *deleteUnattachedDisks* op 1 om te verwijderen van de niet-gekoppelde schijven.

 ```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done

```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde niet-beheerde schijven 

Niet-beheerde schijven zijn opgeslagen als VHD-bestanden [pagina-blobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure Storage-accounts](../../storage/common/storage-create-storage-account.md). Het volgende script toont hoe niet-gekoppelde niet-beheerde schijven (pagina-BLOB's) met behulp van de eigenschap LeaseStatus vinden. Het doorlopen van de niet-beheerde schijven in alle de storage-accounts in een abonnement en controleert u of de *LeaseStatus* eigenschap worden ontgrendeld zodat niet-gekoppelde niet-beheerde schijven vinden. De *LeaseStatus* eigenschap is ingesteld op vergrendeld als een niet-beheerde schijf is gekoppeld aan een virtuele machine. 

We raden u aan de eerste uitvoering van het script door in te stellen de *deleteUnattachedVHDs* variabele op 0 om weer te geven van alle niet-gekoppelde schijven. Bekijk de niet-gekoppelde schijven en voer het script uit door *deleteUnattachedVHDs* op 1 om te verwijderen van de niet-gekoppelde schijven.


 ```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=1

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 

```

## <a name="next-steps"></a>Volgende stappen

[Storage-account verwijderen](../../storage/common/storage-create-storage-account.md)



