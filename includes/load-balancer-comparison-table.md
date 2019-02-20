---
title: bestand opnemen
description: bestand opnemen
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/08/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 1d3ce900f7354b31e999c12b8e1eb0e23d391fcb
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56078523"
---
| | Standaard SKU | Basis-SKU |
| --- | --- | --- |
| Grootte van back-end-pool | Ondersteunt maximaal 1000 instanties. | Ondersteunt maximaal 100 instanties. |
| Eindpunten voor de back-end-pool | Elke virtuele machine in een virtueel netwerk, inclusief combinaties van virtuele machines, beschikbaarheidssets, virtuele-machineschaalsets. | Virtuele machines in een beschikbaarheidsset of virtuele-machineschaalset. |
| [Statuscontroles](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Gedrag statustest inactief](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP-verbindingen blijven actief als de instantietests inactief zijn __en__ als alle tests inactief zijn. | TCP-verbindingen blijven actief als tests inactief zijn voor de instantie. Alle TCP-verbindingen worden beëindigd als alle tests inactief zijn. |
| Beschikbaarheidszones | In de standaard SKU, zone-redundante en zonegebonden front-ends voor inkomend en uitgaand verkeer, koppelingen van uitgaande stromen overleven zonefouten, taakverdeling tussen zones. | Niet beschikbaar. |
| Diagnostiek | Azure Monitor, multi-dimensionale metrische gegevens, waaronder bytes en pakkettellers, status van gezondheidstest, verbindingspogingen (TCP SYN), status van de uitgaande verbinding (geslaagde en mislukte SNAT-stromen), metingen van actief gegevensvlak | Alleen Azure Log Analytics voor openbare Load Balancer, SNAT-uitputtingswaarschuwing, telling van status van back-end-pool. |
| HA-poorten | Interne load balancer | Niet beschikbaar. |
| Standaardbeveiliging | Openbare IP-adressen, openbare Load Balancer-eindpunten en interne Load Balancer-eindpunten zijn niet beschikbaar voor inkomende stromen, tenzij ze in een netwerkbeveiligingsgroep op de whitelist zijn geplaatst. | Standaard open, netwerkbeveiligingsgroep optioneel. |
| [Uitgaande verbindingen](../articles/load-balancer/load-balancer-outbound-connections.md) | U kunt uitgaande NAT op basis van pools expliciet definiëren met [uitgaande regels](../articles/load-balancer/load-balancer-outbound-rules-overview.md). U kunt meerdere front-ends gebruiken met afmeldingsmogelijkheid per taakverdelingsregel. Er _moet_ expliciet een uitgaand scenario worden gemaakt voor de virtuele machine, beschikbaarheidsset of virtuele-machineschaalset om gebruik te kunnen maken van uitgaande connectiviteit.  Service-eindpunten voor virtuele netwerken kunnen zonder uitgaande connectiviteit te definiëren, worden bereikt, en tellen niet mee voor de verwerkte gegevens.  Alle openbare IP-adressen, inclusief Azure PaaS-services die niet beschikbaar zijn als VNet-service-eindpunten, moeten worden bereikt via uitgaande connectiviteit en tellen mee als verwerkte gegevens. Als er maar één interne Load Balancer een virtuele machine, beschikbaarheidsset of virtuele-machineschaalset ondersteunt, zijn er geen uitgaande verbindingen via standaard-SNAT beschikbaar; gebruik dan [uitgaande regels](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Programmering van uitgaande SNAT is specifiek voor het transportprotocol op basis van het protocol van de regel voor de inkomende taakverdeling. | Eén front-end, willekeurig geselecteerd als er meerdere front-ends aanwezig zijn.  Als er maar één interne Load Balancer een virtuele machine, beschikbaarheidsset of virtuele-machineschaalset ondersteunt, wordt standaard-SNAT gebruikt. |
| [Regels voor uitgaand verkeer](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Verklarende uitgaande NAT-configuratie, met openbare IP-adressen, openbare IP-voorvoegsels of beide, configureerbare time-out (van 4-120 minuten) voor inactiviteit van uitgaand verkeer, aangepaste toewijzing van SNAT-poort | Niet beschikbaar. |
|  [TCP opnieuw instellen bij inactiviteit](../articles/load-balancer/load-balancer-tcp-reset.md) | Schakel in dat TCP opnieuw wordt ingesteld (TCP RST) voor welke regel dan ook als er sprake is van inactiviteit | Niet beschikbaar |
| [Meerdere frontends](../articles/load-balancer/load-balancer-multivip-overview.md) | Inkomend en [uitgaand](../articles/load-balancer/load-balancer-outbound-connections.md) | Alleen inkomend |
| Beheerbewerkingen | De meeste bewerkingen < 30 seconden | Meestal 60-90 seconden. |
| SLA | 99,99% voor gegevenspad met twee gezonde virtuele machines. | Niet van toepassing. | 
| Prijzen | Kosten worden in rekening gebracht op basis van het aantal regels en verwerkte inkomende en uitgaande gegevens die zijn gekoppeld aan de resource.  | Geen kosten. |
|  |  |  |
