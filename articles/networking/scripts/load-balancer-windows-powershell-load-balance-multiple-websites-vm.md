---
title: Azure PowerShell-voorbeeldscript - taakverdeling toe te passen meerdere websites met Azure PowerShell | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - taakverdeling toe te passen meerdere websites op de virtuele machine
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
ms.openlocfilehash: 44cb76a09cdaacd1bffd653bae6a5735a81dc6f6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31597816"
---
# <a name="load-balance-multiple-websites"></a>Taakverdeling maken voor meerdere websites

Dit voorbeeldscript wordt een virtueel netwerk maakt met twee virtuele machines (VM) die lid van een beschikbaarheidsset zijn. Een load balancer wordt doorgestuurd verkeer voor twee afzonderlijke IP-adressen naar de twee virtuele machines. Nadat het script is uitgevoerd, kan u implementeren web server-software aan de virtuele machines en de host meerdere websites, elk met een eigen IP-adres.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) en voer vervolgens `Connect-AzureRmAccount` uit om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtueel netwerk, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Hiermee maakt u een Azure beschikbaarheidsset voor maximale beschikbaarheid. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u een subnetconfiguratie. Deze configuratie wordt gebruikt bij het maken van het virtueel netwerk. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel netwerk. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Maakt een front-end-IP-configuratie voor een load balancer. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Maakt een back-end-pool-adresconfiguratie voor een load balancer. |
| [Nieuwe AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Hiermee maakt u een NLB-probe. Een NLB-probe wordt gebruikt voor het controleren van elke virtuele machine in de NLB-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet meer naar die virtuele machine doorgestuurd. |
| [Nieuwe AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Hiermee maakt u een NLB-regel. In dit voorbeeld wordt een regel gemaakt voor poort 80. Wanneer HTTP-verkeer bij de NLB aankomt, wordt dat doorgestuurd naar poort 80 van een van de virtuele machines in de NLB-set. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Hiermee maakt u een load balancer. |
| [Nieuwe AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Hiermee definieert u geavanceerde functies voor de virtuele netwerkinterface. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u een netwerkinterface. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Hiermee maakt u een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties van de virtuele machine. De configuratie wordt gebruikt tijdens het maken van de virtuele machine. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
