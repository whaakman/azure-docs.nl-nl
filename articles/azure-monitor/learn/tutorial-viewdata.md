---
title: Verzamelde gegevens van Azure Log Analytics weergeven of analyseren | Microsoft Docs
description: Dit artikel bevat een zelfstudie waarin wordt beschreven hoe u kunt zoeken in logboeken en de gegevens kunt analyseren die zijn opgeslagen in uw Log Analytics-resource via de portal Zoeken in logboeken.  In deze zelfstudie worden enkele eenvoudige query's uitgevoerd voor het retourneren van verschillende soorten gegevens en worden resultaten geanalyseerd.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/31/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: c64c0837956c59cf7a810bb06ca63bc21c5158d1
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186426"
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>Gegevens weergeven of analyseren die zijn verzameld met Zoeken in logboeken van Log Analytics

In Log Analytics kunt u logboekzoekopdrachten gebruiken door query's samen te stellen voor het analyseren van de verzamelde gegevens. U kunt hierbij gebruikmaken van bestaande dashboards die u kunt aanpassen met grafische weergaven van uw beste zoekopdrachten.  Nu u het verzamelen van operationele gegevens uit uw Azure VM's en activiteitenlogboeken hebt gedefinieerd, leert u in deze zelfstudie het volgende:

> [!div class="checklist"]
> * Een eenvoudige zoekopdracht naar gebeurtenisgegevens uitvoeren en functies gebruiken om de resultaten te filteren en te wijzigen 
> * Meer informatie over het werken met prestatiegegevens

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine [hebben verbonden met de Log Analytics-werkruimte](../../azure-monitor/learn/quick-collect-azurevm.md).  

Het maken en bewerken van query's kan, naast interactief werken met de geretourneerde gegevens, op twee manieren worden uitgevoerd.  Voor basisquery's gebruikt u de pagina Zoeken in logboeken in Azure Portal en voor geavanceerde query's kunt u de portal Geavanceerde analyse gebruiken. Zie voor meer informatie over het verschil in functionaliteit tussen de twee portals het onderwerp [Portals voor het maken en bewerken van logboekquery's in Azure Log Analytics](../../azure-monitor/log-query/portals.md)

In deze zelfstudie werken we met Zoeken in logboeken in Azure Portal. 

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="open-the-log-search-portal"></a>De portal Zoeken in logboeken openen 
Open eerst de portal Zoeken in logboeken.   

1. Klik in Azure Portal op **Alle services**. Typ **Monitor** in de lijst met resources. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Monitor**.
2. Selecteer in het navigatiemenu van Monitor de optie **Log Analytics** en selecteer vervolgens een werkruimte.

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

