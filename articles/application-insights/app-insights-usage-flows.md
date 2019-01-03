---
title: Gebruiker navigatiepatronen met Gebruikersstromen in Azure Application Insights analyseren | Microsoft docs
description: Analyseren hoe gebruikers navigeren tussen de pagina's en functies van uw web-app.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 54155346c7f417be8beb0ced8ef825ec22a08237
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971610"
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Gebruiker navigatiepatronen met Gebruikersstromen in Application Insights analyseren

![Hulpprogramma voor Application Insights-Gebruikersstromen](./media/app-insights-usage-flows/00001-flows.png)

Het hulpprogramma Gebruikersstromen laat zien hoe gebruikers navigeren tussen de pagina's en functies van uw site. Het is erg handig voor het beantwoorden van vragen zoals:

* Hoe gebruikers navigeren vanuit een pagina op uw site?
* Wat gebruikers klikken op een pagina van uw site?
* Waar zijn de plaatsen die gebruikers verloop meest van uw site?
* Zijn er locaties waar gebruikers de dezelfde actie herhaaldelijk herhalen?

Het hulpprogramma Gebruikersstromen begint bij een eerste paginaweergave, aangepaste gebeurtenis of uitzonderingen die u opgeeft. Deze initiële gebeurtenis worden gegeven, ziet Gebruikersstromen u de gebeurtenissen die hebben plaatsgevonden vóór en daarna tijdens gebruikerssessies. Regels van verschillende dikte laten zien hoe vaak elk pad is gevolgd door gebruikers. Speciale **-sessie gestart** knooppunten weergeven wanneer een sessie in de volgende knooppunten is begonnen. **Sessie beëindigd** knooppunten tonen hoeveel gebruikers verzonden geen paginaweergaven of aangepaste gebeurtenissen na de vorige knooppunt markeren waar gebruikers waarschijnlijk bent uw site.

> [!NOTE]
> Uw Application Insights-resource moet paginaweergaven of aangepaste gebeurtenissen met het hulpprogramma Gebruikersstromen bevatten. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](../azure-monitor/app/javascript.md).
>
>

## <a name="start-by-choosing-an-initial-event"></a>Begin met het kiezen van een oorspronkelijke gebeurtenis

![Kies een oorspronkelijke gebeurtenis voor Gebruikersstromen](./media/app-insights-usage-flows/00002-flows-initial-event.png)

Kies een weergave van de eerste pagina, het aangepaste gebeurtenis of de uitzondering om te fungeren als startpunt voor de visualisatie om te beginnen met het beantwoorden van vragen met het hulpprogramma Gebruikersstromen:

1. Klik op de koppeling in de **wat doen gebruikers na...?**  titel, of klik op de **bewerken** knop.
2. Selecteer een paginaweergave, aangepaste gebeurtenis of uitzondering van de **initiële gebeurtenis** vervolgkeuzelijst.
3. Klik op **maken graph**.

De kolom "Stap 1" van de visualisatie ziet u wat gebruikers heeft vaakst direct na de eerste gebeurtenis, van boven naar beneden van meest naar zo min mogelijk gerangschikt. De "Stap 2" en de volgende kolommen weergeven wat gebruikers heeft daarna, het maken van een afbeelding van alle de manieren waarop gebruikers hebben genavigeerd via uw site.

Standaard voorbeelden van het hulpprogramma Gebruikersstromen willekeurig alleen de afgelopen 24 uur van paginaweergaven en aangepaste gebeurtenissen van uw site. U kunt het tijdsbereik vergroot en het saldo van prestaties en de nauwkeurigheid van steekproeven in het menu bewerken wijzigen.

Als het aantal paginaweergaven, aangepaste gebeurtenissen en uitzonderingen niet relevant voor u, klikt u op de **X** op de knooppunten die u wilt verbergen. Wanneer u hebt geselecteerd de knooppunten die u wilt verbergen, klikt u op de **maken graph** knop onder de visualisatie. Als u wilt zien welke van de knooppunten die u hebt verborgen, klikt u op de **bewerken** knop en kijk vervolgens naar de **uitgesloten gebeurtenissen** sectie.

Als paginaweergaven of aangepaste gebeurtenissen die ontbreekt verwacht u te zien op de visualisatie:

* Controleer de **uitgesloten gebeurtenissen** sectie de **bewerken** menu.
* Gebruik de knoppen plus op **anderen** knooppunten minder frequent gebeurtenissen worden opgenomen in de visualisatie.
* Als de paginaweergave of het aangepaste gebeurtenis die u verwacht wordt niet regelmatig worden verzonden door gebruikers, verhoog het tijdsbereik van de visualisatie in de **bewerken** menu.
* Zorg ervoor dat de paginaweergave, aangepaste gebeurtenis of een uitzondering die u verwacht, is ingesteld om te worden verzameld door de Application Insights-SDK in de broncode van uw site. [Meer informatie over het verzamelen van aangepaste gebeurtenissen.](../azure-monitor/app/api-custom-events-metrics.md)

Als u zien van meer stappen in de visualisatie wilt, gebruikt u de **vorige stappen** en **Vervolgstappen** vervolgkeuzelijsten boven de visualisatie.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Na het bezoek aan een pagina of -functie, waar gebruikers gaan en wat ze op?

