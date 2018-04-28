---
title: Azure PowerShell-voorbeeldscript - verkeer routeren via een virtueel netwerkapparaat | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - verkeer routeren via een virtueel netwerkapparaat met firewall.
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 3b3aabed8c74dcb7561d7a40d09af9e3cf17ad8d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

Met dit voorbeeldscript wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Er wordt ook een virtuele machine gemaakt waarvoor doorsturen via IP is ingeschakeld, voor het routeren van netwerkverkeer tussen de twee subnetten. Na het uitvoeren van het script kunt u netwerksoftware, bijvoorbeeld een firewalltoepassing, op de VM implementeren.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) en voer vervolgens `Connect-AzureRmAccount` uit om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u back-end- en DMZ-subnetten. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via het Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u een virtuele netwerkinterface en schakelt u doorsturen via IP in voor deze interface. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hiermee maakt u een netwerkbeveiligingsgroep (NSG). |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Hiermee maakt u NSG-regels waarmee inkomend verkeer via HTTP- en HTTPS-poorten naar de VM worden toegestaan. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Hiermee koppelt u de NSG's en routetabellen aan subnetten. |
| [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Hiermee maakt u een routetabel voor alle routes. |
| [New-AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Routes naar verkeer leiden tussen subnetten en het Internet via de virtuele machine maakt. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine en koppelt u de NIC hieraan. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, evenals de beheerdersreferenties. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Hiermee verwijdert u een resourcegroep en de bijhorende resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
