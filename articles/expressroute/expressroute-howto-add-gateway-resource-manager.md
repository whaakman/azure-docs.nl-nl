---
title: 'De gateway van een virtueel netwerk met een VNet voor ExpressRoute toevoegen: PowerShell: Azure | Microsoft Docs'
description: Dit artikel helpt u bij het toevoegen van VNet-gateway naar een bestaande Resource Manager VNet voor ExpressRoute.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 02/21/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 3c91fd6140b460d29b33e7d9b1fabafbbcf99422
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57406215"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Een virtuele netwerkgateway configureren voor ExpressRoute met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassiek - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Dit artikel helpt u bij het toevoegen en verwijderen van de gateway van een virtueel netwerk (VNet) voor een bestaande VNet vergroten of verkleinen. De stappen voor deze configuratie van toepassing op VNets die zijn gemaakt met het Resource Manager-implementatiemodel voor een ExpressRoute-configuratie. Zie voor meer informatie, [virtuele netwerkgateways voor ExpressRoute](expressroute-about-virtual-network-gateways.md).

## <a name="before-beginning"></a>Voordat u begint

### <a name="working-with-powershell"></a>Werken met PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [working with cloud shell](../../includes/expressroute-cloud-shell-powershell-about.md)]

### <a name="configuration-reference-list"></a>Lijst van configuratie-verwijzing

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet koppelen aan een ExpressRoute-circuit. Zie [een Virtueelnetwerk koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md).