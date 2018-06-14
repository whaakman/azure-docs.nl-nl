---
title: 'Azure CLI-voorbeeldscript: met CLI een momentopname van een beheerde schijf kopiëren (verplaatsen) naar hetzelfde of een ander abonnement | Microsoft Docs'
description: 'Azure CLI-voorbeeldscript: met CLI een momentopname van een beheerde schijf kopiëren (verplaatsen) naar hetzelfde of een ander abonnement'
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 904f564d2f3c0584851309780eb2ccca379534fa
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29850202"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Met CLI een momentopname van een beheerde schijf kopiëren naar hetzelfde of een ander abonnement

Met dit script wordt een momentopname van een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement. Gebruik dit script om een momentopname te verplaatsen naar een ander abonnement in dezelfde regio als de bovenliggende momentopname.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een momentopname te maken in het doelabonnement met behulp van de id van de bronmomentopname. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Hiermee haalt u alle eigenschappen van een momentopname op aan de hand van de naam en de eigenschappen van de resourcegroep van de momentopname. De id-eigenschap wordt gebruikt om de momentopname naar een ander abonnement te kopiëren.  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_create) | Hiermee wordt een momentopname gekopieerd door een momentopname te maken in een ander abonnement met dezelfde id en naam als de bovenliggende momentopname.  |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken van een momentopname](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-scriptvoorbeelden voor virtuele machines en beheerde schijven vindt u in de [Azure-documentatie voor Linux-VM's](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
