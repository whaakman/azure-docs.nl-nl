---
title: Azure-logboekanalyse gegevens importeren in Power BI | Microsoft Docs
description: Power BI is een cloudgebaseerde business analytics-service van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets van gegevens biedt.  In dit artikel wordt beschreven hoe configureren en Log Analytics-gegevens in Power BI importeren en configureren om automatisch te vernieuwen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: fb05ddabab3702299df0e81e8dda5af5cb676c1a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127506"
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Azure-logboekanalyse gegevens importeren in Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is een service in de cloud business analytics van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets van gegevens biedt.  U kunt de resultaten van een zoekopdracht Log Analytics-logboek importeren in een gegevensset met de Power BI zodat u van de functies profiteren kunt zoals het combineren van gegevens uit verschillende bronnen en het delen van rapporten op het web en mobiele apparaten.

## <a name="overview"></a>Overzicht
Om gegevens te importeren uit een werkruimte voor logboekanalyse Power BI, maakt u een gegevensset in Power BI op basis van een zoekquery logboek in logboekanalyse.  De query wordt uitgevoerd telkens wanneer die de dataset wordt vernieuwd.  Vervolgens kunt u Power BI-rapporten die gebruikmaken van gegevens uit de gegevensset.  Voor het maken van de gegevensset in Power BI, exporteren van uw query van logboekanalyse naar [Power Query (M) taal](https://msdn.microsoft.com/library/mt807488.aspx).  U dit vervolgens gebruiken voor het maken van een query in Power BI Desktop en vervolgens publiceren naar Power BI als een gegevensset.  Hieronder vindt u de details voor dit proces.

![Log Analytics naar Power BI](media/log-analytics-powerbi/overview.png)

## <a name="export-query"></a>Export-query
Eerst maakt u een [logboek zoeken](log-analytics-log-search-new.md) die de gegevens retourneert van logboekanalyse dat u wilt de Power BI-gegevensset te vullen.  U deze query te exporteren [Power Query (M) taal](https://msdn.microsoft.com/library/mt807488.aspx) die kan worden gebruikt door Power BI Desktop.

1. De zoekopdracht logboek maken in logboekanalyse uitpakken van de gegevens voor uw gegevensset.
2. Als u de zoekopdracht logboek portal gebruikt, klikt u op **Power BI**.  Als u het Analytics-portal, selecteert u **exporteren** > **Power BI Query (M)**.  Beide opties de query exporteren naar een tekstbestand aangeroepen **PowerBIQuery.txt**. 

    ![Logboek zoekopdracht exporteren](media/log-analytics-powerbi/export-logsearch.png) ![Logboek zoekopdracht exporteren](media/log-analytics-powerbi/export-analytics.png)

3. Open het tekstbestand en kopieer de inhoud.

## <a name="import-query-into-power-bi-desktop"></a>Query importeren in Power BI Desktop
Power BI Desktop is een bureaubladtoepassing waarmee u gegevenssets en rapporten maken die kunnen worden gepubliceerd naar Power BI.  U kunt deze ook gebruiken voor het maken van een query met de Power Query-taal geëxporteerd van logboekanalyse. 

1. Installeer [Power BI Desktop](https://powerbi.microsoft.com/desktop/) als u niet al op uw computer en vervolgens de toepassing open.
2. Selecteer **gegevens ophalen** > **leeg Query** om een nieuwe query te openen.  Selecteer vervolgens **geavanceerde Editor** en plak de inhoud van het geëxporteerde bestand in de query. Klik op **Gereed**.

    ![Power BI Desktop-query](media/log-analytics-powerbi/desktop-new-query.png)

5. De query wordt uitgevoerd, en de resultaten worden weergegeven.  U wordt mogelijk gevraagd om referenties voor verbinding met Azure.  
6. Typ een beschrijvende naam voor de query.  De standaardwaarde is **Query1**. Klik op **sluiten en toepassen** de gegevensset toevoegen aan het rapport.

    ![De naam van Power BI Desktop](media/log-analytics-powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Publiceren naar Power BI
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



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) voor query's opbouwen die kunnen worden geëxporteerd naar Power BI.
* Meer informatie over [Power BI](http://powerbi.microsoft.com) visualisaties op basis van logboekanalyse uitvoer bouwen.
