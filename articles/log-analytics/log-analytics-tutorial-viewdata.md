---
title: Weergeven of analyseren verzamelde gegevens van Azure Log Analytics | Microsoft Docs
description: Dit artikel bevat een zelfstudie waarin wordt beschreven hoe maken zoekopdrachten logboek en analyseren van gegevens die zijn opgeslagen in uw Log Analytics-resource met de portal logboek zoeken.  De zelfstudie omvat het uitvoeren van enkele eenvoudige query's voor verschillende soorten gegevens en analyseren van resultaten retourneren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/26/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: dfcbb925a16ca1e53d10b7bf70d03e62bc9dae69
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>Weergeven of gegevens die worden verzameld met logboekanalyse logboek search analyseren

In Log Analytics kunt u gebruikmaken van logboek zoekopdrachten door query's naar de verzamelde gegevens te analyseren, gebruikt u vooraf bestaande dashboards die u kunt aanpassen met grafische weergave van uw waardevolste zoekopdrachten samen te stellen.  Nu dat u het verzamelen van operationele gegevens van uw Azure VM's en activiteitenlogboeken hebt gedefinieerd, in deze zelfstudie leert u hoe:

> [!div class="checklist"]
> * Uw Azure-logboekanalyse-bron upgraden naar de nieuwe querytaal 
> * Uitvoeren van een eenvoudige zoekopdrachten gebeurtenisgegevens en functies gebruiken om te wijzigen en de resultaten filteren 
> * Meer informatie over het werken met prestatiegegevens

Als u het voorbeeld in deze zelfstudie, moet u een bestaande virtuele machine hebben [verbonden met de werkruimte voor logboekanalyse](log-analytics-quick-collect-azurevm.md).  

