---
title: Azure PowerShell-voorbeeldscript - verkeer routeren via een virtueel netwerkapparaat | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - verkeer routeren via een virtueel netwerkapparaat met firewall.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 332d6ecc6becc9f1219c3b90cbbf35f8fa85faa9
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>Voorbeeldscript voor verkeer routeren via een virtueel netwerkapparaat

Met dit voorbeeldscript wordt een virtueel netwerk met front-end- en back-end-subnetten gemaakt. Er wordt ook een virtuele machine gemaakt waarvoor doorsturen via IP is ingeschakeld, voor het routeren van netwerkverkeer tussen de twee subnetten. Na het uitvoeren van het script kunt u netwerksoftware, bijvoorbeeld een firewalltoepassing, op de VM implementeren.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/powershell) of vanuit een lokale installatie van PowerShell. Als u lokale PowerShell gebruikt, vereist dit script de AzureRM PowerShell-module versie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te zien welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript


[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht uit om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```
## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de volgende tabel is een koppeling naar opdrachtspecifieke documentatie:

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Hiermee maakt u back-end- en DMZ-subnetten. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres voor toegang tot de VM via internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u een virtuele netwerkinterface en schakelt u doorsturen via IP in voor deze interface. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hiermee maakt u een netwerkbeveiligingsgroep (NSG). |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Hiermee maakt u NSG-regels waarmee inkomend verkeer via HTTP- en HTTPS-poorten naar de VM worden toegestaan. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Hiermee koppelt u de NSG's en routetabellen aan subnetten. |
| [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Hiermee maakt u een routetabel voor alle routes. |
| [New-AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Hiermee maakt u routes om verkeer tussen subnetten en internet via de VM te routeren. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine en koppelt u de NIC hieraan. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, samen met beheerdersreferenties. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Hiermee verwijdert u een resourcegroep en de bijhorende resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Meer PowerShell-voorbeeldscripts voor virtuele netwerken vindt u in [PowerShell-voorbeelden voor virtuele netwerken](../powershell-samples.md).