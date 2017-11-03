---
title: Het schrijven van query's in de Stream Analytics | Microsoft Docs
description: Query's in de Stream Analytics en querygegevens schrijven | leren padsegment.
keywords: het schrijven van query's, gegevens opvragen, een query schrijven van query's schrijven
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 215b774c20d80a67b1cefa2634131bd44860c692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-write-queries-in-stream-analytics"></a>Het schrijven van query's in de Stream Analytics
Schrijven van query's voor stroomverwerkingslogica schrijft in Azure Stream Analytics wordt geïmplementeerd als een 'permanente query' die is gedefinieerd voordat een taak wordt gestart en wordt uitgevoerd op gegevens, zoals de taak is bereikt. De gegevenstransformatie wordt uitgedrukt in een SQL-achtige query-taal, die is grotendeels een subset van T-SQL met enkele taal extensies als toegevoegd [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) gebruikt voor de tijdelijke semantiek express.

## <a name="writing-queries"></a>Schrijven van query's:
1. Klik in de Stream Analytics-taak in de Azure portal op **Query**.
   
    ![Query selecteren](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    Klik in de Azure-portal op **Query**.
   
    ![Selecteer de Query-Preview](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Nieuwe taken hebben een querysjabloon om u te helpen u op weg. De querysjabloon voert een 'Pass Through-' query die in alle projecten velden van invoer gebeurtenissen in de uitvoer.  
   
   * Als u ten minste één invoer en uitvoer voor uw taak hebt gedefinieerd, kunt u de tijdelijke aanduiding "[YourOutputAlias]" vervangen en eerst wordt gebruikt voor de velden "[YourInputAlias]" met de aliassen van de invoer en uitvoer die u wenst. Bovendien kunt u nog steeds ontwerpen en testen van uw query in de klassieke Azure-Portal zonder te definiëren in- en uitgangen op in de taak.
   * Als u uitvoeren van meer dan een eenvoudige pass-through-verwerking wilt, kunt u de definitie van de query bewerken. Om te beginnen met het ontwerpen van query, Bekijk enkele algemene query patronen worden vastgelegd [hier](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Querygegevens venster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Als u wilt valideren querygegevens werkt:
U kunt testen dat uw query werkt zoals verwacht in de browser worden uitgevoerd via een of meer lokale JSON-bestanden met testgegevens. Dit wordt niet de taak start of hebben invloed op de facturering.

> [!NOTE]
> Testen van query's in de browser is momenteel niet ondersteund in de Azure portal.  
> 
> 

1. Zorg ervoor dat er zijn geen fouten in de query (anders de knop testen wordt uitgeschakeld) en klik vervolgens op de knop testen.  
   
   ![Querygegevens Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. U wordt gevraagd om op te geven van bestanden voor elk van de invoerwaarden waarnaar wordt verwezen in de query. In dit voorbeeld wordt de query voor de sjabloon wordt gehandhaafd-is, zodat u het dialoogvenster gevraagd om voor invoer met de naam 'yourinputalias'.
   
   ![De query gegevens testen](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Blader naar een testbestand. Verschillende voorbeeldbestanden zijn beschikbaar op [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) en u kunt ook voorbeeldgegevens ophalen uit de invoer van uw eigen gegevens stream via de functie voorbeeldgegevens op het tabblad invoer.
   
   ![Invoer voor de query](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. Nadat u het dialoogvenster te sluiten, de query wordt uitgevoerd via de testgegevens en ziet u de resultaten onder aan de pagina van de Query.
   
   ![Samenvatting van de query](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

