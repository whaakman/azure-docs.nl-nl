---
title: Tegel verwijzing voor weergave Designer in Azure Log Analytics | Microsoft Docs
description: Ontwerper in Log Analytics kunt u aangepaste weergaven maken in de Azure portal die verschillende visualisaties van gegevens in de werkruimte voor logboekanalyse bevatten. Dit artikel bevat een verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.
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
ms.openlocfilehash: 9512a3f45ba6b03af4b0c9bee444948381f4fdcb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="log-analytics-view-designer-tile-reference"></a>Log Analytics-ontwerper tegel verwijzing
De ontwerper in Log Analytics kunt u aangepaste weergaven maken in de Azure portal die verschillende visualisaties van gegevens in de werkruimte voor logboekanalyse bevatten. Dit artikel bevat een verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.

Er zijn andere artikelen die beschikbaar zijn voor weergave Designer:

* [Designer bekijken](log-analytics-view-designer.md) -overzicht van de ontwerpfunctie voor weergaven en procedures voor het maken en bewerken van aangepaste weergaven.
* [Visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md) -verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en vervolgens de query's in alle weergaven moeten worden geschreven in de [nieuwe querytaal](https://go.microsoft.com/fwlink/?linkid=856078).  De weergaven die zijn gemaakt voordat de werkruimte werd bijgewerkt worden doorloopt geconverteerd.

De volgende tabel bevat de verschillende soorten tegels beschikbaar in de ontwerpfunctie voor weergaven.  De volgende secties beschrijven elk tegeltype in detail en hun eigenschappen.

