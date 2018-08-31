---
title: Met behulp van de portal zoeken in Logboeken in Azure Log Analytics | Microsoft Docs
description: In dit artikel bevat een zelfstudie waarin wordt beschreven hoe u Logboeken en analyseren van gegevens die zijn opgeslagen in uw Log Analytics-werkruimte met behulp van de portal zoeken in Logboeken.  In deze zelfstudie worden enkele eenvoudige query's uitgevoerd voor het retourneren van verschillende soorten gegevens en worden resultaten geanalyseerd.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 532df20a7639f42d8ba1c840a5fd19f0ad0e4042
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246330"
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Zoekopdrachten in Logboeken in Azure Log Analytics met behulp van de portal zoeken in Logboeken maken

In dit artikel bevat een zelfstudie waarin wordt beschreven hoe u Logboeken en analyseren van gegevens die zijn opgeslagen in uw Log Analytics-werkruimte met behulp van de portal zoeken in Logboeken.  In deze zelfstudie worden enkele eenvoudige query's uitgevoerd voor het retourneren van verschillende soorten gegevens en worden resultaten geanalyseerd.  Dit artikel gaat over functies in de portal zoeken in Logboeken voor de query te wijzigen in plaats van rechtstreeks wijzigen.  Zie voor meer informatie over het rechtstreeks bewerken van de query de [: referentie voor querytaal](https://go.microsoft.com/fwlink/?linkid=856079).

Zie voor informatie over het maken van zoekopdrachten in de portal Advanced Analytics in plaats van de portal zoeken in logboeken [aan de slag met de Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587).  Beide portals gebruiken dezelfde querytaal voor toegang tot dezelfde gegevens in de Log Analytics-werkruimte.

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie wordt ervan uitgegaan dat u al een Log Analytics-werkruimte met ten minste één verbonden bron die wordt gegenereerd voor de query's te analyseren.  

- Als u een werkruimte hebt, kunt u een gratis service met behulp van de procedure op [aan de slag met Log Analytics-werkruimte](log-analytics-get-started.md).
- Verbinding maken met ten minste één [Windows agent](log-analytics-windows-agent.md) of één [Linux-agent](log-analytics-linux-agents.md) naar de werkruimte.  

## <a name="open-the-log-search-portal"></a>De portal Zoeken in logboeken openen
Open eerst de portal Zoeken in logboeken. 

1. Open Azure Portal.
2. Navigeer naar Log Analytics en selecteer uw werkruimte.
3. Selecteer **Logboeken**.


## <a name="create-a-simple-search"></a>Een eenvoudige zoekopdracht maken
De snelste manier om gegevens op te halen om mee te werken, is met een eenvoudige query waarmee alle records in de tabel worden opgehaald.  Als u een Windows- of Linux-client hebt verbonden met uw werkruimte, hebt u gegevens in de gebeurtenis- (Windows) of Syslog-tabel (Linux).

Typ een van de volgende query's in het zoekvak en klik op de zoekknop.  

```
Event
```
```
Syslog
```

Gegevens worden geretourneerd in de standaardlijstweergave en u kunt zien hoeveel records er in totaal zijn geretourneerd.

