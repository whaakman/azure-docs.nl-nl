---
title: Interactieve rapporten maken met Azure Monitor werkmappen | Microsoft docs
description: Vereenvoudig complexe rapportage met vooraf gedefinieerde en aangepaste geparameteriseerde werkmappen
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: c04af7fcf996c9b86c472383eac5658e5504b27e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139819"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Interactieve rapporten maken met Azure Monitor-werkmappen

Werkmappen tekst, combineren [Analytics-query's](https://docs.microsoft.com/azure/application-insights/app-insights-analytics), Azure metrische gegevens en parameters in uitgebreide interactieve rapporten. Werkmappen kunnen worden bewerkt door andere teamleden die toegang tot de dezelfde Azure-resources hebben.

Werkmappen zijn handig voor scenario's zoals:

* Het gebruik van uw app verkennen wanneer u niet de metrische gegevens van belang van tevoren weet: aantal gebruikers, bewaarsnelheden, conversieverhoudingen, enzovoort. In tegenstelling tot andere tools voor gebruiksanalyse kunnen werkmappen u combineren van meerdere soorten visualisaties en analyses, waardoor ze ideaal voor dit soort vrije verkennen.
* Uitleg over naar uw team hoe een nieuw uitgebrachte functie wordt uitgevoerd, door de gebruiker weergegeven aantallen voor interacties die sleutels en andere metrische gegevens.
* Delen van de resultaten van een A / B experimenteren in uw app met andere leden van uw team. U kunt uitgelegd van de doelstellingen van het experiment met tekst en vervolgens elke metrische gegevens en het Analytics-query gebruikt voor het evalueren van het experiment, samen met duidelijke call-outs voor of elke metrische waarde boven of onder-doel is weergeven.
* De impact van een storing rapportage over het gebruik van uw app, combineren van gegevens, tekstuitleg en een beschrijving van de volgende stappen om te voorkomen dat storingen in de toekomst.

## <a name="starting-with-a-template-or-saved-workbook"></a>Beginnen met een sjabloon of opgeslagen werkmap

Een werkmap is opgebouwd uit secties die bestaan uit onafhankelijk bewerkbare grafieken, tabellen, tekst, en besturingselementen voor de invoer. Voor meer informatie over werkmappen, is het raadzaam een openen. 

Selecteer **werkmappen** in het menu links uit in de Application Insights voor uw app-ervaring.

![Schermafbeelding van de navigatie met werkmappen](./media/app-insights-usage-workbooks/001-workbooks.png)

Hiermee wordt een galerie werkmap met een aantal vooraf gedefinieerde werkmappen om u aan de slag te helpen gestart.

![Schermafbeelding van de werkmap-galerie](./media/app-insights-usage-workbooks/002-workbook-gallery.png)

We beginnen met de **standaardsjabloon**, bevindt zich onder de kop **Quick start**.

![Schermafbeelding van de werkmap-galerie](./media/app-insights-usage-workbooks/003-default-template.png)

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Bewerken, rangschikken, klonen en secties van de werkmap verwijderen

Werkmappen hebben twee modi: **bewerkingsmodus**, en **Leesmodus**. Als de standaardwerkmap voor het eerst wordt gestart, wordt deze geopend **bewerkingsmodus**. Hiermee wordt alle inhoud van de werkmap, inclusief stappen en parameters die anders zijn verborgen. **Leesmodus** geeft een vereenvoudigde stijl van de rapportweergave. Hiermee kunt u abstracte onmiddellijk de complexiteit die is opgenomen in een rapport maken terwijl u nog steeds de onderliggende mechanics slechts enkele klikken wanneer dat nodig is voor de wijziging.

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/editing-controls-new.png)

1. Wanneer u klaar bent bewerken van een sectie, klikt u op **gedaan bewerken** in de linkerbenedenhoek van de sectie.

2. Voor het maken van een duplicaat van een sectie, klikt u op de **in deze sectie klonen** pictogram. Het is een geweldige manier het herhalen voor een query zonder verlies van vorige iteraties dubbele secties te maken.

3. Als u wilt verplaatsen van een sectie in een werkmap, klikt u op de **omhoog** of **omlaag** pictogram.

4. Als u wilt een sectie definitief verwijderen, klikt u op de **verwijderen** pictogram.

## <a name="adding-text-and-markdown-sections"></a>Tekst en Markdown secties toe te voegen

