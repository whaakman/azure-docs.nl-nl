---
title: Over ExpressRoute-gateways voor virtueel netwerk - Azure | Microsoft Docs
description: Meer informatie over virtuele netwerkgateways voor ExpressRoute. In dit artikel bevat informatie over gateway-SKU's en -typen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 18615cf737eedcd188fd59d2aa98482210b9333a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991594"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>Virtuele ExpressRoute-netwerkgateway en FastPath
Als u wilt verbinding maken met uw Azure-netwerk en uw on-premises netwerk via ExpressRoute, moet u eerst een virtuele netwerkgateway maken. Een virtuele netwerkgateway heeft een tweeledig doel: exchange IP-routes tussen de netwerken en netwerkverkeer routeren. In dit artikel wordt uitgelegd gatewaytypen, gateway-SKU's en geschatte prestatieverbetering SKU. Dit artikel wordt ook uitgelegd ExpressRoute [FastPath](#fastpath), een functie waarmee het netwerkverkeer van uw on-premises netwerk om over te slaan van de gateway van het virtuele netwerk om prestaties te verbeteren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Gatewaytypen

Wanneer u een virtuele netwerkgateway maakt, moet u verschillende instellingen opgeven. Een van de vereiste instellingen '-GatewayType', geeft aan of de gateway wordt gebruikt voor ExpressRoute of VPN-verkeer. De twee gatewaytypen zijn:

* **VPN** - versleuteld verkeer wordt verzonden via het openbare Internet, gebruikt u het Gatewaytype 'Vpn'. Dit is ook een VPN-gateway genoemd. Site-naar-site-, punt-naar-site- en VNet-naar-VNet-verbindingen gebruiken allemaal een VPN-gateway.

* **ExpressRoute** : als u wilt verzenden van netwerkverkeer op een privéverbinding, gebruikt u het Gatewaytype 'ExpressRoute'. Dit wordt ook wel een ExpressRoute-gateway genoemd en is van het type van de gateway die wordt gebruikt bij het configureren van ExpressRoute.

Elk virtueel netwerk kan maar één virtuele netwerkgateway per type gateway hebben. U kunt voor een virtueel netwerk bijvoorbeeld één gateway gebruiken die -GatewayType Vpn gebruikt en één die -GatewayType ExpressRoute gebruikt.

## <a name="gwsku"></a>Gateway-SKU's
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Als u upgraden van uw gateway naar een krachtigere gateway-SKU wilt, in de meeste gevallen kunt u de 'Resize-AzVirtualNetworkGateway' PowerShell-cmdlet. Dit werkt voor upgrades naar Standard en HighPerformance-SKU's. Echter, om te upgraden naar de SKU UltraPerformance, moet u de gateway opnieuw maken. Opnieuw maken van een gateway veroorzaakt uitvaltijd.

### <a name="aggthroughput"></a>Geschatte prestaties per gateway-SKU
De volgende tabel ziet u de gatewaytypen en de geschatte prestaties. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> De prestaties van toepassingen, is afhankelijk van meerdere factoren, zoals de end-to-end-latentie en het aantal verkeer stroomt van die de toepassing wordt geopend. De getallen in de tabel staan de bovengrens die de toepassing in theorie in een ideale omgeving bereiken kan.
>
>

### <a name="zrgw"></a>Zone-redundante gateway-SKU 's

U kunt ook ExpressRoute-gateways in Azure-Beschikbaarheidszones implementeren. Dit fysiek en logisch gescheiden in verschillende Beschikbaarheidszones, uw on-premises netwerkconnectiviteit naar Azure beveiligen tegen storingen van de zone-niveau.

![Zone-redundante ExpressRoute-gateway](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Zone-redundante gateways specifieke nieuwe gateway-SKU's voor ExpressRoute-gateway gebruiken.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

De nieuwe gateway-SKU's ondersteunen ook andere implementatie-opties om het beste aan uw behoeften te voldoen. Bij het maken van een virtuele netwerkgateway met behulp van de nieuwe gateway-SKU's, hebt u ook de mogelijkheid om de gateway in een specifieke zone te implementeren. Dit is een zonegebonden gateway genoemd. Wanneer u een zonegebonden gateway implementeert, worden alle exemplaren van de gateway in dezelfde Beschikbaarheidszone geïmplementeerd.

## <a name="fastpath"></a>FastPath
ExpressRoute-gateway voor virtueel netwerk is ontworpen voor netwerkroutes uitwisselen en netwerkverkeer te routeren. FastPath is ontworpen voor het verbeteren van de prestaties voor het pad van gegevens tussen uw on-premises netwerk en het virtuele netwerk. Wanneer dit is ingeschakeld, verzendt FastPath netwerkverkeer rechtstreeks naar virtuele machines in het virtuele netwerk, overslaan van de gateway. 

FastPath is beschikbaar op [ExpressRoute Direct](expressroute-erdirect-about.md) alleen. Met andere woorden, kunt u deze functie alleen als u [verbinding maken met uw virtuele netwerk](expressroute-howto-linkvnet-arm.md) aan een ExpressRoute-circuit dat is gemaakt op de poort van een ExpressRoute-Direct. FastPath is nog steeds vereist voor een virtuele netwerkgateway moet worden gemaakt voor het uitwisselen van routes tussen virtuele netwerken en on-premises netwerk. De virtuele netwerkgateway moet Ultra-Performance of ErGw3AZ.

FastPath biedt geen ondersteuning voor de volgende functies:
* UDR op het gatewaysubnet: als u een UDR van toepassing op het gatewaysubnet van uw virtuele netwerk dat het netwerkverkeer van uw on-premises netwerk worden verzonden naar de virtuele netwerkgateway wordt voortgezet.
* VNet-Peering: als u beschikt over andere virtuele netwerken gekoppeld aan de map die is verbonden met ExpressRoute, het netwerkverkeer van uw on-premises netwerk naar de andere virtuele netwerken (dat wil zeggen de zogeheten 'Knooppunt' vnet's blijft) worden verzonden naar het virtuele netwerk gateway. De tijdelijke oplossing is voor de virtuele netwerken aan ExpressRoute-circuit rechtstreeks verbinding te maken.

## <a name="resources"></a>REST-API's en PowerShell-cmdlets
Zie voor aanvullende technische bronnen en de syntaxis van de specifieke vereisten bij het gebruik van REST-API's en PowerShell-cmdlets voor configuraties van virtuele gateway, de volgende pagina's:

| **Klassiek** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Volgende stappen
Zie [overzicht van ExpressRoute](expressroute-introduction.md) voor meer informatie over beschikbare verbindingsconfiguraties.

Zie [een virtuele netwerkgateway maken voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) voor meer informatie over het maken van ExpressRoute-gateways.

Zie [maken van een zone-redundante virtuele-netwerkgateway](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) voor meer informatie over het configureren van de zone-redundante gateways.

Zie [virtueel netwerk koppelen aan ExpressRoute](expressroute-howto-linkvnet-arm.md) voor meer informatie over het inschakelen van FastPath. 
