---
title: Azure Stream Analytics-taak Staten
description: In dit artikel beschrijft de verschillende statussen van een Stream Analytics-taak
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/06/2019
ms.openlocfilehash: 28e0e69d3a6a4d3a38146cbf2c49426b3b16c784
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961581"
---
# <a name="azure-stream-analytics-job-states"></a>Azure Stream Analytics-taak Staten

Een Stream Analytics-taak kan worden in een van de vier statussen op elk moment. U kunt de status van uw taak op de pagina overzicht van uw Stream Analytics-taak vinden in de Azure-portal. 

| Status | Description | Aanbevolen acties |
| --- | --- | --- |
| **Wordt uitgevoerd** | Uw taak wordt uitgevoerd op Azure lezen van gebeurtenissen die afkomstig zijn van de gedefinieerde invoerbronnen, deze worden verwerkt en de resultaten naar de geconfigureerde uitvoerlocaties schrijven. | Het is een best practice om bij te houden van de prestaties van uw taak door de bewaking van [belangrijke metrische gegevens](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). |
| **Gestopt** | Uw taak is gestopt en worden de gebeurtenissen niet verwerkt. | N.v.t. | 
| **Gedegradeerd** | Tijdelijke fouten zijn waarschijnlijk invloed op uw taak. Stream Analytics probeert onmiddellijk te herstellen van dergelijke fouten en Ga terug naar een status die wordt uitgevoerd (binnen enkele minuten). Deze fouten kunnen gebeuren vanwege netwerkproblemen, beschikbaarheid van andere Azure-resources, deserialisatie fouten enzovoort. Prestaties van uw taak wordt mogelijk beïnvloed wanneer taak gedegradeerde status heeft.| U kunt kijken de [diagnostische of activiteit logboeken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) voor meer informatie over de oorzaak van deze tijdelijke fouten. In gevallen, zoals de deserialisatie-fouten, het verdient neemt u corrigerende maatregelen om ervoor te zorgen gebeurtenissen worden niet onjuist gevormd. Als de taak wordt de limiet voor het gebruik van resource wordt bereikt, probeert te vergroten het SU-nummer of [uw taak parallel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). In andere gevallen waar kunt u geen actie ondernemen, Stream Analytics probeert te herstellen naar een *met* staat.  |
| **Mislukt** | Uw taak is een kritieke fout, wat resulteert in een mislukte status aangetroffen. Gebeurtenissen worden niet gelezen en verwerkt. Runtime-fouten zijn een veelvoorkomende oorzaak van de taken in een foutstatus loopt. | U kunt [waarschuwingen configureren](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) zodat u een melding ontvangen wanneer de taak gaat u naar de status mislukt. <br> <br>U kunt fouten opsporen met behulp van [activiteit en diagnostische logboeken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) om te bepalen van de hoofdoorzaak te achterhalen en los het probleem.|

## <a name="next-steps"></a>Volgende stappen
* [Waarschuwingen instellen voor Azure Stream Analytics-taken](stream-analytics-set-up-alerts.md)
* [Metrische gegevens beschikbaar zijn in Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [Problemen oplossen met behulp van de activiteit en diagnostische logboeken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)
