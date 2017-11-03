---
title: Voorbeelden van Azure CLI | Microsoft Docs
description: Voorbeelden van Azure CLI
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 7977460f61bfdabd399e45e86d9bbf2e5083992b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cli-samples-for-networking"></a>Azure CLI-voorbeelden voor netwerken

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI.

| | |
|-|-|
|**Connectiviteit tussen Azure-resources**||
| [Een virtueel netwerk voor toepassingen met meerdere lagen maken](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Het verkeer naar de front-end-subnet is beperkt tot HTTP- en SSH, terwijl het verkeer naar de back-end-subnet wordt beperkt tot MySQL, poort 3306. |
| [Peer twee virtuele netwerken](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Maakt en verbinding maakt twee virtuele netwerken in dezelfde regio. |
| [-Routeverkeer via een virtueel netwerkapparaat](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Maakt een virtueel netwerk met de front-end en back-end-subnetten en een virtuele machine kan verkeer leiden tussen de twee subnetten. |
| [Filteren van binnenkomende en uitgaande netwerkverkeer voor VM](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hiermee maakt een virtueel netwerk met front-end en back-end-subnetten. Binnenkomend netwerkverkeer naar de front-end-subnet is beperkt tot HTTP, HTTPS en SSH. Uitgaand verkeer naar Internet van de back-end-subnet is niet toegestaan. |
|**Load balancing en verkeer richting**||
| [Load balance verkeer naar VM's voor hoge beschikbaarheid](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Maakt verschillende virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Taakverdeling maken voor meerdere websites op virtuele machines](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hiermee maakt twee virtuele machines met meerdere IP-configuraties, lid is van een Azure Beschikbaarheidsset, toegankelijk via een Azure Load Balancer. |
| [Directe verkeer over meerdere regio's voor hoge beschikbaarheid](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Hiermee maakt twee app-serviceabonnementen, twee web-apps, een traffic manager-profiel en twee traffic manager-eindpunten. |
| | |
