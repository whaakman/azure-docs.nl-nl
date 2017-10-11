---
title: 'Planning en ontwerp voor cross-premises verbindingen: Azure VPN-Gateway | Microsoft Docs'
description: Meer informatie over VPN-Gateway planning en ontwerp voor cross-premises en hybride VNet-naar-VNet-verbindingen
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 0ebc3ef4a64432e993dd6ed69766bb64544fe433
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a>Planning en ontwerp voor VPN Gateway

Plannen en ontwerpen van uw cross-premises en VNet-naar-VNet-configuraties kunnen zijn eenvoudige of complexe, afhankelijk van de behoeften van uw netwerk. Dit artikel begeleidt u bij basic plannings- en ontwerpoverwegingen.

## <a name="planning"></a>Plannen

### <a name="compare"></a>Opties voor cross-premises-connectiviteit

Als u uw lokale sites veilige verbinding met een virtueel netwerk wilt, hebt u drie verschillende manieren om dit te doen: Site-naar-Site, punt-naar-Site en ExpressRoute. Vergelijk de verschillende cross-premises-verbindingen die beschikbaar zijn. Welke optie die u kiest kan afhankelijk zijn van verschillende overwegingen, zoals:

* Wat voor doorvoer heeft uw oplossing nodig?
* Wilt u communiceren via het openbare internet met een beveiligd VPN, of via een particuliere verbinding?
* Hebt u de beschikking over een openbaar IP-adres?
* Bent u van plan om een VPN-apparaat te gebruiken? Zo ja, is het compatibel?
* Verbindt u slechts enkele computers met elkaar, of u wilt een persistente verbinding voor uw site?
* Welk type VPN-gateway is vereist voor de oplossing die u wilt maken?
* Welke gateway SKU moet u gebruiken?

### <a name="planningtable"></a>Tabel plannen

De volgende tabel kunt u de beste connectiviteitsoptie voor uw oplossing bepalen.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>Werkstroom

De volgende lijst geeft een overzicht van de algemene werkstroom voor cloud-connectiviteit.

1. Ontwerpen en plannen van de topologie van uw netwerkverbinding en vermeld de-adresruimtes voor alle netwerken die u verbinding wilt maken.
2. Maak een Azure-netwerk. 
3. Een VPN-gateway voor het virtuele netwerk maken.
4. Maken en verbindingen met on-premises netwerken of andere virtuele netwerken configureren (indien nodig).
5. Maak en configureer een punt-naar-Site-verbinding voor uw Azure VPN-gateway (indien nodig).

## <a name="design"></a>Ontwerp
### <a name="topologies"></a>Topologieën voor verbinding

