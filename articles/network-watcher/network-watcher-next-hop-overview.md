---
title: Inleiding tot de volgende hop in Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de netwerk-Watcher volgende hop-functionaliteit
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bb2ca0486b3b3d27a77b70927cb3cbfbeac12c7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Inleiding tot de volgende hop in Azure-netwerk-Watcher

Verkeer van een virtuele machine wordt verzonden naar een bestemming op basis van de effectieve routes die zijn gekoppeld aan een NIC. Volgende hop opgehaald uit het volgende hoptype en IP-adres van een pakket vanuit een specifieke virtuele machine en de NIC. Dit helpt te bepalen of het pakket wordt omgeleid naar de bestemming of is het verkeer wordt zwarte gaan. Een onjuiste configuratie van routes door de gebruiker, waarbij een verkeer wordt omgeleid naar een on-premises locatie of een virtueel apparaat, kan leiden tot problemen met de netwerkverbinding. Volgende hop retourneert ook de routetabel die zijn gekoppeld aan de volgende hop. Tijdens het opvragen van een volgende hop als de route is gedefinieerd als een gebruiker gedefinieerde route worden die route geretourneerd. Anders retourneert de volgende hop 'Systeemroute'.

![overzicht van volgende hop][1]

Hier volgt een lijst van de volgende hoptypen die kunnen worden geretourneerd bij het opvragen van de volgende hop.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Geen

### <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van volgende hop vinden van problemen met de netwerkverbinding in via [controleren van de volgende hop op een virtuele machine](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













