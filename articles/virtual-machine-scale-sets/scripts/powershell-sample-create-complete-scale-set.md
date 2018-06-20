---
title: Voorbeelden van Azure PowerShell - Een volledige virtuele-machineschaalset maken | Microsoft Docs
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
ms.date: 05/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5bbdcf8b45af6cf0c56af52941f205267ebdecbd
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839704"
---
# <a name="create-a-complete-virtual-machine-scale-set-with-powershell"></a>Een volledige virtuele-machineschaalset maken met PowerShell
Met dit script maakt u een virtuele-machineschaalset waarop Windows Server 2016 wordt uitgevoerd. Afzonderlijke resources worden geconfigureerd en gemaakt; ze maken geen gebruik van de [ingebouwde opties voor het maken van resources die hier beschikbaar zijn in New-AzureRmVmss](powershell-sample-create-simple-scale-set.md). Nadat het script is uitgevoerd, hebt u via RDP toegang tot de virtuele machine.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/complete-scale-set/complete-scale-set.ps1 "Create a complete virtual machine scale set")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep, de schaalset en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName
```

## <a name="script-explanation"></a>Uitleg van het script
In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u een subnetconfiguratie. Deze configuratie wordt gebruikt bij het maken van het virtueel netwerk. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel netwerk. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Hiermee maakt u een front-endconfiguratie voor een IP-adres voor een load balancer. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Hiermee maakt u een back-endconfiguratie voor een adresgroep voor een load balancer. |
| [New-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Hiermee maakt u binnenkomende NAT-regelconfiguratie voor een load balancer. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Hiermee maakt u een load balancer. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Hiermee maakt u een testconfiguratie voor een load balancer. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Hiermee maakt u een regelconfiguratie voor een load balancer. |
| [Set-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/Set-AzureRmLoadBalancer) | Werk de load balancer bij met de opgegeven gegevens. |
| [New-AzureRmVmssIpConfig](/powershell/module/AzureRM.Compute/New-AzureRmVmssIpConfig) | Maak een IP-configuratie voor de VM-exemplaren van de schaalset. De VM-exemplaren zijn verbonden met de back-endpool van de load balancer, de NAT-pool en het virtueel-netwerksubnet. |
| [New-AzureRmVmssConfig](/powershell/module/AzureRM.Compute/New-AzureRmVmssConfig) | Hiermee wordt een schaalsetconfiguratie gemaakt. Deze configuratie bevat informatie zoals het aantal VM-exemplaren dat moet worden gemaakt, de VM-SKU (grootte) en de upgradebeleidsmodus. De configuratie wordt toegevoegd aan andere cmdlets en wordt gebruikt tijdens het maken van de schaalset. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile) | Definieer de installatiekopie die moet worden gebruikt voor de VM-exemplaren en voeg deze toe aan de schaalsetconfiguratie. |
| [Set-AzureRmVmssOsProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile) | Definieer de gebruikersnaam- en wachtwoordreferenties van de beheerder en het VM-naamgevingsvoorvoegsel. Voeg deze waarden toe aan de schaalsetconfiguratie. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/AzureRM.Compute/Add-AzureRmVmssNetworkInterfaceConfiguration) | Voeg de virtuele-netwerkinterface toe aan de VM-exemplaren op basis van de IP-configuratie. Voeg deze waarden toe aan de schaalsetconfiguratie. |
| [New-AzureRmVmss](/powershell/module/AzureRM.Compute/New-AzureRmVmss) | Maak de schaalaanpassingsset op basis van de informatie in de schaalsetconfiguratie. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. |

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

U kunt extra PowerShell-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor virtuele-machineschaalsets](../powershell-samples.md).
