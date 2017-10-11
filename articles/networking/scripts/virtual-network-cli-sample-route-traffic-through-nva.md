---
title: Azure CLI-voorbeeldscript - routeverkeer via een virtueel netwerkapparaat | Microsoft Docs
description: 'Azure CLI - voorbeeldscript: de Route-verkeer via een firewall netwerk virtueel apparaat.'
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
ms.openlocfilehash: 78091b515c00591a4af8d807945475b6be50188a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

Dit voorbeeldscript wordt een virtueel netwerk gemaakt met front-end en back-end-subnetten. Een virtuele machine wordt gemaakt met doorsturen via IP is ingeschakeld voor het routeren van netwerkverkeer tussen de twee subnetten. U kunt na het uitvoeren van het script netwerksoftware, zoals een firewalltoepassing naar de virtuele machine implementeren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Voorbeeld van een script


[!code-azurecli-interactive[belangrijkste](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "verkeer routeren via een virtueel netwerkapparaat")]

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
| [AZ netwerksubnet maken](/cli/azure/network/vnet/subnet#create) | Maakt back-end en DMZ subnetten. |
| [AZ netwerk openbare ip-maken](/cli/azure/network/public-ip#create) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via het Internet. |
| [AZ netwerk nic maken](/cli/azure/network/nic#create) | Maakt een virtuele netwerkinterface en doorsturen via IP inschakelen voor deze. |
| [nsg voor AZ netwerk maken](/cli/azure/network/nsg#create) | Maakt een netwerkbeveiligingsgroep (NSG). |
| [AZ netwerk nsg regel maken](/cli/azure/network/nsg/rule#create) | NSG-regels waarmee HTTP en HTTPS-poorten inkomend naar de virtuele machine maakt. |
| [AZ network vnet subnet bijwerken](/cli/azure/network/vnet/subnet#update)| Koppelt de nsg's en routetabellen aan subnetten. |
| [AZ netwerk routetabel maken](/cli/azure/network/route-table#create)| Een routetabel voor alle routes maken. |
| [netwerkroute AZ-routetabel maken](/cli/azure/network/route-table/route#create)| Routes naar verkeer leiden tussen subnetten en het Internet via de virtuele machine maakt. |
| [AZ vm maken](/cli/azure/vm#create) | Hiermee maakt u een virtuele machine en de NIC koppelt aan het. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine te gebruiken en de beheerdersreferenties. |
| [AZ groep verwijderen](/cli/azure/group#delete) | Hiermee verwijdert u een resourcegroep en alle resources die deze bevat. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](/cli/azure/overview).

Aanvullende netwerken CLI scriptvoorbeelden vindt u in de [overzicht netwerken van Azure-documentatie](../cli-samples.md)