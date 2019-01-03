---
title: App Service-plan overzicht - Azure | Microsoft Docs
description: Lees hoe App Service-plannen voor Azure App Service, en hoe ze profiteren van uw ervaring.
keywords: appservice, azure appservice, schaal, schaalbaar, schaalbaarheid, app service-plan, kosten app service
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ab04d1288eb3a851774128b8aaaae03868c2ffa7
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731138"
---
# <a name="azure-app-service-plan-overview"></a>Overzicht van Azure App Service-plan

In App Service wordt een app uitgevoerd in een _App Service-plan_. Een App Service-plan definieert een set rekenresources waarmee een web-app kan worden uitgevoerd. Deze compute-resources zijn vergelijkbaar met de [ _serverfarm_ ](https://wikipedia.org/wiki/Server_farm) in conventionele webhosting. Een of meer apps kunnen worden geconfigureerd om te worden uitgevoerd op de dezelfde computerbronnen (of in hetzelfde App Service-plan).

Wanneer u een App Service-plan in een bepaalde regio (bijvoorbeeld, West-Europa) maakt, wordt een set van compute-resources gemaakt voor het abonnement in deze regio. Welke apps u in deze App Service-plan op deze instanties uitgevoerd plaatsen compute-resources zoals gedefinieerd door uw App Service-plan. Elke App Service-plan definieert:

- Regio (VS-West, VS-Oost, etc.)
- Aantal VM-exemplaren
- Grootte van VM-exemplaren (klein, middelgroot, groot)
- Prijscategorie (Free, Shared, Basic, Standard, Premium, PremiumV2, geïsoleerd, verbruik)

De _prijscategorie_ van een App Service plan bepaalt van welke App Service-functies die u aan en hoeveel u betaalt voor het abonnement. Er zijn enkele categorieën worden ondergebracht van Prijscategorieën:

- **Gedeelde compute**: **Gratis** en **gedeelde**, de twee basis-lagen, wordt een app op dezelfde Azure-VM als andere App Service-apps, met inbegrip van apps van andere klanten. Deze lagen toewijzen CPU quota's voor elke app die wordt uitgevoerd op de gedeelde resources en de resources niet opschalen.
- **Toegewezen compute**: De **Basic**, **Standard**, **Premium**, en **PremiumV2** lagen apps uitvoeren op de toegewezen virtuele machines van Azure. Alleen de apps in hetzelfde App Service-plan, delen de dezelfde compute-resources. Hoe hoger de laag de meer VM-exemplaren zijn beschikbaar voor scale-out.
- **Geïsoleerde**: Deze laag wordt toegewezen virtuele machines van Azure uitgevoerd op specifieke Azure Virtual Networks, waarmee u netwerkisolatie boven op compute isolatie aan uw apps. De maximale uitbreidbare mogelijkheden biedt.
- **Verbruik**: Deze laag is alleen beschikbaar voor [functie-apps](../azure-functions/functions-overview.md). Deze functies dynamisch, afhankelijk van de werkbelasting kan worden geschaald. Zie voor meer informatie, [Azure Functions vergelijking van hostingabonnementen](../azure-functions/functions-scale.md).

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Elke laag biedt ook een specifieke subset van de App Service-functies. Deze functies omvatten aangepaste domeinen en SSL-certificaten, automatisch schalen, implementatiesleuven, back-ups, Traffic Manager-integratie en meer. Hoe hoger de laag, meer functies zijn beschikbaar. Als u wilt weten welke functies worden ondersteund in elke prijscategorie, Zie [App Service-plan details](https://azure.microsoft.com/pricing/details/app-service/plans/).

<a name="new-pricing-tier-premiumv2"></a>

> [!NOTE]
> De nieuwe **PremiumV2** prijscategorie biedt [Dv2-serie VM's](../virtual-machines/windows/sizes-general.md#dv2-series) met snellere processoren, SSD-opslag en twee keer zoveel geheugen-kernverhouding per **Standard** laag. **PremiumV2** ondersteunt ook grotere schalen dankzij een verhoogd aantal Instances terwijl het toch alle geavanceerde mogelijkheden bevat die in de Standard-abonnement. Alle functies die beschikbaar zijn in de bestaande **Premium** laag zijn opgenomen in **PremiumV2**.
>
> Net als bij andere speciale lagen, drie VM-grootten zijn beschikbaar voor deze laag:
>
> - Klein (één CPU-core, 3,5 GiB geheugen) 
> - Gemiddeld (twee CPU-kernen, 7 GiB geheugen) 
> - Groot (vier CPU-kernen, 14 GiB geheugen)  
>
> Voor **PremiumV2** informatie over de prijzen, Zie [prijzen voor App Service](https://azure.microsoft.com/pricing/details/app-service/).
>
> Aan de slag met de nieuwe **PremiumV2** prijscategorie, Zie [configureren PremiumV2-laag voor App Service](app-service-configure-premium-tier.md).

## <a name="how-does-my-app-run-and-scale"></a>Hoe mijn app uitvoeren en schalen?

In de **gratis** en **gedeelde** lagen, een app ontvangt CPU minuten op een gedeelde VM-exemplaar en niet opschalen. In de andere lagen, een app wordt uitgevoerd en kan worden geschaald als volgt.

Wanneer u een app in App Service maakt, wordt deze geplaatst in een App Service-plan. Wanneer de app wordt uitgevoerd, wordt deze uitgevoerd op alle VM-exemplaren die worden geconfigureerd in de App Service-plan. Als meerdere apps zich in hetzelfde App Service-plan, delen ze allemaal dezelfde VM-instanties. Als u meerdere implementatiesites gebruiken voor een app hebt, wordt de implementatiesites voor alle ook uitvoeren op dezelfde VM-instanties. Als u logboeken met diagnostische gegevens inschakelen, back-ups uitvoeren of WebJobs worden uitgevoerd, ze ook gebruiken CPU-cycli en het geheugen op deze VM-instanties.

De App Service-plan is op deze manier kunnen de schaaleenheid van de App Service-apps. Als het plan is geconfigureerd voor het uitvoeren van vijf VM-instanties, en vervolgens alle apps in het abonnement worden uitgevoerd op alle vijf exemplaren. Het plan is geconfigureerd voor automatisch schalen, en vervolgens alle apps in het plan samen worden geschaald op basis van de instellingen voor automatisch schalen.

Zie voor meer informatie over een app uitschalen [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="cost"></a>

## <a name="how-much-does-my-app-service-plan-cost"></a>Wat kost mijn App Service-plan?

Deze sectie wordt beschreven hoe App Service-apps worden in rekening gebracht. Zie voor gedetailleerde, regio-specifieke informatie over de prijzen, [prijzen voor App Service](https://azure.microsoft.com/pricing/details/app-service/).

Met uitzondering van **gratis** laag, een App Service-plan wordt een uurtarief voor de compute-resources die worden gebruikt.

- In de **gedeelde** laag, elke app ontvangt een quotum van CPU-minuten, dus _elke app_ wordt in rekening gebracht per uur voor het CPU-quotum.
- In de compute-lagen (**Basic**, **Standard**, **Premium**, **PremiumV2**), het App Service-plan definieert het aantal virtuele machine exemplaren van de apps worden geschaald, zodat _elk VM-exemplaar_ in de App Service-plan heeft een uurtarief. Deze VM-exemplaren worden in rekening gebracht de hetzelfde, ongeacht hoeveel apps zijn die daarop worden uitgevoerd. Om te voorkomen van onverwachte kosten, Zie [opschonen van een App Service-plan](app-service-plan-manage.md#delete).
- In de **geïsoleerd** laag, de App Service-omgeving bepaalt het aantal van de geïsoleerde werknemers die uw apps worden uitgevoerd en _elke werknemer_ per uur in rekening wordt gebracht. Er is bovendien een base uurtarief voor de actieve App Service Environment zelf. 
- (Alleen voor azure Functions) De **verbruik** laag dynamisch toegewezen VM-exemplaren te reserveren voor een functie-app werkbelasting en dynamisch wordt berekend per seconde door Azure. Zie voor meer informatie, [prijzen van Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

U niet in rekening gebracht voor het gebruik van de App Service-functies die beschikbaar zijn voor u (configureren van aangepaste domeinen, SSL-certificaten, implementatiesleuven, back-ups, enz.). De uitzonderingen zijn:

- App Service-domeinen - betaalt u bij de aankoop in Azure en wanneer u deze verlengt elk jaar.
- App Service-certificaten - betaalt u bij de aankoop in Azure en wanneer u deze verlengt elk jaar.
- SSL-verbindingen op basis van IP - er zijn een uurtarief voor elke IP gebaseerde SSL-verbinding, maar sommige de **Standard** laag of hoger beschikt u over een IP-gebaseerd SSL verbinding gratis. Op basis van SNI SSL-verbindingen zijn gratis.

> [!NOTE]
> Als u App Service met andere Azure-service integreren, moet u mogelijk rekening houden met de kosten van deze andere services. Bijvoorbeeld, als u Azure Traffic Manager voor het schalen van uw app geografisch, Azure Traffic Manager ook kosten in rekening gebracht gebruiken u op basis van uw gebruik. Zie voor een schatting van uw kosten cross-services in Azure, [prijscalculator](https://azure.microsoft.com/pricing/calculator/). 
>
>

## <a name="what-if-my-app-needs-more-capabilities-or-features"></a>Wat gebeurt er als mijn app meer mogelijkheden of functies nodig?

Uw App Service-plan kan op elk gewenst moment omhoog of omlaag worden geschaald. Het is net zo eenvoudig als de prijscategorie van het abonnement wilt wijzigen. U kunt een lagere prijscategorie in eerste instantie kiezen en later opschalen als u meer App Service-functies nodig hebt.

U kunt bijvoorbeeld starten voor het testen van uw web-app in een **gratis** App Service plannen en betaal niets. Als u wilt toevoegen de [aangepaste DNS-naam](app-service-web-tutorial-custom-domain.md) naar de web-app alleen opgeschaald uw plan zodat **gedeelde** laag. Later, wanneer u wilt toevoegen een [aangepast SSL-certificaat](app-service-web-tutorial-custom-ssl.md), opgeschaald zodat uw plan **Basic** laag. Als u wilt hebben [faseringsomgevingen](deploy-staging-slots.md), opgeschaald zodat **Standard** laag. Als u meer kernen, geheugen of opslag, kan worden uitgebreid naar een grotere VM-grootte in dezelfde laag.

De dezelfde werkt in de omgekeerde volgorde. Als u denkt dat u niet meer nodig hebt van de functies of onderdelen van een hogere laag, u omlaag kunt schalen naar een lagere laag, waarmee u aanzienlijk bespaart.

Zie voor informatie over het omhoog schalen van de App Service-plan, [opschalen van een app in Azure](web-sites-scale.md).

Als uw app zich in hetzelfde App Service-plan met andere apps, kunt u van de app om prestaties te verbeteren door te isoleren van de compute-resources. U kunt dit doen door het verplaatsen van de app naar een afzonderlijke App Service-plan. Zie voor meer informatie, [verplaatsen van een app naar een andere App Service-plan](app-service-plan-manage.md#move).

## <a name="should-i-put-an-app-in-a-new-plan-or-an-existing-plan"></a>Moet ik een app in een nieuw abonnement of een bestaand plan plaatsen?

Aangezien u voor de rekenbronnen uw App Service-plan betaalt kan worden toegewezen (Zie [wat kost mijn Appservice-plan?](#cost)), kunt u mogelijk geld besparen door meerdere apps in een App Service-plan te brengen. U kunt apps toevoegen aan een bestaand plan zolang het plan heeft onvoldoende resources om de belasting te verwerken. Echter, houd er rekening mee dat rekenresources apps in hetzelfde App Service-plan dat alle delen dezelfde. Om te bepalen dat of de nieuwe app om de benodigde bronnen heeft, moet u weten wat de capaciteit van de bestaande App Service-plan en de verwachte belasting voor de nieuwe app. Een App Service-plan overbelasting kan mogelijk leiden tot uitvaltijd voor uw nieuwe en bestaande apps.

Kan worden geïsoleerd uw app in een nieuw App Service-plan wanneer:

- De app is resource-intensieve.
- Wilt u de app onafhankelijk van andere apps schalen het bestaande plan.
- De app moet een resource in een andere geografische regio.

Op deze manier kunt u een nieuwe set met resources toewijzen voor uw app en bovendien profiteren van meer controle over uw apps.

## <a name="manage-an-app-service-plan"></a>Beheren van een App Service-plan

> [!div class="nextstepaction"]
> [Beheren van een App Service-plan](app-service-plan-manage.md)
