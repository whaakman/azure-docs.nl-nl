---
title: Azure CLI-voorbeeldscript - taakverdeling toe te passen meerdere websites met de Azure CLI | Microsoft Docs
description: Azure CLI-voorbeeldscript - taakverdeling toe te passen meerdere websites op de virtuele machine
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
ms.openlocfilehash: 98b07bfabf2d01c7ae3db7365cfbab3639c6f026
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-multiple-websites"></a>Taakverdeling maken voor meerdere websites

Dit voorbeeldscript wordt een virtueel netwerk maakt met twee virtuele machines (VM) die lid van een beschikbaarheidsset zijn. Een load balancer wordt doorgestuurd verkeer voor twee afzonderlijke IP-adressen naar de twee virtuele machines. Nadat het script is uitgevoerd, kan u implementeren web server-software aan de virtuele machines en de host meerdere websites, elk met een eigen IP-adres.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtueel netwerk, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ network vnet maken](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Maakt een virtueel Azure-netwerk en subnet. |
| [AZ netwerk openbare ip-maken](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Hiermee maakt een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [AZ network Load Balancer maken](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Hiermee maakt u een Azure Load Balancer. |
| [AZ network Load Balancer-test maken](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Hiermee maakt u een load balancer-test. Een load balancer-test wordt gebruikt voor het bewaken van elke virtuele machine in de load balancer-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet doorgestuurd naar de virtuele machine. |
| [AZ network Load Balancer-regel maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Maakt een regel voor load balancer. In dit voorbeeld wordt een regel gemaakt voor poort 80. HTTP-verkeer ontvangt bij de load balancer wordt doorgestuurd naar poort 80 een van de virtuele machines in de load balancer-set. |
| [AZ netwerk lb frontend-IP-maken](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#az_network_lb_frontend_ip_create) | Maak een frontend-IP-adres voor de Load Balancer. |
| [AZ netwerk lb-adresgroep maken](https://docs.microsoft.com/cli/azure/network/lb/address-pool#az_network_lb_address_pool_create) | Maakt een back-end-adresgroep. |
| [AZ netwerk nic maken](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Hiermee maakt u een virtueel netwerkkaart en gekoppeld aan het virtuele netwerk en subnet. |
| [AZ vm beschikbaarheidsset maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Hiermee maakt een beschikbaarheidsset. Beschikbaarheidssets waarborgen uptime van toepassingen via de virtuele machines verspreid over fysieke resources zo dat als fout optreedt, wordt de hele set niet gedaan. |
| [AZ netwerk nic ip-configuratie maken](https://docs.microsoft.com/cli/azure/network/nic/ip-config#az_network_nic_ip_config_create) | Hiermee maakt u een IP-confiuration. U moet de functie Microsoft.Network/AllowMultipleIpConfigurationsPerNic is ingeschakeld voor uw abonnement hebben. De primaire IP-configuratie per NIC, kan slechts één configuratie worden aangeduid met de--vlag primair maken. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine om te worden gebruikt en administratieve referenties.  |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende netwerken CLI scriptvoorbeelden vindt u in de [overzicht van Azure-netwerken documentatie](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
