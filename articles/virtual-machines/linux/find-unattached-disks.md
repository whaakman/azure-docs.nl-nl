---
title: Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven | Microsoft Docs
description: Over het zoeken en niet-gekoppelde Azure beheerde en onbeheerde (VHD's / pagina-blobs) schijven verwijderen met behulp van Azure CLI.
services: virtual-machines-linux
documentationcenter: ''
author: ramankumarlive
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: 034d2433b2251351d54c2c5f41f78b5d45ab80e1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55470753"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Zoeken en verwijderen van niet-gekoppelde Azure beheerde en onbeheerde schijven
Wanneer u een virtuele machine (VM) in Azure, standaard verwijdert worden niet alle schijven die zijn gekoppeld aan de virtuele machine verwijderd. Deze functie helpt voorkomen dat gegevens verloren gaan vanwege het onbedoeld verwijderen van virtuele machines. Nadat een virtuele machine is verwijderd, blijft u betalen voor niet-gekoppelde schijven. Dit artikel leest u hoe om te zoeken en verwijderen van een niet-gekoppelde schijven en onnodige kosten reduceren. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Beheerde schijven: Zoeken en niet-gekoppelde schijven verwijderen 

Het volgende script zoekt naar niet-gekoppelde [beheerde schijven](managed-disks-overview.md) door te controleren van de waarde van de **ManagedBy** eigenschap. Wanneer een beheerde schijf is gekoppeld aan een virtuele machine, de **ManagedBy** eigenschap bevat de resource-ID van de virtuele machine. Wanneer een beheerde schijf niet-gekoppeld, is de **ManagedBy** eigenschap null is. Het script onderzoekt alle beheerde schijven in een Azure-abonnement. Wanneer het script zoekt naar een beheerde schijf met de **ManagedBy** eigenschap is ingesteld op null, het script wordt bepaald dat de schijf niet-gekoppeld is.

>[!IMPORTANT]
>Voer het script eerst uit door in te stellen de **deleteUnattachedDisks** in op 0. Deze actie kunt u vinden en weergeven van alle niet-gekoppelde beheerde schijven.
>
>Nadat u alle niet-gekoppelde schijven bekijkt, voer het script opnieuw uit en stel de **deleteUnattachedDisks** in op 1. Deze actie kunt u alle niet-gekoppelde beheerde schijven verwijderen.
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

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Niet-beheerde schijven: Zoeken en niet-gekoppelde schijven verwijderen 

Niet-beheerde schijven worden VHD-bestanden die zijn opgeslagen als [pagina-blobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Het volgende script zoekt naar niet-gekoppelde niet-beheerde schijven (pagina-blobs) door te controleren van de waarde van de **LeaseStatus** eigenschap. Wanneer een niet-beheerde schijf is gekoppeld aan een virtuele machine, de **LeaseStatus** eigenschap is ingesteld op **vergrendeld**. Wanneer een niet-beheerde schijf niet-gekoppeld, is de **LeaseStatus** eigenschap is ingesteld op **ontgrendeld**. Het script onderzoekt alle niet-beheerde schijven in de Azure storage-accounts in een Azure-abonnement. Wanneer het script zoekt naar een niet-beheerde schijf met een **LeaseStatus** eigenschap ingesteld op **ontgrendeld**, het script wordt bepaald dat de schijf niet-gekoppeld is.

>[!IMPORTANT]
>Voer het script eerst uit door in te stellen de **deleteUnattachedVHDs** in op 0. Deze actie kunt u vinden en weergeven van alle niet-gekoppelde niet-beheerde VHD's.
>
>Nadat u alle niet-gekoppelde schijven bekijkt, voer het script opnieuw uit en stel de **deleteUnattachedVHDs** in op 1. Deze actie kunt u alle niet-gekoppelde niet-beheerde VHD's te verwijderen.
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

[Opslagaccount verwijderen](../../storage/common/storage-create-storage-account.md)



