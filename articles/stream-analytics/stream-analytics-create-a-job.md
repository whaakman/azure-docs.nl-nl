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
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Het maken van een taak analytics verwerking voor Stream Analytics
De resource op het hoogste niveau in Azure Stream Analytics is een Stream Analytics-taak.  Bestaat uit een of meer invoer gegevensbronnen, een query uitdrukken van de gegevenstransformatie en een of meer uitvoer-doelen die resultaten naar worden geschreven. Samen kunnen deze gebruikers verwerken voor streaming gegevensscenario data-analyses uitvoeren.

Om te starten met Stream Analytics, te beginnen met het maken van een nieuwe Stream Analytics-taak.  Houd er rekening mee dat deze actie heeft geen gevolgen voor facturering totdat de taak is gestart.

1. Meld u aan bij de online [klassieke Azure-portal](http://manage.windowsazure.com) of de [Azure-portal](https://portal.azure.com/).
2. In de portal: **op nieuw**, klikt u vervolgens op **Data Services** of **gegevensanalyse** afhankelijk van uw portal en klik vervolgens op **Azure Stream Analytics**of **Stream Analytics** en vervolgens **snelle invoer**.
   
   ![Wizard gegevensverwerking analytics-taak](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Gegevensanalyse taakverwerking maken](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Geef de gewenste configuratie voor de Stream Analytics-taak.
   
   * In de **taaknaam** Voer een unieke naam voor de Stream Analytics-taak. Wanneer de **taaknaam** is gevalideerd, verschijnt een groen vinkje in het vak de naam van de taak. De **taaknaam** mag alleen alfanumerieke tekens bevatten en de '-' bevatten, en moet tussen 3 en 63 tekens.
   * Gebruik **regio** in de Azure portal of **locatie** in de Azure portal om op te geven van de geografische locatie waar u de taak uitvoeren.
   * Als u de Azure-portal, selecteer of maak een opslagaccount wilt gebruiken als de **Opslagaccount regionale controle**. Dit opslagaccount wordt gebruikt voor het opslaan van de bewakingsgegevens voor alle Stream Analytics-taken die in deze regio worden uitgevoerd.
   * Als de Azure-portal, geeft u een nieuwe of bestaande **resourcegroep** voor verwante resources voor uw toepassing.
4. Nadat de nieuwe opties voor de Stream Analytics-taak zijn geconfigureerd, klikt u op **Stream Analytics-taak maken**. Het kan enkele minuten duren voordat de Stream Analytics-taak moet worden gemaakt. U kunt de status kunt u de voortgang in Notification hubs.
   
   ![Gegevens analytics verwerking taak Notification hubs](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure portal gegevensanalyse verwerken taak taak maken](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. De nieuwe taak wordt de status weergegeven **gemaakt**. U ziet dat de **Start** knop is uitgeschakeld. De invoer van de taak, de query en de uitvoer configureren voordat u begint met de taak.
   
   ![Gegevensverwerking analytics-taak Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portal gegevensanalyse taakstatus verwerken](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

