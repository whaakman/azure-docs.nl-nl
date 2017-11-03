---
title: 'Azure CLI-Script voorbeeld: een virtuele machine maken vanuit een momentopname | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: een virtuele machine maken vanuit een momentopname'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: be282f79445c505ece7c6115df7a29c20a6a5f02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Een virtuele machine maken vanuit een momentopname met CLI

Dit script maakt een virtuele machine van een momentopname van een besturingssysteemschijf.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een beheerde schijven, virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ momentopname weergeven](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Opgehaald momentopname met behulp van de naam van de momentopname en de naam van resourcegroep. De eigenschap id van het geretourneerde object wordt gebruikt voor het maken van een beheerde schijf.  |
| [AZ schijf maken](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Beheerde schijven vanuit een momentopname maakt met behulp van een momentopname van Id, de schijfnaam van de, opslagtype en grootte  |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Hiermee maakt u een VM die gebruikmaakt van een beheerde besturingssysteemschijf |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
