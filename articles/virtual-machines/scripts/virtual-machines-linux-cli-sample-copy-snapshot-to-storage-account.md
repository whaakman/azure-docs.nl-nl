---
title: "Azure CLI-voorbeeldscript - momentopname van de Export/kopiëren als VHD naar een opslagaccount in andere regio | Microsoft Docs"
description: "Azure CLI-voorbeeldscript - momentopname van de Export/kopiëren als VHD naar een opslagaccount in dezelfde of een ander abonnement"
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
ms.openlocfilehash: cd0128256e016a329b0940eec4be41426cdaf51a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a>Export/kopiëren beheerde momentopnamen als VHD naar een opslagaccount in andere regio met CLI

Dit script wordt een momentopname van een beheerde exporteert naar een opslagaccount in andere regio. Het genereert eerst de SAS-URI van de momentopname en vervolgens gebruikt om deze te kopiëren naar een opslagaccount in andere regio. Dit script gebruiken voor het onderhouden van back-up van uw beheerde schijven in andere regio voor herstel na noodgevallen. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het genereren van SAS-URI voor een momentopname van een beheerde en kopieert de momentopname naar een opslagaccount met SAS-URI. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ momentopname toegang verlenen](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_grant_access) | Genereert alleen-lezen SA's dat wordt gebruikt voor de onderliggende VHD-bestand kopiëren naar een opslagaccount of downloaden met on-premises  |
| [start AZ storage-blob kopiëren](https://docs.microsoft.com/cli/azure/storage/blob/copy#az_storage_blob_copy_start) | Een blob kopieert asynchroon uit één opslagaccount naar een andere |

## <a name="next-steps"></a>Volgende stappen

[Een beheerde schijf vanaf een VHD maken](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Een virtuele machine maken van een beheerde schijf](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine en beheerd schijven CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
