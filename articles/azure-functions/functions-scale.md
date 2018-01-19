---
title: Azure Functions schalen en het hosten van | Microsoft Docs
description: Informatie over hoe u kiest tussen Azure Functions verbruik plannings- en App Service-abonnement.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, verbruik plan, app service-abonnement, verwerking van gebeurtenissen, webhooks, dynamische compute, zonder server-architectuur
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5be2fe57287f816434b6d6fdf40dbbcb0dd435f4
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions schalen en die als host fungeert

U kunt Azure Functions uitvoeren in twee verschillende modi: verbruik plannings- en Azure App Service-abonnement. Het plan verbruik wijst automatisch rekencapaciteit wanneer uw code wordt uitgevoerd, indien nodig om belasting te verwerken uitgeschaald en vervolgens omlaag geschaald wanneer de code wordt niet uitgevoerd. U hoeft niet te betalen voor niet-actieve virtuele machines en hoeft niet te worden van tevoren capaciteit reserveren. In dit artikel is gericht op het plan verbruik een [zonder server](https://azure.microsoft.com/overview/serverless-computing/) app-model. Zie voor meer informatie over de werking van de App Service-abonnement de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

>[!NOTE]  
> [Linux-host](functions-create-first-azure-function-azure-cli-linux.md) is momenteel alleen beschikbaar op een App Service-abonnement.

Als u niet bekend bent met Azure Functions, raadpleegt u de [overzicht van Azure Functions](functions-overview.md).

Wanneer u een functie-app maakt, moet u een hosting plan voor de functies die de app bevat. In beide modi, een exemplaar van de *Azure Functions host* de functies worden uitgevoerd. Het type van besturingselementen voor plannen:

* Hoe host exemplaren worden uitgebreid.
* De bronnen die beschikbaar voor elke host zijn.

Op dit moment moet u het type van het plan tijdens het maken van de functie-app hosten. U kunt deze later wijzigen. 

U kunt op een App Service-plan schalen tussen lagen andere hoeveelheid bronnen toewijzen. Op het plan verbruik verwerkt Azure Functions automatisch alle brontoewijzing.

## <a name="consumption-plan"></a>Verbruiksabonnement

Wanneer u een plan verbruik gebruikt, worden instanties van de Azure Functions-host dynamisch toegevoegd en verwijderd op basis van het aantal binnenkomende gebeurtenissen. Dit abonnement automatisch wordt geschaald en u in rekening worden gebracht rekenresources alleen wanneer uw functies worden uitgevoerd. Op een plan verbruik kan een functie voor een maximum van tien minuten uitgevoerd. 

> [!NOTE]
> De standaardtime-out voor de functies worden uitgevoerd op een plan verbruik is 5 minuten. De waarde kan worden verhoogd tot 10 minuten voor de functie-App met het wijzigen van de eigenschap `functionTimeout` in de [host.json](functions-host-json.md#functiontimeout) projectbestand.

Facturering is gebaseerd op aantal uitvoeringen, uitvoeringstijd en geheugen dat wordt gebruikt. Facturering wordt getotaliseerd over alle functies binnen een functie-app. Zie voor meer informatie de [Azure Functions pagina met prijzen].

Het plan verbruik is de standaardinstelling hosting-plan en biedt de volgende voordelen:
- U betaalt alleen wanneer uw functies worden uitgevoerd.
- Automatisch geschaald uitbreiden, zelfs tijdens perioden van hoge laadt.

## <a name="app-service-plan"></a>App Service-plan

In het App Service-plan uw functie-apps worden uitgevoerd via exclusieve virtuele machines op basis, standaard, Premium en geïsoleerde SKU's, vergelijkbaar met Web-Apps, API-Apps en mobiele Apps. Toegewezen virtuele machines worden toegewezen aan uw App Service-apps, wat betekent dat de host functies altijd wordt uitgevoerd. App Service-abonnementen ondersteuning voor Linux.

Houd rekening met een App Service-plan in de volgende gevallen:
- U hebt bestaande, onderbenutte virtuele machines waarop andere App Service-exemplaren.
- Verwacht u dat uw apps functie continu of bijna continu uit te voeren. In dit geval kan een App Service-Plan worden kosteneffectiever zijn.
- U moet meer opties voor de CPU of geheugen dan is opgegeven op het plan verbruik.
- U moet langer zijn dan de maximale uitvoeringstijd toegestaan op het plan verbruik (van 10 minuten) uitvoeren.
- Hebt u de functies die alleen beschikbaar op een App Service-abonnement, zoals ondersteuning voor App Service-omgeving, VNET/VPN-verbinding en grotere virtuele machine nodig. 
- U wilt uitvoeren van de functie-app op Linux of wilt u een aangepaste installatiekopie op voor het uitvoeren van uw functies opgeven.

Een virtuele machine worden losgekoppeld van de kosten van het aantal uitvoeringen, uitvoeringstijd en geheugen dat wordt gebruikt. Als gevolg hiervan Betaal je geen meer dan de kosten van de VM-instantie die u toewijst. Zie voor meer informatie over de werking van de App Service-abonnement de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Met App Service-abonnement kunt u handmatig uitschalen door meer VM-exemplaren toe te voegen of u automatisch schalen kunt inschakelen. Zie voor meer informatie [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). U kunt ook opschalen door een ander App Service-abonnement te kiezen. Zie voor meer informatie [een app in Azure opschalen](../app-service/web-sites-scale.md). 

Als u van plan bent voor het uitvoeren van JavaScript-functies op een App Service-abonnement, moet u een plan dat minder vcpu's is kiezen. Zie voor meer informatie de [single core-App Service-abonnementen Kies](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###AlwaysOn

Als u op een App Service-plan uitvoert, moet u inschakelen de **altijd op** instellen zodat uw app in de functie correct wordt uitgevoerd. Op een App Service-abonnement gaat de runtime van functions inactief na een paar minuten van inactiviteit, zodat alleen HTTP-triggers '' uw functies ontwaken wordt. Dit is vergelijkbaar met hoe WebJobs moet Always On ingesteld hebben. 

AlwaysOn is alleen beschikbaar op een App Service-abonnement. Functie apps die het automatisch door het platform wordt geactiveerd op een plan verbruik.

## <a name="storage-account-requirements"></a>Vereisten voor een opslagaccount

Een functie-app vereist op een plan verbruik of een App Service-abonnement en een algemene Azure Storage-account die ondersteuning biedt voor opslag in Azure Blob, wachtrijen, bestanden en tabel. Azure Functions gebruikt intern, Azure Storage voor bewerkingen zoals het beheren van triggers en functies die logboekregistratie. Sommige opslagaccounts bieden geen ondersteuning voor wachtrijen en tabellen, zoals alleen-blob storage-accounts (met inbegrip van premium-opslag) en opslagaccounts met replicatie van de zone-redundante opslag. Deze accounts zijn gefilterd uit de **Opslagaccount** blade als u een functie-app.

<!-- JH: Does using a PRemium Storage account improve perf? -->

Zie voor meer informatie over opslagaccounttypen, [introductie van de Azure Storage-services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Hoe werkt dit plan verbruik

In het plan verbruik schaalt de controller schaal automatisch CPU en geheugenbronnen door toe te voegen extra exemplaren van de host van functies, op basis van het aantal gebeurtenissen die de functies worden geactiveerd op. Elk exemplaar van de host van functies is beperkt tot 1,5 GB aan geheugen.  Een exemplaar van de host is de functie-App, wat betekent dat alle functies in een functie resources van de app delen binnen een exemplaar en de schaal op hetzelfde moment.

Wanneer u het verbruik die als host fungeert voor plan gebruikt, wordt de functie codebestanden worden opgeslagen op Azure-bestandsshares op de belangrijkste storage-account van de functie. Wanneer u de belangrijkste storage-account van de functie-app verwijdert, wordt de functie code-bestanden worden verwijderd en kunnen niet worden hersteld.

> [!NOTE]
> Wanneer u een blob-trigger op een plan verbruik, kunnen er maximaal 10 minuten vertraging bij de verwerking van nieuwe blobs als een functie-app niet actief is geworden. Nadat de functie-app wordt uitgevoerd, worden onmiddellijk blobs verwerkt. Overweeg om te voorkomen dat deze initiële vertraging, een van de volgende opties:
> - Host functie-app op een App Service-abonnement met altijd op ingeschakeld.
> - Gebruik een ander mechanisme voor het activeren van de blob verwerken, zoals een gebeurtenis raster-abonnement of een wachtrijbericht met de blob-naam. Zie voor een voorbeeld de [voorbeelden voor de blob invoer binding](functions-bindings-storage-blob.md#input---example).

### <a name="runtime-scaling"></a>Runtime-schaling

Azure Functions maakt gebruik van een onderdeel genaamd de *scale controller* om te controleren van de snelheid van gebeurtenissen en bepalen of u wilt uitbreiden of schalen. De scale-controller gebruikt methodiek voor elk triggertype. Bijvoorbeeld, wanneer u een Azure Queue storage-trigger, schalen het op basis van de lengte van de wachtrij en de leeftijd van oudste bericht uit de wachtrij.

De eenheid van de schaal is de functie-app. Wanneer de functie-app is uitgebreid, worden aanvullende bronnen toegewezen aan meerdere exemplaren van de Azure Functions-host uitvoeren. Als u daarentegen, zoals Reken-aanvraag wordt verlaagd, de controller scale functie host worden exemplaren verwijderd. Het aantal exemplaren wordt uiteindelijk geschaald naar beneden op nul wanneer er geen functies worden uitgevoerd binnen een functie-app.

![Schaal controller controle van gebeurtenissen en het maken van exemplaren](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Understanding schaalmogelijkheden

Schalen kan variëren van een aantal factoren en schaal anders op basis van de trigger en de geselecteerde taal. Er zijn echter enkele aspecten van de schaal die in het systeem bestaat vandaag:
* Een enkele functie-app worden alleen geschaald tot maximaal 200 exemplaren. Slechts één exemplaar mogelijk meer dan één bericht of aanvraag tegelijk verwerken echter zodat er een limiet van het aantal gelijktijdige uitvoeringen niet.
* Nieuwe exemplaren wordt alleen toegewezen maximaal één keer voor elke 10 seconden.

Andere triggers mogelijk ook andere schaalbaarheidsgrenzen, evenals een gedocumenteerde hieronder:

* [Event Hub](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Aanbevolen procedures en patronen voor schaalbare apps

Er zijn veel aspecten van een functie-app die is van invloed op hoe goed het kunnen worden geschaald, zoals de hostconfiguratie, runtime footprint en efficiëntie van de resource.  Weergave de [schaalbaarheid sectie van het artikel van de overwegingen met betrekking tot prestaties](functions-best-practices.md#scalability-best-practices) voor meer informatie.

### <a name="billing-model"></a>Factureringsmodel

Financieel voor het verbruik plan wordt in detail beschreven op de [Azure Functions pagina met prijzen]. Gebruik op het niveau van de functie-app wordt geaggregeerd en telt alleen de tijd die de functiecode wordt uitgevoerd. Hier volgen de eenheden voor facturering: 
* **Brongebruik in gigabyte (GB-s) seconden**. Berekend als een combinatie van de grootte van het systeemgeheugen en de uitvoeringstijd voor alle functies binnen een functie-app. 
* **Uitvoeringen**. Geteld telkens wanneer een functie wordt uitgevoerd in reactie op een gebeurtenistrigger.

[Azure Functions pagina met prijzen]: https://azure.microsoft.com/pricing/details/functions
