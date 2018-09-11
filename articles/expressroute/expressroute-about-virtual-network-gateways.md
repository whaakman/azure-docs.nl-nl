---
title: Over Azure ExpressRoute-gateways voor virtueel netwerk | Microsoft Docs
description: Meer informatie over virtuele netwerkgateways voor ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: cherylmc
ms.openlocfilehash: 34d84a27406f0ebabd7bca576ee443da1d0c9bcd
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348801"
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Virtuele netwerkgateways voor ExpressRoute
Een virtuele netwerkgateway wordt gebruikt voor het verzenden van netwerkverkeer tussen virtuele netwerken van Azure en on-premises locaties. U kunt een virtuele netwerkgateway kan worden gebruikt voor verkeer van ExpressRoute of VPN-verkeer. In dit artikel richt zich op de virtuele netwerkgateways voor ExpressRoute.

## <a name="gateway-types"></a>Gatewaytypen

Wanneer u een gateway voor een virtueel netwerk maakt, geeft u verschillende instellingen op. Een van de vereiste instellingen '-GatewayType', geeft aan of de gateway wordt gebruikt voor ExpressRoute of VPN-verkeer. De twee gatewaytypen zijn: 

* **VPN** - versleuteld verkeer wordt verzonden via het openbare Internet, gebruikt u het Gatewaytype 'Vpn'. Dit is ook een VPN-gateway genoemd. Site-naar-site-, punt-naar-site- en VNet-naar-VNet-verbindingen gebruiken allemaal een VPN-gateway.

* **ExpressRoute** : als u wilt verzenden van netwerkverkeer op een privéverbinding, gebruikt u het Gatewaytype 'ExpressRoute'. Dit wordt ook wel een ExpressRoute-gateway genoemd en is van het type gateway die u gebruikt bij het configureren van ExpressRoute.


Elk virtueel netwerk kan maar één virtuele netwerkgateway per type gateway hebben. U kunt voor een virtueel netwerk bijvoorbeeld één gateway gebruiken die -GatewayType Vpn gebruikt en één die -GatewayType ExpressRoute gebruikt.

## <a name="gwsku"></a>Gateway-SKU's
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Als u upgraden van uw gateway naar een krachtigere gateway-SKU wilt, in de meeste gevallen kunt u de grootte-AzureRmVirtualNetworkGateway PowerShell-cmdlet. Dit werkt voor upgrades naar Standard en HighPerformance-SKU's. Echter, om te upgraden naar de SKU UltraPerformance, moet u de gateway opnieuw maken. Opnieuw maken van een gateway veroorzaakt uitvaltijd.

### <a name="aggthroughput"></a>Geschatte prestaties per gateway-SKU
De volgende tabel ziet u de gatewaytypen en de geschatte prestaties. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> De prestaties van toepassingen, is afhankelijk van meerdere factoren, zoals de end-to-end-latentie en het aantal verkeer stroomt van die de toepassing wordt geopend. De getallen in de tabel staan de bovengrens die de toepassing in theorie in een ideale omgeving bereiken kan. 
> 
>

### <a name="zrgw"></a>Zone-redundante gateway-SKU's (Preview)

U kunt ook ExpressRoute-gateways in Azure-Beschikbaarheidszones implementeren. Dit fysiek en logisch gescheiden in verschillende Beschikbaarheidszones, uw on-premises netwerkconnectiviteit naar Azure beveiligen tegen storingen van de zone-niveau.

![Zone-redundante ExpressRoute-gateway](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Zone-redundante gateways specifieke nieuwe gateway-SKU's voor ExpressRoute-gateway gebruiken. De nieuwe SKU's zijn momenteel beschikbaar in **Public Preview**.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

De nieuwe gateway-SKU's ondersteunen ook andere implementatie-opties om het beste aan uw behoeften te voldoen. Bij het maken van een virtuele netwerkgateway met behulp van de nieuwe gateway-SKU's, hebt u ook de mogelijkheid om de gateway in een specifieke zone te implementeren. Dit is een zonegebonden gateway genoemd. Wanneer u een zonegebonden gateway implementeert, worden alle exemplaren van de gateway in dezelfde Beschikbaarheidszone geïmplementeerd. Als u wilt registreren in de Preview-versie, Zie [maken van een zone-redundante virtuele-netwerkgateway](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md).

## <a name="resources"></a>REST-API's en PowerShell-cmdlets
Zie voor aanvullende technische bronnen en de syntaxis van de specifieke vereisten bij het gebruik van REST-API's en PowerShell-cmdlets voor configuraties van virtuele gateway, de volgende pagina's:

| **Klassiek** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Volgende stappen
Zie [overzicht van ExpressRoute](expressroute-introduction.md) voor meer informatie over beschikbare verbindingsconfiguraties.

Zie [een virtuele netwerkgateway maken voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) voor meer informatie over het maken van ExpressRoute-gateways.
