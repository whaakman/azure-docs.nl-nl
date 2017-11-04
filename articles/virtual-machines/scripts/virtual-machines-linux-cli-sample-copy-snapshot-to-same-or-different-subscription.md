---
title: Azure CLI-voorbeeldscript - exemplaar (verplaatsen)-momentopname van een beheerde schijf met dezelfde of verschillende abonnement CLI | Microsoft Docs
description: Azure CLI-voorbeeldscript - exemplaar (verplaatsen)-momentopname van een beheerde schijf met dezelfde of verschillende abonnement CLI
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
ms.openlocfilehash: 7c301a314ee946bb9199650bb7f674b8dce7c141
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Momentopname van een beheerde schijf kopiëren naar hetzelfde of een ander abonnement met CLI

Dit script wordt een momentopname van een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement. Dit script gebruiken voor het verplaatsen van een momentopname naar ander abonnement in dezelfde regio bevinden als de momentopname van de bovenliggende.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het maken van een momentopname in het doelabonnement met de Id van de momentopname van de bron. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ momentopname weergeven](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Hiermee haalt u de eigenschappen van een momentopname met de naam en eigenschappen van de momentopname van de bronnengroep. Id-eigenschap wordt gebruikt voor het kopiëren van de momentopname naar ander abonnement.  |
| [AZ momentopname maken](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_create) | Een momentopname opgehaald door het maken van een momentopname in een ander abonnement met de Id en de naam van de momentopname van de bovenliggende.  |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken vanuit een momentopname](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine en beheerd schijven CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
