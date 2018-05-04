---
title: Inleiding tot de volgende hop in Azure-netwerk-Watcher | Microsoft Docs
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
ms.openlocfilehash: bbb782e700781dcfedbbd340c7d10db53767b035
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Gebruik de volgende hop voor het vaststellen van problemen met de virtuele machine routering

Verkeer van een virtuele machine (VM) wordt verzonden naar een bestemming op basis van de effectieve routes die zijn gekoppeld aan een netwerkinterface (NIC). Volgende hop krijgt het volgende hoptype en IP-adres van een pakket van een specifieke virtuele machine en NIC. Kent de volgende hop, kunt u bepalen of verkeer wordt omgeleid naar de bestemming, of of het verkeer wordt nergens wordt verzonden. Een onjuiste configuratie van routes, waarin verkeer wordt omgeleid naar een on-premises locatie of een virtueel apparaat, kan leiden tot problemen met de netwerkverbinding. Volgende hop retourneert ook de routetabel die zijn gekoppeld aan de volgende hop. Als de route is gedefinieerd als een gebruiker gedefinieerde route, wordt die route geretourneerd. Retourneert anders volgende hop **Systeemroute**.

![overzicht van volgende hop](./media/network-watcher-next-hop-overview/figure1.png)

De volgende hops dat kunnen worden geretourneerd door de volgende hop zijn als volgt:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* VnetPeering
* Geen

Zie voor meer informatie over elk volgend hoptype, [Routering-overzicht](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de volgende hop voor het vaststellen van problemen met VM-netwerk de routering, [onderzoeken VM-netwerk problemen met de routering](diagnose-vm-network-routing-problem.md).
