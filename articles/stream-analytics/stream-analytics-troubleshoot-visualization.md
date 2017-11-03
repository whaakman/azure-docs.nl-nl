---
title: Visualiseren en Stream Analytics-taken oplossen | Microsoft Docs
description: Ontdek hoe u een pijplijn Stream Analytics-taak voor self-service het oplossen van problemen met de functie voor het diagram van diagnostische gegevens te visualiseren.
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 820b73a5dbf9bb108e189313cf6ee2b924ab04c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualiseren en Stream Analytics-taken oplossen
In de Stream Analytics, net als bij andere technologieën cloud-gebaseerde is probleemoplossing soms nodig te onderzoeken waarom een taak geen levert de verwachte uitvoer (of eigenlijk uitvoer). Met dit concept in gedachten biedt Stream Analytics de mogelijkheid voor het visualiseren van een streaming-taak. Dit is ook handig als een hulpmiddel voor het modelleren en heeft het voordeel aan clientzijde voor deze vereisen documentatie van hun werk.

De invoer is in het deelvenster visualisatie zichtbaar evenals de query wordt uitgevoerd en vervolgens alle de uitvoer geconfigureerd. - Of configuratieproblemen problemen kunnen duidelijker en kan ook handig om te zien van een visuele representatie van uw configuratie zijn.

## <a name="using-the-diagnosis-diagram-tool"></a>Hulpprogramma voor het diagnose diagram
Voor toegang tot deze visualizer, klikt u op de knop 'Diagnose diagram' in het gedeelte 'Instellingen' van de Stream Analytics-taak.

![Stream-Analytics-Troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Alle invoer en uitvoer is kleurcode om aan te geven van de huidige status van dit onderdeel, zoals wordt weergegeven als volgt.

![Stream-Analytics-Troubleshoot-visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Wanneer de gebruiker wil om te kijken naar tussenliggende querystappen om te begrijpen van het patroon van de gegevensstroom binnen een taak, biedt het hulpprogramma visualisatie een weergave van de uitsplitsing van de query in de bijbehorende stappen onderdeel en de volgorde van de stroom. Op elke stap van de query te klikken, ziet u de bijbehorende sectie in een query bewerken deelvenster zoals geïllustreerd. 

![Stream-Analytics-Troubleshoot-visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

