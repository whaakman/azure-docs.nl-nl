---
title: Het starten van de streaming-taken in de Stream Analytics | Microsoft Docs
description: Hoe een streaming-taak uitgevoerd in Azure Stream Analytics | leren padsegment.
keywords: Streaming-taken
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fb084f1c6b53e2582849e71271e8114a22dcf05c
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Een streaming-taak uitvoeren in Azure Stream Analytics
Wanneer een taak invoeren, de query en de uitvoer zijn alle opgegeven dat kunt u de Stream Analytics-taak starten.

De taak starten:

1. Klik in de klassieke Azure-portal, vanuit het dashboard taak **Start** aan de onderkant van de pagina.
   
   ![Taak knop starten](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   Klik in de Azure-portal op **Start** boven aan de pagina van de taak.
   
   ![Azure portal starttaak knop](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Geef een **Start uitvoer** waarde om te bepalen wanneer deze taak zal starten uitvoer te produceren. De standaardinstelling voor taken die eerder niet is gestart is **begintijd taak**, wat betekent dat de taak gaat onmiddellijk verwerken van gegevens. U kunt ook opgeven een **aangepaste** tijd in het verleden (voor het gebruiken van historische gegevens) of in de toekomst (tot vertragen tot een toekomstig tijdstip). Voor het geval wanneer een taak is eerder gestart en gestopt, de optie **laatste tijd geëindigd** is beschikbaar om te kunnen hervat de taak van de laatste keer dat uitvoer en voorkomen dat gegevens verloren gaan.  

> [!NOTE]
> Wanneer u partities gebruikt, vertegenwoordigt laatste tijd geëindigd het minimum van tijd van laatste uitvoer voor alle partities.
   
   ![Streaming-taak tijd starten](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portal Start streaming-taak tijd](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Bevestig uw selectie. Status van de taak wordt gewijzigd in *starten* en wordt binnenkort verplaatst naar *met* nadat de taak is gestart. U kunt de voortgang van de **Start** bewerking in de **Notification Hub**:
   
   ![Streaming-taak uitgevoerd](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Azure-portal streaming-taak uitgevoerd](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