![Gebruikersstromen gebruiken om te begrijpen waar gebruikers klikken op](./media/app-insights-usage-flows/00003-flows-one-step.png)

Als uw eerste gebeurtenis een paginaweergave is, is de eerste kolom ('stap 1') van de visualisatie een snelle manier om te begrijpen wat gebruikers heeft direct na de pagina. Probeer te openen van uw site in een venster naast de visualisatie van de Gebruikersstromen. Vergelijk uw verwachtingen van hoe gebruikers met de pagina aan de lijst met gebeurtenissen in de kolom 'Stap 1 interacteren'. Een UI-element op de pagina met een hoog onbelangrijke naar uw team kan vaak worden tussen de meest gebruikte op de pagina. Het kan een goed startpunt voor ontwerpverbeteringen naar uw site zijn.

Als uw eerste gebeurtenis een aangepaste gebeurtenis is, wordt de eerste kolom ziet wat gebruikers direct na deze acties heeft. Net als bij paginaweergaven, kunt u overwegen of het waargenomen gedrag van uw gebruikers overeenkomt met de doelstellingen en de verwachtingen van uw team. Als uw geselecteerde initiële gebeurtenis "Toegevoegd Item aan winkelwagen" is, bijvoorbeeld, bekijken om te zien als "Gaat u naar afhandeling" en 'Aankoop is voltooid' wordt weergegeven in de visualisatie kort daarna. Als gebruiker af van uw verwachtingen wijkt, gebruikt u de visualisatie om te begrijpen hoe gebruikers krijgen "zo" voor het huidige ontwerp van uw site.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Waar zijn de plaatsen die gebruikers verloop meest van uw site?

Bekijk voor **sessie beëindigd** knooppunten die high-up worden weergegeven in een kolom in de visualisatie, met name al vroeg in een stroom. Dit betekent dat veel gebruikers waarschijnlijk gegevensverloop van uw site nadat u het voorgaande pad van pagina's en interacties van de gebruikersinterface. Soms verloop - na het voltooien van bijvoorbeeld een aankoop op een e-commerce-site - er wordt verwacht maar verloop is meestal een teken van problemen met het ontwerp, slechte prestaties of andere problemen met de site die kan worden verbeterd.

Houd er rekening mee dat **sessie beëindigd** knooppunten zijn alleen op basis van telemetriegegevens die zijn verzameld door deze Application Insights-resource. Als Application Insights heeft ontvangen telemetrie voor bepaalde gebruikersinteracties, gebruikers kunnen nog steeds zijn aan de slag kunt met uw site op deze manieren nadat het hulpprogramma Gebruikersstromen zegt de sessie is beëindigd.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Zijn er locaties waar gebruikers de dezelfde actie herhaaldelijk herhalen?

Zoeken naar een paginaweergave of het aangepaste gebeurtenis die door veel gebruikers via de volgende stappen in de visualisatie wordt herhaald. Dit betekent doorgaans dat gebruikers terugkerende acties op uw site uitvoeren. Als u herhaling vinden, denken over het wijzigen van het ontwerp van uw site of nieuwe functionaliteit toegevoegd om te beperken de herhalingen. Bijvoorbeeld, toe te voegen functionaliteit voor bulksgewijs bewerken als u gebruikers terugkerende acties uitvoeren op elke rij van een tabelelement kunt vinden.

## <a name="common-questions"></a>Veelgestelde vragen

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Ondersteunt het initiële gebeurtenis vertegenwoordigen de eerste keer dat de gebeurtenis wordt weergegeven in een sessie of elk gewenst moment wordt deze weergegeven in een sessie?

De eerste gebeurtenis in de visualisatie wordt alleen de eerste keer dat een gebruiker die in de weergave of het aangepaste gebeurtenis tijdens een sessie verzonden vertegenwoordigt. Als gebruikers de eerste gebeurtenis meerdere keren in een sessie verzenden kunnen en vervolgens de kolom 'Stap 1' alleen ziet u hoe gebruikers zich gedragen na de *eerste* exemplaar van de eerste gebeurtenis, niet voor alle instanties.

### <a name="some-of-the-nodes-in-my-visualization-are-too-high-level-for-example-a-node-that-just-says-button-clicked-how-can-i-break-it-down-into-more-detailed-nodes"></a>Sommige van de knooppunten in de visualisatie zijn te op hoog niveau. Bijvoorbeeld, een knooppunt dat alleen "Knop op wordt geklikt." Hoe kan ik deze opsplitsen in meer gedetailleerde knooppunten?

Gebruik de **splitsen op basis van** opties in de **bewerken** menu:

1. Kies de gebeurtenis die u opsplitsen wilt de **gebeurtenis** menu.
2. Kies een dimensie in het **dimensie** menu. Bijvoorbeeld, hebt u een gebeurtenis met de naam 'Geklikt', probeert een aangepaste eigenschap genaamd 'Knop Name'.

## <a name="next-steps"></a>Volgende stappen

* [Gebruiksoverzicht](app-insights-usage-overview.md)
* [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
* [Retentie](app-insights-usage-retention.md)
* [Aangepaste gebeurtenissen toevoegen aan uw app](../azure-monitor/app/api-custom-events-metrics.md)