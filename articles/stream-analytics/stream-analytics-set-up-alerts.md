---
title: Waarschuwingen instellen voor query's in de Stream Analytics | Microsoft Docs
description: Understanding Stream Analytics waarschuwingen
keywords: waarschuwingen instellen
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/26/2017
ms.author: samacha
ms.openlocfilehash: cdbd7da36f3eb2795fc4548915ebbf18200ca44d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Waarschuwingen instellen voor Azure Stream Analytics-taken
## <a name="introduction-monitor-page"></a>: Introductiepagina Monitor
U kunt waarschuwingen instellen voor het activeren van een waarschuwing wanneer een metriek bereikt een voorwaarde die u opgeeft. U kunt bijvoorbeeld een waarschuwing voor een voorwaarde die u als volgt instellen:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Regels kunnen worden ingesteld op metrische gegevens via de portal of kunnen worden geconfigureerd [programmatisch](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) over Bewerkingslogboeken gegevens.

## <a name="set-up-alerts-in-the-azure-portal"></a>Stel waarschuwingen in de Azure portal
1. Open de Stream Analytics-taak die u wilt maken van een waarschuwing voor in de Azure-portal. 

2. In de **taak** blade, klikt u op de **bewaking** sectie.  

3. In de **metriek** blade, klikt u op de **waarschuwing toevoegen** opdracht.

      ![Installatie van de Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Voer een naam en beschrijving.

5. Gebruik de selectoren voor het definiëren van de voorwaarde die de waarschuwing wordt verzonden.

6. Bevatten informatie over waar de waarschuwing moet gaan.

      ![Instellen van een waarschuwing voor een Azure Streaming Analytics-taak](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Zie voor meer informatie over het configureren van waarschuwingen in de Azure-portal [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

