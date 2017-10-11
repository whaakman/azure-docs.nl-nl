---
title: Verwijzing voor weergave Designer in OMS Log Analytics deel | Microsoft Docs
description: Ontwerper in Log Analytics kunt u aangepaste weergaven maken in de OMS-console die verschillende visualisaties van gegevens in de OMS-opslagplaats bevatten. Dit artikel bevat een verwijzing van de instellingen voor elk van de onderdelen visualisatie beschikbaar voor gebruik in aangepaste weergaven.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40a6101576708936404447576d704a49666143fe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Log Analytics-ontwerper visualisatie onderdeelverwijzing
De ontwerper in Log Analytics kunt u aangepaste weergaven maken in de OMS-console die verschillende visualisaties van gegevens uit de opslagplaats OMS bevatten. Dit artikel bevat een verwijzing van de instellingen voor elk van de onderdelen visualisatie beschikbaar voor gebruik in aangepaste weergaven.

Er zijn andere artikelen die beschikbaar zijn voor weergave Designer:

* [Designer bekijken](log-analytics-view-designer.md) -overzicht van de ontwerpfunctie voor weergaven en procedures voor het maken en bewerken van aangepaste weergaven.
* [Tegel verwijzing](log-analytics-view-designer-tiles.md) -verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en vervolgens de query's in alle weergaven moeten worden geschreven in de [nieuwe querytaal](https://go.microsoft.com/fwlink/?linkid=856078).  De weergaven die zijn gemaakt voordat de werkruimte werd bijgewerkt worden doorloopt geconverteerd.

De volgende tabel beschrijft de verschillende soorten tegels beschikbaar in de ontwerpfunctie voor weergaven.  De volgende secties beschrijven elk tegeltype in detail en hun eigenschappen.

