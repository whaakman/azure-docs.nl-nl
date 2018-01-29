---
title: Gebruiker navigatiepatronen met een gebruiker in Azure Application Insights loopt analyseren | Microsoft docs
description: Analyseren hoe gebruikers navigeren tussen de pagina's en functies van uw web-app.
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/24/2018
ms.author: mbullwin; daviste
ms.openlocfilehash: 63139bfb190959284cb5d25df1a8dd91bd7d913f
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Gebruiker navigatiepatronen met een gebruiker in Application Insights loopt analyseren

![Hulpprogramma voor Application Insights gebruiker stroomt](./media/app-insights-usage-flows/00001-flows.png)

Het hulpprogramma gebruiker loopt visualiseren hoe gebruikers navigeren tussen de pagina's en onderdelen van uw site. Het is ideaal voor het beantwoorden van vragen als:

* Hoe gebruikers navigeren vanuit een pagina op uw site?
* Wat gebruikers klikken op een pagina op uw site?
* Waar zijn de plaatsen die gebruikers meest verloop van uw site?
* Zijn er plaatsen waar gebruikers de herhalen dezelfde steeds?

Het hulpprogramma loopt van de gebruiker wordt gestart van een weergave van de eerste pagina, aangepaste gebeurtenis of uitzondering die u opgeeft. Deze eerste gebeurtenis gezien, ziet loopt van de gebruiker u de gebeurtenissen die hebben plaatsgevonden voor en daarna tijdens gebruikerssessies. Regels verschillende dikte van het aantal keren dat elk pad werd gevolgd door gebruikers worden weergegeven. Speciale **-sessie gestart** knooppunten weergeven waar de volgende knooppunten een sessie begon. **Sessie beëindigd** knooppunten weergeven hoeveel gebruikers verzonden geen paginaweergaven of aangepaste gebeurtenissen na de vorige knooppunt markeren waar gebruikers waarschijnlijk was uw site.

> [!NOTE]
> Uw Application Insights-resource moet bevatten paginaweergaven of aangepaste gebeurtenissen met het hulpprogramma loopt van de gebruiker. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](app-insights-javascript.md).
>
>

## <a name="start-by-choosing-an-initial-event"></a>Door het kiezen van een eerste gebeurtenis starten

![Kies een eerste gebeurtenis voor gebruiker stroomt](./media/app-insights-usage-flows/00002-flows-initial-event.png)

Om te beginnen met het beantwoorden van vragen met het hulpprogramma gebruiker loopt, kies een weergave van de eerste pagina, aangepaste gebeurtenis of uitzondering als het startpunt voor de visualisatie:

1. Klik op de koppeling in de **wat gebruikers doen na...?**  title of klik op de **bewerken** knop.
2. Selecteer een paginaweergave, aangepaste gebeurtenis of uitzondering vanuit de **eerste gebeurtenis** vervolgkeuzelijst.
3. Klik op **maken grafiek**.

De kolom "Stap 1" van de visualisatie wordt weergegeven wat gebruikers gedaan meest direct na de eerste gebeurtenis, van boven naar beneden van meest naar minste frequente gerangschikt. De 'Stap 2' en de volgende kolommen weergegeven wat gebruikers gedaan daarna maken van een afbeelding van alle gebruikers manieren bent gegaan door de site.

Standaard voorbeelden voor het hulpprogramma gebruiker loopt willekeurig alleen de afgelopen 24 uur van paginaweergaven en aangepaste gebeurtenis van uw site. U kunt het tijdsbereik verhogen en wijzigen van het saldo van prestaties en de nauwkeurigheid van steekproeven in het menu Bewerken.

Als het aantal paginaweergaven, aangepaste gebeurtenissen en uitzonderingen niet relevant voor u, klikt u op de **X** op de knooppunten die u wilt verbergen. Wanneer u hebt geselecteerd de knooppunten die u wilt verbergen, klikt u op de **maken grafiek** knop onder de visualisatie. Overzicht van alle knooppunten die u hebt verborgen, klikt u op de **bewerken** klik vervolgens kijken naar de **uitgesloten gebeurtenissen** sectie.

Als paginaweergaven of aangepaste gebeurtenissen die ontbreken u verwacht dat op de visualisatie:

* Controleer de **uitgesloten gebeurtenissen** sectie het **bewerken** menu.
* Gebruik de knoppen plus op **anderen** knooppunten minder frequent gebeurtenissen worden opgenomen in de visualisatie.
* Als de pagina of aangepaste gebeurtenis die u verwacht zelden wordt verzonden door gebruikers Verhoog het tijdsbereik van de visualisatie in de **bewerken** menu.
* Zorg ervoor dat de pagina weergeven, aangepaste gebeurtenis of uitzondering u verwacht dat is ingesteld om te worden verzameld door de Application Insights-SDK in de broncode van uw site. [Meer informatie over het verzamelen van aangepaste gebeurtenissen.](app-insights-api-custom-events-metrics.md)

Als u meer stappen in de visualisatie zien wilt, gebruikt u de **vorige stappen** en **Vervolgstappen** opgegeven waarin dropdowns boven de visualisatie.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Nadat u hebt een pagina of het onderdeel bezocht, waarbij gebruikers gaan en wat ze op?

