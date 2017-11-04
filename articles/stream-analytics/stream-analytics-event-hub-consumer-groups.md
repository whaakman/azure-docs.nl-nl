---
title: Fouten opsporen in Azure Stream Analytics met event hub-ontvangers | Microsoft Docs
description: Aanbevolen procedures voor het overwegen van Event Hubs consumer-groepen in de Stream Analytics-taken opvragen.
keywords: limiet van Event hub, klantengroep
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: ede3137de92e251f4ad020bc1ce3f041918242b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a>Fouten opsporen in Azure Stream Analytics met event hub-ontvangers

U kunt Azure Event Hubs in Azure Stream Analytics opnemen of gegevens uit een taak uitvoeren. Er is een best practice voor het gebruik van Event Hubs meerdere consumergroepen gebruiken om te controleren of de taak schaalbaarheid. Een reden hiervoor is dat het aantal lezers in de Stream Analytics-taak voor een specifieke invoer betrekking heeft op het aantal lezers in een enkel consumergroep. Het exacte aantal ontvangers is gebaseerd op interne implementatiedetails voor de scale-out-topologie-logica. Het aantal ontvangers is extern niet toegankelijk. Het aantal lezers kunt wijzigen op de begintijd van taak of tijdens upgrades van de taak.

> [!NOTE]
> Wanneer het aantal lezers gewijzigd tijdens de upgrade van een taak, worden naar het controlelogboeken tijdelijke waarschuwingen geschreven. Stream Analytics-taken worden automatisch herstellen van deze problemen van voorbijgaande aard.

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Het aantal lezers per partitie overschrijdt de limiet van vijf Event Hubs

Scenario's waarin het aantal lezers per partitie de Event Hubs-limiet van vijf overschrijdt omvatten het volgende:

* Meerdere SELECT-instructies: als u meerdere SELECT-instructies die naar verwijzen **dezelfde** event hub-invoer, elke SELECT-instructie zorgt ervoor dat een nieuwe ontvanger worden gemaakt.
* UNION: Wanneer u een UNION, het is mogelijk om meerdere invoerwaarden die naar verwijzen de **dezelfde** event hub- en groep.
* SELF-join: Bij gebruik van een SELF JOIN-bewerking is het mogelijk om te verwijzen naar de **dezelfde** event hub meerdere keren.

## <a name="solution"></a>Oplossing

De volgende aanbevolen procedures verminderen scenario's waarin het aantal lezers per partitie de limiet van vijf voor Event Hubs overschrijdt.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Uw query splitsen in meerdere stappen met behulp van een WITH-component

De component WITH Hiermee geeft u een tijdelijk benoemde resultatenset die door een FROM-component in de query kan worden verwezen. De component WITH wordt gedefinieerd in het bereik van de uitvoering van één SELECT-instructie.

Bijvoorbeeld, in plaats van deze query:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Gebruik deze query:

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Zorg ervoor dat de invoer aan verschillende groepen consumenten binden

Voor query's waarin drie of meer invoerwaarden zijn verbonden met de dezelfde consumergroep van Event Hubs, afzonderlijke consumergroepen te maken. Dit is vereist voor het maken van aanvullende Stream Analytics-invoer.


## <a name="get-help"></a>Help opvragen
Voor meer informatie en ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)
