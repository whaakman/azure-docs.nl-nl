---
title: Aan de slag met Logboeken-query's in Azure Monitor | Microsoft Docs
description: In dit artikel bevat een zelfstudie voor aan de slag logboeken-query's schrijven in Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.openlocfilehash: d9c94f7bd1a10e5db084e4541081998dc3de2f05
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56749698"
---
# <a name="get-started-with-azure-monitor-log-queries"></a>Aan de slag met Azure Monitor logboeken-query 's


> [!NOTE]
> U moet voltooien [aan de slag met Azure Monitor Log-Analytics](get-started-portal.md) voordat het voltooien van deze zelfstudie.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

In deze zelfstudie leert u Azure LMonitor logboeken-query's schrijven. Deze leert u hoe aan:

- Structuur van de query's begrijpen
- Query-resultaten sorteren
- Filter de resultaten van query
- Een tijdsperiode opgeven
- Welke velden moeten worden opgenomen in de resultaten
- Definiëren en gebruiken van aangepaste velden
- Cumulatieve en groep-resultaten


## <a name="writing-a-new-query"></a>Een nieuwe query schrijven
Query's kunnen beginnen met ofwel een tabelnaam wordt opgegeven of de *zoeken* opdracht. U moet beginnen met een tabelnaam wordt opgegeven, omdat deze een duidelijke bereik voor de query wordt gedefinieerd en verbetert de prestaties van query's zowel relevantie van de resultaten.

> [!NOTE]
> De Kusto-query-taal die wordt gebruikt door Azure Monitor is hoofdlettergevoelig. Trefwoorden voor de taal worden doorgaans intern kleine letters. Wanneer u de namen van tabellen of kolommen in een query, zorg ervoor dat u het juiste geval is, zoals wordt weergegeven in het deelvenster schema.

### <a name="table-based-queries"></a>Query's op basis van een tabel
Azure Monitor organiseert logboekgegevens in tabellen, elk bestaat uit meerdere kolommen. Alle tabellen en kolommen worden weergegeven in het deelvenster schema in Log Analytics in de Analytics-portal. Identificeert een tabel die u geïnteresseerd bent en klikt u vervolgens Kijk eens een deel van de gegevens:

```Kusto
SecurityEvent
| take 10
```

De bovenstaande query retourneert 10 resultaten van de *SecurityEvent* tabel in een specifieke volgorde. Dit is een veelgebruikte manier een blik op een tabel en inzicht in de structuur en inhoud. We bekijken hoe deze wordt gemaakt:

* De query wordt gestart met de naam van de tabel *SecurityEvent* -in dit gedeelte definieert u het bereik van de query.
* Het sluisteken (|) opdrachten zijn gescheiden, zodat de uitvoer van het eerste item in de invoer van de volgende opdracht. U kunt een willekeurig aantal doorgesluisd elementen kunt toevoegen.
* Na de pipe is de **nemen** opdracht, waarbij een bepaald aantal willekeurige records geretourneerd uit de tabel.

We kunnen de query daadwerkelijk uitvoeren zelfs zonder toe te voegen `| take 10` : die nog steeds geldig zou zijn, maar het kan maximaal 10.000 resultaten worden geretourneerd.

### <a name="search-queries"></a>Zoekquery 's
Zoekquery's zijn minder gestructureerde en doorgaans meer geschikt is voor het zoeken van records die een specifieke waarde in een van de kolommen bevatten:

```Kusto
search in (SecurityEvent) "Cryptographic"
| take 10
```

Deze query zoekt naar de *SecurityEvent* tabel voor records die de zin 'Cryptografische' bevatten. Deze records worden 10 records geretourneerd en weergegeven. Als we weglaat de `in (SecurityEvent)` deel en alleen worden uitgevoerd `search "Cryptographic"`, de zoekopdracht gaat over *alle* tabellen die langer duren en minder efficiënt.

> [!NOTE]
> Standaard wordt een tijdsbereik van _afgelopen 24 uur_ is ingesteld. Als u een ander bereik, gebruikt u de tijdkiezer (naast de *gaat* knop) of een expliciete tijd toevoegen bereikfilter aan uw query.