![Gebruik van de gebruiker stromen om te zien waarop gebruikers klikken op](./media/app-insights-usage-flows/00003-flows-one-step.png)

Als uw eerste gebeurtenis een paginaweergave is, is de eerste kolom ('stap 1) van de visualisatie een snelle manier om te begrijpen wat gebruikers is onmiddellijk na de pagina te bezoeken. Probeer uw site openen in een venster naast de visualisatie loopt van de gebruiker. Vergelijk de verwachtingen van hoe gebruikers met de pagina aan de lijst met gebeurtenissen in de kolom 'Stap 1 interacteren'. Vaak het geval is, kan een UI-element op de pagina die lijkt te verwaarlozen naar uw team zijn tussen de meest gebruikte op de pagina. Het kan een goed startpunt voor ontwerp verbeteringen aan de site zijn.

Als uw eerste gebeurtenis een aangepaste gebeurtenis is, toont de eerste kolom gebruikers heeft na het uitvoeren van deze actie. Net als bij paginaweergaven Overweeg of het waargenomen gedrag van uw gebruikers, overeenkomt met de doelstellingen en de verwachtingen van uw team. Bijvoorbeeld zoeken om te zien als 'Ga naar Afhandeling' en 'Aankoop voltooid' wordt weergegeven in de visualisatie kort nadien als de geselecteerde eerste gebeurtenis 'Toegevoegd Item aan winkelwagen' is. Als gebruiker af van uw verwachtingen wijkt, gebruikt u de visualisatie om te begrijpen hoe gebruikers krijgen 'afgevangen' door de huidige ontwerp van uw site.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Waar zijn de plaatsen die gebruikers meest verloop van uw site?

Bekijk voor **sessie beëindigd** knooppunten die hoge-up worden weergegeven in een kolom in de visualisatie, met name vroeg in een stroom. Dit betekent dat veel gebruikers waarschijnlijk churned van uw site nadat u het voorgaande pad van pagina's en interacties van de gebruikersinterface. Soms verloop - na het voltooien van bijvoorbeeld een aankoop op een e-commerce-site - wordt verwacht maar verloop is meestal een teken van problemen met het ontwerp, slechte prestaties of andere problemen met de site die kan worden verbeterd.

Houd er rekening mee dat **sessie beëindigd** knooppunten alleen worden gebaseerd op telemetrie die is verzameld door deze Application Insights-resource. Als Application Insights heeft ontvangen telemetrie voor bepaalde gebruikersinteracties, kunnen gebruikers nog steeds hebt uitgevoerd in uw site op deze manieren nadat het hulpprogramma gebruiker loopt vertelt de sessie beëindigd.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Zijn er plaatsen waar gebruikers de herhalen dezelfde steeds?

Zoek naar een paginaweergave of aangepaste gebeurtenis die door veel gebruikers via de volgende stappen in de visualisatie wordt herhaald. Dit betekent meestal dat gebruikers herhalende taken op uw site uitvoeren wilt. Als u herhaling vinden, kunt u zien over het wijzigen van het ontwerp van uw site of het toevoegen van nieuwe functionaliteit herhaling verminderen. Bijvoorbeeld, toe te voegen bulksgewijs bewerkingsfunctionaliteit als u gebruikers, herhalende taken uitvoeren voor elke rij van een table-element vinden.

## <a name="common-questions"></a>Veelgestelde vragen

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Komt het eerste gebeurtenis vertegenwoordigen de eerste keer dat de gebeurtenis wordt weergegeven in een sessie of elk gewenst moment wordt deze weergegeven in een sessie?

De eerste gebeurtenis op de visualisatie wordt alleen de eerste keer dat een gebruiker die in de weergave of aangepaste gebeurtenis tijdens een sessie verzonden vertegenwoordigt. Als gebruikers de eerste gebeurtenis meerdere keren in een sessie verzenden kunnen, wordt de kolom 'Stap 1' alleen ziet u hoe gebruikers zich gedragen na de *eerste* exemplaar van de eerste gebeurtenis, niet alle exemplaren.

### <a name="some-of-the-nodes-in-my-visualization-are-too-high-level-for-example-a-node-that-just-says-button-clicked-how-can-i-break-it-down-into-more-detailed-nodes"></a>Sommige van de knooppunten in mijn visualisatie zijn te op hoog niveau. Bijvoorbeeld, een knooppunt dat zojuist zegt "Knop op wordt geklikt." Hoe kan ik opsplitsen in meer gedetailleerde knooppunten?

Gebruik de **gesplitst** opties in de **bewerken** menu:

1. Kies de gebeurtenis die u onderverdelen wilt de **gebeurtenis** menu.
2. Kies een dimensie in het **dimensie** menu. Als er een gebeurtenis 'Geklikt' genoemd, probeer bijvoorbeeld een aangepaste eigenschap met de naam 'Knop Name'.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht gebruik](app-insights-usage-overview.md)
* [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
* [Retentie](app-insights-usage-retention.md)
* [Aangepaste gebeurtenissen toe te voegen aan uw app](app-insights-api-custom-events-metrics.md)