---
title: 'Azure CLI-Script voorbeeld: een virtuele machine maken door het koppelen van een beheerde schijf als besturingssysteemschijf | Microsoft Docs'
description: 'Azure CLI-Script voorbeeld: een virtuele machine maken door het koppelen van een beheerde schijf als besturingssysteemschijf'
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
ms.openlocfilehash: 2141ea4fd25dfc69ada02c54c4f6b6b717b8e7db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>Maak een virtuele machine met behulp van een bestaande beheerde OS-schijf met CLI

Dit script maakt een virtuele machine door het koppelen van een bestaande beheerde schijf als de besturingssysteemschijf. Gebruik dit script in de voorgaande scenario's:
* Een virtuele machine maken van een bestaande beheerde OS schijf die is opgehaald uit een beheerde schijf in een ander abonnement
* Een virtuele machine maken van een bestaande beheerde schijf dat is gemaakt vanaf een speciale VHD-bestand 
* Een virtuele machine maken van een bestaande beheerde OS schijf die is gemaakt vanuit een momentopname 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten voor het ophalen van beheerde schijfeigenschappen, een beheerde schijf koppelen met een nieuwe virtuele machine en een virtuele machine maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ schijf weergeven](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Haalt de eigenschappen van de schijf met de naam van schijf en de naam van resourcegroep beheerd. Id-eigenschap wordt gebruikt om een beheerde schijf koppelen met een nieuwe virtuele machine |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Hiermee maakt u een VM die gebruikmaakt van een beheerde besturingssysteemschijf |
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
