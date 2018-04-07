---
title: Bewaking van waarschuwingen voor Azure Stream Analytics-taken ingesteld
description: In dit artikel wordt beschreven hoe de Azure portal gebruiken voor het instellen van de bewaking en waarschuwingen voor Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: 2498c0960ef8fd50064e40428f87d106abf10ecd
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
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
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

