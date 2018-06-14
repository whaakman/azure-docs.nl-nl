---
title: Azure CLI-voorbeeldscript - routeverkeer via een virtueel netwerkapparaat | Microsoft Docs
description: 'Azure CLI - voorbeeldscript: de Route-verkeer via een firewall netwerk virtueel apparaat.'
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
ms.openlocfilehash: 24b0c6873721ef196b1c0dc6d6a357f87a4a5e39
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846363"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

Dit voorbeeldscript wordt een virtueel netwerk gemaakt met front-end en back-end-subnetten. Een virtuele machine wordt gemaakt met doorsturen via IP is ingeschakeld voor het routeren van netwerkverkeer tussen de twee subnetten. U kunt na het uitvoeren van het script netwerksoftware, zoals een firewalltoepassing naar de virtuele machine implementeren.

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

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, het virtuele netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en de front-end-subnet. |
| [AZ netwerksubnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Maakt back-end en DMZ subnetten. |
| [AZ netwerk openbare ip-maken](/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via het Internet. |
| [AZ netwerk nic maken](/cli/azure/network/nic#az_network_nic_create) | Maakt een virtuele netwerkinterface en doorsturen via IP inschakelen voor deze. |
| [nsg voor AZ netwerk maken](/cli/azure/network/nsg#az_network_nsg_create) | Maakt een netwerkbeveiligingsgroep (NSG). |
| [AZ netwerk nsg regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | NSG-regels waarmee HTTP en HTTPS-poorten inkomend naar de virtuele machine maakt. |
| [AZ network vnet subnet bijwerken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Koppelt de nsg's en routetabellen aan subnetten. |
| [AZ netwerk routetabel maken](/cli/azure/network/route-table#az_network_route_table_create)| Een routetabel voor alle routes maken. |
| [netwerkroute AZ-routetabel maken](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Routes naar verkeer leiden tussen subnetten en het Internet via de virtuele machine maakt. |
| [AZ vm maken](/cli/azure/vm#az_vm_create) | Hiermee maakt u een virtuele machine en de NIC koppelt aan het. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine te gebruiken en de beheerdersreferenties. |
| [az group delete](/cli/azure/group#az_group_delete) | Hiermee verwijdert u een resourcegroep en alle resources die deze bevat. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende netwerken CLI scriptvoorbeelden vindt u in de [overzicht netwerken van Azure-documentatie](../cli-samples.md)