---
title: Aan de slag met Azure Monitor Log Analytics | Microsoft Docs
description: Dit artikel bevat een zelfstudie voor het gebruik van Log Analytics in Azure portal om query's schrijven.
services: log-analytics
author: bwren
manager: carmonm
ms.service: log-analytics
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: bwren
ms.openlocfilehash: 17b5c0b459e70909d9f305beb8bf87b83f1cf65c
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296522"
---
# <a name="get-started-with-log-analytics-in-azure-monitor"></a>Aan de slag met Log Analytics in Azure Monitor

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

In deze zelfstudie leert u hoe u Log Analytics in Azure portal gebruiken voor het schrijven van Azure Monitor logboeken-query's. Deze leert u hoe aan:

- Log Analytics gebruiken voor een eenvoudige query schrijven
- Het schema van uw gegevens begrijpen
- Filteren, sorteren en resultaten van Groepsbeleid
- Een tijdsbereik toepassen
- Grafieken maken
- Opslaan en laden van query 's
- Exporteren en delen van query 's

Zie voor een zelfstudie over het schrijven van Logboeken-query's, [aan de slag met Logboeken-query's in Azure Monitor](get-started-queries.md).<br>
Zie voor meer informatie over de logboeken-query's [overzicht van het logboek query's in Azure Monitor](log-query-overview.md).

## <a name="meet-log-analytics"></a>Voldoen aan de Log Analytics
Log Analytics is een web-hulpprogramma gebruikt om te schrijven en uitvoeren van Azure Monitor logboeken-query's. Open het door het selecteren van **logboeken** in het menu van Azure Monitor. Het begint al met een nieuwe lege query.

![Startpagina](media/get-started-portal/homepage.png)

## <a name="firewall-requirements"></a>Firewall-vereisten
Uw browser vereist voor het gebruik van Log Analytics, toegang tot de volgende adressen. Als uw browser de Azure-portal via een firewall openen is, moet u toegang tot deze adressen inschakelen.

| Uri | IP | Poorten |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80,443 |
| api.loganalytics.io | Dynamisch | 80,443 |
| docs.loganalytics.io | Dynamisch | 80,443 |

## <a name="basic-queries"></a>Eenvoudige query 's
Query's kunnen worden gebruikt om te zoeken naar termen, trends te identificeren, patronen analyseren en veel andere inzichten op basis van uw gegevens te kunnen bieden. Beginnen met een eenvoudige query uitvoert:

```Kusto
Event | search "error"
```

Deze query zoekt naar de _gebeurtenis_ tabel voor records met de term _fout_ in een eigenschap.

Query's kunnen beginnen met ofwel een tabelnaam wordt opgegeven of een [zoeken](/kusto/query/searchoperator) opdracht. Het bovenstaande voorbeeld begint met de naam van de tabel _gebeurtenis_, die alle records opgehaald uit de tabel Event. Opdrachten, zijn gescheiden, zodat de uitvoer van de eerste die als de invoer van de volgende opdracht fungeert in het sluisteken (|). U kunt een willekeurig aantal opdrachten toevoegen aan een eenvoudige query uitvoeren.

Een andere manier om te schrijven die dezelfde query zou zijn:

```Kusto
search in (Event) "error"
```

In dit voorbeeld **zoeken** is afgestemd op de _gebeurtenis_ tabel en alle records in de tabel wordt gezocht naar de term _fout_.

## <a name="running-a-query"></a>Een query uit te voeren
Een query uitvoeren door te klikken op de **uitvoeren** knop of te drukken **Shift + Enter**. Houd rekening met de volgende details vast te stellen de code die wordt uitgevoerd en de gegevens die wordt geretourneerd:

- Regeleinden: Een enkele einde kunt uw query gemakkelijker te lezen. Meerdere regeleinden opsplitsen deze in afzonderlijke query's.
- Cursor: Plaats de cursor ergens in de query wilt uitvoeren. De huidige query wordt beschouwd als de code worden totdat een lege regel is gevonden.
- Tijdsbereik - een tijdsbereik van _afgelopen 24 uur_ is standaard ingesteld. Voor het gebruik van een ander bereik, de tijdkiezer gebruiken of een expliciete tijd toevoegen bereikfilter aan uw query.


## <a name="understand-the-schema"></a>Informatie over het schema
Het schema is een verzameling van tabellen visueel worden gegroepeerd in een logische categorie. Aantal van de categorieën zijn van oplossingen voor bewaking. De _LogManagement_ categorie bevat algemene gegevens, zoals Windows en Syslog-gebeurtenissen, prestatiegegevens en agent heartbeats.

