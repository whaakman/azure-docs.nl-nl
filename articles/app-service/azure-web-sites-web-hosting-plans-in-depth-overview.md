---
title: App Service-plannen in Azure App Service Web Apps | Microsoft Docs
description: Lees hoe App Service-plannen voor Azure App Service werk, en hoe ze uw beheerervaring profiteren.
keywords: App Service, Azure App Service, schaal, schaalbaar, App Service-abonnement, kosten App Service
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: fb5b782f09bdd8c8a862eddfbd65b0f86ef8d08c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="app-service-plans-in-azure-app-service-web-apps"></a>App Service-abonnementen in Azure App Service Web Apps

App Service-abonnementen staan voor de verzameling van fysieke resources die worden gebruikt voor het hosten van uw apps.

In App Service-plannen wordt het volgende gedefinieerd:

- Regio (VS-West, VS-Oost, enz.)
- Schaalaanpassingsaantal (een, twee, drie exemplaren, enz.)
- De exemplaargrootte van het (klein, normaal, groot)
- SKU (gratis gedeeld, Basic, Standard, Premium, PremiumV2, geïsoleerd)

Web-Apps, Mobile Apps, API-Apps, Apps van de functie (of functies), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) alle uitvoeren in een App Service-plan.  Apps in hetzelfde abonnement, en dezelfde regio kunnen delen van een App Service-abonnement. 

Alle toepassingen die zijn toegewezen aan een **App Service-abonnement** de bronnen die zijn gedefinieerd door het delen. Deze delen, bespaart geld bij het hosten van meerdere apps in een enkele App Service-abonnement.

Uw **App Service-abonnement** kunt opschalen van **vrije** en **gedeelde** lagen naar **Basic**, **standaard**,  **Premium**, en **geïsoleerd** lagen. Elke hogere lagen hebt u toegang tot meer bronnen en functies.

Als uw App Service-abonnement is ingesteld op **Basic** servicetier of hoger, klikt u vervolgens kunt u bepalen de **grootte** en telling van de virtuele machines worden geschaald.

Bijvoorbeeld, als uw plan is geconfigureerd voor gebruik van twee 'kleine' exemplaren in de **standaard** laag, alle apps in dat plan uitgevoerd voor beide exemplaren. Apps hebben toegang tot de **standaard** servicetier functies. Plan exemplaren apps waarop zijn volledig beheerde en maximaal beschikbaar.

> [!IMPORTANT]
> De prijscategorie (SKU) van de App Service-abonnement bepaalt de kosten en niet het aantal apps die erin worden gehost.

Dit artikel bevat informatie over de belangrijkste kenmerken van een App Service-abonnement, zoals de prijzen voor lagen en schaal en hoe deze werken bij het beheren van uw apps.

## <a name="new-pricing-tier-premiumv2"></a>Nieuwe prijscategorie: PremiumV2

