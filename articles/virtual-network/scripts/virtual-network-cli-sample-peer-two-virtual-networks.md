---
title: Azure CLI-voorbeeldscript - Peer twee virtuele netwerken | Microsoft Docs
description: 'Azure CLI - voorbeeldscript: de Peer twee virtuele netwerken.'
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 244d7f6ff64643386c417d708f7fb1e9bbc34209
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="peer-two-virtual-networks"></a>Peer twee virtuele netwerken

Dit script wordt gemaakt en twee virtuele netwerken in dezelfde regio verbindt via het netwerk van Azure. Wanneer het script wordt uitgevoerd, beschikt u over een peering tussen twee virtuele netwerken.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/bash), of vanuit een lokale installatie van de Azure CLI. Als u de CLI lokaal gebruikt, wordt dit script vereist dat u versie 2.0.28 uitvoert of hoger. Ga voor de geïnstalleerde versie uitvoeren `az --version`. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtuele machine en alle gerelateerde resources. Elke opdracht in de volgende tabel is gekoppeld aan de opdracht specifieke documentatie bij:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Hiermee maakt u een virtueel Azure-netwerk en -subnet. |
| [AZ network vnet-peering maken](/cli/azure/network/vnet/peering#az_network_vnet_peering_create) | Maakt een peering tussen twee virtuele netwerken.  |
| [az group delete](/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Extra virtuele netwerk CLI scriptvoorbeelden kunnen u vinden in [virtuele netwerk CLI voorbeelden](../cli-samples.md).