![Eenvoudige query](media/tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Alleen de eerste paar eigenschappen van elk record worden weergegeven.  Klik op **Meer weergeven** om alle eigenschappen van een bepaald record weer te geven.

## <a name="filter-results-of-the-query"></a>Resultaten van de query filteren
Aan de linkerkant van het scherm bevindt zich het filterdeelvenster waarin u filteropties kunt toevoegen aan de query zonder deze rechtstreeks te wijzigen.  Er worden eigenschappen van meerdere records weergegeven voor dat recordtype en u kunt een of meer eigenschapswaarden selecteren om uw zoekresultaten te verfijnen.

Als u werkt met een **gebeurtenis**, schakelt u het selectievakje in naast **Fout** onder **EVENTLEVELNAME**.   Als u met **Syslog** werkt, schakelt u het selectievakje naast **err** onder **SEVERITYLEVEL** in.  Hiermee wordt de query gewijzigd in een van het volgende om de resultaten te beperken tot foutgebeurtenissen.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filteren](media/tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Voeg eigenschappen toe aan het filterdeelvenster door **Toevoegen aan filters** te selecteren vanuit het eigenschappenmenu van een van de records.

![Toevoegen aan filtermenu](media/tutorial-viewdata/log-analytics-portal-eventlist-03.png)

U kunt hetzelfde filter instellen door **Filter** te selecteren in het eigenschappenmenu voor een record met de waarde die u wilt filteren.  

De optie **Filter** is alleen beschikbaar voor eigenschappen waarvan de naam blauw is wanneer u er met de muisaanwijzer over beweegt.  Dit zijn *doorzoekbare* velden die zijn geïndexeerd voor zoekvoorwaarden.  Grijze velden zijn *op vrije tekst doorzoekbare* velden waarvoor alleen de optie **Verwijzingen weergeven** beschikbaar is.  Met deze optie worden records geretourneerd die deze waarde in een eigenschap bevatten.

U kunt de resultaten op één eigenschap groeperen door de optie **Groeperen op** in het recordmenu te selecteren.  Hiermee voegt u de operator [samenvatten](/azure/kusto/query/summarizeoperator) toe aan de query die de resultaten in een grafiek weergeeft.  U kunt groeperen op meer dan één eigenschap, maar dan moet u de query rechtstreeks bewerken.  Selecteer het recordmenu naast de eigenschap **Computer** en selecteer **Groeperen op computer**.  

![Groeperen op computer](media/tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Werken met resultaten
De portal Zoeken in logboeken heeft een aantal functies voor het werken met de resultaten van een query.  U kunt resultaten sorteren, filteren en groeperen om de gegevens te analyseren zonder de werkelijke query te wijzigen.  Resultaten van een query worden niet standaard gesorteerd.

Als u de gegevens in tabelvorm wilt bekijken, zodat u over extra opties voor filteren en sorteren beschikt, klikt u op **Tabel**.  

![Tabelweergave](media/tutorial-viewdata/log-search-portal-table-01.png)

Klik op de pijl voor een record om de details voor dat record weer te geven.

![Resultaten sorteren](media/tutorial-viewdata/log-search-portal-table-02.png)

U kunt op elk veld sorteren door op de kolomkop te klikken.

![Resultaten sorteren](media/tutorial-viewdata/log-search-portal-table-03.png)

Filter de resultaten op een specifieke waarde in de kolom door te klikken op de filterknop en een filtervoorwaarde op te geven.

![Resultaten filteren](media/tutorial-viewdata/log-search-portal-table-04.png)

U kunt groeperen op een kolom door de kolomkop naar de bovenkant van de resultaten te slepen.  U kunt op meerdere velden groeperen door meerdere kolommen naar boven te slepen.

![Resultaten groeperen](media/tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Werken met prestatiegegevens
Prestatiegegevens voor Windows- en Linux-agents worden opgeslagen in de Log Analytics-werkruimte in de tabel **Perf**.  Prestatierecords zien er hetzelfde uit als alle andere records en we gaan een eenvoudige query schrijven waarmee alle prestatierecords op dezelfde manier worden geretourneerd als gebeurtenissen.

```
Perf
```

![Prestatiegegevens](media/tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Het retourneren van miljoenen records voor alle prestatieobjecten en -meteritems is niet erg nuttig.  U kunt dezelfde methoden gebruiken die u hierboven hebt gebruikt voor het filteren van de gegevens of de volgende query rechtstreeks in het zoekvak van het logboek typen.  Hiermee worden alleen processorgebruikrecords voor zowel Windows- en Linux-computers geretourneerd.

```
Perf | where ObjectName == "Processor"  | where CounterName == "% Processor Time"
```

![Processorgebruik](media/tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

Dit beperkt de gegevens tot een bepaald meteritem, maar plaatst deze nog niet in een vorm die handig is.  U kunt de gegevens weergegeven in een lijndiagram, maar u moet deze eerst groeperen op Computer en TimeGenerated.  Als u wilt groeperen op meerdere velden, moet u de query rechtstreeks wijzigen. Wijzig de query daarom als volgt.  Hiermee wordt de functie [avg](/azure/kusto/query/avg-aggfunction) in de eigenschap **CounterValue** gebruikt om de gemiddelde waarde voor elk uur te berekenen.

```
Perf  
| where ObjectName == "Processor"  | where CounterName == "% Processor Time"
| summarize avg(CounterValue) by Computer, TimeGenerated
```

![Prestatiegegevensgrafiek](media/tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Nu de gegevens naar behoren zijn gegroepeerd, kunt u deze weergeven in een visuele grafiek door de operator [render](/azure/kusto/query/renderoperator) toe te voegen.  

```
Perf  
| where ObjectName == "Processor" | where CounterName == "% Processor Time" 
| summarize avg(CounterValue) by Computer, TimeGenerated 
| render timechart
```

![Lijndiagram](media/tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u basiszoekopdrachten voor logboeken maakt om gebeurtenis- en prestatiegegevens te analyseren.  Ga naar de volgende zelfstudie voor meer informatie over hoe u de gegevens kunt visualiseren door een dashboard te maken.

> [!div class="nextstepaction"]
> [Log Analytics-dashboards maken en delen](tutorial-logs-dashboards.md)
