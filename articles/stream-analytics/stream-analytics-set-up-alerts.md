---
title: Instellen van waarschuwingen voor Azure Stream Analytics-taken bewaken
description: In dit artikel wordt beschreven hoe u Azure portal gebruiken voor het instellen van bewaking en waarschuwingen voor Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 727747d84d0db32c73fc1a200bcea7e5c149d24b
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554908"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Waarschuwingen instellen voor Azure Stream Analytics-taken
U kunt waarschuwingen instellen voor het activeren van een waarschuwing wanneer een metrische waarde bereikt een voorwaarde die u opgeeft. U kunt bijvoorbeeld een waarschuwing voor een voorwaarde als volgt instellen:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Regels kunnen worden ingesteld op de metrische gegevens via de portal of kunnen worden geconfigureerd [programmatisch](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) Bewerkingslogboeken gegevens.

## <a name="set-up-alerts-in-the-azure-portal"></a>Stel waarschuwingen in Azure portal
1. Open de Stream Analytics-taak die u wilt een waarschuwing maken voor in de Azure-portal. 

2. In de **taak** blade, klikt u op de **bewaking** sectie.  

3. In de **Metric** blade, klikt u op de **waarschuwing toevoegen** opdracht.

      ![Azure portal Stream Analytics waarschuwingen instellen](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Voer een naam en een beschrijving.

5. Gebruik het selectoren voor het definiëren van de voorwaarde waarin de waarschuwing wordt verzonden.

6. Bevatten informatie over waar de waarschuwing moet gaan.

      ![Instellen van een waarschuwing voor een Azure stream Analytics-taak](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Zie voor meer informatie over het configureren van waarschuwingen in Azure portal [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

