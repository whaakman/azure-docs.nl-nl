---
title: Azure Functions-schaal en hosting | Microsoft Docs
description: Informatie over het kiezen tussen het verbruiksabonnement voor Azure Functions en App Service-plan.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functies, verbruiksabonnement, app service-plan, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d2d171235a23d3e41fda6172efe29b3bb358f0e
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804175"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions-schaal en hosting

Azure Functions wordt uitgevoerd in twee verschillende modi: Verbruiksabonnement en Azure App Service-plan. Het verbruiksabonnement rekencapaciteit automatisch toegewezen wanneer uw code wordt uitgevoerd. Uw app is uitgeschaald wanneer dat nodig is om belasting te verwerken en omlaag worden geschaald wanneer de code wordt niet uitgevoerd. U hoeft te betalen voor niet-actieve virtuele machines of vooraf capaciteit reserveren.

> [!NOTE]  
> Verbruiksabonnement voor Linux is [nu in openbare Preview](https://azure.microsoft.com/updates/azure-functions-consumption-plan-for-linux-preview/).

Als u niet bekend bent met Azure Functions, raadpleegt u de [overzicht van Azure Functions](functions-overview.md).

Wanneer u een functie-app maakt, kiest u het hostingabonnement voor functies in de app. Een abonnement van een exemplaar van de *Azure Functions host* voert u de functies. Het type abonnement besturingselementen:

* Hoe hostexemplaren worden uitgeschaald.
* De resources die beschikbaar voor elke host zijn.

> [!IMPORTANT]
> U moet het type van het hostingabonnement tijdens het maken van de functie-app. U wijzigen deze later niet.

U kunt op een App Service-plan schalen tussen lagen verschillende hoeveelheid resources toewijzen. Op het plan verbruik verwerkt Azure Functions automatisch alle brontoewijzing. 

## <a name="consumption-plan"></a>Verbruiksabonnement

Wanneer u een verbruiksabonnement gebruikt, worden instanties van de Azure Functions-host dynamisch toegevoegd en verwijderd op basis van het aantal binnenkomende gebeurtenissen. Dit plan serverloze automatisch wordt geschaald en u betaalt voor computerresources alleen wanneer uw functies worden uitgevoerd. In een verbruiksabonnement verloopt een functie-uitvoering na een periode die is geconfigureerd.

> [!NOTE]
> De standaardtime-out voor functies in een verbruiksabonnement is 5 minuten. De waarde voor de functie-App tot een maximum van tien minuten kan worden verhoogd met het wijzigen van de eigenschap `functionTimeout` in de [host.json](functions-host-json.md#functiontimeout) projectbestand.

Facturering is gebaseerd op aantal uitvoeringen, uitvoeringstijd en geheugen dat wordt gebruikt. Facturering wordt samengevoegd voor alle functies in een functie-app. Zie voor meer informatie de [Pagina prijzen voor Azure Functions].

Het verbruiksabonnement is de standaardinstelling hostingabonnement en biedt de volgende voordelen:

* U betaalt alleen wanneer uw functies worden uitgevoerd.
* Automatisch opwaarts schalen, zelfs tijdens perioden van hoge laden.

## <a name="app-service-plan"></a>App Service-plan

In de toegewezen App Service-plan, uw functie-apps worden uitgevoerd via exclusieve virtuele machines op Basic, Standard, Premium en geïsoleerd SKU's, die is hetzelfde als andere apps in App Service. Toegewezen virtuele machines worden toegewezen aan uw functie-app, wat betekent dat de host functies kan worden [altijd wordt uitgevoerd](#always-on). App Service-plannen ondersteuning voor Linux.

Houd rekening met een App Service-plan in de volgende gevallen:

* U hebt bestaande, weinig gebruikte virtuele machines die al een andere App Service-exemplaren worden uitgevoerd.
* Uw functie-apps voortdurend of vrijwel continu uitvoeren. In dit geval mag een App Service-Plan rendabeler.
* U moet meer opties voor de CPU of geheugen dan op het abonnement Consumption wordt geleverd.
* Uw code moet langer zijn dan de maximale uitvoeringstijd toegestaan op het plan verbruik, dat maximaal tien minuten wordt uitgevoerd.
* Gewenste functies die alleen beschikbaar op een App Service-plan, zoals ondersteuning voor App Service-omgeving, VNET-VPN-connectiviteit en grotere VM-grootten zijn.
* U wilt uitvoeren van uw functie-app op Linux of u wilt mogelijk een aangepaste installatiekopie op voor het uitvoeren van uw functies.

Een virtuele machine worden losgekoppeld-kosten van het aantal uitvoeringen, uitvoeringstijd en geheugen dat wordt gebruikt. Als gevolg hiervan wordt niet betaalt u meer dan de kosten van het VM-exemplaar dat u toewijst. Zie voor meer informatie over de werking van de App Service-plan de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/overview-hosting-plans.md). 

U kunt handmatig opwaarts schalen door meer VM-exemplaren toe te voegen met een App Service-plan, of kunt u automatisch schalen inschakelen. Zie voor meer informatie, [aantal exemplaren handmatig of automatisch schalen](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). U kunt ook omhoog schalen door een andere App Service-plan te kiezen. Zie voor meer informatie, [opschalen van een app in Azure](../app-service/web-sites-scale.md). 

Bij het uitvoeren van JavaScript-functies op een App Service-plan, moet u een plan dat minder vcpu's is kiezen. Zie voor meer informatie, [Kies één core-App Service-plannen](functions-reference-node.md#choose-single-vcpu-app-service-plans).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

###<a name="always-on"></a> Altijd ingeschakeld

Als u op een App Service-plan uitvoert, moet u inschakelen de **altijd op** instellen zodat uw functie-app correct wordt uitgevoerd. Op een App Service-plan gaat de functions-runtime niet-actieve na een paar minuten van inactiviteit, zodat alleen HTTP-triggers '' uw functies inschakelt. Altijd is op alleen beschikbaar op een App Service-plan. Functie-apps die het automatisch door het platform wordt geactiveerd op een verbruiksabonnement.

## <a name="what-is-my-hosting-plan"></a>Wat is mijn hostingabonnement

Om te bepalen het hostingabonnement door uw functie-app gebruikt, Zie **App Service-plan / prijscategorie** in de **overzicht** tabblad voor de functie-app in de [Azure-portal](https://portal.azure.com). De prijscategorie wordt ook aangegeven voor App Service-plannen. 

![Plan voor vergroten/verkleinen weergeven in de portal](./media/functions-scale/function-app-overview-portal.png)

U kunt ook de Azure CLI gebruiken om te bepalen van het abonnement, als volgt:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Wanneer de uitvoer van deze opdracht is `dynamic`, uw functie-app is in het verbruiksabonnement. Alle andere waarden geven lagen van een App Service-plan.

Zelfs met Always On ingeschakeld, de time-out voor afzonderlijke functies uitvoeren wordt bepaald door de `functionTimeout` instellen in de [host.json](functions-host-json.md#functiontimeout) projectbestand.

## <a name="storage-account-requirements"></a>Vereisten voor een opslagaccount

Voor een verbruiksabonnement of een App Service-plan is een functie-app vereist een algemeen Azure Storage-account, die ondersteuning biedt voor Azure Blob, wachtrij, bestanden en Table storage. Dit is omdat functies is afhankelijk van Azure Storage voor bewerkingen zoals het beheren van triggers en uitvoeringen van de functie logboekregistratie, maar sommige opslagaccounts bieden geen ondersteuning wachtrijen en tabellen. Deze accounts, met onder andere alleen-blob storage-accounts (met inbegrip van de premium-opslag) en opslagaccounts met replicatie van de zone-redundante opslag, zijn gefilterd-out van uw bestaande **Opslagaccount** selecties bij het maken van een functie-app.

<!-- JH: Does using a Premium Storage account improve perf? -->

Zie voor meer informatie over de storage-accounttypen, [introductie van de Azure Storage-services](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>De werking van het plan verbruik

In het abonnement Consumption schaalt de controller schaal automatisch CPU en geheugenbronnen door toe te voegen extra exemplaren van de host van functies, op basis van het aantal gebeurtenissen die de functies ervan worden geactiveerd op. Elk exemplaar van de host functies is beperkt tot 1,5 GB aan geheugen.  Een exemplaar van de host is de functie-app, wat betekent dat alle functies in een functie-app delen resource binnen een exemplaar en de schaal op hetzelfde moment. Functie-apps die de dezelfde verbruiksabonnement delen worden onafhankelijk geschaald.  

Wanneer u het Verbruikshostingplan, wordt de functie codebestanden worden opgeslagen op Azure-bestandsshares op het belangrijkste opslagaccount van de functie. Wanneer u het belangrijkste opslagaccount van de functie-app verwijdert, wordt de functie code-bestanden worden verwijderd en kunnen niet worden hersteld.

> [!NOTE]
> Wanneer u een blobtrigger in een verbruiksabonnement gebruikt, kunnen er maximaal 10 minuten duren in de verwerking van nieuwe blobs. Deze vertraging treedt op wanneer een functie-app niet actief is geworden. Nadat de functie-app wordt uitgevoerd, worden onmiddellijk blobs verwerkt. Om te voorkomen dat deze vertraging koude start, gebruikt u een App Service-plan met **Always On** ingeschakeld of trigger van Event Grid gebruiken. Zie voor meer informatie, [naslagartikel voor de blob-trigger binding](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Runtime schalen

Azure Functions maakt gebruik van een component, genaamd de *schaal controller* de snelheid van gebeurtenissen controleren en bepalen of de schaal vergroten of te schalen. De controller schalen maakt gebruik van heuristiek voor elk triggertype. Bijvoorbeeld, als u een trigger voor Azure Queue storage, deze kan worden geschaald op basis van de lengte van de wachtrij en de leeftijd van de oudste wachtrijbericht.

De eenheid van de schaal is de functie-app. Wanneer de functie-app is uitgebreid, worden aanvullende resources toegewezen aan meerdere exemplaren van de Azure Functions-host uitvoeren. Daarentegen, zoals Reken-aanvraag wordt verminderd, de schaal controller functie host worden exemplaren verwijderd. Het aantal exemplaren is uiteindelijk omlaag geschaald op nul wanneer er zijn geen functies worden uitgevoerd binnen een functie-app.

![Schaal controller gebeurtenissen controleren en het maken van instanties](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Understanding schaalmogelijkheden

Schalen kan variëren van een aantal factoren, en schaal anders op basis van de trigger en taal selecteren. Er zijn een echter een paar aspecten van schaalaanpassing die momenteel aanwezig zijn in het systeem:

* Een functie-app kan zelfstandig omhoog worden geschaald naar maximaal 200 exemplaren. Slechts één exemplaar kan meer dan één bericht of aanvraag tegelijk verwerken, dus er is een limiet instellen voor het aantal gelijktijdige uitvoeringen niet.
* Nieuwe instanties wordt alleen maximaal elke 10 seconden worden toegewezen.

Verschillende triggers mogelijk ook andere limieten voor schalen, evenals gedocumenteerde hieronder:

* [Event Hub](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Aanbevolen procedures en patronen voor schaalbare apps

Er zijn veel aspecten van een functie-app die is van invloed op hoe goed deze wordt uitgebreid, met inbegrip van de hostconfiguratie van de, runtime footprint en efficiëntie van bronnen.  Zie voor meer informatie de [gedeelte van de schaalbaarheid van het artikel van de overwegingen met betrekking tot prestaties](functions-best-practices.md#scalability-best-practices). U moet ook rekening houden met de werking van verbindingen als uw functie-app worden geschaald. Zie voor meer informatie, [over het beheren van verbindingen in Azure Functions](manage-connections.md).

### <a name="billing-model"></a>Factureringsmodel

De facturering voor het abonnement Consumption wordt in detail beschreven op de [Pagina prijzen voor Azure Functions]. Gebruik op het niveau van de functie-app wordt geaggregeerd en telt alleen de tijd die de functiecode wordt uitgevoerd. Hier volgen de eenheden voor facturering:

* **Resourceverbruik in gigabyte-seconden (GB-s)**. Berekend als een combinatie van de grootte van geheugen en de runtime voor alle functies in een functie-app. 
* **Uitvoeringen**. Geteld telkens wanneer een functie wordt uitgevoerd in reactie op een trigger voor de gebeurtenis.

[Pagina prijzen voor Azure Functions]: https://azure.microsoft.com/pricing/details/functions
