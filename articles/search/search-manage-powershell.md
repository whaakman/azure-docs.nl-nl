---
title: PowerShell-scripts met Az.Search module - Azure Search
description: Maken en een Azure Search-service configureren met PowerShell. U kunt een service omhoog of omlaag schalen, beheren-beheerder en query api-sleutels en systeemgegevens van de query.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: heidist
ms.openlocfilehash: 8f07468ccff4431e1afdf66aedc72599ddc0c25b
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620594"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Uw Azure Search-service met PowerShell beheren
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

U kunt PowerShell-cmdlets en scripts uitvoeren in Windows, Linux, of in [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) maken en configureren van Azure Search. De **Az.Search** module breidt Azure PowerShell] met volledige pariteit de [Azure Search Management REST API's](https://docs.microsoft.com/rest/api/searchmanagement). Met Azure PowerShell en **Az.Search**, kunt u de volgende taken uitvoeren:

> [!div class="checklist"]
> * [Overzicht van de search-services in uw abonnement](#list-search-services)
> * [Informatie ophalen over een specifieke search-service](#get-search-service-information)
> * [Maken of verwijderen van een service](#create-or-delete-a-service)
> * [Admin API-sleutels opnieuw genereren](#regenerate-admin-keys)
> * [Maken of verwijderen van de query api-sleutels](#create-or-delete-query-keys)
> * [Een service schaalt door vergroten of verkleinen van replica's en partities](#scale-replicas-and-partitions)

PowerShell kan niet worden gebruikt om de naam, regio of laag van uw service te wijzigen. Toegewezen resources worden toegewezen als een service wordt gemaakt. Wijzigen van de onderliggende hardware (type locatie of het knooppunt), moet een nieuwe service. Er zijn geen hulpprogramma's of API's voor het overbrengen van inhoud van de ene service naar een andere. Alle inhoudsbeheer is via [REST](https://docs.microsoft.com/rest/api/searchservice/) of [.NET](https://docs.microsoft.com/dotnet/api/?term=microsoft.azure.search) API's, en als u verplaatsen van indexen wilt, moet u opnieuw maken en deze op een nieuwe service opnieuw te laden. 

Er zijn geen speciale PowerShell-opdrachten voor het beheer van inhoud, kunt u PowerShell-script dat aanroepen van REST- of .NET voor het maken en indexen laden kunt schrijven. De **Az.Search** module zelf zorgt niet voor deze bewerkingen.

Andere taken niet ondersteund via PowerShell of een andere API (portal-alleen) zijn onder andere:
+ [Een cognitive services-resource koppelen](cognitive-search-attach-cognitive-services.md) voor [AI verrijkt indexeren](cognitive-search-concept-intro.md). Een cognitive service is gekoppeld aan een set vaardigheden, niet een abonnement of -service.
+ [Invoegtoepassing bewakingsoplossingen](search-monitor-usage.md#add-on-monitoring-solutions) of [zoekverkeer](search-traffic-analytics.md) gebruikt voor het bewaken van Azure Search.

<a name="check-versions-and-load"></a>

## <a name="check-versions-and-load-modules"></a>Versies controleren en te laden modules

De voorbeelden in dit artikel zijn interactief en verhoogde machtigingen zijn vereist. Azure PowerShell (de **Az** module) moet worden geïnstalleerd. Zie voor meer informatie, [Azure PowerShell installeren](/powershell/azure/overview).

### <a name="powershell-version-check-51-or-later"></a>Controle van PowerShell-versie (5.1 of hoger)

Lokale PowerShell moet 5.1 of hoger, op een ondersteund besturingssysteem.

```azurepowershell-interactive
$PSVersionTable.PSVersion
```

### <a name="load-azure-powershell"></a>Laden van Azure PowerShell

Als u niet zeker weet of **Az** is geïnstalleerd, wordt de volgende opdracht uitvoeren als verificatiestap. 

```azurepowershell-interactive
Get-InstalledModule -Name Az
```

Sommige systemen kunnen niet automatisch laden modules. Als u een fout optreedt op de vorige opdracht, probeert het laden van de module en als dat mislukt, gaat u terug naar de installatie-instructies om te zien als u een stap hebt gemist.

```azurepowershell-interactive
Import-Module -Name Az
```

### <a name="connect-to-azure-with-a-browser-sign-in-token"></a>Verbinding maken met Azure met een browser aanmelden token

Verbinding maken met een abonnement in PowerShell kunt u uw referenties voor portal aanmelden. U kunt ook [niet-interactief verifiëren met een service-principal](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

```azurepowershell-interactive
Connect-AzAccount
```

Als u meerdere Azure-abonnementen bevatten, stelt u uw Azure-abonnement. Een lijst van uw huidige abonnementen wilt bekijken, moet u deze opdracht uitvoeren.

```azurepowershell-interactive
Get-AzSubscription | sort SubscriptionName | Select SubscriptionName
```

Als u het abonnement, moet u de volgende opdracht uitvoeren. In het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName ContosoSubscription
```

<a name="list-search-services"></a>

## <a name="list-all-azure-search-services-in-your-subscription"></a>Lijst van alle Azure Search-services in uw abonnement

De volgende opdrachten zijn van [ **Az.Resources**](https://docs.microsoft.com/powershell/module/az.resources/?view=azps-1.4.0#resources), retourneren informatie over bestaande resources en services in uw abonnement ingericht. Als u niet hoeveel search-services al zijn gemaakt weet, retourneert deze opdrachten die informatie, zodat u een reis naar de portal.

De eerste opdracht retourneert alle search-services.

```azurepowershell-interactive
Get-AzResource -ResourceType Microsoft.Search/searchServices | ft
```

In de lijst met services, informatie over een specifieke resource te retourneren.

```azurepowershell-interactive
Get-AzResource -ResourceName <service-name>
```

Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : westus
ResourceId        : /subscriptions/<alpha-numeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="import-azsearch"></a>Az.Search importeren

Opdrachten vanaf [ **Az.Search** ](https://docs.microsoft.com/powershell/module/az.search/?view=azps-1.4.0#search) zijn niet beschikbaar totdat u de module niet laden.

```azurepowershell-interactive
Install-Module -Name Az.Search
```

### <a name="list-all-azsearch-commands"></a>Lijst met alle Az.Search opdrachten

Als verificatiestap, een lijst met opdrachten die zijn opgegeven in de module te retourneren.

```azurepowershell-interactive
Get-Command -Module Az.Search
```

Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

```
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-AzSearchAdminKeyPair            0.7.1      Az.Search
Cmdlet          Get-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          Get-AzSearchService                 0.7.1      Az.Search
Cmdlet          New-AzSearchAdminKey                0.7.1      Az.Search
Cmdlet          New-AzSearchQueryKey                0.7.1      Az.Search
Cmdlet          New-AzSearchService                 0.7.1      Az.Search
Cmdlet          Remove-AzSearchQueryKey             0.7.1      Az.Search
Cmdlet          Remove-AzSearchService              0.7.1      Az.Search
Cmdlet          Set-AzSearchService                 0.7.1      Az.Search
```

## <a name="get-search-service-information"></a>Informatie over de zoekservice ophalen

Na **Az.Search** is geïmporteerd en u weet dat de resourcegroep waarin uw search-service uitvoeren met [Get-AzSearchService](https://docs.microsoft.com/powershell/module/az.search/get-azsearchservice?view=azps-1.4.0) om terug te keren van de definitie van de service, zoals naam, regio, laag en replica en partitie wordt geteld.

```azurepowershell-interactive
Get-AzSearchService -ResourceGroupName <resource-group-name>
```

Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

```
Name              : my-demo-searchapp
ResourceGroupName : demo-westus
ResourceType      : Microsoft.Search/searchServices
Location          : West US
Sku               : Standard
ReplicaCount      : 1
PartitionCount    : 1
HostingMode       : Default
ResourceId        : /subscriptions/<alphanumeric-subscription-ID>/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```

## <a name="create-or-delete-a-service"></a>Maken of verwijderen van een service

[**Nieuwe AzSearchService** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) wordt gebruikt voor het [Maak een nieuwe zoekservice](search-create-service-portal.md).

```azurepowershell-interactive
New-AzSearchService -ResourceGroupName "demo-westus" -Name "my-demo-searchapp" -Sku "Standard" -Location "West US" -PartitionCount 3 -ReplicaCount 3
``` 
Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Id                : /subscriptions/<alphanumeric-subscription-ID>/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 3
PartitionCount    : 3
HostingMode       : Default
Tags
```     

## <a name="regenerate-admin-keys"></a>Beheerder sleutels opnieuw genereren

[**Nieuwe AzSearchAdminKey** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchadminkey?view=azps-1.4.0) wordt gebruikt voor het beheer worden meegenomen [API-sleutels](search-security-api-keys.md). Twee beheersleutels worden gemaakt met elke service voor geverifieerde toegang. Sleutels zijn vereist voor elke aanvraag. Beide beheersleutels zijn functioneel equivalent, het verlenen van volledige toegang voor schrijven naar een service voor zoeken met de mogelijkheid om alle gegevens ophalen of maken en verwijderen van een object. Er bestaan twee sleutels zodat u kunt een opgetreden bij het vervangen van de andere. 

U kunt alleen opnieuw genereren een op een tijdstip, opgegeven als de `primary` of `secondary` sleutel. Service niet wordt onderbroken, vergeet niet om bij te werken van alle clientcode voor het gebruik van een secundaire sleutel bij het verlengen van de primaire sleutel. Vermijd de sleutels wijzigen terwijl de bewerkingen zijn tijdens de overdracht.

Als u had verwacht, als u sleutels opnieuw genereren zonder bij te werken clientcode, mislukken aanvragen met behulp van de oude sleutel. Alle nieuwe sleutels opnieuw genereren is niet permanent worden vergrendeld uw service en u kunt nog steeds toegang tot de service via de portal. Nadat u primaire en secundaire sleutel opnieuw genereert, kunt u clientcode voor het gebruik van de nieuwe sleutels bijwerken en bewerkingen dienovereenkomstig wordt hervat.

Waarden voor de API-sleutels worden gegenereerd door de service. U kunt een aangepaste sleutel voor Azure Search te gebruiken kan niet opgeven. Er is ook geen door de gebruiker gedefinieerde naam voor de admin API-sleutels. Verwijzingen naar de sleutel zijn tekenreeksen, ofwel vast `primary` of `secondary`. 

```azurepowershell-interactive
New-AzSearchAdminKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -KeyKind Primary
```

Resultaten zijn vergelijkbaar zijn met de volgende uitvoer. Beide sleutels worden geretourneerd, zelfs als u één voor één alleen wijzigen.

```
Primary                    Secondary
-------                    ---------
<alphanumeric-guid>        <alphanumeric-guid>  
```

## <a name="create-or-delete-query-keys"></a>Maken of verwijderen van querysleutels

[**Nieuwe AzSearchQueryKey** ](https://docs.microsoft.com/powershell/module/az.search/new-azsearchquerykey?view=azps-1.4.0) wordt gebruikt om query te maken [API-sleutels](search-security-api-keys.md) voor alleen-lezen toegang vanaf de client-apps naar een Azure Search-index. Querysleutels worden gebruikt om een specifieke index ten behoeve van het ophalen van zoekresultaten te verifiëren. Querysleutels Verleen niet alleen-lezen toegang tot andere items in de service, zoals een index, gegevensbron of indexeerfunctie.

U kunt een sleutel voor Azure Search te gebruiken kan niet opgeven. API-sleutels worden gegenereerd door de service.

```azurepowershell-interactive
New-AzSearchQueryKey -ResourceGroupName <resource-group-name> -ServiceName <search-service-name> -Name <query-key-name> 
```

## <a name="scale-replicas-and-partitions"></a>Schaal replica's en partities

[**Set-AzSearchService** ](https://docs.microsoft.com/powershell/module/az.search/set-azsearchservice?view=azps-1.4.0) wordt gebruikt voor het [vergroten of verkleinen van replica's en partities](search-capacity-planning.md) factureerbare resources in uw service aanpassen. Replica's of partities verhogen wordt toegevoegd aan uw factuur, die beide vaste en variabele kosten in rekening gebracht. Als u meer verwerkingskracht tijdelijke nodig hebt, kunt u de replica's en partities die voor het afhandelen van de werkbelasting verhogen. Het gebied bewaking in de overzichtspagina van de portal heeft tegels op de latentie van query, query's per seconde, en beperking, die aangeeft of de huidige capaciteit is voldoende.

Het kan even toevoegen aan of te verwijderen. Aanpassingen aan capaciteit optreden op de achtergrond, zodat bestaande workloads om door te gaan. Extra capaciteit wordt gebruikt voor binnenkomende aanvragen zodra deze gereed zijn, zonder aanvullende configuratie vereist is. 

Capaciteit verwijderen kan worden ervaren. Alle indexeren en indexeerfunctie taken voorafgaand aan de capaciteit vermindert stoppen wordt aanbevolen om te voorkomen dat de verwijderde aanvragen. Als dat niet haalbaar is, kunt u overwegen minder capaciteit incrementeel, één replica en partitie op een tijdstip, totdat de nieuwe doel-niveaus zijn bereikt.

Nadat u de opdracht hebt ingediend, is er geen manier om op te heffen halverwege via. U moet wachten totdat de opdracht is voltooid voordat de aantallen herzien.

```azurepowershell-interactive
Set-AzSearchService -ResourceGroupName <resource-group-name> -Name <search-service-name> -PartitionCount 6 -ReplicaCount 6
```

Resultaten zijn vergelijkbaar zijn met de volgende uitvoer.

```
ResourceGroupName : demo-westus
Name              : my-demo-searchapp
Location          : West US
Sku               : Standard
ReplicaCount      : 6
PartitionCount    : 6
HostingMode       : Default
Id                : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/demo-westus/providers/Microsoft.Search/searchServices/my-demo-searchapp
```


## <a name="next-steps"></a>Volgende stappen

Bouw een [index](search-what-is-an-index.md), [query uitvoeren in een index](search-query-overview.md) via de portal, REST-API's of de .NET SDK.

* [Een Azure Search-index maken in Azure portal](search-create-index-portal.md)
* [Instellen van een indexeerfunctie om gegevens te laden van andere services](search-indexer-overview.md)
* [Query uitvoeren op een Azure Search-index met behulp van Search explorer in Azure portal](search-explorer.md)
* [Azure Search in .NET gebruiken.](search-howto-dotnet-sdk.md)