![Schema](media/get-started-portal/schema.png)

Gegevens zijn onderverdeeld in elke tabel in kolommen met verschillende gegevenstypen, zoals aangegeven door de pictogrammen naast de kolomnaam van de. Bijvoorbeeld, de _gebeurtenis_ tabel weergegeven in de schermafbeelding bevat kolommen zoals _Computer_ is tekst, _Culture_ die een getal is en _TimeGenerated_ die datum/tijd is.

## <a name="filter-the-results"></a>De resultaten te filteren
Beginnen met het ophalen van alles wat de _gebeurtenis_ tabel.

```Kusto
Event
```

Log Analytics automatisch het bereik van resultaten op basis van:

- Tijdsbereik:  Query's zijn standaard beperkt tot de laatste 24 uur.
- Het aantal resultaten: Resultaten zijn beperkt tot maximaal 10.000 records.

Deze query is zeer algemene en deze te veel resultaten worden geretourneerd. U kunt de resultaten door de tabelelementen, of door expliciet een filter toe te voegen aan de query filteren. Filteren van resultaten in de tabelelementen van toepassing op de bestaande resultatenset, terwijl een filter op de query zelf krijgt u een nieuwe gefilterde resultaat ingesteld en kan daarom meer nauwkeurige resultaten opleveren.

### <a name="add-a-filter-to-the-query"></a>Een filter toevoegen aan de query
Er is een pijl naar links van elke record. Klik op deze pijl om de details voor een bepaalde record openen.

Beweeg de muisaanwijzer boven de naam van een kolom voor de '+' en '-' pictogrammen om weer te geven. Klik op het teken '+' als u een filter op dat alleen de records met dezelfde waarde wordt geretourneerd. Klik op "-" wilt uitsluiten van records met deze waarde en klik vervolgens op **uitvoeren** de query opnieuw uitvoeren.

![Filter toevoegen aan query](media/get-started-portal/add-filter.png)

### <a name="filter-through-the-table-elements"></a>Filteren door de tabelelementen
Nu laten we ons richten op gebeurtenissen met een ernst van _fout_. Dit is opgegeven in een kolom met de naam _EventLevelName_. U moet Schuif naar rechts om te zien in deze kolom.

Klik op het filterpictogram naast de titel van de kolom en selecteer waarden in het pop-upvenster dat _begint met_ de tekst _fout_:

![Filteren](media/get-started-portal/filter.png)


## <a name="sort-and-group-results"></a>Resultaten sorteren en groeperen
De resultaten zijn nu verkleind om op te nemen alleen foutgebeurtenissen van SQL Server, hebt gemaakt in de afgelopen 24 uur. De resultaten worden echter niet gesorteerd op geen enkele manier. De resultaten wilt sorteren op een specifieke kolom, zoals _timestamp_ bijvoorbeeld, klikt u op de kolomtitel. Één klik wordt gesorteerd in oplopende volgorde, terwijl een tweede klik wordt gesorteerd in aflopende volgorde.

![Kolom sorteren](media/get-started-portal/sort-column.png)

Er is een andere manier om te organiseren resultaten door groepen. De resultaten van de groep op een specifieke kolom, sleept u eenvoudigweg de kolomkop boven de andere kolommen. Voor het maken van subgroepen, sleept u andere kolommen de bovenste balk ook.

![Groepen](media/get-started-portal/groups.png)

## <a name="select-columns-to-display"></a>Kolommen selecteren om weer te geven
De tabel met resultaten wordt vaak een groot aantal kolommen bevat. Mogelijk vindt u enkele van de geretourneerde kolommen worden niet standaard weergegeven, of kunt u sommige verwijderen, de kolommen die worden weergegeven. Selecteer de kolommen weer te geven, klikt u op de knop kolommen:

![Kolommen selecteren](media/get-started-portal/select-columns.png)


## <a name="select-a-time-range"></a>Selecteer een tijdsbereik
Standaard Log Analytics is van toepassing de _afgelopen 24 uur_ tijdsbereik. Voor het gebruik van een ander bereik, selecteert u een andere waarde via de tijdkiezer en klikt u op **uitvoeren**. Naast de vooraf gedefinieerde waarden, kunt u de _aangepast tijdsbereik_ optie om een absolute bereik voor uw query te selecteren.

![Tijdkiezer](media/get-started-portal/time-picker.png)

Wanneer u een aangepast tijdsbereik selecteert, wordt de geselecteerde waarden worden in UTC, die wordt mogelijk anders dan uw lokale tijdzone.