De nieuwe **PremiumV2** prijscategorie biedt [Dv2-serie VMs](../virtual-machines/windows/sizes-general.md#dv2-series) met snellere processors, SSD-opslag en dubbele geheugen-core-verhouding vergeleken met **standaard** laag. **PremiumV2** biedt ook ondersteuning voor hogere schaal via toegenomen exemplaren terwijl u nog steeds de geavanceerde mogelijkheden gevonden in de standaard-plan. Alle functies die beschikbaar zijn in de bestaande **Premium** laag zijn opgenomen in **PremiumV2**.

Net als bij andere speciale lagen, drie VM-grootten zijn beschikbaar voor deze laag:

- Kleine (1 CPU-kernen, 3.5 GiB geheugen) 
- Normaal (2 CPU-kernen, 7 GiB geheugen) 
- Grote (4 CPU-kernen, 14 GiB geheugen)  

Voor **PremiumV2** prijsgegevens Zie [prijzen van App Service](/pricing/details/app-service/).

Aan de slag met het nieuwe **PremiumV2** prijscategorie, Zie [PremiumV2 configureren-laag voor App Service](app-service-configure-premium-tier.md).

## <a name="apps-and-app-service-plans"></a>Apps en App Service-abonnementen

Een app in App Service kan worden gekoppeld aan slechts één App Service-abonnement op elk moment.

Zowel apps als abonnementen zijn opgenomen in een **resourcegroep**. Een resourcegroep fungeert als de grens van de levenscyclus voor elke bron die binnen het. U kunt resourcegroepen gebruiken voor het beheren van alle onderdelen van een toepassing samen.

Omdat één resourcegroep meerdere App Service-abonnementen hebt kan, kunt u verschillende apps voor verschillende fysieke resources kunt toewijzen.

U kunt bijvoorbeeld resources tussen de ontwikkeling, testen en productie-omgevingen scheiden. Met afzonderlijke omgevingen voor productie en ontwikkelen en testen, kunt u resources isoleren. Op deze manier gaat testen op basis van een nieuwe versie van uw apps load niet concurreren voor dezelfde bronnen als uw productie-apps, die echte klanten fungeren.

Wanneer u meerdere abonnementen in één resourcegroep hebt, kunt u ook een toepassing die geografische regio's omvat definiëren.

Bijvoorbeeld: een maximaal beschikbare app uitgevoerd in twee gebieden bevat ten minste twee plannen, één voor elke regio en een app die is gekoppeld aan elk plan. In dit geval worden alle exemplaren van de app vervolgens opgenomen in één resourcegroep. Met een resourcegroep met meerdere abonnementen en meerdere apps gemakkelijk te beheren, te beheren en bekijk de status van de toepassing.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Maken van een App Service-abonnement of bestaande gebruiken

Wanneer u een nieuwe Web-App in App Service, kunt u hosting resources delen door het plaatsen van de app in een bestaand App Service-abonnement. Om te bepalen of de nieuwe app over de benodigde resources beschikt, moet u de capaciteit van de bestaande App Service-abonnement en de verwachte belasting voor de nieuwe app begrijpen. Te veel bronnen toewijzen kan mogelijk leiden tot uitvaltijd voor uw nieuwe en bestaande apps.

Het is raadzaam het isoleren van uw app in een nieuwe App Service plannen wanneer:

- App is een bronintensieve.
- App heeft verschillende schalen factoren van de andere apps die worden gehost in een bestaand abonnement.
- App moet resource in een andere geografische regio.

Op deze manier kunt u een nieuwe set bronnen toewijzen voor uw app en meer controle over uw apps te krijgen.

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken

> [!TIP]
> Als u een App Service-omgeving hebt, raadpleegt u [maken van een App Service-abonnement in een App-serviceomgeving](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

U kunt een lege App Service-abonnement maken of als onderdeel van het maken van de app.

In de [Azure-portal](https://portal.azure.com), klikt u op **nieuw** > **Web en mobiel**, en selecteer vervolgens **Web-App** of een ander type App Service-app.

![Een app maken in de Azure-portal.][createWebApp]

Vervolgens kunt u selecteren of maken van de App Service-plan voor de nieuwe app.

 ![Maak een App Service-abonnement.][createASP]

Als u wilt een App Service-abonnement maken, klikt u op **[+] maken nieuwe**, type de **App Service-abonnement** een naam en selecteer vervolgens een geschikte **locatie**. Klik op **prijscategorie**, en selecteer vervolgens een juiste prijscategorie voor de service. Selecteer **weergeven van alle** naar de weergave meer opties, zoals prijzen **vrije** en **gedeelde**. Nadat u de prijscategorie hebt geselecteerd, klikt u op de **Selecteer** knop.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Een app verplaatsen naar een ander App Service-abonnement

U kunt een app verplaatsen naar een andere App Service-plan in de [Azure-portal](https://portal.azure.com). U kunt apps verplaatsen tussen plannen zolang de abonnementen zijn de _dezelfde resourcegroep en de geografische regio_.

Een app verplaatsen naar een ander abonnement:

- Navigeer naar de app die u wilt verplaatsen.
- In de **Menu**, zoekt u naar de **App Service-Plan** sectie.
- Selecteer **wijziging App Service-abonnement** om het proces te starten.

**App Service-abonnement wijzigen** Hiermee opent u de **App Service-abonnement** selector. Op dit moment kunt u een bestaand abonnement verplaatsen van deze app in. Alleen plannen in dezelfde resourcegroep en regio worden weergegeven.

![Selector van App Service-plan.][change]

Elk plan heeft zijn eigen prijscategorie. Bijvoorbeeld, een site te verplaatsen van een gratis laag naar een Standard-laag, kunt u alle apps die zijn toegewezen aan het gebruik van de functies en bronnen van de Standard-laag.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonen van een app naar een andere App Service-plan

Als u wilt verplaatsen van de app naar een andere regio, een alternatief is app klonen. Klonen wordt een kopie van uw app in een nieuw of bestaand App Service-abonnement in elke regio.

U vindt **kloon App** in de **ontwikkelingsprogramma's** gedeelte van het menu.

> [!IMPORTANT]
> Klonen, is enkele beperkingen die u op over kunt lezen [Azure App Service-App klonen](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Schalen van een App Service-abonnement

Er zijn drie manieren om een abonnement schalen:

- **Wijzig het plan de prijscategorie**. Een plan in de basisstaffel kan worden geconverteerd naar Standard en alle apps die zijn toegewezen om aan de functies van de prijscategorie Standard gebruiken.
- **Wijzig de exemplaargrootte van de planning**. Als u bijvoorbeeld kunt een plan in de basisstaffel die gebruikmaakt van kleine exemplaren voor het gebruik van grote exemplaren worden gewijzigd. Alle apps die gekoppeld aan een plan nu zijn kunnen gebruiken voor de extra geheugen en CPU-resources die de grotere exemplaargrootte biedt.
- **Wijzigen van de exemplaren van het plan**. Bijvoorbeeld, kan een standaard-plan dat wordt uitgebreid naar drie exemplaren worden geschaald tot 10 exemplaren. Premium-abonnement kan worden uitgebreid naar 20 exemplaren (afhankelijk van beschikbaarheid). Alle apps die gekoppeld aan een plan nu zijn kunnen gebruiken voor de extra geheugen en CPU-bronnen die het groter aantal exemplaren biedt.

U kunt de prijscategorie laag en de exemplaar-grootte wijzigen door te klikken op de **omhoog schalen** item onder instellingen voor de app of het App Service-abonnement. Wijzigingen toepassen op de App Service-abonnement en van invloed zijn op alle apps die deze als host fungeert.

 ![Waarden voor een app opschalen instellen.][pricingtier]

## <a name="app-service-plan-cleanup"></a>App Service plan opschonen

> [!IMPORTANT]
> **App Service-plannen** waarvoor geen apps die zijn gekoppeld aan hen nog steeds kosten omdat ze nog steeds de rekencapaciteit reserveren.

Om te voorkomen onverwachte kosten wanneer de laatste app, gehost in een App Service-abonnement wordt verwijderd, is de resulterende lege App Service-abonnement standaard ook verwijderd.

## <a name="summary"></a>Samenvatting

App Service-abonnementen vertegenwoordigen een reeks functies en mogelijkheden die u in uw apps delen kunt. App Service-plannen bieden u de flexibiliteit om specifieke apps aan een set resources toewijzen en verder optimaliseren het gebruik van uw Azure-resource. Op deze manier als u geld wilt besparen op de testomgeving kunt u een plan voor meerdere apps delen. U kunt ook doorvoer voor uw productieomgeving maximaliseren door schalen in meerdere regio's en plannen.

## <a name="whats-changed"></a>Wat is er gewijzigd

- Zie voor een handleiding voor het wijzigen van Websites in App Service,: [Azure App Service en de invloed ervan op bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
