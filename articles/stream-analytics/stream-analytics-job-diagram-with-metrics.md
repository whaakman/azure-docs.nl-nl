---
title: Gegevensgestuurde foutopsporing in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u uw Azure Stream Analytics-taak oplossen met behulp van het taakdiagram en metrische gegevens in Azure portal.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/01/2017
ms.openlocfilehash: 4a6d359b27b9a2e52d71ed5f8547041645147605
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61479915"
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a>Gegevensgestuurde foutopsporing met behulp van het taakdiagram

Het taakdiagram op de **bewaking** blade in Azure portal kunt u uw pijplijn taak visualiseren. Hierin worden invoer, uitvoer en querystappen weergegeven. U kunt het taakdiagram gebruiken om de metrische gegevens voor elke stap te onderzoeken, om de bron van het probleem sneller te isoleren bij het oplossen van problemen.

## <a name="using-the-job-diagram"></a>Met behulp van het taakdiagram

In de Azure-portal, terwijl in een Stream Analytics-taak, onder **ondersteuning + probleemoplossing**, selecteer **taakdiagram**:

![Taakdiagram met metrische gegevens - locatie](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

Selecteer elke querystap om te zien van de bijbehorende sectie in een deelvenster voor de querybewerking. Een grafiek met metrische gegevens voor de stap wordt in een onderste deelvenster op de pagina weergegeven.

![Taakdiagram met metrische gegevens over - basic-taak](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Als u wilt zien van de partities van de Azure Event Hubs-invoer, selecteer **...** Er wordt een snelmenu weergegeven. U kunt ook de input fusie zien.

![Taakdiagram met metrische gegevens over - partitie uitbreiden](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

Selecteer het partitieknooppunt de grafiek met metrische gegevens voor slechts één partitie. De metrische gegevens worden weergegeven aan de onderkant van de pagina.

![Diagram met metrische gegevens over - metrische gegevens in de taak](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

Als u wilt zien van de grafiek met metrische gegevens voor een fusie, selecteer het knooppunt fusie. Het volgende diagram laat zien dat er zijn geen gebeurtenissen zijn verwijderd of aangepast.

![Taakdiagram met metrische gegevens over - raster](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

Als u wilt zien van de details van de metrische waarde en de tijd, wijst u de grafiek.

![Diagram met metrische gegevens over de taak - Beweeg de muisaanwijzer](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Problemen oplossen met behulp van metrische gegevens

De **QueryLastProcessedTime** metrische waarde geeft aan wanneer de gegevens voor het ontvangen van een specifieke stap. Door te kijken naar de topologie, kunt u achteruit werken vanaf de om te zien welke stap geen gegevens ontvangt. Als een stap geen gegevens ontvangt, gaat u naar de querystap vlak voor. Controleer of de vorige querystap heeft een bepaalde periode, en als er voldoende tijd is verstreken voor het uitvoeren van gegevens. (Let op dat moment windows zijn uitgelijnd op het uur.)
 
Als de vorige querystap een invoerprocessor is, gebruikt u de metrische invoerwaarden antwoord geven op uw de volgende gerichte vragen. Ze kunnen u helpen bepalen of een taak is ophalen van gegevens van invoerbronnen. Controleer elke partitie als de query gepartitioneerd is.
 
### <a name="how-much-data-is-being-read"></a>Hoeveel gegevens worden gelezen?

*   **InputEventsSourcesTotal** is het aantal gelezen gegevenseenheden. Bijvoorbeeld, het aantal blobs.
*   **InputEventsTotal** is het aantal gelezen gebeurtenissen. Deze metrische waarde is beschikbaar per partitie.
*   **InputEventsInBytesTotal** is het aantal gelezen bytes.
*   **InputEventsLastArrivalTime** wordt bijgewerkt met de wachtrijduur van elke ontvangen gebeurtenis.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Is de tijd vooruit? Als er feitelijke gebeurtenissen worden gelezen, mogelijk geen interpunctie worden uitgegeven.

*   **InputEventsLastPunctuationTime** geeft aan wanneer er interpunctie is gegenereerd om ervoor te zorgen dat de tijd vooruit blijft lopen. Als er geen interpunctie wordt gegenereerd, kan gegevensstroom blokkeren.
 
### <a name="are-there-any-errors-in-the-input"></a>Zijn er fouten in de invoer?

*   **InputEventsEventDataNullTotal** is een telling van gebeurtenissen die null gegevens hebben.
*   **InputEventsSerializerErrorsTotal** is een telling van gebeurtenissen die niet goed kunnen worden gedeserialiseerd.
*   **InputEventsDegradedTotal** is een telling van gebeurtenissen die een ander probleem dan bij deserialisatie.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Gebeurtenissen worden verwijderd of aangepast zijn?

*   **InputEventsEarlyTotal** is het aantal gebeurtenissen die een tijdstempel voordat de bovengrens hebben.
*   **Inputeventsearlytotal** is het aantal gebeurtenissen die een tijdstempel na de bovengrens hebben.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** is het aantal gebeurtenissen verwijderd voordat de begintijd van de taak.
 
### <a name="are-we-falling-behind-in-reading-data"></a>We dalen achter bij het lezen van gegevens?

*   **Invoer (totaal) aan gebeurtenissen met achterstand** vertelt u hoeveel berichten moeten worden gelezen voor Event Hubs en Azure IoT Hub-invoer. Als dit aantal groter dan 0 is, betekent dat de taak als waarmee ze zich aandienen de gegevens niet verwerken. U moet in dit geval Verhoog het aantal Streaming-eenheden en/of zorg ervoor dat uw taak kan worden geparallelliseerd. U kunt meer informatie over deze zien op de [query-parallellisatie pagina](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). 


## <a name="get-help"></a>Help opvragen
Voor verdere ondersteuning, kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Stream Analytics query language-referentie](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)
