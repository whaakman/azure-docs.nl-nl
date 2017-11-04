---
title: Verouderde virtuele Azure-netwerkgateway-SKU's | Microsoft Docs
description: Oude virtual network gateway-SKU's.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2017
ms.author: cherylmc
ms.openlocfilehash: d5127c7fa512bad49817fa4c8edf3a16ca2f7d60
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Werken met virtuele netwerkgateway SKU's (verouderde SKU's)

In dit artikel bevat informatie over de bestaande (oude) virtuele netwerkgateway SKU's. Het verouderde SKU's werkt nog steeds in beide implementatiemodellen voor VPN-gateways die al zijn gemaakt. Klassieke VPN-gateways blijven gebruiken van de verouderde SKU's, zowel voor gateways voor bestaande en nieuwe gateways. Bij het maken van nieuwe Resource Manager VPN gateways, gebruikt u de nieuwe gateway-SKU's. Zie voor meer informatie over de nieuwe SKU's [over VPN-Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Geschatte geaggregeerde doorvoer per SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Ondersteunde configuraties op SKU- en VPN-type

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Een gateway (wijziging een gateway-SKU) vergroten of verkleinen

U kunt de grootte van een gateway-SKU binnen dezelfde SKU-familie. Bijvoorbeeld, als er een standaard SKU, u kunt de grootte op een HighPerformance-SKU. U kan niet de grootte van uw VPN-gateways tussen de oude SKU's en de nieuwe SKU-families. Bijvoorbeeld, gaan u niet van een standaard SKU naar een SKU VpnGw2.

>[!IMPORTANT]
>Wanneer u het formaat van een gateway, hebt u 20-30 minuten uitvaltijd voor deze gateway terwijl is wordt gewijzigd.
>
>

Als u een gateway-SKU voor het klassieke implementatiemodel, kunt u de volgende opdracht gebruiken:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Als u een gateway-SKU voor het Resource Manager-implementatiemodel, kunt u de volgende opdracht gebruiken:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migreren naar de nieuwe gateway-SKU 's

Als u met het implementatiemodel van Resource Manager werkt, kunt u migreren naar de nieuwe gateway-SKU's. Als u met het klassieke implementatiemodel werkt, kunt u niet migreren naar de nieuwe SKU's en in plaats daarvan moet blijven gebruiken van de verouderde SKU's.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de nieuwe Gateway-SKU's [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gwsku).

Zie voor meer informatie over configuratie-instellingen, [configuratie-instellingen over VPN-Gateway](vpn-gateway-about-vpn-gateway-settings.md).
