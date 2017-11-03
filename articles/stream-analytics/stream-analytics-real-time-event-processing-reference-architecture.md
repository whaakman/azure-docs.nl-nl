---
title: Realtime-gebeurtenissen verwerken met de verwerking van de Stream Analytics-gebeurtenis | Microsoft Docs
description: Meer informatie over hoe een set van Azure-services kan samenwerken voor het inschakelen van de verwerking van realtime-gebeurtenissen en analyses.
keywords: realtime verwerking, verwerking van gebeurtenissen, referentiearchitectuur
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: samacha
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: samacha
ms.openlocfilehash: a5206f74e61c3cfd210ff80654e31f466ce08977
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Verwijzen naar architectuur: realtime-gebeurtenissen verwerken met Microsoft Azure Stream Analytics
De referentiearchitectuur voor realtime-gebeurtenissen verwerken met Azure Stream Analytics is bedoeld voor een algemene blauwdruk voor het implementeren van een realtime platform als een oplossing voor de stroom-de verwerking van service (PaaS) met Microsoft Azure.

## <a name="summary"></a>Samenvatting
Traditioneel zijn analytics-oplossingen gebaseerd op de mogelijkheden, zoals ETL (extract, transform, load) en datawarehousing, waar gegevens worden opgeslagen voordat de analyse. Veranderende vereisten, met inbegrip van meer snel binnenkomende gegevens, zijn dit bestaande model om de limiet te pushen. De mogelijkheid voor het analyseren van gegevens binnen het verplaatsen van streams voorafgaand aan de opslag is een oplossing en terwijl het is niet een nieuwe mogelijkheid, de methode niet algemeen is vastgesteld tussen alle bedrijfstak verticalen. 

Microsoft Azure biedt een uitgebreide catalogus analytics-technologieën die u kunnen ondersteunen een matrix van de oplossing voor verschillende scenario's en vereisten. Selecteren welke Azure-services te implementeren voor een end-to-end-oplossing, kan een gegeven van de breedte van de offerings uitdaging zijn. Dit artikel is ontworpen om de mogelijkheden en de interoperabiliteit van de verschillende Azure-services die ondersteuning bieden voor een gebeurtenis streaming-oplossing te beschrijven. Hierin wordt uitgelegd sommige scenario's waarin klanten van dit type benadering profiteren kunnen.

## <a name="contents"></a>Inhoud
* Managementsamenvatting
* Inleiding tot realtime-analyses
* Toegevoegde waarde van realtime-gegevens in Azure
* Algemene scenario's voor realtime-analyses
* Architectuur en onderdelen
  * Gegevensbronnen
  * Gegevensintegratie laag
  * Realtime-analyses laag
  * Data Storage-laag
  * Presentatie / laag-verbruik
* Conclusie

**Auteur:** Jeroen Feddersen, Solution Architect Datacenter Insights van uitmuntende Microsoft Corporation

**Gepubliceerd:** januari 2015

**Revisie:** 1.0

**Download:** [realtime-gebeurtenissen verwerken met Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

