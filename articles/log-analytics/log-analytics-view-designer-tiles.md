---
title: Een Naslaggids voor de tegels Designer bekijken in de Azure Log Analytics | Microsoft Docs
description: Ontwerp weergeven in Log Analytics gebruikt, kunt u aangepaste weergaven maken in de Azure portal die tal van gegevensvisualisaties weergeven in de werkruimte voor logboekanalyse. Dit artikel is een handleiding met de instellingen voor de tegels die beschikbaar in uw aangepaste weergaven zijn.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: a14a6bf196c165bdffa0a9d5d343c0430cff7f29
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="reference-guide-to-view-designer-tiles-in-log-analytics"></a>Naslaggids voor de ontwerper tegels in Log Analytics
Ontwerp weergeven in Azure Log Analytics gebruikt, kunt u aangepaste weergaven maken in de Azure portal die tal van gegevensvisualisaties in uw werkruimte voor logboekanalyse. Dit artikel is een handleiding met de instellingen voor de tegels die beschikbaar in uw aangepaste weergaven zijn.

Zie voor meer informatie over Designer bekijken:

* [Designer bekijken](log-analytics-view-designer.md): biedt een overzicht van Designer bekijken en -procedures voor het maken en bewerken van aangepaste weergaven.
* [Visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md): biedt een Naslaggids voor de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.

> [!NOTE]
> Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), de query's in alle weergaven moeten worden geschreven in de [nieuwe querytaal](https://go.microsoft.com/fwlink/?linkid=856078). De weergaven die zijn gemaakt vóór de upgrade van de werkruimte worden automatisch geconverteerd.

De beschikbare ontwerper tegels worden in de volgende tabel beschreven:  

