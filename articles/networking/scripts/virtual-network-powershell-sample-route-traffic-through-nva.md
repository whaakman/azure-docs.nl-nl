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
ms.openlocfilehash: 7470ef655e07e2eff2e80bfc104a1b42fa950a0e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66156799"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Verkeer routeren via een virtueel netwerkapparaat

Met dit voorbeeldscript wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Er wordt ook een virtuele machine gemaakt waarvoor doorsturen via IP is ingeschakeld, voor het routeren van netwerkverkeer tussen de twee subnetten. Na het uitvoeren van het script kunt u netwerksoftware, bijvoorbeeld een firewalltoepassing, op de VM implementeren.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) en voer vervolgens `Connect-AzAccount` uit om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Hiermee maakt u back-end- en DMZ-subnetten. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via Internet. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Hiermee maakt u een virtuele netwerkinterface en schakelt u doorsturen via IP in voor deze interface. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Hiermee maakt u een netwerkbeveiligingsgroep (NSG). |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Hiermee maakt u NSG-regels waarmee inkomend verkeer via HTTP- en HTTPS-poorten naar de VM worden toegestaan. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)| Hiermee koppelt u de NSG's en routetabellen aan subnetten. |
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)| Hiermee maakt u een routetabel voor alle routes. |
| [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)| Hiermee maakt u routes om verkeer te routeren tussen subnetten en Internet via de virtuele machine. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Hiermee maakt u een virtuele machine en koppelt u de NIC hieraan. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, evenals de beheerdersreferenties. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)  | Hiermee verwijdert u een resourcegroep en de bijhorende resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende PowerShell-voorbeeldscripts voor netwerken vindt u in de [Documentatie met een overzicht van Azure-netwerken](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
