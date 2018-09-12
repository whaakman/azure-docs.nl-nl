---
title: Inleiding tot de volgende hop in Azure Network Watcher | Microsoft Docs
description: In dit artikel biedt een overzicht van de netwerk-Watcher volgende hop-mogelijkheid.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 28eacdce922e26d391cf34f78cb03ead9c6887a1
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391263"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Gebruik de volgende hop voor het vaststellen van problemen met de virtuele machine routering

Verkeer van een virtuele machine (VM) wordt verzonden naar een bestemming op basis van de effectieve routes die zijn gekoppeld aan een netwerkinterface (NIC). Volgende hop worden de volgende hoptype en het IP-adres van een pakket opgehaald uit een specifieke virtuele machine en NIC De volgende hop weet, kunt u bepalen als verkeer wordt omgeleid naar de bestemming en of het verkeer er nergens worden verzonden. Een onjuiste configuratie van routes, waarbij verkeer wordt omgeleid naar een on-premises locatie of een virtueel apparaat, kan leiden tot problemen met de netwerkverbinding. Volgende hop retourneert ook de routetabel die zijn gekoppeld aan de volgende hop. Als de route is gedefinieerd als een gebruiker gedefinieerde route, wordt die route geretourneerd. Volgende hop anders retourneert **Systeemroute**.

![overzicht van volgende hop](./media/network-watcher-next-hop-overview/figure1.png)

De volgende hops dat kunnen worden geretourneerd door de volgende hop-mogelijkheid zijn als volgt:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* VirtualNetworkServiceEndpoint 
* MicrosoftEdge
* Geen

Zie voor meer informatie over elk volgend hoptype [routeringoverzicht](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van volgende hop voor het vaststellen van problemen met VM-netwerk de routering, [vaststellen VM-netwerk op problemen met de routering](diagnose-vm-network-routing-problem.md).
