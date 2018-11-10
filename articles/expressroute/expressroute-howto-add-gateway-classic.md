---
title: 'Configureren van een VNet-gateway voor ExpressRoute met behulp van PowerShell: klassiek: Azure | Microsoft Docs'
description: Configureren van een VNet-gateway voor een klassieke implementatie model VNet met behulp van PowerShell voor een ExpressRoute-configuratie.
documentationcenter: na
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 11/05/2018
ms.author: charwen
ms.openlocfilehash: 82deca87f2410afab7b63f03b222db71db8d9b5c
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218218"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Een virtuele netwerkgateway configureren voor ExpressRoute met behulp van PowerShell (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassiek - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

In dit artikel begeleidt u door de stappen voor het toevoegen en verwijderen van de gateway van een virtueel netwerk (VNet) voor een bestaande VNet vergroten of verkleinen. De stappen voor deze configuratie zijn specifiek voor VNets die zijn gemaakt met behulp van de **klassieke implementatiemodel** en dat wordt gebruikt in een ExpressRoute-configuratie. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Voordat u begint
Controleer of dat u de Azure PowerShell-cmdlets die nodig zijn voor deze configuratie hebt geïnstalleerd (1.0.2 of hoger). Als u de cmdlets niet hebt geïnstalleerd, moet u dit doen voordat u begint met de configuratiestappen. Zie voor meer informatie over het installeren van Azure PowerShell [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u de VNet-gateway hebt gemaakt, kunt u uw VNet koppelen aan een ExpressRoute-circuit. Zie [een Virtueelnetwerk koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md).

