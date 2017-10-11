---
title: Met behulp van de portal logboek zoeken in Azure Log Analytics | Microsoft Docs
description: Dit artikel bevat een zelfstudie waarin wordt beschreven hoe maken zoekopdrachten logboek en analyseren van gegevens die zijn opgeslagen in de werkruimte voor logboekanalyse via de portal logboek zoeken.  De zelfstudie omvat het uitvoeren van enkele eenvoudige query's voor verschillende soorten gegevens en analyseren van resultaten retourneren.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 6fc556ceb34cde26d5f3789a2397cdaa34b0b84d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Logboek zoekopdrachten maken in Azure-logboekanalyse via de portal logboek zoeken

> [!NOTE]
> Dit artikel wordt beschreven in de Azure-logboekanalyse met behulp van de nieuwe query language van de portal logboek zoeken.  U kunt meer informatie over de nieuwe taal en ophalen van de procedure voor het bijwerken van de werkruimte op [Upgrade van uw Azure-logboekanalyse-werkruimte naar nieuwe logboek zoekopdracht](log-analytics-log-search-upgrade.md).  
>
> Als uw werkruimte is niet naar de nieuwe query language bijgewerkt, moet u verwijzen naar [vinden van gegevens met behulp van logboek zoekopdrachten in logboekanalyse](log-analytics-log-searches.md) voor meer informatie over de huidige versie van de portal logboek zoeken.

Dit artikel bevat een zelfstudie waarin wordt beschreven hoe maken zoekopdrachten logboek en analyseren van gegevens die zijn opgeslagen in de werkruimte voor logboekanalyse via de portal logboek zoeken.  De zelfstudie omvat het uitvoeren van enkele eenvoudige query's voor verschillende soorten gegevens en analyseren van resultaten retourneren.  Dit artikel gaat over functies in de portal logboek zoeken voor de query te wijzigen in plaats van rechtstreeks wijzigen.  Zie voor meer informatie over het rechtstreeks bewerken van de query de [Query Language reference](https://go.microsoft.com/fwlink/?linkid=856079).

