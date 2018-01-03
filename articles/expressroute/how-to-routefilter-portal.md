---
title: 'Routefilters voor Azure ExpressRoute Microsoft-peering configureren: Portal | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u routefilters configureren voor Microsoft-Peering met de Azure portal
documentationcenter: na
services: expressroute
author: ganesr
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: ganesr
ms.openlocfilehash: 0129a48e43e90001785a5977d4b0d1fd9fa9fd7d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-route-filters-for-microsoft-peering-azure-portal"></a>Routefilters voor Microsoft-peering configureren: Azure-portal
> [!div class="op_single_selector"]
> * [Azure-portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure-CLI](how-to-routefilter-cli.md)
> 

Routefilters zijn een manier om te gebruiken een subset van de ondersteunde services via Microsoft-peering. De stappen in dit artikel te configureren en beheren van routefilters voor ExpressRoute-circuits.

Dynamics 365-services en Office 365-services zoals Exchange Online, SharePoint Online en Skype voor bedrijven en Azure-services zoals opslag en SQL-database zijn toegankelijk via Microsoft-peering. Wanneer Microsoft-peering in een ExpressRoute-circuit is geconfigureerd, worden alle voorvoegsels die betrekking hebben op deze services worden geadverteerd via de BGP-sessies worden tot stand gebracht. Een BGP-communitywaarde is gekoppeld aan elke voorvoegsel van de service die wordt aangeboden via het voorvoegsel. Zie voor een lijst van de BGP-Communitywaarden en deze worden toegewezen aan services [BGP-community's](expressroute-routing.md#bgp).

Als u de verbinding met alle services vereist, wordt een groot aantal voorvoegsels worden geadverteerd via BGP. Dit verhoogt de aanzienlijk de grootte van de routetabellen onderhouden door routers in uw netwerk. Als u van plan bent te gebruiken alleen een subset van services die worden aangeboden via Microsoft-peering, kunt u de grootte van uw routetabellen op twee manieren verkleinen. U kunt:

- Filter ongewenste voorvoegsels door routefilters zijn toegepast op de BGP-community's. Dit is een standaardprocedure voor netwerken en meestal wordt gebruikt binnen meerdere netwerken.

- Routefilters definiëren en toegepast op uw ExpressRoute-circuit. Een routefilter is een nieuwe resource waarmee u de lijst met services die u van plan bent te gebruiken via Microsoft-peering te selecteren. ExpressRoute-routers worden alleen de lijst met voorvoegsels die deel uitmaken van de services die zijn geïdentificeerd in het routefilter verzenden.

### <a name="about"></a>Over routefilters

Wanneer Microsoft-peering is geconfigureerd op uw ExpressRoute-circuit, is de Microsoft-randrouters tot stand brengen een combinatie van BGP-sessies met routers van de rand (jouw e-mailadres of uw connectiviteitsprovider). Er is geen routes worden geadverteerd naar uw netwerk. U moet een routefilter koppelen zodat route-advertisements met uw netwerk.

Een routefilter kunt u identificeren services die u gebruiken wilt via Microsoft-peering voor uw ExpressRoute-circuit. Het is in wezen een witte lijst van alle waarden van de BGP-community. Als een bron routeren filter wordt gedefinieerd en gekoppeld aan een ExpressRoute-circuit, worden alle voorvoegsels die zijn toegewezen aan de BGP-Communitywaarden worden geadverteerd naar uw netwerk.

Als u routefilters met Office 365-services op deze te koppelen, moet u de autorisatie voor Office 365-services via ExpressRoute gebruiken hebben. Als u bent niet gemachtigd om te gebruiken van Office 365-services via ExpressRoute, mislukt de bewerking routefilters koppelen. Zie voor meer informatie over het autorisatieproces [Azure ExpressRoute voor Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). De verbinding met Dynamics 365 services is niet vereist voor eventuele toestemming.

> [!IMPORTANT]
> Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd, voordat u 1 augustus 2017 hebben alle service voorvoegsels die worden geadverteerd via Microsoft-peering, zelfs als routefilters zijn niet gedefinieerd. Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd op of na 1 augustus 2017 geen geen voorvoegsels aangekondigd totdat een routefilter is gekoppeld aan het circuit.
> 
> 

### <a name="workflow"></a>Werkstroom

Om te kunnen maken met services via Microsoft-peering, moet u de volgende configuratiestappen uit:

- U moet een actief ExpressRoute-circuit met Microsoft-peering ingerichte hebben. De volgende instructies kunt u deze taken:
  - [Maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit ingeschakeld door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.
  - [Microsoft-peering maken](expressroute-howto-routing-portal-resource-manager.md) als u de BGP-sessie rechtstreeks beheren. Of vraag uw connectiviteitsprovider Microsoft-peering voor uw circuit inrichten.

-  U moet maken en configureren van een routefilter.
    - Identificeer de services die u met gebruiken via Microsoft-peering
    - De lijst met BGP-Communitywaarden die zijn gekoppeld aan de services te identificeren
    - Maak een regel om toe te staan de Voorvoegsellijst die overeenkomen met de waarden van BGP-community

-  U moet de routefilter koppelen aan ExpressRoute-circuit.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de configuratie, zorg er dan voor dat u voldoet aan de volgende criteria:

 - Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

 - U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.

 - U hebt een actieve Microsoft-peering. Volg de instructies op [maken en wijzigen van peeringconfiguratie](expressroute-howto-routing-portal-resource-manager.md)


## <a name="prefixes"></a>Stap 1: Haal een lijst met voorvoegsels en BGP-Communitywaarden

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Een lijst met waarden van BGP-community

BGP-Communitywaarden die zijn gekoppeld aan services toegankelijk zijn via Microsoft-peering is beschikbaar in de [routeringsvereisten voor ExpressRoute](expressroute-routing.md) pagina.

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Maak een lijst van de waarden die u wilt gebruiken

Maak een lijst van BGP-Communitywaarden die u wilt gebruiken in het routefilter. De BGP-communitywaarde voor Dynamics 365-services is een voorbeeld: 12076:5040.

## <a name="filter"></a>Stap 2: Maak een routefilter en een filterregel

Een routefilter kan slechts één regel, en de regel moet van het type 'Toestaan'. Deze regel kan een lijst met BGP-Communitywaarden die zijn gekoppeld hebben.

### <a name="1-create-a-route-filter"></a>1. Maken van een routefilter
U kunt een routefilter maken door de optie voor het maken van een nieuwe resource. Klik op **nieuw** > **Networking** > **RouteFilter**, zoals wordt weergegeven in de volgende afbeelding:

![Maken van een routefilter](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

U moet het routefilter plaatsen in een resourcegroep. 

![Maken van een routefilter](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Een filterregel maken

U kunt toevoegen en regels bijwerken door het selecteren van het tabblad van de regel beheren voor uw routefilter.

![Maken van een routefilter](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


De services die u wilt verbinding maken met in de vervolgkeuzelijst en opslaan van de regel wanneer u klaar bent, kunt u selecteren.

![Maken van een routefilter](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <a name="attach"></a>Stap 3: De routefilter koppelen aan een ExpressRoute-circuit

U kunt de routefilter koppelen aan een circuit door met de knop 'Circuit toevoegen' en het ExpressRoute-circuit in de vervolgkeuzelijst.

![Maken van een routefilter](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

Als de connectiviteitsprovider configureert voor uw ExpressRoute-peering circuit vernieuwt u het circuit vanuit de blade van ExpressRoute-circuit voordat u de knop 'Circuit toevoegen' selecteren.

![Maken van een routefilter](.\media\how-to-routefilter-portal\RefreshExpressRouteCircuit.png)

## <a name="tasks"></a>Algemene taken

### <a name="getproperties"></a>Ophalen van de eigenschappen van een routefilter

U kunt de eigenschappen van een routefilter weergeven bij het openen van de resource in de portal.

![Maken van een routefilter](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


### <a name="updateproperties"></a>De eigenschappen van een routefilter bijwerken

U kunt de lijst met BGP-Communitywaarden die is gekoppeld aan een circuit door het selecteren van de knop 'Beheren regel' bijwerken.


![Maken van een routefilter](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Maken van een routefilter](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


### <a name="detach"></a>Ontkoppelen van een routefilter van een ExpressRoute-circuit

Als u wilt loskoppelen van een met het routefilter voor de-circuit, klik met de rechtermuisknop op het circuit en klik op 'losgekoppeld'.

![Maken van een routefilter](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


### <a name="delete"></a>Een routefilter verwijderen

U kunt een routefilter verwijderen door het selecteren van de knop verwijderen. 

![Maken van een routefilter](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).