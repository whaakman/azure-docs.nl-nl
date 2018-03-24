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
ms.openlocfilehash: c06e69dd9d1997500589659e936dc25ee01ed145
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
Voor de huidige SKU's (VpnGw1, VpnGw2 en VPNGW3) die u wilt vergroten of verkleinen van uw gateway-SKU om bij te werken naar een krachtigere, kunt u de `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet. Ook kunt u de gateway-SKU-grootte die met deze cmdlet downgraden. Als u de standaard gateway-SKU, [gebruik in plaats daarvan deze instructies](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) het formaat van uw gateway.

De volgende PowerShell-voorbeeld ziet u een gateway-SKU voor VpnGw2 wordt gewijzigd.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

U kunt ook de grootte van een gateway in de Azure portal door te gaan naar de **configuratie** pagina voor uw virtuele netwerkgateway en het selecteren van een andere SKU uit de vervolgkeuzelijst.