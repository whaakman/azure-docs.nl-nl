---
title: Inleiding tot IP-stroom controleren in Azure Network Watcher | Microsoft Docs
description: Deze pagina bevat een overzicht van de Network Watcher-IP-stroom controleren mogelijkheid
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: jdial
ms.openlocfilehash: 88cb7e2cd04d13ade5c581a1ff2dc09669d89ab2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838001"
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Inleiding tot IP-stroom controleren in Azure Network Watcher

IP-stroom wordt gecontroleerd of een pakket wordt toegestaan of verboden vanaf of naar een virtuele machine. De informatie bestaat uit de richting, protocol, lokale IP, externe IP-, lokale poort en externe poort. Als het pakket is geweigerd door een beveiligingsgroep, wordt de naam van de regel die het pakket geweigerd geretourneerd. Terwijl de IP-bron- of doelserver kan worden gekozen, IP-stroom controleren helpt beheerders snel vaststellen van connectiviteitsproblemen met van of naar het internet en van of naar de on-premises omgeving.

IP-stroom controleren ziet op de regels voor alle Netwerkbeveiligingsgroepen (nsg's) toegepast op de netwerkinterface, zoals een subnet of een virtuele machine NIC. Netwerkverkeer wordt vervolgens geverifieerd op basis van de geconfigureerde instellingen naar of van de netwerkinterface. IP-stroom controleren is nuttig bij het bevestigen van als binnenkomende of uitgaande verkeer naar of van een virtuele machine wordt geblokkeerd door een regel in een Netwerkbeveiligingsgroep.

Een exemplaar van Network Watcher moet worden gemaakt in alle regio's die u van plan bent om uit te voeren van IP-stroom controleren. Network Watcher is een regionale service en kan alleen worden uitgevoerd op resources in dezelfde regio. Het exemplaar dat wordt gebruikt, heeft geen invloed op de resultaten van IP-stroom controleren, zoals een route die is gekoppeld aan de NIC of subnet wordt nog steeds worden geretourneerd.

![1][1]

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie als een pakket wordt toegestaan of geweigerd voor een specifieke virtuele machine via de portal. [Controleren of verkeer is toegestaan op een virtuele machine met het IP-stroom controleren met behulp van de portal](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png

