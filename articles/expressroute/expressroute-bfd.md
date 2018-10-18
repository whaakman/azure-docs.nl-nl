---
title: BFD configureren via ExpressRoute | Microsoft Docs
description: Dit document bevat instructies over het configureren van BFD via privé-peering van een ExpressRoute-circuit.
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: ''
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 8/17/2018
ms.author: rambala
ms.openlocfilehash: 97ac49ff5cc50621fd902f77c2543a428c7e8bae
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49384749"
---
# <a name="configure-bfd-over-expressroute"></a>BFD via ExpressRoute configureren

ExpressRoute ondersteunt in twee richtingen doorsturen van detectie (BFD) via persoonlijke peering. Door in te schakelen BFD via ExpressRoute, kunt u de foutdetectie koppeling tussen Microsoft Enterprise edge (MSEE)-apparaten en de routers waarop u het ExpressRoute-circuit (PE) beëindigen versnellen. (Als u met beheerde laag-3-verbinding-service), kunt u ExpressRoute beëindigen via Routering klant-Edge-apparaten of routing Partner-Edge-apparaten. Dit document helpt u bij de noodzaak van BFD en BFD inschakelen via ExpressRoute.

## <a name="need-for-bfd"></a>Noodzaak van BFD

Het volgende diagram toont het voordeel van het inschakelen van BFD via ExpressRoute-circuit: [ ![1]][1]

U kunt ExpressRoute-circuit door een Layer 2-verbindingen of beheerde laag-3-verbindingen. In beide gevallen als er een of meer Layer-2-apparaten in het pad van ExpressRoute-verbinding, ligt de verantwoordelijkheid van de koppeling fouten opsporen in het pad met de bovenliggende BGP.

Op de apparaten MSEE worden keepalive-BGP en wacht-tijd doorgaans geconfigureerd als 60 en 180 seconden respectievelijk. Daarom na een fout in het tot duren zou drie minuten voor het detecteren van een mislukte koppeling verkeer naar en alternatieve verbinding.

U kunt de BGP-timers beheren door het configureren van lagere BGP-keepalive en wacht-tijd op het apparaat van klant-edge-peering. Als de BGP-timers niet tussen de twee peering apparaten overeen komen, gebruikt de BGP-sessie tussen de peers. de laagste timerwaarde. De BGP-keepalive kan slechts drie seconden, en de wachtstand-tijd in volgorde van tientallen seconden worden ingesteld. Echter instellen BGP timers minder agressief voorkeur, omdat het protocol intensieve proces.

In dit scenario kunt BFD. BFD biedt weinig overhead koppeling foutdetectie in een tijdsinterval subsecond. 


## <a name="enabling-bfd"></a>BFD inschakelen

BFD is onder alle de zojuist gemaakte ExpressRoute persoonlijke peering interfaces op de msee's standaard geconfigureerd. Om in te schakelen BFD, moet u daarom alleen BFD configureren op uw PEs. Proces in twee stappen voor het configureren van BFD is: u moet de BFD configureren op de interface en vervolgens koppelen aan de BGP-sessie.

Hieronder ziet u een voorbeeldconfiguratie PE (met behulp van Cisco IOS XE). 

    interface TenGigabitEthernet2/0/0.150
      description private peering to Azure
      encapsulation dot1Q 15 second-dot1q 150
      ip vrf forwarding 15
      ip address 192.168.15.17 255.255.255.252
      bfd interval 300 min_rx 300 multiplier 3


    router bgp 65020
      address-family ipv4 vrf 15
        network 10.1.15.0 mask 255.255.255.128
        neighbor 192.168.15.18 remote-as 12076
        neighbor 192.168.15.18 fall-over bfd
        neighbor 192.168.15.18 activate
        neighbor 192.168.15.18 soft-reconfiguration inbound
      exit-address-family

>[!NOTE]
>Om in te schakelen BFD onder een bestaande persoonlijke peering; u moet de peering instellen. Zie [opnieuw instellen van ExpressRoute-peerings][ResetPeering]
>

## <a name="bfd-timer-negotiation"></a>BFD Timer-onderhandeling

Tussen BFD peers bepalen de tragere van de twee computers de verzending van fouten. Msee's BFD verzenden/ontvangen intervallen zijn ingesteld op 300 milliseconden. Door het configureren van hogere waarden, kunt u afdwingen dat deze intervallen moet meer zijn; maar, geen korter.

>[!NOTE]
>Als u geografisch redundante ExpressRoute-circuits voor persoonlijke peering hebt geconfigureerd of Site-naar-Site IPSec VPN-connectiviteit heeft als de back-up voor ExpressRoute-privépeering; BFD inschakelen via de privé-peering wilt helpen sneller een ExpressRoute-verbindingsfout na failover. 
>

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende koppelingen voor meer informatie en hulp:

- [Een ExpressRoute-circuit maken en wijzigen][CreateCircuit]
- [Routering voor een ExpressRoute-circuit maken en wijzigen][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/BFD_Need.png "BFD versnelt tijd aftrek van koppeling"

<!--Link References-->
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ResetPeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering






