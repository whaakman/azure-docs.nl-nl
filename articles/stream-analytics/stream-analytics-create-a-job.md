---
title: Het maken van een taak analytics verwerking voor Stream Analytics | Microsoft Docs
description: Maken van een taak analytics verwerking voor Stream Analytics | leren padsegment.
keywords: gegevensverwerking analytics
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 82b9c861fee820cdb0ca1891e0e62436f422d838
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Het maken van een taak analytics verwerking voor Stream Analytics
De resource op het hoogste niveau in Azure Stream Analytics is een Stream Analytics-taak.  Bestaat uit een of meer invoer gegevensbronnen, een query uitdrukken van de gegevenstransformatie en een of meer uitvoer-doelen die resultaten naar worden geschreven. Samen kunnen deze gebruikers verwerken voor streaming gegevensscenario data-analyses uitvoeren.

Om te starten met Stream Analytics, te beginnen met het maken van een nieuwe Stream Analytics-taak.  Houd er rekening mee dat deze actie heeft geen gevolgen voor facturering totdat de taak is gestart.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **nieuw** > **gegevens en analyse** > **Stream Analytics-taak**.
3. Selecteer **Maken**.
   
3. Geef de gewenste configuratie voor de Stream Analytics-taak.
   
   * In de **taaknaam** Voer een unieke naam voor de Stream Analytics-taak. Wanneer de **taaknaam** is gevalideerd, verschijnt een groen vinkje in het vak de naam van de taak. De **taaknaam** mag alleen alfanumerieke tekens bevatten en de '-' bevatten, en moet tussen 3 en 63 tekens.
   * Gebruik **locatie** om op te geven van de geografische locatie waar u de taak uitvoeren.
   * Geef een nieuwe of bestaande **resourcegroep** voor verwante resources voor uw toepassing.
4. Selecteer **Maken**.
Het kan enkele minuten duren voordat de Stream Analytics-taak moet worden gemaakt. U kunt de status kunt u de voortgang in Notification hubs.
    
   ![Azure portal gegevensanalyse verwerken taak taak maken](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. De nieuwe taak wordt de status weergegeven **gemaakt**. U ziet dat de **Start** knop is uitgeschakeld. De invoer van de taak, de query en de uitvoer configureren voordat u begint met de taak.

   
   ![Azure portal gegevensanalyse taakstatus verwerken](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

