---
title: Voorbeelden van Azure PowerShell - Schaalset met één zone | Microsoft Docs
description: Voorbeelden van Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4e4f6d26c119a113bab0220828c847f438e7bc7a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-single-zone-virtual-machine-scale-set-with-powershell"></a>Een virtuele-machineschaalset met één zone maken met PowerShell
Dit script maakt een virtuele-machineschaalset met Windows Server 2016 in een enkele Beschikbaarheidszone. Nadat het script is uitgevoerd, hebt u via RDP toegang tot de virtuele machine.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1 "Create single-zone scale set")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep, de schaalset en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script
In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u een configuratieobject dat het subnet van het virtuele netwerk definieert. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel netwerk en subnet. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een statisch openbaar IP-adres. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Hiermee maakt u een configuratieobject dat de front-end van de load balancer definieert, met inbegrip van het openbare IP-adres. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Hiermee maakt u een configuratieobject dat de back-end van de load balancer definieert. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Hiermee maakt u een load balancer op basis van de front-end en back-end configuratieobjecten. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Hiermee maakt u een load balancer-statustest voor het bewaken van verkeer op TCP-poort 80. Als twee achtereenvolgende mislukte pogingen met intervallen van 15 seconden optreden, wordt het eindpunt als niet in orde beschouwd. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Hiermee maakt u een configuratieobject dat de load balancer-regels definieert om verkeer te distribueren op TCP-poort 80 van de front-endpool naar de back-endpool. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Hiermee werkt u de load balancer bij met de statustest en load balancer-regels. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Hiermee maakt u een configuratieobject dat een groep netwerkbeveiligingsregels definieert voor verkeer op TCP-poort 80. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hiermee maakt u een netwerkbeveiligingsgroep en -regel. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Hiermee werkt u het subnet van het virtuele netwerk bij dat aan de netwerkbeveiligingsgroep is gekoppeld. Elk VM-exemplaar dat is verbonden met het subnet, neemt de regels voor netwerkbeveiligingsgroepen over. |
| [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) | Hiermee werkt u het virtuele netwerk met de configuratie van het subnet en de netwerkbeveiligingsgroep bij. |
| [New-AzureRmVmssIpConfig](/powershell/module/azurerm.compute/new-azurermvmssipconfig) | Hiermee maakt u een configuratieobject dat de IP-adresgegevens van de virtuele-machineschaalset definieert voor het verbinden van VM-exemplaren met de load balancer van de back-endpool en de binnenkomende NAT-pool.
| [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) | Hiermee maakt u een configuratieobject dat het aantal VM-exemplaren in een schaalset, de grootte van het VM-exemplaar en de modus voor upgradebeleid definieert. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/azurerm.compute/set-azurermvmssstorageprofile) | Hiermee maakt u een configuratieobject waarin de VM-installatiekopie moet worden gebruikt voor de VM-exemplaren. |
| [Set-AzureRmVmssOsProfile](/powershell/module/azurerm.compute/set-azurermvmssosprofile) | Hiermee maakt u een configuratieprofiel dat de naam en de gebruikersreferenties van het VM-exemplaar definieert. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/azurerm.compute/add-azurermvmssnetworkinterfaceconfiguration) | Hiermee maakt u een configuratieobject dat de adapterkaart van het virtuele netwerk definieert voor elk VM-exemplaar. |
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Hiermee combineert u alle configuratieobjecten voor het maken van de virtuele-machineschaalset. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Hiermee wordt informatie over de virtuele-machineschaalset opgehaald. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Hiermee voegt u een VM-extensie voor Custom Script toe om een eenvoudige webtoepassing te installeren. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Hiermee werkt u het model van de virtuele-machineschaalset bij om de VM-extensie toe te passen. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Hiermee haalt u informatie op over het toegewezen openbare IP-adres dat door de load balancer wordt gebruikt. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. |

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

U kunt extra PowerShell-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor virtuele-machineschaalsets](../powershell-samples.md).