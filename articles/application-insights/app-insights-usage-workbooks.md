---
title: Onderzoeken en gebruiksgegevens delen met interactieve Workbooks in Azure Application Insights | Microsoft docs
description: Demografische analyse van de gebruikers van uw web-app.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/12/2017
ms.reviewer: daviste
ms.author: mbullwin
ms.openlocfilehash: 016a26acc153fba1c38d926fd5389d02755c2ff5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644291"
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Onderzoeken en gebruiksgegevens delen met interactieve workbooks in Application Insights

Werkmappen combineren [Azure Application Insights](app-insights-overview.md) gegevensvisualisaties, [Analytics-query's](app-insights-analytics.md), en de tekst in interactieve documenten. Werkmappen kunnen worden bewerkt door andere leden van een team met toegang tot de dezelfde Azure-resource. Dit betekent dat de query's en besturingselementen die worden gebruikt voor het maken van een werkmap zijn beschikbaar voor andere mensen lezen van de werkmap, zodat ze gemakkelijk te verkennen, uitbreiden en controleren op fouten.

Werkmappen zijn handig voor scenario's zoals:

* Het gebruik van uw app verkennen wanneer u niet de metrische gegevens van belang van tevoren weet: aantal gebruikers, bewaarsnelheden, conversieverhoudingen, enzovoort. In tegenstelling tot andere tools voor gebruiksanalyse in Application Insights kunnen werkmappen u combineren van meerdere soorten visualisaties en analyses, waardoor ze ideaal voor dit soort vrije verkennen.
* Uitleg over naar uw team hoe een nieuw uitgebrachte functie wordt uitgevoerd, door de gebruiker weergegeven aantallen voor interacties die sleutels en andere metrische gegevens.
* Delen van de resultaten van een A / B experimenteren in uw app met andere leden van uw team. U kunt uitgelegd van de doelstellingen van het experiment met tekst en vervolgens elke metrische gegevens en het Analytics-query gebruikt voor het evalueren van het experiment, samen met duidelijke call-outs voor of elke metrische waarde boven of onder-doel is weergeven.
* De impact van een storing rapportage over het gebruik van uw app, combineren van gegevens, tekstuitleg en een beschrijving van de volgende stappen om te voorkomen dat storingen in de toekomst.

> [!NOTE]
> Uw Application Insights-resource moet paginaweergaven of aangepaste gebeurtenissen aan het gebruik van werkmappen bevatten. [Meer informatie over het instellen van uw app voor het verzamelen van paginaweergaven automatisch met de Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Bewerken, rangschikken, klonen en secties van de werkmap verwijderen

Een werkmap is een en-klare van secties: onafhankelijk bewerkbare gebruik visualisaties, grafieken, tabellen, tekst of de resultaten van de Analytics-query.

Als u wilt de inhoud van een sectie van de werkmap bewerken, klikt u op de **bewerken** knop onder en rechts van de sectie van de werkmap.

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/editing-controls.png)

1. Wanneer u klaar bent bewerken van een sectie, klikt u op **gedaan bewerken** in de linkerbenedenhoek van de sectie.

2. Voor het maken van een duplicaat van een sectie, klikt u op de **in deze sectie klonen** pictogram. Het is een geweldige manier het herhalen voor een query zonder verlies van vorige iteraties dubbele secties te maken.

3. Als u wilt verplaatsen van een sectie in een werkmap, klikt u op de **omhoog** of **omlaag** pictogram.

4. Als u wilt een sectie definitief verwijderen, klikt u op de **verwijderen** pictogram.

## <a name="adding-usage-data-visualization-sections"></a>Gebruik visualisatie gegevenssecties toevoegen

Werkmappen bieden vier typen ingebouwde gebruik analytics visualisaties. Alle antwoorden op een algemene vraag over het gebruik van uw app. Tabellen en grafieken dan deze vier secties wilt toevoegen, moet u Analytics-query secties (Zie hieronder) toevoegen.

Als u wilt toevoegen van gebruikers, sessies, gebeurtenissen en retentie sectie naar uw werkmap, gebruik de **gebruikers toevoegen** of andere bijbehorende knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

![De gebruikerssectie in werkmappen](./media/app-insights-usage-workbooks/users-section.png)

**Gebruikers** secties beantwoorden 'hoeveel gebruikers een pagina weergegeven of een functie van Mijn site gebruikt?'

**Sessies** secties beantwoorden "het aantal sessies gebruikers besteden aan bepaalde pagina bekijken of met behulp van een functie van Mijn site?"

**Gebeurtenissen** secties beantwoorden 'het aantal keren dat gebruikers bepaalde pagina bekijken of gebruik van bepaalde functie van Mijn site?'

Elk van deze sectietypen drie biedt dezelfde set besturingselementen en visualisaties:

* [Meer informatie over het bewerken van gebruikers, sessies en gebeurtenissen secties](app-insights-usage-segmentation.md)
* In-/ uitschakelen de voornaamste grafiek, histogram rasters, automatische inzichten en voorbeeld gebruikers visualisaties met behulp van de **grafiek weergeven**, **raster weergeven**, **Insights weergeven**, en **Voorbeeld van deze gebruikers** selectievakjes boven aan elke sectie.

![Retentie-sectie in werkmappen](./media/app-insights-usage-workbooks/retention-section.png)

**Retentie** secties beantwoorden "Van de mensen die een pagina weergegeven of bepaalde functie op een dag of week gebruikt, hoeveel zijn teruggekomen in een volgende dag of week?"

* [Meer informatie over het bewerken van retentie secties](app-insights-usage-retention.md)
* Schakelen tussen de optionele totale Bewaarweken grafiek met behulp van de **grafiek met totale retentie weergeven** selectievakje aan de bovenkant van de sectie.

## <a name="adding-application-insights-analytics-sections"></a>Application Insights Analytics secties toevoegen

![Analytics-sectie in werkmappen](./media/app-insights-usage-workbooks/analytics-section.png)

De gedeelte van een Application Insights Analytics-query toevoegen aan uw werkmap, gebruikt u de **toevoegen analysequery** knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

Analytics-query secties kunnen u het toevoegen van willekeurige query's over uw Application Insights-gegevens in werkmappen. Deze flexibiliteit betekent Analytics query secties moet uw op voor het beantwoorden van vragen over uw site dan de vier genoemde voor gebruikers, sessies, gebeurtenissen en retentie, zoals:

* Het aantal uitzonderingen uw site throw tijdens dezelfde periode als een afname in gebruik
* Wat was de verdeling van de laadtijden voor pagina's voor gebruikers die een pagina bekijken?
* Hoeveel gebruikers een set pagina's weergegeven op uw site, maar niet een andere set pagina's? Dit kan nuttig zijn om te begrijpen als u clusters van gebruikers die gebruikmaken van verschillende subsets van functionaliteit van uw site zijn (Gebruik de `join` operator met de `kind=leftanti` modifier in de querytaal van Log Analytics).

Gebruik de [Log Analytics query language reference](https://docs.loganalytics.io/) voor meer informatie over het schrijven van query's.

## <a name="adding-text-and-markdown-sections"></a>Tekst en Markdown secties toe te voegen

Koppen, uitleg en opmerkingen toevoegen aan uw werkmappen omzetten kunnen een set met tabellen en grafieken in een boodschap. Tekst secties in ondersteuning voor werkmappen de [Markdown-syntaxis](https://daringfireball.net/projects/markdown/) voor tekst met opmaak, zoals kopteksten, vet, cursief en lijsten met opsommingstekens.

Als u wilt een tekstsectie toevoegen aan uw werkmap, gebruikt u de **tekst toevoegen** knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Opslaan en delen van werkmappen met uw team

Werkmappen worden opgeslagen in een Application Insights-resource in de **mijn rapporten** persoonlijke in u of in de sectie de **gedeelde rapporten** sectie die toegankelijk is voor iedereen met toegang tot de Application Insights-resource. Als u wilt alle werkmappen in de bron weergeven, klikt u op de **Open** knop in de actiebalk.

Voor het delen van een werkmap die op dat moment wordt **mijn rapporten**:

1. Klik op **Open** in de actiebalk
2. Klik op de knop '...' naast de werkmap die u wilt delen
3. Klik op **verplaatsen naar gedeelde rapporten**.

Als u wilt delen een werkmap met een koppeling of via e-mail, klikt u op **delen** in de actiebalk. Houd er rekening mee dat ontvangers van de koppeling moeten toegang hebben tot deze resource in Azure portal om de werkmap weer te geven. Als u wilt bewerken, moeten ontvangers minimaal Inzender-rechten voor de resource.

Een koppeling naar een werkmap in een Azure-Dashboard vastmaken:

1. Klik op **Open** in de actiebalk
2. Klik op de knop '...' naast de werkmap die u wilt vastmaken
3. Klik op **vastmaken aan dashboard**.

## <a name="next-steps"></a>Volgende stappen

## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)
    
