---
title: Voorbeelden van Azure PowerShell | Microsoft Docs
description: Voorbeelden van Azure PowerShell
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: georgewallace
ms.openlocfilehash: 0bca4fb6874bd265f0ae9faeb4219abeb4ffb6d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-networking"></a>Azure PowerShell-voorbeelden voor netwerken

De volgende tabel bevat koppelingen naar de scripts die zijn gebouwd met behulp van Azure PowerShell.

| | |
|-|-|
|**Connectiviteit tussen Azure-resources**||
| [Een virtueel netwerk voor toepassingen met meerdere lagen maken](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Het verkeer naar de front-end-subnet is beperkt tot HTTP, terwijl het verkeer naar de back-end-subnet wordt beperkt tot de SQL-poort 1433. |
| [Peer twee virtuele netwerken](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Maakt en verbinding maakt twee virtuele netwerken in dezelfde regio. |
| [-Routeverkeer via een virtueel netwerkapparaat](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Maakt een virtueel netwerk met de front-end en back-end-subnetten en een virtuele machine kan verkeer leiden tussen de twee subnetten. |
| [Filteren van binnenkomende en uitgaande netwerkverkeer voor VM](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Binnenkomend netwerkverkeer naar de front-end-subnet is beperkt tot HTTP en HTTPS... Uitgaand verkeer naar Internet van de back-end-subnet is niet toegestaan. |
|**Load balancing en verkeer richting**||
| [Load balance verkeer naar VM's voor hoge beschikbaarheid](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Maakt verschillende virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Taakverdeling maken voor meerdere websites op virtuele machines](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hiermee maakt twee virtuele machines met meerdere IP-configuraties, lid is van een Azure Beschikbaarheidsset, toegankelijk via een Azure Load Balancer. |
| [Directe verkeer over meerdere regio's voor hoge beschikbaarheid](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Hiermee maakt twee app-serviceabonnementen, twee web-apps, een traffic manager-profiel en twee traffic manager-eindpunten. |
| | |
