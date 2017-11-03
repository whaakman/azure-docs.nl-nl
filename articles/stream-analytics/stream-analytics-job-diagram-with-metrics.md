---
title: Azure Stream Analytics gegevensgestuurde foutopsporing met behulp van het diagram taak | Microsoft Docs
description: De Stream Analytics-taak oplossen met behulp van de taak diagram en metrische gegevens.
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/01/2017
ms.author: samacha
ms.openlocfilehash: 5b689c07bf8baa531c7a50ca50ed5140c1787e7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a>Gegevensgestuurde foutopsporing met behulp van de taak-diagram

Diagram van de taak op de **bewaking** blade in de Azure portal kunt u uw pijplijn taak visualiseren. Deze bevat invoer, uitvoer en querystappen. Het diagram taak kunt u de metrische gegevens voor elke stap sneller isoleren van de bron van een probleem opgetreden bij het oplossen van problemen onderzoeken.

## <a name="using-the-job-diagram"></a>Met behulp van de taak-diagram

In de Azure portal, terwijl in een Stream Analytics-taak onder **ondersteuning + probleemoplossing**, selecteer **taak diagram**:

![Diagram van de taak met metrische gegevens - locatie](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

Elke stap van de query voor een overzicht van de bijbehorende sectie in een query deelvenster bewerken te selecteren. Een grafiek metrische gegevens voor de stap wordt weergegeven in een onderste deelvenster op de pagina.

![Diagram van de taak met metrische gegevens - basic taak](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Overzicht van de partities van de invoer Azure Event Hubs, selecteer **...** Een contextmenu wordt weergegeven. U kunt ook de invoer fusie zien.

![Diagram van de taak met metrische gegevens - partitie uitbreiden](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

Overzicht van de metrische grafiek voor slechts één partitie, selecteer het knooppunt partitie. De metrische gegevens worden aan de onderkant van de pagina weergegeven.

![Diagram van de taak met metrische gegevens - meer metrische gegevens](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

Overzicht van de grafiek metrische gegevens voor een fusie, selecteer het knooppunt fusie. Het volgende diagram ziet u dat er geen gebeurtenissen zijn verwijderd of gewijzigd.

![Diagram van de taak met metrische gegevens - raster](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

Voor de details van de metrische waarde en de tijd, wijst u de grafiek.

![Taak diagram met metrische gegevens: houd de muisaanwijzer](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Problemen oplossen met behulp van de metrische gegevens

De **QueryLastProcessedTime** metriek geeft aan wanneer de gegevens voor het ontvangen van een specifieke stap. Door te kijken naar de topologie, kunt u achterwaarts werken vanuit de uitvoer-processor om te zien welke stap is niet ontvangen van gegevens. Als een stap niet van gegevens ophalen is, gaat u naar de stap query net vóór. Controleer of de vorige stap van de query heeft een tijdvenster en als voldoende tijd is verstreken voor deze gegevens uit te voeren. (Let op dat moment windows zijn uitgelijnd op het uur.)
 
Als de vorige stap van de query een invoer-processor is, gebruiken de invoer metrische gegevens om te beantwoord de volgende gerichte vragen. Ze kunnen u helpen bepalen of een taak is ophalen van gegevens uit de invoerbronnen. Controleer elke partitie als de query gepartitioneerd is.
 
### <a name="how-much-data-is-being-read"></a>Hoeveel gegevens wordt gelezen?

*   **InputEventsSourcesTotal** is het aantal eenheden lezen. Bijvoorbeeld, het aantal blobs.
*   **InputEventsTotal** is het aantal gebeurtenissen dat is gelezen. Deze metrische waarde is beschikbaar per partitie.
*   **InputEventsInBytesTotal** is het aantal bytes dat is gelezen.
*   **InputEventsLastArrivalTime** is bijgewerkt met de tijd van elke ontvangen gebeurtenis in de wachtrij.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Is tijd vooruitgang? Als feitelijke gebeurtenissen worden gelezen, kan interpunctie niet worden verleend.

*   **InputEventsLastPunctuationTime** geeft aan wanneer er interpunctie is gegenereerd om ervoor te zorgen dat de tijd vooruit blijft lopen. Als interpunctie niet wordt verleend, kan gegevensstroom ophalen geblokkeerd.
 
### <a name="are-there-any-errors-in-the-input"></a>Zijn er fouten in de invoer?

*   **InputEventsEventDataNullTotal** is een aantal van gebeurtenissen die null-gegevens.
*   **InputEventsSerializerErrorsTotal** is het aantal gebeurtenissen dat kan niet correct worden gedeserialiseerd.
*   **InputEventsDegradedTotal** is een aantal van gebeurtenissen die een probleem gehad andere dan met deserialisatie.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Worden gebeurtenissen verwijderd of aangepast?

*   **InputEventsEarlyTotal** is het aantal gebeurtenissen die u een tijdstempel van de toepassing voordat de bovengrens hebt.
*   **InputEventsLateTotal** is het aantal gebeurtenissen die gedurende de tijdstempel van een toepassing na de bovengrens hebben.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** is het aantal gebeurtenissen verwijderd voordat de begintijd van de taak.
 
### <a name="are-we-falling-behind-in-reading-data"></a>We dalen achter bij het lezen van gegevens?

*   **InputEventsSourcesBackloggedTotal** vertelt u hoeveel meer berichten moeten worden gelezen voor Event Hubs en Azure IoT Hub-invoer.


## <a name="get-help"></a>Help opvragen
Voor meer informatie en ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)
