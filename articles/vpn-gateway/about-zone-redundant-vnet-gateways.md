---
title: Zone-redundante virtuele netwerkgateways in Azure-Beschikbaarheidszones | Microsoft Docs
description: Meer informatie over VPN-Gateway en ExpressRoute-gateways in Beschikbaarheidszones.
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 27bac5265a5e884b808c4ccb58fda0b2fffeb774
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975648"
---
# <a name="about-zone-redundant-virtual-network-gateways-in-azure-availability-zones"></a>Zone-redundante virtuele netwerkgateways in Azure-Beschikbaarheidszones

U kunt VPN en ExpressRoute-gateways in implementeren [Azure Availability Zones](../availability-zones/az-overview.md). Dit zorgt voor tolerantie, schaalbaarheid en hogere mate van beschikbaarheid naar virtuele netwerkgateways. Implementeren van gateways in Azure-Beschikbaarheidszones fysiek en logisch, scheidt gateways binnen een regio, terwijl u uw on-premises netwerkconnectiviteit naar Azure beveiligt tegen storingen van de zone-niveau.

### <a name="zrgw"></a>Zone-redundante gateways

Voor het automatisch implementeren van uw virtuele netwerkgateways in meerdere beschikbaarheidszones, kunt u zone-redundante virtuele netwerkgateways. Met zone-redundante gateways, kunt u profiteren van zoneflexibiliteit voor toegang tot uw essentiële, schaalbare services op Azure.

<br>
<br>

![zone-redunant gateways afbeelding](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Zonegebonden gateways

U kunt zonegebonden gateways gebruiken voor het implementeren van gateways in een specifieke zone. Wanneer u een zonegebonden gateway implementeert, worden alle exemplaren van de gateway in dezelfde Beschikbaarheidszone geïmplementeerd.

<br>
<br>

![afbeelding van zonegebonden gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Gateway-SKU's

Zone-redundante en zonegebonden gateways zijn beschikbaar als nieuwe gateway-SKU's. We hebben nieuwe virtuele netwerkgateway-SKU's toegevoegd in de AZ Azure-regio's. Deze SKU's zijn vergelijkbaar met de bijbehorende bestaande SKU's voor ExpressRoute en VPN-Gateway, behalve dat ze specifiek voor de zone-redundante en zonegebonden gateways zijn.

De nieuwe gateway SKU's zijn:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>Openbare IP-SKU 's

Zone-redundante gateways en zonegebonden gateways beide zijn afhankelijk van de Azure openbare IP-adresresource *Standard* SKU. De configuratie van de Azure openbare IP-adresresource wordt bepaald of de gateway die u implementeert zone-redundante of zonegebonden. Als u een openbare IP-resource met maakt een *Basic* SKU, de gateway heeft geen eventuele zoneredundantie en de gateway-resources worden regionale.

### <a name="pipzrg"></a>Zone-redundante gateways

Wanneer u een openbaar IP-adres met maakt de **Standard** openbare IP-SKU zonder een zone op te geven, het gedrag verschilt, afhankelijk van of de gateway een VPN-gateway of een ExpressRoute-gateway is. 

* Voor een VPN-gateway, wordt de twee gateway-instanties worden geïmplementeerd in elke 2 buiten deze drie zones om zone-redundantie te verlenen. 
* Voor een ExpressRoute-gateway, omdat er meer dan twee instanties, kunt de gateway overbruggen alle drie zones.

### <a name="pipzg"></a>Zonegebonden gateways

Wanneer u een openbaar IP-adres met maakt de **Standard** openbare IP-SKU en geef de Zone (1, 2 of 3), alle gatewayexemplaren wordt geïmplementeerd in dezelfde regio.

### <a name="piprg"></a>Regionale gateways

Wanneer u een openbaar IP-adres met maakt de **Basic** openbare IP-SKU, de gateway wordt geïmplementeerd als een regionale gateway en hoeft niet elke zone-redundantie ingebouwd in de gateway.

## <a name="faq"></a>Veelgestelde vragen

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Wat verandert er wanneer ik deze nieuwe SKU's implementeren?

U kunt uw gateways met zone-redundantie implementeren vanuit het perspectief. Dit betekent dat alle exemplaren van de gateways worden geïmplementeerd in Azure-Beschikbaarheidszones en elke Beschikbaarheidszone een ander fout- en -domein is. Dit maakt uw gateways betrouwbare, beschikbare en flexibele op fouten van de zone.

### <a name="can-i-use-the-azure-portal"></a>Kan ik de Azure-portal gebruiken?

Ja, kunt u de Azure-portal naar de nieuwe SKU's implementeren. Echter, ziet u deze nieuwe SKU's alleen in de Azure-regio's waarvoor Azure-Beschikbaarheidszones.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Welke regio's zijn beschikbaar voor de nieuwe SKU's worden gebruikt?

De nieuwe SKU's zijn beschikbaar in Azure-regio's waarvoor Azure Availability Zones - VS centraal, Frankrijk-centraal en West-Europa. Vanaf nu maken we de Zone-redundante Gateways beschikbaar wilt voor u in andere openbare Azure-regio.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kan ik wijzigen/migreren/upgrade mijn bestaande virtuele netwerkgateways met zone-redundante of zonegebonden-gateways?

Migreren van uw bestaande virtuele netwerkgateways met zone-redundante of zonegebonden gateways wordt momenteel niet ondersteund. U kunt echter uw bestaande gateway verwijderen en opnieuw maken van een zone-redundante of zonegebonden-gateway.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kan ik een VPN- en Expressroute-gateways in hetzelfde virtuele netwerk implementeren?

Meerdere VPN- en Expressroute-gateways in hetzelfde virtuele netwerk wordt ondersteund. U moet echter een/27 reserveren IP-adresbereik voor het gatewaysubnet.

## <a name="next-steps"></a>Volgende stappen

[Een zone-redundante virtuele netwerkgateway maken](create-zone-redundant-vnet-gateway.md)