---
title: Fouten opsporen in bewerking te gebruiken en in de Stream Analytics-service registreert | Microsoft Docs
description: Gebruik Stream Analytics-bewerkingslogboeken
keywords: Service-Logboeken
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 2ee0871752dc2a3da345339fb826340d44ae48d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Fouten opsporen-service en bewerking logboeken met Stream Analytics-taken
Operationele logboekregistratie berichten naar gebruikers details van de records gerelateerd aan beheerbewerkingen op alle Azure-services te geven. Deze informatie kan worden gebruikt voor foutopsporing zoals status, informatie over de voortgang van deze taak weergeven in Azure Stream Analytics en foutberichten bijhouden van de voortgang van een taak na verloop van tijd van starten op de verwerking van om uit te voeren.

## <a name="find-operation-logs-in-the-azure-portal"></a>Bewerking wordt geregistreerd in de Azure-portal zoeken
Bewerkingslogboeken toegankelijk zijn op twee manieren:  

* Dashboard van de Stream Analytics-taak  
* Management-Services in de klassieke Azure portal  

## <a name="dashboard-of-the-stream-analytics-job"></a>Dashboard van de Stream Analytics-taak
Een koppeling naar de bijbehorende logboeken van een Stream Analytics-taak wordt weergegeven op het tabblad van de taak-Dashboard. Als u op de koppeling klikt, wordt de filters ingesteld op een manier dat het meest recente logboeken voor die specifieke taak bevat.

  ![Selecteer beheerservices Logboeken](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Management-Services
Om handmatig naar de logboeken van de bewerking voor Stream Analytics en andere services in de klassieke Azure portal:

1. Klik op **beheerservices** in de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer **Stream Analytics** voor **Type** en de naam van de taak voor **servicenaam**.  
   
   ![Selecteer de Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Controlelogboeken niet vinden in de Azure-portal
Voor operationele logboeken voor uw Stream Analytics-taak in de Azure portal, klikt u **Bladeren** en selecteer vervolgens **controlelogboeken**.

  ![Azure-portal Stream Analytics selecteren](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Hiermee opent u een weergave met gebeurtenissen van de afgelopen zeven dagen voor alle resources in uw abonnement.  U kunt filteren om gebeurtenissen te bekijken van een type opgeven of de periode door te klikken op de **Filter** opdracht.

  ![Azure-portal Stream Analytics selecteren](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Logboekdetails van het ophalen
U kunt filteren op tijdsbereik en bekijk de logboeken voor de taak de Status.

Klik in de Azure-portal op de **Details** knop onderaan in het venster om meer details over een geselecteerde gebeurtenis te bekijken. 

  ![Details van selecteren](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Klik in de Azure-portal op een logboekvermelding om de gedetailleerde gebeurtenissen erin te bekijken.

  ![Azure-portal Details selecteren](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

U kunt openen vanuit de **Detail** weergeven door te klikken op de gebeurtenis.

  ![Azure-portal Details selecteren](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Fouten opsporen in een mislukte taak
Klik op het zoekpictogram en het type 'mislukt' in de Azure portal. Hiermee geeft u een resultaat van alle logboeken met fouten. 

  ![Foutopsporing van een taak die is mislukt](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

In de Azure portal kunt u filteren op niveau van het bericht om weer te geven **Kritiek** gebeurtenissen.

  ![Azure portal foutopsporing](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Selecteer een van de fouten en klik op de **Details** voor meer informatie over de fout.  Bepaalde foutberichten bevatten ook informatie over het beperken van het probleem. 

  ![Details van bewerking](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Als u contact moet [ondersteuning](https://azure.microsoft.com/support/options/) of vindt u informatie voor het team via de [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), houd rekening met de Details van de bewerking specifiek de **correlatie-ID**. 

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