Koppen, uitleg en opmerkingen toevoegen aan uw werkmappen omzetten kunnen een set met tabellen en grafieken in een boodschap. Tekst secties in ondersteuning voor werkmappen de [Markdown-syntaxis](https://daringfireball.net/projects/markdown/) voor tekst met opmaak, zoals kopteksten, vet, cursief en lijsten met opsommingstekens.

Als u wilt een tekstsectie toevoegen aan uw werkmap, gebruikt u de **tekst toevoegen** knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

## <a name="adding-query-sections"></a>Query secties toevoegen

![Gedeelte van de query in werkmappen](./media/app-insights-usage-workbooks/analytics-section-new.png)

Gedeelte query toevoegen aan uw werkmap, gebruikt u de **query toevoegen** knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

Query secties zijn zeer flexibel en kunnen worden gebruikt om aan te vragen beantwoorden zoals:

* Het aantal uitzonderingen uw site throw tijdens dezelfde periode als een afname in gebruik
* Wat was de verdeling van de laadtijden voor pagina's voor gebruikers die een pagina bekijken?
* Hoeveel gebruikers een set pagina's weergegeven op uw site, maar niet een andere set pagina's? Dit kan nuttig zijn om te begrijpen als u clusters van gebruikers die gebruikmaken van verschillende subsets van functionaliteit van uw site zijn (Gebruik de `join` operator met de `kind=leftanti` modifier in de querytaal van Log Analytics).

U ook worden niet alleen beperkt tot de werkmap van de query uit de context van de toepassing gestart. U kunt query's uitvoeren voor meerdere apps met Application Insights bewaakt, evenals Log Analytics-werkruimten, zolang u toegang tot deze resources hebt.

Query van aanvullende externe Application Insights-resources gebruiken de **app** id.

```
union app('app01').requests, app('app02').requests, requests
| summarize count() by bin(timestamp, 1h)
```  

Deze query is het combineren van aanvragen van drie verschillende toepassingen. Een app met de naam app01, een app met de naam app02 en de aanvragen van de lokale Application Insights-resource.

Ophalen van gegevens uit een extern gebruik van de Log Analytics-werkruimte de **werkruimte** id.

Voor meer informatie over meerdere bronnen query's verwijzen naar de [officiële richtlijnen](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).

### <a name="advanced-analytic-query-settings"></a>Geavanceerde analytische query-instellingen

Elke sectie bevat een eigen geavanceerde instellingen, die toegankelijk via het Instellingenpictogram zijn ![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/005-settings.png) gelegen aan de rechterkant van de **parameters toevoegen** knop.

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/0006-settings-expanded.png)

   |         |          |
   | ---------------- |:-----|
   | **Aangepaste breedte**    | Stel dit neemt een item, zodat u veel items op één regel zodat u kunt uw grafieken en tabellen beter te organiseren in uitgebreide interactieve rapporten passen een willekeurige grootte.  |
   | **Voorwaardelijk zichtbaar** | Gebruik deze stappen op basis van een parameter in de Leesmodus verbergen. |
   | **Een parameter exporteren**| Hiermee wordt een geselecteerde rij in het raster of diagram ertoe leiden dat latere stappen waarden wijzigen of zichtbaar worden.  |
   | **Query weergeven als er niet bewerken** | U ziet nu de query boven de grafiek of de tabel, zelfs wanneer in de leesmodus.
   | **Open op de knop analytics weergeven wanneer er geen bewerkingen** | Hiermee wordt de blauw pictogram Analytics toegevoegd aan de rechter hoek van de grafiek om één klik toegang te verlenen.|

De meeste van deze instellingen zijn relatief gemakkelijk, maar om te begrijpen **exporteert u een parameter** is het beter om te controleren van een werkmap die van deze functionaliteit gebruikmaakt.

Een van de vooraf gedefinieerde werkmappen bevat informatie over actieve gebruikers.

De eerste sectie van de werkmap is gebaseerd op gegevens van analytische query:

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/003-active-users.png)

Het tweede gedeelte is ook gebaseerd op analytische query voor gegevens, maar het selecteren van een rij in de eerste tabel, wordt de inhoud van de grafiek interactief bijgewerkt:

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/004-active-users-trend.png)

 Dit is mogelijk door gebruik te maken van de **wanneer een item is geselecteerd, exporteert u een parameter** geavanceerde instellingen die zijn ingeschakeld in de tabel Analytics-query.

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/007-settings-export.png)

De geëxporteerde waarden maakt vervolgens gebruik van de tweede analytics-query als een rij is geselecteerd. Als er geen rij is geselecteerd, wordt standaard de rij die de algehele waarden vertegenwoordigt. 

```
let start = startofday(ago({TimeRange} + {Metric}));
union customEvents, pageViews
| where timestamp >= start
| where name in ({Activities}) or '*' in ({Activities}) or ('%' in ({Activities}) and itemType == 'pageView') or ('#' in ({Activities}) and itemType == 'customEvent')
{OtherFilters}
| where '{Filter}' == '' or '{Filter}' == '🔸 Overall' or {AnalyzeBy} == replace('🔹 ', '', '{Filter}')
| evaluate activity_engagement(user_Id, timestamp, start, now(), 1d, {Metric})
| where timestamp >= startofday(ago({TimeRange}))
| project timestamp, ["Active User"] = dcount_activities_outer
| render timechart 
```