Maken en bewerken van query's, naast interactief werken met de geretourneerde gegevens kunnen uitgevoerde een van twee manieren zijn.  De pagina logboek zoeken in de Azure portal gebruiken voor eenvoudige query of geavanceerde query's, kunt u de portal geavanceerde analyses. Zie voor meer informatie over het verschil in functionaliteit tussen de twee portals [Portals voor het maken en bewerken van logboek-query's in Azure Log Analytics](log-analytics-log-search-portals.md)

In deze zelfstudie werken we met logboek Search in de Azure portal. 

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure portal
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="open-the-log-search-portal"></a>De portal logboek zoeken openen 
Start via de portal logboek zoeken.   

1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
2. Selecteer een werkruimte in het deelvenster Log Analytics-abonnementen en selecteer vervolgens de **logboek zoeken** tegel.<br> ![Meld u knop Zoeken](media/log-analytics-tutorial-viewdata/azure-portal-01.png)

Hebt u wellicht de banner aan de bovenkant van uw pagina logboekanalyse resource in de portal uitnodiging om bij te werken.<br> ![Log Analytics upgrade wordt aangekondigd in de Azure portal](media/log-analytics-tutorial-viewdata/log-analytics-portal-upgradebanner.png)

Een nieuwe querytaal logboekanalyse onlangs toegevoegd aan zorgt ervoor dat het eenvoudiger om samen te stellen query's, vergelijken gegevens uit verschillende gegevensbronnen en analyseren om snel trends of problemen te identificeren.

Een upgrade is eenvoudig.  Het proces starten door te klikken op de banner waarin staat dat **meer informatie en upgrade**.  Lees de aanvullende informatie over de upgrade op de pagina met informatie over de upgrade en klik vervolgens op **nu bijwerken**.

Het proces wordt een paar minuten duren en gedurende deze tijd kunt u de voortgang onder volgen **meldingen** in het menu. U kunt meer lezen over de [voordelen van de nieuwe querytaal](log-analytics-log-search-upgrade.md#why-the-new-language).

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

![Eenvoudige query](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Alleen de eerste enkele eigenschappen van elke record worden weergegeven.  Klik op **meer** om alle eigenschappen voor een bepaalde record weer te geven.

## <a name="filter-results-of-the-query"></a>Filteren van resultaten van de query
Is het filterdeelvenster dat kunt u filteren op in de query zonder het te wijzigen rechtstreeks toevoegen aan de linkerkant van het scherm.  Eigenschappen van meerdere records voor dat recordtype worden weergegeven en kunt u een of meer eigenschapswaarden om uw zoekresultaten te verfijnen.

Als u met werkt **gebeurtenis**, schakel het selectievakje in naast **fout** onder **EVENTLEVELNAME**.   Als u met werkt **Syslog**, schakel het selectievakje in naast **err** onder **FOUTCODE**.  Hiermee wordt de query gewijzigd in een van de volgende om de resultaten op foutgebeurtenissen te beperken.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filteren](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Eigenschappen voor het filterdeelvenster toevoegen door te selecteren **toevoegen aan filters** vanuit het menu van de eigenschap op een van de records.

![Toevoegen aan het filtermenu](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-03.png)

U kunt hetzelfde filter instellen door het selecteren van **Filter** in het menu van de eigenschap voor een record met de waarde die u wilt filteren.  

U hoeft alleen de **Filter** optie voor eigenschappen met de naam in blauw wanneer u de muisaanwijzer.  Dit zijn *doorzoekbare* velden die zijn geïndexeerd voor voorwaarden zoeken.  Velden grijs zijn *vrije tekst doorzoekbare* velden die alleen de **verwijzingen** optie.  Deze optie retourneert records die deze waarde in een eigenschap.

U kunt de resultaten op één eigenschap groeperen door het selecteren van de **groeperen op** optie in het menu record.  Hiermee voegt u toe een [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operator aan de query die de resultaten in een grafiek weergegeven.  U kunt groeperen op meer dan één eigenschap, maar u moet de query rechtstreeks bewerken.  Selecteer het menu record naast de de **Computer** eigenschap en selecteer **groeperen op 'Computer'**.  

![Groeperen op computer](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Werken met resultaten
De portal zoeken logboek heeft een aantal functies voor het werken met de resultaten van een query.  U kunt sorteren, filter en de resultaten van Groepsbeleid voor het analyseren van de gegevens zonder de werkelijke query te wijzigen.  Resultaten van een query worden niet standaard gesorteerd.

Bekijk de gegevens in tabelvorm, wat zorgt voor extra opties voor filteren en sorteren, klikt u op **tabel**.  

![Tabelweergave](media/log-analytics-tutorial-viewdata/log-search-portal-table-01.png)

Klik op de pijl door een record weergeven van de details voor deze record.

![Resultaten sorteren](media/log-analytics-tutorial-viewdata/log-search-portal-table-02.png)

Sorteren op elk veld door te klikken op de kolomkop.

![Resultaten sorteren](media/log-analytics-tutorial-viewdata/log-search-portal-table-03.png)

De resultaten van een specifieke waarde in de kolom door te klikken op de filterknop en het leveren van een filtervoorwaarde filteren.

![Resultaten filteren](media/log-analytics-tutorial-viewdata/log-search-portal-table-04.png)

Groeperen op een kolom met de kolomkop naar de bovenkant van de resultaten te slepen.  U kunt meerdere velden groeperen door meerdere kolommen naar boven te slepen.

![Resultaten van Groepsbeleid](media/log-analytics-tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Werken met prestatiegegevens
Prestatiegegevens voor Windows- en Linux-agents wordt opgeslagen in de werkruimte voor logboekanalyse in de **Perf** tabel.  Prestatiegegevens zoekt net als elke andere record en we gaan een eenvoudige query schrijven waarmee retourneert dat alle records van de prestaties op dezelfde manier als met gebeurtenissen.

```
Perf
```

![Prestatiegegevens](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Hoewel miljoenen records voor alle prestatieobjecten en prestatiemeteritems retourneren is niet erg nuttig.  U kunt de methodes die u hierboven hebt gebruikt voor het filter de gegevens of typ de volgende query rechtstreeks in het zoekvak van het logboek.  Hiermee wordt alleen processor gebruik records voor zowel Windows- en Linux-computers.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processorgebruik](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

Dit beperkt de gegevens naar een bepaald item, maar deze nog niet plaatsen in een formulier dat is bijzonder nuttig.  U kunt de gegevens in een lijndiagram worden weergegeven, maar u moet eerst het groeperen van Computer- en TimeGenerated.  Als u wilt groeperen op meerdere velden, moet u de query rechtstreeks wijzigen, zodat de query met de volgende wijzigen.  Dit maakt gebruik van de [Gem](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) functioneren in de **tegenwaarde** eigenschap bij de berekening van de gemiddelde waarde voor elk uur.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Prestaties van een grafiek gegevensbron](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Nu dat de gegevens worden naar behoren gegroepeerd, kunt u deze weergeven in een grafiek visual door toe te voegen de [renderen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Lijndiagram](media/log-analytics-tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd basic logboek van zoekopdrachten voor het analyseren van gebeurtenissen en prestatiegegevens maken.  Ga naar de volgende zelfstudie voor meer informatie over hoe de gegevens visualiseren door het maken van een dashboard.

> [!div class="nextstepaction"]
> [Maken en delen van logboekanalyse dashboards](log-analytics-tutorial-dashboards.md)