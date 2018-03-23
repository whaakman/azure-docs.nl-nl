---
title: Voorbeeld van Azure CLI-script - Beheerde schijven kopiëren (verplaatsen) naar hetzelfde of een ander abonnement | Microsoft Docs
description: Voorbeeld van Azure CLI-script - Beheerde schijven kopiëren (verplaatsen) naar hetzelfde of een ander abonnement
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
ms.openlocfilehash: dfdbc0563810447a1a214356b5153afe38d9cf2f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Beheerde schijven kopiëren naar hetzelfde of een ander abonnement met CLI

Met dit script wordt een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement, maar wel in dezelfde regio. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een nieuwe beheerde schijf te maken in het doelabonnement met gebruikmaking van de id van de beheerde bronschijf. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Hiermee haalt u alle eigenschappen van een beheerde schijf op aan de hand van de naam en de eigenschappen van de resourcegroep van de beheerde schijf. De eigenschap Id wordt gebruikt om de beheerde schijf naar een ander abonnement te kopiëren.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Hiermee kopieert u een beheerde schijf door een nieuwe beheerde schijf te maken in een ander abonnement aan de hand van de id en de naam van de bovenliggende beheerde schijf.  |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken van een beheerde schijf](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

U kunt extra CLI-scriptvoorbeelden voor virtuele machines en beheerde schijven vinden in de [Azure-documentatie voor Linux-VM's](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