## <a name="adding-metrics-sections"></a>Metrische gegevens secties toevoegen

Metrische gegevens secties bieden u volledige toegang tot Azure Monitor metrische gegevens opnemen in uw interactieve rapporten. Veel van de vooraf gedefinieerde werkmappen bevat zowel analytische query voor gegevens en metrische gegevens, zodat u kunt profiteren van het beste van beide functies op één plek. U hebt ook de mogelijkheid om op te halen metrische gegevens van resources in een van de abonnementen die u toegang tot hebt.

Hier volgt een voorbeeld van gegevens van virtuele machines worden opgehaald in een werkmap om te bieden van een visualisatie raster van CPU-prestaties:

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/008-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Parameter secties toevoegen

Werkmapparameters kunnen u waarden in de werkmap zonder handmatig bewerken in de secties query of tekst wijzigen.  Dit verwijdert de vereiste van dat u hoeft te begrijpen van de onderliggende analytics-querytaal en aanzienlijk breidt de potentiële doelgroep van rapportage op basis van een werkmap.

De waarden van parameters in query-, tekst- of andere gedeelten van de parameter zijn vervangen door de naam van de parameter accolades, plaatsen, zoals ``{parameterName}``.  Namen van taakparameters zijn beperkt tot dezelfde regels als JavaScript-id's, in feite alfabetische tekens of onderstrepingstekens bevatten, gevolgd door alfanumerieke tekens of onderstrepingstekens bevatten. Bijvoorbeeld, **a1** is toegestaan, maar **1a** is niet toegestaan.

Parameters zijn lineair, starten vanaf de bovenkant van een werkmap en doorgestuurd naar latere stappen.  Parameters die zijn gedeclareerd later in een werkmap kunnen overschrijven die zijn gedeclareerd verder omhoog.  Hiermee kunt ook parameters dat query's gebruiken om toegang te krijgen tot de waarden van parameters gedefinieerd verder omhoog.  In de stap van de parameter zelf zijn parameters ook lineaire, links naar rechts, waarbij parameters aan de rechterkant een parameter die eerder in dat dezelfde stap gedeclareerd kunnen vertrouwen.
 
Er zijn vier typen parameters die momenteel worden ondersteund:

  |         |          |
   | ---------------- |:-----|
   | **Tekst**    | de gebruiker bewerkt een tekstvak in en u kunt eventueel een query in te vullen in de standaard-waarde opgeven. |
   | **Vervolgkeuzelijst** | De gebruiker kiest uit een set waarden. |
   | **Tijdsbereikkiezer**| De gebruiker gaat kiezen uit een vooraf gedefinieerde set waarden voor tijd of kies uit meer dan een aangepast tijdsbereik.|
   | **Bronkiezer** | De gebruiker kiest uit de bronnen die zijn geselecteerd voor de werkmap.|

### <a name="using-a-text-parameter"></a>Met behulp van een tekstparameter

De waarde van een gebruiker typt in het tekstvak wordt vervangen rechtstreeks in de query, zonder aanhalingstekens of aanhalingstekens voor. Als de waarde die u moet een tekenreeks is de query aanhalingstekens rond de parameter moet hebben (zoals **{parameter}**).

Hiermee wordt de waarde in een tekstvak in op overal worden gebruikt. Mag een tabelnaam, kolomnaam, naam van de functie, operator, enzovoort.

Het parametertype tekst is een instelling **standaardwaarde ophalen uit analytische query**, waarmee de auteur van werkmap een query gebruiken voor het vullen van de standaardwaarde voor dit tekstvak.

Wanneer u de standaardwaarde van een analytics-query, worden alleen de eerste waarde van de eerste rij (rij 0, 0 kolom) wordt gebruikt als de standaardwaarde. Daarom is het raadzaam om uw query om terug te keren slechts één rij en één kolom te beperken. Andere gegevens die zijn geretourneerd door de query wordt genegeerd. 

Ongeacht de query retourneert de waarde wordt vervangen rechtstreeks met geen aanhalingstekens of aanhalingstekens. Als de query geen rijen worden geretourneerd, wordt het resultaat van de parameter is een lege tekenreeks (als de parameter niet vereist is) of een niet-gedefinieerde (als de parameter vereist is).

### <a name="using-a-dropdown"></a>Met behulp van een vervolgkeuzelijst

