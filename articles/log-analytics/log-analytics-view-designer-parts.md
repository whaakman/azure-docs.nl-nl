---
title: Een Naslaggids voor de onderdelen Designer bekijken in de Azure Log Analytics | Microsoft Docs
description: Ontwerp weergeven in Log Analytics gebruikt, kunt u aangepaste weergaven maken in de Azure portal die tal van gegevensvisualisaties weergeven in de werkruimte voor logboekanalyse. Dit artikel is een Naslaggids voor de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: b560022f1f727949021a599e1ab342551f64c084
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129129"
---
# <a name="reference-guide-to-view-designer-visualization-parts-in-log-analytics"></a>Naslaggids voor de ontwerper visualisatie delen in Log Analytics
Ontwerp weergeven in Azure Log Analytics gebruikt, kunt u aangepaste weergaven maken in de Azure portal die een aantal gegevensvisualisaties van uw werkruimte voor logboekanalyse. Dit artikel is een Naslaggids voor de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.

Zie voor meer informatie over Designer bekijken:

* [Designer bekijken](log-analytics-view-designer.md): biedt een overzicht van Designer bekijken en -procedures voor het maken en bewerken van aangepaste weergaven.
* [Tegel verwijzing](log-analytics-view-designer-tiles.md): biedt een verwijzing naar de instellingen voor elke tegel beschikbaar in uw aangepaste weergaven.


De beschikbare typen van de ontwerper-tegel worden in de volgende tabel beschreven:

| Weergavetype | Beschrijving |
|:--- |:--- |
| [Lijst met query 's](#list-of-queries-part) |Geeft een lijst van logboek zoekquery's weer. U kunt elke query om de resultaten weer te geven. |
| [Aantal en de lijst](#number-and-list-part) |De koptekst van weergegeven een enkel getal waarin een telling van records uit een zoekquery logboek. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft. |
| [Twee getallen en lijst](#two-numbers-and-list-part) |De koptekst weergegeven twee getallen die met een aantal records van de afzonderlijke logboek zoekquery's weergeven. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft. |
| [Ring en lijst](#donut-and-list-part) |De header één getal overzicht van de kolom van een waarde in een logboek-query wordt weergegeven. De Ring worden de resultaten van de eerste drie records grafisch weergegeven. |
| [Twee tijdlijnen en lijst](#two-timelines-and-list-part) |De resultaten van twee logboek-query's weergegeven de koptekst gedurende een bepaalde periode als kolomdiagrammen met een toelichting die één getal overzicht van de kolom van een waarde in een logboek-query wordt weergegeven. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft. |
| [Informatie](#information-part) |De koptekst wordt weergegeven voor statische tekst en een optionele link. Een of meer items met een statische titel en tekst worden weergegeven in de lijst. |
| [Lijndiagram en toelichting lijst](#line-chart-callout-and-list-part) |De header geeft een lijndiagram met meerdere reeksen door een query logboek gedurende een bepaalde tijd een toelichting bij een samenvattingswaarde weer. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft. |
| [Lijndiagram en lijst](#line-chart-and-list-part) |De koptekst wordt weergegeven een lijndiagram met meerdere reeksen door een query logboek gedurende een bepaalde periode. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft. |
| [Stack van regel grafieken onderdeel](#stack-of-line-charts-part) |Hier wordt weergegeven in drie afzonderlijke lijndiagrammen met meerdere reeksen door een query logboek gedurende een bepaalde periode. |

De volgende secties worden de typen tegel en hun eigenschappen in detail beschreven.

## <a name="list-of-queries-part"></a>Lijst met query's onderdeel
De lijst met query's onderdeel geeft een lijst van logboek zoekquery's. U kunt elke query om de resultaten weer te geven. De weergave bevat één query standaard en kunt u selecteren **+ Query** om toe te voegen extra query's.

![Lijst met query's weergeven](media/log-analytics-view-designer/view-list-queries.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Titel |De tekst die wordt weergegeven boven aan de weergave. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Vooraf geselecteerde filters |Een door komma's gescheiden lijst met eigenschappen die u wilt opnemen in het linkerdeelvenster filterdeelvenster wanneer u een query selecteren. |
| Weergavemodus |De eerste weergave die wordt weergegeven wanneer de query is geselecteerd. Nadat de query wordt geopend, kunt u alle beschikbare weergaven selecteren. |
| **Query's** | |
| Zoekquery |De query wilt uitvoeren. |
| Beschrijvende naam | De beschrijvende naam die wordt weergegeven. |

## <a name="number-and-list-part"></a>Aantal en de lijst onderdeel
De koptekst van weergegeven een enkel getal waarin een telling van records uit een zoekquery logboek. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft.

![Lijst met query's weergeven](media/log-analytics-view-designer/view-number-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven boven aan de weergave. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Titel** | |
| Legenda |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Query’s uitvoeren |De query wilt uitvoeren voor de koptekst. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. De eerste twee eigenschappen voor de eerste tien records in de resultaten worden weergegeven. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de `Sort` opdracht in de query naar de records in de lijst sorteren. De query uitvoeren en retourneren van alle records, kunt u **alle**. |
| Grafiek verbergen |Selecteer deze koppeling naar het uitschakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Naam en waarde scheidingselement |Het scheidingsteken één teken gebruiken om te parseren van de eigenschap text in meerdere waarden. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Doorklikken navigatie | De actie is ondernomen wanneer u op een item in de lijst klikt.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** |**> Kolomtitels** |
| Naam |De tekst die wordt weergegeven boven aan de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie [algemene instellingen](#thresholds). |

## <a name="two-numbers-and-list-part"></a>Twee getallen en deel van de lijst
De kop heeft twee getallen die met een telling van records uit afzonderlijke logboek zoekquery's weer. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft.

![Twee getallen & lijstweergave](media/log-analytics-view-designer/view-two-numbers-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven boven aan de weergave. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Titel navigatie** | |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **Titel** | |
| Legenda |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Query’s uitvoeren |De query wilt uitvoeren voor de koptekst. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. De eerste twee eigenschappen voor de eerste tien records in de resultaten worden weergegeven. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de `Sort` opdracht in de query naar de records in de lijst sorteren. De query uitvoeren en retourneren van alle records, kunt u **alle**. |
| Grafiek verbergen |Selecteer deze koppeling naar het uitschakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Naam en waarde scheidingselement |Het scheidingsteken één teken gebruiken om te parseren van de eigenschap text in meerdere waarden. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Doorklikken navigatie | De actie is ondernomen wanneer u op een item in de lijst klikt.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** |**> Kolomtitels** |
| Naam |De tekst die wordt weergegeven boven aan de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie [algemene instellingen](#thresholds). |

## <a name="donut-and-list-part"></a>Ring en lijst onderdeel
De header één getal overzicht van de kolom van een waarde in een logboek-query wordt weergegeven. De Ring worden de resultaten van de eerste drie records grafisch weergegeven.

![Ring en lijst weergeven](media/log-analytics-view-designer/view-donut-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Header** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de header. |
| **Ring** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de ring. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **Ring** |**> Center** |
| Tekst |De tekst die wordt weergegeven onder de waarde binnen de ring. |
| Bewerking |De bewerking uit te voeren op de eigenschap value om samen te vatten het als één waarde.<ul><li>Som: Voegt de waarden van alle records.</li><li>Percentage: De verhouding van de records geretourneerd door de waarden in **leiden waarden die worden gebruikt in de bewerking center** aan het totaal aantal records in de query.</li></ul> |
| Resultaatwaarden gebruikt in middenbewerking |Selecteer desgewenst het plusteken (+) toevoegen van een of meer waarden. De resultaten van de query zijn beperkt tot records met de waarden van de eigenschappen die u opgeeft. Als er geen waarden worden toegevoegd, worden alle records opgenomen in de query. |
| **Extra opties** |**> Kleuren** |
| Kleur 1<br>Kleur 2<br>Kleur 3 |Selecteer de kleur voor elk van de waarden die worden weergegeven in de ring. |
| **Extra opties** |**> Geavanceerde kleur toewijzen** |
| Waarde van veld |Typ de naam van een veld weer te geven als een andere kleur als het is opgenomen in de ring. |
| Kleur |Selecteer de kleur voor de unieke veld. |
| **lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling naar het uitschakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Naam en waarde scheidingselement |Het scheidingsteken één teken gebruiken om te parseren van de eigenschap text in meerdere waarden. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Doorklikken navigatie | De actie is ondernomen wanneer u op een item in de lijst klikt.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** |**> Kolomtitels** |
| Naam |De tekst die wordt weergegeven boven aan de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie [algemene instellingen](#thresholds). |

## <a name="two-timelines-and-list-part"></a>Twee tijdlijnen en lijst onderdeel
De resultaten van twee logboek-query's weergegeven de koptekst gedurende een bepaalde periode als kolomdiagrammen met een toelichting die één getal overzicht van de kolom van een waarde in een logboek-query wordt weergegeven. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft.

![Twee tijdlijnen en lijst weergeven](media/log-analytics-view-designer/view-two-timelines-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Titel navigatie** | |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **Grafiek eerste<br>tweede grafiek** | |
| Legenda |De tekst die wordt weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur |De kleur die moet worden gebruikt voor de kolommen in de reeks. |
| Query’s uitvoeren |De query wilt uitvoeren voor de eerste reeks. Het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking |De bewerking uit te voeren op de eigenschap value om samen te vatten het als een enkelvoudige waarde voor de toelichting.<ul><li>Som: De som van de waarden van alle records.</li><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Laatste steekproef: de waarde van het laatste interval dat opgenomen in de grafiek.</li><li>Eerste voorbeeld: de waarde van het eerste interval dat opgenomen in de grafiek.</li><li>Aantal: De telling van alle records die zijn geretourneerd door de query.</li></ul> |
| **lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling naar het uitschakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Doorklikken navigatie | De actie is ondernomen wanneer u op een item in de lijst klikt.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** |**> Kolomtitels** |
| Naam |De tekst die wordt weergegeven boven aan de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie [algemene instellingen](#thresholds). |

## <a name="information-part"></a>Informatie-onderdeel
De koptekst wordt weergegeven voor statische tekst en een optionele link. Een of meer items met een statische titel en tekst worden weergegeven in de lijst.

![Informatie weergeven](media/log-analytics-view-designer/view-information.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Kleur |De achtergrondkleur voor de koptekst. |
| **Header** | |
| Installatiekopie |De installatiekopiebestand dat wordt weergegeven in de header. |
| Label |De tekst die wordt weergegeven in de koptekst. |
| **Header** |**> Koppeling** |
| Label |De tekst van de koppeling. |
| URL |De Url voor de koppeling. |
| **Informatie-items** | |
| Titel |De tekst die wordt weergegeven voor de titel van elk item. |
| Inhoud |De tekst die voor elk item wordt weergegeven. |

## <a name="line-chart-callout-and-list-part"></a>Lijndiagram en toelichting deel van de lijst
De header bevat een lijndiagram met meerdere reeksen door een query logboek gedurende de tijd en een toelichting met een samengevatte waarde. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft.

![Lijndiagram en toelichting lijstweergave](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Header** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de header. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query wilt uitvoeren voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* sleutelwoord, de x-as van de grafiek wordt gebruikt voor dit tijdsinterval blijft. Als de query niet bevat de *interval* sleutelwoord, het uur van de x-as gebruikt. |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **Lijndiagram** |**> Toelichting** |
| Titel van de toelichting |De tekst die wordt weergegeven boven de waarde van de toelichting. |
| Reeksnaam |De waarde van de eigenschap voor de reeks moet worden gebruikt voor de waarde van de toelichting. Als geen reeksen is opgegeven, worden alle records in de query gebruikt. |
| Bewerking |De bewerking uit te voeren op de eigenschap value om samen te vatten het als een enkelvoudige waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die zijn geretourneerd door de query.</li><li>Laatste steekproef: de waarde van het laatste interval dat opgenomen in de grafiek.</li><li>Max: De maximale waarde uit de intervallen die zijn opgenomen in de grafiek.</li><li>Min: De minimale waarde uit de intervallen die zijn opgenomen in de grafiek.</li><li>Som: De som van de waarden van alle records.</li></ul> |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de waardetypen en, desgewenst, aan de waarden te converteren. De *eenheid* type geeft de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in selecteert *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast etiket |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen naam is opgegeven, alleen de *eenheid* type wordt weergegeven. |
| **lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling naar het uitschakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Naam en waarde scheidingselement |Het scheidingsteken één teken gebruiken om te parseren van de eigenschap text in meerdere waarden. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Doorklikken navigatie | De actie is ondernomen wanneer u op een item in de lijst klikt.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** |**> Kolomtitels** |
| Naam |De tekst die wordt weergegeven boven aan de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie [algemene instellingen](#thresholds). |

## <a name="line-chart-and-list-part"></a>Grafiek en lijst onderdeel van regel
De koptekst wordt een lijndiagram met meerdere reeksen door een query logboek weergegeven gedurende een bepaalde periode. De lijst bevat de top tien resultaten van een query met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode aangeeft.

![Regel grafiek en een lijst weergeven](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Header** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de header. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query wilt uitvoeren voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* sleutelwoord, de x-as van de grafiek wordt gebruikt voor dit tijdsinterval blijft. Als de query niet bevat de *interval* sleutelwoord, het uur van de x-as gebruikt. |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de waardetypen en, desgewenst, aan de waarden te converteren. De *eenheid* type geeft de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in selecteert *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast etiket |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen naam is opgegeven, alleen de *eenheid* type wordt weergegeven. |
| **lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling naar het uitschakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Naam en waarde scheidingselement |Het scheidingsteken één teken gebruiken om te parseren van de eigenschap text in meerdere waarden. Zie voor meer informatie [algemene instellingen](#sparklines). |
| Doorklikken navigatie | De actie is ondernomen wanneer u op een item in de lijst klikt.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **lijst** |**> Kolomtitels** |
| Naam |De tekst die wordt weergegeven boven aan de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie [algemene instellingen](#thresholds). |

## <a name="stack-of-line-charts-part"></a>Stack van regel grafieken onderdeel
De stack van lijndiagram worden drie afzonderlijke lijndiagrammen met meerdere reeksen door een query logboek na verloop van tijd weergegeven, zoals hier wordt weergegeven:

![Stapel lijndiagrammen](media/log-analytics-view-designer/view-stack-line-charts.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |De installatiekopiebestand dat wordt weergegeven naast het resultaat in de header. |
| **Grafiek 1<br>grafiek 2<br>grafiek 3** |**> Header** |
| Titel |De tekst die wordt weergegeven boven aan de grafiek. |
| Subtitel |De tekst die wordt weergegeven onder de titel boven aan de grafiek. |
| **Grafiek 1<br>grafiek 2<br>grafiek 3** |**Lijndiagram** |
| Query’s uitvoeren |De query wilt uitvoeren voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* sleutelwoord, de x-as van de grafiek wordt gebruikt voor dit tijdsinterval blijft. Als de query niet bevat de *interval* sleutelwoord, het uur van de x-as gebruikt. |
| Doorklikken navigatie | De actie die wordt uitgevoerd wanneer u klikt u op de koptekst.  Zie voor meer informatie [algemene instellingen](#click-through-navigation). |
| **Grafiek** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de waardetypen en, desgewenst, aan de waarden te converteren. De *eenheid* type geeft de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in selecteert *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast etiket |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen naam is opgegeven, alleen de *eenheid* type wordt weergegeven. |

## <a name="common-settings"></a>Algemene instellingen
De volgende secties worden de instellingen die gemeenschappelijk voor verschillende visualisatie delen zijn.

### <a name="name-value-separator"></a>Naam en waarde scheidingselement
De naam en waarde scheidingsteken is het scheidingsteken één teken gebruiken om te parseren van de eigenschap text van een lijst met query in meerdere waarden. Als u een scheidingsteken opgeeft, kunt u opgeven namen voor elk veld, gescheiden door het dezelfde scheidingsteken in de **naam** vak.

Neem bijvoorbeeld een eigenschap genaamd *locatie* die waarden, zoals opgenomen *Redmond gebouw 41* en *Bellevue gebouw 12*. U kunt een streepje (-) opgeven voor het scheidingselement naam en waarde en *stad gebouw* voor de naam. Deze aanpak elke waarde in de twee eigenschappen aangeroepen geparseerd *stad* en *gebouw*.

### <a name="click-through-navigation"></a>Doorklikken navigatie
Doorklikken navigatie wordt gedefinieerd welke actie wordt uitgevoerd wanneer u klikt u op een kop of lijstitem in een weergave.  Hiermee wordt een query in ofwel geopend de [logboek zoeken portal](log-analytics-log-search-portals.md#log-search) of een andere weergave te starten.

De volgende tabel beschrijft de instellingen voor doorklikken navigatie.

| Instelling           | Beschrijving |
|:--|:--|
| Zoeken in logboeken (automatisch) | Logboek zoeken worden uitgevoerd wanneer u een header-item selecteren.  Dit is dezelfde logboek zoekopdracht die het item is gebaseerd op.
| Zoeken in logboeken        | Logboek zoeken worden uitgevoerd wanneer u een item in een lijst selecteert.  Typ de query in de **navigatie query** vak.   Gebruik *{geselecteerde item}* ook de syntaxis voor het item dat de gebruiker geselecteerd.  Bijvoorbeeld, als de query bevat een kolom met de naam *Computer* en de navigatie-query is *{geselecteerde item}*, een query zoals *Computer = 'Computer'* wordt uitgevoerd wanneer u selecteert een computer. Als de navigatie-query is *Type gebeurtenis {geselecteerde item} =*, de query *Type = gebeurtenis Computer = 'Computer'* wordt uitgevoerd. |
| Weergave              | De weergave te openen wanneer u een header-item of een item in een lijst selecteert.  Selecteer de naam van een weergave in de werkruimte in de **weergavenaam** vak. |



### <a name="sparklines"></a>Sparklines
Een sparkline is een kleine lijndiagram die laat zien van de waarde van een item uit de lijst gedurende een bepaalde periode. Voor visualisatie delen met een lijst, kunt u selecteren of een horizontale balk waarmee wordt aangegeven van de relatieve waarde van een numerieke kolom of een sparkline waarmee wordt aangegeven van de waarde ervan gedurende een periode worden weergegeven.

De volgende tabel beschrijft de instellingen voor sparklines:

| Instelling | Beschrijving |
|:--- |:--- |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. |
| Bewerking |Als sparklines zijn ingeschakeld, is dit de bewerking uit te voeren op elke eigenschap in de lijst om de waarden voor de sparkline te berekenen.<ul><li>Laatste steekproef: de laatste waarde voor de reeks via het tijdsinterval.</li><li>Max: De maximale waarde voor de reeks via het tijdsinterval.</li><li>Min: De minimale waarde voor de reeks via het tijdsinterval.</li><li>Som: De som van de waarden voor de reeks via het tijdsinterval.</li><li>Overzicht: Maakt gebruik van dezelfde `measure` opdracht als de query in de header.</li></ul> |

### <a name="thresholds"></a>Drempelwaarden
U kunt een gekleurde pictogram naast elk item in een lijst weergeven met behulp van de drempelwaarden. Drempelwaarden bieden u een snelle visuele indicator van items die groter zijn dan een bepaalde waarde of een bepaald bereik vallen. U kunt bijvoorbeeld een groen pictogram voor items met een acceptabele waarde, geel als de waarde valt binnen een bereik dat een waarschuwing geeft en rood weergeven als deze een foutwaarde overschrijdt.

Wanneer u drempelwaarden voor een deel inschakelt, moet u een of meer drempels opgeven. Als de waarde van een item groter dan de drempelwaarde en lager is dan de volgende drempelwaarde is, wordt de kleur voor deze waarde gebruikt. Als het item groter dan de hoogste drempelwaarde is, wordt een andere kleur wordt gebruikt. 

Elke set drempelwaarde heeft een drempelwaarde met een waarde van **standaard**. Dit is de kleur die ingesteld als er geen andere waarden zijn overschreden. U kunt toevoegen of verwijderen van drempelwaarden door het selecteren van de **toevoegen** (+) of **verwijderen** (knop x).

De volgende tabel beschrijft de instellingen voor drempelwaarden:

| Instelling | Beschrijving |
|:--- |:--- |
| Drempelwaarden inschakelen |Selecteer deze koppeling om een Kleurenpictogram aan de linkerkant van elke waarde weer te geven. Het pictogram geeft de status van de waarde ten opzichte van de opgegeven drempelwaarden. |
| Naam |De naam van de drempelwaarde. |
| Drempelwaarde |De waarde voor de drempelwaarde. De kleur van de status van elk lijstitem is ingesteld op de kleur van de hoogste drempelwaarde in die door de waarde van het item wordt overschreden. Als er geen drempelwaarde wordt overschreden, wordt een standaardkleur gebruikt. |
| Kleur |De kleur die de drempelwaarde aangeeft. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) ter ondersteuning van de query's in visualisatie delen.
