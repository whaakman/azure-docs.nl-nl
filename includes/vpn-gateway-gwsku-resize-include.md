---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 081352a23e6a0d8f9e2daa77eca1f8ac85172ff6
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418225"
---
Voor de huidige SKU's (VpnGw1, VpnGw2 en VPNGW3) die u wilt vergroten of verkleinen van de gateway-SKU om bij te werken naar een krachtigere, kunt u de `Resize-AzVirtualNetworkGateway` PowerShell-cmdlet. U kunt ook de gateway-SKU-grootte die met deze cmdlet downgraden. Als u van de Basic gateway-SKU gebruikmaakt, [gebruik in plaats daarvan deze instructies](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) formaat van uw gateway.

De volgende PowerShell-voorbeeld ziet u een gateway-SKU wordt uitgebreid tot VpnGw2.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

U kunt ook het formaat van een gateway in Azure portal door te gaan naar de **configuratie** pagina voor de gateway van virtueel netwerk en u een andere SKU selecteert in de vervolgkeuzelijst.