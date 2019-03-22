---
title: 'Azure CLI-voorbeeldscript: met CLI een momentopname van een beheerde schijf kopiëren (verplaatsen) naar hetzelfde of een ander abonnement | Microsoft Docs'
description: 'Azure CLI-voorbeeldscript: met CLI een momentopname van een beheerde schijf kopiëren (verplaatsen) naar hetzelfde of een ander abonnement'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 744990b06b5585763615eb69909c29a27dee5a7f
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249525"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Met CLI een momentopname van een beheerde schijf kopiëren naar hetzelfde of een ander abonnement

Met dit script wordt een momentopname van een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement. Gebruik dit script voor de volgende scenario's:

1. Een momentopname in Premium-opslag (Premium_LRS) migreren naar de standaardopslag (Standard_LRS of Standard_ZRS) om uw kosten te beperken.
1. Een momentopname van lokaal redundante opslag (Premium_LRS, Standard_LRS) migreren naar zone-redundante opslag (Standard_ZRS) om te profiteren van de hogere betrouwbaarheid van ZRS-opslag.
1. Een momentopname van een verplaatsen naar een ander abonnement in dezelfde regio voor een langere periode.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een momentopname te maken in het doelabonnement met behulp van de id van de bronmomentopname. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot) | Hiermee haalt u alle eigenschappen van een momentopname op aan de hand van de naam en de eigenschappen van de resourcegroep van de momentopname. De id-eigenschap wordt gebruikt om de momentopname naar een ander abonnement te kopiëren.  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot) | Hiermee wordt een momentopname gekopieerd door een momentopname te maken in een ander abonnement met dezelfde id en naam als de bovenliggende momentopname.  |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Extra virtuele machines en beheerde schijven CLI-voorbeeldscripts vindt u de [documentatie voor Azure Windows VM's](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
