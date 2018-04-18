---
title: Visualiseren en Azure Stream Analytics-taken oplossen
description: In dit artikel wordt beschreven hoe een Stream Analytics-taak doen selfservice het oplossen van problemen met de functie van de diagram diagnostische gegevens te visualiseren.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 523802f1f9a1dda19c5b6a66da7bc26fee851bd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualiseren en Stream Analytics-taken oplossen
In de Stream Analytics, net als bij andere technologieën cloud-gebaseerde is probleemoplossing soms nodig te onderzoeken waarom een taak geen levert de verwachte uitvoer (of eigenlijk uitvoer). Met dit concept in gedachten biedt Stream Analytics de mogelijkheid voor het visualiseren van een streaming-taak. Dit is ook handig als een hulpmiddel voor het modelleren en heeft het voordeel aan clientzijde voor deze vereisen documentatie van hun werk.

De invoer is in het deelvenster visualisatie zichtbaar evenals de query wordt uitgevoerd en vervolgens alle de uitvoer geconfigureerd. - Of configuratieproblemen problemen kunnen duidelijker en kan ook handig om te zien van een visuele representatie van uw configuratie zijn.

## <a name="using-the-diagnosis-diagram-tool"></a>Hulpprogramma voor het diagnose diagram
Voor toegang tot deze visualizer, klikt u op de knop 'Diagnose diagram' in het gedeelte 'Instellingen' van de Stream Analytics-taak.

![Stream-Analytics-Troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Alle invoer en uitvoer is kleurcode om aan te geven van de huidige status van dit onderdeel, zoals wordt weergegeven als volgt.

![stream-analytics-troubleshoot-visualization-input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Wanneer de gebruiker wil om te kijken naar tussenliggende querystappen om te begrijpen van het patroon van de gegevensstroom binnen een taak, biedt het hulpprogramma visualisatie een weergave van de uitsplitsing van de query in de bijbehorende stappen onderdeel en de volgorde van de stroom. Op elke stap van de query te klikken, ziet u de bijbehorende sectie in een query bewerken deelvenster zoals geïllustreerd. 

![stream-analytics-troubleshoot-visualization-intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