| Tegel | Beschrijving |
|:--- |:--- |
| [Aantal](#number-tile) |Enkel getal met het aantal records uit een query. |
| [Twee getallen](#two-numbers-tile) |Twee één getallen met het aantal records uit twee verschillende query's. |
| [Ring](#donut-tile) |Ring grafiek op basis van een query met een totale waarde in het midden. |
| [Lijndiagram & toelichting](#line-chart-amp-callout-tile) |Lijndiagram op basis van een query en een toelichting bij een totale waarde. |
| [Lijndiagram](#line-chart-tile) |Lijndiagram op basis van een query. |
| [Twee tijdlijnen](#two-timelines-tile) |Kolomdiagram met twee reeksen, die elk op basis van een afzonderlijke query. |

## <a name="number-tile"></a>Aantal tegel
De **getal** tegel één getal met het aantal records uit een query logboek en een label wordt weergegeven.

![Aantal tegel](media/log-analytics-view-designer/tile-number.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die moet worden weergegeven onder de naam van de tegel. |
| **Tile** | |
| Legenda |De tekst die moet worden weergegeven onder de waarde. |
| Query’s uitvoeren |De query wilt uitvoeren.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel.  Dit biedt een alternatieve bericht als gegevens niet beschikbaar voor de tegel zijn.  Dit wordt meestal gebruikt voor het bieden van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens afkomstig zijn beschikbaar. |
| Query’s uitvoeren |Query uitvoeren om te worden uitgevoerd om te controleren of de gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd.  Als u geen bericht opgeeft *uitvoeren Assessment* wordt weergegeven. |


## <a name="two-numbers-tile"></a>Twee getallen tegel
De **twee nummer** tegel twee getallen met het aantal records uit twee verschillende logboek-query's en een label voor elke softwaretoepassing wordt weergegeven.

![Twee getallen tegel](media/log-analytics-view-designer/tile-two-numbers.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die moet worden weergegeven onder de naam van de tegel. |
| **Eerste tegel** | |
| Legenda |De tekst die moet worden weergegeven onder de waarde. |
| Query’s uitvoeren |De query wilt uitvoeren.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Tweede tegel** | |
| Legenda |De tekst die moet worden weergegeven onder de waarde. |
| Query’s uitvoeren |De query wilt uitvoeren.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel.  Dit biedt een alternatieve bericht als gegevens niet beschikbaar voor de tegel zijn.  Dit wordt meestal gebruikt voor het bieden van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens afkomstig zijn beschikbaar. |
| Query’s uitvoeren |Query uitvoeren om te worden uitgevoerd om te controleren of de gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd.  Als u geen bericht opgeeft *uitvoeren Assessment* wordt weergegeven. |


## <a name="donut-tile"></a>Tegel ring
De **ring** tegel één getal gedestilleerd uit de kolom van een waarde in een logboek-query wordt weergegeven.  De Ring worden de resultaten van de eerste drie records grafisch weergegeven.

![Tegel ring](media/log-analytics-view-designer/tile-donut.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die moet worden weergegeven onder de naam van de tegel. |
| **Ring** | |
| Query’s uitvoeren |De query voor de ring uit te voeren.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Dit is doorgaans een query die gebruikmaakt van de **meting** trefwoord om samen te vatten resultaten. |
| **Ring** |**> Center** |
| Tekst |De tekst die moet worden weergegeven onder de waarde binnen de ring. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde.<br><br>-Som: De waarden van alle records met de waarde van eigenschap toevoegen.<br>-Percentage: Percentage van de opgetelde waarden van records met de waarde van de eigenschap vergeleken met de opgetelde waarden van alle records. |
| Resultaatwaarden die worden gebruikt in de bewerking center |Klik eventueel op het plusteken om een of meer waarden toevoegen.  De resultaten van de query is beperkt tot records met de waarden van de eigenschappen die u opgeeft.  Als er geen waarden worden toegevoegd, dan alle records in de query worden opgenomen. |
| **Ring** |**> Aanvullende opties** |
| Kleuren |De kleur die moet worden weergegeven voor elk van de drie belangrijkste eigenschappen.  Als u alternatieve kleuren opgeven voor specifieke eigenschapswaarden wilt, gebruik geavanceerde kleur toewijzen. |
| Geavanceerde kleur toewijzen |Geeft een kleur voor specifieke waarden van eigenschappen.  Als de waarde die u opgeeft in de bovenste drie is, wordt in plaats van de standaardkleur de alternatieve kleur weergegeven.  Als de eigenschap niet in de bovenste drie, wordt niet de kleur weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel.  Dit biedt een alternatieve bericht als gegevens niet beschikbaar voor de tegel zijn.  Dit wordt meestal gebruikt voor het bieden van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens afkomstig zijn beschikbaar. |
| Query’s uitvoeren |Query uitvoeren om te worden uitgevoerd om te controleren of de gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd.  Als u geen bericht opgeeft *uitvoeren Assessment* wordt weergegeven. |


## <a name="line-chart-tile"></a>Regel grafiek tegel
De **lijndiagram** tegel wordt een lijndiagram met meerdere reeksen door een query logboek weergegeven gedurende een bepaalde periode.  

![Lijndiagram & toelichting tegel](media/log-analytics-view-designer/tile-line-chart.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die moet worden weergegeven onder de naam van de tegel. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query voor het lijndiagram worden uitgevoerd.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Dit is doorgaans een query die gebruikmaakt van de **meting** trefwoord om samen te vatten resultaten.  Als de query gebruikt de **interval** sleutelwoord vervolgens de x-as van de grafiek gebruikt dit tijdsinterval blijft.  Als de query niet bevat de **interval** sleutelwoord en vervolgens elk uur intervallen worden gebruikt voor de x-as. |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query.  Deze informatie wordt gebruikt voor de labels worden weergegeven in de grafiek die wijzen op de waardetypen en desgewenst voor het converteren van de waarden.  De **eenheidstype** geeft de categorie van de eenheid en definieert de **huidige eenheidstype** waarden die beschikbaar zijn.  Als u een waarde in selecteert **converteren naar** en vervolgens de numerieke waarden worden geconverteerd van de **huidige eenheid** type de **converteren naar** type. |
| Aangepast etiket |De tekst die moet worden weergegeven voor de Y-as naast het label voor het eenheidstype.  Als er geen naam is opgegeven, wordt alleen het eenheidstype weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel.  Dit biedt een alternatieve bericht als gegevens niet beschikbaar voor de tegel zijn.  Dit wordt meestal gebruikt voor het bieden van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens afkomstig zijn beschikbaar. |
| Query’s uitvoeren |Query uitvoeren om te worden uitgevoerd om te controleren of de gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd.  Als u geen bericht opgeeft *uitvoeren Assessment* wordt weergegeven. |


## <a name="line-chart--callout-tile"></a>De tegel diagram & toelichting regel
De **regel grafiek & toelichting** tegel bevat een lijndiagram met meerdere reeksen door een query logboek gedurende de tijd en een toelichting met een samengevatte waarde.  

![Lijndiagram & toelichting tegel](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die moet worden weergegeven onder de naam van de tegel. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query voor het lijndiagram worden uitgevoerd.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Dit is doorgaans een query die gebruikmaakt van de **meting** trefwoord om samen te vatten resultaten.  Als de query gebruikt de **interval** sleutelwoord vervolgens de x-as van de grafiek gebruikt dit tijdsinterval blijft.  Als de query niet bevat de **interval** sleutelwoord en vervolgens elk uur intervallen worden gebruikt voor de x-as. |
| **Lijndiagram** |**> Toelichting** |
| Toelichting |De tekst om weer te geven boven de waarde van de toelichting. |
| Naam van de gegevensreeks |De waarde van de eigenschap voor de reeks moet worden gebruikt voor de waarde van de toelichting.  Als geen reeksen is opgegeven, worden alle records in de query gebruikt. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde voor de toelichting.<br>-Gemiddelde: Gemiddelde van de waarde van alle records.<br><br>-Aantal: De telling van alle records geretourneerd door de query.<br>-Het laatste voorbeeld: Waarde van het laatste interval dat is opgenomen in de grafiek.<br>-Max: Maximumwaarde van het interval dat is opgenomen in de grafiek.<br>-Min: Minimale waarde uit de intervallen opgenomen in de grafiek.<br>-Som: De som van de waarde van alle records. |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query.  Deze informatie wordt gebruikt voor de labels worden weergegeven in de grafiek die wijzen op de waardetypen en desgewenst voor het converteren van de waarden.  De **eenheidstype** geeft de categorie van de eenheid en definieert de **huidige eenheidstype** waarden die beschikbaar zijn.  Als u een waarde in selecteert **converteren naar** en vervolgens de numerieke waarden worden geconverteerd van de **huidige eenheid** type de **converteren naar** type. |
| Aangepast etiket |De tekst die moet worden weergegeven voor de Y-as naast het label voor het eenheidstype.  Als er geen naam is opgegeven, wordt alleen het eenheidstype weergegeven. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel.  Dit biedt een alternatieve bericht als gegevens niet beschikbaar voor de tegel zijn.  Dit wordt meestal gebruikt voor het bieden van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens afkomstig zijn beschikbaar. |
| Query’s uitvoeren |Query uitvoeren om te worden uitgevoerd om te controleren of de gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd.  Als u geen bericht opgeeft *uitvoeren Assessment* wordt weergegeven. |


## <a name="two-timelines-tile"></a>Twee tijdlijnen tegel
De **twee tijdlijnen** tegel geeft de resultaten van twee logboek-query's na verloop van tijd als kolomdiagrammen.  Een toelichting wordt weergegeven voor elke reeks.  

![Twee tijdlijnen tegel](media/log-analytics-view-designer/tile-two-timelines.png)

| Instelling | Beschrijving |
|:--- |:--- |
| Naam |Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving |De tekst die moet worden weergegeven onder de naam van de tegel. |
| Eerste grafiek | |
| Legenda |De tekst die moet worden weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur |De kleur die moet worden gebruikt voor de kolommen in de eerste reeks. |
| Grafiek-Query |De query wilt uitvoeren voor de eerste reeks.  De telling van het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde voor de toelichting.<br><br>-Gemiddelde: Gemiddelde van de waarde van alle records.<br>-Aantal: De telling van alle records geretourneerd door de query.<br>-Het laatste voorbeeld: Waarde van het laatste interval dat is opgenomen in de grafiek.<br>-Max: Maximumwaarde van het interval dat is opgenomen in de grafiek. |
| **Tweede grafiek** | |
| Legenda |De tekst die moet worden weergegeven onder het bijschrift voor de tweede reeks. |
| Kleur |De kleur die moet worden gebruikt voor de kolommen in de tweede reeks. |
| Grafiek-Query |De query wilt uitvoeren voor de tweede reeks.  De telling van het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde voor de toelichting.<br><br>-Gemiddelde: Gemiddelde van de waarde van alle records.<br>-Aantal: De telling van alle records geretourneerd door de query.<br>-Het laatste voorbeeld: Waarde van het laatste interval dat is opgenomen in de grafiek.<br>-Max: Maximumwaarde van het interval dat is opgenomen in de grafiek. |
| **Geavanceerde** |**> Gegevensstroom verificatie** |
| Ingeschakeld |Selecteer als gegevensstroom verificatie moet zijn ingeschakeld voor de tegel.  Dit biedt een alternatieve bericht als gegevens niet beschikbaar voor de tegel zijn.  Dit wordt meestal gebruikt voor het bieden van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en de gegevens afkomstig zijn beschikbaar. |
| Query’s uitvoeren |Query uitvoeren om te worden uitgevoerd om te controleren of de gegevens beschikbaar voor de weergave.  Als de query geen resultaten oplevert, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht |Bericht dat wordt weergegeven als de verificatie-gegevensstroom query geen gegevens geretourneerd.  Als u geen bericht opgeeft *uitvoeren Assessment* wordt weergegeven. |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) ter ondersteuning van de query's in de tegels.
* Voeg [visualisatie delen](log-analytics-view-designer-parts.md) aan uw aangepaste weergave.
