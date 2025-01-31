---
title: Azure Functions-schaal en hosting | Microsoft Docs
description: Informatie over het kiezen tussen het verbruiksabonnement voor Azure Functions en Premium-abonnement.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functies, verbruiksabonnement, premium-abonnement, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 03/27/2019
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3253cc7e379ae63880d533f14bc76e7af5a4425a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050556"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions-schaal en hosting

Wanneer u een functie-app in Azure maakt, moet u een hostingabonnement kiezen voor uw app. Er zijn drie hostingabonnementen beschikbaar voor Azure Functions: [Verbruiksabonnement](#consumption-plan), [Premium-abonnement](#premium-plan), en [App Service-plan](#app-service-plan).

Het hostingabonnement die u kiest, bepaalt de volgende problemen:

* Hoe uw functie-app wordt geschaald.
* De beschikbare resources voor elke functie-app-exemplaar.
* Ondersteuning voor geavanceerde functies, zoals de VNET-connectiviteit.

Compute-kracht verbruik- en Premium-abonnementen automatisch toegevoegd wanneer uw code wordt uitgevoerd. Uw app is uitgeschaald wanneer dat nodig is om belasting te verwerken en omlaag worden geschaald wanneer de code is gestopt. Voor het abonnement Consumption hoeft u ook te betalen voor niet-actieve virtuele machines of vooraf capaciteit reserveren.  

Premium-abonnement biedt aanvullende functies zoals premium compute-exemplaren, de mogelijkheid om te houden voor onbepaalde tijd de warme van exemplaren en VNet-connectiviteit.

App Service-plan kunt u profiteren van toegewezen infrastructuur, die u beheert. Uw functie-app niet schalen op basis van gebeurtenissen, wat betekent dat is nooit worden geschaald tot nul. (Vereist dat [altijd op](#always-on) is ingeschakeld.)

> [!NOTE]
> U kunt schakelen tussen verbruik en Premium-abonnementen door het wijzigen van de eigenschap schema van de resource voor de functie-app.

## <a name="hosting-plan-support"></a>Ondersteuning voor hosting plan

Ondersteuning van functies valt in de volgende twee categorieën:

* _Algemeen beschikbaar (GA)_ : volledig ondersteund en goedgekeurd voor gebruik in productieomgevingen.
* _Preview-versie_: nog niet volledig ondersteund en goedgekeurd voor gebruik in productieomgevingen.

In de volgende tabel geeft het huidige niveau van ondersteuning voor de drie hostingabonnementen bij het uitvoeren van op Windows of Linux:

| | Verbruiksabonnement | Premium-abonnement | Toegewezen planning |
|-|:----------------:|:------------:|:----------------:|
| Windows | Algemene beschikbaarheid | preview | Algemene beschikbaarheid |
| Linux | preview | N/A | Algemene beschikbaarheid |

## <a name="consumption-plan"></a>Verbruiksabonnement

Wanneer u het verbruiksabonnement gebruikt, worden instanties van de Azure Functions-host dynamisch toegevoegd en verwijderd op basis van het aantal binnenkomende gebeurtenissen. Dit plan serverloze automatisch wordt geschaald en u betaalt voor computerresources alleen wanneer uw functies worden uitgevoerd. In een verbruiksabonnement verloopt een functie-uitvoering na een periode die is geconfigureerd.

Facturering is gebaseerd op aantal uitvoeringen, uitvoeringstijd en geheugen dat wordt gebruikt. Facturering wordt samengevoegd voor alle functies in een functie-app. Zie voor meer informatie de [Azure Functions-pagina met prijzen](https://azure.microsoft.com/pricing/details/functions/).

Het verbruiksabonnement is de standaardinstelling hostingabonnement en biedt de volgende voordelen:

* Betaalt alleen wanneer uw functies worden uitgevoerd
* Automatisch opwaarts schalen, zelfs tijdens perioden van hoge laden

Functie-apps in dezelfde regio kunnen worden toegewezen aan de dezelfde verbruiksabonnement. Er is geen nadeel of gevolgen voor meerdere apps die worden uitgevoerd in hetzelfde abonnement voor gebruik. Meerdere apps toewijzen aan de dezelfde verbruiksabonnement heeft geen invloed op flexibiliteit, schaalbaarheid en betrouwbaarheid van elke app.

## <a name="premium-plan"></a>Premium-abonnement (preview)

Wanneer u het Premium-abonnement gebruikt, zijn exemplaren van de Azure Functions-host toegevoegd en verwijderd op basis van het aantal inkomende gebeurtenissen net als bij het verbruiksabonnement.  Premium-abonnement ondersteunt de volgende functies:

* Onbeperkte warme exemplaren om te voorkomen dat een koude start
* VNet-connectiviteit
* Onbeperkte uitvoeringstijd
* Premium-exemplaargrootten (één kern, twee core en exemplaren van vier kerngeheugens)
* Meer voorspelbare prijzen
* High-app-toewijzing voor abonnementen met meerdere functie-apps

Meer informatie over het configureren van deze opties vindt u de [Azure Functions premium plan document](functions-premium-plan.md).

Facturering voor het Premium-abonnement is in plaats van facturering per uitvoering en het geheugen verbruikt, gebaseerd op het aantal core-seconden uitvoeringstijd en geheugen dat wordt gebruikt voor vereiste en gereserveerde instanties.  Ten minste één exemplaar moet warme ten allen tijde. Dit betekent dat er een vaste maandelijkse prijs per actieve schema, ongeacht het aantal uitvoeringen.

Houd rekening met het premium-abonnement voor Azure Functions in de volgende situaties:

* Uw functie-apps voortdurend of vrijwel continu uitvoeren.
* U moet meer opties voor de CPU of geheugen dan wat wordt geleverd door het verbruiksabonnement.
* Uw code moet langer zijn dan de [maximale uitvoeringstijd toegestaan](#timeout) op het verbruiksabonnement.
* U functies die alleen beschikbaar op een Premium-abonnement, zoals VNET/VPN-verbinding zijn nodig hebben.

> [!NOTE]
> De preview van premium-plan ondersteunt momenteel alleen Azure-functies op Windows.

Bij het uitvoeren van JavaScript-functies op een Premium-abonnement, moet u een exemplaar dat minder vcpu's is kiezen. Zie voor meer informatie de [single-core Premium-abonnementen kiezen](functions-reference-node.md#considerations-for-javascript-functions).  

## <a name="app-service-plan"></a>Toegewezen (App Service)-abonnement

Uw functie-apps kunnen ook uitvoeren op de dezelfde toegewezen virtuele machines als andere App Service-apps (Basic, Standard, Premium en geïsoleerd SKU's).

Houd rekening met een App Service-plan in de volgende situaties:

* U hebt bestaande, weinig gebruikte virtuele machines die al een andere App Service-exemplaren worden uitgevoerd.
* Wilt u mogelijk een aangepaste installatiekopie op voor het uitvoeren van uw functies.

U betaalt hetzelfde voor functie-apps in een App Service-Plan als bij andere App Service-resources, zoals web-apps. Zie voor meer informatie over de werking van de App Service-plan de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/overview-hosting-plans.md).

Bij een App Service-abonnement, kunt u handmatig opwaarts schalen door meer VM-exemplaren toe te voegen. U kunt ook inschakelen voor automatisch schalen. Zie voor meer informatie, [aantal exemplaren handmatig of automatisch schalen](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). U kunt ook omhoog schalen door een andere App Service-plan te kiezen. Zie voor meer informatie, [opschalen van een app in Azure](../app-service/web-sites-scale.md). 

Bij het uitvoeren van JavaScript-functies op een App Service-plan, moet u een plan dat minder vcpu's is kiezen. Zie voor meer informatie, [Kies één core-App Service-plannen](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a> Altijd ingeschakeld

Als u op een App Service-plan uitvoert, moet u inschakelen de **altijd op** instellen zodat uw functie-app correct wordt uitgevoerd. Op een App Service-plan gaat de functions-runtime niet-actieve na een paar minuten van inactiviteit, zodat alleen HTTP-triggers '' uw functies inschakelt. Altijd is op alleen beschikbaar op een App Service-plan. Functie-apps die het automatisch door het platform wordt geactiveerd op een verbruiksabonnement.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Zelfs met Always On ingeschakeld, de time-out voor afzonderlijke functies uitvoeren wordt bepaald door de `functionTimeout` instellen in de [host.json](functions-host-json.md#functiontimeout) projectbestand.

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Het hostingabonnement van een bestaande toepassing bepalen

Om te bepalen het hostingabonnement door uw functie-app gebruikt, Zie **App Service-plan / prijscategorie** in de **overzicht** tabblad voor de functie-app in de [Azure-portal](https://portal.azure.com). De prijscategorie wordt ook aangegeven voor App Service-plannen.

![Plan voor vergroten/verkleinen weergeven in de portal](./media/functions-scale/function-app-overview-portal.png)

U kunt ook de Azure CLI gebruiken om te bepalen van het abonnement, als volgt:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Wanneer de uitvoer van deze opdracht is `dynamic`, uw functie-app is in het verbruiksabonnement. Wanneer de uitvoer van deze opdracht is `ElasticPremium`, uw functie-app is in het Premium-abonnement. Alle andere waarden geven aan verschillende lagen van een App Service-plan.

## <a name="storage-account-requirements"></a>Vereisten voor een opslagaccount

Voor elk plan is een functie-app vereist een algemeen Azure Storage-account, die ondersteuning biedt voor Azure Blob, wachtrij, bestanden en Table storage. Dit is omdat functies zijn afhankelijk van Azure Storage voor bewerkingen zoals het beheren van triggers en uitvoeringen van de functie logboekregistratie, maar sommige opslagaccounts bieden geen ondersteuning wachtrijen en tabellen. Deze accounts, met onder andere alleen-blob storage-accounts (met inbegrip van de premium-opslag) en opslagaccounts met replicatie van de zone-redundante opslag, zijn gefilterd-out van uw bestaande **Opslagaccount** selecties bij het maken van een functie-app.

<!-- JH: Does using a Premium Storage account improve perf? -->

Zie voor meer informatie over de storage-accounttypen, [introductie van de Azure Storage-services](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>De werking van het verbruik en premium-abonnementen

In het verbruik en de premium-abonnementen, kan de infrastructuur van Azure Functions worden geschaald, de CPU en geheugenbronnen door toe te voegen extra exemplaren van de host van functies, op basis van het aantal gebeurtenissen die de functies ervan worden geactiveerd op. Elk exemplaar van de host van de functies in het verbruiksabonnement is beperkt tot 1,5 GB aan geheugen en één CPU.  Een exemplaar van de host is de gehele functie-app, wat betekent dat alle functies in een functie-app delen resource binnen een exemplaar en de schaal op hetzelfde moment. Functie-apps die de dezelfde verbruiksabonnement delen worden onafhankelijk geschaald.  In het premium-abonnement bepaalt de grootte van uw abonnement het beschikbare geheugen en CPU voor alle apps in het abonnement op dat exemplaar.  

Functie codebestanden worden opgeslagen op Azure-bestandsshares op het belangrijkste opslagaccount van de functie. Wanneer u het belangrijkste opslagaccount van de functie-app verwijdert, wordt de functie code-bestanden worden verwijderd en kunnen niet worden hersteld.

> [!NOTE]
> Wanneer u een blobtrigger in een verbruiksabonnement gebruikt, kunnen er maximaal 10 minuten duren in de verwerking van nieuwe blobs. Deze vertraging treedt op wanneer een functie-app niet actief is geworden. Nadat de functie-app wordt uitgevoerd, worden onmiddellijk blobs verwerkt. Het Premium-abonnement gebruiken om te voorkomen dat deze vertraging koude start, of gebruik de [trigger van Event Grid](functions-bindings-event-grid.md). Zie voor meer informatie, [naslagartikel voor de blob-trigger binding](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Runtime schalen

Azure Functions maakt gebruik van een component, genaamd de *schaal controller* de snelheid van gebeurtenissen controleren en bepalen of de schaal vergroten of te schalen. De controller schalen maakt gebruik van heuristiek voor elk triggertype. Bijvoorbeeld, als u een trigger voor Azure Queue storage, deze kan worden geschaald op basis van de lengte van de wachtrij en de leeftijd van de oudste wachtrijbericht.

De eenheid van de schaal voor Azure Functions is de functie-app. Wanneer de functie-app is uitgebreid, worden aanvullende resources toegewezen aan meerdere exemplaren van de Azure Functions-host uitvoeren. Daarentegen, zoals Reken-aanvraag wordt verminderd, de schaal controller functie host worden exemplaren verwijderd. Het aantal exemplaren is uiteindelijk omlaag geschaald op nul wanneer er zijn geen functies worden uitgevoerd binnen een functie-app.

![Schaal controller gebeurtenissen controleren en het maken van instanties](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Understanding schaalmogelijkheden

Schalen kan variëren van een aantal factoren, en schaal anders op basis van de trigger en taal selecteren. Er zijn enkele van de complexiteit van schaalmogelijkheden rekening mee moet houden:

* Een functie-app kan zelfstandig omhoog worden geschaald naar maximaal 200 exemplaren. Slechts één exemplaar kan meer dan één bericht of aanvraag tegelijk verwerken, dus er is een limiet instellen voor het aantal gelijktijdige uitvoeringen niet.
* Voor HTTP-triggers, wordt nieuwe instanties alleen toegewezen maximaal eenmaal per 1 seconde.
* Voor niet-HTTP-triggers, wordt nieuwe instanties alleen toegewezen maximaal elke 30 seconden.

Verschillende triggers mogelijk ook andere limieten voor schalen, evenals gedocumenteerde hieronder:

* [Event Hub](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Aanbevolen procedures en patronen voor schaalbare apps

Er zijn veel aspecten van een functie-app die is van invloed op hoe goed deze wordt uitgebreid, met inbegrip van de hostconfiguratie van de, runtime footprint en efficiëntie van bronnen.  Zie voor meer informatie de [gedeelte van de schaalbaarheid van het artikel van de overwegingen met betrekking tot prestaties](functions-best-practices.md#scalability-best-practices). U moet ook rekening houden met de werking van verbindingen als uw functie-app worden geschaald. Zie voor meer informatie, [over het beheren van verbindingen in Azure Functions](manage-connections.md).

### <a name="billing-model"></a>Factureringsmodel

Facturering voor de verschillende abonnementen wordt in detail beschreven op de [Azure Functions-pagina met prijzen](https://azure.microsoft.com/pricing/details/functions/). Gebruik op het niveau van de functie-app wordt geaggregeerd en telt alleen de tijd die de functiecode wordt uitgevoerd. Hier volgen de eenheden voor facturering:

* **Resourceverbruik in gigabyte-seconden (GB-s)** . Berekend als een combinatie van de grootte van geheugen en de runtime voor alle functies in een functie-app. 
* **Uitvoeringen**. Geteld telkens wanneer een functie wordt uitgevoerd in reactie op een trigger voor de gebeurtenis.

Handige query's en informatie over hoe u meer informatie over uw factuur verbruik vindt [in de facturering Veelgestelde vragen over](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ).

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="service-limits"></a>Servicelimieten

De volgende tabel staan de limieten die betrekking hebben op de functie-apps bij uitvoering in de verschillende hostingabonnementen:

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]
