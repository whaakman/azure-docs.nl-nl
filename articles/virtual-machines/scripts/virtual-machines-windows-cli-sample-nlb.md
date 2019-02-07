---
title: 'Azure CLI-voorbeeldscript: een Windows Server 2016-VM met NLB maken | Microsoft Docs'
description: 'Azure CLI-voorbeeldscript: een Windows Server 2016-VM met NLB maken'
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: jeconnoc
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 76e7d550c53ad2e9d1c1a1bab5ea4000279fcaa4
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700414"
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Zorgen voor verkeer met gelijke taakverdeling tussen maximaal beschikbare virtuele machines

Met dit voorbeeldscript maakt u alles wat nodig is voor het uitvoeren van verschillende virtuele Ubuntu-machines die zijn geconfigureerd in een maximaal beschikbare configuratie met gelijke taakverdeling. Nadat het script is uitgevoerd, beschikt u over drie virtuele machines, die gekoppeld zijn aan een Azure-beschikbaarheidsset en toegankelijk zijn via een Azure Load Balancer.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-windows-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, een virtuele machine, een beschikbaarheidsset, een load balancer en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Hiermee maakt u een virtueel Azure-netwerk en -subnet. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) | Hiermee maakt u een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) | Hiermee maakt u een Azure Network Load Balancer (NLB). |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) | Hiermee maakt u een NLB-probe. Een NLB-probe wordt gebruikt voor het controleren van elke virtuele machine in de NLB-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet meer naar die virtuele machine doorgestuurd. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Hiermee maakt u een NLB-regel. In dit voorbeeld wordt een regel gemaakt voor poort 80. Wanneer HTTP-verkeer bij de NLB aankomt, wordt dat doorgestuurd naar poort 80 van een van de virtuele machines in de NLB-set. |
| [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) | Maakt een regel voor NLB Network Address Translation (NAT).  NAT-regels wijzen een poort van de NLB toe aan een poort op een virtuele machine. In dit voorbeeld wordt een NAT-regel gemaakt voor SSH-verkeer naar elke virtuele machine in de NLB-set.  |
| [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg) | Hiermee maakt u een netwerkbeveiligingsgroep (NSG), die fungeert als een beveiligingsgrens tussen het internet en de virtuele machine. |
| [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Hiermee maakt u een NSG-regel om binnenkomend verkeer toe te staan. In dit voorbeeld wordt poort 22 geopend voor SSH-verkeer. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic) | Hiermee maakt u een netwerkkaart die gekoppeld wordt aan het virtuele netwerk, het subnet en de NSG. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Hiermee maakt u een beschikbaarheidsset. Beschikbaarheidssets waarborgen de uptime van toepassingen door de virtuele machines te verdelen over fysieke resources, zodat als er een fout optreedt, niet de hele set wordt getroffen. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Hiermee maakt u de virtuele machine en verbindt u deze met de netwerkkaart, het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, samen met beheerdersreferenties.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-scriptvoorbeelden voor virtuele machines vindt u in de [Azure-documentatie voor Windows-VM's](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
