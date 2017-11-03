---
title: 'Azure PowerShell-Script voorbeeld: Maak een VM van Windows NLB | Microsoft Docs'
description: 'Azure PowerShell-Script voorbeeld: Maak een VM van Windows NLB'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 25bcbbcd1615e01a384825d7bd1582a528e91f71
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Load balance verkeer tussen de maximaal beschikbare virtuele machines

Dit voorbeeldscript wordt gemaakt van alles wat u nodig voor het uitvoeren van meerdere virtuele machines van het Windows Server 2016 geconfigureerd in een maximaal beschikbare en taakverdeling configuratie laden. Nadat het script is uitgevoerd, hebt u drie virtuele machines, die lid zijn van een Azure Beschikbaarheidsset, en toegankelijk zijn via een Azure Load Balancer.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de implementatie te maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u de subnetconfiguratie van een. Deze configuratie wordt gebruikt met het proces voor het virtueel netwerk maken. |
| [Nieuwe-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel netwerk. |
| [Nieuwe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres. |
| [Nieuwe AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Maakt een front-end-IP-configuratie voor een load balancer. |
| [Nieuwe AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Maakt een back-end-pool-adresconfiguratie voor een load balancer. |
| [Nieuwe AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Maakt een test-configuratie voor een load balancer. |
| [Nieuwe AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Maakt de regelconfiguratie van een voor een load balancer. |
| [Nieuwe AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Maakt een binnenkomende NAT-regelconfiguratie voor een load balancer. |
| [Nieuwe AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Hiermee maakt u een load balancer. |
| [Nieuwe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Maakt een groep regel de netwerkbeveiligingsconfiguratie. Deze configuratie wordt gebruikt voor het maken van een regel voor het NSG wanneer het NSG wordt gemaakt. |
| [Nieuwe AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Een netwerkbeveiligingsgroep maakt. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Hiermee haalt u informatie over het subnet. Deze informatie wordt gebruikt bij het maken van een netwerkinterface. |
| [Nieuwe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Maakt een netwerkinterface. |
| [Nieuwe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Maakt een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties VM. De configuratie wordt gebruikt tijdens het maken van VM. |
| [Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Maak een virtuele machine. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle resources binnen. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
