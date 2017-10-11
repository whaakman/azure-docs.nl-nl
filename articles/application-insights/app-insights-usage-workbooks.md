---
title: Onderzoek en gebruiksgegevens delen met interactieve werkmappen in Azure Application Insights | Microsoft docs
description: Demografische analyse van gebruikers van uw web-app.
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: 75028b4fbda43d90f56690a33c7eb624fce049c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Onderzoek en gebruiksgegevens delen met interactieve werkmappen in Application Insights

Werkmappen combineren [Azure Application Insights](app-insights-overview.md) gegevensvisualisaties, [analysequery's](app-insights-analytics.md), en tekst in de interactieve documenten. Werkmappen kunnen worden bewerkt door andere teamleden met toegang tot de dezelfde Azure-resource. Dit betekent dat de query's en besturingselementen die worden gebruikt voor het maken van een werkmap zijn beschikbaar voor andere mensen lezen van de werkmap, zodat ze gemakkelijk te verkennen, uitbreiden en controleren op fouten.

Werkmappen worden gebruikt voor scenario's zoals:

* Het gebruik van uw app verkennen wanneer u niet de metrische gegevens van belang van tevoren weet: aantal gebruikers, retentie, conversie tarieven, enzovoort. In tegenstelling tot andere gebruik hulpprogramma's voor webanalyse in Application Insights kunnen werkmappen u meerdere soorten visualisaties en analyses, waardoor ze ideaal voor dit soort vrije vorm exploratie combineren.
* Uitleg over naar uw team hoe een nieuw uitgebrachte functie wordt uitgevoerd, door de gebruiker weergegeven aantallen voor belangrijke interacties en andere metrische gegevens.
* Delen van de resultaten van een A / B-experiment in uw app met andere leden van uw team. U kunt uitgelegd van de doelstellingen voor het experiment met tekst en vervolgens elk gebruik van metrische gegevens en Analytics query gebruikt voor het evalueren van het experiment, samen met duidelijke call-outs voor of elke metriek boven - of hieronder-doel is weergeven.
* De impact van een storing rapportage over het gebruik van uw app, het combineren van gegevens, tekstuitleg en een beschrijving van de volgende stappen om te voorkomen dat uitval in de toekomst.

> [!NOTE]
> Uw Application Insights-resource moet bevatten paginaweergaven of aangepaste gebeurtenissen werkmappen gebruiken. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Bewerken, rangschikken, klonen en secties van de werkmap verwijderen

Is een werkmap een en-klare van secties: onafhankelijk bewerkbare gebruik visualisaties, grafieken, tabellen, tekst of Analytics queryresultaten.

De inhoud van een sectie werkmap bewerken, klikt u op de **bewerken** knop hieronder en naar rechts van de werkmap-sectie.

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/editing-controls.png)

1. Wanneer u klaar bent u een sectie bewerkt, klikt u op **gedaan bewerken** in de linkerbenedenhoek van het gedeelte.

2. Een duplicaat van een sectie toevoegen, klikt u op de **klonen in deze sectie** pictogram. Dubbele secties maken is een geweldige naar manier om een query zonder verlies van vorige iteraties herhalen.

3. Als u wilt verplaatsen van een sectie in een werkmap, klikt u op de **omhoog** of **omlaag** pictogram.

4. Een sectie als permanent wilt verwijderen, klikt u op de **verwijderen** pictogram.

## <a name="adding-usage-data-visualization-sections"></a>Gebruik gegevensvisualisatie secties toevoegen

Werkmappen bieden vier typen ingebouwde gebruik analytics visualisaties. Alle antwoorden op een algemene vraag over het gebruik van uw app. Tabellen en grafieken dan deze vier secties wilt toevoegen, voeg Analytics query secties (Zie hieronder).

Voor het toevoegen van gebruikers, sessies, gebeurtenissen of bewaren sectie met uw werkmap, gebruik de **gebruikers toevoegen** of andere bijbehorende knop aan de onderkant van de werkmap of aan de onderkant van een willekeurig gedeelte.

![De sectie gebruikers in werkmappen](./media/app-insights-usage-workbooks/users-section.png)

**Gebruikers** secties beantwoorden 'hoeveel gebruikers een pagina weergegeven of gebruikt een functie van Mijn site?'

**Sessies** secties beantwoorden 'hoeveel sessies gebruikers hoeven te besteden aan een pagina te bekijken of met bepaalde functie van Mijn site?'

**Gebeurtenissen** secties beantwoorden ' hoe vaak gebruikers bepaalde pagina bekijken of bepaalde functie van Mijn site gebruiken?'

Elk van deze drie sectietypen biedt dezelfde set besturingselementen en visualisaties:

