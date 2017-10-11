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
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d17ed3dff08f00a1d6a2108608e42b29f95fbd84
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Gebruiker navigatiepatronen met een gebruiker in Application Insights loopt analyseren

![Hulpprogramma voor Application Insights gebruiker stroomt](./media/app-insights-usage-flows/flows.png)

Het hulpprogramma gebruiker loopt visualiseren hoe gebruikers navigeren tussen de pagina's en onderdelen van uw site. Het is ideaal voor het beantwoorden van vragen als:
* Hoe gebruikers navigeren vanuit een pagina op uw site?
* Wat gebruikers klikken op een pagina op uw site?
* Waar zijn de plaatsen die gebruikers meest verloop van uw site?
* Zijn er plaatsen waar gebruikers de herhalen dezelfde steeds?

Het hulpprogramma loopt van de gebruiker wordt gestart van een weergave van de eerste pagina of de gebeurtenis die u opgeeft. Deze pagina of aangepaste gebeurtenis gezien, ziet loopt van de gebruiker u de paginaweergaven en aangepaste gebeurtenissen die gebruikers verzonden onmiddellijk daarna twee tijdens een sessie daarna, enzovoort stappen. Regels verschillende dikte van het aantal keren dat elk pad werd gevolgd door gebruikers worden weergegeven. Speciale knooppunten 'Sessie beëindigd' weergegeven hoeveel gebruikers verzonden geen paginaweergaven of aangepaste gebeurtenissen na de vorige knooppunt markeren waar gebruikers waarschijnlijk was uw site.



> [!NOTE]
> Uw Application Insights-resource moet bevatten paginaweergaven of aangepaste gebeurtenissen met het hulpprogramma loopt van de gebruiker. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Gestart door een weergave van de eerste pagina of aangepaste gebeurtenis te kiezen

![Kies een eerste gebeurtenis voor gebruiker stroomt](./media/app-insights-usage-flows/flows-initial-event.png)

Kies een weergave van de eerste pagina of aangepaste gebeurtenis moeten fungeren als het startpunt voor de visualisatie om te beginnen het beantwoorden van vragen met het hulpprogramma loopt van gebruiker:
1. Klik op de koppeling in de ' wat gebruikers doen na...? ' titel of klik op de knop bewerken. 
2. Selecteer een paginaweergave of aangepaste gebeurtenis uit de vervolgkeuzelijst 'Initiële vastleggen'.
3. Klik op 'Grafiek maken'.

De kolom "Stap 1" van de visualisatie wordt weergegeven wat gebruikers gedaan meest direct na de eerste gebeurtenis, boven naar beneden van meest naar minst-behaalde gerangschikt. De 'Stap 2' en de volgende kolommen weergegeven wat gebruikers gedaan daarna maken van een afbeelding van alle gebruikers manieren bent gegaan door de site.

Standaard voorbeelden voor het hulpprogramma gebruiker loopt willekeurig alleen de afgelopen 24 uur van paginaweergaven en aangepaste gebeurtenis van uw site. U kunt het tijdsbereik verhogen en wijzigen van het saldo van prestaties en de nauwkeurigheid van steekproeven in het menu Bewerken.

Als het aantal paginaweergaven en aangepaste gebeurtenissen niet relevant voor u, klikt u op 'X' op de knooppunten die u wilt verbergen. Wanneer u de knooppunten die u wilt verbergen hebt geselecteerd, klikt u op de knop 'Grafiek maken' onder de visualisatie. Om te zien met alle knooppunten hebt verborgen, klikt u op de knop bewerken en bekijk het gedeelte 'Uitgesloten gebeurtenissen'.

Als paginaweergaven of aangepaste gebeurtenissen die ontbreken u verwacht dat op de visualisatie:
* Controleer het gedeelte 'Uitgesloten gebeurtenissen' in het menu Bewerken.
* Het besturingselement 'Detailniveau' in het menu Bewerken minder frequent gebeurtenissen worden opgenomen in de visualisatie gebruiken.
* Verhoog het tijdsbereik van de visualisatie in het menu Bewerken als de pagina of aangepaste gebeurtenis die u verwacht zelden wordt verzonden door gebruikers.
* Zorg ervoor dat de pagina weergeven of aangepaste gebeurtenis die u verwacht dat is ingesteld om te worden verzameld door de Application Insights-SDK in de broncode van uw site. [Meer informatie over het verzamelen van aangepaste gebeurtenissen.](app-insights-api-custom-events-metrics.md)

Als u meer stappen in de visualisatie zien wilt, gebruikt u de schuifregelaar 'Het aantal stappen' in het menu Bewerken.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Nadat u hebt een pagina of het onderdeel bezocht, waarbij gebruikers gaan en wat ze op?

![Gebruik van de gebruiker stromen om te zien waarop gebruikers klikken op](./media/app-insights-usage-flows/flows-one-step.png)

