---
title: Inleiding tot IP-stroom controleren in de Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de IP-netwerk-Watcher stroom mogelijkheid controleren
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b2ad45e76320c59d18dce7b39166679801b4170a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Inleiding tot IP-stroom controleren in de Azure-netwerk-Watcher

IP-stroom Controleer controles of als een pakket wordt toegestaan of geweigerd naar of van een virtuele machine op basis van 5-tuple informatie. Deze informatie bestaat uit de richting, protocol, lokaal IP-, externe IP-, lokale poort en externe poort. Als het pakket wordt geweigerd door een beveiligingsgroep, wordt de naam van de regel die het pakket geweigerd geretourneerd. Terwijl de IP-bron- of doelserver kan worden gekozen, is deze functie kan beheerders een Analyseer snel problemen met de netwerkverbinding van of met het internet en van of naar de on-premises omgeving.

IP-stroom controleren is bedoeld voor een netwerkinterface van een virtuele machine. Netwerkverkeer is geverifieerd op basis van de geconfigureerde instellingen naar of van de netwerkinterface. Deze functie is handig bij de bevestiging als een regel in een Netwerkbeveiligingsgroep toegangsroutes en uitgaande verkeer naar of van een virtuele machine wordt geblokkeerd.

Een exemplaar van netwerk-Watcher moet worden gemaakt in alle regio's die u van plan bent om uit te voeren van IP-stroom controleren. Netwerk-Watcher is een regionale service en kan alleen worden uitgevoerd op resources in dezelfde regio. Dit heeft geen invloed op de resultaten van IP-stroom controleert u de route die zijn gekoppeld aan de NIC nog steeds worden geretourneerd.

![1][1]

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie als een pakket wordt toegestaan of geweigerd voor een specifieke virtuele machine via de portal. [Controleer of verkeer is toegestaan op een virtuele machine met het IP-stromen controleren met behulp van de portal](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