| Tegel | Beschrijving |
|:--- |:--- |
| [Aantal](#number-tile) |Het aantal records uit een query. |
| [Twee getallen](#two-numbers-tile) |Het aantal records uit twee verschillende query's. |
| [Ring](#donut-tile) | Een grafiek die gebaseerd op een query met een totale waarde in het midden. |
| [Lijndiagram en toelichting](#line-chart-amp-callout-tile) | Een lijndiagram dat gebaseerd op een query en een toelichting met een totale waarde. |
| [Lijndiagram](#line-chart-tile) |Een lijndiagram dat gebaseerd op een query. |
| [Twee tijdlijnen](#two-timelines-tile) | Een kolomdiagram met twee reeksen, elk op basis van een afzonderlijke query. |

De volgende secties worden de typen tegel en hun eigenschappen in detail beschreven.

## <a name="number-tile"></a>Aantal tegel
De **getal** tegel worden weergegeven voor zowel het aantal records uit een query logboek en een label.

![Aantal tegel](media/log-analytics-view-designer/tile-number.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Tile** | |
| Legenda |De tekst die wordt weergegeven onder de waarde. |
| Query’s uitvoeren |De query die wordt uitgevoerd. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer deze koppeling als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel. Deze aanpak biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken de benadering gebruiken om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Het bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd. Als u geen bericht, bieden een *uitvoeren Assessment* statusbericht wordt weergegeven. |


## <a name="two-numbers-tile"></a>Twee getallen tegel
Deze tegel wordt het aantal records uit twee verschillende logboek-query's en een label voor elk weergegeven.

![Twee getallen tegel](media/log-analytics-view-designer/tile-two-numbers.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Eerste tegel** | |
| Legenda |De tekst die wordt weergegeven onder de waarde. |
| Query’s uitvoeren |De query die wordt uitgevoerd. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Tweede tegel** | |
| Legenda |De tekst die wordt weergegeven onder de waarde. |
| Query’s uitvoeren |De query die wordt uitgevoerd. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer deze koppeling als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel. Deze aanpak biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken de benadering gebruiken om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Het bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd. Als u geen bericht, bieden een *uitvoeren Assessment* statusbericht wordt weergegeven. |


## <a name="donut-tile"></a>Tegel ring
De **ring** tegel één getal overzicht van de kolom van een waarde in een logboek-query wordt weergegeven. De Ring worden de resultaten van de eerste drie records grafisch weergegeven.

![Tegel ring](media/log-analytics-view-designer/tile-donut.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Ring** | |
| Query’s uitvoeren |De query die wordt uitgevoerd voor de ring. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. |
| **Ring** |**> Center** |
| Tekst |De tekst die wordt weergegeven onder de waarde binnen de ring. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als één waarde.<ul><li>Som: De waarden van alle records met de waarde van eigenschap toevoegen.</li><li>Percentage: Percentage van de opgetelde waarden van records met de waarde van de eigenschap vergeleken met de opgetelde waarden van alle records.</li></ul> |
| Resultaatwaarden die worden gebruikt in de bewerking center |Selecteer desgewenst het plusteken (+) toevoegen van een of meer waarden. De resultaten van de query zijn beperkt tot records met de waarden van de eigenschappen die u opgeeft. Als er geen waarden worden toegevoegd, worden alle records opgenomen in de query. |
| **Ring** |**> Aanvullende opties** |
| Kleuren |De kleur die wordt weergegeven voor elk van de drie belangrijkste eigenschappen. Gebruik opgeven van alternatieve kleuren voor specifieke eigenschapswaarden *geavanceerde kleur toewijzing*. |
| Geavanceerde kleur toewijzen |Geeft een kleur die specifieke eigenschapswaarden vertegenwoordigt. Als de waarde die u opgeeft in de belangrijkste drie is, wordt de kleur van de alternatieve in plaats van de standaard kleur weergegeven. Als de eigenschap niet in de belangrijkste drie is, wordt de kleur niet weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer deze koppeling als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel. Deze aanpak biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken de benadering gebruiken om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Het bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd. Als u geen bericht, bieden een *uitvoeren Assessment* statusbericht wordt weergegeven. |


## <a name="line-chart-tile"></a>Regel grafiek tegel
Deze tegel is een lijndiagram waarin meerdere reeksen door een query logboek gedurende een bepaalde periode. 

![De grafiek en toelichting tegel regel](media/log-analytics-view-designer/tile-line-chart.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query die wordt uitgevoerd voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* sleutelwoord, de x-as worden gebruikt voor dit tijdsinterval blijft. Als de query geen gebruik maakt van de *interval* sleutelwoord, het uur van de x-as gebruikt. |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query. Deze informatie wordt gebruikt voor de labels worden weergegeven in de grafiek die wijzen op de waardetypen en desgewenst voor het converteren van de waarden. De **eenheidstype** geeft de categorie van de eenheid en definieert de **huidige eenheidstype** waarden die beschikbaar zijn. Als u een waarde in selecteert **converteren naar** en vervolgens de numerieke waarden worden geconverteerd van de **huidige eenheid** type de **converteren naar** type. |
| Aangepast etiket |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen naam is opgegeven, alleen de *eenheid* type wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer deze koppeling als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel. Deze aanpak biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken de benadering gebruiken om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Het bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd. Als u geen bericht, bieden een *uitvoeren Assessment* statusbericht wordt weergegeven. |


## <a name="line-chart-and-callout-tile"></a>De grafiek en toelichting tegel regel
Deze tegel heeft zowel een lijn grafiek die gedurende een bepaalde tijd een toelichting bij een samenvattingswaarde query uitvoeren op meerdere reeksen in een logboek weergegeven. 

![De grafiek en toelichting tegel regel](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die wordt weergegeven onder de naam van de tegel. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query die wordt uitgevoerd voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* sleutelwoord, de x-as worden gebruikt voor dit tijdsinterval blijft. Als de query geen gebruik maakt van de *interval* sleutelwoord, het uur van de x-as gebruikt. |
| **Lijndiagram** |**> Toelichting** |
| Titel van de toelichting | De tekst die wordt weergegeven boven de waarde van de toelichting. |
| Naam van de gegevensreeks |De waarde van de eigenschap series moet worden gebruikt als de waarde van de toelichting. Als geen reeksen is opgegeven, worden alle records in de query gebruikt. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als een enkelvoudige waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die zijn geretourneerd door de query.</li><li>Laatste steekproef: de waarde van het laatste interval dat opgenomen in de grafiek.</li><li>Max: De maximale waarde van de intervallen die zijn opgenomen in de grafiek.</li><li>Min: De minimale waarde van de intervallen die zijn opgenomen in de grafiek.</li><li>Som: De som van de waarden van alle records.</li></ul> |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de waardetypen en, desgewenst, aan de waarden te converteren. De *eenheid* type geeft de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in selecteert *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast etiket |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen naam is opgegeven, alleen de *eenheid* type wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer deze koppeling als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel. Deze aanpak biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken de benadering gebruiken om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Het bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd. Als u geen bericht, bieden een *uitvoeren Assessment* statusbericht wordt weergegeven. |


## <a name="two-timelines-tile"></a>Twee tijdlijnen tegel
De **twee tijdlijnen** tegel geeft de resultaten van twee logboek-query's na verloop van tijd als kolomdiagrammen. Een toelichting wordt weergegeven voor elke reeks. 

![Twee tijdlijnen tegel](media/log-analytics-view-designer/tile-two-timelines.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die wordt weergegeven onder de naam van de tegel. |
| Eerste grafiek | |
| Legenda |De tekst die wordt weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur |De kleur die wordt gebruikt voor de kolommen in de eerste reeks. |
| Grafiek-query |De query die wordt uitgevoerd voor de eerste reeks. Het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als een enkelvoudige waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die zijn geretourneerd door de query.</li><li>Laatste steekproef: de waarde van het laatste interval dat opgenomen in de grafiek.</li><li>Max: De maximale waarde van de intervallen die zijn opgenomen in de grafiek.</li></ul> |
| **Tweede grafiek** | |
| Legenda |De tekst die wordt weergegeven onder het bijschrift voor de tweede reeks. |
| Kleur |De kleur die wordt gebruikt voor de kolommen in de tweede reeks. |
| Grafiek-Query |De query die wordt uitgevoerd voor de tweede reeks. Het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking |De bewerking die wordt uitgevoerd op de eigenschap value om samen te vatten het als een enkelvoudige waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die zijn geretourneerd door de query.</li><li>Laatste steekproef: de waarde van het laatste interval dat opgenomen in de grafiek.</li><li>Max: De maximale waarde van de intervallen die zijn opgenomen in de grafiek. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer deze koppeling als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel. Deze aanpak biedt een alternatieve bericht als gegevens niet beschikbaar is. U normaal gesproken de benadering gebruiken om een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens beschikbaar. |
| Query’s uitvoeren |De query die wordt uitgevoerd om te bepalen of gegevens beschikbaar voor de weergave. Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Het bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd. Als u geen bericht, bieden een *uitvoeren Assessment* statusbericht wordt weergegeven. |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) ter ondersteuning van de query's in de tegels.
* Voeg [visualisatie delen](log-analytics-view-designer-parts.md) aan uw aangepaste weergave.
