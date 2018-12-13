---
title: 'Toevoegen van een virtuele netwerkgateway met een VNet voor ExpressRoute: PowerShell: Azure | Microsoft Docs'
description: Dit artikel helpt u bij het toevoegen van VNet-gateway naar een bestaande Resource Manager VNet voor ExpressRoute.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 04/17/2017
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 8f8d021adae1cb25198c53e486fdb50e42ac211b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097910"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Een virtuele netwerkgateway configureren voor ExpressRoute met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassiek - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Dit artikel helpt u bij het toevoegen en verwijderen van de gateway van een virtueel netwerk (VNet) voor een bestaande VNet vergroten of verkleinen. De stappen voor deze configuratie zijn specifiek voor VNets die zijn gemaakt met het implementatiemodel van Resource Manager die worden gebruikt voor een ExpressRoute-configuratie. Zie voor meer informatie over virtuele netwerkgateways en configuratie-instellingen voor ExpressRoute gateway [virtuele netwerkgateways voor ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Voordat u begint
Controleer of de nieuwste Azure PowerShell-cmdlets zijn geïnstalleerd. Als u de meest recente cmdlets nog niet hebt geïnstalleerd, moet u om dit te doen voordat u begint met de configuratiestappen. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet koppelen aan een ExpressRoute-circuit. Zie [een Virtueelnetwerk koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md).

