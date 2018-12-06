---
title: Verplaatsen van een openbare peering op Azure ExpressRoute naar Microsoft-peering | Microsoft Docs
description: Dit artikel laat u de stappen voor het verplaatsen van uw openbare peering aan Microsoft-peering voor ExpressRoute.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2018
ms.author: cherylmc
ms.openlocfilehash: 579f8874459004ef6bfa0d0794ab09333e053acb
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966108"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Verplaatsen van een openbare peering naar het Microsoft-peering

ExpressRoute biedt ondersteuning voor het gebruik van Microsoft-peering met routefilters voor Azure PaaS-services, zoals Azure Storage en Azure SQL Database. U hebt nu maar één routeringsdomein nodig voor toegang tot Microsoft PaaS- en SaaS-services. U kunt routefilters gebruiken om de PaaS-servicevoorvoegsels selectief te adverteren voor Azure-regio's die u wilt verbruiken.

Dit artikel helpt u de configuratie van een openbare peering naar Microsoft-peering zonder uitvaltijd. Zie voor meer informatie over routering domeinen en peerings [ExpressRoute-circuits en Routeringsdomeinen](expressroute-circuit-peerings.md).


## <a name="before"></a>Voordat u begint

* Voor verbinding met Microsoft-peering, moet u instellen en beheren van NAT bevinden. Uw connectiviteitsprovider kan instellen en beheren van de NAT als een beheerde service. Als u van plan bent voor toegang tot het Azure PaaS en SaaS van de Azure-services op Microsoft-peering, is het belangrijk om de grootte van de NAT IP-adresgroep correct. Zie voor meer informatie over NAT voor ExpressRoute, de [NAT-vereisten voor Microsoft-peering](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Als u openbare peering en momenteel zijn IP-netwerkregels voor openbare IP-adressen die worden gebruikt voor toegang tot [Azure Storage](../storage/common/storage-network-security.md) of [Azure SQL Database](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md), moet u ervoor zorgen dat de NAT IP-adresgroep geconfigureerd met Microsoft is-peering opgenomen in de lijst met openbare IP-adressen voor de Azure-opslagaccount of Azure SQL-account.

* Als u wilt verplaatsen naar Microsoft-peering zonder uitvaltijd, gebruik de stappen in dit artikel in de volgorde waarin ze worden weergegeven.

## <a name="create"></a>1. Microsoft-peering maken

Als Microsoft-peering niet is gemaakt, kunt u een van de volgende artikelen te maken met het Microsoft-peering gebruiken. Als uw connectivity provider biedt beheerde layer 3-services, kunt u de connectiviteitsprovider om in te schakelen van Microsoft-peering voor uw circuit vragen.

  * [Microsoft-peering maken met Azure portal](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Microsoft-peering maken met Azure Powershell](expressroute-howto-routing-arm.md#msft)
  * [Microsoft-peering maken met Azure CLI](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Valideren van Microsoft-peering is ingeschakeld

Controleer of de Microsoft-peering is ingeschakeld en het aangekondigde openbare voorvoegsels zijn in de geconfigureerde status.

  * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure-CLI](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Configureren en een routefilter koppelen aan het circuit

Standaard kan nieuwe Microsoft-peerings niet alle voorvoegsels adverteren totdat een routefilter is gekoppeld aan het circuit. Wanneer u een routefilterregel maakt, kunt u de lijst met service-community's voor Azure-regio's die u gebruiken voor Azure PaaS-services, wilt zoals wordt weergegeven in de volgende schermafbeelding:

![Openbare peering samenvoegen](./media/how-to-move-peering/public.png)

Configureer routefilters met behulp van een van de volgende artikelen:

  * [Routefilters voor Microsoft-peering met behulp van Azure portal configureren](how-to-routefilter-portal.md)
  * [Routefilters voor Microsoft-peering met behulp van Azure PowerShell configureren](how-to-routefilter-powershell.md)
  * [Routefilters voor Microsoft-peering met behulp van Azure CLI configureren](how-to-routefilter-cli.md)

## <a name="delete"></a>4. De openbare peering verwijderen

Nadat u hebt gecontroleerd dat de Microsoft-peering is geconfigureerd en de voorvoegsels die u wilt gebruiken voor het correct op de Microsoft-peering worden geadverteerd, kunt u vervolgens de openbare peering verwijderen. Als u wilt de openbare peering verwijderen, gebruikt u een van de volgende artikelen:

  * [Openbare Azure-peering met behulp van Azure portal verwijderen](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Verwijderen van de openbare Azure-peering met behulp van Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)
  * [Verwijderen van de openbare Azure-peering met behulp van CLI](howto-routing-cli.md#deletepublic)
  
## <a name="view"></a>5. Weergave-peerings
  
U ziet een lijst met alle ExpressRoute-circuits en -peerings in Azure portal. Zie voor meer informatie, [details van de weergave-Microsoft-peering](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
