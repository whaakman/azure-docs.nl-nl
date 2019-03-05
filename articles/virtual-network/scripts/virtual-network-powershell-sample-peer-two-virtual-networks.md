---
title: Azure PowerShell-voorbeeldscript - twee virtuele netwerken peeren | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - twee virtuele netwerken peeren
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
ms.openlocfilehash: 98081e0404ceca162941ad17ad6fcd6839663dac
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650305"
---
# <a name="peer-two-virtual-networks-script-sample"></a>Voorbeeldscript voor peering van twee virtuele netwerken

Met dit voorbeeldscript worden twee virtuele netwerken in dezelfde regio gemaakt en verbonden via het Azure-netwerk. Nadat het script is uitgevoerd, maakt u een peering tussen twee virtuele netwerken.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/powershell) of vanuit een lokale installatie van PowerShell. Als u PowerShell lokaal gebruikt, vereist dit script de versie 5.4.1 of hoger van de Az PowerShell-module. Voer `Get-Module -ListAvailable Az` uit om na te gaan welke versie er is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Voer de volgende opdracht uit om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtuele machine en alle gerelateerde resources. Elke opdracht in de volgende tabel is een koppeling naar specifieke documentatie over de opdracht:

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)| Hiermee maakt u een virtueel Azure-netwerk en -subnet. |
| [Add-AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering) | Hiermee maakt u een peering tussen twee virtuele netwerken.  |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Meer PowerShell-voorbeeldscripts voor virtuele netwerken vindt u in [PowerShell-voorbeelden voor virtuele netwerken](../powershell-samples.md).