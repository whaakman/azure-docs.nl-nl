---
title: Verouderde met Azure virtual network VPN-gateway-SKU's | Microsoft Docs
description: Over het werken met de oude virtuele netwerkgateway SKU 's Basic, Standard en HighPerformance.
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
ms.openlocfilehash: efce8379ecafe6e8e044b654a3c5b392ca8e9cea
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55506359"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Werken met virtual network gateway-SKU's (verouderde SKU's)

In dit artikel bevat informatie over het verouderde (oude) virtueel netwerkgateway SKU's. Het verouderde SKU's werkt nog steeds in beide implementatiemodellen voor VPN-gateways die al zijn gemaakt. Klassieke VPN-gateways echter ook doorgaan met de verouderde SKU's, zowel voor bestaande gateways, en voor de nieuwe gateways. Bij het maken van nieuwe Resource Manager VPN gateways, gebruikt u de nieuwe gateway-SKU's. Zie voor meer informatie over de nieuwe SKU's [over VPN-Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Gateway-SKU's

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>Geschatte geaggregeerde doorvoer per SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Ondersteunde configuraties per SKU- en VPN-type

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Formaat van een gateway

U kunt het formaat van uw gateway op een gateway-SKU binnen dezelfde SKU-familie. Bijvoorbeeld, als u een standaard-SKU hebt, u kunt het formaat naar een HighPerformance-SKU. U kunt geen echter grootte van uw VPN-gateway tussen de oude SKU's en de nieuwe SKU-families. U kan geen bijvoorbeeld Ga van een standaard-SKU naar een VpnGw2-SKU of een basis-SKU naar VpnGw1.

Als u wilt het formaat van een gateway voor het klassieke implementatiemodel, gebruik de volgende opdracht:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Als u wilt het formaat van een gateway voor het Resource Manager-implementatiemodel met behulp van PowerShell, gebruik de volgende opdracht:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
U kunt ook het formaat van een gateway in Azure portal.

## <a name="change"></a>Met de nieuwe gateway SKU's wijzigen

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de nieuwe Gateway-SKU's, [Gateway-SKU's](vpn-gateway-about-vpngateways.md#gwsku).

Zie voor meer informatie over configuratie-instellingen, [over VPN-gatewayconfiguratie-instellingen](vpn-gateway-about-vpn-gateway-settings.md).