Als uw eerste gebeurtenis een paginaweergave is, is de eerste kolom ('stap 1) van de visualisatie een snelle manier om te begrijpen wat gebruikers is onmiddellijk na de pagina te bezoeken. Probeer uw site openen in een venster naast de visualisatie loopt van de gebruiker. Vergelijk de verwachtingen van hoe gebruikers met de pagina aan de lijst met gebeurtenissen in de kolom 'Stap 1 interacteren'. Vaak het geval is, kan een UI-element op de pagina die lijkt te verwaarlozen naar uw team zijn tussen de meest gebruikte op de pagina. Het kan een goed startpunt voor ontwerp verbeteringen aan de site zijn.

Als uw eerste gebeurtenis een aangepaste gebeurtenis is, toont de eerste kolom gebruikers heeft na het uitvoeren van deze actie. Net als bij paginaweergaven Overweeg of het waargenomen gedrag van uw gebruikers, overeenkomt met de doelstellingen en de verwachtingen van uw team. Bijvoorbeeld zoeken om te zien als 'Ga naar Afhandeling' en 'Aankoop voltooid' wordt weergegeven in de visualisatie kort nadien als de geselecteerde eerste gebeurtenis 'Toegevoegd Item aan winkelwagen' is. Als gebruikersgedrag veel van uw verwachtingen verschilt, gebruikt u de visualisatie om te begrijpen hoe gebruikers krijgen 'afgevangen' door de huidige ontwerp van uw site.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Waar zijn de plaatsen die gebruikers meest verloop van uw site?

Bekijk voor 'Sessie beëindigd' knooppunten die hoge-up worden weergegeven in een kolom in de visualisatie, met name vroeg in een stroom. Dit betekent dat veel gebruikers waarschijnlijk churned van uw site nadat u het voorgaande pad van pagina's en interacties van de gebruikersinterface. Soms verloop - na het voltooien van bijvoorbeeld een aankoop op een e-commerce-site - wordt verwacht maar verloop is meestal een teken van problemen met het ontwerp, slechte prestaties of andere problemen met de site die kan worden verbeterd.

Houd er rekening mee dat 'sessie beëindigd' knooppunten alleen worden gebaseerd op telemetrie die is verzameld door deze Application Insights-resource. Als Application Insights heeft ontvangen telemetrie voor bepaalde gebruikersinteracties, kunnen gebruikers nog steeds hebt uitgevoerd in uw site op deze manieren nadat het hulpprogramma gebruiker loopt vertelt de sessie beëindigd.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Zijn er plaatsen waar gebruikers de herhalen dezelfde steeds?

Zoek naar een paginaweergave of aangepaste gebeurtenis die door veel gebruikers via de volgende stappen in de visualisatie wordt herhaald. Dit betekent meestal dat gebruikers herhalende taken op uw site uitvoeren wilt. Als u herhaling vinden, kunt u zien over het wijzigen van het ontwerp van uw site of het toevoegen van nieuwe functionaliteit herhaling verminderen. Bijvoorbeeld, toe te voegen bulksgewijs bewerkingsfunctionaliteit als u gebruikers, herhalende taken uitvoeren voor elke rij van een table-element vinden.

## <a name="common-questions"></a>Veelgestelde vragen

### <a name="why-do-steps-appear-disconnected"></a>Waarom worden stappen verbroken weergegeven?

![Gebruiker-stromen met niet-verbonden stappen](./media/app-insights-usage-flows/flows-disconnected.png)

Als de verbinding van stappen (kolommen) in een visualisatie loopt van de gebruiker wordt verbroken, betekent dit geen van de paden die door gebruikers tussen de stappen zijn vaak moet worden weergegeven. Minder frequente gebeurtenissen om op te geven de visualisatie zodat de stappen worden verbonden weergegeven, pas de schuifregelaar 'Detailniveau' in het menu Bewerken.

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Komt het eerste gebeurtenis vertegenwoordigen de eerste keer dat de gebeurtenis wordt weergegeven in een sessie of elk gewenst moment wordt deze weergegeven in een sessie?

De eerste gebeurtenis op de visualisatie wordt alleen de eerste keer dat een gebruiker die in de weergave of aangepaste gebeurtenis tijdens een sessie verzonden vertegenwoordigt. Als gebruikers de eerste gebeurtenis meerdere keren in een sessie verzenden kunnen, wordt de kolom 'Stap 1' alleen ziet u hoe gebruikers zich gedragen na de *eerste* exemplaar van de eerste gebeurtenis, niet alle exemplaren.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht gebruik](app-insights-usage-overview.md)
* [Gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)
* [Retentie](app-insights-usage-retention.md)
* [Aangepaste gebeurtenissen toe te voegen aan uw app](app-insights-api-custom-events-metrics.md)