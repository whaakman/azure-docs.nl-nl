---
title: 'Netwerken van Azure Stack: De verschillen en overwegingen met betrekking tot'
description: Meer informatie over verschillen en overwegingen bij het werken met netwerken in Azure-Stack.
services: azure-stack
keywords: 
author: mattbriggs
manager: femila
ms.author: mabrigg
ms.date: 02/28/2018
ms.topic: article
ms.service: azure-stack
ms.openlocfilehash: 4c881a5f5e64ddc9fc67060208f3bef6ae0f5028
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="considerations-for-azure-stack-networking"></a>Overwegingen voor het Azure-Stack-netwerken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Netwerken in Azure-Stack biedt veel van de functies die u in Azure, met enkele verschillen die u begrijpen vinden moet voordat u implementeren gaat.


Dit artikel bevat een overzicht van de unieke aandachtspunten voor netwerken en de bijbehorende functies in Azure-Stack. Zie voor meer informatie over belangrijke verschillen tussen Azure-Stack en Azure, de [belangrijke overwegingen](azure-stack-considerations.md) onderwerp.


## <a name="cheat-sheet-networking-differences"></a>Blad cheats: netwerken verschillen

|Service | Functie | Azure (global) | Azure Stack |
| --- | --- | --- | --- |
| DNS | Multi-tenant DNS | Ondersteund| Nog niet ondersteund|
| |DNS AAAA-records|Ondersteund|Niet ondersteund|
| |DNS-zones per abonnement|100 (standaard)<br>Op aanvraag kunnen worden verhoogd.|100|
| |DNS-recordsets per zone|5000 (standaard)<br>Op aanvraag kunnen worden verhoogd.|5000|
||Naamservers voor zoneoverdracht|Azure bieden vier naamservers voor elke gebruiker (tenant)-zone die is gemaakt.|Azure Stack biedt twee naamservers voor elke gebruiker (tenant)-zone die is gemaakt.|
| Virtueel netwerk|Peering op virtueel netwerk|Twee virtuele netwerken in dezelfde regio verbinden via het Azure-backbone-netwerk.|Nog niet ondersteund|
| |IPv6-adressen|U kunt een IPv6-adres toewijzen als onderdeel van de [configuratie netwerkinterfaces](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions).|Alleen IPv4 wordt ondersteund.|
|VPN-gateways|Punt-naar-Site VPN-Gateway|Ondersteund|Nog niet ondersteund|
| |Vnet-to-Vnet Gateway|Ondersteund|Nog niet ondersteund|
| |VPN-Gateway-SKU 's|Ondersteuning voor Basic, GW1, GW2, GW3, standaard hoge prestaties, zeer hoge prestaties. |Ondersteuning voor Basic, Standard en hoog-presterende SKU's.|
|Load balancer|IPv6-openbare IP-adressen|Ondersteuning voor een IPv6-openbare IP-adres toewijzen aan een load balancer.|Alleen IPv4 wordt ondersteund.|
|Network Watcher|Netwerk-Watcher tenantnetwerk bewakingsmogelijkheden|Ondersteund|Nog niet ondersteund|
|CDN|Content Delivery Network-profielen|Ondersteund|Nog niet ondersteund|
|Toepassingsgateway|Taakverdeling van laag 7|Ondersteund|Nog niet ondersteund|
|Traffic Manager|Route voor binnenkomend verkeer voor optimale toepassingsprestaties en betrouwbaarheid.|Ondersteund|Nog niet ondersteund|
|ExpressRoute|Een snelle, persoonlijke verbinding instellen met Microsoft cloud-services van uw lokale infrastructuur of plaatsing faciliteit.|Ondersteund|Ondersteuning voor het Azure-Stack verbinden met een Express Route-circuit.|

## <a name="next-steps"></a>Volgende stappen

[DNS in Azure Stack](azure-stack-dns.md)