![Eenvoudige query](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Alleen de eerste paar eigenschappen van elk record worden weergegeven.  Klik op **Meer weergeven** om alle eigenschappen van een bepaald record weer te geven.

![Details van record](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Stel het tijdsbereik
Elke record die worden verzameld door Log Analytics is een **TimeGenerated** eigenschap met de datum en tijd waarop de record is gemaakt.  Een query in de portal zoeken in Logboeken retourneert alleen records met een **TimeGenerated** binnen het bereik dat wordt weergegeven aan de linkerkant van het scherm.  

U kunt het tijdfilter wijzigen door de vervolgkeuzelijst selecteren of door het wijzigen van de schuifregelaar.  De schuifregelaar wordt een staafdiagram waarin het relatieve aantal records voor elk segment van de tijd binnen het bereik weergegeven.  Dit segment varieert, afhankelijk van het bereik.

Het bereik van de tijd standaard is **1 dag**.  Wijzig deze waarde in **7 dagen**, en het totale aantal records moet vergroten.

![Datum tijdsbereik](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Resultaten van de query filteren
Aan de linkerkant van het scherm bevindt zich het filterdeelvenster waarin u filteropties kunt toevoegen aan de query zonder deze rechtstreeks te wijzigen.  Verschillende eigenschappen van de records die worden geretourneerd worden met de waarden van de top 10 met het aantal records weergegeven.

Als u werkt met een **gebeurtenis**, schakelt u het selectievakje in naast **Fout** onder **EVENTLEVELNAME**.   Als u met **Syslog** werkt, schakelt u het selectievakje naast **err** onder **SEVERITYLEVEL** in.  Hiermee wordt de query gewijzigd in een van het volgende om de resultaten te beperken tot foutgebeurtenissen.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filteren](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Voeg eigenschappen toe aan het filterdeelvenster door **Toevoegen aan filters** te selecteren vanuit het eigenschappenmenu van een van de records.

![Toevoegen aan filtermenu](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

U kunt hetzelfde filter instellen door **Filter** te selecteren in het eigenschappenmenu voor een record met de waarde die u wilt filteren.  

U hoeft alleen de **Filter** optie Eigenschappen met de naam blauw.  Dit zijn *doorzoekbare* velden die zijn geïndexeerd voor zoekvoorwaarden.  Grijze velden zijn *op vrije tekst doorzoekbare* velden waarvoor alleen de optie **Verwijzingen weergeven** beschikbaar is.  Met deze optie worden records geretourneerd die deze waarde in een eigenschap bevatten.

![Filtermenu](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

U kunt de resultaten op één eigenschap groeperen door de optie **Groeperen op** in het recordmenu te selecteren.  Hiermee voegt u de operator [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) toe aan de query die de resultaten in een grafiek weergeeft.  U kunt groeperen op meer dan één eigenschap, maar dan moet u de query rechtstreeks bewerken.  Selecteer het recordmenu naast de eigenschap **Computer** en selecteer **Groeperen op computer**.  

![Groeperen op computer](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Werken met resultaten
De portal Zoeken in logboeken heeft een aantal functies voor het werken met de resultaten van een query.  U kunt resultaten sorteren, filteren en groeperen om de gegevens te analyseren zonder de werkelijke query te wijzigen.  Resultaten van een query worden niet standaard gesorteerd.

Als u de gegevens in tabelvorm wilt bekijken, zodat u over extra opties voor filteren en sorteren beschikt, klikt u op **Tabel**.  

![Tabelweergave](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Klik op de pijl voor een record om de details voor dat record weer te geven.

![Resultaten sorteren](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

U kunt op elk veld sorteren door op de kolomkop te klikken.

![Resultaten sorteren](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filter de resultaten op een specifieke waarde in de kolom door te klikken op de filterknop en een filtervoorwaarde op te geven.

![Resultaten filteren](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

U kunt groeperen op een kolom door de kolomkop naar de bovenkant van de resultaten te slepen.  U kunt op meerdere velden groeperen door meerdere kolommen naar boven te slepen.

![Resultaten groeperen](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Werken met prestatiegegevens
Prestatiegegevens voor Windows- en Linux-agents worden opgeslagen in de Log Analytics-werkruimte in de tabel **Perf**.  Prestatierecords zien er net als bij alle andere records en we een eenvoudige query waarmee alle prestatierecords op dezelfde manier als met gebeurtenissen geretourneerd kunt schrijven.

```
Perf
```

![Prestatiegegevens](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Het retourneren van miljoenen records voor alle prestatieobjecten en -meteritems is niet erg nuttig.  U kunt dezelfde methoden gebruiken die u hierboven hebt gebruikt voor het filteren van de gegevens of de volgende query rechtstreeks in het zoekvak van het logboek typen.  Hiermee worden alleen processorgebruikrecords voor zowel Windows- en Linux-computers geretourneerd.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processorgebruik](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Dit beperkt de gegevens tot een bepaald meteritem, maar plaatst deze nog niet in een vorm die handig is.  U kunt de gegevens weergegeven in een lijndiagram, maar u moet deze eerst groeperen op Computer en TimeGenerated.  Als u wilt groeperen op meerdere velden, moet u de query rechtstreeks wijzigen. Wijzig de query daarom als volgt.  Hiermee wordt de functie [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) in de eigenschap **CounterValue** gebruikt om de gemiddelde waarde voor elk uur te berekenen.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Prestatiegegevensgrafiek](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Nu de gegevens naar behoren zijn gegroepeerd, kunt u deze weergeven in een visuele grafiek door de operator [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) toe te voegen.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Lijndiagram](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de Log Analytics-querytaal op [aan de slag met de Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856079).
- Doorloop een zelfstudie met de [portal Advanced Analytics](https://go.microsoft.com/fwlink/?linkid=856587) waarmee u de dezelfde query's uitvoeren en toegang tot dezelfde gegevens als de portal zoeken in Logboeken.