## <a name="sort-and-top"></a>Sorteren en top
Terwijl **nemen** is handig om een paar records, de resultaten zijn geselecteerd en wordt weergegeven in willekeurige volgorde. Als u een geordende weergeven, kunt u **sorteren** door de gewenste kolom:

```Kusto
SecurityEvent   
| sort by TimeGenerated desc
```

Die al te veel resultaten kan worden geretourneerd en kan ook enige tijd duren. De bovenstaande query sorteert *het gehele* SecurityEvent tabel door de kolom TimeGenerated. De Analytics-portal beperkt vervolgens de weergave alleen 10.000 records wilt weergeven. Deze aanpak is natuurlijk niet optimaal.

De beste manier om op te halen, alleen de meest recente 10 records is met **boven**, die de hele tabel aan de serverzijde sorteert en retourneert vervolgens de bovenste records:

```Kusto
SecurityEvent
| top 10 by TimeGenerated
```

Aflopend is de standaardinstelling sorteervolgorde, zodat we meestal laat de **desc** argument. De uitvoer ziet er als volgt:

![Top 10](media/get-started-queries/top10.png)


## <a name="where-filtering-on-a-condition"></a>Waar: filteren op een voorwaarde
Filters, zoals aangegeven door de naam, filteren de gegevens door een specifieke voorwaarde. Dit is de meest voorkomende manier om te beperken van de queryresultaten naar relevante informatie.

U kunt een filter toevoegen aan een query met de **waar** operator gevolgd door een of meer voorwaarden. De volgende query retourneert bijvoorbeeld alleen *SecurityEvent* records waarin _niveau_ gelijk is aan _8_:

```Kusto
SecurityEvent
| where Level == 8
```

Bij het schrijven van de filtervoorwaarden, kunt u de volgende expressies:

| Expressie | Description | Voorbeeld |
|:---|:---|:---|
| == | Gelijkheid controleren<br>(hoofdlettergevoelig) | `Level == 8` |
| =~ | Gelijkheid controleren<br>(niet hoofdlettergevoelig) | `EventSourceName =~ "microsoft-windows-security-auditing"` |
| !=, <> | Ongelijkheid controleren<br>(beide expressies zijn identiek) | `Level != 4` |
| *en*, *of* | Vereist tussen de voorwaarden| `Level == 16 or CommandLine != ""` |

Als u wilt filteren op meerdere voorwaarden, kunt u ofwel **en**:

```Kusto
SecurityEvent
| where Level == 8 and EventID == 4672
```

of meerdere overbrengen **waar** elementen een na de andere:

```Kusto
SecurityEvent
| where Level == 8 
| where EventID == 4672
```
    
> [!NOTE]
> Waarden hebben verschillende typen, zodat u wellicht te gieten om uit te voeren vergelijking op het juiste type. Bijvoorbeeld, SecurityEvent *niveau* kolom is van het type String, zodat u cast-conversie moet deze naar een numeriek type zoals *int* of *lang*, voordat u de numerieke operators erop kunt gebruiken: `SecurityEvent | where toint(Level) >= 10`

## <a name="specify-a-time-range"></a>Een tijdsperiode opgeven

### <a name="time-picker"></a>Tijdkiezer
De tijdkiezer naast de knop uitvoeren en geeft aan dat we bij het opvragen van alleen de records van de afgelopen 24 uur. Dit is het tijdsbereik voor standaard toegepast op alle query's. Als u alleen de records van het afgelopen uur, selecteer _afgelopen uur_ en voer de query opnieuw uit.

![Tijdkiezer](media/get-started-queries/timepicker.png)


