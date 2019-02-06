---
title: Azure CLI-voorbeeldscript - VM-netwerkverkeer filteren | Microsoft Docs
description: Azure CLI-voorbeeldscript - binnenkomend en uitgaand VM-netwerkverkeer filteren.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
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
ms.author: jdial
ms.openlocfilehash: 6045cabdc97dad72a54e28a6f4fbee3126056d2e
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55748230"
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Binnenkomend en uitgaand VM-netwerkverkeer filteren

Met dit voorbeeldscript wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Binnenkomend netwerkverkeer naar het front-end-subnet is beperkt tot HTTP, HTTPS en SSH, terwijl uitgaand verkeer naar Internet vanuit de back-end-subnet is niet toegestaan. Nadat het script is uitgevoerd, hebt u een virtuele machine met twee NIC's. Elke NIC is verbonden met een ander subnet.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](/cli/azure/network/vnet) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Hiermee maakt u een back-end-subnet. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet) | Hiermee koppelt u NSG's aan subnetten. |
| [az network public-ip create](/cli/azure/network/public-ip) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via Internet. |
| [az network nic create](/cli/azure/network/nic) | Hiermee maakt u virtuele netwerkinterfaces en koppelt u ze aan de front-end- en back-end-subnetten van het virtuele netwerk. |
| [az network nsg create](/cli/azure/network/nsg) | Hiermee maakt u netwerkbeveiligingsgroepen (NSG's) die zijn gekoppeld aan de front-end- en back-end-subnetten. |
| [az network nsg rule create](/cli/azure/network/nsg/rule) |Hiermee maakt u NSG-regels die bepaalde poorten tot specifieke subnetten blokkeren of toestaan. |
| [az vm create](/cli/azure/vm) | Hiermee maakt u virtuele machines en koppelt u een NIC aan elke virtuele machine. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, evenals de beheerdersreferenties. |
| [az group delete](/cli/azure/group) | Hiermee verwijdert u een resourcegroep en de bijhorende resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende netwerken CLI-voorbeeldscripts vindt u de [netwerken overzicht van Azure-documentatie](../cli-samples.md)
