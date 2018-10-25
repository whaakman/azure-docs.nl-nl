---
title: Gebeurtenissen in realtime verwerken met behulp van Azure Stream Analytics-gebeurtenisverwerking
description: Dit artikel beschrijft de referentiearchitectuur voor het bereiken van gebeurtenissen in realtime-verwerking en analyse met behulp van Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 08ad2d853ab909ea859ffd1230dd651aa6661500
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987627"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Referentie-architectuur: gebeurtenissen verwerken met Microsoft Azure Stream Analytics in realtime
De referentiearchitectuur voor realtime-gebeurtenissen verwerken met Azure Stream Analytics is een algemene blauwdruk bedoeld voor het implementeren van een realtime platform als een oplossing voor het verwerken van streams van service (PaaS) met Microsoft Azure.

## <a name="summary"></a>Samenvatting
Traditioneel zijn analytics-oplossingen gebaseerd op de mogelijkheden, zoals ETL (extract, transform, load) en datawarehousing, waar gegevens worden opgeslagen voordat de analyse. Veranderende eisen, met inbegrip van meer snel binnenkomende gegevens, zijn deze bestaande model pushen met de limiet. De mogelijkheid voor het analyseren van gegevens binnen streams voordat u opslag verplaatsen van een oplossing is en het is niet een nieuwe functie, de aanpak niet breed is vastgesteld in alle branche verticalen. 

Microsoft Azure biedt een uitgebreide catalogus van analytics-technologieën die geschikt voor het zijn ondersteunen van een matrix van de oplossing voor verschillende scenario's en vereisten. Selecteren welke Azure-services te implementeren voor een end-to-end-oplossing kan lastig zijn gegeven van de breedte van de aanbiedingen. Dit document is ontworpen om de mogelijkheden en maken tussen verschillende Azure-services die ondersteuning bieden voor een gebeurtenis-streaming-oplossing te beschrijven. Hierin wordt uitgelegd enkele van de scenario's waarin klanten van dit soort aanpak profiteren kunnen.

## <a name="contents"></a>Inhoud
* Uitvoeringssamenvatting
* Inleiding tot realtime analyses
* Toegevoegde waarde van realtime-gegevens in Azure
* Algemene scenario's voor analyse in realtime
* Architectuur en onderdelen
  * Gegevensbronnen
  * Gegevensintegratie laag
  * Realtime analyses laag
  * Opslaglaag voor gegevens
  * Presentatie / verbruik laag
* Conclusie

**Auteur:** Charles Feddersen, Solution Architect, Datacenter inzichten van hoogwaardige zakelijke prestaties, Microsoft Corporation

**Gepubliceerd:** januari 2015

**Revisie:** 1.0

**Download:** [realtime-gebeurtenissen verwerken met Microsoft Azure Stream Analytics](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Help opvragen
Voor verdere ondersteuning kunt u proberen de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