### <a name="time-filter-in-query"></a>Tijdfilter in query
U kunt ook uw eigen tijdsbereik definiëren door een time-filter toe te voegen aan de query. Het is raadzaam te plaatsen van het tijdfilter direct na de naam van de tabel: 

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| where toint(Level) >= 10
```

In het bovenstaande tijdfilter `ago(30m)` "30 minuten geleden" betekent, zodat deze query alleen records uit de laatste 30 minuten retourneert. Andere tijdseenheden inclusief dagen (2d), minuten (25m) en seconden (per 10).


## <a name="project-and-extend-select-and-compute-columns"></a>Project en uitbreiden: Selecteer en kolommen berekenen
Gebruik **project** om specifieke kolommen om op te nemen in de resultaten te selecteren:

```Kusto
SecurityEvent 
| top 10 by TimeGenerated 
| project TimeGenerated, Computer, Activity
```

Het vorige voorbeeld wordt deze uitvoer gegenereerd:

![De resultaten van de query-project](media/get-started-queries/project.png)

U kunt ook **project** namen van kolommen wijzigen en nieuwe te definiëren. Project wordt het volgende voorbeeld het volgende doen:

* Selecteer alleen de *Computer* en *TimeGenerated* oorspronkelijke kolommen.
* Wijzig de naam van de *activiteit* kolom *EventDetails*.
* Maak een nieuwe kolom met de naam *EventCode*. De **substring()** functie wordt gebruikt om op te halen van alleen de eerste vier tekens van het veld activiteit.


```Kusto
SecurityEvent
| top 10 by TimeGenerated 
| project Computer, TimeGenerated, EventDetails=Activity, EventCode=substring(Activity, 0, 4)
```

**uitbreiden** houdt u alle oorspronkelijke kolommen in de resultatenset en nieuwe zijn in definieert. De volgende query gebruikt **uitbreiden** om toe te voegen een *lokale tijd* kolom, die een gelokaliseerde TimeGenerated-waarde bevat.

```Kusto
SecurityEvent
| top 10 by TimeGenerated
| extend localtime = TimeGenerated-8h
```

## <a name="summarize-aggregate-groups-of-rows"></a>Samenvatting: Rijgroepen samenvoegen
Gebruik **samenvatten** identificeren groepen records, op basis van een of meer kolommen en aggregaties op hen van toepassing. De meest voorkomende gebruik van **samenvatten** is *aantal*, die het aantal resultaten retourneert in elke groep.

De volgende query controleert alle *voor prestaties* records van het afgelopen uur, gegroepeerd door *ObjectName*, en de records in elke groep telt: 
```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName
```

Soms is het handig om groepen te definiëren voor meerdere dimensies. Elke unieke combinatie van deze waarden worden gedefinieerd voor een afzonderlijke groep:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName, CounterName
```

Er is een ander algemeen gebruik wiskundige of statistische berekeningen uitvoeren op elke groep. Bijvoorbeeld, het volgende wordt het gemiddelde wordt berekend *CounterValue* voor elke computer:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer
```

De resultaten van deze query zijn helaas geen betekenis heeft, omdat we verschillende prestatiemeters vermengd. Om dit meer af te stemmen, moeten we de gemiddelde afzonderlijk voor elke combinatie van berekenen *CounterName* en *Computer*:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer, CounterName
```

### <a name="summarize-by-a-time-column"></a>Samenvatten op een time-kolom
Resultaten groeperen kan ook worden gebaseerd op een time-kolom, of een andere continue waarde. Samenvatting van gewoon `by TimeGenerated` echter zou groepen maken voor elke één milliseconde gedurende het tijdsbereik, omdat dit unieke waarden zijn. 

Voor het maken van groepen op basis van doorlopende waarden, het is raadzaam om het bereik in beheerbare eenheden met behulp van **bin**. De volgende query worden geanalyseerd *Perf* records die het meten van beschikbaar geheugen (*beschikbare megabytes (MB)*) op een specifieke computer. Het berekenen van de gemiddelde waarde voor elke periode als 1 uur, gedurende de afgelopen 7 dagen:

```Kusto
Perf 
| where TimeGenerated > ago(7d)
| where Computer == "ContosoAzADDS2" 
| where CounterName == "Available MBytes" 
| summarize avg(CounterValue) by bin(TimeGenerated, 1h)
```

Als u de uitvoer duidelijker, schakelt u weer te geven als een tijdgrafiek van het beschikbare geheugen na verloop van tijd:

![Querygeheugen na verloop van tijd](media/get-started-queries/chart.png)



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [zoekquery's schrijven](search-queries.md)