Starten door te kijken naar de diagrammen in de [over VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel. Het artikel bevat basic diagrammen, het implementatiemodel voor elke topologie en de beschikbare implementatiehulpmiddelen die u kunt gebruiken voor het implementeren van uw configuratie.

### <a name="designbasics"></a>Basisprincipes van ontwerp

De volgende secties worden de basisprincipes van VPN-gateway. 

#### <a name="servicelimits"></a>Limieten voor netwerken services

Blader door de tabellen om weer te geven [netwerken services limieten](../azure-subscription-service-limits.md#networking-limits). Uw ontwerp mogelijk van invloed op de grenzen die worden vermeld.

#### <a name="subnets"></a>Over subnetten

Als u verbindingen maakt, moet u rekening houden met subnet bereiken. U kunt geen overlappende subnet-adresbereiken. Een overlappend subnet is als een virtueel netwerk of on-premises locatie bevat dezelfde adresruimte met de andere locatie. Dit betekent dat u uw netwerk engineers voor uw lokale on-premises netwerken moet reserveren een bereik moet worden gebruikt voor uw Azure-IP-adressering ruimte/subnetten. U moet een adresruimte die niet wordt gebruikt op de lokale on-premises netwerk.

Vermijden overlappende subnetten is ook belangrijk wanneer u met VNet-naar-VNet-verbindingen werkt. Als uw subnetten overlappen en een IP-adres in zowel de verzendende en de bestemming VNets bestaat, mislukt de VNet-naar-VNet-verbindingen. Azure kan niet de gegevens routeren naar het andere VNet omdat het doeladres deel uitmaakt van de verzendende VNet.

VPN-Gateways vereist een specifiek subnet met de naam van een gatewaysubnet. Voor een goede werking moeten alle gatewaysubnetten de naam GatewaySubnet krijgen. Zorg ervoor dat niet als de naam van een andere naam voor uw gatewaysubnet en virtuele machines of iets anders in het gatewaysubnet niet implementeren. Zie [Gatewaysubnetten](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Over lokale netwerkgateways

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. In het klassieke implementatiemodel, de lokale netwerkgateway aangeduid als een lokale netwerksite. Wanneer u een lokale netwerkgateway configureert, u een naam geven, geeft het openbare IP-adres van de on-premises VPN-apparaat en de adresvoorvoegsels die zich in de on-premises locatie opgeven. Azure kijkt naar de bestemmingsadressen voor netwerkverkeer, raadpleegt de configuratie die u hebt opgegeven voor de lokale netwerkgateway en routeert pakketten dienovereenkomstig. U kunt de adresvoorvoegsels zo nodig wijzigen. Zie voor meer informatie [lokale netwerkgateways](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Over gatewaytypen

Als u het juiste Gatewaytype voor uw topologie is essentieel. Als u het verkeerde type selecteert, werkt uw gateway niet goed. Het Gatewaytype bepaalt hoe de gateway zelf is verbonden. Het is een vereiste configuratie-instelling voor het Resource Manager-implementatiemodel.

De gatewaytypen zijn:

* VPN
* ExpressRoute

#### <a name="connectiontype"></a>Over verbindingstypen

Elke configuratie vereist een specifiek verbindingstype. De verbindingstypen zijn:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

#### <a name="vpntype"></a>Over VPN-typen

Elke configuratie vereist een specifiek VPN-type. Als u twee configuraties combineert - u maakt bijvoorbeeld een site-naar-site-verbinding en een punt-naar-site-verbinding met hetzelfde VNet - dan moet u een VPN-type gebruiken dat voldoet aan de vereisten van beide verbindingen.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

De volgende tabellen tonen de VPN-type, zoals deze wordt toegewezen aan de configuratie van elke verbinding. Zorg ervoor dat het VPN-type voor uw gateway overeenkomt met de configuratie die u wilt maken. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>VPN-apparaten voor Site-naar-Site-verbindingen

Configureren van een Site-naar-Site-verbinding, ongeacht implementatiemodel, moet u de volgende items:

* Een VPN-apparaat dat compatibel is met Azure VPN-gateways
* Een openbare IPv4-adres die zich niet achter een NAT bevinden.

U moet uw VPN-apparaat configureren ervaring hebben of dat er iemand die het apparaat voor u configureren kunt.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Houd rekening met het geforceerd tunnel routering

U kunt voor de meeste configuraties configureren geforceerde tunneling. Geforceerde tunneling kunt u omleiding of 'force' alle internetverkeer terug naar uw on-premises locatie via een Site-naar-Site VPN-tunnel voor inspectie en controle. Dit is een kritieke beveiligingsvereiste voor de meeste bedrijven beleidsregels. 

Zonder geforceerde tunneling wordt internetverkeer van uw virtuele machines in Azure altijd passeren van Azure netwerkinfrastructuur rechtstreeks uit met het Internet, zonder de optie kunt u controleren of het verkeer controleren. Niet-geautoriseerde toegang tot het Internet kan mogelijk leiden tot vrijgeven van informatie of andere typen schendingen van de beveiliging.

Een geforceerde tunneling verbinding kan worden geconfigureerd in beide implementatiemodellen en met behulp van verschillende hulpprogramma's. Zie voor meer informatie [geforceerde tunneling configureren](vpn-gateway-forced-tunneling-rm.md).

**Geforceerde tunneling diagram**

![Azure VPN-Gateway geforceerde tunneling diagram](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Volgende stappen

Zie de [Veelgestelde vragen over het VPN-Gateway](vpn-gateway-vpn-faq.md) en [over VPN-Gateway](vpn-gateway-about-vpngateways.md) artikelen voor meer informatie om u te helpen uw ontwerp.

Zie voor meer informatie over specifieke gatewayinstellingen [over VPN-Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).