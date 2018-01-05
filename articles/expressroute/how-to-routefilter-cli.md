---
title: 'Routefilters voor Azure ExpressRoute Microsoft-peering configureren: CLI | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u routefilters configureren voor Microsoft-Peering met Azure CLI
documentationcenter: na
services: expressroute
author: anzaman
manager: ganesr
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: anzaman
ms.openlocfilehash: a85a68393f3dc946db651791de9efff0694f9989
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-route-filters-for-microsoft-peering-azure-cli"></a>Routefilters voor Microsoft-peering configureren: Azure CLI

> [!div class="op_single_selector"]
> * [Azure-portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure-CLI](how-to-routefilter-cli.md)
> 

Routefilters zijn een manier om te gebruiken een subset van de ondersteunde services via Microsoft-peering. De stappen in dit artikel te configureren en beheren van routefilters voor ExpressRoute-circuits.

Dynamics 365-services en Office 365-services zoals Exchange Online, SharePoint Online en Skype voor bedrijven, zijn toegankelijk via de Microsoft-peering. Wanneer Microsoft-peering in een ExpressRoute-circuit is geconfigureerd, worden alle voorvoegsels die betrekking hebben op deze services worden geadverteerd via de BGP-sessies worden tot stand gebracht. Een BGP-communitywaarde is gekoppeld aan elke voorvoegsel van de service die wordt aangeboden via het voorvoegsel. Zie voor een lijst van de BGP-Communitywaarden en deze worden toegewezen aan services [BGP-community's](expressroute-routing.md#bgp).

Als u de verbinding met alle services vereist, wordt een groot aantal voorvoegsels worden geadverteerd via BGP. Dit verhoogt de aanzienlijk de grootte van de routetabellen onderhouden door routers in uw netwerk. Als u van plan bent te gebruiken alleen een subset van services die worden aangeboden via Microsoft-peering, kunt u de grootte van uw routetabellen op twee manieren verkleinen. U kunt:

* Filter ongewenste voorvoegsels door routefilters zijn toegepast op de BGP-community's. Dit is een standaardprocedure voor netwerken en meestal wordt gebruikt binnen meerdere netwerken.

* Routefilters definiëren en toegepast op uw ExpressRoute-circuit. Een routefilter is een nieuwe resource waarmee u de lijst met services die u van plan bent te gebruiken via Microsoft-peering te selecteren. ExpressRoute-routers worden alleen de lijst met voorvoegsels die deel uitmaken van de services die zijn geïdentificeerd in het routefilter verzenden.

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

* U moet een actief ExpressRoute-circuit met Microsoft-peering ingerichte hebben. De volgende instructies kunt u deze taken:
  * [Maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit ingeschakeld door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.
  * [Microsoft-peering maken](howto-routing-cli.md) als u de BGP-sessie rechtstreeks beheren. Of vraag uw connectiviteitsprovider Microsoft-peering voor uw circuit inrichten.

* U moet maken en configureren van een routefilter.
  * Identificeer de services die u met gebruiken via Microsoft-peering
  * De lijst met BGP-Communitywaarden die zijn gekoppeld aan de services te identificeren
  * Maak een regel om toe te staan de Voorvoegsellijst die overeenkomen met de waarden van BGP-community

* U moet de routefilter koppelen aan ExpressRoute-circuit.

## <a name="before-you-begin"></a>Voordat u begint

Installeer eerst de meest recente versie van de CLI-opdrachten (2.0 of hoger). Zie [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli) en [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.

* Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](howto-circuit-cli.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.

* U hebt een actieve Microsoft-peering. Volg de instructies op [maken en wijzigen van peeringconfiguratie](howto-routing-cli.md)

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Aanmelden bij uw Azure-account en uw abonnement te selecteren

Om te beginnen met uw configuratie, moet u zich aanmelden bij uw Azure-account. Met de volgende voorbeelden kunt u verbinding kunt maken:

```azurecli
az login
```

Controleer de abonnementen voor het account.

```azurecli
az account list
```

Selecteer het abonnement waarvoor u wilt maken van een ExpressRoute-circuit.

```azurecli
az account set --subscription "<subscription ID>"
```

## <a name="prefixes"></a>Stap 1: Haal een lijst met voorvoegsels en BGP-Communitywaarden

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Een lijst met waarden van BGP-community

Gebruik de volgende cmdlet om de lijst met BGP-Communitywaarden die zijn gekoppeld aan services toegankelijk zijn via Microsoft-peering en de lijst met voorvoegsels die zijn gekoppeld:

```azurecli
az network route-filter rule list-service-communities
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Maak een lijst van de waarden die u wilt gebruiken

Maak een lijst van BGP-Communitywaarden die u wilt gebruiken in het routefilter. De BGP-communitywaarde voor Dynamics 365-services is een voorbeeld: 12076:5040.

## <a name="filter"></a>Stap 2: Maak een routefilter en een filterregel

Een routefilter kan slechts één regel, en de regel moet van het type 'Toestaan'. Deze regel kan een lijst met BGP-Communitywaarden die zijn gekoppeld hebben.

### <a name="1-create-a-route-filter"></a>1. Maken van een routefilter

Maak eerst het routefilter. De opdracht 'az netwerk route-filter maken' maakt alleen een bron routeren filter. Nadat u de resource hebt gemaakt, moet u vervolgens een regel maken en koppelen aan het object van het filter route. Voer de volgende opdracht om de bron van een route filter te maken:

```azurecli
az network route-filter create -n MyRouteFilter -g MyResourceGroup
```

### <a name="2-create-a-filter-rule"></a>2. Een filterregel maken

Voer de volgende opdracht om een nieuwe regel te maken:
 
```azurecli
az network route-filter rule create --filter-name MyRouteFilter -n CRM --communities 12076:5040 --access Allow -g MyResourceGroup
```

## <a name="attach"></a>Stap 3: De routefilter koppelen aan een ExpressRoute-circuit

Voer de volgende opdracht de routefilter koppelen aan het ExpressRoute-circuit:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --route-filter MyRouteFilter
```

## <a name="tasks"></a>Algemene taken

### <a name="getproperties"></a>Ophalen van de eigenschappen van een routefilter

Als u de eigenschappen van een routefilter, gebruikt u de volgende opdracht:

```azurecli
az network route-filter show -g ExpressRouteResourceGroupName --name MyRouteFilter 
```

### <a name="updateproperties"></a>De eigenschappen van een routefilter bijwerken

Als de routefilter is al gekoppeld aan een circuit, updates aan de lijst met BGP-community automatisch wijzigingen worden doorgegeven juiste voorvoegsel aankondiging via de BGP-sessies. U kunt de BGP-community-lijst van de routefilter met de volgende opdracht bijwerken:

```azurecli
az network route-filter rule update --filter-name MyRouteFilter -n CRM -g ExpressRouteResourceGroupName --add communities '12076:5040' --add communities '12076:5010'
```

### <a name="detach"></a>Ontkoppelen van een routefilter van een ExpressRoute-circuit

Zodra een routefilter wordt losgekoppeld van het ExpressRoute-circuit, worden er geen voorvoegsels zijn geadverteerd via de BGP-sessie. U kunt een routefilter van een ExpressRoute-circuit met de volgende opdracht loskoppelen:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --remove routeFilter
```

### <a name="delete"></a>Een routefilter verwijderen

U kunt een routefilter alleen verwijderen als deze niet is gekoppeld aan elk circuit. Zorg ervoor dat het routefilter is niet gekoppeld aan elk circuit voordat u probeert te verwijderen. U kunt een routefilter met de volgende opdracht verwijderen:

```azurecli
az network route-filter delete -n MyRouteFilter -g MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
