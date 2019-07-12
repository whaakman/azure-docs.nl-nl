---
title: Verplaatsen van een openbare peering aan Microsoft-peering - Azure ExpressRoute | Microsoft Docs
description: Dit artikel laat u de stappen voor het verplaatsen van uw openbare peering aan Microsoft-peering voor ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 5581e2a5c2fe2ee5e154870f7ffc2ab1c3c0f3b4
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592146"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Verplaatsen van een openbare peering naar het Microsoft-peering

Dit artikel helpt u de configuratie van een openbare peering naar Microsoft-peering zonder uitvaltijd. ExpressRoute biedt ondersteuning voor het gebruik van Microsoft-peering met routefilters voor Azure PaaS-services, zoals Azure Storage en Azure SQL Database. U hebt nu maar één routeringsdomein nodig voor toegang tot Microsoft PaaS- en SaaS-services. U kunt routefilters gebruiken om de PaaS-servicevoorvoegsels selectief te adverteren voor Azure-regio's die u wilt verbruiken.

Openbare Azure-peering heeft 1 NAT IP-adres dat is gekoppeld aan elke BGP-sessie. Microsoft-peering, kunt u uw eigen NAT-toewijzingen configureren, evenals routefilters voor advertenties selectief voorvoegsel gebruiken. Openbare Peering is een Unidirectioneel service met behulp van welke connectiviteit wordt altijd gestart vanuit uw WAN met Microsoft Azure-services. Microsoft Azure-services worden niet verbindingen in uw netwerk via de deze routeringsdomein kunnen initiëren.

Zodra de openbare peering is ingeschakeld, kunt u verbinding met alle Azure-services. We kunt niet u selectief kiezen services waarvoor we routes te adverteren. Terwijl Microsoft-peering wordt een bidirectionele connectiviteit waar de verbinding van Microsoft Azure-service, samen met uw WAN kan worden gestart. Zie voor meer informatie over routering van domeinen en peering [ExpressRoute-circuits en Routeringsdomeinen](expressroute-circuit-peerings.md).

## <a name="before"></a>Voordat u begint

