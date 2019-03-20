---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f6fd4039614dbd7c1a2b2c6ba8403502a6420fe3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57908549"
---
Voor de huidige SKU's (VpnGw1, VpnGw2 en VPNGW3) die u wilt vergroten of verkleinen van de gateway-SKU om bij te werken naar een krachtigere, kunt u de `Resize-AzVirtualNetworkGateway` PowerShell-cmdlet. U kunt ook de gateway-SKU-grootte die met deze cmdlet downgraden. Als u van de Basic gateway-SKU gebruikmaakt, [gebruik in plaats daarvan deze instructies](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) formaat van uw gateway.

De volgende PowerShell-voorbeeld ziet u een gateway-SKU wordt uitgebreid tot VpnGw2.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

U kunt ook het formaat van een gateway in Azure portal door te gaan naar de **configuratie** pagina voor de gateway van virtueel netwerk en u een andere SKU selecteert in de vervolgkeuzelijst.