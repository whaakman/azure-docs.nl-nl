---
title: 'Planning en ontwerp voor cross-premises verbindingen: Azure VPN-Gateway | Microsoft Docs'
description: Meer informatie over VPN-Gateway plannen en ontwerpen voor cross-premises en hybride VNet-naar-VNet-verbindingen
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/27/2017
ms.author: yushwang
ms.openlocfilehash: 7802061ba09a30ca34ed3804ace846118c5edb9b
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56235358"
---
# <a name="planning-and-design-for-vpn-gateway"></a>Planning en ontwerp voor VPN Gateway

Plannen en ontwerpen van uw cross-premises en VNet-naar-VNet-configuraties kunnen worden eenvoudig of ingewikkeld zijn, afhankelijk van de behoeften van uw netwerk. Dit artikel begeleidt u bij basic plannings- en ontwerpoverwegingen.

## <a name="planning"></a>Planning

### <a name="compare"></a>Opties voor cross-premises connectiviteit

Als u uw on-premises sites veilig verbinding maken met een virtueel netwerk wilt, hebt u drie verschillende manieren om dit te doen: Site-naar-Site, punt-naar-Site en ExpressRoute. Vergelijk de verschillende cross-premises-verbindingen die beschikbaar zijn. Welke optie die u kiest kan afhankelijk zijn van verschillende overwegingen, zoals:

* Wat voor doorvoer heeft uw oplossing nodig?
* Wilt u communiceren via het openbare internet met een beveiligd VPN, of via een particuliere verbinding?
* Hebt u de beschikking over een openbaar IP-adres?
* Bent u van plan om een VPN-apparaat te gebruiken? Zo ja, is het compatibel?
* Verbindt u slechts enkele computers met elkaar, of u wilt een persistente verbinding voor uw site?
* Welk type VPN-gateway is vereist voor de oplossing die u wilt maken?
* Welke gateway-SKU moet u gebruiken?

### <a name="planningtable"></a>Tabel plannen

De volgende tabel kan u helpen bij het kiezen van de beste connectiviteitsoptie voor uw oplossing.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>Werkstroom

De volgende lijst geeft een overzicht van de algemene werkstroom voor cloud-connectiviteit:

1. Ontwerp en plannen van de topologie van uw verbinding en vermeld de-adresruimtes voor alle netwerken die u verbinding wilt maken.
2. Maak een Azure-netwerk. 
3. Een VPN-gateway voor het virtuele netwerk maken.
4. Maken en configureren van verbindingen met on-premises netwerken of andere virtuele netwerken (indien nodig).
5. Maak en configureer een punt-naar-Site-verbinding voor uw Azure VPN-gateway (indien nodig).

## <a name="design"></a>Ontwerp
### <a name="topologies"></a>Verbindingstopologieën

