---
title: Azure CLI-voorbeeldscript - Peer twee virtuele netwerken | Microsoft Docs
description: Azure CLI-voorbeeldscript - Peer twee virtuele netwerken
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 8ad1e7de85f851b5db6764175bc1136dd19d418d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29852367"
---
# <a name="peer-two-virtual-networks"></a>Peer twee virtuele netwerken

Dit script wordt gemaakt en twee virtuele netwerken in de dezelfde regio trhough het Azure-netwerk met elkaar verbindt. Nadat het script is uitgevoerd, maakt u een peering tussen twee virtuele netwerken.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en subnet. |
| [AZ network vnet-peering maken](https://docs.microsoft.com/cli/azure/network/vnet/peering#az_network_vnet_peering_create) | Maakt een peering tussen twee virtuele netwerken.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende netwerken CLI scriptvoorbeelden vindt u in de [overzicht van Azure-netwerken documentatie](../cli-samples.md).