* [Meer informatie over het bewerken van gebruikers, sessies en gebeurtenissen secties](app-insights-usage-segmentation.md)
* Uitschakelen van de belangrijkste grafiek, histogram rasters, automatische insights en voorbeeld gebruikers visualisaties met behulp van de **grafiek weergeven**, **raster weergeven**, **Insights weergeven**, en **voorbeeld van deze gebruikers** selectievakjes aan het begin van elke sectie.

![De sectie bewaren in werkmappen](./media/app-insights-usage-workbooks/retention-section.png)

**Bewaartermijn** secties beantwoorden "Mensen die een pagina weergegeven of bepaalde functie op een dag of week gebruikt, hoeveel afkomstig terug in de volgende dag of week?"

* [Meer informatie over het bewerken van retentie secties](app-insights-usage-retention.md)
* Overschakelen de optionele algehele bewaren grafiek met de **weergeven algehele bewaren grafiek** selectievakje aan de bovenkant van de sectie.

## <a name="adding-application-insights-analytics-sections"></a>Application Insights Analytics secties toevoegen

![Analytics-sectie in werkmappen](./media/app-insights-usage-workbooks/analytics-section.png)

U kunt een gedeelte van de query Application Insights Analytics met uw werkmap toevoegen met de **toevoegen Analytics query** knop aan de onderkant van de werkmap of aan de onderkant van een willekeurig gedeelte.

Analytics query secties kunnen u het toevoegen van willekeurige query's via uw Application Insights-gegevens in werkmappen. Deze flexibiliteit betekent Analytics query secties zijn uw Ga om voor het beantwoorden van vragen over uw site dan de vier bovenstaande voor gebruikers, sessies, gebeurtenissen en retentie, zoals:

* Het aantal uitzonderingen uw site throw tijdens dezelfde periode als een daling in gebruik
* Wat is de distributie van laadtijden voor pagina's voor gebruikers die bepaalde pagina bekijken?
* Hoeveel gebruikers bepaalde set pagina's weergegeven op uw site, maar niet in een andere set pagina's? Dit is handig om te begrijpen als u clusters van gebruikers die gebruikmaken van verschillende subsets van functionaliteit van uw site hebt (Gebruik de `join` operator met de `kind=leftanti` aanpassingsfunctie in de Log Analytics query language).

Gebruik de [Log Analytics query language reference](https://docs.loganalytics.io/) voor meer informatie over het schrijven van query's.

## <a name="adding-text-and-markdown-sections"></a>Tekst- en Markdown secties toe te voegen

Helpt zet koppen, uitleg en commentaar toe te voegen aan uw werkmappen een set van tabellen en grafieken in sprekersnotities. Tekst secties in ondersteuning voor werkmappen de [Markdown-syntaxis](https://daringfireball.net/projects/markdown/) voor tekstopmaak, zoals kopteksten, vet, cursief en lijsten met opsommingstekens.

Als u wilt een tekstsectie toevoegen aan uw werkmap, gebruiken de **tekst toevoegen** knop aan de onderkant van de werkmap of aan de onderkant van een willekeurig gedeelte.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Opslaan en delen van werkmappen met uw team

Werkmappen zijn opgeslagen in een Application Insights-resource in de **mijn rapporten** persoonlijke in u of in de sectie de **gedeeld rapporten** sectie die toegankelijk is voor iedereen met toegang tot de Application Insights-resource. U kunt de werkmappen in de resource op de **Open** knop in de actiebalk.

Voor het delen van een werkmap die momenteel **mijn rapporten**:

1. Klik op **Open** in de actiebalk
2. Klik op de knop '...' naast de werkmap die u wilt delen
3. Klik op **verplaatsen naar de gedeelde rapporten**.

Als u wilt delen een werkmap met een koppeling of via e-mail, klikt u op **delen** in de actiebalk. Houd er rekening mee dat ontvangers van de koppeling moeten toegang hebben tot deze resource in de Azure portal om de werkmap weer te geven. Als u wilt bewerken, ontvangers moeten minimaal Inzender-rechten voor de resource.

Een koppeling naar een werkmap in een Azure-Dashboard vastmaken:

1. Klik op **Open** in de actiebalk
2. Klik op de knop '...' naast de werkmap die u wilt vastmaken
3. Klik op **vastmaken aan dashboard**.

## <a name="next-steps"></a>Volgende stappen

## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen ervaringen gebruik, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u al aangepaste gebeurtenissen of paginaweergaven verzendt, gebruik de informatie over het gebruik hulpprogramma's voor meer informatie over hoe gebruikers gebruiken voor uw service.
    - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)
    
