---
title: Overzicht van Azure App Service-plan | Microsoft Docs
description: Lees hoe App Service-plannen voor Azure App Service werk, en hoe ze uw beheerervaring profiteren.
keywords: App service, azure app service, schaal, schaalbaar, schaalbaarheid, app service-abonnement, Servicekosten app
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: 268844eae8dc06937529e79d52515cad2f6da3f4
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-app-service-plan-overview"></a>Overzicht van Azure App Service-plan

In App Service een app wordt uitgevoerd in een _App Service-abonnement_. Een App Service-plan definieert een aantal rekenresources voor een web-app om uit te voeren. Deze compute resources zijn vergelijkbaar met de [ _serverfarm_ ](https://wikipedia.org/wiki/Server_farm) in conventionele webhosting. Een of meer apps kunnen worden geconfigureerd om te worden uitgevoerd op de dezelfde computerbronnen (of in dezelfde App Service-abonnement).

Wanneer u een App Service-abonnement maakt in een bepaalde regio (bijvoorbeeld, West-Europa), wordt een set van rekenresources gemaakt voor dit abonnement in deze regio. Welke apps u in dit App Service-abonnement uitvoeren op deze zetten rekenresources zoals gedefinieerd door uw App Service-abonnement. Elke App Service-abonnement worden gedefinieerd:

- Regio (VS-West, VS-Oost, enz.)
- Aantal VM-exemplaren
- Grootte van de VM-instanties (klein, normaal, groot)
- Prijscategorie (vrije, gedeelde, Basic, Standard, Premium, PremiumV2, geïsoleerd, verbruik)

De _prijscategorie_ van een App Service plan bepaalt welke App Service-functies die u krijgt en hoeveel u betalen voor het plan. Er zijn enkele categorieën van Prijscategorieën:

- **Gedeelde compute**: **vrije** en **gedeelde**, de twee lagen, wordt uitgevoerd een app op dezelfde Azure virtuele machine als andere App Service-apps, waaronder apps van andere klanten baseren. Deze lagen toewijzen CPU quota voor elke app die wordt uitgevoerd op de gedeelde bronnen en de resources kunnen niet worden uitgebreid.
- **Speciale berekenings**: de **Basic**, **standaard**, **Premium**, en **PremiumV2** lagen apps op specifieke Azure uitvoeren Virtuele machines. Alleen apps in hetzelfde App Service-plan delen de dezelfde rekenresources. Hoe hoger de laag, meer VM-exemplaren zijn beschikbaar voor scale-out.
- **Geïsoleerde**: deze laag toegewezen Azure-virtuele machines op specifieke Azure Virtual Networks, waardoor netwerkisolatie boven compute-isolatie naar uw apps op wordt uitgevoerd. Het biedt de maximale uitbreidbare mogelijkheden.
- **Verbruik**: deze laag is alleen beschikbaar voor [apps werken](../azure-functions/functions-overview.md). Het schalen de functies voor dynamisch, afhankelijk van de werkbelasting. Zie voor meer informatie [vergelijking van Azure Functions hosting plannen](../azure-functions/functions-scale.md).

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Elke laag biedt ook een specifieke subset van App Service-functies. Deze functies omvatten aangepaste domeinen en SSL-certificaten, automatisch schalen, implementatiesites, back-ups, Traffic Manager-integratie en meer. Hoe hoger de laag, meer functies zijn beschikbaar. Als u wilt weten welke functies worden ondersteund in elke prijscategorie, Zie [details van App Service-plan](https://azure.microsoft.com/pricing/details/app-service/plans/).

<a name="new-pricing-tier-premiumv2"></a>

> [!NOTE]
> De nieuwe **PremiumV2** prijscategorie biedt [Dv2-serie VMs](../virtual-machines/windows/sizes-general.md#dv2-series) met snellere processors, SSD-opslag en dubbele geheugen-core-verhouding vergeleken met **standaard** laag. **PremiumV2** biedt ook ondersteuning voor hogere schaal via toegenomen exemplaren terwijl u nog steeds de geavanceerde mogelijkheden gevonden in de standaard-plan. Alle functies die beschikbaar zijn in de bestaande **Premium** laag zijn opgenomen in **PremiumV2**.
>
> Net als bij andere speciale lagen, drie VM-grootten zijn beschikbaar voor deze laag:
>
> - Kleine (één CPU-kern, 3.5 GiB geheugen) 
> - Normaal (twee CPU-kernen, 7 GiB geheugen) 
> - Grote (vier CPU-kernen, 14 GiB geheugen)  
>
> Voor **PremiumV2** prijsgegevens Zie [prijzen van App Service](https://azure.microsoft.com/pricing/details/app-service/).
>
> Aan de slag met het nieuwe **PremiumV2** prijscategorie, Zie [PremiumV2 configureren-laag voor App Service](app-service-configure-premium-tier.md).

## <a name="how-does-my-app-run-and-scale"></a>Hoe mijn app voeren en te schalen?

In de **vrije** en **gedeelde** lagen, een app ontvangt CPU minuten op een gedeeld exemplaar van de virtuele machine en kan niet worden uitgebreid. In andere categorieën een app wordt uitgevoerd en wordt als volgt worden geschaald.

Wanneer u een app in App Service maakt, wordt deze geplaatst in een App Service-abonnement. Wanneer de app wordt uitgevoerd, wordt deze uitgevoerd op alle VM-instanties die worden geconfigureerd in de App Service-abonnement. Als meerdere apps zich in dezelfde App Service-abonnement, delen ze allemaal dezelfde VM-exemplaren. Als u meerdere implementatiesites voor een app hebt, worden alle implementatiesites ook uitvoeren op dezelfde virtuele machine-exemplaren. Als u logboeken met diagnostische gegevens inschakelen, back-ups uitvoeren of webtaken worden uitgevoerd, ze ook gebruiken CPU-cycli en geheugen voor deze VM-exemplaren.

De App Service-abonnement is op deze manier kunnen de schaaleenheid van de App Service-apps. Als het plan is geconfigureerd voor het uitvoeren van vijf VM-instanties, vervolgens uitvoeren alle apps in het plan op alle vijf exemplaren. Als het plan is geconfigureerd voor automatisch schalen, en vervolgens alle apps in het plan samen worden uitgebreid gebaseerd op de instellingen voor automatisch schalen.

Zie voor informatie over het uitbreiden van een app, [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="cost"></a>

## <a name="how-much-does-my-app-service-plan-cost"></a>Wat kost mijn App Service-abonnement?

Deze sectie beschrijft hoe App Service-apps worden gefactureerd. Zie voor gedetailleerde, regiospecifieke prijsinformatie [prijzen van App Service](https://azure.microsoft.com/pricing/details/app-service/).

Met uitzondering van **vrije** laag, een App Service-abonnement voert een kosten per uur op de rekenresources die wordt gebruikt.

- In de **gedeelde** laag, elke app ontvangt een quotum van minuten CPU, dus _elke app_ wordt in rekening gebracht per uur voor de CPU-quotum.
- In de toegewezen compute lagen (**Basic**, **standaard**, **Premium**, **PremiumV2**), de App Service-abonnement definieert het aantal VM exemplaren de apps worden geschaald, zodat _elke VM-instantie_ in App Service plan heeft een per uur kosten. Deze VM-exemplaren in rekening worden gebracht de dezelfde ongeacht hoeveel apps worden uitgevoerd op deze. Om te voorkomen onverwachte kosten, Zie [opschonen van een App Service-plan](app-service-plan-manage.md#delete).
- In de **geïsoleerd** laag, de App-serviceomgeving definieert het aantal geïsoleerde werknemers die uw apps worden uitgevoerd en _elke werknemer_ per uur in rekening wordt gebracht. Er is bovendien een per uur startkosten voor de actieve de App-serviceomgeving zelf. 
- (Alleen azure functies) De **verbruik** laag dynamisch toegewezen VM-instanties om de werkbelasting van een functie-app service en wordt in rekening gebracht dynamisch per seconde door Azure. Zie voor meer informatie [prijzen van Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

U niet ophalen in rekening gebracht voor het gebruik van de App Service-functies die beschikbaar zijn voor u (configureren van aangepaste domeinen, SSL-certificaten, implementatiesites, back-ups, enz.). De uitzonderingen zijn:

- App Service-domeinen - betaalt u bij de aankoop van een in Azure en wanneer u deze elk jaar vernieuwen.
- Certificaten van App Service - betaalt u bij de aankoop van een in Azure en wanneer u deze elk jaar vernieuwen.
- SSL-verbindingen op basis van IP - daar het een uur kosten voor elke SSL op basis van IP-verbinding, maar sommige **standaard** servicetier of hoger kunt u één IP-gebaseerde SSL-verbinding gratis. SSL-verbindingen op basis van SNI zijn gratis.

> [!NOTE]
> Als u App Service met een andere Azure-service integreert, moet u mogelijk rekening houden met de kosten van deze andere services. Bijvoorbeeld, als u Azure Traffic Manager geografisch gezien schalen van uw app in Azure Traffic Manager ook kosten u op basis van uw gebruik. Zie voor een schatting van de kosten voor cross-services in Azure, [prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/). 
>
>

## <a name="what-if-my-app-needs-more-capabilities-or-features"></a>Wat gebeurt er als mijn app meer mogelijkheden of functies nodig?

Uw App Service-abonnement kan omhoog en omlaag op elk gewenst moment worden uitgebreid. Het is net zo eenvoudig als het wijzigen van de prijscategorie van het plan. U kunt een lagere prijscategorie eerst kiezen en later opschalen wanneer u meer App Service-functies nodig.

Bijvoorbeeld, u kunt starten voor het testen van uw web-app in een **vrije** App Service plan en betaalt nothing. Als u wilt toevoegen uw [aangepaste DNS-naam](app-service-web-tutorial-custom-domain.md) naar de web-app, net uw abonnement schalen tot **gedeelde** laag. Later, wanneer u wilt toevoegen een [aangepaste SSL-certificaat](app-service-web-tutorial-custom-ssl.md), uw abonnement maximaal schalen **Basic** laag. Als u wilt hebben [faseringsomgevingen](web-sites-staged-publishing.md), maximaal schalen **standaard** laag. Wanneer u meer kernen, geheugen of opslag nodig hebt, kunt u opschalen naar een grotere VM-grootte in dezelfde categorie.

Dezelfde werkt in de omgekeerde volgorde. Wanneer u van mening bent dat u niet langer moet de functies of onderdelen van een hogere laag, moet u met een lagere tier die u geld bespaart omlaag schalen.

Zie voor informatie over het schalen van het App Service-plan, [een app in Azure opschalen](web-sites-scale.md).

Als uw app zich in dezelfde App Service-abonnement met andere apps, kunt u de app om prestaties te verbeteren door te isoleren van de rekenresources. U kunt dit doen door de app te verplaatsen naar een afzonderlijke App Service-abonnement. Zie voor meer informatie [een app te verplaatsen naar een ander App Service-plan](app-service-plan-manage.md#move).

## <a name="should-i-put-an-app-in-a-new-plan-or-an-existing-plan"></a>Moet ik een app plaatsen in een nieuw plan of een bestaand abonnement?

Omdat u uw App Service-abonnement voor computermiddelen betaalt toewijst (Zie [wat kost mijn App Service-abonnement?](#cost)), mogelijk kunt u geld besparen door meerdere apps in een App Service-abonnement te stellen. U kunt blijven apps toevoegen aan een bestaand abonnement zolang het plan heeft onvoldoende bronnen om de belasting te verwerken. Houd echter rekening mee dat apps in hetzelfde App Service plan dat alle delen dezelfde bronnen berekenen. Om te bepalen dat of de nieuwe app de benodigde resources heeft, moet u de capaciteit van de bestaande App Service-abonnement en de verwachte belasting voor de nieuwe app begrijpen. Een App Service-plan overbelasting kan veroorzaken uitvaltijd voor uw nieuwe en bestaande apps.

Isolaat uw app in een nieuwe App Service-plan wanneer:

- De app is een bronintensieve.
- Wilt u de bestaande abonnement voor de app onafhankelijk van andere apps schalen.
- De app moet resource in een andere geografische regio.

Op deze manier kunt u een nieuwe set bronnen toewijzen voor uw app en meer controle over uw apps te krijgen.

## <a name="manage-an-app-service-plan"></a>Een App Service-abonnement beheren

> [!div class="nextstepaction"]
> [Een App Service-abonnement beheren](app-service-plan-manage.md)
