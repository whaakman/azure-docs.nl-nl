---
title: Een referentie-handleiding voor de Weergaveontwerper-onderdelen in Azure Log Analytics | Microsoft Docs
description: Met behulp van ontwerper in Log Analytics, kunt u aangepaste weergaven maken in Azure portal die tal van gegevensvisualisaties in uw Log Analytics-werkruimte worden weergegeven. In dit artikel is een handleiding verwijzing naar de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.
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
ms.component: ''
ms.openlocfilehash: c59df6c1d28df2ffc63f3760e4d1fa47bda37b83
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964041"
---
# <a name="reference-guide-to-view-designer-visualization-parts-in-log-analytics"></a>Naslaggids voor Weergaveontwerper visualisatie delen in Log Analytics
Met behulp van Designer bekijken in Azure Log Analytics, kunt u aangepaste weergaven maken in Azure portal die tal van gegevensvisualisaties in uw Log Analytics-werkruimte. In dit artikel is een handleiding verwijzing naar de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.

Zie voor meer informatie over Designer bekijken:

* [Designer weergeven](log-analytics-view-designer.md): biedt een overzicht van de Weergaveontwerper en procedures voor het maken en bewerken van aangepaste weergaven.
* [Tegel verwijzing](log-analytics-view-designer-tiles.md): bevat een verwijzing naar de instellingen voor elke tegel beschikbaar in uw aangepaste weergaven.


De beschikbare typen van de Weergaveontwerper-tegel worden in de volgende tabel beschreven:

| Weergavetype | Beschrijving |
|:--- |:--- |
| [Lijst met query 's](#list-of-queries-part) |Geeft een lijst van logboek zoekquery's. U kunt elke query om de resultaten weer te geven. |
| [Getal en lijst](#number-and-list-part) |De koptekst wordt weergegeven een enkel getal dat geeft het aantal records van een zoekquery logboek. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft. |
| [Twee getallen en lijst](#two-numbers-and-list-part) |De header geeft twee getallen die het aantal records van de afzonderlijke log zoekquery's weergeven. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft. |
| [Ring en lijst](#donut-and-list-part) |De koptekst wordt weergegeven een enkel getal met een overzicht van de kolom van een waarde in een logboekquery. De Ring worden de resultaten van de top drie records grafisch weergegeven. |
| [Twee tijdlijnen en lijst](#two-timelines-and-list-part) |De resultaten van twee logboeken-query's weergegeven de koptekst na verloop van tijd als kolomdiagrammen, met een toelichting waarop een enkel getal met een overzicht van de kolom van een waarde in een logboekquery. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft. |
| [Informatie](#information-part) |De koptekst wordt weergegeven voor statische tekst en een optionele koppeling. De lijst bevat een of meer items met een statische titel en tekst. |
| [Lijndiagram, bijschrift en lijst](#line-chart-callout-and-list-part) |De header geeft een lijndiagram met meerdere reeksen uit een logbestand querymogelijkheden op basis van tijd en een toelichting met de samengevatte waarde weer. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft. |
| [Lijndiagram en lijst](#line-chart-and-list-part) |De header geeft een lijndiagram met meerdere reeksen uit een query voor na verloop van tijd weer. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft. |
| [Stack van regel grafieken onderdeel](#stack-of-line-charts-part) |Drie afzonderlijke lijndiagrammen, met meerdere reeksen uit een query voor na verloop van tijd weergeven |

De volgende secties beschrijven de tegeltypen en de bijbehorende eigenschappen in detail.

## <a name="list-of-queries-part"></a>Lijst met query's-onderdeel
De lijst met query's onderdeel geeft een lijst van logboek zoekquery's. U kunt elke query om de resultaten weer te geven. De weergave bevat één query standaard, en u kunt selecteren **+ Query** toevoegen van extra query's.

![Lijst met query's weergeven](media/log-analytics-view-designer-parts/view-list-queries.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de weergave. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Vooraf geselecteerde filters |Een door komma's gescheiden lijst met eigenschappen op te nemen in het deelvenster links filteren wanneer u een query selecteren. |
| Weergavemodus |De eerste weergave die wordt weergegeven wanneer de query is geselecteerd. U kunt alle beschikbare weergaven selecteren na het openen van de query. |
| **Query's** | |
| Zoekquery |De query wilt uitvoeren. |
| Beschrijvende naam | De beschrijvende naam die wordt weergegeven. |

## <a name="number-and-list-part"></a>Aantal en de lijst met onderdeel
De koptekst wordt weergegeven een enkel getal dat geeft het aantal records van een zoekquery logboek. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft.

![Lijst met query's weergeven](media/log-analytics-view-designer-parts/view-number-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de weergave. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Titel** | |
| Legenda |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Query’s uitvoeren |De query wilt uitvoeren voor de header. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. De eerste twee eigenschappen voor de eerste tien records in de resultaten worden weergegeven. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Balken worden automatisch gemaakt die zijn gebaseerd op de relatieve waarde van de numerieke kolom.<br><br>Gebruik de `Sort` opdracht in de query om de records in de lijst te sorteren. Voor het uitvoeren van de query en alle records retourneren, kunt u **alle**. |
| Grafiek verbergen |Selecteer deze koppeling om uit te schakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Naam en waarde scheidingsteken |Het scheidingsteken één teken gebruiken om te parseren van de teksteigenschap in meerdere waarden. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Navigatie via klikken | Wanneer u op een item in de lijst met klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** |**> Aangepaste titels** |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **Lijst met** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie, [algemene instellingen die u](#thresholds). |

## <a name="two-numbers-and-list-part"></a>Twee getallen en lijst onderdeel
De header heeft twee getallen die een aantal records van de afzonderlijke log zoekquery's worden weergegeven. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft.

![Twee getallen en lijst weergeven](media/log-analytics-view-designer-parts/view-two-numbers-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de weergave. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Titel navigatie** | |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Titel** | |
| Legenda |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Query’s uitvoeren |De query wilt uitvoeren voor de header. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| **Lijst met** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. De eerste twee eigenschappen voor de eerste tien records in de resultaten worden weergegeven. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de `Sort` opdracht in de query om de records in de lijst te sorteren. Voor het uitvoeren van de query en alle records retourneren, kunt u **alle**. |
| Grafiek verbergen |Selecteer deze koppeling om uit te schakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Naam en waarde scheidingsteken |Het scheidingsteken één teken gebruiken om te parseren van de teksteigenschap in meerdere waarden. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Navigatie via klikken | Wanneer u op een item in de lijst met klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** |**> Aangepaste titels** |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **Lijst met** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie, [algemene instellingen die u](#thresholds). |

## <a name="donut-and-list-part"></a>Ring en lijst
De koptekst wordt weergegeven een enkel getal met een overzicht van de kolom van een waarde in een logboekquery. De Ring worden de resultaten van de top drie records grafisch weergegeven.

![Ring en lijst weergeven](media/log-analytics-view-designer-parts/view-donut-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Header** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de header. |
| **ringdiagram** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de ring. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **ringdiagram** |**> Center** |
| Tekst |De tekst die wordt weergegeven onder de waarde binnen de ring. |
| Bewerking |De bewerking uit te voeren op de eigenschap value om samen te vatten het als één waarde.<ul><li>Som: Hiermee voegt u de waarden van alle records toe.</li><li>Percentage: De verhouding van de records die zijn geretourneerd door de waarden in **leiden tot waarden die worden gebruikt in middenbewerking** naar het totaal aantal records in de query.</li></ul> |
| Resultaatwaarden gebruikt in middenbewerking |(Optioneel) Selecteer het plusteken (+) om toe te voegen een of meer waarden. De resultaten van de query zijn beperkt tot records met de eigenschapswaarden die u opgeeft. Als er geen waarden worden toegevoegd, worden alle records worden opgenomen in de query. |
| **Aanvullende opties** |**> Kleuren** |
| Kleur 1<br>Kleur 2<br>Kleur 3 |Selecteer de kleur voor elk van de waarden die worden weergegeven in de ring. |
| **Aanvullende opties** |**> Geavanceerde kleurtoewijzing** |
| Veldwaarde |Typ de naam van een veld weer te geven als een andere kleur als het is opgenomen in de ring. |
| Kleur |Selecteer de kleur voor het uniek veld. |
| **Lijst met** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling om uit te schakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Naam en waarde scheidingsteken |Het scheidingsteken één teken gebruiken om te parseren van de teksteigenschap in meerdere waarden. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Navigatie via klikken | Wanneer u op een item in de lijst met klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** |**> Aangepaste titels** |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **Lijst met** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie, [algemene instellingen die u](#thresholds). |

## <a name="two-timelines-and-list-part"></a>Twee tijdlijnen en lijst onderdeel
De resultaten van twee logboeken-query's weergegeven de koptekst na verloop van tijd als kolomdiagrammen, met een toelichting waarop een enkel getal met een overzicht van de kolom van een waarde in een logboekquery. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft.

![Twee tijdlijnen en lijst weergeven](media/log-analytics-view-designer-parts/view-two-timelines-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Titel navigatie** | |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Eerste grafiek<br>tweede grafiek** | |
| Legenda |De tekst die wordt weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur |De kleur moet worden gebruikt voor de kolommen in de reeks. |
| Query’s uitvoeren |De query wilt uitvoeren voor de eerste reeks. Het aantal records via elk tijdsinterval wordt vertegenwoordigd door de grafiekkolommen. |
| Bewerking |De bewerking uit te voeren op de eigenschap value om samen te vatten het als één waarde voor de toelichting.<ul><li>Som: De som van de waarden van alle records.</li><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Laatste voorbeeld: de waarde van het afgelopen interval dat opgenomen in de grafiek.</li><li>Eerste voorbeeld: de waarde van het eerste interval dat opgenomen in de grafiek.</li><li>Aantal: De telling van alle records die worden geretourneerd door de query.</li></ul> |
| **Lijst met** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling om uit te schakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Navigatie via klikken | Wanneer u op een item in de lijst met klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** |**> Aangepaste titels** |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **Lijst met** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie, [algemene instellingen die u](#thresholds). |

## <a name="information-part"></a>Informatiegedeelte
De koptekst wordt weergegeven voor statische tekst en een optionele koppeling. De lijst bevat een of meer items met een statische titel en tekst.

![Informatie weergeven](media/log-analytics-view-designer-parts/view-information.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Kleur |De achtergrondkleur voor de header. |
| **Header** | |
| Installatiekopie |Het afbeeldingsbestand dat wordt weergegeven in de header. |
| Label |De tekst die wordt weergegeven in de header. |
| **Header** |**> Koppeling** |
| Label |De tekst van de koppeling. |
| URL |De Url voor de koppeling. |
| **Informatie-items** | |
| Titel |De tekst die wordt weergegeven voor de titel van elk item. |
| Inhoud |De tekst die wordt weergegeven voor elk item. |

## <a name="line-chart-callout-and-list-part"></a>Lijndiagram, bijschrift en lijst onderdeel
De koptekst wordt een lijndiagram met meerdere reeksen uit een query voor tijd en een toelichting met de samengevatte waarde weergegeven. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft.

![Lijndiagram, bijschrift en lijst](media/log-analytics-view-designer-parts/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Header** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de header. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query wilt uitvoeren voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* trefwoord, de x-as van de grafiek wordt gebruikt voor dit tijdsinterval. Als de query niet onder de *interval* #trefwoord, de x-as wordt per uur intervallen. |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijndiagram** |**> Bijschrift** |
| Titel van bijschrift |De tekst die wordt weergegeven boven de waarde van bijschrift. |
| Reeksnaam |De waarde van de eigenschap voor de reeks moet worden gebruikt voor de waarde van bijschrift. Als er geen reeks is opgegeven, worden alle records van de query worden gebruikt. |
| Bewerking |De bewerking uit te voeren op de eigenschap value om samen te vatten het als één waarde voor de toelichting.<ul><li>Gemiddelde: Het gemiddelde van de waarden van alle records.</li><li>Aantal: De telling van alle records die worden geretourneerd door de query.</li><li>Laatste voorbeeld: de waarde van het afgelopen interval dat opgenomen in de grafiek.</li><li>Maximale: De maximumwaarde van de tijden die zijn opgenomen in de grafiek.</li><li>Min.: De minimale waarde uit de intervallen die zijn opgenomen in de grafiek.</li><li>Som: De som van de waarden van alle records.</li></ul> |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de typen en, optioneel, om te converteren van de waarden. De *eenheid* type Hiermee geeft u de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast label |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen label is opgegeven, alleen de *eenheid* type weergegeven. |
| **Lijst met** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling om uit te schakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Naam en waarde scheidingsteken |Het scheidingsteken één teken gebruiken om te parseren van de teksteigenschap in meerdere waarden. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Navigatie via klikken | Wanneer u op een item in de lijst met klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** |**> Aangepaste titels** |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **Lijst met** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie, [algemene instellingen die u](#thresholds). |

## <a name="line-chart-and-list-part"></a>Grafiek en een lijst met onderdeel van regel
Een lijndiagram met meerdere reeksen uit een query voor weergegeven de koptekst na verloop van tijd. De lijst bevat de bovenste tien resultaten van een query, met een grafiek die de relatieve waarde van een numerieke kolom of de wijziging na verloop van tijd aangeeft.

![Regel grafiek en een lijst weergeven](media/log-analytics-view-designer-parts/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze koppeling om het pictogram weer te geven. |
| **Header** | |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de header. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de header. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query wilt uitvoeren voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* trefwoord, de x-as van de grafiek wordt gebruikt voor dit tijdsinterval. Als de query niet onder de *interval* #trefwoord, de x-as wordt per uur intervallen. |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de typen en, optioneel, om te converteren van de waarden. De *eenheid* type Hiermee geeft u de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast label |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen label is opgegeven, alleen de *eenheid* type weergegeven. |
| **Lijst met** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst. Het aantal van de records die worden geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer deze koppeling om uit te schakelen van de grafiek aan de rechterkant van de numerieke kolom. |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Naam en waarde scheidingsteken |Het scheidingsteken één teken gebruiken om te parseren van de teksteigenschap in meerdere waarden. Zie voor meer informatie, [algemene instellingen die u](#sparklines). |
| Navigatie via klikken | Wanneer u op een item in de lijst met klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Lijst met** |**> Aangepaste titels** |
| Naam |De tekst die wordt weergegeven aan de bovenkant van de eerste kolom. |
| Waarde |De tekst die wordt weergegeven aan de bovenkant van de tweede kolom. |
| **Lijst met** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Selecteer deze koppeling om in te schakelen drempelwaarden. Zie voor meer informatie, [algemene instellingen die u](#thresholds). |

## <a name="stack-of-line-charts-part"></a>Stack van regel grafieken onderdeel
De stack van lijndiagram weergegeven drie afzonderlijke lijndiagrammen, met meerdere reeksen uit een query voor gedurende een periode, zoals hier wordt weergegeven:

![Stapel lijndiagrammen](media/log-analytics-view-designer-parts/view-stack-line-charts.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |De tekst die wordt weergegeven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer deze koppeling naar een nieuwe groep maken in de weergave, vanaf de huidige weergave. |
| Pictogram |Het afbeeldingsbestand dat wordt weergegeven naast het resultaat in de header. |
| **Grafiek 1<br>grafiek 2<br>grafiek 3** |**> Header** |
| Titel |De tekst die wordt weergegeven aan de bovenkant van de grafiek. |
| Subtitel |De tekst die wordt weergegeven onder de titel aan de bovenkant van de grafiek. |
| **Grafiek 1<br>grafiek 2<br>grafiek 3** |**Lijndiagram** |
| Query’s uitvoeren |De query wilt uitvoeren voor het lijndiagram. De eerste eigenschap een tekstwaarde is en de tweede eigenschap is een numerieke waarde. Deze query gebruikt normaal gesproken de *meting* trefwoord om samen te vatten resultaten. Als de query gebruikt de *interval* trefwoord, de x-as van de grafiek wordt gebruikt voor dit tijdsinterval. Als de query niet onder de *interval* #trefwoord, de x-as wordt per uur intervallen. |
| Navigatie via klikken | Wanneer u op de kop klikt uitgevoerde actie.  Zie voor meer informatie, [algemene instellingen die u](#click-through-navigation). |
| **Grafiek** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer deze koppeling naar een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden moeten worden geretourneerd door de query. Deze informatie wordt gebruikt op gegevenslabels weergegeven die wijzen op de typen en, optioneel, om te converteren van de waarden. De *eenheid* type Hiermee geeft u de categorie van de eenheid en definieert de beschikbare *huidige eenheid* waarden. Als u een waarde in *converteren naar*, de numerieke waarden worden geconverteerd van de *huidige eenheid* type de *converteren naar* type. |
| Aangepast label |De tekst die wordt weergegeven voor de y-as naast het label voor de *eenheid* type. Als er geen label is opgegeven, alleen de *eenheid* type weergegeven. |

## <a name="common-settings"></a>Algemene instellingen
De volgende secties beschrijven de instellingen die gemeenschappelijk voor verschillende onderdelen van de visualisatie zijn.

### <a name="name-value-separator"></a>Naam en waarde scheidingsteken
Het scheidingsteken naam en waarde is het scheidingsteken één teken gebruiken om te parseren van de teksteigenschap uit een lijst met query naar meerdere waarden. Als u een scheidingsteken opgeeft, kunt u opgeven namen voor elk veld, gescheiden door het dezelfde scheidingsteken in de **naam** vak.

Neem bijvoorbeeld een eigenschap genaamd *locatie* die waarden, zoals opgenomen *Redmond gebouw 41* en *Bellevue gebouw 12*. U kunt een streepje (-) opgeven voor de naam en waarde scheidingsteken en *plaats ontwikkelen* voor de naam. Deze aanpak parseert elke waarde in de twee eigenschappen aangeroepen *plaats* en *gebouw*.

### <a name="click-through-navigation"></a>Navigatie via klikken
Navigatie via klikken wordt gedefinieerd welke actie moet worden ondernomen wanneer u een koptekst of lijstitem in een weergave op.  Hiermee wordt een query in een geopend de [portal zoeken in logboeken](log-analytics-log-search-portals.md) of een andere weergave te starten.

De volgende tabel beschrijft de instellingen voor navigatie via klikken.

| Instelling           | Beschrijving |
|:--|:--|
| Zoeken in logboeken (automatisch) | Zoeken in Logboeken worden uitgevoerd wanneer u een header-item selecteren.  Dit is dezelfde zoeken in logboeken die het item is gebaseerd op.
| Zoeken in logboeken        | Zoeken in Logboeken om uit te voeren wanneer u een item in een lijst selecteert.  Typ de query in de **navigatiequery** vak.   Gebruik *{geselecteerde item}* om op te nemen van de syntaxis voor het item dat de gebruiker heeft geselecteerd.  Bijvoorbeeld, als de query heeft een kolom met de naam *Computer* en de navigatiequery is *{geselecteerde item}*, een query zoals *Computer = 'Computer'* wordt uitgevoerd wanneer u selecteert een computer. Als de navigatiequery is *Type = Event {geselecteerde item}*, de query *Type = Event Computer = 'Computer'* wordt uitgevoerd. |
| Weergave              | De weergave wilt openen wanneer u een header-item of een item in een lijst selecteert.  Selecteer de naam van een weergave in uw werkruimte in de **weergavenaam** vak. |



### <a name="sparklines"></a>Sparklines
Een sparkline is een kleine lijndiagram waarin de waarde van een lijstitem na verloop van tijd ziet. Voor de onderdelen van de visualisatie met een lijst, kunt u selecteren of om een horizontale balk, waarmee wordt aangegeven van de relatieve waarde van een numerieke kolom, of een sparkline, waarmee wordt aangegeven van de waarde na verloop van tijd weer te geven.

De volgende tabel beschrijft de instellingen voor sparklines:

| Instelling | Beschrijving |
|:--- |:--- |
| Sparklines inschakelen |Selecteer deze koppeling om een sparkline in plaats van een horizontale balk weer te geven. |
| Bewerking |Als sparklines zijn ingeschakeld, is dit de bewerking uit te voeren op elke eigenschap in de lijst om de waarden voor de sparkline te berekenen.<ul><li>Laatste voorbeeld: de laatste waarde voor de reeks gedurende de tijdsinterval.</li><li>Maximale: De maximumwaarde voor de reeks gedurende de tijdsinterval.</li><li>Min.: De minimale waarde voor de reeks gedurende de tijdsinterval.</li><li>Som: De som van de waarden voor de serie over het tijdsinterval.</li><li>Overzicht: Maakt gebruik van dezelfde `measure` opdracht als de query in de header.</li></ul> |

### <a name="thresholds"></a>Drempelwaarden
U kunt een gekleurde pictogram naast elk item in een lijst weergeven met behulp van de drempelwaarden. Drempelwaarden bieden u een snelle visuele indicator van de items die meer dan een bepaalde waarde of binnen een bepaald bereik vallen. U kunt bijvoorbeeld een groen pictogram voor items met een acceptabele waarde, geel als de waarde binnen een bereik dat een waarschuwing geeft en rood weergeven als er een foutwaarde overschrijdt.

Wanneer u drempelwaarden voor een onderdeel inschakelt, moet u een of meer drempels opgeven. Als de waarde van een item groter dan de drempelwaarde en lager is dan de volgende drempelwaarde is, wordt de kleur voor die waarde gebruikt. Als het item groter is dan de hoogste drempelwaarde, wordt een andere kleur wordt gebruikt. 

Elke set drempelwaarde heeft een drempelwaarde met een waarde van **standaard**. Dit is de kleur die ingesteld als er geen andere waarden is overschreden. U kunt toevoegen of verwijderen van drempelwaarden door het selecteren van de **toevoegen** (+) of **verwijderen** (knop x).

De volgende tabel beschrijft de instellingen voor de drempelwaarden:

| Instelling | Beschrijving |
|:--- |:--- |
| Drempelwaarden inschakelen |Selecteer deze koppeling om een Kleurenpictogram aan de linkerkant van elke waarde weer te geven. Het pictogram geeft de status van de waarde ten opzichte van de opgegeven drempelwaarden. |
| Naam |De naam van de drempelwaarde. |
| Drempelwaarde |De waarde voor de drempelwaarde. De kleur van de status van elk lijstitem is ingesteld op de kleur van de hoogste drempelwaarde die door de waarde van het item wordt overschreden. Als er geen drempelwaarden worden overschreden, wordt de standaardkleur van een gebruikt. |
| Kleur |De kleur die geeft aan de drempelwaarde dat. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [zoekopdrachten](log-analytics-log-search.md) ter ondersteuning van de query's in de visualisatie delen.
