---
title: bestand opnemen
description: bestand opnemen
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 8/8/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 6514bcdbe79db4a22b2a4bf13e5808bbb9abbb06
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40026466"
---
| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| Grootte van de back-end | Biedt ondersteuning voor maximaal 1000 instanties | Biedt ondersteuning voor maximaal 100 exemplaren |
| Back-end-pool eindpunten | elke virtuele machine in één virtueel netwerk, met inbegrip van de combinatie van virtuele machines, beschikbaarheidssets, virtuele-machineschaalsets. | Virtuele machines in een beschikbaarheid van één set of virtuele machine de schaalset is ingesteld. |
| [Statuscontroles](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Statustest omlaag gedrag](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP-verbindingen op exemplaar test omlaag overleven __en__ op alle tests omlaag. | TCP-verbindingen blijven op exemplaar test omlaag. Alle TCP-verbindingen wordt beëindigd op alle tests omlaag. |
| Beschikbaarheidszones | In de basis-SKU, Zone-redundante en zonegebonden front-ends voor inkomend en uitgaand, uitgaande stromen toewijzingen zone storingen, taakverdeling tussen zones. | N.v.t.|
| Diagnostiek | Azure Monitor, multi-dimensionale metrische gegevens, waaronder bytes en pakket-tellers, health probe status, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (SNAT geslaagde en mislukte stromen), actieve gegevens vlak metingen | Azure Log Analytics voor openbare Load Balancer alleen SNAT uitputting van waarschuwing, back-end-pool health tellen. |
| Maximaal beschikbare poorten | Interne load balancer | N.v.t. |
| Standaard beveiligen | Standaard gesloten voor openbare IP-adres en de Load Balancer-eindpunten en een netwerkbeveiligingsgroep moet worden gebruikt om expliciet whitelist voor verkeer stromen. | Standaard opent, worden de netwerkbeveiligingsgroep is optioneel. |
| [Uitgaande verbindingen](../articles/load-balancer/load-balancer-outbound-connections.md) | Meerdere front-ends met per regel opt-out voor taakverdeling. Een uitgaande scenario _moet_ expliciet worden gemaakt voor de virtuele machine om te kunnen gebruiken uitgaande connectiviteit.  Service-eindpunten voor virtueel netwerk kan worden bereikt zonder de uitgaande connectiviteit en tellen niet mee met gegevens die worden verwerkt.  Alle openbare IP-adressen, met inbegrip van Azure PaaS-services die niet beschikbaar als VNet-Service-eindpunten, moeten worden bereikt via de uitgaande connectiviteit en het aantal voor gegevens die worden verwerkt. Bij een interne Load Balancer een virtuele machine levert is, zijn uitgaande verbindingen via standaard SNAT niet beschikbaar. Uitgaande SNAT programmeren is transportprotocol bepaald op basis van het protocol van de inkomende regel voor taakverdeling. | Één front-end willekeurig worden geselecteerd als er meerdere front-ends aanwezig zijn.  Wanneer u alleen een interne Load Balancer is een virtuele machine fungeert, wordt standaard SNAT wordt gebruikt. |
| [Meerdere frontends](../articles/load-balancer/load-balancer-multivip-overview.md) | Inkomende en [uitgaande](../articles/load-balancer/load-balancer-outbound-connections.md) | Alleen binnenkomende gegevens |
| Beheerbewerkingen | De meeste bewerkingen < 30 seconden | 60-90 seconden standaard. |
| SLA | 99,99% voor het gegevenspad met twee in orde virtuele machines. | Impliciete in VM SLA. | 
| Prijzen | In rekening gebracht op basis van het aantal regels, gegevens verwerkte binnenkomende of uitgaande die zijn gekoppeld aan de resource.  | Er zijn geen kosten in rekening gebracht |
|  |  |  |
