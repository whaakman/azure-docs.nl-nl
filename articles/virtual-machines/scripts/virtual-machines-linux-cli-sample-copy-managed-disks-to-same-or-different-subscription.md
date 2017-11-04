---
title: "Azure CLI-voorbeeldscript - schijven op dezelfde of verschillende abonnement die worden beheerd kopiëren (verplaatsen) | Microsoft Docs"
description: Azure CLI-voorbeeldscript - exemplaar (verplaatsen) beheerd schijven met hetzelfde of een ander abonnement
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
ms.openlocfilehash: 8ff34f3d0b11c47f19205b92aebfc96e5cd5a014
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Beheerde schijven met hetzelfde of een ander abonnement CLI kopiëren

Dit script wordt een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement, maar in dezelfde regio. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het maken van een nieuwe beheerde schijf in het doelabonnement met de Id van de beheerde bronschijf. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ schijf weergeven](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Hiermee haalt u de eigenschappen van een beheerde schijf met de naam en eigenschappen van de bronnengroep van de beheerde schijf. Id-eigenschap wordt gebruikt voor de beheerde schijf kopiëren naar een ander abonnement.  |
| [AZ schijf maken](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Kopieën een beheerde schijf door het maken van een nieuwe schijf in een ander abonnement met behulp van Id beheerd en de naam van de bovenliggende beheerde schijf.  |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken van een beheerde schijf](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine en beheerd schijven CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