| Weergavetype | Beschrijving |
|:--- |:--- |
| [Lijst met query 's](#list-of-queries-part) |Geeft een lijst van logboek zoekquery's weer.  De gebruiker kan klikken op elke query om de resultaten weer te geven. |
| [Aantal & lijst met](#number-amp-list-part) |Kop heeft één nummer tonen aantal records uit een zoekquery logboek.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode. |
| [Twee getallen & lijst met](#two-numbers-amp-list-part) |Kop heeft twee getallen met het aantal records van de afzonderlijke logboek zoekquery's.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode. |
| [Ring & lijst met](#donut-amp-list-part) |Koptekst één getal gedestilleerd uit de kolom van een waarde in een logboek-query wordt weergegeven.  De Ring worden de resultaten van de eerste drie records grafisch weergegeven. |
| [Twee tijdlijnen & lijst met](#two-timelines-amp-list-part) |Header bevat de resultaten van twee logboek-query's na verloop van tijd als kolomdiagrammen met een toelichting weergeven van een enkel getal dat is gedestilleerd uit de kolom van een waarde in een logboek-query.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode. |
| [Informatie](#information-part) |Koptekst wordt weergegeven voor statische tekst en een optionele link.  Een of meer artikelen met statische tekst en titel hier weergegeven. |
| [Lijndiagram, toelichting & lijst](#line-chart-callout-amp-list-part) |Koptekst bevat een lijndiagram met meerdere reeksen door een query logboek gedurende de tijd en een toelichting met een samengevatte waarde.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode. |
| [Lijndiagram & lijst met](#line-chart-amp-list-part) |Koptekst wordt een lijndiagram met meerdere reeksen door een query logboek weergegeven gedurende een bepaalde periode.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode. |
| [Stack van regel grafieken onderdeel](#stack-of-line-charts-part) |Drie afzonderlijke lijndiagrammen met meerdere reeksen door een query logboek weergegeven gedurende een bepaalde periode. |

## <a name="list-of-queries-part"></a>Lijst met query's onderdeel
Geeft een lijst van logboek zoekquery's weer.  De gebruiker kan klikken op elke query om de resultaten weer te geven.  De weergave bevat één query standaard en klikt u op **+ Query** om toe te voegen extra query's.

![Lijst met query's weergeven](media/log-analytics-view-designer/view-list-queries.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Titel |Tekst om weer te geven aan de bovenkant van de weergave. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Vooraf geselecteerd filters |Door komma's gescheiden lijst met eigenschappen die u wilt opnemen in het linkerdeelvenster filterdeelvenster wanneer de gebruiker een query selecteert. |
| Modus weergeven |Beginweergave weergegeven als de query hebt geselecteerd.  De gebruiker kan beschikbare weergaven na het openen van de query selecteren. |
| **Query's** | |
| Zoekopdracht |De query wilt uitvoeren. |
| Beschrijvende naam |Beschrijvende naam van de query weer te geven voor de gebruiker. |

## <a name="number--list-part"></a>Aantal & lijst met onderdeel
Kop heeft één nummer tonen aantal records uit een zoekquery logboek.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode.

![Lijst met query's weergeven](media/log-analytics-view-designer/view-number-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de weergave. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze optie om de weergave pictogram. |
| **Titel** | |
| Legenda |Tekst om weer te geven aan de bovenkant van de header. |
| Query’s uitvoeren |De query wilt uitvoeren voor de koptekst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst.  De eerste twee eigenschappen voor de eerste tien records in de resultaten weergegeven.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de opdracht sorteren in de query om de records in de lijst te sorteren.  De gebruiker klikt op alle Zie dat de query wordt uitgevoerd en alle records. |
| Grafiek verbergen |Selecteer de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Kleur |De kleur van de balken of sparklines. |
| De naam van & scheidingsteken |Willekeurig teken scheidingsteken als u wilt het parseren van de eigenschap text in meerdere waarden.  Zie [algemene instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query |De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [algemene instellingen](#navigation-query) voor meer informatie. |
| **Lijst** |**> Kolomtitels** |
| Naam |Weer te geven aan de bovenkant van de eerste kolom van de lijst met tekst. |
| Waarde |Weer te geven aan de bovenkant van de tweede kolom van de lijst met tekst. |
| **Lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Deze optie selecteren om drempelwaarden.  Zie [algemene instellingen](#thresholds) voor meer informatie. |

## <a name="two-numbers--list-part"></a>Twee getallen & lijst onderdeel
Kop heeft twee getallen met het aantal records van de afzonderlijke logboek zoekquery's.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode.

![Twee getallen & lijstweergave](media/log-analytics-view-designer/view-two-numbers-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de weergave. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze optie om de weergave pictogram. |
| **Titel** | |
| Legenda |Tekst om weer te geven aan de bovenkant van de header. |
| Query’s uitvoeren |De query wilt uitvoeren voor de koptekst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| **Lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst.  De eerste twee eigenschappen voor de eerste tien records in de resultaten weergegeven.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de opdracht sorteren in de query om de records in de lijst te sorteren.  De gebruiker klikt op alle Zie dat de query wordt uitgevoerd en alle records. |
| Grafiek verbergen |Selecteer de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| De naam van & scheidingsteken |Willekeurig teken scheidingsteken als u wilt het parseren van de eigenschap text in meerdere waarden.  Zie [algemene instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query |De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [algemene instellingen](#navigation-query) voor meer informatie. |
| **Lijst** |**> Kolomtitels** |
| Naam |Weer te geven aan de bovenkant van de eerste kolom van de lijst met tekst. |
| Waarde |Weer te geven aan de bovenkant van de tweede kolom van de lijst met tekst. |
| **Lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Deze optie selecteren om drempelwaarden.  Zie [algemene instellingen](#thresholds) voor meer informatie. |

## <a name="donut--list-part"></a>Ring & lijst met onderdeel
Koptekst één getal gedestilleerd uit de kolom van een waarde in een logboek-query wordt weergegeven.  De Ring worden de resultaten van de eerste drie records grafisch weergegeven.

![Ring & lijst weergeven](media/log-analytics-view-designer/view-donut-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze optie om de weergave pictogram. |
| **Koptekst** | |
| Titel |Tekst om weer te geven aan de bovenkant van de header. |
| Subtitel |De tekst die moet worden weergegeven onder de titel aan de bovenkant van de header. |
| **Ring** | |
| Query’s uitvoeren |De query voor de ring uit te voeren.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn. |
| **Ring** |**> Center** |
| Tekst |De tekst die moet worden weergegeven onder de waarde binnen de ring. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde.<br><br>-Som: De waarden van alle records toevoegen.<br>-Percentage: Percentage van de records geretourneerd door de waarden in **leiden waarden die worden gebruikt in de bewerking center** aan het totaal aantal records in de query. |
| Resultaatwaarden die worden gebruikt in de bewerking center |Klik eventueel op het plusteken om een of meer waarden toevoegen.  De resultaten van de query is beperkt tot records met de waarden van de eigenschappen die u opgeeft.  Als er geen waarden worden toegevoegd, worden alle records opgenomen in de query. |
| **Extra opties** |**> Kleuren** |
| Kleur 1<br>Kleur 2<br>Kleur 3 |Selecteer de kleur voor elk van de waarden die worden weergegeven in de ring. |
| **Extra opties** |**> Geavanceerde kleur toewijzen** |
| Waarde van veld |Typ de naam van een veld weer te geven als een andere kleur als het is opgenomen in de ring. |
| Kleur |Selecteer de kleur voor de unieke veld. |
| **Lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| De naam van & scheidingsteken |Willekeurig teken scheidingsteken als u wilt het parseren van de eigenschap text in meerdere waarden.  Zie [algemene instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query |De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [algemene instellingen](#navigation-query) voor meer informatie. |
| **Lijst** |**> Kolomtitels** |
| Naam |Weer te geven aan de bovenkant van de eerste kolom van de lijst met tekst. |
| Waarde |Weer te geven aan de bovenkant van de tweede kolom van de lijst met tekst. |
| **Lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Deze optie selecteren om drempelwaarden.  Zie [algemene instellingen](#thresholds) voor meer informatie. |

## <a name="two-timelines--list-part"></a>Twee tijdlijnen & lijst met onderdeel
Header bevat de resultaten van twee logboek-query's na verloop van tijd als kolomdiagrammen met een toelichting weergeven van een enkel getal dat is gedestilleerd uit de kolom van een waarde in een logboek-query.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode.

![Twee tijdlijnen & lijst weergeven](media/log-analytics-view-designer/view-two-timelines-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze optie om de weergave pictogram. |
| **Grafiek eerste<br>tweede diagram** | |
| Legenda |De tekst die moet worden weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur |De kleur die moet worden gebruikt voor de kolommen in de reeks. |
| Query’s uitvoeren |De query wilt uitvoeren voor de eerste reeks.  De telling van het aantal records over elk tijdsinterval wordt vertegenwoordigd door de kolommen van de grafiek. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde voor de toelichting.<br><br>-Som: De som van de waarde van alle records.<br>-Gemiddelde: Gemiddelde van de waarde van alle records.<br>-Het laatste voorbeeld: Waarde van het laatste interval dat is opgenomen in de grafiek.<br>-Het eerste voorbeeld: De waarde van het eerste interval opgenomen in de grafiek.<br>-Aantal: De telling van alle records geretourneerd door de query. |
| **Lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Navigatie-query |De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [algemene instellingen](#navigation-query) voor meer informatie. |
| **Lijst** |**> Kolomtitels** |
| Naam |Weer te geven aan de bovenkant van de eerste kolom van de lijst met tekst. |
| Waarde |Weer te geven aan de bovenkant van de tweede kolom van de lijst met tekst. |
| **Lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Deze optie selecteren om drempelwaarden.  Zie [algemene instellingen](#thresholds) voor meer informatie. |

## <a name="information-part"></a>Informatie-onderdeel
Koptekst wordt weergegeven voor statische tekst en een optionele link.  Een of meer artikelen met statische tekst en titel hier weergegeven.

![Informatie weergeven](media/log-analytics-view-designer/view-information.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Kleur |De achtergrondkleur voor de koptekst. |
| **Koptekst** | |
| Installatiekopie |Installatiekopiebestand moet worden weergegeven in de header. |
| Label |De tekst die moet worden weergegeven in de header. |
| **Koptekst** |**> Koppeling** |
| Label |Tekst koppeling. |
| URL |De URL voor de koppeling. |
| **Informatie-Items** | |
| Titel |De tekst die moet worden weergegeven voor de titel van elk item. |
| Inhoud |De tekst die moet worden weergegeven voor elk item. |

## <a name="line-chart-callout--list-part"></a>Lijndiagram, toelichting & lijst onderdeel
Koptekst bevat een lijndiagram met meerdere reeksen door een query logboek gedurende de tijd en een toelichting met een samengevatte waarde.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode.

![Lijndiagram, toelichting & lijstweergave](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze optie om de weergave pictogram. |
| **Koptekst** | |
| Titel |Tekst om weer te geven aan de bovenkant van de header. |
| Subtitel |De tekst die moet worden weergegeven onder de titel aan de bovenkant van de header. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query voor het lijndiagram worden uitgevoerd.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Dit is doorgaans een query die gebruikmaakt van de **meting** trefwoord om samen te vatten resultaten.  Als de query gebruikt de **interval** sleutelwoord vervolgens de x-as van de grafiek gebruikt dit tijdsinterval blijft.  Als de query niet bevat de **interval** sleutelwoord en vervolgens elk uur intervallen worden gebruikt voor de x-as. |
| **Lijndiagram** |**> Toelichting** |
| Titel van de toelichting |De tekst die moet worden weergegeven boven de waarde van de toelichting. |
| Naam van de gegevensreeks |De waarde van de eigenschap voor de reeks moet worden gebruikt voor de waarde van de toelichting.  Als geen reeksen is opgegeven, worden alle records in de query gebruikt. |
| Bewerking |De bewerking uit te voeren op de eigenschap value samengevat in één waarde voor de toelichting.<br><br>-Gemiddelde: Gemiddelde van de waarde van alle records.<br>-Aantal telling van alle records geretourneerd door de query.<br>-Het laatste voorbeeld: Waarde van het laatste interval dat is opgenomen in de grafiek.<br>-Max: Maximumwaarde van het interval dat is opgenomen in de grafiek.<br>-Min: Minimale waarde uit de intervallen opgenomen in de grafiek.<br>-Som: De som van de waarde van alle records. |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query.  Deze informatie wordt gebruikt voor de labels worden weergegeven in de grafiek die wijzen op de waardetypen en desgewenst voor het converteren van de waarden.  Het eenheidstype geeft de categorie van de eenheid en definieert de huidige eenheidstype waarden die beschikbaar zijn.  Als u een waarde in converteren naar en vervolgens de numerieke waarden van het huidige eenheid type worden geconverteerd naar het converteren naar type. |
| Aangepast etiket |De tekst die moet worden weergegeven voor de Y-as naast het label voor het eenheidstype.  Als er geen naam is opgegeven, wordt alleen het eenheidstype weergegeven. |
| **Lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| De naam van & scheidingsteken |Willekeurig teken scheidingsteken als u wilt het parseren van de eigenschap text in meerdere waarden.  Zie [algemene instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query |De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [algemene instellingen](#navigation-query) voor meer informatie. |
| **Lijst** |**> Kolomtitels** |
| Naam |Weer te geven aan de bovenkant van de eerste kolom van de lijst met tekst. |
| Waarde |Weer te geven aan de bovenkant van de tweede kolom van de lijst met tekst. |
| **Lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Deze optie selecteren om drempelwaarden.  Zie [algemene instellingen](#thresholds) voor meer informatie. |

## <a name="line-chart--list-part"></a>Grafiek & lijst met onderdeel van regel
Koptekst wordt een lijndiagram met meerdere reeksen door een query logboek weergegeven gedurende een bepaalde periode.  Lijst geeft de top tien resultaten door een query met een grafiek waarin de relatieve waarde van een numerieke kolom of de wijziging gedurende een bepaalde periode.

![De weergave diagram & lijst met regels](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| Pictogram gebruiken |Selecteer deze optie om de weergave pictogram. |
| **Koptekst** | |
| Titel |Tekst om weer te geven aan de bovenkant van de header. |
| Subtitel |De tekst die moet worden weergegeven onder de titel aan de bovenkant van de header. |
| **Lijndiagram** | |
| Query’s uitvoeren |De query voor het lijndiagram worden uitgevoerd.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Dit is doorgaans een query die gebruikmaakt van de **meting** trefwoord om samen te vatten resultaten.  Als de query gebruikt de **interval** sleutelwoord vervolgens de x-as van de grafiek gebruikt dit tijdsinterval blijft.  Als de query niet bevat de **interval** sleutelwoord en vervolgens elk uur intervallen worden gebruikt voor de x-as. |
| **Lijndiagram** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query.  Deze informatie wordt gebruikt voor de labels worden weergegeven in de grafiek die wijzen op de waardetypen en desgewenst voor het converteren van de waarden.  Het eenheidstype geeft de categorie van de eenheid en definieert de huidige eenheidstype waarden die beschikbaar zijn.  Als u een waarde in converteren naar en vervolgens de numerieke waarden van het huidige eenheid type worden geconverteerd naar het converteren naar type. |
| Aangepast etiket |De tekst die moet worden weergegeven voor de Y-as naast het label voor het eenheidstype.  Als er geen naam is opgegeven, wordt alleen het eenheidstype weergegeven. |
| **Lijst** | |
| Query’s uitvoeren |De query wilt uitvoeren voor de lijst.  Het aantal records dat wordt geretourneerd door de query wordt weergegeven. |
| Grafiek verbergen |Selecteer de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| Kleur |De kleur van de balken of sparklines. |
| Bewerking |De bewerking uit te voeren voor de sparkline.  Zie [algemene instellingen](#sparklines) voor meer informatie. |
| De naam van & scheidingsteken |Willekeurig teken scheidingsteken als u wilt het parseren van de eigenschap text in meerdere waarden.  Zie [algemene instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query |De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Zie [algemene instellingen](#navigation-query) voor meer informatie. |
| **Lijst** |**> Kolomtitels** |
| Naam |Weer te geven aan de bovenkant van de eerste kolom van de lijst met tekst. |
| Waarde |Weer te geven aan de bovenkant van de tweede kolom van de lijst met tekst. |
| **Lijst** |**> Drempelwaarden** |
| Drempelwaarden inschakelen |Deze optie selecteren om drempelwaarden.  Zie [algemene instellingen](#thresholds) voor meer informatie. |

## <a name="stack-of-line-charts-part"></a>Stack van regel grafieken onderdeel
Drie afzonderlijke lijndiagrammen met meerdere reeksen door een query logboek weergegeven gedurende een bepaalde periode.

![-Stack van lijndiagrammen](media/log-analytics-view-designer/view-stack-line-charts.png)

| Instelling | Beschrijving |
|:--- |:--- |
| **Algemeen** | |
| Groepstitel |Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep |Selecteer een nieuwe groep maken in de weergave begint bij de huidige weergave. |
| Pictogram |Installatiekopiebestand moet worden weergegeven naast het resultaat in de header. |
| **Grafiek 1<br>grafiek 2<br>grafiek 3** |**> Header** |
| Titel |Tekst om weer te geven aan de bovenkant van de grafiek. |
| Subtitel |De tekst die moet worden weergegeven onder de titel boven aan de grafiek. |
| **Grafiek 1<br>grafiek 2<br>grafiek 3** |**Lijndiagram** |
| Query’s uitvoeren |De query voor het lijndiagram worden uitgevoerd.  De eerste eigenschap moet een tekstwaarde is en de tweede eigenschap van een numerieke waarde zijn.  Dit is doorgaans een query die gebruikmaakt van de **meting** trefwoord om samen te vatten resultaten.  Als de query gebruikt de **interval** sleutelwoord vervolgens de x-as van de grafiek gebruikt dit tijdsinterval blijft.  Als de query niet bevat de **interval** sleutelwoord en vervolgens elk uur intervallen worden gebruikt voor de x-as. |
| **Grafiek** |**> Y-as** |
| Logaritmische schaal gebruiken |Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden |Geef de eenheden voor de waarden die zijn geretourneerd door de query.  Deze informatie wordt gebruikt voor de labels worden weergegeven in de grafiek die wijzen op de waardetypen en desgewenst voor het converteren van de waarden.  Het eenheidstype geeft de categorie van de eenheid en definieert de huidige eenheidstype waarden die beschikbaar zijn.  Als u een waarde in converteren naar en vervolgens de numerieke waarden van het huidige eenheid type worden geconverteerd naar het converteren naar type. |
| Aangepast etiket |De tekst die moet worden weergegeven voor de Y-as naast het label voor het eenheidstype.  Als er geen naam is opgegeven, wordt alleen het eenheidstype weergegeven. |

## <a name="common-settings"></a>Algemene instellingen
De volgende secties worden de instellingen die gemeenschappelijk zijn voor verschillende visualisatie delen.

### <a name="name-value-separator">De naam van & scheidingsteken</a>
Willekeurig teken scheidingsteken als u wilt het parseren van de eigenschap text van een lijst met query in meerdere waarden.  Als u een scheidingsteken opgeeft, kunt u de namen voor elk veld van elkaar gescheiden door hetzelfde scheidingsteken wordt gebruikt in het vak naam opgeven.

Neem bijvoorbeeld een eigenschap genaamd *locatie* die waarden, zoals opgenomen *Redmond gebouw 41* en *Bellevue Building12*.  U kunt opgeven: voor de naam van & scheidingsteken en *stad gebouw* voor de naam.  Dit zou elke waarde in de twee eigenschappen aangeroepen parseren *stad* en *gebouw*.

### <a name="navigation-query">Navigatie-query</a>
De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteert.  Gebruik *{geselecteerde item}* om op te nemen van de syntaxis voor het item dat de gebruiker geselecteerd.

Bijvoorbeeld, als de query bevat een kolom met de naam *Computer* en de navigatie-query is *{geselecteerde item}*, een query zoals *Computer = 'Computer'* zou worden uitgevoerd wanneer de gebruiker een computer hebt geselecteerd.  Als de navigatie-query is *Type gebeurtenis {geselecteerde item} =* vervolgens de query *Type = gebeurtenis Computer = 'Computer'* zou worden uitgevoerd.

### <a name="sparklines">Sparklines</a>
Een sparkline is een kleine lijndiagram die laat zien van de waarde van een item uit de lijst gedurende een bepaalde periode.  Voor visualisatie delen met een lijst, kunt u selecteren of een horizontale balk aangeeft van de relatieve waarde van een numerieke kolom of een sparkline de waarde waarmee wordt aangegeven gedurende een periode worden weergegeven.

De volgende tabel beschrijft de instellingen voor sparklines.

| Instelling | Beschrijving |
|:--- |:--- |
| Sparklines inschakelen |Selecteer om sparkline in plaats van de horizontale balk weer te geven. |
| Bewerking |Als sparklines zijn ingeschakeld, is dit de bewerking uit te voeren op elke eigenschap in de lijst om de waarden voor de sparkline te berekenen.<br><br>-Het laatste voorbeeld: De laatste waarde voor de reeks via het tijdsinterval.<br>-Max: Maximumwaarde voor de reeks via het tijdsinterval.<br>-Min: Minimale waarde voor de reeks via het tijdsinterval.<br>-Som: De som van waarden voor de reeks via het tijdsinterval.<br>-Overzicht: Maakt gebruik van dezelfde opdracht meting als de query in de header. |

### <a name="thresholds">Drempelwaarden</a>
Drempelwaarden kunnen u een gekleurde pictogram naast elk item weergeven in een lijst waarin u een snelle visuele indicator van items die groter zijn dan een bepaalde waarde of een bepaald bereik vallen.  U kan bijvoorbeeld een groen pictogram voor items met een acceptabele waarde, geel als de waarde valt binnen een bereik dat een waarschuwing geeft en rood weergegeven als deze groter is dan de foutwaarde van een.

Wanneer u drempelwaarden voor een deel inschakelt, moet u een of meer drempels opgeven.  Als de waarde van een item groter dan de drempelwaarde en lager is dan de volgende drempelwaarde is, wordt deze kleur gebruikt.  Als het item groter dan vervolgens hoogste drempelwaarde is, wordt deze kleur ingesteld.   

Elke set drempelwaarde heeft een drempelwaarde met een waarde van **standaard**.  Dit is de kleur die is ingesteld als er geen andere waarden zijn overschreden.  U kunt toevoegen of verwijderen van drempelwaarden door te klikken op de  **+**  of **x** knop.

De volgende tabel beschrijft de instellingen voor tresholds.

| Instelling | Beschrijving |
|:--- |:--- |
| Drempelwaarden inschakelen |Selecteer een Kleurenpictogram weergeven aan de linkerkant van elke waarde die aangeeft van de status ten opzichte van de opgegeven drempelwaarden. |
| Naam |Naam voor de drempelwaarde. |
| Drempelwaarde |Waarde voor de drempelwaarde.  De kleur van de status van elk lijstitem is ingesteld op de kleur van de hoogste drempelwaarde is overschreden door de waarde van het artikel.  Er is een standaard-drempelwaarde is de kleur als er geen drempelwaarde wordt overschreden. |
| Kleur |De kleur voor de drempelwaarde. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) ter ondersteuning van de query's in visualisatie delen.
