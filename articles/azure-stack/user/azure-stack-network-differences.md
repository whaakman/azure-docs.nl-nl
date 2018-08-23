---
title: Azure Stack verschillen en overwegingen met betrekking tot netwerken | Microsoft Docs
description: Meer informatie over de verschillen en overwegingen als u werkt met netwerken in Azure Stack.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.author: brenduns
ms.date: 08/02/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: scottnap
ms.openlocfilehash: 50fe3c0c7fda745047c71afb8eedf7fa8806c4ec
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054446"
---
# <a name="considerations-for-azure-stack-networking"></a>Overwegingen voor Azure Stack-netwerken

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack-netwerken heeft veel van de functies van Azure-netwerken. Er zijn echter enkele belangrijke verschillen die u begrijpen moet voordat u een Azure Stack-netwerk implementeert.

Dit artikel bevat een overzicht van de unieke overwegingen voor Azure Stack-netwerken en de bijbehorende functies. Zie voor meer informatie over belangrijke verschillen tussen Azure Stack en Azure, de [belangrijke overwegingen met betrekking tot](azure-stack-considerations.md) artikel.

## <a name="cheat-sheet-networking-differences"></a>Overzichtskaart: netwerken verschillen

| Service | Functie | Azure (wereldwijd) | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | Multitenant DNS | Ondersteund | Nog niet ondersteund |
|  | DNS AAAA-records | Ondersteund | Niet ondersteund |
|  | DNS-zones per abonnement | 100 (standaard)<br>Op aanvraag kunnen worden verhoogd. | 100 |
|  | DNS-recordsets per zone | 5000 (standaard)<br>Op aanvraag kunnen worden verhoogd. | 5000 |
|  | Naamservers voor zoneoverdracht | Azure biedt vier de naamservers voor elke gebruiker (tenant)-zone die wordt gemaakt. | Azure Stack biedt twee naamservers voor elke gebruiker (tenant)-zone die wordt gemaakt. |
| Virtual Network | Peering op virtueel netwerk | Verbinding maken met twee virtuele netwerken in dezelfde regio via het Azure-backbone-netwerk. | Nog niet ondersteund |
|  | IPv6-adressen | U kunt een IPv6-adres toewijzen als onderdeel van de [Netwerkinterfaceconfiguratie](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions). | Alleen IPv4 wordt ondersteund. |
|  | DDoS-beschermingsplan | Ondersteund | Nog niet ondersteund. |
|  | Virtual Machine Scale sets IP-configuraties | Ondersteund | Nog niet ondersteund. |
|  | Persoonlijke toegang tot Services (Subnet) | Ondersteund | Nog niet ondersteund. |
|  | Service-eindpunten | Ondersteund voor interne (niet-Internet) verbinding met Azure-Services. | Nog niet ondersteund. |
| Alleen IPv4 wordt ondersteund. | Service-eindpuntbeleid | Ondersteund | Nog niet ondersteund. |
|  | Service-Tunnels | Ondersteund | Nog niet ondersteund.  |
| Netwerkbeveiligingsgroepen | Uitgebreide beveiligingsregels | Ondersteund | Nog niet ondersteund. |
|  | Effectieve beveiligingsregels | Ondersteund | Nog niet ondersteund. |
|  | Toepassingsbeveiligingsgroepen | Ondersteund | Nog niet ondersteund. |
| Virtuele netwerkgateways | Punt-naar-Site VPN-Gateway | Ondersteund | Nog niet ondersteund. |
|  | Vnet-naar-Vnet-Gateway | Ondersteund | Nog niet ondersteund. |
|  | Gateway van het Type virtueel netwerk | Azure ondersteunt VPN<br> ExpressRoute <br> Hyper-netto | Azure Stack biedt ondersteuning voor VPN-type op dit moment. |
|  | VPN-Gateway-SKU 's | Ondersteuning voor Basic, GW1, GW2, GW3, standaard krachtige, zeer hoge prestaties. | Ondersteuning voor Basic, Standard en High Performance SKU's. |
|  | VPN-Type | Azure ondersteunt zowel op basis van beleid en op basis van Route. | Azure Stack biedt ondersteuning voor Route op basis van alleen. |
|  | BGP-instellingen | Azure biedt ondersteuning voor configuratie van-adres van BGP-Peering en gewicht van Peer. | BGP-Peering-adres en het gewicht van Peer worden automatisch geconfigureerd in Azure Stack. Er is geen manier voor de gebruiker deze instellingen configureren met hun eigen waarden. |
|  | Standaard-Gateway-Site | Azure ondersteunt de configuratie van een standaard-site voor geforceerde tunneling. | Nog niet ondersteund. |
|  | Gateway vergroten of verkleinen | Azure biedt ondersteuning voor de gateway vergroten of verkleinen na de implementatie. | Wijzigen van het formaat niet ondersteund. |
|  | Actief/actief-configuratie | Ondersteund | Nog niet ondersteund. |
|  | IKE-/ IPSec-beleid | Azure ondersteunt aangepaste IPSec-beleid-configuraties. | Nog niet ondersteund. |
|  | UsePolicyBasedTrafficSelectors | Azure ondersteunt het gebruik van op beleid gebaseerde verkeerkiezers met op route gebaseerde gatewayverbindingen. | Nog niet ondersteund. |
| Load balancer | SKU | Basic en Standard Load Balancers worden ondersteund | De Basic Load Balancer wordt ondersteund.  De SKU-eigenschap wordt niet ondersteund. |
|  | Zones | Beschikbaarheidszones worden ondersteund. | Nog niet ondersteund |
|  | Ondersteuning voor inkomende NAT-regels voor Service-eindpunten | Azure ondersteunt het opgeven van Service-eindpunten voor binnenkomende NAT-regels. | Azure Stack ondersteunt nog geen Service-eindpunten, zodat deze kunnen niet worden opgegeven. |
|  | Protocol | Azure biedt ondersteuning voor GRE of ESP op te geven. | Protocolklasse wordt niet ondersteund in Azure Stack. |
| Openbaar IP-adres | Openbare IP-Adresversie | Azure ondersteunt zowel IPv4 als IPv6 | Alleen IPv4 wordt ondersteund. |
| Netwerkinterface | De tabel effectieve Route ophalen | Ondersteund | Nog niet ondersteund. |
|  | Effectieve ACL's ophalen | Ondersteund | Nog niet ondersteund. |
|  | Versneld netwerken inschakelen | Ondersteund | Nog niet ondersteund. |
|  | Doorsturen via IP | Standaard uitgeschakeld.  Kan worden ingeschakeld. | Bij het omschakelen van deze instelling wordt niet ondersteund.  Op standaard. |
|  | Meerdere IP-configuraties per interface | Ondersteund | Nog niet ondersteund. |
|  | Toepassingsbeveiligingsgroepen | Ondersteund | Nog niet ondersteund. |
|  | Interne DNS-naamlabel | Ondersteund | Nog niet ondersteund. |
|  | Privé-IP-adres versie | Zowel IPv4 als IPv6 worden ondersteund. | Alleen IPv4 wordt ondersteund. |
|  | Primaire IP-configuratie | Ondersteund. Hiermee geeft u de primaire IP-configuratie op de interface. | Nog niet ondersteund. |
| Network Watcher | Network Watcher tenantnetwerk bewakingsmogelijkheden | Ondersteund | Nog niet ondersteund. |
| CDN | Content Delivery Network-profielen | Ondersteund | Nog niet ondersteund. |
| Toepassingsgateway | Taakverdeling voor laag 7 | Ondersteund | Nog niet ondersteund. |
| Traffic Manager | Inkomend verkeer routeren voor optimale toepassingsprestaties en betrouwbaarheid. | Ondersteund | Nog niet ondersteund. |
| ExpressRoute | Instellen van een snelle particuliere verbinding met Microsoft cloud-services van uw on-premises infrastructuur of CO-locatiefaciliteit. | Ondersteund | Ondersteuning voor Azure Stack verbinden met een Express Route-circuit. |

## <a name="next-steps"></a>Volgende stappen

[DNS in Azure Stack](azure-stack-dns.md)