---
title: Azure CLI-voorbeeldscript - verkeer routeren via een virtueel netwerkapparaat | Microsoft Docs
description: Azure CLI-voorbeeldscript - verkeer routeren via een virtueel netwerkapparaat met firewall.
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
ms.openlocfilehash: 110e8c6a57cd2cdfedec808e073f04b70a55362d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465570"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Verkeer routeren via een virtueel netwerkapparaat

Met dit voorbeeldscript wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Er wordt ook een virtuele machine gemaakt waarvoor doorsturen via IP is ingeschakeld, voor het routeren van netwerkverkeer tussen de twee subnetten. Na het uitvoeren van het script kunt u netwerksoftware, bijvoorbeeld een firewalltoepassing, op de VM implementeren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Voorbeeldscript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](/cli/azure/network/vnet) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Hiermee maakt u back-end- en DMZ-subnetten. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via Internet. |
| [az network nic create](/cli/azure/network/nic) | Hiermee maakt u een virtuele netwerkinterface en schakelt u doorsturen via IP in voor deze interface. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Hiermee maakt u een netwerkbeveiligingsgroep (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Hiermee maakt u NSG-regels waarmee inkomend verkeer via HTTP- en HTTPS-poorten naar de VM worden toegestaan. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Hiermee koppelt u de NSG's en routetabellen aan subnetten. |
| [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create)| Hiermee maakt u een routetabel voor alle routes. |
| [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create)| Hiermee maakt u routes om verkeer te routeren tussen subnetten en Internet via de virtuele machine. |
| [az vm create](/cli/azure/vm#az_vm_create) | Hiermee maakt u een virtuele machine en koppelt u de NIC hieraan. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, evenals de beheerdersreferenties. |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep en de bijhorende resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende netwerken CLI-voorbeeldscripts vindt u de [netwerken overzicht van Azure-documentatie](../cli-samples.md)
