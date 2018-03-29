---
title: Over de virtuele netwerkgateways ExpressRoute | Microsoft Docs
description: Meer informatie over virtuele netwerkgateways voor ExpressRoute.
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: cherylmc
ms.openlocfilehash: ae971e7743d6dd3269c0a4f976bd2a5316300f58
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Virtuele netwerkgateways voor ExpressRoute
Een virtuele netwerkgateway wordt gebruikt voor het verzenden van netwerkverkeer tussen virtuele netwerken van Azure en on-premises locaties. Wanneer u een ExpressRoute-verbinding configureert, moet u maken en configureren van een virtuele netwerkgateway en een verbinding met virtual network gateway.

Wanneer u een gateway voor een virtueel netwerk maakt, geeft u verschillende instellingen op. Een van de vereiste instellingen geeft aan of de gateway wordt gebruikt voor ExpressRoute of Site-naar-Site VPN-verkeer. In het Resource Manager-implementatiemodel, de instelling is '-GatewayType'.

Als het netwerkverkeer op een persoonlijke verbinding die wordt verzonden, gebruikt u het Gatewaytype 'ExpressRoute'. Dit wordt ook wel een ExpressRoute-gateway genoemd. Als het netwerkverkeer versleutelde via het openbare Internet worden verzonden, gebruikt u het Gatewaytype 'VPN-'. Dit wordt ook wel een VPN-gateway genoemd. Site-naar-site-, punt-naar-site- en VNet-naar-VNet-verbindingen gebruiken allemaal een VPN-gateway.

Elk virtueel netwerk kan maar één virtuele netwerkgateway per type gateway hebben. U kunt voor een virtueel netwerk bijvoorbeeld één gateway gebruiken die -GatewayType Vpn gebruikt en één die -GatewayType ExpressRoute gebruikt. Dit artikel is gericht op de virtuele netwerkgateway ExpressRoute.

## <a name="gwsku"></a>Gateway-SKU's
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Als u upgraden van uw gateway uit naar een krachtige gateway SKU wilt, in de meeste gevallen kunt u de 'Formaat AzureRmVirtualNetworkGateway' PowerShell-cmdlet. Dit werkt voor upgrades naar Standard en HighPerformance SKU's. Echter te kunnen upgraden naar de UltraPerformance SKU, moet u de gateway opnieuw maken.

### <a name="aggthroughput"></a>Geschatte prestaties door de gateway-SKU
De volgende tabel ziet u de gatewaytypen en de geschatte prestaties. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> De prestaties van toepassingen is afhankelijk van meerdere factoren, zoals de end-to-end-latentie en het nummer van de verkeersstromen die de toepassing wordt geopend. De getallen in de tabel vertegenwoordigen de bovengrens die de toepassing theoretisch in een ideale omgeving kunt bereiken. 
> 
>

## <a name="resources"></a>REST-API's en PowerShell-cmdlets
Zie voor aanvullende technische bronnen en specifieke syntaxis vereisten bij het gebruik van REST-API's en PowerShell-cmdlets voor gateway-configuraties van virtuele netwerken, de volgende pagina's:

| **Klassiek** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Volgende stappen
Zie [overzicht van ExpressRoute](expressroute-introduction.md) voor meer informatie over beschikbare verbinding configuraties.

Zie [maken van een virtuele netwerkgateway voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) voor meer informatie over het maken van ExpressRoute-gateways.
