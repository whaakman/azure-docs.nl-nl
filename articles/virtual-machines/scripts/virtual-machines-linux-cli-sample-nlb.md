---
title: Azure CLI Script voorbeeld - een virtuele Linux-machine maken met NLB | Microsoft Docs
description: Azure CLI Script voorbeeld - een virtuele Linux-machine maken met NLB
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fdddf54dceab23394dd26a5c10fa57b921f8cd34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-highly-available-vm"></a>Een maximaal beschikbare virtuele machine maken

Dit voorbeeldscript wordt gemaakt van alles wat u nodig voor het uitvoeren van verschillende Ubuntu virtuele machines geconfigureerd in een maximaal beschikbare en taakverdeling configuratie laden. Nadat het script is uitgevoerd, hebt u drie virtuele machines, die lid zijn van een Azure Beschikbaarheidsset, en toegankelijk zijn via een Azure Load Balancer. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine, beschikbaarheidsset, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en subnet. |
| [AZ netwerk openbare ip-maken](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [AZ network Load Balancer maken](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Hiermee maakt u een Azure-netwerk Load Balancer (NLB). |
| [AZ network Load Balancer-test maken](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Maakt een NLB-test. Een NLB-test wordt gebruikt voor het bewaken van elke virtuele machine in de set met NLB. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet doorgestuurd naar de virtuele machine. |
| [AZ network Load Balancer-regel maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Maakt een regel voor NLB. In dit voorbeeld wordt een regel gemaakt voor poort 80. Als HTTP-verkeer bij de NLB aankomt, wordt deze doorgestuurd naar poort 80 een van de virtuele machines in de set met NLB. |
| [AZ netwerk lb-nat-regel voor binnenkomende verbindingen maken](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#az_network_lb_inbound_nat_rule_create) | Maakt een regel voor NLB Network Address Translation (NAT).  Een poort van de NLB toewijzen NAT-regels aan een poort op een virtuele machine. In dit voorbeeld wordt een NAT-regel gemaakt voor SSH-verkeer naar elke virtuele machine in de set met NLB.  |
| [nsg voor AZ netwerk maken](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Maakt een netwerkbeveiligingsgroep (NSG), die een beveiligingsgrens tussen het internet en de virtuele machine. |
| [AZ netwerk nsg regel maken](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Maakt een NSG-regel voor binnenkomend verkeer toestaan. In dit voorbeeld wordt poort 22 voor SSH-verkeer geopend. |
| [AZ netwerk nic maken](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Hiermee maakt u een virtueel netwerkkaart en gekoppeld aan het virtuele netwerk, subnet en NSG. |
| [AZ vm beschikbaarheidsset maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Hiermee maakt een beschikbaarheidsset. Beschikbaarheidssets waarborgen uptime van toepassingen via de virtuele machines verspreid over fysieke resources zo dat als fout optreedt, wordt de hele set niet gedaan. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine om te worden gebruikt en administratieve referenties.  |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
