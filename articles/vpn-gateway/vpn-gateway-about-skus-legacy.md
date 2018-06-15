---
title: Verouderde virtueel netwerk van Azure VPN-gateway-SKU's | Microsoft Docs
description: Werken met de oude virtuele netwerkgateway SKU's; Basic, Standard en HighPerformance.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2018
ms.author: cherylmc
ms.openlocfilehash: 4feecb9c1e91e1bc6c66a610c092e7bf894886e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30190225"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Werken met virtuele netwerkgateway SKU's (verouderde SKU's)

In dit artikel bevat informatie over de bestaande (oude) virtuele netwerkgateway SKU's. Het verouderde SKU's werkt nog steeds in beide implementatiemodellen voor VPN-gateways die al zijn gemaakt. Klassieke VPN-gateways blijven gebruiken van de verouderde SKU's, zowel voor gateways voor bestaande en nieuwe gateways. Bij het maken van nieuwe Resource Manager VPN gateways, gebruikt u de nieuwe gateway-SKU's. Zie voor meer informatie over de nieuwe SKU's [over VPN-Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Geschatte geaggregeerde doorvoer per SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Ondersteunde configuraties op SKU- en VPN-type

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Een gateway vergroten of verkleinen

U kunt de grootte van uw gateway uit naar een gateway-SKU binnen dezelfde SKU-familie. Bijvoorbeeld, als er een standaard SKU, u kunt de grootte op een HighPerformance-SKU. U kunt echter kan niet het formaat van uw VPN-gateway tussen de oude SKU's en de nieuwe SKU-families. U kunt niet bijvoorbeeld van een standaard SKU gaan een SKU VpnGw2 of een basis-SKU voor VpnGw1.

Als u een gateway voor het klassieke implementatiemodel, kunt u de volgende opdracht gebruiken:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Als u een gateway voor het Resource Manager-implementatiemodel met behulp van PowerShell, gebruikt u de volgende opdracht:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
U kunt ook de grootte van een gateway in de Azure portal.

## <a name="change"></a>Wijzig in de nieuwe gateway-SKU 's

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de nieuwe Gateway-SKU's [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gwsku).

Zie voor meer informatie over configuratie-instellingen, [configuratie-instellingen over VPN-Gateway](vpn-gateway-about-vpn-gateway-settings.md).