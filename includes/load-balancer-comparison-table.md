---
title: bestand opnemen
description: bestand opnemen
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 9/26/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 74835734a1dd37e07efa96fd3aa76fc0d349e47f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47454363"
---
| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| Grootte van de back-end | Biedt ondersteuning voor maximaal 1000 instanties | Biedt ondersteuning voor maximaal 100 exemplaren |
| Back-end-pool eindpunten | elke virtuele machine in één virtueel netwerk, met inbegrip van de combinatie van virtuele machines, beschikbaarheidssets, virtuele-machineschaalsets. | Virtuele machines in een beschikbaarheid van één set of virtuele machine de schaalset is ingesteld. |
| [Statuscontroles](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Statustest omlaag gedrag](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP-verbindingen op exemplaar test omlaag overleven __en__ op alle tests omlaag. | TCP-verbindingen blijven op exemplaar test omlaag. Alle TCP-verbindingen wordt beëindigd op alle tests omlaag. |
| Beschikbaarheidszones | In de basis-SKU, Zone-redundante en zonegebonden front-ends voor inkomend en uitgaand, uitgaande stromen toewijzingen zone storingen, taakverdeling tussen zones. | Niet beschikbaar |
| Diagnostiek | Azure Monitor, multi-dimensionale metrische gegevens, waaronder bytes en pakket-tellers, health probe status, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (SNAT geslaagde en mislukte stromen), actieve gegevens vlak metingen | Azure Log Analytics voor openbare Load Balancer alleen SNAT uitputting van waarschuwing, back-end-pool health tellen. |
| Maximaal beschikbare poorten | Interne load balancer | Niet beschikbaar |
| Standaard beveiligen | Standaard inkomende gesloten voor openbare IP-adres en de Load Balancer-eindpunten en een netwerkbeveiligingsgroep moet worden gebruikt om expliciet whitelist voor verkeer stromen. | Standaard opent, worden de netwerkbeveiligingsgroep is optioneel. |
| [Uitgaande verbindingen](../articles/load-balancer/load-balancer-outbound-connections.md) | U kunt expliciet definiëren de uitgaande NAT op basis van een groep met [regels voor uitgaand verkeer](../articles/load-balancer/load-balancer-outbound-rules-overview.md). U kunt meerdere front-ends met per load balancing regel opt-out. Een uitgaande scenario _moet_ expliciet worden gemaakt voor de virtuele machine om te kunnen gebruiken uitgaande connectiviteit.  Service-eindpunten voor virtueel netwerk kan worden bereikt zonder de uitgaande connectiviteit en tellen niet mee met gegevens die worden verwerkt.  Alle openbare IP-adressen, met inbegrip van Azure PaaS-services die niet beschikbaar als VNet-Service-eindpunten, moeten worden bereikt via de uitgaande connectiviteit en het aantal voor gegevens die worden verwerkt. Wanneer een interne Load Balancer een virtuele machine levert is, zijn uitgaande verbindingen via standaard SNAT niet beschikbaar. Gebruik [regels voor uitgaand verkeer](../articles/load-balancer/load-balancer-outbound-rules-overview.md) in plaats daarvan. Uitgaande SNAT programmeren is transportprotocol bepaald op basis van het protocol van de inkomende regel voor taakverdeling. | Één front-end willekeurig worden geselecteerd als er meerdere front-ends aanwezig zijn.  Wanneer u alleen een interne Load Balancer is een virtuele machine fungeert, wordt standaard SNAT wordt gebruikt. |
| [Regels voor uitgaand verkeer](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Declaratieve uitgaande NAT-configuratie, zoals welke openbare IP-adressen of openbare IP-prefix(en), configureerbare uitgaande time-out voor inactiviteit, aangepaste SNAT poort toewijzing | Niet beschikbaar |
|  [TCP-opnieuw instellen op niet-actieve](../articles/load-balancer/load-balancer-tcp-reset.md) | TCP (TCP RST) op voor time-out bij inactief opnieuw ingesteld op elke regel inschakelen | Niet beschikbaar |
| [Meerdere frontends](../articles/load-balancer/load-balancer-multivip-overview.md) | Inkomende en [uitgaande](../articles/load-balancer/load-balancer-outbound-connections.md) | Alleen binnenkomende gegevens |
| Beheerbewerkingen | De meeste bewerkingen < 30 seconden | 60-90 seconden standaard. |
| SLA | 99,99% voor het gegevenspad met twee in orde virtuele machines. | [Impliciete in VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). | 
| Prijzen | In rekening gebracht op basis van het aantal regels, gegevens verwerkte binnenkomende of uitgaande die zijn gekoppeld aan de resource.  | Er zijn geen kosten in rekening gebracht |
|  |  |  |