Eerst kijken naar de diagrammen in de [over VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel. Dit artikel bevat een eenvoudige diagrammen, het implementatiemodel voor elke topologie en de beschikbare implementatiehulpprogramma's die kunt u de configuratie implementeren.

### <a name="designbasics"></a>Basisprincipes van ontwerpen

De volgende secties worden de basisbeginselen van VPN-gateway. 

#### <a name="servicelimits"></a>Netwerklimieten-services

Blader door de tabellen om weer te geven [netwerken services limieten](../azure-subscription-service-limits.md#networking-limits). De vermelde limieten mogelijk van invloed op uw ontwerp.

#### <a name="subnets"></a>Over subnetten

Wanneer u verbindingen maakt, moet u rekening houden met de subnet-adresbereiken. U kunt geen overlappende subnet-adresbereiken. Een overlappend subnet is als een virtueel netwerk of on-premises locatie bevat de dezelfde adresruimte die de andere locatie bevat. Dit betekent dat u moet de technici van uw netwerk voor uw lokale on-premises netwerken een adresbereik moet worden gebruikt voor uw Azure-IP-adres te reserveren adressering ruimte/subnetten. U moet een adresruimte die niet wordt gebruikt op de lokale on-premises netwerk.

Vermijden van overlappende subnetten is ook belangrijk wanneer u met VNet-naar-VNet-verbindingen werkt. Als de subnetten van elkaar overlappen, en een IP-adres in zowel de verzendende en de bestemming vnet's bestaat, mislukt de VNet-naar-VNet-verbindingen. Azure kan niet de gegevens doorsturen naar het andere VNet omdat het doeladres deel uit van de verzendende VNet maakt.

VPN-Gateways vereist een specifiek subnet met de naam een gatewaysubnet. Voor een goede werking moeten alle gatewaysubnetten de naam GatewaySubnet krijgen. Vergeet niet om de naam van een andere naam voor het gatewaysubnet en virtuele machines of iets anders in het gatewaysubnet niet implementeren. Zie [Gatewaysubnetten](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Over lokale netwerkgateways

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. In het klassieke implementatiemodel, de lokale netwerkgateway aangeduid als een lokale netwerksite. Wanneer u een lokale netwerkgateway configureert, u een naam geven, geef het openbare IP-adres van de on-premises VPN-apparaat en de adresvoorvoegsels die zich in de on-premises locatie opgeven. Azure kijkt naar de bestemmingsadressen voor netwerkverkeer, raadpleegt de configuratie die u hebt opgegeven voor de lokale netwerkgateway en routeert pakketten dienovereenkomstig. U kunt de adresvoorvoegsels zo nodig wijzigen. Zie voor meer informatie, [lokale netwerkgateways](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Over gatewaytypen

Selecteren van het juiste Gatewaytype voor uw topologie is essentieel. Als u het verkeerde type selecteert, goed uw gateway niet. Het Gatewaytype bepaalt hoe de gateway zelf is verbonden. Het is een vereiste configuratie-instelling voor het Resource Manager-implementatiemodel.

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

De volgende tabellen ziet het VPN-type als het wordt toegewezen aan de configuratie van elke verbinding. Zorg ervoor dat de VPN-type voor uw gateway komt overeen met de configuratie die u wilt maken. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>VPN-apparaten voor Site-naar-Site-verbindingen

Als u wilt configureren van een Site-naar-Site-verbinding, ongeacht het implementatiemodel, moet u de volgende items:

* Een VPN-apparaat dat compatibel is met Azure VPN-gateways
* Een openbare IPv4-adres dat zich niet achter een NAT-apparaat

U moet over een ervaring voor configuratie van uw VPN-apparaat of iemand anders die het apparaat voor u configureren kunt hebben.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Houd rekening met geforceerde tunnels routering

Voor de meeste configuraties kunt u een geforceerde tunneling configureren. Geforceerde tunneling kunt u omleiding of 'force' alle internetverkeer terug naar uw on-premises locatie via een Site-naar-Site VPN-tunnel voor inspectie en controle. Dit is een essentiële vereiste voor de meeste bedrijven beleid. 

Zonder geforceerde tunnels wordt via internetverkeer van uw virtuele machines in Azure altijd van Azure network-infrastructuur rechtstreeks uit met het Internet, zonder de optie waarmee u kunt controleren of het verkeer controleren. Niet-geautoriseerde toegang tot Internet kan mogelijk leiden tot vrijgeven van informatie of andere typen schendingen van de beveiliging.

Een geforceerde tunneling verbinding kan worden geconfigureerd in beide implementatiemodellen en met behulp van verschillende hulpprogramma's. Zie voor meer informatie, [geforceerde tunneling configureren](vpn-gateway-forced-tunneling-rm.md).

**Geforceerde tunneling diagram**

![Azure VPN-Gateway geforceerde tunneling diagram](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Volgende stappen

Zie de [Veelgestelde vragen over VPN-Gateway](vpn-gateway-vpn-faq.md) en [over VPN-Gateway](vpn-gateway-about-vpngateways.md) artikelen voor meer informatie voor hulp bij het ontwerp.

Zie voor meer informatie over specifieke gateway-instellingen, [informatie over VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).
