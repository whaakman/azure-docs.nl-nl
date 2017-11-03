---
title: Azure CLI-voorbeeldscript - Load balance verkeer naar VM's voor hoge beschikbaarheid | Microsoft Docs
description: Azure CLI-voorbeeldscript - Load balance verkeer naar VM's voor hoge beschikbaarheid
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: b04e9b22fa35f6202bc3fee547519b679f439a3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Load balance verkeer naar VM's voor hoge beschikbaarheid

Dit voorbeeldscript wordt gemaakt van alles wat u nodig voor het uitvoeren van verschillende Ubuntu virtuele machines geconfigureerd in een maximaal beschikbare en taakverdeling configuratie laden. Nadat het script is uitgevoerd, hebt u drie virtuele machines, die lid zijn van een Azure Beschikbaarheidsset, en toegankelijk zijn via een Azure Load Balancer. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine, beschikbaarheidsset, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en subnet. |
| [AZ netwerk openbare ip-maken](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [AZ network Load Balancer maken](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Hiermee maakt u een Azure load balancer. |
| [AZ network Load Balancer-test maken](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Hiermee maakt u een load balancer-test. Een load balancer-test wordt gebruikt voor het bewaken van elke virtuele machine in de load balancer-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet doorgestuurd naar de virtuele machine. |
| [AZ network Load Balancer-regel maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Maakt een regel voor load balancer. In dit voorbeeld wordt een regel gemaakt voor poort 80. HTTP-verkeer ontvangt bij de load balancer wordt doorgestuurd naar poort 80 een van de virtuele machines in de LB-set. |
| [AZ netwerk lb-nat-regel voor binnenkomende verbindingen maken](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#az_network_lb_inbound_nat_rule_create) | Regel voor load balancer Network Address Translation (NAT) maakt.  Een poort van de load balancer toewijzen NAT-regels aan een poort op een virtuele machine. In dit voorbeeld wordt een NAT-regel gemaakt voor SSH-verkeer naar elke virtuele machine in de load balancer-set.  |
| [nsg voor AZ netwerk maken](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Maakt een netwerkbeveiligingsgroep (NSG), die een beveiligingsgrens tussen het internet en de virtuele machine. |
| [AZ netwerk nsg regel maken](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Maakt een NSG-regel voor binnenkomend verkeer toestaan. In dit voorbeeld wordt poort 22 voor SSH-verkeer geopend. |
| [AZ netwerk nic maken](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Hiermee maakt u een virtueel netwerkkaart en gekoppeld aan het virtuele netwerk, subnet en NSG. |
| [AZ vm beschikbaarheidsset maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Hiermee maakt een beschikbaarheidsset. Beschikbaarheidssets waarborgen uptime van toepassingen via de virtuele machines verspreid over fysieke resources zo dat als fout optreedt, wordt de hele set niet gedaan. |
| [AZ vm maken](/cli/azure/vm#create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine om te worden gebruikt en administratieve referenties.  |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende voorbeelden van Azure toegang CLI script kunnen u vinden in de [documentatie Azure Networking](../cli-samples.md).