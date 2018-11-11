---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 11/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fab00e5281bb91bce10228b3bc2e9cfd503d5d5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219814"
---
U moet een VNet en een gatewaysubnet eerst maken voordat u op de volgende taken kunt werken.

> [!NOTE]
> Deze voorbeelden niet van toepassing op S2S en ExpressRoute-configuraties naast elkaar bestaan.
> Zie voor meer informatie over het werken met gateways in een configuratie gebruiken [naast elkaar bestaande verbindingen configureren](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Een gateway toevoegen

Gebruik de onderstaande opdracht om een gateway te maken. Zorg ervoor dat vervangen door uw eigen waarden.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Controleer of dat de gateway is gemaakt

Gebruik de volgende opdracht om te controleren dat de gateway is gemaakt. Met deze opdracht haalt ook de gateway-ID, die u nodig hebt voor andere bewerkingen.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Formaat van een gateway

Er zijn een aantal [Gateway-SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de Gateway-SKU op elk gewenst moment wijzigen.

> [!IMPORTANT]
> Met deze opdracht werkt niet voor UltraPerformance-gateway. Als u wilt wijzigen van de gateway naar een UltraPerformance-gateway, verwijdert u eerst de bestaande ExpressRoute-gateway en maak vervolgens een nieuwe UltraPerformance-gateway. Als u wilt downgraden van de gateway van een UltraPerformance-gateway, verwijdert u eerst de UltraPerformance-gateway en maak vervolgens een nieuwe gateway. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht om een gateway te verwijderen

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```