Het parametertype van de vervolgkeuzelijst kunt u maken van een besturingselement voor de vervolgkeuzelijst, zodat de selectie van een of meer waarden.

De vervolgkeuzelijst wordt gevuld met een analytics-query. Als de query één kolom retourneert, zijn de waarden in die kolom de **waarde** en de **label** in het besturingselement voor vervolgkeuzelijst. Als de query twee kolommen retourneert, de eerste kolom is de **waarde**, en de tweede kolom is de **label** wordt weergegeven in de vervolgkeuzelijst.  Als de query drie kolommen worden geretourneerd, wordt de 3e kolom wordt gebruikt om aan te geven van de standaardselectie in deze vervolgkeuzelijst.  Deze kolom kan elk type, maar de eenvoudigste manier is om te gebruiken bool of numerieke typen, waarbij 0 is ingesteld op false en 1 is ingesteld op true.

 Als de kolom een tekenreekstype is null/leeg tekenreeks wordt beschouwd als onwaar en een andere waarde wordt beschouwd als ' True '. Voor enkelvoudige selectie vervolgkeuzelijsten, wordt de eerste waarde met de waarde true als de standaardselectie gebruikt.  Alle waarden met de waarde true worden gebruikt voor meerdere selectie vervolgkeuzelijsten, zoals de standaard geselecteerd. De items in de vervolgkeuzelijst worden weergegeven in welke volgorde de query rijen geretourneerd. 

We bekijken de parameters die aanwezig zijn in de lijst met actieve gebruikers. Klik op het symbool bewerken naast **TimeRange**.

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/009-time-range.png)

De Parameter bewerken menu-item wordt geopend:

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/010-time-range-edit.png)

De query gebruikt een functie van de querytaal van analytics met de naam een **datatable** waarmee u een willekeurige tabel, volledige van inhoud, buiten het dunne lucht genereren. Bijvoorbeeld, de volgende analytics-query:

```
datatable( column1:string, column2:string )
[
 "row 1 column 1", "row 1 column 2",
"row 2 column 1", "row 2 column 2"
]
```

Het resultaat genereert:

![Application Insights werkmappen sectie besturingselementen bewerken](./media/app-insights-usage-workbooks/011-data-table.png)

Een voorbeeld van een meer van toepassing is een vervolgkeuzelijst gebruiken om op te halen uit een aantal landen met de naam:

```
customEvents
| where timestamp >= ago(14d)
| summarize count() by client_CountryOrRegion
| top 100 by count_
| project  client_CountryOrRegion 
| order by client_CountryOrRegion asc
```

De query worden resultaten als volgt weergegeven:

![Vervolgkeuzelijst voor land/regio](./media/app-insights-usage-workbooks/012-country-dropdown.png)

Vervolgkeuzelijsten zijn zeer krachtige hulpprogramma's voor het aanpassen en het maken van interactieve rapporten.

### <a name="time-range-parameters"></a>Bereik tijdparameters

Terwijl u uw eigen aangepaste tijd bereik parameter via het parametertype van de vervolgkeuzelijst maken kunt, kunt u de out-of-box parametertype voor tijdsbereik ook gebruiken als u niet dezelfde mate van flexibiliteit hoeft. 

Time range parametertypen hebben 15 standaard bereiken die naar de laatste 90 dagen gaan van vijf minuten. Er is ook een optie om toe te staan selectie aangepast tijdsbereik, waardoor de operator van het rapport om te kiezen expliciete starten en stoppen van de waarden voor het tijdsbereik.

### <a name="resource-picker"></a>Resourcekiezer

Het parametertype van de resource-kiezer biedt u de mogelijkheid om het bereik van uw rapport voor bepaalde typen resources te. Een voorbeeld van vooraf gedefinieerde werkmap die gebruikmaakt van het type resource datumkiezer is de **fout Insights** werkmap.

![Vervolgkeuzelijst voor land/regio](./media/app-insights-usage-workbooks/013-resource-picker.png)

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

## <a name="contributing-workbook-templates"></a>Werkmapsjablonen bijdragen

Hebt u een geweldige werkmapsjabloon hebt gemaakt en wilt delen met de community? Ga voor meer informatie naar onze [GitHub-opslagplaats](https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/README.md).

## <a name="next-steps"></a>Volgende stappen
- Om in te schakelen gebruik ervaringen, beginnen met het verzenden [aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) of [paginaweergaven](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Als u aangepaste gebeurtenissen of paginaweergaven al verzendt, Verken de hulpprogramma's voor gebruik als u wilt weten hoe gebruikers gebruiken voor uw service.
    - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
    - [Trechters](usage-funnels.md)
    - [Retentie](app-insights-usage-retention.md)
    - [Gebruikersstromen](app-insights-usage-flows.md)
    - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)