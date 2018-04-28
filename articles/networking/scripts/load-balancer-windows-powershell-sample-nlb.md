---
title: Azure PowerShell-Script voorbeeld - Load balance verkeer naar VM's voor hoge beschikbaarheid | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Load balance verkeer naar VM's voor hoge beschikbaarheid
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 61d65cbdcf7867cc7a2a225648dc8b1ab7137bcb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Load balance verkeer naar VM's voor hoge beschikbaarheid

Dit voorbeeldscript wordt gemaakt van alles wat u nodig voor het uitvoeren van verschillende Windows virtuele machines geconfigureerd in een maximaal beschikbare en taakverdeling configuratie laden. Nadat het script is uitgevoerd, beschikt u over drie virtuele machines, die gekoppeld zijn aan een Azure-beschikbaarheidsset en toegankelijk zijn via een Azure Load Balancer.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) en voer vervolgens `Connect-AzureRmAccount` uit om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, een virtuele machine, een beschikbaarheidsset, een load balancer en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u een subnetconfiguratie. Deze configuratie wordt gebruikt bij het maken van het virtueel netwerk. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel Azure-netwerk en -subnet. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Hiermee maakt u een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Hiermee maakt u een Azure load balancer. |
| [Nieuwe AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Hiermee maakt u een load balancer-test. Een load balancer-test wordt gebruikt voor het bewaken van elke virtuele machine in de load balancer-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet meer naar die virtuele machine doorgestuurd. |
| [Nieuwe AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Maakt een regel voor load balancer. In dit voorbeeld wordt een regel gemaakt voor poort 80. HTTP-verkeer ontvangt bij de load balancer wordt doorgestuurd naar poort 80 een van de virtuele machines in de load balancer-set. |
| [Nieuwe AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Maakt een regel voor load balancer NAT (Network Address Translation).  Een poort van de load balancer toewijzen NAT-regels aan een poort op een virtuele machine. In dit voorbeeld wordt een NAT-regel gemaakt voor SSH-verkeer naar elke virtuele machine in de load balancer-set.  |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hiermee maakt u een netwerkbeveiligingsgroep (NSG), die fungeert als een beveiligingsgrens tussen het internet en de virtuele machine. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Hiermee maakt u een NSG-regel om binnenkomend verkeer toe te staan. In dit voorbeeld wordt poort 22 geopend voor SSH-verkeer. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u een netwerkkaart die gekoppeld wordt aan het virtuele netwerk, het subnet en de NSG. |
| [Nieuwe AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Hiermee maakt u een beschikbaarheidsset. Beschikbaarheidssets waarborgen de uptime van toepassingen door de virtuele machines te verdelen over fysieke resources, zodat als er een fout optreedt, niet de hele set wordt getroffen. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Hiermee maakt u een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties van de virtuele machine. De configuratie wordt gebruikt tijdens het maken van de virtuele machine. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Hiermee maakt u de virtuele machine en verbindt u deze met de netwerkkaart, het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, samen met beheerdersreferenties.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