Voor verbinding met Microsoft-peering, moet u instellen en beheren van NAT bevinden. Uw connectiviteitsprovider kan instellen en beheren van de NAT als een beheerde service. Als u van plan bent voor toegang tot het Azure PaaS en SaaS van de Azure-services op Microsoft-peering, is het belangrijk om de grootte van de NAT IP-adresgroep correct. Zie voor meer informatie over NAT voor ExpressRoute, de [NAT-vereisten voor Microsoft-peering](expressroute-nat.md#nat-requirements-for-microsoft-peering). Wanneer u verbinding via Azure ExpressRoute(Microsoft peering) aan Microsoft maakt, hebt u meerdere koppelingen naar Microsoft. De ene koppeling is uw bestaande internetverbinding en de andere gaat via ExpressRoute. Bepaald verkeer naar Microsoft kan via internet gaan maar kan terugkeren via ExpressRoute of andersom.

![Tweerichtingsconnectiviteit](./media/how-to-move-peering/bidirectional-connectivity.jpg)

> [!Warning]
> De NAT IP-adresgroep die wordt geadverteerd aan Microsoft mag niet worden geadverteerd op internet. Dit verbreekt de connectiviteit met andere Microsoft-services.

Raadpleeg [asymmetrische routering met meerdere netwerkpaden](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing) voor onder voorbehoud van asymmetrische routering voor het configureren van Microsoft-peering.

* Als u openbare peering en momenteel zijn IP-netwerkregels voor openbare IP-adressen die worden gebruikt voor toegang tot [Azure Storage](../storage/common/storage-network-security.md) of [Azure SQL Database](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md), moet u ervoor zorgen dat de NAT IP-adresgroep geconfigureerd met Microsoft is-peering opgenomen in de lijst met openbare IP-adressen voor de Azure-opslagaccount of Azure SQL-account.<br>
* Als u wilt verplaatsen naar Microsoft-peering zonder uitvaltijd, gebruik de stappen in dit artikel in de volgorde waarin ze worden weergegeven.

## <a name="create"></a>1. Microsoft-peering maken

Als Microsoft-peering niet is gemaakt, kunt u een van de volgende artikelen te maken met het Microsoft-peering gebruiken. Als uw connectivity provider biedt beheerde layer 3-services, kunt u de connectiviteitsprovider om in te schakelen van Microsoft-peering voor uw circuit vragen.

Als de laag-3 wordt beheerd door u is de volgende informatie is vereist voordat u doorgaat:

* Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.<br>
* Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.<br>
* Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt. Voor zowel de primaire en secundaire koppelingen moet u de dezelfde VLAN-ID.<br>
* AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.<br>
* Geadverteerde voorvoegsels: U kunt een lijst van alle voorvoegsels die u van plan bent om te adverteren via de BGP-sessie moet opgeven. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. Als u van plan bent om een set voorvoegsels te verzenden, kunt u een door komma's gescheiden lijst verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.<br>
* Naam van Routeringsregister: U kunt het RIR / IRR op basis waarvan het AS-nummer en de voorvoegsels zijn geregistreerd.

* **Optionele** -klant-ASN: Als u voorvoegsels adverteert die niet zijn geregistreerd op de AS-nummer peering, kunt u het AS-nummer waaraan ze zijn geregistreerd.<br>
* **Optionele** -een MD5-hash, als u ervoor kiest een te gebruiken.

Gedetailleerde instructies voor het inschakelen van de Microsoft-peering kunnen worden gevonden in de volgende artikelen:

* [Microsoft-peering maken met Azure portal](expressroute-howto-routing-portal-resource-manager.md#msft)<br>
* [Microsoft-peering maken met Azure Powershell](expressroute-howto-routing-arm.md#msft)<br>
* [Microsoft-peering maken met Azure CLI](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Valideren van Microsoft-peering is ingeschakeld

Controleer of de Microsoft-peering is ingeschakeld en het aangekondigde openbare voorvoegsels zijn in de geconfigureerde status.

* [Azure Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)<br>
* [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)<br>
* [Azure-CLI](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Configureren en een routefilter koppelen aan het circuit

Standaard nieuwe Microsoft-peering kan niet alle voorvoegsels adverteren totdat een routefilter is gekoppeld aan het circuit. Wanneer u een routefilterregel maakt, kunt u de lijst met service-community's voor Azure-regio's die u wilt gebruiken voor Azure PaaS-services. Dit biedt u de flexibiliteit om te filteren van de routes aan de hand van uw behoeften, zoals wordt weergegeven in de volgende schermafbeelding:

![Openbare peering samenvoegen](./media/how-to-move-peering/routefilter.jpg)

Configureer routefilters met behulp van een van de volgende artikelen:

* [Routefilters voor Microsoft-peering met behulp van Azure portal configureren](how-to-routefilter-portal.md)<br>
* [Routefilters voor Microsoft-peering met behulp van Azure PowerShell configureren](how-to-routefilter-powershell.md)<br>
* [Routefilters voor Microsoft-peering met behulp van Azure CLI configureren](how-to-routefilter-cli.md)

## <a name="delete"></a>4. De openbare peering verwijderen

Nadat u hebt gecontroleerd dat de Microsoft-peering is geconfigureerd en de voorvoegsels die u wilt gebruiken voor het correct op de Microsoft-peering worden geadverteerd, kunt u vervolgens de openbare peering verwijderen. Als u wilt de openbare peering verwijderen, gebruikt u een van de volgende artikelen:

* [Openbare Azure-peering met behulp van Azure portal verwijderen](expressroute-howto-routing-portal-resource-manager.md#deletepublic)<br>
* [Verwijderen van de openbare Azure-peering met behulp van Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)<br>
* [Verwijderen van de openbare Azure-peering met behulp van CLI](howto-routing-cli.md#deletepublic)
  
## <a name="view"></a>5. Weergave-peerings
  
U ziet een lijst met alle ExpressRoute-circuits en -peerings in Azure portal. Zie voor meer informatie, [details van de weergave-Microsoft-peering](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
