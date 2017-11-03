---
title: Azure PowerShell-Script voorbeeld - Load balance verkeer naar VM's voor hoge beschikbaarheid | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Load balance verkeer naar VM's voor hoge beschikbaarheid
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: c77def8906b151f2cc6e4bbc4188be8ecbeac732
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Load balance verkeer naar VM's voor hoge beschikbaarheid

Dit voorbeeldscript wordt gemaakt van alles wat u nodig voor het uitvoeren van verschillende Windows virtuele machines geconfigureerd in een maximaal beschikbare en taakverdeling configuratie laden. Nadat het script is uitgevoerd, hebt u drie virtuele machines, die lid zijn van een Azure Beschikbaarheidsset, en toegankelijk zijn via een Azure Load Balancer.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine, beschikbaarheidsset, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u de subnetconfiguratie van een. Deze configuratie wordt gebruikt met het proces voor het virtueel netwerk maken. |
| [Nieuwe-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Maakt een virtueel Azure-netwerk en subnet. |
| [Nieuwe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Hiermee maakt een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [Nieuwe AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Hiermee maakt u een Azure load balancer. |
| [Nieuwe AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Hiermee maakt u een load balancer-test. Een load balancer-test wordt gebruikt voor het bewaken van elke virtuele machine in de load balancer-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet doorgestuurd naar de virtuele machine. |
| [Nieuwe AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Maakt een regel voor load balancer. In dit voorbeeld wordt een regel gemaakt voor poort 80. HTTP-verkeer ontvangt bij de load balancer wordt doorgestuurd naar poort 80 een van de virtuele machines in de load balancer-set. |
| [Nieuwe AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Maakt een regel voor load balancer NAT (Network Address Translation).  Een poort van de load balancer toewijzen NAT-regels aan een poort op een virtuele machine. In dit voorbeeld wordt een NAT-regel gemaakt voor SSH-verkeer naar elke virtuele machine in de load balancer-set.  |
| [Nieuwe AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Maakt een netwerkbeveiligingsgroep (NSG), die een beveiligingsgrens tussen het internet en de virtuele machine. |
| [Nieuwe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Maakt een NSG-regel voor binnenkomend verkeer toestaan. In dit voorbeeld wordt poort 22 voor SSH-verkeer geopend. |
| [Nieuwe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u een virtueel netwerkkaart en gekoppeld aan het virtuele netwerk, subnet en NSG. |
| [Nieuwe AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Hiermee maakt een beschikbaarheidsset. Beschikbaarheidssets waarborgen uptime van toepassingen via de virtuele machines verspreid over fysieke resources zo dat als fout optreedt, wordt de hele set niet gedaan. |
| [Nieuwe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Maakt een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties VM. De configuratie wordt gebruikt tijdens het maken van VM. |
| [Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | De virtuele machine maakt en met de netwerkkaart, virtueel netwerk, subnet en NSG is verbonden. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine om te worden gebruikt en administratieve referenties.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
