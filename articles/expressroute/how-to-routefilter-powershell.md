---
title: 'Routefilters voor Azure ExpressRoute Microsoft-peering configureren: PowerShell | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u routefilters configureren voor Microsoft-Peering met behulp van PowerShell
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
ms.openlocfilehash: 9d953ea68e1e14ae12aa401af935d207f0747e8c
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="configure-route-filters-for-microsoft-peering-powershell"></a>Routefilters voor Microsoft-peering configureren: PowerShell
> [!div class="op_single_selector"]
> * [Azure-portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure-CLI](how-to-routefilter-cli.md)
> 

Routefilters zijn een manier om te gebruiken een subset van de ondersteunde services via Microsoft-peering. De stappen in dit artikel te configureren en beheren van routefilters voor ExpressRoute-circuits.

Dynamics 365-services en Office 365-services zoals Exchange Online, SharePoint Online en Skype voor bedrijven en Azure openbare-services, zoals opslag en SQL-database zijn toegankelijk via Microsoft-peering. Van openbare Azure-services kunnen worden geselecteerd op basis van per regio en per openbare service kan niet worden gedefinieerd. 

Wanneer Microsoft-peering is geconfigureerd op een ExpressRoute-circuit en een routefilter is gekoppeld, worden alle voorvoegsels die zijn geselecteerd voor deze services worden geadverteerd via de BGP-sessies worden tot stand gebracht. Een BGP-communitywaarde is gekoppeld aan elke voorvoegsel van de service die wordt aangeboden via het voorvoegsel. Zie voor een lijst van de BGP-Communitywaarden en deze worden toegewezen aan services [BGP-community's](expressroute-routing.md#bgp).

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
  - [Maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit ingeschakeld door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.
  - [Microsoft-peering maken](expressroute-circuit-peerings.md) als u de BGP-sessie rechtstreeks beheren. Of vraag uw connectiviteitsprovider Microsoft-peering voor uw circuit inrichten.

-  U moet maken en configureren van een routefilter.
    - Identificeer de services die u met gebruiken via Microsoft-peering
    - De lijst met BGP-Communitywaarden die zijn gekoppeld aan de services te identificeren
    - Maak een regel om toe te staan de Voorvoegsellijst die overeenkomen met de waarden van BGP-community

-  U moet de routefilter koppelen aan ExpressRoute-circuit.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de configuratie, zorg er dan voor dat u voldoet aan de volgende criteria:

 - Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor meer informatie.

  > [!NOTE]
  > Download de nieuwste versie van de PowerShell-galerie in plaats van met het installatieprogramma. Het installatieprogramma ondersteunt momenteel niet de vereiste cmdlets.
  > 

 - Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

 - U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Het ExpressRoute-circuit moet zich in een status ingericht en zijn ingeschakeld.

 - U hebt een actieve Microsoft-peering. Volg de instructies op [maken en wijzigen van peeringconfiguratie](expressroute-circuit-peerings.md)

### <a name="log-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Voordat u begint met deze configuratie, moet u zich aanmelden bij uw Azure-account. De cmdlet vraagt u om de aanmeldingsreferenties voor uw Azure-account. Na het aanmelden, worden de instellingen van uw account gedownload zodat ze beschikbaar zijn voor Azure PowerShell.

Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
```

Als u meerdere Azure-abonnementen hebt, controleert u de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

Geef het abonnement op dat u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="prefixes"></a>Stap 1: Haal een lijst met voorvoegsels en BGP-Communitywaarden

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Een lijst met waarden van BGP-community

Gebruik de volgende cmdlet om de lijst met BGP-Communitywaarden die zijn gekoppeld aan services toegankelijk zijn via Microsoft-peering en de lijst met voorvoegsels die zijn gekoppeld:

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Maak een lijst van de waarden die u wilt gebruiken

Maak een lijst van BGP-Communitywaarden die u wilt gebruiken in het routefilter. De BGP-communitywaarde voor Dynamics 365-services is een voorbeeld: 12076:5040.

## <a name="filter"></a>Stap 2: Maak een routefilter en een filterregel

Een routefilter kan slechts één regel, en de regel moet van het type 'Toestaan'. Deze regel kan een lijst met BGP-Communitywaarden die zijn gekoppeld hebben.

### <a name="1-create-a-route-filter"></a>1. Maken van een routefilter

Maak eerst het routefilter. De opdracht 'New-AzureRmRouteFilter' maakt alleen een bron routeren filter. Nadat u de resource hebt gemaakt, moet u vervolgens een regel maken en koppelen aan het object van het filter route. Voer de volgende opdracht om de bron van een route filter te maken:

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Een filterregel maken

U kunt een set van BGP-community's opgeven als een door komma's gescheiden lijst, zoals wordt weergegeven in het voorbeeld. Voer de volgende opdracht om een nieuwe regel te maken:
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-the-rule-to-the-route-filter"></a>3. De regel toevoegen aan het routefilter

Voer de volgende opdracht de filterregel toevoegen aan het routefilter:
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="attach"></a>Stap 3: De routefilter koppelen aan een ExpressRoute-circuit

Voer de volgende opdracht de routefilter koppelen aan het ExpressRoute-circuit, ervan uitgaande dat u hebt alleen Microsoft-peering:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="tasks"></a>Algemene taken

### <a name="getproperties"></a>Ophalen van de eigenschappen van een routefilter

Als u de eigenschappen van een routefilter, gebruikt u de volgende stappen uit:

1. Voer de volgende opdracht om op te halen van de bron routeren filter:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. De route filterregels ophalen voor de bron van de route-filter met de volgende opdracht:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

### <a name="updateproperties"></a>De eigenschappen van een routefilter bijwerken

Als de routefilter is al gekoppeld aan een circuit, updates aan de lijst met BGP-community automatisch wijzigingen worden doorgegeven juiste voorvoegsel aankondiging via de BGP-sessies. U kunt de BGP-community-lijst van de routefilter met de volgende opdracht bijwerken:

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

### <a name="detach"></a>Ontkoppelen van een routefilter van een ExpressRoute-circuit

Zodra een routefilter wordt losgekoppeld van het ExpressRoute-circuit, worden er geen voorvoegsels zijn geadverteerd via de BGP-sessie. U kunt een routefilter van een ExpressRoute-circuit met de volgende opdracht loskoppelen:
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="delete"></a>Een routefilter verwijderen

U kunt een routefilter alleen verwijderen als deze niet is gekoppeld aan elk circuit. Zorg ervoor dat het routefilter is niet gekoppeld aan elk circuit voordat u probeert te verwijderen. U kunt een routefilter met de volgende opdracht verwijderen:

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
