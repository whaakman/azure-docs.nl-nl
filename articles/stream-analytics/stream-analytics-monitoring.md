---
title: Understanding Stream Analytics-taak bewaken | Microsoft Docs
description: Informatie over Stream Analytics-taak controleren
keywords: query-monitor
services: stream-analytics
documentationcenter: 
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 7474f45494c6190ffcac354e75458b18f5777fb9
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Stream Analytics-taak bewaking en het controleren van query's begrijpen

## <a name="introduction-the-monitor-page"></a>Inleiding: De pagina van de monitor
De Azure portal beide surface prestatie metrische gegevens die kunnen worden gebruikt om te controleren en problemen oplossen van de prestaties van uw query en -taak. Overzicht van deze metrische gegevens, blader naar de Stream Analytics-taak u geïnteresseerd bent in de metrische gegevens voor zien en de **bewaking** sectie op de pagina overzicht.  

![Bewaking van de koppeling](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Het venster wordt weergegeven, zoals wordt weergegeven:

![Bewakingstaak Dashboard](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metrische gegevens beschikbaar voor Stream Analytics
| Gegevens                 | Definitie                               |
| ---------------------- | ---------------------------------------- |
| Gebruikspercentage voor Streaming-eenheden       | Het gebruik van de Streaming-eenheid is toegewezen aan een taak op het tabblad schaal van de taak. Hierboven, wordt hoge kans dat de verwerking van gebeurtenissen kan worden vertraagd of gestopt Bezig met het of moet deze indicator 80% bereiken. |
| Invoergebeurtenissen           | De hoeveelheid gegevens die door de Stream Analytics-taak in het aantal gebeurtenissen dat is ontvangen. Dit kan worden gebruikt om te valideren dat gebeurtenissen worden verzonden naar de invoerbron. |
| Uitvoergebeurtenis          | De hoeveelheid gegevens die naar het uitvoerdoel, in aantal gebeurtenissen dat door de Stream Analytics-taak wordt verzonden. |
| Out-van-Order-gebeurtenissen    | Het aantal gebeurtenissen ontvangen volgorde die zijn verwijderd of een aangepaste tijdstempel, op basis van het beleid voor het ordenen van gebeurtenis gegeven. Dit kan worden beïnvloed door de configuratie van de instelling tolerantieperiode Out geplaatst. |
| Gegevensconversiefouten | Het aantal gegevens conversiefouten die door een Stream Analytics-taak. |
| Runtimefouten         | Totale aantal fouten met betrekking tot de verwerking van query's (met uitzondering van fouten aangetroffen tijdens het opnemen van gebeurtenissen of outputing resultaten) |
| Late invoergebeurtenissen      | Aantal gebeurtenissen dat binnenkomt laat vanuit de bron die ofwel verwijderd of de tijdstempel is aangepast, op basis van de configuratie van de gebeurtenis ordening beleid van de instelling tolerantieperiode voor Late ontvangst. |
| Functieaanvragen      | Het aantal aanroepen naar de Azure Machine Learning-functie (indien aanwezig). |
| Mislukte functieaanvragen | Het aantal mislukte Azure Machine Learning-functieaanroepen (indien aanwezig). |
| Functiegebeurtenissen        | Aantal gebeurtenissen dat is verzonden naar de Azure Machine Learning-functie (indien aanwezig). |
| Invoergebeurtenisbytes      | De hoeveelheid gegevens die zijn ontvangen door de Stream Analytics-taak, in bytes. Dit kan worden gebruikt om te valideren dat gebeurtenissen worden verzonden naar de invoerbron. |


## <a name="customizing-monitoring-in-the-azure-portal"></a>Bewaking aanpassen in de Azure portal
U kunt het type van de grafiek, metrische gegevens weergegeven, aanpassen en tijdsbereik in de instellingen van de grafiek bewerken. Zie voor meer informatie [hoe aanpassen bewaking](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Query-tijd van de Monitor-grafiek](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Meest recente uitvoer
Een ander interessant gegevenspunt voor het bewaken van de taak is de tijd van de laatste uitvoer, die wordt weergegeven op de pagina overzicht.
Dit is de toepassing-tijd (dat wil zeggen de tijd die met behulp van de tijdstempel van de gegevens van de gebeurtenis) van de meest recente uitvoer van de taak.

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

