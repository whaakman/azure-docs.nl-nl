---
title: Azure PowerShell-voorbeeldscript - routeverkeer via een virtueel netwerkapparaat | Microsoft Docs
description: 'Azure PowerShell - voorbeeldscript: de Route-verkeer via een firewall netwerk virtueel apparaat.'
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 883d28dac72a66c2186d222f72b04d68e532cead
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

Dit voorbeeldscript wordt een virtueel netwerk gemaakt met front-end en back-end-subnetten. Een virtuele machine wordt gemaakt met doorsturen via IP is ingeschakeld voor het routeren van netwerkverkeer tussen de twee subnetten. U kunt na het uitvoeren van het script netwerksoftware, zoals een firewalltoepassing naar de virtuele machine implementeren.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, het virtuele netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Maakt een virtueel Azure-netwerk en de front-end-subnet. |
| [Nieuwe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Maakt back-end en DMZ subnetten. |
| [Nieuwe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres voor toegang tot de virtuele machine via het Internet. |
| [Nieuwe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Maakt een virtuele netwerkinterface en doorsturen via IP inschakelen voor deze. |
| [Nieuwe AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Maakt een netwerkbeveiligingsgroep (NSG). |
| [Nieuwe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | NSG-regels waarmee HTTP en HTTPS-poorten inkomend naar de virtuele machine maakt. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Koppelt de nsg's en routetabellen aan subnetten. |
| [Nieuwe AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Een routetabel voor alle routes maken. |
| [Nieuwe AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Routes naar verkeer leiden tussen subnetten en het Internet via de virtuele machine maakt. |
| [Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine en de NIC koppelt aan het. Deze opdracht geeft ook aan de installatiekopie van de virtuele machine te gebruiken en de beheerdersreferenties. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Hiermee verwijdert u een resourcegroep en alle resources die deze bevat. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Aanvullende voorbeelden voor netwerken PowerShell-script kunnen worden gevonden in de [overzicht van Azure-netwerken documentatie](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).