Zie voor informatie over het maken van zoekopdrachten in de portal Advanced Analytics in plaats van de portal zoeken logboek [aan de slag met de Portal Analytics](https://go.microsoft.com/fwlink/?linkid=856587).  Beide portals gebruiken dezelfde querytaal voor toegang tot dezelfde gegevens in de werkruimte voor logboekanalyse.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u al een werkruimte voor logboekanalyse met ten minste één verbonden bron die gegevens voor de query's hebt genereert te analyseren.  

- Als u een werkruimte geen hebt, kunt u een gratis abonnement met de procedure op [aan de slag met een werkruimte voor logboekanalyse](log-analytics-get-started.md).
- Verbinding maken met ten minste één [Windows-agent](log-analytics-windows-agents.md) of één [Linux-agent](log-analytics-linux-agents.md) naar de werkruimte.  

## <a name="open-the-log-search-portal"></a>De portal logboek zoeken openen
Start via de portal logboek zoeken.  U kunt openen in de Azure-portal of de OMS-portal.

1. Open de Azure-portal.
2. Navigeer met logboekanalyse en selecteer uw werkruimte.
3. Selecteer **logboek zoeken** blijven in de Azure portal of start de OMS-portal door te selecteren **OMS-Portal** en vervolgens te klikken op de knop logboek zoeken.

![Meld u knop Zoeken](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Maken van een eenvoudige zoekopdrachten
De snelste manier om op te halen van sommige gegevens voor gebruik met is een eenvoudige query die als resultaat alle records in de tabel geeft.  Als u hebt een Windows- of Linux-clients die zijn verbonden met uw werkruimte vervolgens hebt u gegevens in de gebeurtenis (Windows) of de tabel Syslog (Linux).

Typ een de volgende query's in het zoekvak en klik op de zoekknop.  

```
Event
```
```
Syslog
```

Gegevens worden geretourneerd in de standaardlijstweergave en kunt u zien hoeveel totale records geretourneerd.

![Eenvoudige query](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Alleen de eerste enkele eigenschappen van elke record worden weergegeven.  Klik op **meer** om alle eigenschappen voor een bepaalde record weer te geven.

![Details van de records](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Het bereik van de tijd instellen
Elke record die is verzameld door Log Analytics is een **TimeGenerated** eigenschap met de datum en tijd waarop de record is gemaakt.  Een query in de portal logboek Search retourneert alleen records met een **TimeGenerated** binnen het bereik voor tijd die aan de linkerkant van het scherm wordt weergegeven.  

U kunt de tijdfilter wijzigen door de vervolgkeuzelijst selecteren of door het wijzigen van de schuifregelaar.  De schuifregelaar wordt weergegeven een staafdiagram waarin het relatieve aantal records voor elk segment tijd binnen het bereik.  Dit segment is afhankelijk van het bereik.

Het standaardbereik van de tijd is **1 dag**.  Wijzig deze waarde in **7 dagen**, en het totale aantal records moet verhogen.

![Datum tijd bereik](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filteren van resultaten van de query
Is het filterdeelvenster dat kunt u filteren op in de query zonder het te wijzigen rechtstreeks toevoegen aan de linkerkant van het scherm.  Verschillende eigenschappen van de records geretourneerd worden met de top 10 van waarden met hun aantal records weergegeven.

Als u met werkt **gebeurtenis**, schakel het selectievakje in naast **fout** onder **EVENTLEVELNAME**.   Als u met werkt **Syslog**, schakel het selectievakje in naast **err** onder **FOUTCODE**.  Hiermee wordt de query gewijzigd in een van de volgende om de resultaten op foutgebeurtenissen te beperken.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filteren](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Eigenschappen voor het filterdeelvenster toevoegen door te selecteren **toevoegen aan filters** vanuit het menu van de eigenschap op een van de records.

![Toevoegen aan het filtermenu](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

U kunt hetzelfde filter instellen door het selecteren van **Filter** in het menu van de eigenschap voor een record met de waarde die u wilt filteren.  

U hoeft alleen de **Filter** optie voor eigenschappen met de naam in blauw.  Dit zijn *doorzoekbare* velden die zijn geïndexeerd voor voorwaarden zoeken.  Velden grijs zijn *vrije tekst doorzoekbare* velden die alleen de **verwijzingen** optie.  Deze optie retourneert records die deze waarde in een eigenschap.

![Filtermenu](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

U kunt de resultaten op één eigenschap groeperen door het selecteren van de **groeperen op** optie in het menu record.  Hiermee voegt u toe een [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operator aan de query die de resultaten in een grafiek weergegeven.  U kunt groeperen op meer dan één eigenschap, maar u moet de query rechtstreeks bewerken.  Selecteer het menu record naast de de **Computer** eigenschap en selecteer **groeperen op 'Computer'**.  

![Groeperen op computer](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Werken met resultaten
De portal zoeken logboek heeft een aantal functies voor het werken met de resultaten van een query.  U kunt sorteren, filter en de resultaten van Groepsbeleid voor het analyseren van de gegevens zonder de werkelijke query te wijzigen.  Resultaten van een query worden niet standaard gesorteerd.

Bekijk de gegevens in tabelvorm, wat zorgt voor extra opties voor filteren en sorteren, klikt u op **tabel**.  

![Tabelweergave](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Klik op de pijl door een record weergeven van de details voor deze record.

![Resultaten sorteren](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Sorteren op elk veld door te klikken op de kolomkop.

![Resultaten sorteren](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

De resultaten van een specifieke waarde in de kolom door te klikken op de filterknop en het leveren van een filtervoorwaarde filteren.

![Resultaten filteren](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Groeperen op een kolom met de kolomkop naar de bovenkant van de resultaten te slepen.  U kunt meerdere velden groeperen door meerdere kolommen naar boven te slepen.

![Resultaten van Groepsbeleid](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Werken met prestatiegegevens
Prestatiegegevens voor Windows- en Linux-agents wordt opgeslagen in de werkruimte voor logboekanalyse in de **Perf** tabel.  Prestatiegegevens net als elke andere record zoeken en we een eenvoudige query waarin retourneert dat alle records van de prestaties op dezelfde manier als met gebeurtenissen kunt schrijven.

```
Perf
```

![Prestatiegegevens](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Hoewel miljoenen records voor alle prestatieobjecten en prestatiemeteritems retourneren is niet erg nuttig.  U kunt de methodes die u hierboven hebt gebruikt voor het filter de gegevens of typ de volgende query rechtstreeks in het zoekvak van het logboek.  Hiermee wordt alleen processor gebruik records voor zowel Windows- en Linux-computers.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processorgebruik](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Dit beperkt de gegevens naar een bepaald item, maar deze nog niet plaatsen in een formulier dat is bijzonder nuttig.  U kunt de gegevens in een lijndiagram worden weergegeven, maar u moet eerst het groeperen van Computer- en TimeGenerated.  Als u wilt groeperen op meerdere velden, moet u de query rechtstreeks wijzigen, zodat de query met de volgende wijzigen.  Dit maakt gebruik van de [Gem](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) functioneren in de **tegenwaarde** eigenschap bij de berekening van de gemiddelde waarde voor elk uur.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Prestaties van een grafiek gegevensbron](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Nu dat de gegevens worden naar behoren gegroepeerd, kunt u deze weergeven in een grafiek visual door toe te voegen de [renderen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Lijndiagram](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de Log Analytics query language op [aan de slag met de Portal Analytics](https://go.microsoft.com/fwlink/?linkid=856079).
- Een zelfstudie met Doorloop de [Advanced Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587) waarmee u de dezelfde query's uitvoeren en toegang hebben tot dezelfde gegevens als de portal logboek zoeken.
