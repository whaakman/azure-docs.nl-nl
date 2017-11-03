---
title: Azure CLI-Script steekproef - maken van een beheerde schijf van een VHD-bestand in een opslagaccount in hetzelfde abonnement | Microsoft Docs
description: Azure CLI-Script steekproef - maken van een beheerde schijf van een VHD-bestand in een opslagaccount in hetzelfde abonnement
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ede9457f5843d0a8a04503779970a553c5ed4f96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>Maak een beheerde schijf van een VHD-bestand in een opslagaccount in hetzelfde abonnement met CLI

Dit script maakt een beheerde schijf van een VHD-bestand in een opslagaccount in hetzelfde abonnement. Dit script gebruiken voor het importeren van een gespecialiseerde (geen gegeneraliseerde/Sysprep voorbereide) VHD naar beheerde OS-schijf voor het maken van een virtuele machine. Of gebruik het importeren van een VHD-gegevens naar beheerde gegevensschijf. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het maken van een beheerde schijf vanaf een VHD. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ schijf maken](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Hiermee maakt u een beheerde schijf met de URI van een VHD in een opslagaccount in hetzelfde abonnement |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken door het koppelen van een beheerde schijf als besturingssysteemschijf](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine en beheerd schijven CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
