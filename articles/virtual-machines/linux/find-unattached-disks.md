---
title: Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven | Microsoft Docs
description: Het vinden en niet-gekoppelde Azure beheerde en onbeheerde (VHD's / pagina-blobs)-schijven verwijderen met behulp van Azure CLI.
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
ms.openlocfilehash: 281e51783af05e02346b537f0abccdb2def38b31
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
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

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Schijven zonder begeleiding: zoeken en verwijderen van niet-gekoppelde schijven 

Niet-beheerde schijven zijn VHD-bestanden die worden opgeslagen als [pagina-blobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Het volgende script wordt gezocht naar niet-gekoppelde niet-beheerde schijven (pagina-blobs) aan de hand van de waarde van de **LeaseStatus** eigenschap. Wanneer een niet-beheerde schijf is gekoppeld aan een VM de **LeaseStatus** eigenschap is ingesteld op **vergrendeld**. Wanneer een niet-beheerde schijf ontkoppeld is, de **LeaseStatus** eigenschap is ingesteld op **ontgrendeld**. Het script onderzoekt de niet-beheerde schijven in de Azure storage-accounts in een Azure-abonnement. Wanneer het script wordt gezocht naar een niet-beheerde schijf met een **LeaseStatus** eigenschap ingesteld op **ontgrendeld**, het script bepaald of de schijf ontkoppeld is.

>[!IMPORTANT]
>Voer eerst het script door in te stellen de **deleteUnattachedVHDs** variabele op 0. Deze actie kunt u zoeken en weergeven van alle niet-gekoppelde onbeheerde VHD's.
>
>Nadat u alle niet-gekoppelde schijven, voert u het script opnieuw en stelt de **deleteUnattachedVHDs** variabele op 1. Deze actie kunt u alle niet-gekoppelde onbeheerde VHD's te verwijderen.
>

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



