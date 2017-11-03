---
title: Azure CLI-script steekproef - maken van een netwerk voor toepassingen met meerdere lagen | Microsoft Docs
description: 'Azure CLI-script voorbeeld: een virtueel netwerk voor toepassingen met meerdere lagen maken.'
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Een netwerk voor toepassingen met meerdere lagen maken

Dit voorbeeldscript wordt een virtueel netwerk gemaakt met front-end en back-end-subnetten. Het verkeer naar de front-end-subnet is beperkt tot HTTP- en SSH, terwijl het verkeer naar de back-end-subnet wordt beperkt tot MySQL, poort 3306. Nadat het script is uitgevoerd, hebt u twee virtuele machines, één in elk subnet dat u kunt webserver en MySQL-software te implementeren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Voorbeeld van een script


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, het virtuele netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](/cli/azure/group#create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](/cli/azure/network/vnet#create) | Maakt een virtueel Azure-netwerk en de front-end-subnet. |
| [AZ netwerksubnet maken](/cli/azure/network/vnet/subnet#create) | Maakt een back-end-subnet. |
| [AZ netwerk openbare ip-maken](/cli/azure/network/public-ip#create) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via het Internet. |
| [AZ netwerk nic maken](/cli/azure/network/nic#create) | Virtuele netwerkinterfaces maakt en gekoppeld aan het virtuele netwerk front-end en back-end-subnetten. |
| [nsg voor AZ netwerk maken](/cli/azure/network/nsg#create) | Netwerk maakt beveiligingsgroepen (NSG) die gekoppeld aan de front-end en back-end subnetten zijn. |
| [AZ netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) |Maakt NSG-regels die bepaalde poorten tot specifieke subnetten blokkeren of toestaan. |
| [AZ vm maken](/cli/azure/vm#create) | Hiermee maakt u virtuele machines en een NIC koppelt aan elke virtuele machine. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine te gebruiken en de beheerdersreferenties. |
| [AZ groep verwijderen](/cli/azure/group#delete) | Hiermee verwijdert u een resourcegroep en alle resources die deze bevat. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](/cli/azure/overview).

Aanvullende netwerken CLI scriptvoorbeelden vindt u in de [overzicht netwerken van Azure-documentatie](../cli-samples.md)