Als de query bevat expliciet een filter voor _TimeGenerated_, het tijdstip datumkiezer titel wordt weergegeven _in query instellen_. Handmatige selectie wordt uitgeschakeld om te voorkomen dat een conflict.


## <a name="charts"></a>Grafieken
Naast het retourneren van resultaten in een tabel, kunnen de queryresultaten worden weergegeven in de visuele opmaak. Gebruik de volgende query uit als een voorbeeld:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Standaard worden de resultaten weergegeven in een tabel. Klik op _grafiek_ om de resultaten in een grafische weergave weer:

![Staafdiagram](media/get-started-portal/bar-chart.png)

De resultaten worden weergegeven in een gestapeld staafdiagram. Klik op _gestapeld_ en selecteer _Pie_ om een andere weergave van de resultaten weer te geven:

![Cirkeldiagram](media/get-started-portal/pie-chart.png)

Verschillende eigenschappen van de weergave, zoals x en y-as of groepering en splitsen voorkeuren, het kunnen handmatig worden gewijzigd in de balk met besturingselementen.

U kunt de gewenste weergave ook instellen in de query zelf, met behulp van de render-operator.

### <a name="smart-diagnostics"></a>Slimme diagnostische gegevens
Op een tijdgrafiek als er een plotselinge piek of een stap in uw gegevens, ziet u mogelijk een gemarkeerde punt op de regel. Dit geeft aan dat _Smart Diagnostics_ heeft een combinatie van eigenschappen die de onverwachte wijziging wegfilteren geïdentificeerd. Klik op het punt voor meer informatie over het filter en de gefilterde versie te zien. Hiermee kunt u bepalen wat de oorzaak van de wijziging:

![Slimme diagnostische gegevens](media/get-started-portal/smart-diagnostics.png)

## <a name="pin-to-dashboard"></a>Vastmaken aan dashboard
Als u wilt een diagram of de tabel op een van uw Azure-gedeelde dashboards vastmaken, klikt u op het pictogram vastmaken.

![Vastmaken aan dashboard](media/get-started-portal/pin-dashboard.png)

Bepaalde vereenvoudigen worden toegepast op een grafiek, wanneer u deze aan een dashboard vastmaken:

- Rijen en kolommen in de tabel: Als u wilt een tabel aan het dashboard vastmaken, moet deze vier of minder kolommen hebben. Alleen de bovenste zeven rijen worden weergegeven.
- Beperking: Er zijn query's automatisch beperkt tot de afgelopen 14 dagen.
- BIn aantal beperkingen: Als u een grafiek die een groot aantal afzonderlijke opslaglocaties heeft weergeven, kleiner is ingevulde opslaglocaties automatisch worden gegroepeerd in één _anderen_ bin.

## <a name="save-queries"></a>Query's opslaan
Als u een handige query hebt gemaakt, is het raadzaam opslaan of delen met anderen. De **opslaan** pictogram is op de bovenste balk.

U kunt de querypagina van de volledige, of één query opslaan als een functie. Functies zijn query's die ook kunnen worden verwezen door andere query's. Als u wilt een query opslaan als een functie, moet u een functiealias, dit de naam die wordt gebruikt is voor het aanroepen van deze query als waarnaar wordt verwezen door andere query's opgeven.

![Functie opslaan](media/get-started-portal/save-function.png)

Log Analytics-query's worden altijd opgeslagen in een geselecteerde werkruimte en gedeeld met andere gebruikers van deze werkruimte.

## <a name="load-queries"></a>Laden van query 's
Het pictogram van de Query is op het gebied rechtsboven. Dit geeft een lijst van alle opgeslagen query's per categorie. Ook kunt u specifieke query's markeren als favorieten in de toekomst snel vinden. Dubbelklik op een opgeslagen query toe te voegen aan het huidige venster.

![Queryverkenner](media/get-started-portal/query-explorer.png)

## <a name="export-and-share-as-link"></a>Exporteren en delen als koppeling
Log Analytics ondersteunt meerdere methoden voor exporteren:

- Excel: De resultaten opslaan als een CSV-bestand.
- Power BI: De resultaten exporteren naar Power BI. Zie [importeren van Azure Monitor-logboekgegevens in Power BI](../../azure-monitor/platform/powerbi.md) voor meer informatie.
- Een koppeling delen: De query zelf kan worden gedeeld als een koppeling die vervolgens kan worden verzonden en uitgevoerd door andere gebruikers die toegang tot dezelfde werkruimte hebben.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Monitor logboeken-query's schrijven](get-started-queries.md).
