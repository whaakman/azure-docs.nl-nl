---
title: 'Een VNet-gateway configureren voor ExpressRoute met behulp van PowerShell: klassieke: Azure | Microsoft Docs'
description: Configureer een VNet-gateway voor een klassieke implementatie model VNet met behulp van PowerShell voor een ExpressRoute-configuratie.
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 195a38fa45f1c514a93980e777fb0d8238aa3f3f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Configureren van een virtuele netwerkgateway voor ExpressRoute met behulp van PowerShell (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassieke - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

In dit artikel begeleidt u bij de stappen voor het toevoegen en verwijderen van de gateway van een virtueel netwerk (VNet) voor een bestaande VNet vergroten of verkleinen. De stappen voor deze configuratie zijn specifiek voor VNets die zijn gemaakt met de **klassieke implementatiemodel** en die worden gebruikt in een ExpressRoute-configuratie. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Voordat u begint
Controleer of u de Azure PowerShell-cmdlets die nodig zijn voor deze configuratie hebt geïnstalleerd (1.0.2 of hoger). Als u de cmdlets nog niet hebt geïnstalleerd, moet u dit doen voordat u begint met de configuratiestappen. Zie voor meer informatie over het installeren van Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet koppelen aan een ExpressRoute-circuit. Zie [een virtueel netwerk koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md).

