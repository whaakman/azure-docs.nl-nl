---
title: Azure-logboekanalyse gegevens importeren in Power BI | Microsoft Docs
description: Power BI is een cloudgebaseerde business analytics-service van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets van gegevens biedt.  In dit artikel wordt beschreven hoe u Log Analytics-gegevens in Power BI importeren en configureren om automatisch te vernieuwen.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: bwren
ms.openlocfilehash: e687a1ee8ac4f565062e57b07cdfa9ac5e6bbf4f
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Azure-logboekanalyse gegevens importeren in Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is een service in de cloud business analytics van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets van gegevens biedt.  U kunt de resultaten van een zoekopdracht Log Analytics-logboek importeren in een gegevensset met de Power BI zodat u kunt profiteren van de functies zoals combineren van gegevens uit verschillende bronnen en het delen van rapporten op het web en mobiele apparaten.

In dit artikel biedt details over het importeren van Log Analytics-gegevens in Power BI en planning van deze automatisch worden vernieuwd.  Er zijn verschillende processen opgenomen voor een [bijgewerkt](#upgraded-workspace) en een [verouderde](#legacy-workspace) werkruimte.

## <a name="upgraded-workspace"></a>Bijgewerkte werkruimte


Voor het importeren van gegevens uit een [bijgewerkt van de werkruimte voor logboekanalyse](log-analytics-log-search-upgrade.md) in Power BI kunt u een gegevensset maken in Power BI op basis van een zoekquery logboek in logboekanalyse.  De query wordt uitgevoerd telkens wanneer die de dataset wordt vernieuwd.  Vervolgens kunt u Power BI-rapporten die gebruikmaken van gegevens uit de gegevensset.  Voor het maken van de gegevensset in Power BI, exporteren van uw query van logboekanalyse naar [Power Query (M) taal](https://msdn.microsoft.com/library/mt807488.aspx).  U dit vervolgens gebruiken voor het maken van een query in Power BI Desktop en vervolgens publiceren naar Power BI als een gegevensset.  Hieronder vindt u de details voor dit proces.

![Log Analytics naar Power BI](media/log-analytics-powerbi/overview.png)

### <a name="export-query"></a>Export-query
Eerst maakt u een [logboek zoeken](log-analytics-log-search-new.md) die de gegevens retourneert van logboekanalyse dat u wilt de Power BI-gegevensset te vullen.  U deze query te exporteren [Power Query (M) taal](https://msdn.microsoft.com/library/mt807488.aspx) die kan worden gebruikt door Power BI Desktop.

1. De zoekopdracht logboek maken in logboekanalyse uitpakken van de gegevens voor uw gegevensset.
2. Als u de zoekopdracht logboek portal gebruikt, klikt u op **Power BI**.  Als u het Analytics-portal, selecteert u **exporteren** > **Power BI Query (M)**.  Beide opties de query exporteren naar een tekstbestand aangeroepen **PowerBIQuery.txt**. 

    ![Logboek zoekopdracht exporteren](media/log-analytics-powerbi/export-logsearch.png) ![Logboek zoekopdracht exporteren](media/log-analytics-powerbi/export-analytics.png)

3. Open het tekstbestand en kopieer de inhoud.

### <a name="import-query-into-power-bi-desktop"></a>Query importeren in Power BI Desktop
Power BI Desktop is een bureaubladtoepassing waarmee u gegevenssets en rapporten maken die kunnen worden gepubliceerd naar Power BI.  U kunt deze ook gebruiken voor het maken van een query met de Power Query-taal geëxporteerd van logboekanalyse. 

1. Installeer [Power BI Desktop](https://powerbi.microsoft.com/desktop/) als u niet al op uw computer en vervolgens de toepassing open.
2. Selecteer **gegevens ophalen** > **leeg Query** om een nieuwe query te openen.  Selecteer vervolgens **geavanceerde Editor** en plak de inhoud van het geëxporteerde bestand in de query. Klik op **Gereed**.

    ![Power BI Desktop-query](media/log-analytics-powerbi/desktop-new-query.png)

5. De query wordt uitgevoerd, en de resultaten worden weergegeven.  U wordt mogelijk gevraagd om referenties voor verbinding met Azure.  
6. Typ een beschrijvende naam voor de query.  De standaardwaarde is **Query1**. Klik op **sluiten en toepassen** de gegevensset toevoegen aan het rapport.

    ![De naam van Power BI Desktop](media/log-analytics-powerbi/desktop-results.png)



### <a name="publish-to-power-bi"></a>Publiceren naar Power BI
Wanneer u naar Power BI publiceert, worden een gegevensset en een rapport gemaakt.  Als u een rapport in Power BI Desktop maakt, zal vervolgens dit worden gepubliceerd met uw gegevens.  Als dat niet het geval is, wordt een leeg rapport gemaakt.  U kunt het rapport in Power BI wijzigen of een nieuwe maken op basis van de gegevensset.

8. Maak een rapport op basis van uw gegevens.  Gebruik [Power BI Desktop documentatie](https://docs.microsoft.com/power-bi/desktop-report-view) als u niet bekend met het bent.  Wanneer u klaar bent om te verzenden naar Power BI, klikt u op **publiceren**.  Wanneer u wordt gevraagd, selecteer een doel in uw Power BI-account.  Tenzij u een specifieke bestemming rekening hebt, gebruik **mijn werkruimte**.

    ![Power BI Desktop publiceren](media/log-analytics-powerbi/desktop-publish.png)

3. Wanneer het publiceren is voltooid, klikt u op **openen in Power BI** Power BI openen met uw nieuwe gegevensset.


### <a name="configure-scheduled-refresh"></a>Geplande vernieuwing configureren
De gegevensset is gemaakt in Power BI heeft dezelfde gegevens die u eerder hebt gezien in Power BI Desktop.  U moet de gegevensset periodiek om de query opnieuw uitvoeren en deze vullen met de meest recente gegevens van logboekanalyse vernieuwen.  

1. Klik op de werkruimte waarnaar u geüpload uw rapport op en selecteer de **gegevenssets** menu. Selecteer het snelmenu naast uw nieuwe gegevensset en selecteer **instellingen**. Onder **gegevensbronreferenties** er is een bericht dat de referenties ongeldig zijn.  Dit is omdat u referenties voor de gegevensset moet worden gebruikt wanneer deze de gegevens worden vernieuwd nog niet hebt opgegeven.  Klik op **bewerken van referenties** en referenties opgeven die toegang hebben tot logboekanalyse.

    ![Planning voor Power BI](media/log-analytics-powerbi/powerbi-schedule.png)

5. Onder **geplande vernieuwing** schakelt u de optie voor het **up-to-date houden van uw gegevens**.  U kunt desgewenst wijzigen de **vernieuwingsfrequentie** en een of meer specifieke tijdstippen worden uitgevoerd van de vernieuwing.

    ![Power BI vernieuwen](media/log-analytics-powerbi/powerbi-schedule-refresh.png)

## <a name="legacy-workspace"></a>Verouderde werkruimte
Wanneer u Power BI met configureert een [verouderde werkruimte voor logboekanalyse](log-analytics-powerbi.md), u logboek query's maken die de resultaten naar overeenkomende gegevenssets in Power BI exporteren.  De query en uitvoer blijft automatisch moet worden uitgevoerd volgens een planning die u definieert voor de gegevensset up-to-date te houden met de meest recente gegevens verzameld door logboekanalyse.

![Log Analytics naar Power BI](media/log-analytics-powerbi/overview-legacy.png)

### <a name="power-bi-schedules"></a>Power BI-schema 's
Een *Power BI planning* bevat een logboek zoeken waarmee een reeks gegevens van logboekanalyse geëxporteerd naar de bijbehorende gegevensset in Power BI en een planning die hoe vaak deze zoekopdracht wordt uitgevoerd definieert voor de gegevensset actueel te houden.

De velden in de gegevensset komt overeen met de eigenschappen van de records geretourneerd door het logboek zoeken.  Als de zoekopdracht records van verschillende typen retourneert vervolgens bevat de gegevensset alle eigenschappen van elk van de opgenomen recordtypen.  

### <a name="connecting-log-analytics-workspace-to-power-bi"></a>Werkruimte voor logboekanalyse verbinding met Power BI
Voordat u van logboekanalyse naar Power BI exporteren kunt, moet u uw werkruimte verbinden met uw Power BI-account met de volgende procedure.  

1. Klik in de OMS-console op de **instellingen** tegel.
2. Selecteer **Accounts**.
3. In de **werkruimte** sectie Klik **verbinding maken met Power BI-Account**.
4. Voer de referenties voor uw Power BI-account.

### <a name="create-a-power-bi-schedule"></a>Maak een planning van Power BI
Maak een Power BI-schema voor elke gegevensset met de volgende procedure.

1. Klik in de OMS-console op de **logboek zoeken** tegel.
2. Typ in een nieuwe query of Selecteer een opgeslagen zoekopdracht die de gegevens waarop u exporteren retourneert wilt naar **Power BI**.  
3. Klik op de **Power BI** knop aan de bovenkant van de pagina opent de **Power BI** dialoogvenster.
4. Geef de informatie in de volgende tabel en klik op **opslaan**.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Naam |De naam om het schema te identificeren wanneer u de lijst met Power BI-schema's weergeven. |
| Opgeslagen zoekopdracht |De zoekopdracht logboek om uit te voeren.  Selecteer de huidige query of Selecteer een bestaande opgeslagen zoekopdracht in de vervolgkeuzelijst. |
| Planning |Hoe vaak de opgeslagen zoekopdracht uitvoeren en exporteren met de Power BI-gegevensset.  De waarde moet tussen 15 minuten en 24 uur. |
| De naam van de gegevensset |De naam van de gegevensset in Power BI.  Deze wordt gemaakt als deze niet bestaat en wordt bijgewerkt als het bestand bestaat. |

### <a name="viewing-and-removing-power-bi-schedules"></a>Weergeven en verwijderen van Power BI-schema 's
De lijst met bestaande Power BI-schema's met de volgende procedure weergeven.

1. Klik in de OMS-console op de **instellingen** tegel.
2. Selecteer **Power BI**.

Naast de details van de planning, worden het aantal keren dat met de planning wordt uitgevoerd in de afgelopen week en de status van de laatste synchronisatie weergegeven.  Als de synchronisatie fouten optreden, kunt u de koppeling om uit te voeren een zoekopdracht logboek voor records met gegevens over de fout kunt klikken.

U kunt een schema verwijderen door te klikken op de **X** in de **verwijderen kolom**.  U kunt een planning uitschakelen door het selecteren van **uit**.  Voor het wijzigen van een schema moet u deze verwijderen en maak deze opnieuw met de nieuwe instellingen.

![Power BI-schema 's](media/log-analytics-powerbi/schedules.png)

### <a name="sample-walkthrough"></a>Voorbeeld-overzicht
De volgende sectie wordt een voorbeeld van een Power BI-schema maken en de bijbehorende gegevensset gebruiken voor het maken van een eenvoudig rapport uitgelegd.  In dit voorbeeld alle prestatiegegevens voor een set computers wordt geëxporteerd naar Power BI en vervolgens een lijngrafiek gemaakt om processorgebruik weer te geven.

#### <a name="create-log-search"></a>Logboek zoekopdracht maken
Begin met het maken van een zoekopdracht logboek voor de gegevens die we willen verzenden naar de gegevensset.  In dit voorbeeld gebruiken we een query alle prestatiegegevens voor computers met een naam die begint retourneert met *srv*.  

![Power BI-schema 's](media/log-analytics-powerbi/walkthrough-query.png)

#### <a name="create-power-bi-search"></a>Zoeken met Power BI maken
We klikt u op de **Power BI** knop opent u het dialoogvenster Power BI en geef de vereiste gegevens.  We willen dat deze zoekopdracht uitvoeren één keer per uur en maken van een gegevensset met de naam *Contoso Perf*.  Aangezien we al de zoekopdracht openen die de gegevens die we willen maakt hebben, wordt de standaardwaarde van houden *huidige zoekquery gebruiken* voor **opgeslagen zoekopdrachten**.

![Zoeken met Power BI](media/log-analytics-powerbi/walkthrough-schedule.png)

#### <a name="verify-power-bi-search"></a>Verify Power BI Search
Om te controleren of we de planning correct hebt gemaakt, wordt de lijst weergeven met Power BI zoekopdrachten op het **instellingen** -tegel in het dashboard OMS.  Er wacht een paar minuten en vernieuw deze weergave totdat deze rapporteert dat de synchronisatie is uitgevoerd.  U gaat doorgaans de gegevensset automatisch vernieuwen plannen.

![Zoeken met Power BI](media/log-analytics-powerbi/walkthrough-schedules.png)

#### <a name="verify-the-dataset-in-power-bi"></a>Controleer of de gegevensset in Power BI
We Meld u aan bij onze account bij [powerbi.microsoft.com](http://powerbi.microsoft.com/) en schuif naar **gegevenssets** onderaan in het linkerdeelvenster.  U ziet dat de *Contoso Perf* gegevensset wordt vermeld die aangeeft dat de export is uitgevoerd.

![Power BI-gegevensset](media/log-analytics-powerbi/walkthrough-datasets.png)

#### <a name="create-report-based-on-dataset"></a>Maken op basis van gegevensset
U selecteert de **Contoso Perf** gegevensset en klik vervolgens op **resultaten** in de **velden** deelvenster op het recht om de velden die deel van deze gegevensset uitmaken te bekijken.  We uitvoeren de volgende acties voor het maken van een lijngrafiek processorgebruik voor elke computer weergegeven.

1. Selecteer de regel grafiek visualisatie.
2. Sleep **ObjectName** naar **rapport niveau filter** en Controleer **Processor**.
3. Sleep **CounterName** naar **rapport niveau filter** en Controleer **percentage processortijd**.
4. Sleep **tegenwaarde** naar **waarden**.
5. Sleep **Computer** naar **legenda**.
6. Sleep **TimeGenerated** naar **as**.

U ziet dat de resulterende lijngrafiek wordt weergegeven met de gegevens van onze gegevensset.

![Power BI-lijndiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

#### <a name="save-the-report"></a>Het rapport opslaan
We het rapport door te klikken op de knop opslaan boven aan het scherm opslaan en valideren nu wordt vermeld in de sectie rapporten in het linkerdeelvenster.

![Power BI-rapporten](media/log-analytics-powerbi/walkthrough-report.png)



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) voor query's opbouwen die kunnen worden geëxporteerd naar Power BI.
* Meer informatie over [Power BI](http://powerbi.microsoft.com) visualisaties op basis van logboekanalyse uitvoer bouwen.
